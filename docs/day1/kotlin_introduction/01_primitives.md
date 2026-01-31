# Chapter 1: Primitive Data Types

In this chapter, you will learn about the basic data types in Kotlin and how values are declared and assigned. 

## Typing
Kotlin is a statically typed language, which means that the type of a variable is known at compile time. You can explicitly specify the type or let the compiler infer it.

```kotlin
val explicit: String = "I am a String"  // Explicit type declaration
val implicit = 42                        // Type inferred as Int
```

!!! tip "Type checking"
    You can check the type of a variable in IntelliJ IDEA by hovering over it or using `Ctrl + Shift + P` (Windows/Linux) or `Cmd + Shift + P` (Mac).

## Mutability: val vs var
In Kotlin, you can declare variables as either mutable or immutable. A mutable variable can be changed after its initial assignment, while an immutable variable cannot.

### val (immutable)

Variables declared with `val` cannot be reassigned:

```kotlin
val unchangable = "I stay the same"
// unchangable = "New"  // Compilation error!
```

### var (mutable)

Variables declared with `var` can be reassigned:

```kotlin
var mutableTrue: Boolean = true
mutableTrue = false  // OK
```

!!! tip "Best Practice"
    Prefer `val` over `var` whenever possible. This makes the code safer and more understandable.

## Boolean (Truth Values)

Booleans represent truth values and can be either `true` or `false`.

```kotlin
val thisIsTrue: Boolean = true
val thisIsFalse = false
```

### Logical Operators

```kotlin
val und: Boolean = true && false  // false
val oder = true || false          // true
```

## Strings and Chars

### Strings

Strings are character sequences and can be combined with string interpolation:

```kotlin
val name: String = "Bruce"
val surname = "Wayne"
val fullName = "$name $surname"  // "Bruce Wayne"
```

!!! tip "String Interpolation"
    With `$` you can insert variables directly into strings. For more complex expressions, use `${expression}`.

### Chars

A single character is defined with single quotes:

```kotlin
val letter: Char = 'D'
```

Internally, Chars are represented as numbers (Unicode code points):

```kotlin
val letterCode: Int = letter.code  // 68
```

Therefore, Chars can be manipulated like numbers, for example by adding an integer value:

```kotlin
val firstLetterChar: Char = 'D' + 1  // 'E'
```

To use a Char in a String, you can use string interpolation:

```kotlin
val charInString = "First letter after D is ${firstLetterChar}"  // "First letter after D is E"
```

## Numbers

### Integers

Kotlin offers various integer types with different sizes:

```kotlin
val age: Short = 36                    // 16 bit
val salary: Int = 2147483647           // 32 bit
val salaryAfterPromotion: Long = salary + 1L  // 64 bit
```

In case of overflow (i.e. you try to save a larger number than whatr can be represented by 32 bits), Kotlin does not
throw an error by default. Instead, it wraps around:

```kotlin
val newSalary = salary + 1  // -2147483648 (overflow!)
```

### Unisgned Integers
Kotlin also supports unsigned integers (only positive values):

```kotlin
val positiveAge: UByte = 36u          // 8 bit
val positiveYear: UShort = 30000u   // 16 bit
val positiveSalary: UInt = 3000000000u // 32 bit
val positiveBigSalary: ULong = 5000000000uL // 64 bit
```

Since these numbers don't support negative values and hence need no sign bit, their maximum value is roughly double that of the signed variants.

### Very Large Numbers

For numbers outside the Long range, there is `BigInteger`:

```kotlin
val veryLargeNumber = BigInteger("123456789123456789123456789123456789123456789")
```
Internally, BigInteger uses an array of smaller integers to represent very large values.

BigInteger supports arithmetic operations as well:

```kotlin
val anotherLargeNumber = BigInteger("987654321987654321987654321987654321987654321")
val sum = veryLargeNumber + anotherLargeNumber
```

### Floating-Point Numbers

For decimal numbers, there are `Float` (32 bit) and `Double` (64 bit):

```kotlin
val hydrogenIonizationEnergy: Float = 13.36F
val precisionOfFloat = 12345612.0F
val debtOfGermanState: Double = -2.7E32  // Scientific notation
```

Using the scientific notation `E` allows you to represent very large or very small numbers easily.

If you do not specify the type, Kotlin uses `Double` by default for decimal numbers:

```kotlin
val pi = 3.141592653589793  // Double by default
``` 

### Power Calculation

There are various ways to calculate powers:

```kotlin
val power1 = 2.0.pow(3)           // 8.0
val power2 = Math.pow(2.0, 3.0)   // 8.0
val power3 = 1 shl 3              // 8 (Bitshift: 0001 -> 1000)
```

## Compile-Time Constants

With `const val` you define constants that are known at compile time:

```kotlin
const val justWahr = true  // Compile-time constant
```

These can only be of primitive types or Strings and must be initialized with a value known at compile time, i.e. cannot be assigned
from a function call or other runtime values.

## Null

The value `null` represents the absence of a value:

```kotlin
val nothing = null
```

!!! note "Null-Safety"
    Learn more about Kotlin's Null-Safety features in [Chapter 4](04_null_safety_and_scope_functions.md).

## Unit

`Unit` is comparable to `void` in Java and represents "no meaningful return value":

```kotlin
val noReturn = Unit
```

!!! note "Unit Type"
    Functions that do not return a value have the return type `Unit`, which can be omitted. Learn more in [Chapter 3](03_functions.md).

## Summary

- Kotlin offers various primitive data types for different use cases
- `val` for immutable, `var` for mutable variables
- String templates with `$` enable easy string concatenation
- Different number types for different size ranges

## Further Reading

- [Kotlin Documentation: Basic Types](https://kotlinlang.org/docs/basic-types.html)

[← Back to the beginning](index.md) | [Continue to Chapter 2: Collections →](02_collections.md)
