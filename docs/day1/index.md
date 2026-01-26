# Day 1: Setup, Kotlin Basics and Tile Parser

[Task 1: Tool Installation and Setup](task1.md)

### Introduction to Kotlin
[Kotlin](https://kotlinlang.org/) is a modern, concise, and safe programming language developed by JetBrains. It runs on the Java Virtual Machine (JVM), making it fully interoperable with Java, and is designed for better productivity and fewer errors. Kotlin is officially supported for Android development and is increasingly used for server-side, web, and multiplatform projects.

#### Why Kotlin?
- **Concise Syntax**: Less boilerplate code compared to Java – e.g., no semicolons needed, smart type inference.
- **Null Safety**: Built-in null checks prevent common runtime errors like NullPointerExceptions.
- **Interoperability**: Seamlessly works with existing Java code and libraries.
- **Modern Features**: Supports functional programming, coroutines for asynchronous code, and more.

#### Kotlin Basics You'll Learn Today
- **Variables and Types**: Declare variables with `val` (immutable) or `var` (mutable). Kotlin infers types automatically: `val name = "Carcassonne"` (String).
- **Functions**: Define reusable code blocks. Example: `fun greet(name: String): String = "Hello, $name!"`
- **Classes and Data Classes**: Create objects. Data classes are perfect for holding data: `data class Tile(val id: Int, val name: String)`.
- **Null Safety**: Use `?` for nullable types and safe calls (`?.`) or Elvis operator (`?:`) to handle nulls gracefully.

!!! tip
    Practice these basics in the IDE. Kotlin's official documentation has interactive examples to try out.

### Introduction to Kotlin Multiplatform (KMP)
[Kotlin Multiplatform](https://kotlinlang.org/multiplatform/) (KMP) is a technology that allows you to write code once and run it on multiple platforms, such as Android, iOS, desktop (Windows, macOS, Linux), and web. It shares business logic while keeping platform-specific UI and APIs separate.

#### Key Concepts
- **Shared Code**: Write common logic (e.g., data models, networking) in Kotlin, compiled to platform-specific binaries.
- **Why KMP?**: Reduces code duplication, speeds up development for cross-platform apps, and leverages Kotlin's strengths across ecosystems.

In this training, we'll build a desktop app for Windows, but the concepts apply to mobile and web too.

---

[Task 1: Tool Installation and Setup](task1.md) | [Task 2: Program the Tile Parser](task2.md) | [Next: Day 2](../day2/index.md)