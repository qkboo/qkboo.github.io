## Functions 와 Lambda

function은 소스 파일에 `fun` 키워드로 서언할 수 있다.

> 타 언어의 class 내부 선언과 차이


### Parameters


#### Default arguments


#### Named arguments


### Function scope

코틀린 fun 키워드로 함수를 선언할 수 있으므로 fun 내부 혹은 클래스 내부에 새 fun을 선언할 수 있다.

#### Local function

fun 키워드로 함수를 선언할 수 있으므로 fun 내부에 새 fun을 선언할 수 있다.

```kt
fun dfs(graph: Graph) {
    fun dfs(current: Vertex, visited: Set<Vertex>) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
            dfs(v, visited)
    }

    dfs(graph.vertices[0], HashSet())
}
```

로컬 함수는 부모 함수의 변수에 접근할 수 있다 - 클로저. 위 코드에서 visited 를 로컬 변수로 선언해 아래 같이 사용할 수 있다:

```kt
fun dfs(graph: Graph) {
   val visited = HashSet<Vertex>()
    fun dfs(current: Vertex) {
        if (!visited.add(current)) return
        for (v in current.neighbors)
            dfs(v, visited)
    }

    dfs(graph.vertices[0], HashSet())
}
```

#### Member function

클래스 혹은 개체 내부에 선언되어 있는 함수를 Member function 이라고 한다.

```kt
class Sample() {
    fun foo() { print("Foo") }
}
```

member function은 `.` 로 접근할 수 있다.

```kt
Sample.foo()
```

위 코드는 Sample 클래스 인스턴스를 생성하고 내부 함수 foo()를 호출하게 된다.

<br>
### Polymorphism

```kt
fun sum(a:Int, b:Int):Int {
    return a + b
}

fun sum(a:Int, b:Int, c:Int ):Int {
    return a + b
}

fun sum():Int {
    return -1
}
```


```
    display( listOf("하나","둘"))
    display( "삼십")
    display( 100 )
```

```kt
fun display( args: List<String>) {

}
fun display( args: String) {

}

fun display( args: Int) {

}
```


## 참조
  - https://kotlinlang.org/docs/reference/functions.html