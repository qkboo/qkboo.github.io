## Extensions

C# 과 Gosu 에 비슷하게 클래스 상속 혹은 데코레이터 디자인 패턴을 사용하지 않고도 새로운 기능을 확장할 수 있게 하고 있다. 이것은 `extensions` 으로 불리는 확장 기능을 통해 새 함수와 속성을 부여하는 것을 지원한다.

### Extension Functions

함수를 익스텐션으로 선언하기 위해서 확장할 형을 이름으로 사용해서 `.` 로 확장할 함수를 정의하게 된다. 아래는 `MutableList` 에 새로운 함수 `swap` 을 정의하고 있다.

```kt
fun MutableList<Int>.swap(index1:Int, index2:Int) {
  val tmp = this[index1] // 'this' 는 MutableList
  this[index1] = this[index2]
  this[index2] = tmp
}
```

또한 매개변수 `T` 를 사용해 제너릭으로 만들 수 있다. 함수 제너릭은 함수가 받아 들이는 형의 표현인 제너릭 형 매개변수를 함수 이름 앞에 정의한다.

```kt
fun <T> MutableList<T>.swap(index1:Int, index2:Int) {
  val tmp = this[index1] // 'this' 는 MutableList
  this[index1] = this[index2]
  this[index2] = tmp
}
```


### 익스텐션는 고정되어 밝혀진다

확장은 클래스를 수정해서 새 멤버를 추가하는 것은 아니지만 `.` 표기로 새 멤버가 고정된 형을 것을 참조할 수 있다. 아래 D.foo() 확장을 하지만 먼저 C.foo() 확장 함수가 결정되어 있으므로 프린트 문에서 C.foo()가 호출되는 것이다.

```kt
open class C

class D: C()

fun C.foo() = "c"
fun D.foo() = "d"

fun printFoo(c: C) {
  println(c.foo())
}

printFoo(D())
```

확장 함수는 런타임시 호출하는 자료 형이 아니라 원형이 고정적으로 처리된다.

그래서 확장 멤버를 오버라이드 해도 같은 이름을 가지지만 다른 시그너처이므로 괜잖다.

```kt
class C {
  fun foo() { println("Member")}
}

fun C.foo(i:Int) { println("Extension")}
```

그래서 C 클래스 인스턴스의 foo(1)는 확장 함수를 처리한다.


### Null 인 받는 측

널이 가능한 수신자 형을 정의할 수 있다. 객체에서 호출할 때 널이면 내부에서 `this == null` 을 점검한다. 그래서 아래 코드 같이 널을 점검 할 필요 없이 `toString()` 을 사용할 수 있는 것이다. 

```kt
fun Any?.toString(): String {
  if( this == null ) return "null"
  // after the null check, 'this' is autocast to a non-null, 
  // so the toString() below resolves to the member function to the Any class
  return toString()
}
```


### 확장 속성

확장 함수와 비슷하게 확장 속성을 지원한다.

```kt
val <T> List<T>.lastIndex: Int
  get() = size -1
```
