---
title: "Basics / Control Flow"
permalink: /kotlin/basics-controlflow/
last_modified_at: 2018-03-20T15:58:49-04:00
toc: true
---

kotlinlang.org의 [Basics/Controllflow](https://kotlinlang.org/docs/reference/control-flow.html) 를 정리했다.

- 필요에 따라 내용에서 **[링크:..]** 는 외부 링크이다.

## Control Flow

### If Expression

코틀린에서 *if*는 표현식이다, 예로 이것은 값을 반환한다.
그러므로 표현식에 삼중연산자 ( condition? then : else)가 없다, 이것은 *if* 가 이 역할을 할 수 있기 때문이다.

``` kotlin
// 전통적인 사용 
var max = a 
if (a < b) 
  max = b 
 
// With else 
var max: Int
if (a > b) 
  max = a 
else 
  max = b 
 
// 표현식으로  
val max = if (a > b) a else b
```

*if* 분기는 구획(Block)을 지을 수 있고, 마지막 표현식이 해당 구획의 값이다:

``` kotlin
val max = if (a > b) { 
    print("Choose a") 
    a 
  } 
  else { 
    print("Choose b") 
    b 
  }
```

만약 *if*를 구문 (예를 들어 값을 반환 하거나 변수에 대입)보다 표현식으로 사용한다면, 표현식은 `else` 분기를 가져야 한다. 

문법 [grammar for *if*](grammar.html#if) 참조.

> 샘플: controlflow_if.kt


### When Expression

*when* 은 C 같은 언어의 스위치 연산자를 대체한다. 아래같이 단순한 형태를 갖는다:

``` kotlin
when (x) {
  1 -> print("x == 1")
  2 -> print("x == 2")
  else -> { // Note the block
    print("x is neither 1 nor 2")
  }
}
```

*when*의 매개변수는 모든 분기를 순차적으로 만족하는 분기 상태까지 일치시켜 갑니다.
*when*은 표현식 혹은 구문으로 사용할 수 있습니다. 만약 표현식으로 사용하면, 만족하는 분기의 값이 전체 표현식의 값이 됩니다.

```kotlin
var z = when(x) { "abc" -> "문자" "str" -> "굴자" else -> "No matches"}
```

만약 *when*이 구문으로 사용되면 각 분기의 값은 무시됩니다. (*if* 처럼 각 분기는 구획일 수 있고, 구획에서 마지막 표현식이 값이다)

*else* 구획은 다른 구획 상태가 만족하지 않으면 진행하게 된다.
The *else*{: .keyword } branch is evaluated if none of the other branch conditions are satisfied.

*when*이 표현식으로 사용되면 *else* 구획은 의무적으로 사용해야 한다, unless the compiler can prove that all possible cases are covered with branch conditions.

구획에 상태 조건이 여러가지 이면 콤마로 묶어서 사용할 수 있다:

``` kotlin
when (x) {
  0, 1 -> print("x == 0 or x == 1")
  else -> print("otherwise")
}
```

구회 상태에 임의의 표현식(꼭 상수가 아닌) 을 사용할 수 있다:
We can use arbitrary expressions (not only constants) as branch conditions

``` kotlin
when (x) {
  parseInt(s) -> print("s encodes x")
  else -> print("s does not encode x")
}
```

브랜치에는 *in* 혹은 *!in* [범위](ranges.html) 사이의 존재 혹은 컬렉션에서 점검할 수 있다:
We can also check a value for being *in*{: .keyword } or *!in*{: .keyword } a [range](ranges.html) or a collection:

``` kotlin
// in and collections
var list = listOf(1,2,3,4)
when(i) {
    in 20..40 -> print("x is in the range")
    in list -> print("x is valid")
    !in 100..200 -> print("x is outside the range")
    else -> print("none of the above")
}
```

특정 형을 점검하기 위해서 *is* 혹은 *!is* 를 사용해서 값이 가진 형을 확인할 수 있다. 단 [smart casts](typecasts.html#smart-casts) 에 의해서 다른 점검 없이 메서드와 속성에 접근할 수 있다.

```kotlin
val hasPrefix = when(x) {
  is String -> x.startsWith("prefix")
  else -> false
}
```

*when*은 *if*-*else* *if* 체인을 대체할 수 있다. 만약 매개변수가 제공되지 않으면 브랜치 상태는 불리언 표현식이고, 브랜치는 그 상태가 true이면 실행된다.

``` kotlin
when {
  x.isOdd() -> print("x is odd")
  x.isEven() -> print("x is even")
  else -> print("x is funny")
}
```

*when* 문법 참조 [grammar for *when*](grammar.html#when).


> 샘플: controlflow_when.kt


### For Loops

*for* 반복문을 제공하는 곳에서 반복한다. 문법은 아래 같다:
*for* loop iterates through anything that provides an iterator. The syntax is as follows:

``` kotlin
for( item in 20..100)
    print("$item:")
```

또한 for 본문은 구획을 할 수 있다:

``` kotlin
var items: IntArray = intArrayOf(1,2,3,4,5,6)
for(item: Int in items) {
    print("$item:")
}
```

앞서 언급했듯이 *for*는 반복자를 제공하는 곳에서 반복을 진행합니다. 예를 들어,
* 멤버가 있거나 - 반환 형의 `iterator()` 확장 함수
  - 멤버가 있거나 - `next()` 확장 함수 그리고
  - 멤버가 있거나 - `Boolean`을 반환하는 `hasNext()` 확장 함수

이 세가지 함수가 `operator`로 마크 되어야 한다.

배열의 `for` 반복은 반복 개체를 생성하지 않고 인덱스 기반의 반복으로 컴파일한다.
배열 혹은 리스트를 인덱스를 통해 반복하려면 아래 같이 한다:

``` kotlin
for( i in items.indices)
    print("${items[i]}:")
```

> 이 "범위 내에서 반복"은 새로운 부가적인 객체를 생성하는 것 없이 최적화되어 컴파일 된다.  Note that this "iteration through a range" is compiled down to optimal implementation with no extra objects created.

또한 `withIndex` 라이브러리 함수를 사용할 수 있다:
Alternatively, you can use the `withIndex` library function:

``` kotlin
// withIndex
for( (index, value) in items.withIndex()) {
    print("$index번째 요소: $value:")
}
```

참조: [grammar for *for*](grammar.html#for).

> 샘플: controlflow_for.kt


### While Loops

일반적인 언어의 사용과 비슷한 *while* 그리고 *do..while*:

``` kotlin
while (x > 0) {
  x--
}

do {
  val y = retrieveData()
} while (y != null) // y is visible here!
```

See the [grammar for *while*{: .keyword }](grammar.html#while).

### 반복문에서 Break 와 continue

코틀린은 전통적인 반복문에서 *break*와 *continue* 연산자를 지원한다.

참조: [Returns and jumps](returns.html).

