# Chapter 3: Functions

Functions are reusable blocks of code that perform a specific task. Kotlin provides powerful features for functional programming.

## Free Functions

In Kotlin, functions don't need to be defined inside classes. These functions are known as "top-level functions".

### Easy functions

```kotlin
fun greet(name: String) {
    println("Hello $name")
}
```

??? example "Try it out"
    ```kotlin
    greet("Bruce")
    ```
    **Expected Result**:
    ```
    Hello Bruce
    ```

### Functions with return value

```kotlin
fun capitalizeFirstChar(s: String): String {
    return s.lowercase().replaceFirstChar { it.uppercase() }
}
```

??? example "Try it out"
    ```kotlin
    println(capitalizeFirstChar("bRucE"))
    ```
    **Expected Result**:
    ```
    Bruce
    ```

### Function use in another function

```kotlin
fun smartGreet(name: String) {
    println("Hello ${capitalizeFirstChar(name)}")
}
```

??? example "Try it out"
    ```kotlin
    smartGreet("bRucE")
    ```
    **Expected Result**:
    ```
    Hello Bruce
    ```

## Higher-Order Functions

Higher-Order Functions are functions that take other functions as parameters or return functions.

```kotlin
fun freeGreet(name: String, manipulationFun: (String) -> String) {
    println("Hello ${manipulationFun(name)}")
}
```

Functions have also types:

| Function Type           | Description |
|-------------------------|-------------|
| `() -> Unit`            | No parameters, no return value |
| `(String) -> String`    | One String parameter, String return |
| `(Int, Int) -> Boolean` | Two Int parameters, Boolean return |

Function types `(String) -> String` can be read as "a function that takes a String and returns a String". Likewise,
function types can have multiple parameters, e.g., `(Int, Int) -> Boolean` is a function type that takes two `Int` parameters and returns a `Boolean`.

Functions can be passed either as a function reference or as a lambda expression.

### Using a function reference

```kotlin
freeGreet("bRucE", ::capitalizeFirstChar)
```

Function references use the `::` operator to refer to an existing function by name.
Note, that the signature of the function must match the expected type. For example, `capitalizeFirstChar` has the
type `(String) -> String`, which matches the expected type of `manipulationFun`. 

If the referenced function is not a top-level function, you can also reference member functions using the same syntax like so: `ClassName::memberFunctionName`.

??? example "Try it out"
    ```kotlin
    freeGreet("bRucE", ::capitalizeFirstChar)
    ```
    **Expected Result**:
    ```
    Hello Bruce
    ```

### Using a lambda expression

```kotlin
freeGreet("Bruce", { name -> name.toCharArray().sorted().joinToString("") })
```

Lambdas are anonymous functions that can be defined inline. The syntax is `{ parameters -> body }`.
In Kotlin, if the last parameter of a function is a function, you can place the lambda outside the parentheses.

```kotlin
freeGreet("Bruce") { name ->
    name.toCharArray().sorted().joinToString("")
}
```

The parameter that is passed to the lambda can also be referenced using the implicit name `it`, if there is only one parameter.

```kotlin
freeGreet("Bruce") {
    it.toCharArray().sorted().joinToString("")
}
```

All of the above examples will output `Hello Bceru`.

## Extension Functions

Extension functions are a way to add new functionality to existing classes without modifying their source code.

### Simple Extension Function

```kotlin
fun GermanState.next(): GermanState {
    val values = enumValues<GermanState>()
    return values[(ordinal + 1) % values.size]
}
```

This example adds a `next()` function to the `GermanState` enum class, which returns the next state in the enum, wrapping around to the first state after the last one.

??? example "Try it out"
    ```kotlin
    println(GermanState.BAYERN.next())
    ```
    **Expected Result**:
    ```
    BERLIN
    ```

In this specific example, we could as well have just added the `next()` function directly to the definition of the `GermanState` enum class:

```kotlin
enum class GermanState {
    BADEN_WUERTTEMBERG,
    BAYERN,
    ...
    SCHLESWIG_HOLSTEIN,
    THUERINGEN;

    fun next(): GermanState {
        val values = enumValues<GermanState>()
        return values[(ordinal + 1) % values.size]
    }
}
```

In fact, extension functions are especially useful for adding functionality to classes we do not own, such as standard library classes or third-party library classes.
For instance, we could add a `capitalizeFirstChar()` extension function to the built-in `String` class:

```kotlin
fun String.capitalizeFirstChar(): String {
    return this.lowercase().replaceFirstChar { it.uppercase() }
}
```

??? example "Try it out"
    ```kotlin
    println("hELLO wORLD".capitalizeFirstChar())
    ```
    **Expected Result**:
    ```
    Hello world
    ```

### Generic Extension Function
We can also add extension functions that work for broader types using generics with Type Bounds.
For example, we can create an extension function `newSum()` for lists that sums up numeric values and returns the result as a `Double`.

```kotlin
fun <T : Number> List<T>.newSum(): Double {
    var output = 0.0
    for (item in this) {
        output += when (item) {
            is Double -> item
            else -> item.toDouble()
        }
    }
    return output
}
```

The generic type is indicated by the angle brackets `<T : Number>`, which means that `T` can be any type that is a subtype of `Number`.
Hence, this extension function works for any list of numeric types, such as `Int`, `Float`, or `Double`, because of the type bound `T : Number`.

??? example "Try it out"
    ```kotlin
    println(listOf(1, 2, 3).newSum())
    println(listOf(1.5, 2.5, 3.0).newSum())
    ```
    **Expected Result**:
    ```
    6.0
    7.0
    ```

The type bounds ensure that only numeric types can be used with this function, preventing potential runtime errors.
However, we still don't know which specific numeric type `T` is at runtime, which is why we cast each item to `Double` using `toDouble()`.

Unfortunately, things get a bit more complicated if we want to check for specific types at runtime, because of Type Erasure.
Type Erasure means that generic type information is not available at runtime, so we cannot do something like this:
```kotlin
fun <T: Number> List<T>.newSum(): T {
    If (T::class == Double::class) {
        // Do something specific for Double
        return 0.0
    } else if (T::class == Float::class) {
        // Do something specific for Float
        return 0.0f
    } else if (T::class == Int::class) {
        // Do something specific for Int
        return 0
    }
    return 0 as T
}
```
The above code will not compile, because the type `T` is erased at runtime, and thus `T::class` is not accessible.
For such cases, we can use `reified` type parameters in combination with `inline` functions.

For example, let's say we want to add the `next()` function from above to the built-in `Enum` class, so that it works for any enum type.

```kotlin
inline fun <reified T: Enum<T>> T.next(): T {
    val values = enumValues<T>()
    return values[(ordinal + 1) % values.size]
}
```

This function now works for any enum type `T`, because we used `reified` to make the type information available at runtime.

??? example "Try it out"
    ```kotlin
    enum class CardinalDirections {
        NORTH, EAST, SOUTH, WEST
    }
    println(CardinalDirections.EAST.next())
    ```
    **Expected Result**:
    ```
    SOUTH
    ```

!!! tip "Reified Type Parameters"
    With `reified` type parameters, you can check the type at runtime using constructs like `is` or `::class`.

Note, however, that `reified` type parameters can only be used in `inline` functions.

### Inline Functions

These type of functions which are created using they keyword `inline` are not
referenced like normal functions, but their code is inlined at the call site during compilation, i.e. copied directly
into the place where they are called. This allows the compiler to retain the type information for `T` at runtime, or 
replace T with the actual type at the call site respectively.

In general, inline functions can improve performance by reducing the overhead of function calls, especially for small functions or higher-order functions that take lambdas as parameters. Note, however, that excessive inlining and inlining of complex functions lead to larger binary sizes.

## Summary

- **Free Functions**: Functions outside of classes
- **Higher-Order Functions**: Functions as parameters or return values
- **Lambdas**: Compact anonymous functions
- **Extension Functions**: Extend existing classes
- **Generic Functions**: With type parameters for flexibility
- **Reified Type Parameters**: Make generic types available at runtime
- **Inline Functions**: For performance optimization

## Best Practices

1. Use extension functions to make APIs more readable
2. Use lambdas for short, simple functions
3. Use `reified` only when really necessary (type information at runtime)
4. Type bounds help make generic functions safe

## Further Reading

- [Kotlin Documentation: Functions](https://kotlinlang.org/docs/functions.html)
- [Kotlin Documentation: Lambdas](https://kotlinlang.org/docs/lambdas.html)
- [Kotlin Documentation: Extensions](https://kotlinlang.org/docs/extensions.html)

[← Back to Chapter 2: Collections](02_collections.md) | [Continue to Chapter 4: Null-Safety and Scope Functions →](04_null_safety_and_scope_functions.md)
