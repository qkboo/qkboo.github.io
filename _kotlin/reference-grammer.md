# Grammer


## Notation

This section informally explains the grammar notation used below.

### Symbols and naming


*터미널 심볼* 이름은 대문자로 시작한다, 예: **SimpleName**
*비 터미널 심볼* 이름은 소문자로 시작한다, 예: **kotlinFile**
각 *production*은 콜론 (:)으로 시작한다.
*심볼 정의*는 많은 productions와 세미콜론 (;)으로 끝나야 한다.
심볼 선언은 *애트리뷰트*를 붙여야 한다. 예) **start** 애트리뷰트는 시작 심볼을 의미한다.

### EBNF 표현식

연산자 `|`는 *대안*을 의미한다.
연산자 `*`는 *반복*을 의미한다( 0 또는 그 이상).
연산자 `+`는 *반복*을 의미한다( 0 또는 그 이상).
연산자 `?`는 *선택*을 의미한다( 0 또는 그 이상).
`alpha { beta }`는 비어있지 않은 beta로 구분된 알파의 리스트를 의미한다. 
연산자 '++'는 공백이 없거나 오퍼랜드 사이에 주석을 허용하는 것을 의미한다.


### Semicolons

코틀린은 "세미콜론 추론"을 제공한다: 문장 구성을 통해, 부문장(예. 구문, 선언 등)은 슈도-토큰 [SEMI](#SEMI), "세미콜론 혹은 줄바꿈"로 분리된다. 많은 경우에 코틀린 코드에 세미콜론은 필요 없다.


## Syntax

