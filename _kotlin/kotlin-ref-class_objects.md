##

새 클래스는 `class` 키워드로 선언한다.

```kt
class Person() {
}
```

클래스 Person 의 객체는 클래스 연산자를 사용해 정의할 수 있다.

```kt
var myCar = Person()
```

> 코틀린에는 `new` 키워드는 없다


class Members

Classes can contain:

- Constructors and initializer blocks
- Functions
- Properties
- Nested and Inner Classes
- Object Declarations


### Constrcutor

코틀린 생성자는 primary constructor와 secondary constructor 가 있다.

```kt
class Person constructor(firstName: String) {
}
```

`constructor` 키우드를 빼고 사용할 수 있다.

```kt
class Person(firstName: String) {
}
```

#### `init{ }` 블럭

클래스 Person 초기화는 `init` 블럭에서 시작한다.

```kt
class Person() {
    init {
        println("Class is created")
    }
}
```

#### `constructor` 키워드

만약 생성자가 어노테이션과 가시성 접근자를 가지면 `constructor` 키워드를 사용해야 한다.

```kt
class Customer public @Inject constructor(name: String) { ... }
```


#### Secondary constructor

secondary constructor는 특수한 함수 형태로 `constructor` 구문으로 선언하고 var, val 변수 키워드는 필요없다.

```kt
class Person() {
    constructor(firstName:String, lastName:String, age:Int ) : this() {
    }
}
```


```kt
class Person {
    constructor(parent: Person) {
        parent.children.add(this)
    }
}
```


Java 스타일로 속성과 초기화를 할 수 있다.

```kt
// java style

class Person2() {
    var firstName:String? = null
    var lastName:String? = null
    var age:Int? = null
    
    constructor(firstName:String, lastName:String, age:Int ) : this() {
        this.firstName = firstName
        this.lastName = lastName
        this.age = age
    }
}

```



#### Properties

클래스 속성과 초기화는 아래 같이 생성자에 선언자를 사용해 선언한다.

```kt
class Person(val firstName: String, val lastName: String, var age:Int) {

}
```

클래스 속성을 생성자로 초기화 하고 객체에서 접근 연산자 `.` 로 필드에 접근하면, 내부의 `get` 그리고 `set` 메서드로 사용 치환되게 된다. 

```kt
var guest = Person("게스트", "", 99)
var jully = Person("Julian", "Andrian", 21)

println("${jully.lastName} ${jully.firstName} is ${jully.age}")
```



### Inheriance

코틀린에서 class 선언은 암묵적으로 `Any` 클래스를 상속한다고 정의되어 있다. 그리고 class 선언시 접근제한자가 없으면 `final` 클래스라고 판단된다.

```kt
class Person() {  // inherits from Any
}
```

상속 가능한 클래스는 `open` 접근제한 어노테이션을 사용해서 선언한다. 그리고 특정 형식을 상속 받으려면 선언시 `:` 다음에 클래스를 정의한다. 그리고 상속 클래스에 primary constructor 초기화를 선언시 할 수 ㅇㅆ다.

```kt
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```

또한 constructor 에서 super() 로 초기화를 할 수 있다.

```kt
open class Base(p: Int)

class Derived2 : Base {
    constructor(p:Int): super(p)
}
```



실제로 

```kt
open class Operations() {
    fun sum(a:Int, b:Int):Int {
        return a+b
    }
    fun div(a:Int, b:Int):Int {
        return a / b
    }
}
```

이 Operations 클래스를 상속해 곱셈 기능을 가진 클래스를 선언한다면,

```kt
class Multioperations(): Operations() {
    fun mul(a: Int, b: Int):Int {
        return a*b
    }
}
```


#### constructor 로 초기화

```kt
class FirstThread:Thread {
    constructor():super()
    constructor(name:String):super() {
        this.name = name
    }
}

var t1 = FirstThread("First")
var t2 = FirstThread("Second")
var t3 = FirstThread()
```


#### 메서드 오버라이딩

자바와 달리 코틀린은 명확한 표기를 요구한다. 그래서 메서드 오버라이드시 `override` 어노테이션 선언하고 상속하는 클래스에서 메서드에 `open` 접근 제한을 해야 한다.


```kt
open class Base {
    open fun v() {}
    fun nv() {}
}
class Derived() : Base() {
    override fun v() {}
}
```

그리고 override 로 지시한 멤버는 `open` 접근제한으로 이를 저지하려면 `final` 어노테이션을 같이 사용한다.

```kt
class Derived() : Base() {
    final override fun v() {}
}
```


#### 속성 오버라이딩

멤버 속성도 메서드 오버라이딩과 비슷하게 동작한다; 슈퍼클래스 멤버 속성을 오버라이드하는 클래스에서 `override` 어오테이션으로 선언해주지만 같은 데이터 형을 사용해야 한다.


```kt
open class Foo {
  open val x: Int get() { ... }
}
class Bar1 : Foo() {
  override val x: Int = ...
}
```

val 변수를 var로 오버라이드 할 수 있지만 그 반대는 안된다. val 속성은 getter 메서드를 가지고, 오버라이드한 var 변수는 setter 메서드를 가진다.

이 `override` 키워드를 primary constructor에 사용할 수 있다. 다음 interface 를 구헌하는 코드를 보자;

```kt
interface Foo {
  val count: Int
}


class Bar1(override val count:Int) : Foo

class Bar2 : Foo {
  override var count: Int = 0
}

```



#### Class casting

`as` 키워드로 캐스팅할 수 있다.

아래 두 클래스가 있으면 

```kt
open class Foo {
  open val x: Int get() { return 5 }
}
class Bar1 : Foo() {
  override val x: Int = 4
}
```



### Abstract class

클래스 와 멤버가 `abstract` 로 선언할 수 있다. abstract 멤버는 클래서 안에서 구현하지 않는다. abstract class , function은 open 어노테이션이 필요없다.

또한 일반 멤버를 abstract 로 오버라이드 할 수 있다.

```kt
open class Base {
    open fun f() {}
}

abstract class Derived : Base() {
    override abstract fun f()
}
```