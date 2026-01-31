# Kotlin Introduction - Self-Study

Welcome to the Kotlin Introduction! This tutorial will guide you step by step through the most important concepts of the Kotlin programming language.

## What is Kotlin?

Kotlin is a modern, statically typed programming language developed by JetBrains. It is fully interoperable with Java and runs on the Java Virtual Machine (JVM).

## Why Kotlin?

- **Modern**: Kotlin offers modern language features like Null-Safety, Extension Functions, and Coroutines
- **Safe**: Through the static type system and Null-Safety, many errors are caught at compile time
- **Interoperable**: Kotlin can seamlessly work with Java code
- **Concise**: Less boilerplate code than Java
- **Versatile**: Can be used for backend, Android, multiplatform, and more

## About This Course

This course was developed to teach you the fundamentals of Kotlin through self-study. Each chapter contains:

- **Theoretical explanations** of concepts
- **Code examples** to follow along
- **Expected results** for self-assessment

## Prerequisites

- Basic programming knowledge (ideally Java)
- IntelliJ IDEA or another Kotlin-supporting IDE

## Course Structure

The course is divided into 8 chapters:


### [Chapter 1: Primitive Data Types](01_primitives.md)
Boolean, Strings, Chars, Numbers, val vs var, Null, Unit

### [Chapter 2: Collections](02_collections.md)
Pair, Triple, List, Set, SortedSet, LinkedHashSet, Map, LinkedHashMap, Enum

### [Chapter 3: Functions](03_functions.md)
Free Functions, Higher-Order Functions, Extension Functions, Generics

### [Chapter 4: Null-Safety and Scope Functions](04_null_safety_and_scope_functions.md)
Nullable Types, Safe Call Operator, Elvis Operator, Not-Null Assertion, let, run, also, apply, with

### [Chapter 5: Functional Patterns](05_functional_patterns.md)
map, filter, fold, groupBy, Chaining Operations, Sequences

### [Chapter 6: Classes and Objects](06_classes.md)
Classes, Inheritance, Data Classes, Design Patterns, Abstract Classes, Interfaces

### [Chapter 7: Coroutines](07_coroutines.md)
Asynchronous Programming, launch, async/await, Structured Concurrency

## How to Work with the material

1. **Read each chapter carefully**
2. **Study the code examples and execute them in your IDE**
3. **Compare your results** with the expected results
4. **Experiment** with the examples

## Your Workspace

Start IntelliJ and create a new Kotlin Project using `File -> New -> Project...` and then choosing Kotlin on the left. Setup your Project like shown and click create:

![new project](../../images/day1/kotlin_introduction/new_project.png)

Wait for the IDE to finish importing the project (lower right)

![new project](../../images/day1/kotlin_introduction/import_project.png)

After the import is finished, your project structure should look like this:

![new project](../../images/day1/kotlin_introduction/initial_project_structure.png)

Right click the blue `Kotlin`-folder and shoose `New -> Kotlin Class/File`. Then add a Main.kt:

![new project](../../images/day1/kotlin_introduction/new_main.png)

Open the Main.kt and add a main function:

![new project](../../images/day1/kotlin_introduction/main_fun.png)

Click the little green triangle/arrow left of the function to build/run the application. You are now set to follow along the course.

You might want to add new files for every chapter to define variables and classes etc. to manage the contents. However, everything is executed through the main-function so you will have to add executable code like the given examples to main in order to run it.


## Tips for Successful Learning

!!! tip "Learning Tips"
    - Take your time with each chapter
    - Experiment with the examples
    - Change values and observe what happens
    - Try to create your own examples
    - Take breaks between chapters

## Helpful Resources

- [Official Kotlin Documentation](https://kotlinlang.org/docs/home.html)
- [Kotlin Playground](https://play.kotlinlang.org/) - Online IDE
- [Kotlin Koans](https://play.kotlinlang.org/koans) - Interactive exercises

## Let's Get Started!

Ready to begin? Start with [Chapter 1: Primitive Data Types](01_primitives.md)!

