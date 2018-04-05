## Generic

자바 같이 코틀린도 형 매개변수 `T` 를 가지고 있다.

```kt
class UserInfo<T>(info:T) {
    init {
        println(info)
    }
}
```

보통 제네릭 클래스 인스턴스는 형 인자를 주어야 한다.

```kt
var user1 = UserInfo<String>("James")
```

그런데 매개변수가 생성자 인자 혹은 다른 이유로 추론이 되면 형 인자를 무시할 수 있다.

```kt
var user1 = UserInfo("James")
```

그렇지만 형 인자를 사용했는데 다른 형으로 생성자 인자를 넘기면 당연히 에러가 난다.

```kt
var user1 = UserInfo<String>(1234)    // 에러
```


### Generic function

제너릭 함수는 함수가 받는 형을 지정하고 함수 이름과 매개변수를 선언한다.

```kt
fun <T> display(info:T) {
  println(info)
}
```


```kt
display<Int>(200)
display<String>("Hahah")
```



https://kotlinlang.org/docs/reference/generics.html