---
title: "Getting Started : 기본 문법의 사용"
permalink: /kotlin/getting-started/
last_modified_at: 2018-03-20T15:58:49-04:00
toc: true
---

kotlinlang.org의 [Getting Started/Basic Syntax](https://kotlinlang.org/docs/reference/basic-syntax.html) 를 정리했다.

- 필요에 따라 내용에서 **[링크:..]** 는 외부 링크이다.


## Basic Syntax

코틀린 코드 작성에 필요한 기본 문법 사용을 보여주고 있다.

### 패키지 선언

소스 파일의 꼭대기에 **package**{: .keyword } 키워드를 사용해서 패키지를 명시합니다:


```kotlin
package my.demo

import java.util.*

// ...
```

소스 파일이 위치한 디렉토리와 패키지 이름이 일치하지 않아도 된다.

[링크: 패키지](https://kotlinlang.org/docs/reference/packages.html)

<br/>

### 함수 정의

함수는 **fun** 키워드로 선언한다

```kotlin
fun name(var:Type,,,)[:Return Type] { 
 
}
```

코드의 시작점으로 *main* 함수를 사용한다.

```kotlin
fun main(args: Array<String>) {

}
```


다음 함수는 두개의 *Int* 매개변수를 가지고 반환 형식으로 *Int*를 선언하고 있다:

```kotlin
fun sum(a: Int, b: Int): Int {
  return a + b
}
```

함수는 Expression body로 반환시 반환 형식을 추정한다. `sum()` 함수를 표현식을 사용해 선언할 수 있다:

```kotlin
fun sum(a: Int, b: Int) = a + b

```

`Unit` 반환 형식의 함수는 값을 반환하지 않을 수 있다:

```kotlin

fun printSum(a: Int, b: Int): Unit {
  print(a + b)
  println("$a + $b = ${a + b}")
}
```

`Unit` 반환 형식은 아래 같이 반환형을 제외한 형태를 표현한 것이다:

```kotlin

fun printSum(a: Int, b: Int) {
  println("$a + $b = ${a + b}")
}
```


[Kotlin functions 참조](/kotlin/kotlin-functions/)


### 변수의 선언

변수는 형식을 지정하고 선언하거나, 임의의 값을 바로 대입할 수 있다. *var*{:.keyword}로 가변형식의 선언 하거나, *val*{:.keyword}로 불변형으로 선언한다.

```
val name[:TYPES] = VALUE    # 불변형 변수
var name[:TYPES] = VALUE    # 가변형 변수
```


~~~kotlin
var hello: String = "안녕하세요"

fun main(args: Array<String>) {
  val hi = "Hi"
  print(hello)
  println(hi)
}
~~~

*val*{:.keyword} 키워드는 변수를 읽기 전용으로, 초기화시 한 번만 값을 대입한다.

~~~kotlin
val foo = 10
#...
foo = 20
~~~

*val*{:.keyword} 로 선언하고 값을 다시 대입하면 *Val cannot be reassigned* 에러가 발생한다.
이렇게 선언하고 컴파일 후 실행하면 다음 에러를 볼 수 있다:

![](/images/kotlin/kotlin-error-val-reassigned.png){: width="500"}


#### val: 불변형 변수 선언

함수 안에서 지역변수를 선언한다.

```kotlin

fun main(args: Array<String>) {
  val a: Int = 1      // 변수 a의 선언과 초기화
  val b = 1           // 'Int' 형식으로 참조
  val; c: Int          // 변수 초기화 (초기값 지정)이 안되면 데이터 형식이 필요하다
  c = 1               // 명확한 대입

  println("a = $a, b = $b, c = $c")
}
```
- 일반적으로 변수는 위와 같이 초기화 한다.


Mutable 변수는 *var*{:.keyword}로 선언한다

```kotlin
fun main(args: Array<String>) {
  var x = 5  // Int 형으로 추정
  x += 1
}
```

{% comment %} [속성과 필드 보기](https://kotlinlang.org/docs/reference/properties.html) {% endcomment %}
[속성과 필드 참조](/kotlin/kotlin-properties/)


### 주석 Comments

또한 함수 혹은 코드 내에서 단어, 줄을 주석 처리 할 수 있다. 주석은 줄 혹은 묶음으로 처리하는데 Java, C 등과 같이 *//*로 한 줄 주석 그리고 */* */*로 묶음 범위에 대해 주석을 처리할 수 있다.

```kotlin
// 한 줄 주석

/*
 * 여러줄에 걸쳐 주석으로 묶을 수 있습니다.
 *
 */
```

자바와 코틀린은 달리 블럭 주석은 중첩할 수 있다:

[코트린 코드 문서화 링크](https://kotlinlang.org/docs/reference/kotlin-doc.html)


### 문자열 템플릿 사용

문자열 안에서 *문자열 템플릿 표현식*을 사용 할 수 있다. *$*{:.keyword} 기호로 시작하는 문자열 템플릿로 문자열에서 변수를 참조하게 할 수 있다.


```kotlin
fun main(args: Array<String>) {
  if (args.size == 0) return

  print("First argument: ${args[0]}")
}
```


[문자열 템플릿 링크](https://kotlinlang.org/docs/reference/basic-types.html#string-templates)


### 조건 표현식 사용하기

조건 판단을 위해서 *if*{:.keyword } 키워드를 사용하는데 다음 같이 **()** 안에 논리식을 이용해서 조건 판단을 한다.

```kotlin
fun max(a: Int, b: Int): Int {
  if (a > b)
    return a
  else
    return b
}
```

위의 코드는 조건 판단을 위해서 **if express** 표현식을 사용해서 Expression body에 적용할 수 있다. 위 함수는 아래 같은 *if*{:.keyword} 표현식으로 사용될 수 있다:

```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```


> [링크: if-expression](https://kotlinlang.org/docs/reference/control-flow.html#if-expression)


### *null*{:.keyword} 가능한 값과 점검

Null-safety 기법으로 *null*{:.keyword}에 안전한 코드를 작성하게 제공하기 위해서 *?*{:.keyword} Null-safety 키워드를 사용할 수 있다.
이것은 변수 참조시 *null*{:.keyword} 값 가능성이 있으면, *null*{:.keyword} 일 수 있음을 명시해서 널에 안전한 코드를 만들 수 있다. 

아래 같이 `null`  인 값이 지정되면 컴파일 에러를 보인다.

![](/images/kotlin/kotlin-nullsafety-null.png){: width="500"}

여기에 null-safety `?` 를 선언해주면 컴파일 에러가 없이 `null`  경우 null 값으로 취급한다.

![](/images/kotlin/kotlin-nullsafety.png){: width="500"}

그런데 꼭 `null` 체크를 해야 한다면 `!!`{:.keyword} 연산자를 변수에 붙여 준다.

```kotlin
fun main(args: Array<String>) {
    var name:String?
    name = null
    print( name!!  )
}
```

이제 name이 `null` 이면 NullPointException을 발생한다.

#### 널 반환

아래 parseInf()라는 함수를 선언시 반환 형식에 다음 같이 *Int?*{:.keyword} 안전호출 연산자를 붙여 반환식에 사용하면 값이 *null*{:.keyword}이 아니면 값을, 반대면 *null*{:.keyword}을 반환한다.

```kotlin
fun parseInteger(s: String): Int? {
    return s.toInt()
}

fun printProduct(arg1: String, arg2: String) {
    val x = parseInt(arg1)
    val y = parseInt(arg2)

    // Using `x * y` yields error because they may hold nulls.
    if (x != null && y != null) {
        // x and y are automatically cast to non-nullable after null check
        println(x * y)
    }
    else {
        println("either '$arg1' or '$arg2' is not a number")
    }    
}

fun main(args: Array<String>) {
    printProduct("6", "7")
    printProduct("a", "7")
    printProduct("a", "b")
}

```

null 체크의 전통적이고 다른 방법으로 *==*{:.keyword} 동등비교 연산자를 사용해서 다음 같이 사용할 수 있다. *null*{:.keyword}인 경우 *return*{:.keyword} 반환하면 된다.

```kotlin
  if (x == null) {
    print("Wrong number format in '${args[0]}'")
    return
  }
  if (y == null) {
    print("Wrong number format in '${args[1]}'")
    return
  }
```


[Null-safety](/kotlin/kotlin-null-safety/})

[링크: Null-safety](https://kotlinlang.org/docs/reference/null-safety.html)



### 형 점검과 자동 형 캐스트

*is*{:.keyword} 연산자는 변수가 데이터 형에 맞는 인스턴스인지 점검 할 수 있다. *is*{:.keyword} 연산자는 만약 불변 변수 혹은 속성이 특정 형식에 대해 형으로 판단 되면 명시적으로 캐스트 할 필요가 없이 형 판단을 한다.

```kotlin
fun getStringLength(obj: Any): Int? {
  // 여기서 `obj`는 자동으로 `String`에 캐스팅된다.
  if (obj is String) {
    return obj.length
  }
  // 형 체크 범위 밖에서는 'obj'는 'Any'형으로 판단된다.
  return null
}

fun main(args: Array<String>) {
    fun printLength(obj: Any) {
        println("'$obj' string length is ${getStringLength(obj) ?: "... err, not a string"} ")
    }
    printLength("Incomprehensibilities")
    printLength(1000)
    printLength(listOf(Any()))
}
```
 - 문자열 템플릿 연산자 *$*{:.keyword}는 함수도 표현식에 사용할 수 있다.

다음 함수에서 Any 형 객체가 스트링 관련 처리 요청시 역시 스트링으로 캐스팅된다. 다음 같이 obj.lengh 스트링 문자열 크기를 요청하면 스트링 캐스팅 된다.

```kotlin
fun getStringLength(obj: Any): Int? {
    if (obj !is String) return null

    // `obj` is automatically cast to `String` in this branch
    return obj.length
}
```

비슷하게 다음 조건식 *if* 절에서 `obj`는 오른쪽 `&&` 뒤에서 자동으로 스트링 캐스팅된다.

```kotlin
fun getStringLength(obj: Any): Int? {
  if (obj is String && obj.length > 0)
    return obj.length

  return null
}
```

[링크: 클래스](https://kotlinlang.org/docs/reference/classes.html)

[링크: 캐스트](https://kotlinlang.org/docs/reference/typecasts.html)



### for 반복문 사용

for 반복문은 *for ... in*{:.keyword} 반복문에 컬렉션을 사용한다.

```kotlin
for (x in 1..5)
  print(x)
```

*for*{:.keyword}와 *in*{:.keyword} 연산자에 컬렉션 요소를 직접 사용한다

```kotlin
fun main(args: Array<String>) {
  for (arg in args)
    print(arg)
}
```

혹은 컬렉션 인덱스를 사용해서 요소에 접근할 수 있다.

```kotlin
for (i in args.indices)
  print(args[i])
```


[Controlflow: for loop](/kotlin/kotlin-controlflow/})

[링크: Controlflow: for loop](https://kotlinlang.org/docs/reference/control-flow.html#for-loops) 


### while 반복문 사용

```kotlin
fun main(args: Array<String>) {
  var i = 0
  while (i < args.size)
    print(args[i++])
}
```

[Controlflow: for loop](/kotlin/kotlin-controlflow/)

[링크: Controlflow: for loop](https://kotlinlang.org/docs/reference/control-flow.html#while-loops) 



### when 표현식

*when()*{:.keyword} 안에 주어진 인스턴스의 식에 따라 분기 한다. 값에 따른 분기 판단은 *->*{:.keyword} 연산자를 사용하고 값의 형식은 자동 연산된다.

```kotlin
fun describe(obj: Any): String =
when (obj) {
    1          -> "One"
    "Hello"    -> "Greeting"
    is Long    -> "Long"
    !is String -> "Not a string"
    else       -> "Unknown"
}

fun main(args: Array<String>) {
    println(describe(1))
    println(describe("Hello"))
    println(describe(1000L))
    println(describe(2))
    println(describe("other"))
}
```

[링크: when 표현](https://kotlinlang.org/docs/reference/control-flow.html#when-expression)


### 범위 연산자 사용하기

*..*{:.keyword} 연산자는 `rangeTo()` 함수의 연산자 형태로 사용한다. *in*{:.keyword} 연산자에 사용해서 주어진 범위 사이의 숫자를 점검할 수 있다.

```kotlin
if (x in 1..y-1)
  print("OK")
```

*!*{:.keyword} 연산자와 함께 사용해서 범위를 벗어나는지 점검한다:

```kotlin
val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
    println("-1 is out of range")
}
if (list.size !in list.indices) {
    println("list size is out of valid list indices range too")
}
```

*for*{:.keyword} 반복문으로 범위 만큼 반복한다. 반복시 `downTo()`, `step()` 함수와 함께 단계로 뛰어 넘을 수 있다.

```kotlin
for (x in 1..5) {
    print(x)
}

for (x in 1..10 step 2) {
    print(x)
}

for (x in 9 downTo 0 step 3) {
    print(x)
}
```


[링크: Ranges 표현식](https://kotlinlang.org/docs/reference/ranges.html) 



### 컬렉션 사용

컬렉션 데이터를 *in*{:.keyword} 연산자와 같이 사용해 데이터 끝까지 반복 할 수 있다.

```kotlin
fun main(args: Array<String>) {
    var names = listOf( "이순신", "홍길동", "강감찬", "박문수", "유성룡")
 
    for( name in names) {
        println(name)
    }
}
```

*if*{:.keyword} 조건절에서는 *in*{:.keyword} 연산자와 함께 사용해 컬렉션이 객체를 내포하는지 점검할 수 있다.

```kotlin
if( "박문수" in names) {
    println("어사 박문수가 있습니다")
}
```

람다 표현식을 사용해서  컬렉션을 필터하고 사상 할 수 있다

```kotlin
  val fruits = listOf("banana", "avocado", "apple", "kiwi")
  fruits
  .filter { it.startsWith("a") }
  .sortedBy { it }
  .map { it.toUpperCase() }
  .forEach { println(it) }
```

---

[링크: 고수준 함수와 람다](https://kotlinlang.org/docs/reference/lambdas.html)

[링크: 람다 표현식](http://kotlinlang.org/docs/reference/lambdas.html#lambda-expressions-and-anonymous-functions)


