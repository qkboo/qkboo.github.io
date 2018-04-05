# Packages

소스파일은 패키지 선언으로 시작해야 한다.

``` kotlin
package foo.bar

fun baz() {}

class Goo {}

// ...
```

소스 파일의 내용 ( 클래스, 함수 들)은 패키지 선언에 포함된다. 그러므로 위 예에서 `baz()`의 완전한 이름은 `foo.bar.baz` 이고, `Goo`는 `foo.bar.Goo` 이다. 

패키지가 명시되지 않으면 파일의 내용은 이름이 없는 "default" 패키지에 속한다. 


## Imports

기본 임포트에 별개로 각 파일은 자신만의 임포트 지시자를 포함할 수 있다.
Apart from the default imports, each file may contain its own import directives.
임프트는 [grammar](grammar.html#import)에 설명되어 있다.

단일 이름을 임포트 할 수 있다. 예)

``` kotlin
import foo.Bar // Bar는 전체 이름 없이 접근할 수 있다
```

또는 범위 (package, class, object 등) 를 포괄하는 접근자 `*`를 사용 할 수 있다.

``` kotlin
import foo.* // `foo` 안의 모든 것을 접근할 수 있다
```

이름이 충돌하면 애매함을 제거하기 위해서 *as* 키워드를 사용해서 지역 내에서 충돌 요소의 이름을 바꾼다:

``` kotlin
import foo.Bar // Bar에 접근할 수 있다
import bar.Bar as bBar // bBar로 'bar.Bar'을 대신한다
```

`import` 키워드는 클래스들을 가져오는데 국한되지 않는다; 다른 선언을 가져오는데 사용할 수 잇다:
 * 최상위 수준 함수들과 속성들;
 * [객체 선언](object-declarations.html#object-declarations)안에 선언되 함수들과 속성들;
 * [enum 상수들](enum-classes.html)

자바와 달리, 코틀린은 "import static" 문법을 분리하지 않는다; 이런 선언은 모두 일반적인 `import` 키워드를 사용해서 가져오게 된다.


## 최상위-수준 선언의 가시성 Visibility of Top-level Declarations

만약 최상위-수준 선언이 *private* 마크되면, 이것은 선언되어 있는 그 파일 안의 사적인 선언이다.  [Visibility Modifiers](visibility-modifiers.html)).
)).