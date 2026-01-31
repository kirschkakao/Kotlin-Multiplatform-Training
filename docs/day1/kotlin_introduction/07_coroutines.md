# Chapter 7: Coroutines

Coroutines are Kotlin's solution for asynchronous programming. They allow you to write asynchronous code that looks like synchronous code but is non-blocking.

## Why Coroutines?

Traditional threads are heavy and blocking. Coroutines are lightweight and can be paused and resumed without blocking the underlying thread.

### The Problem with Threads

In sequential programming, blocking operations (like network calls or delays) block the entire thread:

```kotlin
const val dFine = "d-fine"
const val slogan = "We define Consulting"

fun normalThread() {
    println(dFine)
    Thread.sleep(1000)  // Some blocking operation
    println(slogan)
}
```

Since the thread cannot do anything else while running the blocking operation (e.g. some database query etc.), the application becomes unresponsive.
This behaviour leads to freezing UIs and poor performance in server applications, i.e. an unsatisfying user experience.

To solve this problem, Kotlin provides coroutines.

## Basic Coroutines

To write a suspendable function, i.e. a function that can be paused and resumed without blocking the thread, simply use the `suspend` keyword.

```kotlin
suspend fun defineConsulting() {
    delay(1000)
    println(slogan)
}
```

!!! important "delay vs Thread.sleep"
- `delay()`: Pauses the coroutine, but doesn't block the thread
- `Thread.sleep()`: Blocks the entire thread

To incorporate a suspending function into your sequentially running code, you can use `runBlocking` which starts a coroutine and
blocks the main thread until completion:

```kotlin
fun coroutine() = runBlocking {
    defineConsulting()
    println(dFine)
}

fun main() {
    println("start")
    coroutine()
    println("end")
}
```

**Result**:
```
start
We define Consulting  (after 1 second)
d-fine
end
```

As you can see, using a suspend function alone does not make your code asynchronous. The Strings are printed sequentially, and every print after the delay waits for the delay to finish.
To actually run code asynchronously, you need to start your suspending functions in different coroutines using coroutine builders like `launch` or `async`.

### launch

This builder starts a new coroutine that runs concurrently with the rest of the code which does not return a result (fire-and-forget):

```kotlin
launch {
    delay(1000)
    println("Hallo")
}
```

For example, you can modify the previous example to run `defineConsulting()` in a separate coroutine:

```kotlin
fun coroutine2() = runBlocking {
    launch {
        defineConsulting()
    }
    println(dFine)
}

fun main() {
    println("start")
    coroutine2()
    println("end")
}
```
**Result**:
```
start
d-fine
We define Consulting  (after 1 second)
end
```

Now, `defineConsulting()` runs in parallel to the rest of the code of `coroutine2()`. The main coroutine continues to print `dFine` immediately, while the launched coroutine waits for 1 second before printing the slogan.

Since coroutines are very lightweight, you can start many of them without running into resource issues:

```kotlin
fun leightweightExample() = runBlocking {
    repeat(50000) {
        launch {
            delay(5000)
            print(".")
        }
    }
}
```

**Expected Result**:
```
..................................................  (50,000 dots after 5 seconds)
```

**Explanation**: 50,000 coroutines are started in parallel. All wait 5 seconds and then print a dot.

!!! tip "Performance"
    Threads are expensive (about 1 MB per thread). Coroutines are very lightweight (only a few bytes).

### async/await

The `async` builder is used to start a coroutine that computes a result. It returns a `Deferred<T>`, which is a non-blocking future that you can `await` to get the result.
Let's say we have two suspend functions that fetch a slogan and a name from a remote source (simulated with `delay`):

```kotlin
suspend fun getSlogan(): String {
    delay(1000)
    return slogan
}

suspend fun getName(): String {
    delay(1000)
    return dFine
}

fun notParallelExample() = runBlocking {
    println("${getName()}: ${getSlogan()}")
}
```

Since we do mot start any coroutine, the above code runs sequentially, taking about 2 seconds to complete (1 second for each function).
Instead, we can use `async` to run both operations in parallel:

```kotlin
fun awaitExample() = runBlocking {
    val nameDeferred = async {
        getName()
    }
    val sloganDeferred = async {
        getSlogan()
    }

    println("${nameDeferred.await()}: ${sloganDeferred.await()}")
}
```

This time, both `getName()` and `getSlogan()` run in parallel, and the total execution time is about 1 second.

### awaitAll

Kotlin provides a convenient function `awaitAll` to await multiple `Deferred` values at once:

```kotlin
fun awaitAllExample() = runBlocking {
    val (slogan, name) = awaitAll(
        async { getSlogan() },
        async { getName() }
    )
    println("$name: $slogan")
}
```

Again, both operations run in parallel, and the total execution time is about 1 second.

??? example "Try it out"
    ```kotlin
    val time = measureTimeMillis {
        notParallelExample()
    }
    println(time)
    
    val time2 = measureTimeMillis {
        awaitExample()
    }
    println(time2)
    
    val time3 = measureTimeMillis {
        awaitAllExample()
    }
    println(time3)
    ```
    
    **Expected Result**:
    ```
    d-fine: We define Consulting
    ~2000
    d-fine: We define Consulting
    ~1000
    d-fine: We define Consulting
    ~1000
    ```


### Comparison of launch and async

| Aspect           | launch | async |
|------------------|--------|-------|
| **Return value** | No (`Job`) | Yes (`Deferred<T>`) |
| **Usage**        | Fire-and-forget tasks | Computations with results |
| **Waiting**      | `job.join()` | `deferred.await()` |
| **Example**      | Logging, updates | API calls, calculations |


## Coroutine Contexts and Dispatchers

Dispatchers determine on which thread the coroutine runs:

```kotlin
launch(Dispatchers.IO) {
    // For I/O operations (File, Network)
}

launch(Dispatchers.Default) {
    // For CPU-intensive calculations
}

launch(Dispatchers.Main) {
    // For UI updates (Android)
}
```

Per default, coroutines inherit the context of their parent coroutine. You can check the current dispatcher with `coroutineContext`:

```kotlin
launch {
    println(coroutineContext)
}
```

## Summary

- **Coroutines**: Lightweight alternative to threads
- **suspend**: Functions that can be paused
- **launch**: Starts a "fire-and-forget" coroutine
- **async/await**: For parallel computations with results

!!! danger "Common Mistakes"
    1. Using `Thread.sleep()` in coroutines (blocks!)
    2. `async` without `await()` (no parallelism!)
    3. `runBlocking` in production code (only for tests/main!)

## Further Reading

- [Kotlin Documentation: Coroutines Basics](https://kotlinlang.org/docs/coroutines-basics.html)
- [Kotlin Documentation: Coroutines Guide](https://kotlinlang.org/docs/coroutines-guide.html)
- [kotlinx.coroutines GitHub](https://github.com/Kotlin/kotlinx.coroutines)

## Advanced Topics

For advanced applications, you should familiarize yourself with the following topics:

- **Flow**: Asynchronous data streams
- **Channels**: Communication between coroutines
- **SupervisorJob**: Error handling in coroutine hierarchies
- **CoroutineScope**: Custom scopes

[← Back to Chapter 6: Classes and Objects](06_classes.md) | [Back to the beginning](index.md)
