# Task 1: ViewModels and manual Dependency Injection

To enable the interactivity of our two selection screens, we added state and some click logic to the composables. This approach has two problems. First, we don't want to handle state and business logic in the composables—that violates clean separation of concerns. Second, the interactions don't actually set up anything we could use in other screens; they just manipulate the UI of the current screen.

In the framework of KMP and Jetpack Compose, this is usually solved by using [ViewModels](https://developer.android.com/topic/libraries/architecture/viewmodel) in the domain layer, which are responsible for handling the state and business logic of the app and connecting the UI layer to the data layer. They are lifecycle-aware, which means they survive configuration changes and are automatically cleared when the associated UI is destroyed. This makes them ideal for managing UI-related data and logic in a way that's decoupled from the UI components themselves.

In this task, you will set up ViewModels for the selection screens and implement manual dependency injection to manage the dependencies of the ViewModels. This will allow you to properly manage the state and logic of your app.

## Select Player ViewModel

We start with adding a ViewModel for the Select Player screen. Add a new file `SelectPlayerViewModel.kt` to the domain layer of the `player` package and add the `SelectPlayerViewModel` class which inherits from `ViewModel`:

```kotlin
package org.example.carcassonne.player.domain

import androidx.lifecycle.ViewModel

class SelectPlayerViewModel(
    
): ViewModel () {
}
```

We'll leave the constructor empty for now. The first thing we add to the class is a value for the available player colors:

```kotlin
val availablePlayerColors: List<PlayerColor> = PlayerColor.entries
```

This is the list we'll later use to display the available player colors in the UI.

Now we need a state for the selected player colors. State communication between the ViewModel and the UI layer is usually done with [StateFlows](https://developer.android.com/kotlin/flow/stateflow-and-sharedflow), which are a type of observable data holder that emits the current state and new state updates to its collectors—the frontend in our case. StateFlows are so-called hot flows, which means they're always active and emit the current state to new collectors without requiring manual collection. This means when we change the state in the ViewModel, the UI will automatically react to the change and update itself accordingly.

The data type of a state can be freely chosen, but it's common practice to use a custom `UiState` data class that holds all the necessary information for the UI to display. In our case, we create a `SelectPlayerUiState` data class that holds the list of selected player colors. Add it next to the `SelectPlayerViewModel`:

```kotlin
data class SelectPlayerUiState(
    val selectedPlayerColors: List<PlayerColor> = emptyList()
)
```

Now we can add the state to the ViewModel. The state is usually implemented as a private mutable StateFlow that's only modified within the ViewModel, and a public immutable StateFlow that's exposed to the UI layer for collection. Add the following code to the `SelectPlayerViewModel` class:

```kotlin
private val _uiState = MutableStateFlow(SelectPlayerUiState())
val uiState: StateFlow<SelectPlayerUiState> = _uiState.asStateFlow()
```

To update the state, we need to implement some logic that modifies the list of selected player colors when a color is clicked. This is done using the `update` function of the `MutableStateFlow`, which allows us to atomically update the state based on the current value. For example, to select a color, add the following private function to the `SelectPlayerViewModel` class:

```kotlin
private fun selectColor(playerColor: PlayerColor) {
    _uiState.update { currentState ->
        currentState.copy(
            selectedPlayerColors = currentState.selectedPlayerColors + playerColor
        )
    }
}
```

Note that we use the copy functionality of the data class to create a new instance of `SelectPlayerUiState` with the updated list of selected player colors. This is important because StateFlows only emit updates to collectors when a new instance is emitted, so we need to create a new instance of the state to trigger a UI update.

!!! example "Task"
    Add the complementary private `deselectColor` function.

Now that we have all the necessary prerequisites, we can implement the exposed click logic that will be used by the UI layer. Add the following function to the `SelectPlayerViewModel` class:

```kotlin
fun onClickColor(color: String) {
    val enumPlayerColor = PlayerColor.valueOf(color)
    if (_uiState.value.selectedPlayerColors.contains(enumPlayerColor)) {
        deselectColor(enumPlayerColor)
    } else {
        selectColor(enumPlayerColor)
    }
}
```

Now we need our Select Player screen to use the `SelectPlayerViewModel`. But how do we get the ViewModel into the UI?

## ViewModel Provider and manual Dependency Injection

Generally, there are several libraries for dependency injection in Kotlin, such as [Koin](https://insert-koin.io/) or [Dagger](https://dagger.dev/), which can be used to manage the dependencies of ViewModels and other classes in the app. However, the way these frameworks handle dependencies is mostly implicit and can be hard to understand for beginners. Since this is a training, we want to implement dependency injection manually to better understand the underlying concepts.

In manual dependency injection (MDI), we create a provider class with a ViewModel Factory that the UI layer can use to obtain instances of the ViewModels. This provider class is responsible for creating and managing the instances of the ViewModels and their dependencies.

Add a file `AppViewModelProvider` to the `core.domain` package and add an object `AppViewModelProvider`:

```kotlin
package org.example.carcassonne.core.domain

object AppViewModelProvider {
}
```

The only property of this object is a Factory created with the `viewModelFactory` function, which is used to create instances of ViewModels. The factory takes multiple `initializer` lambdas that return the respective instance of a ViewModel, initialized with all its dependencies (the pattern is similar to what you've seen in the `NavHost`). Add the following to the `AppViewModelProvider` object:

```kotlin
val Factory = viewModelFactory {
    initializer {
        SelectPlayerViewModel()
    }
}
```

Since our `SelectPlayerViewModel` doesn't have any dependencies yet, we can simply return a new instance of it in the initializer. Later, when we add dependencies to the ViewModel, we'll manage those dependencies at this very place.

Now we can use our factory to obtain an instance of the `SelectPlayerViewModel` in the `SelectPlayerScreen`. To do this, we use the `viewModel` function, which takes the factory as a parameter and returns an instance of the ViewModel. Add the following code at the top of the `SelectPlayerScreen` composable:

```kotlin
val viewModel: SelectPlayerViewModel = viewModel(factory = AppViewModelProvider.Factory)
```

Now we have full access to the domain layer—specifically, the `SelectPlayerViewModel`—in our Select Player screen, and we can use it to manage the state and logic of the screen.

### Using State and Business Logic from the ViewModel in the UI

Below the `viewModel` property in the `SelectPlayerScreen` composable, add the following code to collect the state from the ViewModel:

```kotlin
val uiState by viewModel.uiState.collectAsState()
```

This code collects the `uiState` StateFlow from the ViewModel and converts it into a Compose state using the `collectAsState` function. The `by` keyword is used to delegate the state to a variable `uiState`, which can be used in the UI to display the current state.

Now replace all occurrences of the old `selectedPlayerColors` state in the `SelectPlayerScreen` composable with the new `uiState.selectedPlayerColors` state from the ViewModel. Also, replace the old click logic that directly manipulated the state with the new click logic that calls the `onClickColor` function from the ViewModel. Note that the state we use in our ViewModel is again a list of `PlayerColor`, not String. For example, the call to the `SelectOptionScreen` composable should now look like this:

```kotlin
SelectOptionScreen(
    options = viewModel.availablePlayerColors.map { it.name },
    selectedOptions = uiState.selectedPlayerColors.map { it.name },
    onClickOption = viewModel::onClickColor,
    optionRow = {SelectColorRow(playerColorString = it)}
)
```

You can now remove the old `selectedPlayerColors` state from the `SelectPlayerScreen`.

Restart the app and check if the Select Player screen still works as expected.

### Fixing the Preview

You might have already noticed that the preview of the `SelectPlayerScreen` isn't working anymore. This is because the `viewModel` function doesn't work in previews—it requires an active lifecycle, which previews don't have. To fix this, we can create a `SelectPlayerContent` composable that takes the necessary state and click logic as parameters and use this composable in both the `SelectPlayerScreen` and the preview. This way, we can provide mock data and click logic for the preview without relying on the ViewModel.

For example, create the following composable in the `SelectPlayerScreen` file:

```kotlin
@Composable
fun SelectPlayerContent(
    modifier: Modifier = Modifier,
    navigateToSelectDecksScreen: () -> Unit,
    navigateBack: () -> Unit,
    playerColors: List<PlayerColor>,
    selectedPlayerColors: List<PlayerColor>,
    onClickColor: (String) -> Unit
) { 
    // The old content of the SelectPlayerScreen goes here, but with the parameters instead of the ViewModel state and logic
 }
```

Then use this composable in both the `SelectPlayerScreen` and the Preview. For example, the `SelectPlayerScreen` should now look like this:

```kotlin
@Composable
fun SelectPlayerScreen(
    modifier: Modifier = Modifier,
    navigateToSelectDecksScreen: () -> Unit,
    navigateBack: () -> Unit
) {
    val viewModel: SelectPlayerViewModel = viewModel(factory = AppViewModelProvider.Factory)

    val uiState by viewModel.uiState.collectAsState()

    SelectPlayerContent(
        modifier,
        navigateToSelectDecksScreen,
        navigateBack,
        viewModel.availablePlayerColors,
        uiState.selectedPlayerColors,
        viewModel::onClickColor
    )
}
```

## Transfer knowledge to the Select Decks screen

Now that you have set up the ViewModel and manual dependency injection for the Select Player screen, you can transfer this knowledge to the Select Decks screen.

!!! example "Task"
    Add a `SelectDecksViewModel` to the `tile.domain` package and implement similar logic as in the `SelectPlayerViewModel` for managing the state and click logic of the Select Decks screen. Use a custom `SelectDecksUiState` data class for the `_uiState` and initialize its `selectedDecks` property with the base game. Then add an `initializer` for the `SelectDecksViewModel` to the `AppViewModelProvider`. Use the `SelectDecksViewModel` in the `SelectDecksScreen` and remove all leftovers from the old `selectedDecks` state. Fix the preview the same way you did for the Select Player screen.

After you've completed this task, run the app and test that you have a fully functional Select Decks screen that's now connected to the domain layer via the `SelectDecksViewModel`.

## Summary

In this task, you set up ViewModels for the two selection screens to move state and business logic from the UI layer to the domain layer. To connect the two layers via MDI, you implemented the AppViewModelProvider that provides a factory for creating instances of ViewModels with their dependencies. Finally, you learned how to adapt the UI code so you can use the Preview again without relying on the ViewModel.

In the next task, we will learn how to connect the domain layer to the data layer to persist data between the screens and initialize the game state properly.

---

[Previous: Day 3 Overview](../index.md) | [Next: Task 2](task2.md)