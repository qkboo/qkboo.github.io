## Collections 와 String

https://kotlinlang.org/docs/reference/collections.html

### String

```kt
var title = "Welcome to Kotlin"
// String concatenate
var jane = " Jane Doe, " + title + " "
println( title + ", World famous Language" )

    /*
    Stirng is character array
     */
    // Index of strings
    println("title length: ${title.length}")
    print(title[0])
    print(title[1])
    print(title[2])
    print("\n")

    // Iteration
    println("===== stirng iteration =====")
    for (c in title)
        print(c)
    print("\n")

    println("===== stirng template =====")
    // String Template: '$LETERAL'
    println("$title, World famous language")

    // Stirng Template: ${STR[INDEX]}
    print("${title[0]}${title[1]}${title[2]}\n")

    // methods
    println(jane.toLowerCase())
    println(jane.toUpperCase())
    println(jane.split(","))
    println(jane.trim())
```


### Array

```kt
var arrayInt = Array<Int>(5){-1}
arrayInt[0] = 20
arrayInt[1] = 30

println("======= array index ========")
println(arrayInt[0])
println(arrayInt[1])

//
println("======= for ========")
for (a in arrayInt) {
    println(a)
}
println("======= for size ========")
//    for( i in 0..4)
for( i in 0..(arrayInt.size-1))
    println(i)
println("======= for with until ========")
for( i in 0 until arrayInt.size)
    println(i)

println("======= String array ========")
var arrayStr = Array<String>(4){""}
//    for ( i in 0..(arrayStr.size-1)) {
for ( i in 0 until arrayStr.size) {
    print("arrayStr[ $i ] = ")
    arrayStr[i] = readLine()!!
}
```

#### Array 객체

배열 객체는 현재 메모리 주소를 가지고 있다.

```kt
var arrayStr = Array<String>(4){""}
println(arrayStr)
```

arrayStr 객체를 출력하면,

```
[Ljava.lang.String;@724af044
```


### ArrayList

```kt
var arrayList = ArrayList<String>()
arrayList.add("Jan")
arrayList.add("Feb")
arrayList.add("Mar")
arrayList.add("Apr")

// index
println("===== ArrayList: index =====")
arrayList.get(0)

println("===== ArrayList: iterates all =====")
for ( item in arrayList) {
    println(item)
}

for (item in 0 until arrayList.size) {

}
println("===== ArrayList: search =====")
if( arrayList.contains("Jan") ) {
    println("jan is alive")
}
```


### HashMap

Array, List 같이 Index 로 데이터를 저장하는 방식에 비해 키로 데이터를 축적하고 찾을 수 있는 자료 구조를 Hash Map이라고 한다.

| 순서 | 내용 |
| ------------- | ------------------------------------ |
|  | |


```kt
var bookMap = HashMap<Int, String>()
bookMap.put(1, "Kotlin Language Principal")
bookMap[2] = "Basic OpenCV"
bookMap[3] = "OpenGL ES 4"
bookMap[24] = "Google Firebase"

println("====== HashMap: indexing ====== ")
println(bookMap.get(1))
println(bookMap[2])
println(bookMap[3])
println(bookMap[24])

println("====== HashMap: keys ====== ")
for( k in bookMap.keys) {
    //println(bookMap.get(k))
    println(bookMap[k])
}
```

### Mutable, Immutable

Array, HashMap은 가변형 데이터이다.
List, Set는 불변형 데이터이다.

#### HashMap

HashMap 클래스외에 hashMapOf()

```kt
var map1 = hashMapOf( 1 to "", 2 to "")
var map2 = hashMapOf<String,  String>("하나" to "Kotlin")

map2["셋"] = "메이저리그"
//
map2["하나"] = "Android Java"

for(k in map2.keys) {
    println(map2[k])
}

```

#### List

불변형 리스트는 `listOf()` 메서드를 사용한다.

```kt
var list1 = listOf(1,2,4,6,9)
println(list1[1])
// cant assign new value
// list1[1] = 30
```

가변형 리스트는 `mutalbeListOf()`

```kt    
var list2 = mutableListOf(1,2,4,6,9)
list2[1] = 20
```


    val fruits = listOf("banana", "avocado", "apple", "kiwi")
    fruits
            .filter { it.startsWith("a")}
            .sortedBy { it }
            .map { it.toUpperCase() }
            .forEach { println(it) }

#### Set

setOf()

mutableSetOf()
