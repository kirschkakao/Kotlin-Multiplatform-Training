# Task 1: Get to know Jetpack Compose and Material Design 3

In this task, we will learn what [Jetpack Compose](https://developer.android.com/compose) and [Material Design 3](https://m3.material.io/) are and how we use them to build the frontend.

## The Welcome screen

Navigate to the `desktopMain` package and take a look at the `main.kt`. Here you see the `main()` function that defines the entry point of our app. As you can see, the `main()` is just the `application()` function that is assigned to it.
Hover over the function name so that the context menu appears. You see the function definition with the possible arguments and an explanation which at the beginning states "An entry point for the Compose application." Note that `application()` takes two arguments, a Boolean and a lambda, annotated with `@Composable` - the most important building block in Jetpack Compose.

!!! tip "Lambda arguments"
    Recall that in Kotlin, if the last argument of a higher-order function is a lambda, the lambda can be written outside the round parentheses.

### Composables

The composable that is passed to the application is a `Window()`. Hover over the function name to inspect the function definition and note that (1) the `@Composable` annotation makes the function a composable and (2) the last argument of `Window()` is again a composable lambda. Hover over `AppTheme()` and `WelcomeScreen()` and note that these two characteristics are also true for them.

!!! tip "Composables"
    This is an important pattern in the Jetpack Compose framework. Every element is a composable and always also a potential container of other composables.

The outermost composable `Window()` handles the creation of the actual window that opens when you start the app. Comment out the lambda that is passed to `Window()` and start the app. An empty window titled "Carcassonne" opens.

The injected `AppTheme()` composable sets the UI theme, i.e. defines colors, fonts, typography, and shapes. Press CTRL and click on `AppTheme()` to jump to the function definition in the `ui.theme/Theme.kt` file. Here you can see that `AppTheme()` just calls another composable named `MaterialTheme()`. This function sets the theme for the complete app based on Material Design 3 principles according to the arguments `colorScheme`, `typography`, and `shapes`.

### Material Design 3

To cite the [Material Design 3 website](https://m3.material.io/), "Material Design 3 is Google’s open-source design system for building beautiful, usable products." In fact, Material Design 3 is a very well thought-through, comprehensive, and well-documented design system that makes it exceptionally easy to build state-of-the-art frontends based on years of UI/UX research.

!!! example "Task"
    Go to the [Material Design 3 website](https://m3.material.io/) and familiarize yourself with the presented information - especially **Foundations**, **Styles** and **Components**.

#### Color scheme

Go to Styles -> Colors -> [Color roles](https://m3.material.io/styles/color/roles) and check the diagram of all Material color roles. You will notice that the roles presented here match the arguments in the `lightColorScheme` factory function in your open `Theme.kt`. This is no coincidence: we deliberately define our color scheme based on Material Design 3 principles using the builder functions provided by the androidx.compose.material3 package.

Designing a color scheme that conforms to modern color scheme rules, especially with regards to contrast, readability and joy of looking at it, is a difficult task. Fortunately, Material Design provides a [Theme-Builder](https://material-foundation.github.io/material-theme-builder/) where you can build a color scheme based on a picture or one or more core colors.
The builder then provides complete color schemes for light and dark mode and three different contrast levels each that you can export and directly use in your `ui.theme` package. In fact, the light color scheme that came with the code was created using the theme builder based on the Carcassonne logo.

!!! example "Task"
    Play with the theme builder and create and export a color scheme to use in your code. See how the appearance of the application changes with the scheme (maybe you even want to try a high contrast or dark mode?).
    You may use the scheme that you created yourself or stick to the one that came with the course (just revert the files).

#### Typography

Now go back to the `MaterialTheme()` constructor in the `Theme.kt` and jump to the definition of `AppTypography` (CTRL + Click). As you can see, we just use the default definitions provided by the design kit, i.e. we do not inject anything into the `Typography()` constructor. On the Material Design 3 website, go to Styles -> Typography and then in the top menu to "Type scale & tokens". Scroll to section "M3 type scale". Then hover over the `Typography()` constructor and compare the constructor arguments with the presented type scales. As with the color scheme, the names are identical.

#### Use styling in code

The fact that the styles in the styling guide and the code objects have the same name supports the easy application of the rules presented in the guides. For example, you can see where to use which font scale or color and in which combination because you don't have to think about exact numbers or color values. We will learn how to use the styling variables later on.

#### Components

The last and arguably most important part of the website in daily use is the **Components** section. Here you find every frontend element that Material Design 3 provides and how they can and should be used.

!!! example "Task"
    On the Material Design website, go to Components -> Cards and read through all subsections, i.e. Overview, Specs, Guidelines and Accessibility

Now that you have learned what cards in Material Design 3 are, what their specs are and how you should use them, let's find out how you can implement them in the code.

### Back to Jetpack

Go back to the "Overview" subsection of the "Cards" page and scroll to "Availability & resources". Here you see the categories "Design" and "Implementation". We will come back to "Design" later in the course. Click on "Jetpack Compose". The link leads to the [documentation of Jetpack Compose](https://developer.android.com/develop/ui/compose/documentation) and - specifically - the implementation details of the composable `Card`.

!!! example "Task"
    Read through the [Card](https://developer.android.com/develop/ui/compose/components/card) and one or two more "Components" articles to familiarize yourself with the way implementation details are presented here (i.e. how you can get help) and how components are implemented in theory.

Now go back to the `main.kt` and scroll to the `WelcomeScreen()` composable. This is the only screen of our app at the moment. Press CTRL and click on the `WelcomeScreen()` function to jump to the function definition in the `welcome.presentation` package of `commonMain`. As you can see, the `WelcomeScreen()` is a custom composable that we created to display the screen you can see when you start the app, including the logo fly-in-and-out animation.

Now let's find out how this screen is built. As stated before, every element in Jetpack Compose is a composable and every composable can be a container for other composables.

#### Layout composables

The outermost element of the `WelcomeScreen` is a Surface composable. Think of it as a canvas with a specific color on which we place all other elements (note that we set the color of the surface using the `surface` attribute of the Material Design color scheme - can you find the role name in the [Color roles](https://m3.material.io/styles/color/roles)?).<br>
On the surface, we have a Column composable. This kind of composable, together with Box and Row, are the most important layout composables in Jetpack Compose. They are containers that define how their children are arranged. A Column arranges its children vertically, a Row arranges them horizontally and a Box stacks them on top of each other.

!!! example "Task"
    Read [Compose layout basics](https://developer.android.com/develop/ui/compose/layouts/basics) to learn more about layout composables and how to use them.

In our case, the Column contains two elements: A [Button](https://developer.android.com/develop/ui/compose/components/button) and another Column which is wrapped into an [AnimatedVisibility](https://developer.android.com/develop/ui/compose/animation/composables-modifiers#built-in-animated-composables) composable. The latter is responsible for the fly-in-and-out animation of the logo.<br>
The inner Column contains an Image composable that displays the logo and a Text composable that displays the text "The journey begins here". Note that elements in a Column are arranged according to the order in which they are declared in the code, i.e. the element declared first is at the top (for Rows, the element declared first is on the left and for Boxes, the element declared first is at the bottom of the stack).

The way Columns and Rows arrange and align their children can be modified using arrangement and alignment parameters. It depends on the composable to which direction (horizontally or vertically) these parameters are applied.
For example, in a Column which arranges its children vertically, the parameter to arrange the children vertically is `verticalArrangement` and the parameter to align the children horizontally is `horizontalAlignment`. In a Row, which arranges its children horizontally, it's the opposite.

Check out [this codelab](https://developer.android.com/codelabs/basic-android-kotlin-compose-add-images#4) and scroll to **Layout Modifiers**. There you can find some further information and some neat animations at the bottom that explain what the different arrangement modes do in a Column and a Row.

#### Modifiers

You might have noticed that we inject a `Modifier` into almost every composable. Modifiers are a powerful tool in Jetpack Compose that allow you to change the appearance and behavior of composables. They can be used to set properties like size, padding, background color, click listeners and much more. They are applied in the order they are declared, which means that the order of modifiers can affect the final result.<br>
Also note that sometimes we use fresh Modifier objects and sometimes we chain modifiers to the one that is injected into the composable. This is because we want to allow the caller of the composable to inject their own modifiers to change the appearance and behavior of the composable from the outside. If we always created fresh Modifier objects, we would override any modifiers injected by the caller and make it impossible to customize the composable from the outside.

!!! example "Task"
    Read [Compose modifiers](https://developer.android.com/develop/ui/compose/modifiers) to learn more about modifiers and how to use them.

#### State

The last important concept in Jetpack Compose is state. State is any value that can change over time and that affects the appearance or behavior of the UI, because any time a state is updated a *recomposition* takes place. This means that the composable is re-executed with the new state value and the UI is updated accordingly.

This is a fundamental difference from imperative UI frameworks, where you have to manually update the UI when a state changes, for example by calling methods like `setText()` or `setVisibility()`. In Jetpack Compose, you just update the state and the framework takes care of updating the UI for you.

Think, for example, of a simple text field that lets the user input text, which is displayed and updated while the user is typing. In an imperative UI framework, you would have to set a listener on the text field that listens for text changes and then manually update the text field with the new text value. In Jetpack Compose, you just have to define a state variable that holds the current text value and pass it to the text field composable. Whenever the user types something the state variable changes and the text field will automatically update itself with the new value.

!!! example "Task"
    Read [State in Compose](https://developer.android.com/develop/ui/compose/state) to learn more about state and how to use it.

In our `WelcomeScreen`, we have a mutable boolean state variable `showContent` that controls whether the logo and the text are visible or not. When the button is clicked, we toggle the value of `showContent`, which triggers a recomposition of the UI and causes the logo and text to either fly in or fly out depending on the new value of `showContent`.

Note that we wrap the state variable with the delegate `remember`. This is because we want the state to survive recompositions, i.e. we want the value of `showContent` to be preserved when the UI is recomposed. The `remember` delegate allows us to achieve this by storing the state in a way that it is not lost during recomposition. Also, we need `remember` to transform the observable type of the state variable into a regular variable that we can read and write to.

#### Resources

The two Text composables that we use in the `WelcomeScreen` are hardcoded String literals. This is in general not good practice, because it makes it difficult to change the text later on and also makes it impossible to support multiple languages. A better way to handle this is to define the text in a resource file and then load the text from the resource file in the code. This way, you can easily change the text later on by just changing the resource file and you can also support multiple languages by providing different resource files for different languages.

Go back to the `main.kt`. Note that the title parameter of the `Window()` composable is set via the `stringResource()` function which is called with a string resource ID. Jump to the definition of `app_name` in the `strings.xml` file in the `commonMain/composeResources/values` package. Here you can see that the title of the window is defined as a string resource.

!!! example "Task"
    Add resource strings for the two Text composables in the `WelcomeScreen`, i.e. "Click me!" and "The journey begins here". Then load these strings from the resource file in the code using the `stringResource()` function.

While working on the above task, you will notice that your new resources are not suggested by the code-completion functionality of your IDE and that even if you type it out manually, it is regarded as an error. This is because the resource IDs are generated by the build system and are not available until the project is built. To fix this, you need to build the project once after adding new resources (start the app once or just assemble it). After the build, the resource IDs will be generated and you can use them in your code without any errors.

#### Previews

Frontend development is visual by nature — you're creating something whose whole purpose is to be seen on screen. This makes iterative development different from typical backend code: you can't really understand what your code does without seeing the actual UI it produces. But running the app every time you make a small change gets tedious fast.

Jetpack Compose has an elegant solution: Previews. A preview lets you see what a composable looks like without running the entire app. You create one by annotating a function with `@Preview` and calling the composable you want to see inside.

Android previews are fancy — they show automatically and give you detailed layout information. Kotlin Multiplatform previews are a bit more straightforward: you see the composable itself, and you have to manually render it by clicking the Compose Multiplatform icon next to the preview function definition.

Take a look at `WelcomeScreen.kt` and find the `WelcomeScreenPreview()` function. This previews your `WelcomeScreen()` composable. Click the [Jetpack Compose](https://developer.android.com/compose) icon next to it to open the "Desktop Preview" sidebar. You'll see a static snapshot of the `WelcomeScreen()` — in this case, with the logo and text hidden (the initial state).

!!! example "Task"
    Change the initial state of `WelcomeScreen()` such that it shows the logo and text in the preview. Then change the state again to hide the logo and text and check how the preview changes accordingly.


## Summary

You now know how Jetpack Compose structures UI through composables, layout containers, modifiers, and state, and you have seen how Material Design 3 turns design rules into concrete, reusable code objects such as color roles and typography tokens. You've learned how to read the official documentation effectively and how to navigate between design guidelines and implementation details. Plus, you've discovered Previews — a way to iterate on your UI quickly without constantly running the app. You're ready to build more complex screens.

In the next task, we will learn how to create additional screens and navigate between them.

---

[Previous: Day 2 Overview](../index.md) | [Next: Task 2](task2.md)