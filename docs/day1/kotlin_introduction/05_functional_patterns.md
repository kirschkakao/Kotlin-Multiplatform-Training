# Chapter 5: Functional Patterns

Kotlin provides powerful functional programming capabilities for working with collections. These functional patterns allow you to write expressive, readable, and concise code when transforming and processing data.

## Functional Collection Operations

### map

The `map` function transforms each element of a collection by applying a given function:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val doubled = numbers.map { it * 2 }
// Result: [2, 4, 6, 8, 10]
```

??? example "Try it out"
    ```kotlin
    val nameList: List<String?> = listOf("dominik", "SaScha", "kai", "larA", null)
    val sanitizedList = nameList.map { name ->
        name?.let {
            capitalizeFirstChar(name)
        }
    }
    println(sanitizedList)
    ```
    
    **Expected Result**:
    ```
    [Dominik, Sascha, Kai, Lara, null]
    ```

### filter

The `filter` function selects elements that match a given predicate:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val evenNumbers = numbers.filter { it % 2 == 0 }
// Result: [2, 4]
```

### filterNotNull

Removes all `null` values from a collection:

```kotlin
val crazyNameList: List<String?> = listOf(
    "dominik", "SascHa", "kai", "laurA", null, 
    "doMinik", "saschA", "saSCHA", null, "Laura", "Dominik", "kai", "Sascha"
)

val cleanList = crazyNameList.filterNotNull()
// Result: List without null values
```

??? example "Try it out"
    ```kotlin
    val crazyNameList: List<String?> = listOf(
        "dominik", "SascHa", "kai", "laurA", null, 
        "doMinik", "saschA", "saSCHA", null, "Laura", "Dominik", "kai", "Sascha"
    )
    
    val cleanList = crazyNameList.filterNotNull()
    println(cleanList)
    ```
    
    **Expected Result**:
    ```
    [dominik, SascHa, kai, laurA, doMinik, saschA, saSCHA, Laura, Dominik, kai, Sascha]
    ```

### distinct

Removes duplicate elements from a collection:

```kotlin
val numbers = listOf(1, 2, 2, 3, 3, 3, 4)
val uniqueNumbers = numbers.distinct()
// Result: [1, 2, 3, 4]
```

### sorted

Sorts elements in natural order:

```kotlin
val numbers = listOf(3, 1, 4, 1, 5, 9, 2, 6)
val sortedNumbers = numbers.sorted()
// Result: [1, 1, 2, 3, 4, 5, 6, 9]
```

## Chaining Operations

One of the most powerful features of functional programming is the ability to chain multiple operations together:

```kotlin
val sortedDistinctNameList: List<String> = crazyNameList
    .filterNotNull()               // Removes nulls
    .map(::capitalizeFirstChar)    // Normalizes names
    .distinct()                     // Removes duplicates
    .sorted()                       // Sorts alphabetically
```

??? example "Try it out"
    ```kotlin
    val crazyNameList: List<String?> = listOf(
        "dominik", "SascHa", "kai", "laurA", null, 
        "doMinik", "saschA", "saSCHA", null, "Laura", "Dominik", "kai", "Sascha"
    )
    
    fun capitalizeFirstChar(name: String): String {
        return name.lowercase().replaceFirstChar { it.uppercase() }
    }
    
    val sortedDistinctNameList = crazyNameList
        .filterNotNull()
        .map(::capitalizeFirstChar)
        .distinct()
        .sorted()
    
    println(sortedDistinctNameList)
    ```
    
    **Expected Result**:
    ```
    [Dominik, Kai, Laura, Sascha]
    ```

!!! info "Sequences for Large Collections"
    When working with large collections and multiple chained operations, consider using `asSequence()`:
    
    ```kotlin
    val result = largeList
        .asSequence()
        .filter { /* ... */ }
        .map { /* ... */ }
        .toList()
    ```
    
    Sequences evaluate lazily, which can significantly improve performance by avoiding intermediate collections.
    However, every operation that produces a new collection (like `toList()`, `groupBy()` or `sorted()`) will force evaluation, so use them judiciously.

## Advanced Operations

### groupBy

Groups elements by a criterion:

```kotlin
val words = listOf("apple", "apricot", "banana", "blueberry", "cherry")
val groupedByFirstLetter = words.groupBy { it.first() }
// Result: {a=[apple, apricot], b=[banana, blueberry], c=[cherry]}
```

??? example "Try it out"
    ```kotlin
    val crazyNameList: List<String?> = listOf(
        "dominik", "SascHa", "kai", "laurA", null, 
        "doMinik", "saschA", "saSCHA", null, "Laura", "Dominik", "kai", "Sascha"
    )
    
    val frequencyNameList: List<Pair<String, Int>> = crazyNameList
        .filterNotNull()
        .map(::capitalizeFirstChar)
        .groupBy { it }                           // Groups by name
        .map { Pair(it.key, it.value.size) }     // Counts occurrences
        .sortedByDescending { (_, size) -> size } // Sorts by frequency
        .toList()
    
    println(frequencyNameList)
    ```
    
    **Expected Result**:
    ```
    [(Dominik, 3), (Sascha, 3), (Kai, 2), (Laura, 2)]
    ```

### fold

Reduces a collection to a single value by applying an operation. Unlike `map` or `filter`, `fold` doesn't return a collection by default but can return any type as single accumulated result.

**How `fold` works:**

1. You provide an **initial value** (the starting accumulator)
2. For each element in the collection, `fold` calls your lambda with two parameters:
    - `acc` - the current accumulator (starts as the initial value)
    - The current element
3. Your lambda **returns the new accumulator value**
4. After all elements are processed, `fold` returns the final accumulator

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val sum = numbers.fold(0) { acc, number -> 
    acc + number  // The lambda returns the new acc
}
// Step by step: 0 → 1 → 3 → 6 → 10 → 15
```

**Important: The result type can be different from the collection type!** This makes `fold` incredibly flexible:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val concatenated = numbers.fold("") { acc, num ->
    acc + num.toString()  // Returns String, not Int
}
// Result: "12345"

val doubled = numbers.fold(emptyList<Int>()) { acc, num ->
    acc + (num * 2)  // Returns List, not Int
}
// Result: [2, 4, 6, 8, 10]
```

**Without an initial value:**
If you don't provide an initial value, `fold` without a parameter is not available. Instead, use `reduce()`, which uses the first element as the initial value:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val sum = numbers.reduce { acc, number -> acc + number }
// Uses 1 as initial acc, then: 1 → 3 → 6 → 10 → 15
```

??? example "Try it out"
    ```kotlin
    val integerList: List<Int?> = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, null)
    
    // fold with initial value 0, returns Int
    val summedList = integerList.fold(0) { acc, i -> 
        acc + (i ?: 0) * 2 
    }
    println("fold result: $summedList")
    
    // fold to build a String
    val stringResult = integerList.fold(StringBuilder("Numbers: ")) { acc, i ->
        if (i != null) acc.append("$i ") else acc.append("null ")
        acc
    }.toString()
    println("String result: $stringResult")
    ```
    
    **Expected Result**:
    ```
    fold result: 90
    String result: Numbers: 1 2 3 4 5 6 7 8 9 null 
    ```
    
    **Explanation**: 
    - First fold: Initial acc = 0, then: 0 + 1×2 + 2×2 + ... + 9×2 = 90
    - Second fold: Accumulates into a StringBuilder (different type!)

### sumOf

A more concise alternative for summing transformed values:

```kotlin
val numbers = listOf(1, 2, 3, 4, 5)
val sumOfSquares = numbers.sumOf { it * it }
// Result: 55 (1 + 4 + 9 + 16 + 25)
```

??? example "Try it out"
    ```kotlin
    val integerList: List<Int?> = listOf(1, 2, 3, 4, 5, 6, 7, 8, 9, null)
    val otherSum = integerList.sumOf { (it ?: 0) } * 2
    
    println(otherSum)
    ```
    
    **Expected Result**:
    ```
    90
    ```
    
    **Explanation**: (1+2+3+4+5+6+7+8+9)×2 = 45×2 = 90

## Practical Applications

### Fibonacci with fold

A creative use of `fold` to generate Fibonacci numbers. This example demonstrates that `fold` can return a completely different type than the collection it operates on (here: `List<Int>` instead of `Range<Int>`):

```kotlin
fun fibonacci(n: Int): List<Int> {
    return (0 until n).fold(listOf(0, 1)) { acc, e ->
        acc + acc.takeLast(2).sum()  // acc is a List, returns a List
    }
}
```

Here, `fold` starts with an initial value of `listOf(0, 1)` and in each iteration:
1. Takes the last two elements from the accumulator list
2. Sums them
3. Appends the result to the accumulator
4. Returns the updated list as the new accumulator

This shows the power of `fold` - the result type (`List<Int>`) is completely independent of the collection type (`IntRange`).

??? example "Try it out"
    ```kotlin
    fun fibonacci(n: Int): List<Int> {
        return (0 until n).fold(listOf(0, 1)) { acc, e ->
            acc + acc.takeLast(2).sum()
        }
    }
    
    println(fibonacci(8))
    ```
    
    **Expected Result**:
    ```
    [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
    ```

## Functional Collection Operations Overview

| Operation | Description | Return |
|-----------|-------------|--------|
| `map` | Transforms elements | `List<R>` |
| `filter` | Filters elements | `List<T>` |
| `filterNotNull` | Removes null values | `List<T>` |
| `distinct` | Removes duplicates | `List<T>` |
| `sorted` | Sorts elements | `List<T>` |
| `groupBy` | Groups by criterion | `Map<K, List<T>>` |
| `fold` | Reduces to a single value | `R` |
| `sumOf` | Sums transformed values | `Int/Double` |

## Summary

- **Functional Operations**: Transform and process collections declaratively
- **`map`**: Transforms each element using a function
- **`filter`**: Selects elements matching a predicate
- **`filterNotNull`**: Removes null values from collections
- **Chaining**: Combine multiple operations for readable data pipelines
- **`groupBy`**: Groups elements by a key function
- **`fold`**: Reduces collections to single values
- **`sumOf`**: Convenient function for summing transformed values

## Best Practices

1. **Chain operations** for better readability instead of using intermediate variables
2. **Use `filterNotNull()`** instead of manual null checks in map operations
3. **Prefer `sumOf`** over `fold` when simply summing values
4. **Use method references** (`::function`) when applicable for cleaner code
5. **Keep lambda expressions simple** - extract complex logic into separate functions
6. **Consider performance** - chaining creates intermediate collections (use `asSequence()` for large collections)

## Further Reading

- [Kotlin Documentation: Collection Operations](https://kotlinlang.org/docs/collection-operations.html)
- [Kotlin Documentation: Sequences](https://kotlinlang.org/docs/sequences.html)

[← Back to Chapter 4: Null-Safety and Scope Functions](04_null_safety_and_scope_functions.md) | [Continue to Chapter 6: Classes and Objects →](06_classes.md)
