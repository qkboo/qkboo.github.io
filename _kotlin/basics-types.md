---
title: "Basics / 기본 형식"
permalink: /kotlin/basics-basic_types/
last_modified_at: 2018-03-20T15:58:49-04:00
toc: true
---

kotlinlang.org의 [Basics/Basic Types](https://kotlinlang.org/docs/reference/basic-types.html) 를 정리했다.

- 필요에 따라 내용에서 **[링크:..]** 는 외부 링크이다.

## Basic Types


코틀린에서 우리가 일반적으로 변수에서 멤버 함수와 속성을 호출하는 것을 객체로 인식합니다. 모든 데이터 형은 내장되어서 구현되었다고 고려되며... 이 장에서 이런 형식들: 숫자, 문자, 불리언과 배열을 설명합니다.

> In Kotlin, everything is an object in the sense that we can call member functions and properties on any variable. Some types are built-in, because their implementation is optimized, but to the user they look like ordinary classes. In this section we describe most of these types: numbers, characters, booleans and arrays.

## Numbers

코틀린은 숫자를 자바에 가까운 방법으로 다루고 있지만 아주 같지는 않습니다. 예를 들어 숫자에 대해 암시적 확대 변환이 없고 리터럴은 몇몇 사례에서 다릅니다.

> Kotlin handles numbers in a way close to Java, but not exactly the same. For example, there are no implicit widening conversions for numbers, and literals are slightly different in some cases.

코틀린은 아래 같은 내장한 숫자를 다루는 형식을 제공합니다 ( 이것은 자바에 가깝다)

> Kotlin provides the following built-in types representing numbers (this is close to Java):

| Type   | Bit width|
|--------|----------|
| Double | 64       |
| Float  | 32       |
| Long   | 64       |
| Int  | 32       |
| Short  | 16       |
| Byte   | 8        |

> NOTE: 이제 더이상 ASCII 는 숫자가 아니다
Note that characters are not numbers in Kotlin.

### 리터럴 상수 Literal Constants

정수 값에 대해 리터럴 상수의 종류는 다음과 같습니다:

* 10진수 Decimals: `123`
  * Longs 은 대문자 `L`를 붙인다: `123L`
* 16진수 Hexadecimals: `0x0F`
* 2진수 Binaries: `0b00001011`

> NOTE: 8진수 리터럴은 지원 않는다.

코틀린은 부동소수 숫자를 위해 일반적인 표기법을 지원합니다.
> Kotlin also supports a conventional notation for floating-point numbers:
 
* 기본으로 Doubles: `123.5`, `123.5e10`
* Floats은 `f` 또는 `F` 를 붙인다: `123.5f` 

### 표시 Representation

자바 플랫폼에서 숫자는 JVM에 기본형식으로 물리적으로 저장되됩니다. 이것은 널 숫자 (예로 'Int?') 참조일 경우에도, 또한 제너릭으로 엉켜도 그렇게 된다. 
후자의 경우 숫자는 박스화 된다.

> On the Java platform, numbers are physically stored as JVM primitive types, unless we need a nullable number reference (e.g. `Int?`) or generics are involved. 
In the latter cases numbers are boxed.

> NOTE: 숫자가 박싱하면 유일성을 보존하지 못한다. that boxing of numbers does not preserve identity:

아래에서 박싱한 boxedA와 anotherBoxedA는 서로에 다른 참조를 갖게 된다.

``` kotlin
val a: Int = 10000
print(a === a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA === anotherBoxedA) // !!!Prints 'false'!!!
```

다른 한편 동등성은 보장된다: On the other hand, it preserves equality:

``` kotlin
val a: Int = 10000
print(a == a) // Prints 'true'
val boxedA: Int? = a
val anotherBoxedA: Int? = a
print(boxedA == anotherBoxedA) // Prints 'true'
```

### 명시적 변환 Explicit Conversions

다른 표현에 따라, 작은 형식은 큰 형식의 서브타입이 아니다.

> Due to different representations, smaller types are not subtypes of bigger ones.
If they were, we would have troubles of the following sort:

``` kotlin
// Hypothetical code, does not actually compile:
val a: Int? = 1 // A boxed Int (java.lang.Integer)
val b: Long? = a // implicit conversion yields a boxed Long (java.lang.Long)
print(a == b) // Surprise! This prints "false" as Long's equals() check for other part to be Long as well
```

So not only identity, but even equality would have been lost silently all over the place.

As a consequence, smaller types are NOT implicitly converted to bigger types.
This means that we cannot assign a value of type `Byte` to an `Int` variable without an explicit conversion

``` kotlin
val b: Byte = 1 // OK, literals are checked statically
val i: Int = b // ERROR
```

We can use explicit conversions to widen numbers

``` kotlin
val i: Int = b.toInt() // OK: explicitly widened
```

모든 숫자 형식은 아래 변환 함수를 지원한다: Every number type supports the following conversions:

* `toByte(): Byte`
* `toShort(): Short`
* `toInt(): Int`
* `toLong(): Long`
* `toFloat(): Float`
* `toDouble(): Double`
* `toChar(): Char`


Absence of implicit conversions is rarely noticeable because the type is inferred from the context, and arithmetical operations are overloaded for appropriate conversions, for example

``` kotlin
val l = 1L + 3 // Long + Int => Long
```

### 연산 Operations
숫자를 사용한 표준 연산을 지원합니다. 
Kotlin supports the standard set of arithmetical operations over numbers, which are declared as members of appropriate classes (but the compiler optimizes the calls down to the corresponding instructions).
See [Operator overloading](operator-overloading.html).


As of bitwise operations, there're no special characters for them, but just named functions that can be called in infix form, for example:

``` kotlin
val x = (1 shl 2) and 0x000FF000
```

아래는 비트 연산자 목록이다 ('Int', 'Long' 단독)
> Here is the complete list of bitwise operations (available for `Int` and `Long` only):

* `shl(bits)` – signed shift left (Java's `<<`)
* `shr(bits)` – signed shift right (Java's `>>`)
* `ushr(bits)` – unsigned shift right (Java's `>>>`)
* `and(bits)` – bitwise and
* `or(bits)` – bitwise or
* `xor(bits)` – bitwise xor
* `inv()` – bitwise inversion


## 문자 Characters

문자는 `Char` 형식으로 표시된다. 이것은 숫자로 직접 다뤄지지 않는다.
> Characters are represented by the type `Char`. They can not be treated directly as numbers

``` kotlin
fun check(c: Char) {
  if (c == 1) { // ERROR: incompatible types
    // ...
  }
}
```

문자 리터럴은 단일 따옴표로 사용합니다:`'1'`
> Character literals go in single quotes: `'1'`.

Special characters can be escaped using a backslash.
다음 회피문자(escape sequence)를 지원한다: `\t`, `\b`, `\n`, `\r`, `\'`, `\"`, `\\` and `\$`.
다른 문자를 인코드하려면 유니코드 시퀀스를 사용합니다: `'\uFF00'`.

문자를 명시적으로 `Int` 를 사용해서 변환할 수 있다:
> We can explicitly convert a character to an `Int` number:

``` kotlin
fun decimalDigitValue(c: Char): Int {
  if (c !in '0'..'9')
    throw IllegalArgumentException("Out of range")
  return c.toInt() - '0'.toInt() // Explicit conversions to numbers
}
```

숫자와 같이, 문자는 널 참조가 필요할 때 박스된다.
Like numbers, characters are boxed when a nullable reference is needed. Identity is not preserved by the boxing operation.

## Booleans

The type `Boolean` represents booleans, and has two values: *true*{: .keyword } and *false*{: .keyword }.

Booleans are boxed if a nullable reference is needed.

Built-in operations on booleans include

* `||` – lazy disjunction
* `&&` – lazy conjunction
* `!` - negation


## Arrays

코틀린에서 배열은 `Array` 클래스로 표현되고, 이것은 `get`과 `set` 함수 가집니다 (이것은 연산자 재정의에 의해 `[]` 로 바뀝니다) 그리고 `size` 속성을 같은 유용한 멤버 함수를 가집니다.

``` kotlin
class Array<T> private constructor() {
  val size: Int
  fun get(index: Int): T
  fun set(index: Int, value: T): Unit

  fun iterator(): Iterator<T>
  // ...
}
```

배열을 생성하려면 함수 `arrayOf()`에 값을 전달해서 생성할 수 있다. `arrayOf(1,2,3,)`은 배열 [1,2,3]을 만든다. 다른 방법을 `arrayOfNulls()` 라이브러리 함수는 널 요소를 채운 배열을 생성해 낸다.
> To create an array, we can use a library function `arrayOf()` and pass the item values to it, so that `arrayOf(1, 2, 3)` creates an array [1, 2, 3].
Alternatively, the `arrayOfNulls()` library function can be used to create an array of a given size filled with null elements.

Another option is to use a factory function that takes the array size and the function that can return the initial value
of each array element given its index:

``` kotlin
// Creates an Array<String> with values ["0", "1", "4", "9", "16"]
val asc = Array(5, { i -> (i * i).toString() })
```

As we said above, the `[]` operation stands for calls to member functions `get()` and `set()`.

Note: unlike Java, arrays in Kotlin are invariant. This means that Kotlin does not let us assign an `Array<String>`
to an `Array<Any>`, which prevents a possible runtime failure (but you can use `Array<out Any>`, 
see [Type Projections](generics.html#type-projections)).

Kotlin also has specialized classes to represent arrays of primitive types without boxing overhead: `ByteArray`,
`ShortArray`, `IntArray` and so on. These classes have no inheritance relation to the `Array` class, but they
have the same set of methods and properties. Each of them also has a corresponding factory function:

``` kotlin
val x: IntArray = intArrayOf(1, 2, 3)
x[0] = x[1] + x[2]
```

## Strings

Strings are represented by the type `String`. Strings are immutable.
Elements of a string are characters that can be accessed by the indexing operation: `s[i]`.
A string can be iterated over with a *for*{: .keyword }-loop:

``` kotlin
for (c in str) {
  println(c)
}
```

### String Literals

Kotlin has two types of string literals: escaped strings that may have escaped characters in them and raw strings that can contain newlines and arbitrary text. An escaped string is very much like a Java string:

``` kotlin
val s = "Hello, world!\n"
```

Escaping is done in the conventional way, with a backslash. See [Characters](#characters) above for the list of supported escape sequences.

A raw string is delimited by a triple quote (`"""`), contains no escaping and can contain newlines and any other characters:

``` kotlin
val text = """
  for (c in "foo")
    print(c)
"""
```


### String Templates

문자열은 템플릿 표현식을, 표현식을 평가한 결과가 문자열에 합쳐지는 기능을 내장하고 있습니다. 템플릿 표현식은 `$` 기호로 시작합니다.

> Strings may contain template expressions, i.e. pieces of code that are evaluated and whose results are concatenated into the string.
A template expression starts with a dollar sign ($) and consists of either a simple name:

``` kotlin
val i = 10
val s = "i = $i" // evaluates to "i = 10"
```

또는 {} 안에 임의의 식으로 사용할 수 있다: or an arbitrary expression in curly braces:

``` kotlin
val s = "abc"
val str = "$s.length is ${s.length}" // evaluates to "abc.length is 3"
```
템플릿은 원래 문자열과 회피 문자열 양쪽을 지원합니다. 예를 들어 $ 문자를 가공하지 않은 문자열에 (백슬래시 회피문자를 지원하지 않는) 표현하려면 아래같이 합니다.
> Templates are supported both inside raw strings and inside escaped strings.
If you need to represent a literal `$` character in a raw string (which doesn't support backslash escaping), you can use the following syntax:

``` kotlin
val price = """
${'$'}9.99
"""
```
