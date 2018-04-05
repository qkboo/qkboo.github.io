# Coding Conventions

코클린 언어를 위한 코딩 스타일을 다루고 있습니다.


## Naming Style

아래 같은 자바 코딩 습관을 기본으로 합니다:
> If in doubt default to the Java Coding Conventions such as:

* camelCase 이름을 사용한다 ( 그리고 이름에 밑줄_는 피한다)
* 데이터형은 대문자로 시작한다 types start with upper case
* 메서드와 속성은 소문자로 시작한다 methods and properties start with lower case
* 4개의 공백으로 들여쓰기를 한다 use 4 space indentation
* public functions should have documentation such that it appears in Kotlin Doc

## Colon

콜론이 데이터형과 상위 데이터형을 콜론으로 구분하는 곳은 공백을 주어야 하지만 인스턴스와 데이터형을 구분하는 곳은  콜론에 공백이 없습니다

``` kotlin
interface Foo<out T : Any> : Bar {
    fun foo(a: Int): T
}
```

## Lambdas

람다 표현식에서 공백은 컬 브레이스 주변에서 사용해 주고, 역시 매개변수를 나누는 화살표 주변에도 사용한다. 가능하다면 람다는 괄호의 바깥쪽으로 빼준다.

> In lambda expressions, spaces should be used around the curly braces, as well as around the arrow which separates the parameters
from the body. Whenever possible, a lambda should be passed outside of parentheses.

``` kotlin
list.filter { it > 10 }.map { element -> element * 2 }
```

짧고 중첩하지 않은 람다식에서 매개변수를 선언하지 않고 그대로 사용하는 것을 권장낳다. (`it` convention) . 파라매터와 중첩한 람다에서 파라미터는 명시적으로 선언해야 한다.

## Unit

함수가 Unit을 반환하면 반환 형식이 있는것으로 판단한다:

> If a function returns Unit, the return type should be omitted:

``` kotlin
fun foo() { // ": Unit" is omitted here

}
```