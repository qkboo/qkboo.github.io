## Thread

Java 표준라이브러리 Thread 클래스를 사용할 수 있다

### Thread class

http://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html


```kt
class FirstThread():Thread() {

    override fun run() {
        var count = 0
        while( count < 10) {
            println("${this.name} Count $count")
            count++

            try {
                Thread.sleep(1000)
            } catch (ex:Exception) {
                println(ex.message)
            }
        }
    }
}
```

스레드 이름을 초기화해서 시작할 수 있다.

```kt
class FirstThread:Thread { 
  constructor(name:String):super() {}
}
```


### thread()

thread는 코틀린 표준라이브러리인  kotlin.concurrent / [thread](https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.concurrent/thread.html) 를 사용해서 새 jvm thread를 얻을 수 있다.
