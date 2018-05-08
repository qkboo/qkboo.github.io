## Delegated Properties

https://kotlinlang.org/docs/reference/delegated-properties.html#lazy

### 표준 위임

코틀린 표준 라이브러리는 위임을 제공하는 여러 factory 메서드를 제공한다.

#### Lazy

`lazy()` 는 함수로 람브다이고 lazy 속서을 구현하기 위한 위임을 제공해서 `Lazy<T>` 인스턴스를 반환한다. 이것은 `get()` 실행하면 람브다가 lazy() 로 전달하고 결과를 기억했다, 이어지는 get() 호출에도 이 결과를 사용한다.

```kt
val lazyValue: String by lazy {
  println("computed")
  "Hello"
}
```


