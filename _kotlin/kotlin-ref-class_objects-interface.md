## Interfaces

코틀린 interface는 java8 과 비슷하다. 추상 메서드를 선언할 수 있고, 메서드를 구현할 수 있다. 추상 클래스와 다른 점은 상태를 저장할 수 없다. 인터페이스는 interface 키워드로 선언한다.

```kt
interface MyInterface {
    fun bar()
    fun foo() {
      // optional body
    }
}
```

### 인터페이스 구현

클래스, 오브젝트는 하나 그 이상의 인터페이스를 구현할 수 있다.

```kt
class Child : MyInterface {
  override fun bar() {
    // body
  }
}
```


### 인터페이스 속성

인터페이스에 속성을 선언할 수 있다. 인터페이스 속성은 absgtract 일 수 있고, 구현하는 자손에서 구현할 수 있다.

```kt
interface MyInterface {
    val prop: Int // abstract

    val propertyWithImplementation: String
        get() = "foo"

    fun foo() {
        print(prop)
    }
}

class Child : MyInterface {
    override val prop: Int = 29
}
```


### Resolving overriding conflicts

여러 상위 형식들을 상속하면 이 인터페이스들 중에 같은 메서드를 가질 수 있다.

```kt
interface A {
    fun foo() { print("A") }
    fun bar()
}

interface B {
    fun foo() { print("B") }
    fun bar() { print("bar") }
}

class C : A {
    override fun bar() { print("bar") }
}

class D : A, B {
    override fun foo() {
        super<A>.foo()
        super<B>.foo()
    }

    override fun bar() {
        super<B>.bar()
    }
}
```

Interfaces A and B both declare functions foo() and bar(). Both of them implement foo(), but only B implements bar() (bar() is not marked abstract in A, because this is the default for interfaces, if the function has no body). Now, if we derive a concrete class C from A, we, obviously, have to override bar() and provide an implementation.

However, if we derive D from A and B, we need to implement all the methods which we have inherited from multiple interfaces, and to specify how exactly D should implement them. This rule applies both to methods for which we've inherited a single implementation (bar()) and multiple implementations (foo()).




### Comparable

[Comparable](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-comparable/index.html) 인터페이스는 클래스 객체의 비교를 위해 제공한다. 

Collection 클래스에서 제공하는 `sort()` 는 기본 스트링에 대한 비교만 제공한다.

```kt
var listOfNames = ArrayList<String>()
listOfNames.add("Kim")
listOfNames.add("Goh")
listOfNames.add("Jang")

Collections.sort(listOfNames)
```

사용자 클래스는 새로운 규칙에 따라 정의해 주어야 한다. 이를 제공하는 인터페이스가 [Comparable](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-comparable/index.html) 이다.

다음 Person 클래스가 있다.

```kt
class Person(var name:String, var age:Int) { ... }
var listPersons = ArrayList<Person>()
listPersons.add(Person("Kim", 20))
listPersons.add(Person("Ryu", 22))
listPersons.add(Person("Jeon", 29))
listPersons.add(Person("Lee", 19))

Collections.sort(listPersons)           // 에러!!!
```

이 경우 Comparable<Person>을 구현하지 않았다는 컴파일 타임 에러가 난다.

그래서 Comparable 을 구현해 주어야 한다.

```kt
class Person(var name:String, var age:Int): Comparable<Person> {
    override fun compareTo(other: Person): Int {
        return this.age - other.age
    }
}
```




##

https://kotlinlang.org/docs/reference/interfaces.html