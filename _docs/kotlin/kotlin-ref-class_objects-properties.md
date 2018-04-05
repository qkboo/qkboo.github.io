## Properties 와 Field

속성을 사용할 때 속성 이름을 참조하는데 Java에서는 필드라고 한다.


### Getter 와 Setter

속성을 선언할 때 문법은 

```kt
var <propertyName>[: <PropertyType>] [= <property_initializer>]
    [<getter>]
    [<setter>]
```

초기 값과 게터, 세터는 선택적이며 또한 속성자료형도 초기값에서 추론이 가능하면 선택적이다 (혹은 게터의 반환 자료형).

예를 들어:

```kt
var allByDefault: Int? //error: 기본 게터 세터를 위한 명확한 초기화기 필요
var initialized = 1     // Int 이고, 기본 게터 세터가 있다..
```

읽기 전용 속성은 var가 아닌 `val` 로 선언하고 세터가 지원 안된다.

```kt
val simple: Int?      // Int형이고, 기본 게터 그리고, 생성자에서 초기화 되야 한다.
val inferredType = 1  // 
```

사용자 정의 게터 접근자을 쓸 수 있다, 보통 함수 같이 속성 안의 오른쪽에 위치한다.

```kt
val isEmpty: Boolean
  get() = this.size == 0
```

사용자 정의 세터는

```kt
var stringRepresentation: String
  get() = this.toString()
  set(value) {
    setDataFromString(value)
  }
```
 - 관례적으로 세터 인자의 이름은 `value` 이지만 다른 이름을 선택할 수 있다.

코틀린 1.1 이후 속성의 자료형은 게터에서 자료형을 추론할 수 있으면 생략 가능하다.

```kt
val isEmpty get() = this.size() == 0  // Boolean으로 추론 가능
```

만약 접근자의 가시성을 변경해야 거나, 어노테이션을 해야할 거면, 기본 구현을 변경할 필요는 없고, 접근을 본문을 선언할 필요가 없다.

```kt
var setterVisibility: String = "Abc"
  private set         // 세터는 포라이비트이고 기본 구현을 갖는다.

var setterWithAnnotation: Any? = null
  @Inject set         // 세터를 @Inject 어노테이션
```







## 참조
 - https://kotlinlang.org/docs/reference/properties.html