# Chapter 2: Collections

Collections are data structures that can contain multiple elements. Kotlin offers various collection types for different use cases.

## Pair and Triple

### Pair

A `Pair` contains exactly two values:

```kotlin
val coordinate: Pair<Int, Int> = Pair(0, 0)
val gps = 0.1 to -0.5  // Shorter syntax with 'to'
```

### Triple

A `Triple` contains exactly three values:

```kotlin
val andHeight: Triple<Int, Int, Int> = Triple(0, 0, 0)
```

Reasons to use Pair or Triple:
- Grouping two or three related values without creating a separate class
- Returning multiple values from a function
- Readable Code with named components (`first`, `second`, `third`)

You can destructure Pair and Triple into their components:
```kotlin
val (x, y) = coordinate
```

??? example "Try it out"
    ```kotlin
    val coordinate = Pair(1, 2)
    val (x, y) = coordinate
    println("x: $x, y: $y")
    ```
    
    **Expected Result**:
    ```
    x: 1, y: 2
    ```

## List

A `List` is an ordered collection of elements, that allows duplicates, maintains the order of insertion, and allows access by index.
Lists can be initialized using the free function `listOf()`:

```kotlin
val germanStates: List<String> = listOf("Hessen", "Bayern", "Sachsen", "Lalaland")
```

Like with the primitive types, Kotlin infers the type if not explicitly specified:

```kotlin
val germanStates = listOf("Hessen", "Bayern", "Sachsen", "Lalaland") //same type as above
```

Note, that Collections types are generic types. You can create Lists of any type, e.g. `List<Int>`, `List<Double>`, or even custom types like `List<GermanState>`. The type that a list contains is specified within the angle brackets `<...>`.

Other than in languages like Python or JavaScript, Lists in Kotlin are immutable by default. You cannot add or remove elements from them. Its size is fixed after creation.
Also, elements can't be changed:

```kotlin
// germanStates.add("Niedersachen") // This will cause a compilation error
// germanStates[0] = "Hamburg" // This will cause a compilation error
```

You can still create a new List by adding elements to an existing one:

```kotlin
val newGermanStates = germanStates + "Niedersachen"
```

??? example "Try it out"
    ```kotlin
    val germanStates = listOf("Hessen", "Bayern", "Sachsen", "Lalaland")
    val newGermanStates = germanStates + "Niedersachen"
    println(germanStates)
    println(newGermanStates)
    ```
    
    **Expected Result**:
    ```
    [Hessen, Bayern, Sachsen, Lalaland]
    [Hessen, Bayern, Sachsen, Lalaland, Niedersachen]
    ```

If you want a List that can be modified, you can use a `MutableList`.

### Mutable List
A `MutableList` allows adding, removing, and updating elements:

```kotlin
val mutableGermanStates: MutableList<String> = mutableListOf()
mutableGermanStates.add("Hessen") // Add at the end
mutableGermanStates.add(0, "Bayern") // Add at index 0
mutableGermanStates[1] = "Sachsen" // Update element at index 1
```

??? example "Try it out"
    ```kotlin
    val mutableGermanStates = mutableListOf()
    mutableGermanStates.add("Hessen")
    mutableGermanStates.add(0, "Bayern")
    mutableGermanStates[1] = "Sachsen"
    println(mutableGermanStates)
    ```
    
    **Expected Result**:
    ```
    [Bayern, Sachsen]
    ```

### Generic Types

To combine elements of different type in one List, one can use the type `Any` which is the supertype of all types in Kotlin:

```kotlin
val anyList: MutableList<Any> = mutableListOf(1, "")
```

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add element at end/start | O(1) (amortized) |
| Add element at index | O(N) |
| Check element existence | O(N) |
| Add two lists | O(N) |

Lists are ideal when you need to maintain the order of elements and allow duplicates.

## Set

A `Set` is an unordered collection of **unique** elements. Duplicates are automatically removed. As with Lists, Sets are immutable by default.

### Immutable Set

```kotlin
var germanStateSet: Set<String> = setOf("Hessen", "Bayern", "Sachsen", "Hessen")
// Result: {"Hessen", "Bayern", "Sachsen"} - "Hessen" appears only once
```

??? example "Try it out"
    ```kotlin
    val germanStateSet = setOf("Hessen", "Bayern", "Sachsen", "Hessen")
    println(germanStateSet)
    ```
    
    **Expected Result**:
    ```
    [Hessen, Bayern, Sachsen]
    ```

### Mutable Set

```kotlin
var mutableGermanStateSet: MutableSet<String> = mutableSetOf("Hessen", "Bayern", "Sachsen", "Hessen")
```

??? example "Try it out"
    ```kotlin
    val mutableGermanStateSet = mutableSetOf("Hessen", "Bayern", "Sachsen", "Hessen")
    mutableGermanStateSet.add("Bayern")
    println(mutableGermanStateSet)
    ```

    **Expected Result**:
    ```
    [Hessen, Bayern, Sachsen]
    ```

Since Sets do not maintain order, you cannot add elements at a specific index.

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add element | O(1) |
| Add element at index | not possible |
| Check element existence | O(1) |
| Add two sets | O(N) |

Sets are ideal when you frequently need to check if an element exists (O(1) instead of O(N) with Lists).

## SortedSet

A `SortedSet` is a set whose elements are automatically sorted:

```kotlin
val sortedStateSet: SortedSet<String> = sortedSetOf("Hessen", "Bayern", "Sachsen", "Hessen")
// Result: {"Bayern", "Hessen", "Sachsen"} - alphabetically sorted
```

??? example "Try it out"
    ```kotlin
    val sortedStateSet = sortedSetOf("Hessen", "Bayern", "Sachsen", "Hessen")
    println(sortedStateSet)
    ```
    
    **Expected Result**:
    ```
    [Bayern, Hessen, Sachsen]
    ```

Internally, a `SortedSet` is often implemented as a balanced binary search tree. Hence, operations like adding elements or checking for existence have logarithmic time complexity.

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add element | O(ln(N)) |
| Add element at index | not possible |
| Check element existence | O(ln(N)) |
| Add two sets | O(N) |

While `SortedSet` has slower operations compared to `Set`, it is useful when you need to maintain a sorted order of elements, while still have more efficient existence checks than a List.
Note, that a time complexity of O(ln(N)) is still significantly faster than O(N) for large N. For example, for N = 1,000,000, ln(N) is approximately 14, while N is 1,000,000.

## LinkedHashSet

A `LinkedHashSet` is a set that maintains the **insertion order** of elements while still ensuring uniqueness and providing fast lookups. Unlike a regular `Set`, which has no guaranteed order, a `LinkedHashSet` remembers the order in which elements were added.

### Immutable LinkedHashSet

```kotlin
val linkedStateSet: LinkedHashSet<String> = linkedSetOf("Hessen", "Bayern", "Sachsen", "Hessen")
// Result: {"Hessen", "Bayern", "Sachsen"} - in insertion order
```

### Mutable LinkedHashSet

```kotlin
val mutableLinkedStateSet: LinkedHashSet<String> = linkedSetOf("Hessen", "Bayern").toMutableSet() as LinkedHashSet
```

### Time Complexity

| Operation | Complexity |
|-----------|------------|
| Add element | O(1) |
| Add element at index | not possible |
| Check element existence | O(1) |
| Add two sets | O(N) |

`LinkedHashSet` is useful when you need fast existence checks (O(1)) like a regular `Set`, but also want to preserve the order in which elements were added. This is particularly helpful when iterating over the set and the order matters.

!!! info "Trade-offs of LinkedHashSet"
    While `LinkedHashSet` maintains insertion order, it comes with trade-offs compared to a regular `Set`:
    
    - **Memory overhead**: Requires approximately **twice as much memory** as a regular `HashSet` because it maintains both a hash table and a doubly-linked list
    - **Slightly slower operations**: Although still O(1), operations like add/remove have slightly higher constant factors due to pointer management
    - **Cache efficiency**: Poorer cache performance due to additional pointer structures
    
    Use `LinkedHashSet` only when you actually need the insertion order preserved. If order doesn't matter, prefer a regular `Set` for better memory efficiency.

## Map (Key-Value Pairs)

A `Map` stores key-value pairs. As with Sets, keys are unique and existence checks are fast (O(1) on average).

### Immutable Map

```kotlin
val authorMap: Map<String, List<String>> = mapOf(
    "J.R.R. Tolkien" to listOf("Herr der Ringe - Die Gefaehrten", "Der Hobbit")
)
```

Note the slightly more complex type definition here: The keys are of type `String` and the values are of type `List<String>`.

??? example "Try it out"
    ```kotlin
    val authorMap = mapOf(
        "J.R.R. Tolkien" to listOf("Herr der Ringe - Die Gefaehrten", "Der Hobbit")
    )
    println(authorMap["J.R.R. Tolkien"])
    ```
    
    **Expected Result**:
    ```
    [Herr der Ringe - Die Gefaehrten, Der Hobbit]
    ```

### Mutable Map

Again, if you want to modify the Map, you can use a `MutableMap`:

```kotlin
val mutableAuthorMap: MutableMap<String, List<String>> = mutableMapOf()
```

??? example "Try it out"
    ```kotlin
    val mutableAuthorMap: MutableMap<String, List<String>> = mutableMapOf()
    mutableAuthorMap["J.K. Rowling"] = listOf("Harry Potter")
    println(mutableAuthorMap)
    ```
    
    **Expected Result**:
    ```
    {J.K. Rowling=[Harry Potter]}
    ```

### Time Complexity
| Operation | Complexity |
|-----------|------------|
| Add key-value pair | O(1) |
| Check key existence | O(1) |
| Retrieve value by key | O(1) |
| Add element at index | not possible |
| Add two maps | O(N) |

Maps are ideal when you need to associate values with unique keys and require fast lookups.

## LinkedHashMap

A `LinkedHashMap` is a map that maintains the **insertion order** of key-value pairs while still providing fast lookups. Unlike a regular `Map`, which has no guaranteed order, a `LinkedHashMap` remembers the order in which entries were added.

### Immutable LinkedHashMap

```kotlin
val linkedAuthorMap: LinkedHashMap<String, List<String>> = linkedMapOf(
    "J.R.R. Tolkien" to listOf("Herr der Ringe - Die Gefaehrten", "Der Hobbit"),
    "J.K. Rowling" to listOf("Harry Potter")
)
```

### Mutable LinkedHashMap

```kotlin
val mutableLinkedAuthorMap: MutableMap<String, List<String>> = linkedMapOf<String, List<String>>().toMutableMap()
```

### Time Complexity
| Operation | Complexity |
|-----------|------------|
| Add key-value pair | O(1) |
| Check key existence | O(1) |
| Retrieve value by key | O(1) |
| Add element at index | not possible |
| Add two maps | O(N) |

`LinkedHashMap` is useful when you need fast key-value lookups (O(1)) like a regular `Map`, but also want to preserve the order in which entries were added. This is particularly helpful when iterating over the map and the order of entries matters.

!!! info "Trade-offs of LinkedHashMap"
    While `LinkedHashMap` maintains insertion order, it comes with the same trade-offs as `LinkedHashSet`:
    
    - **Memory overhead**: Requires approximately **twice as much memory** as a regular `HashMap`
    - **Slightly slower operations**: Operations have slightly higher constant factors due to maintaining the linked list
    - **Cache efficiency**: Poorer cache performance due to additional pointer structures
    
    Use `LinkedHashMap` only when you actually need the insertion order preserved. If order doesn't matter, prefer a regular `Map` for better memory efficiency.


## Enum

Enums define a fixed set of constants:

```kotlin
enum class GermanState {
    BADEN_WUERTTEMBERG,
    BAYERN,
    BERLIN,
    BRANDENBURG,
    BREMEN,
    HAMBURG,
    HESSEN,
    MECKLENBURG_VORPOMMERN,
    NIEDERSACHSEN,
    NORDRHEIN_WESTFALEN,
    RHEINLAND_PFALZ,
    SAARLAND,
    SACHSEN,
    SACHSEN_ANHALT,
    SCHLESWIG_HOLSTEIN,
    THUERINGEN
}
```

Enums are useful when you have a predefined set of values that a variable can take, like with the german states. You can create Lists or other collections of Enums as well:

```kotlin
val enumStates: List<GermanState> = listOf(GermanState.BAYERN)
```

## When to Use Which Collection?

| Collection        | Use when...                                                                         |
|-------------------|-------------------------------------------------------------------------------------|
| **List**          | Order matters, duplicates are allowed                                               |
| **Set**           | Uniqueness matters, fast existence checks needed, order doesn't matter              |
| **SortedSet**     | Fast existence checks needed, elements must be sorted, uniqueness matters           |
| **LinkedHashSet** | Uniqueness matters, fast existence checks needed, insertion order must be preserved |
| **Map**           | Key-value mappings are needed, order doesn't matter                                 |
| **LinkedHashMap** | Key-value mappings are needed, insertion order must be preserved                    |
| **Enum**          | You have a fixed set of predefined constants with type safety                       |

## Summary

- **List**: Ordered collection with duplicates
- **Set**: Unordered collection without duplicates, fast search
- **SortedSet**: Automatically sorted collection without duplicates, fast search
- **LinkedHashSet**: Collection without duplicates with insertion order preserved, fast search
- **Map**: Key-value pairs
- **LinkedHashMap**: Key-value pairs with insertion order preserved
- **Enum**: Fixed set of predefined constants with type safety
- Choose the right collection based on your requirements for order, uniqueness, and performance

## Further Reading

- [Kotlin Documentation: Collections Overview](https://kotlinlang.org/docs/collections-overview.html)

[← Back to Chapter 1: Primitives](01_primitives.md) | [Continue to Chapter 3: Functions →](03_functions.md)
