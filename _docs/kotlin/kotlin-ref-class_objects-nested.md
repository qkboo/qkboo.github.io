## Nested 와 Inner

https://kotlinlang.org/docs/reference/nested-classes.html

### Nested

클래스 내부에 다른 클래스를 중첩할 수 있다:

```kt
class Outer {
    private val bar: Int = 1
    class Nested {
        fun foo() = 2
    }
}

val demo = Outer.Nested().foo() // == 2
```

Nested 클래스 안에서 Outer class의 멤버를 접근할 수 없다.

```kt
class Outer {
    private val name:String? = null

    class Nested {
        fun show() {
            // nested class can't access outer class's member
            // println("$name")
            println("Nested")
        }
    }
}
```



### Inner

`inner` 로 지시된 클래슨느 아우터 클래스의 멤버를 접근할 숭 ㅣㅆ다.

```kt
class Outer {
    private val name:String? = null
  // Inner class can access outer class's member
  // but It can not instanciate out side this
  inner class MyInner {
      fun show() {
          println("Outer name is $name")
      }
  }
}

var demo = Outer().MyInner().show()
```


### Anonymouse Inner class

anonymouse inner class는 object expression으로 생성한다.


```kt
window.addMouseListener(object: MouseAdapter() {
    override fun mouseClicked(e: MouseEvent) {
        // ...
    }
                                                                                                            
    override fun mouseEntered(e: MouseEvent) {
        // ...
    }
})
```


If the object is an instance of a functional Java interface (i.e. a Java interface with a single abstract method), you can create it using a lambda expression prefixed with the type of the interface:

```kt
val listener = ActionListener { println("clicked") }
```

