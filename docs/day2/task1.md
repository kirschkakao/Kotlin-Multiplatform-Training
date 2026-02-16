# Task 1: Building a Tile view

In this task we will learn how to use [Jetpack Compose](https://developer.android.com/compose) to build the frontend while adding an element to the screen that shows a tile and has buttons to rotate it.

## The Welcome screen

Navigate to the `desktopMain` package and take a look at the `main.kt`. Here you see the `main()` function that defines the entry point of our app. As you can see, the `main()` is just the `application()` function that is assigned to it. Hover over the function name so that the context menu appears. You see the function definition with the possible arguments and an explanation which at the beginning states "An entry point for the Compose application." Note that `application()` takes two arguments, one boolean and one lambda, annotated with `@Composable` - the most important building block in Jetpack Compose.

!!! tipp "Lambda arguments"
    Recall that in Kotlin, if the last argument of an higher order function is a lambda, the lambda can be written outside the round paranthesis

### Composables

The composable that is passed to the application is a `Window()`. Hover over the function name to inspect the function definition and note that 1. the `@Composable` annotation that makes the function a composable and 2. that the last argument of `Window()` is again a composable lambda. Hover over `AppTheme()` and `WelcomeScreen()` and note that these two characteristics are also true for them.

!!! tipp "Composables"
    This is an important pattern in the Jetpack Compose framework. Every element is a composable and always also a potential container of other composables.

The outermost composable `Window()` handles the creation of the actual window that opens when you start the app. Comment out the lambda that is passed to `Window()` and start the app. An empty window titled "Carcassonne" opens.

The injected `AppTheme()` composable sets the UI theme, i.e. defines colors, fonts, typography etc. Press CTRL and click on `AppTheme()` to jump to the function definition in the `ui.theme/Theme.kt` file. Here you can see that `AppTheme()` just calls another composable named `MaterialTheme()`. This function sets the theme for the complete app based on Material Design principles according to the arguments `colorScheme`, `typography` and `shapes`.

### Material Design 3

To cite the [Material Design website](https://m3.material.io/), "Material Design 3 is Google’s open-source design system for building beautiful, usable products." In fact, Material Design 3 is a very well thought-through, comprehensive, well-documented design framework, that makes it exceptionally easy to build state-of-the-art frontends based on years of UI/UX research.

!!! example "Task"
    Go to the [Material Design website](https://m3.material.io/) and familiarize yourself with the presented information - especially **Foundations**, **Styles** and **Components**.



Press CTRL and click on the `WelcomeScreen()` function to jump to the function definition in the `welcome.presentation` package of `commonMain`. Here you see the What you find here is the code that defines how the - currently - only screen of our Carcassonne app looks and behaves.
 

---

[Previous: Day 2 Overview](../index.md) | [Next: Task 2](task2.md)