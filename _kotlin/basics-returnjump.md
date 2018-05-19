---
title: "Basics / Return 과 Jump "
permalink: /kotlin/basics-returnjump/
last_modified_at: 2018-03-20T15:58:49-04:00
toc: true
---

kotlinlang.org의 [Basics/Returns and Jump](https://kotlinlang.org/docs/reference/returns.html) 를 정리했다.

- 필요에 따라 내용에서 **[링크:..]** 는 외부 링크이다.

## Returns and jumps

코틀린은 세가지 점프 연산자를 가지고 있습니다.
 - **return** 함수 혹은 무명함수의 끝 부근에서 기본 반환
 - **break** 닫힌 루프를 종료시킨다
 - **continue** 닫힌 루프의 다음 단계로 진행한다 

### Break와 Continue 라벨

코틀린 표현식은 **label* 로 마크할 수 있다. 라벨은 `@` 사인 뒤에 식별자를 주는데 예를 들어 `abc@`, `foo@` 형식이다. 표현식을 라벨에 마크하려면 그것의 앞에 라벨을 위치한다.

``` kotlin
loop@ for (i in 1..100) {
  // ...
}
```

이제 *break* 혹은 *continue*를 라벨과 붙여써서 식별할 수 있다:

```kotlin
loop@ for (i in 1..100) {
  for (j in 1..100) {
    if (...)
      break@loop
  }
}
```

**break**는 마크한 실행 지점으로 점프한다. **continue** 는 루프의 다음 반복으로 진행한다.


### 라벨에서 Return

코틀린의 함수 리터럴, 지역 함수와 객체 표현식,에서 함수는 중첩할 수 있다. 식별한 **returns**는 다른 함수에서 반환을 허용한다. 가장 대표적인 쓰임새는 람다 표현식에서 반환하는 것으로 아래 같이 쓸 때 다시 호출된다.

```kotlin
fun foo() {
  ints.forEach {
    if (it == 0) return
    print(it)
  }
}
```


**return** 표현식은 가까운 닫힌 함수에서 반환한다. 예를 들어 위의 foo 함수 (이런 지역외 반환은 인라인 함수에 전달되는 람다 표현식에서만 지원한다). 만약 람다 표현식에서 반환할 필요가 있다면 그것에 라벨을 명시하고 **return** 을 명시한다.

```
fun foo() {
  ints.forEach lit@ {
    if (it == 0) return@lit
    print(it)
  }
}
```

이번에 람다 표현식에서만 반환을 한다면 다음 같이 임의의 라벨을 사용하는 것이 더욱 일반적이다; 이런 임의의 라벨은 함수와 같은 이름을 갖는다.

```
fun foo() {
  ints.forEach {
    if (it == 0) return@forEach
    print(it)
  }
}
```

그 대신에 무명함수로 람다 표현식을 교체할 수 있는데, 무명 함수 안의 **return** 구분은 무명 함수 자신에서 반환할 것입니다.

```
fun foo() {
  ints.forEach(fun(value: Int) {
    if (value == 0) return
    print(value)
  })
}
```

When returning a value, the parser gives preference to the qualified return, i.e.

```
return@a 1
```

여기서 **라벨 @a에 1을 반환하기** "return 1 at label @a" 그리고 라벨 표현식 "**(@a 1)**"을 반환하지 않는다.