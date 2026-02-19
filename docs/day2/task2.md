# Task 2: Create a reactive Screen in Jetpack Compose

In this task, we will create a screen that lets the user select and deselect players for a game.

![Select Players Prototype](../images/day2/select_player_screen_prototype.png)

## The Select Player Screen

In the finished game, the first screen that a user will see after the welcome screen ist the "Select Player Screen". This screen should let the user select the players (colors) for the game. In this task, we will implement a prototype of this screen (shown above) that incorporates basic layouts and clickability, but does not have any connection to the backend yet. We will connect the screen to the backend in later tasks.


### Preparation

Before we start implementing the screen, we need to set up the necessary data structures and packages. According to our vertical slice architecture, this means we first create a new package for the *player*-related code.

!!! example "Task"
    Create a new package `player` next to the `tile` package which includes subpackages for all three layers of the architecture (data, domain, presentation). Then add some elements similar to the `tile` package:
    
    - Create a `PlayerColor` enum in the data layer according to the game rules.
    - Create a `Player` data class that includes the player color, number of followers and points (initialize followers to the maximum number of followers and the points to 0).
    - Add a `SelectPlayerScreen.kt` file to the presentation layer and create an empty composable `SelectPlayerScreen()` that has a modifier as argument.

The maximum number of followers one player can have is a global constant and will be used in multiple places, so we can add it as a top-level constant to the `core` package. Create a `Constants.kt` file on the root level of the `core` package and add the following constant:

```kotlin
const val MAX_NUM_OF_FOLLOWERS = 8
```

Unse this constant for the initialization of the followers in the `Player` data class.

### Implement the Screen

Now let us go to the `SelectPlayerScreen()` composable. This screen should let the user select the players (colors) for the game.

First we add a Preview for the `SelectPlayerScreen()` so that we can check what we are doing along the way.

```kotlin
@Preview
@Composable
fun SelectPlayerScreenPreview() {
    SelectPlayerScreen()
}
```

The screen that we want to create should look like this:

![Select Player Screen](../images/day2/select_player_screen.png)

Think about what kind of layout structures you can see in this screen. How are the elements arranged?

!!! tip "Layout"
    The way elements are arranged to each other is one of the most important concepts in frontend development. Before you start coding, take a moment to analyze the screen you want to create and identify the layout structures - use paper and a pencil and draw different layouts until you find an ideal one. This will help you to structure your code in a way that is easy to read and maintain.

The outermost structure is a column, that arranges the single options vertically. Each option on the other hand consists of a row with a checkbox and an option. The option itself is also a row, that contains of a colored icon and a text. So we have a column that contains rows that contain rows.

We'll start simple and first just add a Column with Text elements for each player color:

```kotlin
@Composable
fun SelectPlayerScreen(
    modifier: Modifier = Modifier,
) {
    Column(
        modifier = modifier
    ) {
        PlayerColor.entries.forEach {
            Text(it.name)
        }
    }
}
```

Render the preview and check that it looks something like this:

![Player Color Column](../images/day2/player_color_string_column.png)

!!! example "Task"
    Replace the `Text` composable with a `Row` that contains a `Checkbox` composable and a `Row` for the option. The checkbox should be unchecked by default. Set `onCheckedChange = null` for now. The option row should contain an `Icon` composable using `Icons.Filled.Person` and a `Text` with the name of the color. wrap the capitalized name of the enum entry in the `capitalizeFirstChar()` function that came with the `core.domain` package to format it nicely. 

After you have completed the task, the preview should look like this:

![Player Select Black and White](../images/day2/select_player_black_white.png)

To add the colors to the icons, we can use the `tint` parameter of the `Icon` composable. But first, we need to define a mapping from the `PlayerColor` enum to actual colors.

!!! example "Task"
    Add an extension function `PlayerColor.toColorCode(): Color` to the `PlayerColor` enum that returns a color code for each player color. Use the codes provided by the `androidx.compose.ui.graphics.Color` class, for example `Color.Black` for black, `Color.Red` for red, etc.
    Then use this function to set the tint of the icon in the option row to the corresponding color of the player color.

 After you have completed this task, the preview should look like this:

![Player Select with color](../images/day2/select_player_with_color.png)

The last step for layout we have to take is to arrange and align the elements in a way that looks good. In our case, that is to align the elements such that it conforms to the shown target design.

We start with outermost column. We want to have some space between the options and that they are all aligned to the left. So we add the following parameters to the `Column`:

```kotlin
horizontalAlignment = Alignment.Start,
verticalArrangement = Arrangement.spacedBy(24.dp)
```

Next we go to the outer Row and add an alignemnt parameter to align the checkbox and option row vertically centered:

```kotlin
verticalAlignment = Alignment.CenterVertically
```

Finally, we align the elements in the option row

```kotlin
horizontalArrangement = Arrangement.Start,
verticalAlignment = Alignment.CenterVertically
```

Additionally, we add some spacing between the icon and the text using a `Spacer` composable (where do you have to put it?):

```kotlin
Spacer(modifier = Modifier.width(8.dp))
```

You should now see something like this in the preview:

![Player Select with color aligned](../images/day2/select_player_with_color_aligned.png)

You will notice that there is still a bit of a layout difference with regards to the spacing between the option row and the checkbox. This will be changed as soon as we make the checkbox clickable, because that adds some padding around it (to make it easier to click). So, before we add the clickability, we'll simulate the clickbox by adding a `Modifier.padding(12.dp)` to the checkbox and remove the verticalArrangement method from the column (because the padding already adds 24pt space (2*12pt) between the rows).

Now the Column  layour should match the one in the target design. However, the target still has another background and uses a different font. The first comes from the Surface composable, the latter from our `AppTheme`.

!!! example "Task"
    Wrap the Column in a `Surface` composable and set the background color to `MaterialTheme.colorScheme.surface`. Then wrap the `SelectPlayerScreen()` in the `SelectPlayerScreenPreview()` with the `AppTheme` composable to apply the theme to the preview.

We need to apply the theme to the preview and not the screen itself, because the app has several screens that should all use the same theme, i.e. the theme should be consistent across the complete app. Instead, we will apply the theme later on to the composable that is responsible for navigation between the screens.

### Display the Screen in the App

Now that we have created the `SelectPlayerScreen()`, we want to display it in the app. Since we currently have no navigation implemented, we will just replace the `WelcomeScreen()` with the `SelectPlayerScreen()` in the `main.kt` to check that it works:

```kotlin
fun main() = application {
    Window(
        onCloseRequest = ::exitApplication,
        title = stringResource(Res.string.app_name),
    ) {
        AppTheme {
            SelectPlayerScreen()
        }
    }
}
```

Start the app. You should see the following:

![Select Player Welcome 1](../images/day2/select_player_in_app1.png)

We can just look at it and can't click or interact with the frontend yet. This is because we haven't added any click-function and state to the screen yet. In the next task, we will add state and make the screen interactive.

### Make the screen interactive

The first thing we need to add to our Screen to make it interactive is a state that holds the selected players.

!!! example "Task"
    Add a mutable `selectedPlayerColors` state to the `SelectPlayerScreen()` that holds a list of the selected player colors. Initialize it with an empty list.

Now go to the `Checkbox` composable and set the `checked` parameter to whether the corresponding player color is in the `selectedPlayerColors` list. Then set the `onCheckedChange` parameter to a lambda that adds the player color to the list if it is not already in it, or removes it if it is already in it. Also, you can now remove the padding modifier that we added to simulate the clickbox, because the checkbox is now clickable and has its own clickbox.

```kotlin
Checkbox(
    checked = selectedPlayerColors.contains(color),
    onCheckedChange = {
        selectedPlayerColors = if (selectedPlayerColors.contains(color)) {
            selectedPlayerColors - color
        } else {
            selectedPlayerColors + color
        }
    }
)
```

Note that we can't use `selectedPlayerColors.add(color)` or `selectedPlayerColors.remove(color)` because this would not trigger a recomposition of the screen. This is because the state is not updated with a new value, but the same list is modified. By creating a new list with the added or removed color, we trigger a recomposition and the screen updates accordingly.

Start the app again and check that you can now select and deselect the player colors by clicking on the checkboxes.

#### Make clickability more easy

Right now, you can only click on the checkbox to select or deselect a player color. It would be more user-friendly if you could click on the whole row to select or deselect a player color. To do this, we can add a `Modifier.selectable()` to the outer Row of each option.

!!! example "Task"
    Add a `Modifier.selectable()` to the outer Row of each option that has the same check and lambda as the checkbox for the `selected` and `onClick` parameters. Retest the app and check that you can now click on the whole row to select or deselect a player color.



---

[Previous: Task 1](task1.md) | [Next: Day 3](../day3/index.md)