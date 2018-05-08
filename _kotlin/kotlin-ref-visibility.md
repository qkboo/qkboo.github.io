## Visibility

코틀린의 클래스, 객체, 인터페이스, 생성자, 함수, 속성 과 셋터는 가시성 제한자를 가진다. (겟터는 항상 속성과 같은 가시성을 갖는다) 가시성은 4가지 `private`, `protected`, `internal` 그리고 `public` 있다. 명시적인 제한자가 없으면 기본 가시성은 `public` 이다.


### Packages

Functions, properties 그리고 classes, objects 그리고 interfaces 들은 최상위 위치에 선언할 수 있다. 아래 같이 직접 패키지 내부에 선언한다:

```kt
// 파일 이름: example.kt
package foo

fun baz() {}
class Bar {}
```

최상위 위치의 가시성 제한은 다음 같다
 - 특정 가시성 제한자를 명시하지 않므년 `public` 이 적용되어 어느 코드에서나 접근이 가능하다.
 - `private` 제한자를 선언하면, 선언한 파일 내에서만 접근이 가능하다
 - `internal` 제한자를 선언하면 같은 모듈에서 접근이 가능하다.
 - 최상위 위치에서 `protected` 는 가능하지 않다.
 - 다른 패키지에서 최상위 선언을 사용하려면 `import` 를 사용한다.

```kt
// 파일 이름: example.kt
package foo

private fun foo() {}  // example.kt 파일에서만 접근

public var bar: Int = 5 // 어디서나 접근
  private set           // 셋터는 example.kt에서만 접근

internal val baz = 6    // 같은 모듈에서만 접근
```


### Class 와 Interface

클래스와 인터페이스 가시성 제한자는
 - `private` : 클래스 내부에서만 접근
 - `protected` : `private`와 같지만 subclass 에서도 접근 가능
 - `internal` : 모듈 내부의 클래스를 선언하면 그것의 internal 멤버를 접근 가능
 - `public` : 클래스를 선언한 클래스에서 public을 볼 수 있다.

> NOTE for Java users: outer class does not see private members of its inner classes in Kotlin.

`protect` 멤버를 오버라이드 하고 가시성을 명시하지 않으면, 오버라이드한 멤버는 `protec` 가시성을 갖는다.


```kt
open class Outer {
  private val a = 1
  protected open val b = 2
  internal val c = 3
  val d = 4               // public


  protected class Nested {
    public val e: Int = 5
  }
}

class Subclass : Outer() {
  // a is not visible
  // b, c and d are visible
  // Nested and e are visible

  override val b = 5    // b is protected
}

class Unrelated(o: Outer) {
  // o.a, o.b are not visible
  // o.c, o.d are visible (same module)
  // Outer.Nested is not visible, Nested::e is not visible
}

```


### Constructors

클래스의 primary constructor 의 가시성을 명시하려면 아래 문법을 사용한다.

```kt
class Foo private contructor(a: Int) { ... }
```

위 생성자는 private이다. 모든 contructor는 기본 `public` 으로 클래스가 보이는 모든 곳에서 보여진다.

#### 로컬 선언

로컬 변수, 함수 그리고 클래스는 가시성 제한자를 갖지 않는다.


### Modules

`internal` 가시성은 멤버가 같은 모듈에서만 보여진다고 선언한다. 모듈은 코틀린에서 함께 컴파일한 파일 집합을 말한다.
 - IntelliJ IDEA module
 - Maven project
 - Gradle source
 - Ant 태ㅅ크로 컴파일한 파일 집합




## 참고
 - https://kotlinlang.org/docs/reference/visibility-modifiers.html


