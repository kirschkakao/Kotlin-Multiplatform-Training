# Chapter 6: Classes and Objects

Kotlin is an object-oriented language with modern features. In this chapter, you will learn about different types of classes.

## Basic Classes

Classes are defined using the `class` keyword. They can have properties, methods, and constructors.

```kotlin
class Person(
    val firstName: String,
    val lastName: String
) {
    fun greet() {
        println("Hello, my name is $firstName $lastName")
    }
}
```

Class parameters defined in the primary constructor can be used as properties if prefixed with `val` or `var`.

### Open Classes

Classes in Kotlin are `final` by default. For inheritance, they must be marked as `open`:

```kotlin
enum class TerrainType {
    LAND, WATER, AIR
}

open class Vehicle(
    val modelName: String,
    val terrainType: TerrainType
) {
    protected open fun buildOutputString(): String {
        return "This is a $modelName. It operates ${if(terrainType == TerrainType.AIR) "in" else "on"} " +
                capitalizeFirstChar(terrainType.toString())
    }

    fun print() {
        numOfPrintedVehicles += 1
        println(buildOutputString())
    }

    companion object {
        private var numOfPrintedVehicles: Int = 0
        fun getNumOfPrints(): Int = numOfPrintedVehicles
    }
}
```

Functions marked as `protected` are accessible only within the class and its subclasses and functions marked as `open` can be overridden in subclasses.
A `companion object` is like a static class in Java. It enables class variables and methods.

??? example "Try it out"
    ```kotlin
    val aircraft = Vehicle("Boing 747", TerrainType.AIR)
    //aircraft.buildOutputString() // Not accessible
    aircraft.print()
    println(Vehicle.getNumOfPrints())
    ```
    **Expected Result**:
    ```
    This is a Boing 747. It operates in Air
    1
    ```

Since `getNumOfPrints()` is defined in the companion object, it can be accessed directly via the class name `Vehicle`.

### Inheritance

The Vehicle class can be extended by other classes, for example to specify different types of vehicles like cars, which have an engine type:

```kotlin
enum class EngineType {
    ELECTRIC, COMBUSTION
}

class Car(
    modelName: String,
    terrainType: TerrainType,
    val engineType: EngineType
) : Vehicle(modelName, terrainType) {
    override fun buildOutputString(): String {
        return "${super.buildOutputString()} and has a ${capitalizeFirstChar(engineType.toString())} engine"
    }
}
```

The extension is done using the `:` symbol followed by the base class constructor call.
The `super` keyword is used to access members of the base class like `buildOutputString()`.
Since `numOfPrintedVehicles` is defined in the companion object of the base class, it is shared among all subclasses.
Hence, calling `print()` on any subclass will increment the same counter.

??? example "Try it out"
    ```kotlin
    val car = Car("VW Golf", TerrainType.LAND, EngineType.COMBUSTION)
    car.print()
    println(Vehicle.getNumOfPrints())
    aircraft.print()
    println(Vehicle.getNumOfPrints())
    ```
    **Expected Result**:
    ```
    This is a VW Golf. It operates on Land and has a Combustion engine
    1
    This is a Boing 747. It operates in Air
    2
    ```

## Data Classes

Data classes are specifically designed for holding data. The compiler automatically generates:
- `equals()` / `hashCode()`
- `toString()`
- `copy()`
- `componentN()` for destructuring

Also, data classes can be marked as @Serializable for easy serialization. This means, for example, converting to and from JSON.
Data classes are therefore ideal for DTOs (Data Transfer Objects) in networking or database operations. For example,
let's assume we have a Contacts Table that can be queried using an API. Each returned row can be represented as an instance of the following data class:

```kotlin
data class Contact(
    val firstName: String,
    val lastName: String,
    val mobileNumber: String,
    val street: String? = null,
    val town: String? = null,
    val country: String? = null
)
```

### Copy Function

To manipulate data class instances in an immutable way, the `copy()` function can be used to create modified copies:

```kotlin
var contact = Contact("Bruce", "Wayne", "+123456789")
contact = contact.copy(country = "USA") // Adds country
contact = contact.copy(firstName = "Dick") // Changes first name
```

??? example "Try it out"
    ```kotlin
    var contact = Contact("Bruce", "Wayne", "+123456789")
    println(contact)
    contact = contact.copy(country = "USA")
    println(contact)
    contact = contact.copy(
        firstName = "Dick",
        lastName = "Grayson"
    )
    println(contact)
    ```
    **Expected Result**:
    ```
    Contact(firstName=Bruce, lastName=Wayne, mobileNumber=+123456789, street=null, town=null, country=null)
    Contact(firstName=Bruce, lastName=Wayne, mobileNumber=+123456789, street=null, town=null, country=USA)
    Contact(firstName=Dick, lastName=Grayson, mobileNumber=+123456789, street=null, town=null, country=USA)
    ```

### Destructuring

Like with Pair and Triple, data class instances can be destructured.

```kotlin
data class SmallContact(
    val name: String,
    val number: String
)

val smallContact = SmallContact("Bruce", "123")
val (name, nummer) = smallContact
```

??? example "Try it out"
    ```kotlin
    val smallContact = SmallContact("Bruce", "123")
    val (name, nummer) = smallContact
    println("$name: $number")
    ```
    **Expected Result**:
    ```
    Bruce: 123
    ```

## Design Patterns

Design patterns are proven solutions to common programming problems. Kotlin's language features make it easy to implement many classic design patterns elegantly and concisely. Let's explore some commonly used patterns that are particularly well-suited for Kotlin.

### Flyweight Pattern

The Flyweight pattern is a structural design pattern that **shares instances to save memory**. Instead of creating a new object every time one is requested, the pattern reuses existing instances when possible. This is particularly useful when you need many objects that share common state.

In the example below, we implement a `Player` class where each player is identified by their color. Since we might have thousands of game pieces on a board but only two players (blue and black), it makes sense to share the player instances rather than creating a new `Player` object for each piece. The pattern ensures that for each `PlayerColor`, only one `Player` instance exists in memory.

The key elements of this implementation are:
- **Private constructor**: Prevents direct instantiation from outside the class
- **Companion object**: Acts as a factory to manage and retrieve instances
- **Instance cache**: A `MutableMap` stores already created instances by their color

```kotlin
enum class PlayerColor {
    BLUE, BLACK
}

class Player private constructor(val playerColor: PlayerColor) {
    private var points: Int = 0

    companion object {
        private var INSTANCES: MutableMap<PlayerColor, Player> = mutableMapOf()
        
        fun getPlayer(playerColor: PlayerColor): Player {
            return INSTANCES[playerColor] ?: Player(playerColor).also { 
                INSTANCES[playerColor] = it 
            }
        }
    }
}
```

??? example "Try it out"
    ```kotlin
    val blue = Player.getPlayer(PlayerColor.BLUE)
    val blue2 = Player.getPlayer(PlayerColor.BLUE)
    println(blue === blue2) // Reference equality check
    ```
    **Expected Result**:
    ```
    true
    ```

### Singleton Pattern

The Singleton pattern is a creational design pattern that **ensures only one instance of a class exists** throughout the application's lifecycle. This is useful for resources that should be shared globally, such as database connections, configuration managers, or logging systems.

In the example below, we implement a `Database` class as a singleton. No matter how many times `getDatabase()` is called from different parts of the application, the same `Database` instance will be returned. This prevents multiple database connections from being created unnecessarily, which could waste resources and cause synchronization issues.

The implementation uses:
- **Private constructor**: Prevents external code from creating new instances using `Database()`
- **Nullable INSTANCE variable**: Stores the single instance, initially `null`
- **Factory method `getDatabase()`**: Provides controlled access to the instance
- **Lazy initialization**: The instance is only created when first requested, not at application startup

```kotlin
class Database private constructor() {
    companion object {
        private var INSTANCE: Database? = null

        fun getDatabase(): Database {
            return INSTANCE ?: build().also { INSTANCE = it }
        }

        private fun build(): Database {
            //Initialization code
            return Database()
        }
    }
}
```

??? example "Try it out"
    ```kotlin
    val db1 = Database.getDatabase()
    val db2 = Database.getDatabase()
    println(db1 === db2) // Reference equality check
    ```
    **Expected Result**:
    ```
    true
    ```

### Object Declaration

While the traditional Singleton pattern works well, Kotlin provides a more elegant and concise syntax for creating singletons: **object declarations**. An `object` in Kotlin is both a class declaration and a single instance of that class, combined into one statement. The Kotlin compiler handles all the singleton logic automatically—no companion objects, no private constructors, and no manual instance management needed.

In the example below, we create an `AppContainer` object that serves as a dependency injection container for the application. The `contactTable` property uses **lazy initialization** with the `by lazy` delegate, meaning it won't be created until it's first accessed. This is a common pattern in Android development for managing application-level dependencies.

Key advantages of object declarations:
- **Concise syntax**: No boilerplate code required
- **Thread-safe**: Kotlin guarantees thread-safe initialization
- **Lazy by default**: The object is created only when first accessed
- **Can implement interfaces**: Objects can inherit from classes and implement interfaces

```kotlin
object AppContainer {
    val contactTable: List<String> by lazy {
        listOf()
    }
}
```

!!! tip "Lazy Initialization"
    `by lazy` initializes the property only on first access.

## Abstract Classes

Abstract classes cannot be instantiated and serve as base classes. They can contain both abstract methods
(without implementation) and concrete methods (with implementation). Subclasses must implement the abstract methods.
Also, in contrast to interfaces, abstract classes can have state (properties) that is shared among subclasses, like the `callCount` below.

```kotlin
abstract class Person(protected val name: String) {
    private var ownCallCount: Int = 0
    
    protected fun logCalls() {
        callCount++
        ownCallCount++
        println("I was called $ownCallCount times but in general there were already $callCount calls.")
    }

    abstract fun greet()
    
    companion object {
        private var callCount: Int = 0
    }
}

class Batman : Person("Bruce") {
    override fun greet() {
        println("I am Batm.. ah no shit I meant I am $name")
        logCalls()
    }
}

class Robin : Person("Dick") {
    override fun greet() {
        println("Hi, I am $name")
        logCalls()
    }
}
```

??? example "Try it out"
    ```kotlin
    val bruce = Batman()
    val dick = Robin()
    val dick2 = Robin()
    bruce.greet()
    dick.greet()
    dick2.greet()
    ```
    
    **Expected Result**:
    ```
    I am Batm.. ah no shit I meant I am Bruce
    I was called 1 times but in general there were already 1 calls.
    Hi, I am Dick
    I was called 1 times but in general there were already 2 calls.
    Hi, I am Dick
    I was called 1 times but in general there were already 3 calls.
    ```

## Interfaces

Interfaces define **contracts** that classes must implement. They specify what a class must do, but not how it does it. Unlike abstract classes, interfaces cannot hold state (non-initialized properties), but they can contain:
- Abstract properties (must be overridden)
- Abstract methods (must be implemented)
- Concrete methods with default implementations
- Properties with custom getters

Interfaces are particularly useful for defining common behavior across unrelated classes. For example, in Android development, navigation destinations share common properties regardless of their specific screen implementation:

```kotlin
interface NavigationDestination {
    val route: String        // Navigation route identifier
    val titleRes: Int        // Resource ID for the screen title
}

object ContactDetailsScreen : NavigationDestination {
    override val route = "contact_details"
    override val titleRes = 0
}
```

A class or object can implement multiple interfaces (multiple inheritance of behavior), which is not possible with classes:

```kotlin
interface Clickable {
    fun click()
}

interface Focusable {
    fun focus()
    fun blur() {
        println("Lost focus")  // Default implementation
    }
}

class Button : Clickable, Focusable {
    override fun click() {
        println("Button clicked")
    }
    
    override fun focus() {
        println("Button focused")
    }
    // blur() is inherited with default implementation
}
```

??? example "Try it out"
    ```kotlin
    val button = Button()
    button.click()
    button.focus()
    button.blur()
    ```
    
    **Expected Result**:
    ```
    Button clicked
    Button focused
    Lost focus
    ```

**When to use Interfaces vs. Abstract Classes:**
- Use **interfaces** when you need to define pure contracts without shared state
- Use **abstract classes** when you need to share state or implementation among subclasses
- Interfaces support multiple inheritance, abstract classes don't

## Class Types Comparison

| Type | Use Case | Can Have State? | Can Be Instantiated? | Inheritance |
|------|----------|----------------|---------------------|-------------|
| **Regular Class** | General OOP, custom behavior | Yes | Yes | Single inheritance |
| **Open Class** | Base class for inheritance | Yes | Yes | Can be extended |
| **Data Class** | Data containers (DTOs, models) | Yes | Yes | Can inherit, limited extension |
| **Abstract Class** | Shared implementation + contracts | Yes | No | Single inheritance |
| **Interface** | Pure contracts, multiple behaviors | No (only getters) | No | Multiple inheritance |
| **Object** | Singleton, utility functions | Yes | No (single instance) | Can implement interfaces |
| **Companion Object** | Static-like members in classes | Yes | No | Part of enclosing class |
| **Enum** | Fixed set of constants | Yes (per constant) | No (predefined) | Cannot be extended |

## Summary

- **Classes** are the foundation of object-oriented programming in Kotlin
- **Open classes** allow inheritance - classes are `final` by default for safety
- **Inheritance** uses the `:` syntax and enables code reuse through extension
- **Data classes** automatically generate `equals()`, `hashCode()`, `toString()`, `copy()`, and destructuring
- **Copy function** enables immutable data manipulation of data class instances
- **Companion objects** provide class-level (static-like) members
- **Design Patterns**:
  - **Flyweight**: Shares instances to save memory using a cache
  - **Singleton**: Ensures only one instance exists (manual or via `object`)
  - **Object declarations**: Kotlin's elegant, thread-safe singleton syntax
- **Abstract classes** define shared implementation and state for subclasses
- **Interfaces** define pure contracts and support multiple inheritance
- Choose the right type based on your needs: state, instantiation, and inheritance requirements

## Best Practices

1. **Default to `final`**: Mark classes as `open` only when inheritance is explicitly desired - Kotlin's default is safer
2. **Use data classes for DTOs**: Perfect for Data Transfer Objects, API responses, and database entities
3. **Prefer `object` for singletons**: Use object declarations instead of manual singleton implementations
4. **Choose wisely between abstract classes and interfaces**:
    - Use **abstract classes** when you need shared state or partial implementation
    - Use **interfaces** when defining pure contracts or needing multiple inheritance

5. **Leverage companion objects**: Use them for factory methods, constants, and class-level utilities
6. **Apply design patterns appropriately**:
    - **Flyweight** for memory-intensive scenarios with many similar objects
    - **Singleton** for shared resources (databases, configurations, loggers)

7. **Use `copy()` for immutability**: When working with data classes, prefer `copy()` over mutation
8. **Destructuring for readability**: Use destructuring with data classes when you need multiple properties
9. **Visibility modifiers**: Use `protected` for inheritance-specific members and `private` for internal implementation

## Further Reading

- [Kotlin Documentation: Classes](https://kotlinlang.org/docs/classes.html)
- [Kotlin Documentation: Data Classes](https://kotlinlang.org/docs/data-classes.html)
- [Kotlin Documentation: Objects](https://kotlinlang.org/docs/object-declarations.html)

[← Back to Chapter 5: Functional Patterns](05_functional_patterns.md) | [Continue to Chapter 7: Coroutines →](07_coroutines.md)