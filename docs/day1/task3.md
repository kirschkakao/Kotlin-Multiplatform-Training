# Task 3: Understand the Project Structure

### About This Task
Now that you have installed Android Studio and set up the project, it's time to explore the codebase. Understanding the structure of a Kotlin Multiplatform project is crucial for making informed decisions during development.

### Task: Explore the Carcassonne KMP Project

1. **Open the Project**: Launch Android Studio and open the Carcassonne project you cloned in Task 2.

2. **Understand the Directory Structure**: Familiarize yourself with:
    - `composeApp/` - The shared Compose Multiplatform code
    - `composeApp/src/` - Source code organized by target (common, desktopMain, etc.)
    - `shared/` - Shared Kotlin code used across platforms
    - `build.gradle.kts` - Gradle configuration files for the KMP project

3. **Examine Key Files**:
    - Look at the main App composable in `composeApp/src/desktopMain/`
    - Review the project's dependencies in the root `build.gradle.kts`
    - Understand how the project is organized with the main and shared modules

4. **Run the Project**: Follow the run configuration you set up in Task 2 to ensure everything is working correctly.

### Key Concepts
- **Shared Code**: Code in the `shared` module can be used across platforms (Android, iOS, Desktop, Web)
- **Platform-Specific Code**: Each platform has its own directory (desktopMain, androidMain, etc.) for platform-specific implementations
- **Gradle**: The build system that compiles your Kotlin code into executable applications

### What You Should Understand Now
- How the project is organized
- Where shared code lives vs. platform-specific code
- How to navigate the IDE and find files
- Confidence that the project builds and runs successfully

---

[Previous: Task 2](task2.md) | [Next: Task 4](task4.md)
