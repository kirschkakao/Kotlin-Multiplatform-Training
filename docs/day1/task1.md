# Task 1: Tool Installation and Setup

### Why Android Studio?
Android Studio is the official IDE for Android development, but it's also excellent for Kotlin Multiplatform (KMP) projects. It provides powerful tools for code editing, debugging, and project management. With the Kotlin Multiplatform plugin, you can easily create and manage shared code across platforms. The Compose Multiplatform plugin enables you to build modern, declarative UIs that work on desktop, mobile, and web – perfect for our Carcassonne game!

### Task: Install Tools and Set Up the Project
1. **Download and Install Android Studio**: Go to the [Android Studio download page](https://developer.android.com/studio) and download the latest version for Windows. Follow the installation wizard.
2. **Install Required Plugins**:
   - Open Android Studio and go to **File > Settings > Plugins**.
   - Search for and install the **Kotlin Multiplatform** plugin (enables KMP project creation and management).
   - Search for and install the **Compose Multiplatform IDE Support** plugin (provides better support for Jetpack Compose in multiplatform projects).
3. **Install SDKs**: Ensure the Android SDK is installed (Android Studio usually handles this). For desktop, make sure Java JDK is available.
4. **Check Out the Starting Repository**: Clone or download the start repository from the provided link. It contains an empty KMP app for Windows with the Carcassonne logo and assets.
5. **Run the App**: Open the project in Android Studio, select the desktop run configuration, and click Run. The app should start and display the Carcassonne logo.

### What It Should Look Like Now
- Android Studio is installed and running.
- The plugins are installed (check in Settings > Plugins).
- The project opens without errors.
- Running the app shows a window with the Carcassonne logo on Windows.

!!! tip
    If you encounter issues, check the [Android Studio setup guide](https://developer.android.com/studio/install) or ask the trainer.

---

[Previous: Day 1 Overview](../index.md) | [Next: Task 2](task2.md)