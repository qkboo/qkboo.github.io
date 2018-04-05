## Object Expressions and Declarations

https://kotlinlang.org/docs/reference/object-declarations.html#object-declarations

어떤 때 새 서브 클래스를 만들지 않고, 클래스를 조금 수정한 오브젝트를 생성할 필요가 있다. 자바는 Anonymouse Inner Class 로 처리한다. 코틀린은 *Object Expression*과 *object declaration*으로 다룬다.


### Object declarations

Singleton 은 여러 경우에 유용한데, 코틀린은 쉽게 싱글톤을 선언할 수 있다.

```kt
object DataProviderManager {
    fun registerDataProvider(provider: DataProvider) {
        // ...
    }

    val allDataProviders: Collection<DataProvider>
        get() = // ...
}
```

This is called an object declaration, and it always has a name following the object keyword. Just like a variable declaration, an object declaration is not an expression, and cannot be used on the right hand side of an assignment statement.

Object declaration's initialization is thread-safe.

To refer to the object, we use its name directly:

DataProviderManager.registerDataProvider(...)
Such objects can have supertypes:

```kt
object DefaultListener : MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
        // ...
    }

    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
}
```

> NOTE: object declarations can't be local (i.e. be nested directly inside a function), but they can be nested into other object declarations or non-inner classes.


### Companion Objects

클래스 내부에 객체를 선언할 때는 `companion` 키워드로 지시한다:

```kt
class MyClass {
  companion object Factory {
    fun create(): MyClass = MyClass()
  }
}
```

companion object의 멤버는 클래스 이름을 식별자로 바로 사용한다.

```kt
val instance = MyClass.create()
```

companion object의 이름을 뺄 수 있는데, 이때 이름은 대문자 `Companion` 이 된다.

```kt
class MyClass {
  companion object {
  }
}
val x = Myclass.Companion
```

Note that, even though the members of companion objects look like static members in other languages, at runtime those are still instance members of real objects, and can, for example, implement interfaces:

```kt
interface Factory<T> {
    fun create(): T
}
```


```kt
class MyClass {
    companion object : Factory<MyClass> {
        override fun create(): MyClass = MyClass()
    }
}
```

However, on the JVM you can have members of companion objects generated as real static methods and fields, if you use the @JvmStatic annotation. See the Java interoperability section for more details.

Semantic difference between object expressions and declarations

There is one important semantic difference between object expressions and object declarations:

object expressions are executed (and initialized) immediately, where they are used;
object declarations are initialized lazily, when accessed for the first time;
a companion object is initialized when the corresponding class is loaded (resolved), matching the semantics of a Java static initializer.


### Singleton

[lazy](https://kotlinlang.org/docs/reference/delegated-properties.html#lazy)

companion 과 lazy 함수를 사용해서 singleton 클래스를 구현할 수 있다.

```kt
class Singleton {
    var name: String? = null
    private constructor() {
        println("Instance is created")
    }

    companion object {
        val instance:Singleton by lazy { Singleton() }
    }
}
```

이제 instance 속성을 참조한다

```kt
// first time, instanciating
var s1 = Singleton.instance
s1.name = "James"
println(s1.name)

// after instanciating, call lazy()
var s2 = Singleton.instance
println(s2.name)
```



## 참조
 - https://kotlinlang.org/docs/reference/object-declarations.html#object-declarations