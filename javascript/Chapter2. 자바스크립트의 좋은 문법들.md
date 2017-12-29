# Chapter 2. 자바스크립트의 좋은 문법들

## 1. 공백(Whitespace)
* 보통 공백은 중요하지 않으나 아래의 var와 that 사이에는 공백 필요하며, 다른 빈칸은 제거해도 상관 없음

```js
var that = this;
```

* 주석사용
  - __블록 주석(/\* \*/)__ 대신 __한줄 주석(//)__ 사용을 권장
  - 아래와 같은 __정규 표현식 사용시에 구문 오류가 발생할 가능성이 있음__

```js
/*
      var rm_a a /a*/.match(5);
*/
```

## 2. 이름(Names)
* 하나 이상의 문자, 숫자, \_가 붙은 문자열로 문장, 변수, 매개변수, 속성명, 연산자, 라벨 등에 사용 가능
* 아래의 경우(예약어)는  이름으로 __사용 불가__
  - abstract
  - boolean break byte
  - case catch char class const continue
  - debugger default delete do double
  - else enum export extends
  - false final finally float for function
  - goto
  - if implements import in instanceof int interface
  - long
  - native new null
  - package private protected public
  - return- short static super switch synchronized
  - this throw throws transient true try typeof
  - var volatile void
  - while with
* 또한, 예약어는 객채 리터럴의 속성명이나 객체 속성을 나타낼 때 사용하는 마침표(.) 다음에 사용할 수 없음

## 3. 숫자(Numbers)
* 자바스크립트는 단 하나의 숫자형만 사용
* 내부적으로 64비트 부동소수점 형식, __자바의 double 형과 동일__
* 지수형 표현 가능(e.g. `100 = 1e2`)
* NaN (Not a Number)
  - 수치연산 후 정상적인 값을 얻지 못할 때의 값
  - NaN은 그 자신을 포함하여 어떤 값과도 같지 않기 때문에, __NaN 여부를 확인하기 위해서는 isNaN() 함수 사용__
* 1.79769313486231570e+308 보다 큰 수는 Infinity로 나타냄
* 수치 연산을 위해 Math 객체 사용(e.g. `Math.floor(number)`, 버림)

## 4. 문자열(Strings)
* 작은 따옴표(')나 큰 따옴표(")로 묶어서 나타냄 (둘의 차이는 없음)
* 문자타입은 없음, 문자 하나만 포함하는 문자열 사용
* `\(백슬래시)`는 이스케이프 문자임. 아래와 같은 특수문자는 이스케이프 문자와 함께 사용해야 함

문자 | 해석
---|---
\" | 큰 따옴표
\' | 작은 따옴표
\\\ | 백슬래시
\/ | 슬래시
\b | 백스페이스
\f | 폼피드
\n | LF
\r | CR
\t | 탭
\u + 4자리 16진수 | 유니코드 문자

* 유니코드 문자(참고, `===`는 타입과 값이 같음을 의미)

```js
"A" === "\u0041"
```

* `length`라는 속성 존재 (e.g. `"seven".length`의 값은 5)
* 문자열은 변하지 않음(immutable)
* `+`로 새로운 문자열을 만들 수 있음
* 분리된 문자열이 `+`로 연결 되든 그냥 문자열 하나든 간에, 문자열의 순서가 같으면 같은 문자열임

```js
'c' + 'a' + 't' === 'cat'
```

* 문자열 메소드 존재

```js
'cat'.toUpperCase() === 'CAT'
```

## 5. 문장(Statements)
* `var`문
  - 웹브라우저에서 `<script>` 태그는 컴파일되어 즉시 수행되는 하나의 컴파일 단위
  - 링커(linker)가 없기 때문에 자바스크립트의 모든 구문을 공통적인 전역 이름 공간(namespace)에 저장
  - `var`문은 함수 내부에서 사용시에 함수의 private 변수를 의미
  - 참고: `var` 없이 변수 선언시에는 전역변수로 생성됨
  - `linker`란: 프로그램을 실행하기 위해 필요한 라이브러리 등을 연결하기 위해 사용하는 프로그램. `compiler``assembler`와 함께 사용됨
* 라벨
  - 표현식 문장, 벗어나는 문장, `try`문, `if`문, `switch`문, `while`문, `for`문, `do`문에서 사용 가능
  - 벗어나는 문장에서 선택적으로 지정 가능 (e.g. `break`, `return`, `throw`문)

```js
var i, j;
loop1:
for (i = 0; i < 3; i++) {      //The first for statement is labeled "loop1"
   loop2:
   for (j = 0; j < 3; j++) {   //The second for statement is labeled "loop2"
      if (i === 1 && j === 1) {
         break loop1;
      }
      console.log("i = " + i + ", j = " + j);
   }
}
```

* 블록(`{}`)
  - 중괄호로 둘러싸인 문장의 집합
  - 다른 언어와 달리 블록은 새로운 유효범위(scope)를 생성하지 않음
  - 따라서 변수는 블록 안에서가 아니라 함수의 첫 부분에서 정의해야 함
* `if`문
  - 표현식의 값에 따라 프로그램의 흐름을 변경
  - 거짓에 해당하는 값들
    + false
    + null
    + undefined
    + 빈 문자열 ''
    + 숫자 0
    + NaN
  - 이외의 모든 값은 참(true)
* `switch`, `case`문
  - 다중 분기 수행
  - 표현식과 `case`절의 표현식이 같은지 비교하여 일치하는 `case`절을 수행
  - `case`절에 실행 흐름을 벗어나는 문장(e.g. `break`)을 사용할 수 있음
* `while`문
  - 표현식이 참인 동안 블록을 반복해서 실행
* `for`문
  - 일반적인 형식: `초기화`, `조건`, `증가`의 세가지 절로 제어하는 구조
  - 객체의 속성 이름(또는 키)을 열거하는 형식 (`for in`형식)
  - `for in`형식을 사용할 시에는 아래와 같이 `hasOwnProperty()` 메소드로 속성 이름이 실제로 객체의 속성인지 아니면 프로토타입 체인(prototype chain) 상에 있는 것인지 확인 필요함

```js
for (myvar in obj){
    if(obj.hasOwnProperty(myvar)){
        // 구문
    }
}
```

* `try`, `catch`문
  - 블록을 실행하면서 블록 내에서 발생하는 예외 상황을 포착
  - `catch`절에 예외 객체를 받는 새로운 변수를 정의
* `throw`문
  - 예외를 발생
  - `try` 블록 안에 있으면 `catch`절로 이동
  - 함수 내에 있을 경우 함수 실행을 중단하고 호출한 `try`문의 `catch`절로 이동
*  `return`문
  - 함수에서 호출한 곳으로 되돌아 가는 역할 수행
  - 반환값을 지정가능, 미지정시 `undefined`를 반환함
  - `return`문과 표현식 사이에 줄바꿈 허용하지 않음
* `break`문
  - 반복문이나 `switch`문에서 흐름을 벗어나게 하는 역할
  - 라벨이 주어지면 라벨이 붙은 문장의 끝으로 이동
  - `break`문과 라벨 사이에 줄바꿈 허용하지 않음
* 표현식
  - `=`: 값 할당
  - `+=`: 더하거나 연결
  - `delete`: 객체의 property를 삭제
  - `==`: 값 비교
  - `===`: 값과 타입 비교

## 6. 표현식(Expressions)
* 간단한 표현식 종류
  - 리터럴 값(문자열이나 숫자), 변수
  - 내장값들(`true`, `false`, `null`, `undefined`, `NaN`, `Infinity` 등)
  - `new` 키워드에 의한 호출 표현식, `delete` 키워드 다음에 나오는 세부지정 표현식
  - 괄호로 싸인 표현식, 전치 연산자 다음에 이어지는 표현식 등
* 그 외 기타 표현식
  - 이항 연산자의 표현식
  - `?` 삼항 연산자의 표현식
  - 호출
  - 세부지정(`.` 또는 `[]`)
* 연산자 우선순위

연산자 | 설명
--- | ---
. [] () | 세부지정이나 호출
delete new typeof + - ! | 단항 연산자
\* / % | 곱하기, 나누기, 나머지
\+ - | 더하기/연결, 빼기
\>= <= > < | 같지 않음 비교
\=== !== | 동등
&& | 논리적 and
ll | 논리적 or
?: | 삼항

* 전치 연산자
  - `typeof`: 피연산자의 타입
  - `+`: 숫자로 지정 (피연산자가 숫자가 아닐경우 숫자로 변환. 변환할 수 없으면 `NaN` 반환)
  - `-`: 부정
  - `!`: 논리적 not
* `typeof`연산자의 결과값에는 `number`, `string`, `boolean`, `undefined`, `function`, `object` 등이 있음
  - 피연산자가 배열이나 `null`이면 결과는 모두 `object`이나, 이는 약간 문제가 있음(6장에서 상세 설명)
* 이항 연산자
  - `+`연산자: 더하기 연산을 위해서는 피연산자가 모두 숫자여야 함
  - `/`연산자: 피연산자 두개가 모두 정수형이어도 결과가 실수일 수 있음
* 호출
  - 함수 이름 뒤에 이어지는 한쌍의 괄호
  - 괄호 내에 함수에 전달하는 인수를 포함
* 세부지정
  - `.이름`, `[표현식]`

## 7. 리터럴(Literals)
* 종류
  - 숫자 리터럴
  - 문자열 리터럴
  - 객체 리터럴
  - 배열 리터럴
  - 함수
  - 정규 표현식 리터럴
* 객체 리터럴
  - `{이름:표현식}` or `{문자열:표현식}`
* 배열 리터럴
  - `[표현식,...]` (6장에서 상세 설명)
* 정규 표현식
  - `/정규 표현식 선택/ g i m` (7장에서 상세 설명)

## 8. 함수(Functions)
* 함수값을 정의
* 이름, 매개변수 목록, 몸체를 가질 수 있음
  - `function 이름 (매개변수) {몸체}`

---
[돌아가기 - README.md](README.md)
