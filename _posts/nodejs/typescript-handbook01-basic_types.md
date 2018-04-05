---
title: Typescript / Basic-types
date: 2017-07-12 12:00:00 +0900
layout: post
tags: [nodejs, TypeScript Tutorials, TypeScript]
categories: 
 - Programming
---

## Basic Types

https://www.typescriptlang.org/docs/handbook/basic-types.html

자바스크립과 비슷하게 데이터는 numbers, strings, structures, boolean 

### 변수선언 `let` 과 `var`

`let` 지시 키워드는 지역 변수를 선언시 사용한다.
`var` 지시 키워드는 

### Boolean

Javascriptㅗ가 Typescript에서 **boolean** 식별자에 *true*, *false* 값을 갖는다.

```ts
let isDone: boolean = false;
```


```typescript
let isDone: boolean = false;
isDone
```


    false



### Number

Javascript 같이 TypeScript도 모든 숫자는 실수이다. 이런 숫자를 지시하는 데이터 형이 **number** 이다. 또한 ECMAScript 2015 (이하 ES2015)에서는 10진수와 16진수를 지원하는 리터럴을 지원한다.

```ts
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
```


```typescript
let num = 100.1;
let decimal: number = 10;
console.log( num, decimal);
```

    100.1 10





    undefined



### String

문자형 데이터 형식으로 **string**을 사용한다. 문자 리터럴을 표현하기 위해서 ', " 사용해서 묶어준다.

```ts
let color: stirng = "파랑";
grep = '회색';
```

*template string*을 지원해서 여러줄에 걸치거나 내장한 표현식을 사용할 수 있다. 이런 문자열은 backtick/backquote 인 (**\`**) 를 사용해서 묶고, 표현식은 ```${ expr }``` 같이 내장하면 된다.

```ts
let color: stirng = "파랑";
let age: number = 37;
let sentence: string = `Hello, 컬러는 ${ color }이고,
내 나이는 ${ age + 5 } 살이야.`;
```

문자열 **+** 연산자를 이용해서 여러줄의 문장을 만들어도 된다.

```ts
let sentence: string = "Hello, 컬러는" + color + "\n\n"
 + "내 나이는 " + (age+5) + "살 이야.";
```


```typescript
let color: stirng = "파랑";
color = '하양';
grey = "회색";
let age: number = 37;
let sentence: string = `Hello, 컬러는 ${ color }이고,
내 나이는 ${ age + 5 } 살이야.`;

console.log(color, grey, sentence);
```

    하양 회색 Hello, 컬러는 하양이고,
    내 나이는 42 살이야.





    undefined



### Array

같은 형식의 데이터를 여러개 가지는 배열을 지원한다. 배열 형식은 두 가지 방법중 하나를 쓴다. 하나는 데이터 형식 다음에 **[]** 표현식 혹은 **Array<elemType>** 제너릭 형식을 사용한다.

```ts
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
```


```typescript
let sentenses: string[] = ["배열 데이터 형식을 지원한다", "[] 배열식을 사용하거나", "Array 제너릭을 사용한다."];
console.log(sentenses);
```

    [ '배열 데이터 형식을 지원한다', '[] 배열식을 사용하거나', 'Array 제너릭을 사용한다.' ]





### Tuple

tuple은 요소의 개수가 고정되며 여러 데이터 형식을 지정할 수 있는 배열로 표현할 수 있다.

```ts
let x: [string, number];
x = ["안녕", 10];
```

다음 같이 잘못된 초기화를 하면,

```ts
let x: [string, number];
x = [10, "안녕"]; // 잘못된 초기화
```

```sh
$ tsc tuple.ts
tuple.ts(4,1): error TS2322: Type '[number, string]' is not assignable to type '[string, number]'.
  Type 'number' is not assignable to type 'string'.
```

튜플 요소는 인덱스로 요소에 접근하고, 요소에 선언된 데이터형식을 가져온다.

```ts
x[0].substr(1); // OK
x[1].substr(1); // Error,
```

인덱스 범위를 넘어서 요소에 접근하면 유니온 형식이 사용된다. 튜플 x의 인덱스 3에 문자를 추가하면 **string** 혹은 **number** 가 대입된다.

```ts
x[3] = "반가와"; // OK
```



### Enum

자바스크립의 기본형과 같은 **enum** 은 C#같이 숫자형 값의 집합을 표현한다. enum의 멤버는 값을 명시하지 않으면 **0** 부터 시작한다. 

```ts
enum Color {Red, Green, Blue = 2};
let c: Color = Color.Green;// Red=0, Green=1
```

혹은 멤버의 값을 명시적으로 할 수 있다.

```ts
enum Saturation { Dark = 1.0, Medium = 0.5, Light = 0.1 };
```

enum 멤버를 string에 주면 해당 멤버가 값으로 전환된다.

```ts
let colorName: string = Color[2]; //'Blue'
```


```typescript
enum Color {Red, Green, Blue = 2};
let c: Color = Color.Green;// Red=0, Green=1
let b: Color = Color.Blue;

//
enum Saturation { Dark = 1.0, Medium = 0.5, Light = 0.1 };
let l: Saturation = Saturation.Light;

//
let colorName: string = Color[2];

console.log(c, b, l, colorName);
```

    1 2 0.1 'Blue'





    undefined



### Any

의미 그대로 어떤 값도 받을 수 있다. 받는 값의 타입이 모호할 경우 유용하다. 3자 라이브러리에서 데이터를 받는 다든지, 어떤 동적인 컨텐츠에서 받기도 유용하다. 형 점검을 기피하거나 컴파일시 체크하고자 할 때 유리하다.



```typescript
let notSure: any = 4;
notSure = "문자열도 입력";
notSure = false;
```



any 형은 opt-in, opt-out 체크에도 유리하다.


```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```




any는 데이터 일부만 형을 아는 경우에도 유리하다. 아래 같이 혼합된 배열도 가질 수 있다.


```typescript
let list: any[] = [1, true, "tree"]
list[1] = "hi"
list
```




    [ 1, 'hi', 'tree' ]



### Void

**void**는 any의 반대로 보인다. 보통 함수에서 반환 값이 없을 때 void를 사용한다.
그러나 변수로 사용하기에는 적절치 않다 - **undefined**, **null** 만 대입할 수 있다.


```typescript
function warnUser(): void {
    console.log("sdlsldfjsf");
}

//
let unusable: void = undefined;
```




    undefined



### Null 과 Undefined

Typescript에는 형 자기자신의 값을 갖는 **null**, **undefined**가 있다. 역시 void 만큼 유용하진 않다.

보통 다른 형의 아류형으로 number, string 등에 값으로 대입할 수 있다.

```ts
let u: undefined = undefined;
let n: null = null;
let num: number = null;
```

그러나 **--strictNullChecks** 플래그를 사용시에는 **void**에만 대입할 수 있다. 이것은 에러를 방지할 수 있는데, 이런경우 string or null or undefined에 주고자 할 때는 유니온 형식 string | null | undefined 을 사용하라.

유니온 형식은 다음에...


### Never

**never** 형은 절대 일어나지 않는 값의 형임을 표현한다. 예를 들어 **never**가 함수 혹은 화살표 함수 표현식의 반환형이면 예외를 던지거나 반환하지 않는다. 값도 any 형이 절대 true를 가지지 못하게 할 때 **never**를 채용한다.

**never**는 any가 대입 가능한 모든 형의 아류형으로 **never**에 대입 가능한 no 형이 아류형이다 (never 자신을 제외하고). 어떤 **any** 도 **never**에 대입 안된다.


```typescript
// never를 반환하는 함수는 끝 부분에 다다를 수 없다.
function error(message: string): never {
    throw new Error(message);
}

// 반환형을 never로 추론한다.
function fail() {
    return error("Something failed");
}

// never를 반환하는 함수는 끝 부분에 다다를 수 없다.
function infiniteLoop(): never {
    while (true) {
    }
}
```




    undefined



### Type Assertions

컴파일시 어떤 값은 특정 형임을 지정해 준다. 다른 컴퓨터 언어의 타입 캐스팅 같다. 다만 특별 점검 혹은 데이터 재구조화가 없다. 런타임 적용은 아니고 컴파일러에서 사용해서, 변수가 특별히 점검되었다고 가정한다.

타입 어서션은 두가지 형태가 존재한다. 하나는 꺽쇠괄호를 이용한다:

```ts
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

두번째는 **as** 문법을 이용한다

```ts
let strLength: number = (someValue as string).length;
```

둘 중 선호하는 방법으로 사용하면 된다. 다만 TypeScript을 JSX 와 함께 쓰게 되면 *as* 스타일만 가능하다.

### `typeof` 연산자

객체가 기본형식과 일치하는지 판단하는 연산자. [^1]

```ts
typeof variable === “string”;
typeof variable === “number”
```

### `instanceof` 연산자

https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Operators/instanceof


```typescript
var mySprite:Sprite = new Sprite(); 
trace(mySprite instanceof Sprite); // true 
trace(mySprite instanceof DisplayObject);// true 
trace(mySprite instanceof IEventDispatcher); // true
```

## 참조

[^1]: [How to get type of object in TypeScript](http://rostacik.net/2015/05/27/how-to-get-type-of-object-in-typescript-when-using-union-types-and-type-guards-for-multiple-types-of-function-parameter-or-variable/)
