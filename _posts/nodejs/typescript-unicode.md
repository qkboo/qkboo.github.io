## Unicode Symbols


### ECMAScript 6 Escape Sequence

ES5는 유니코드 3.0 기반이고, ES6는 Unicode version 5.1.0을 기반으로 한다. ES6에서 유니코드를 사용하는 방법은 세가지가 있다:

 - Unicode escapes for code points beyond 16 bits: \u{···}
Can be used in identifiers, string literals, template literals and regular expression literals. They are explained in the next section.

 - 문자열:
    - Iteration honors Unicode code points.
    - Read code point values via `String.prototype.codePointAt()`.
    - Create a string from code point values via `String.fromCodePoint()`.

 - Regular expressions:
    - New flag `/u` (plus boolean property unicode) improves handling of surrogate pairs.

#### 사용할 곳

회피문자는 아래 위치에 사용할 수 있다.

|              위치             | \uHHHH |     \u{···}    |  \xHH |
| ---------------------------- | ----- | ---------------  | ----- |
| Identifiers                  | ✔     |         ✔        |       |  
| String literals              |  ✔    |         ✔        |   ✔   |
| Template literals            |  ✔    |         ✔        |   ✔   |
| Regular expression literals  | ✔     | Only with flag /u|   ✔   |

##### 식별자에 사용 (Identifiers)

식별자에 
 - 4자리수 유니코드 회피문자 `\HHHH`는 하나의 코드 포인트가 된다.
 - 한 유니코드 코드 포인트 회피 `\u{...}`는 하나의 코드 포인트가 된다.

아래 코드의 식별자 `hello` 의 o를 코드 포인트로 대체해도 무방.

```js
> const hello = 123;
> hell\u{6F}
123
```


##### 문자열 리터럴

문자열 리터럴에도 

- 문자열은 UTF-16 code units 으로 내부에 저장한다.
- `\xHH` 헥사 회피식은 UTF-16 code unit를 기대한다.
- 4자리 유니코드 회피식 `\uHHHH` UTF-16 code unit를 기대한다.
- 유니코드 코드 포인트 `\u{···}` 회피식은 해당 코드 포인트의 UTF-16 인코딩을 기대한다(하나 혹은 두개 UTF-16 코드 유니트)


##### 템플릿 리터럴

- 템플릿 리터럴 안에 회피문자는 스트링 리터럴 같이 처리된다.
- 태그한 템플릿에서 태그 함수에 의존해서 해석된다.
  - Cooked: escape sequences are handled like in string literals.
  - Raw: escape sequences are handled as a sequence of characters.

```ts
> `hell\u{6F}` // cooked
'hello'
> String.raw`hell\u{6F}` // raw
'hell\\u{6F}'
```

##### 정규 표현식

유니코드 코드 포인트 회피식은 `/u` 가 지정되야 한다.

```ts
> /^\u{3}$/.test('uuu')
  true
```



#### escape sequence

ES6는 회피문자를 Hex, Unicde 그리고 Unicode Code point 방식을 사용할 수 있다.

##### Hex escape (exactly two hexadecimal digits): `\xHH`

```js
> '\x7A' === 'z'
  true
```

##### Unicode escape (exactly four hexadecimal digits): `\uHHHH`

```js
> '\u007A' === 'z'
true
```

##### Unicode code point escape (1 or more hexadecimal digits): `\u{···}`

```js
> '\u{7A}' === 'z'
  true
```


#### Code point

ES6에서 스트링의 새로운 회피문자가 도입됐다. Unicode code point escapes를 사용해 16비트 이상까지 사용이 가능하다. 

ES5에서는 두 개의 UTF-16 코드로 코드포인트를 합성해야 했다. 예를 들어 U+1D306 인 (𝌆) has code point는 ES6에서 코드포인트 `\u{1D306}` 로 사용하는데, ES5로 대체 쌍 두 개로 인코딩해 보면 `'\uD834\uDF06'` 같이 인코딩한다.

이 코드 포인트는  `\u{  }` 로 감싸서 회피할 수 있다. 이것은 코드 포인트 `0x10ffff` 까지 가능하다.

#### 정규식

정규표현식을 상요해 유니코드 코드 포인트를 선얺ㄹ 수 있다.

```js
\\u\{([0-9a-fA-F]{1,})\}
```


#### 문자열

코드 포인트를 사용하는 
 - `String.fromCodePoint`
 - `String#codePointAt`


### Unicode Symbols

http://www.unicode.org/versions/Unicode6.2.0/ch15.pdf

| --------------------------------- | -------------------------------- |
| Currency symbols                  | Geometrical symbols |
| Letterlike symbols                | Miscellaneous symbols and dingbats |
| Mathematical alphabets            | Emoticons |
| Numerals                          | |
| Mathematical symbols 
| Superscript and subscript symbols | |

Invisible mathematical operators Technical symbols


Enclosed and square symbols
Braille patterns
Western and Byzantine musical symbols Ancient Greek musical notation

#### 15.1 Currency Symbols

통화심볼은 U+20A0–U+20CF 범위에 있다.


## 참고
 - http://exploringjs.com/es6/ch_unicode.html
 - https://mathiasbynens.be/notes/javascript-escapes