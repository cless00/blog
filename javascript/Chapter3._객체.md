#javascript
# Chapter 3. 객체

* 단순한 데이터 타입: 숫자, 문자열, 불리언 (true/false), `null`, `undefined`
  - 그 외에는 모두 객체 (Object): 배열, 함수, 정규식 등 모든 객체들
  - 숫자, 문자열, 불리언: 메소드가 존재하기 때문에 유사 객체, 하지만 값 변경 불가 (immutable)
* 객체: 변경 가능한 속성들의 집합
  - 속성의 이름은 문자열이면 모두 가능, 빈 문자열도 포함
  - 속성의 값은 `undefined`를 제외한 자바스크립트의 모든 값이 사용될 수 있음
* 자바스크립트의 객체는 클래스가 필요 없음 (class-free)
* 객체 하나에 있는 속성들을 다른 객체에 상속하게 해주는 프로토타입 (`prototype`) 연결 특성이 존재함
  - 이를 활용하여 객체를 초기화 하는 시간과 메모리 사용을 줄일 수 있음

## 1. 객체 리터럴
* 새로운 객체를 생성할 때 매우 편리한 표기법을 제공
  - 아무것도 없거나, 하나 이상의 이름/값 쌍들을 둘러싸는 중괄호로 표현

```js
var empty_object = {};
var stooge = {
  "first-name": "Jerome",
  "last-name": "Howard"
};
```

* 속성명은 어떤 문자열도 가능, 공백 포함
  - 속성이름에 사용한 `""`는 자바스크립트에서 사용할 수 있는 유효한 이름이고 예약어가 아닐 경우 생략 가능
  - `,`를 사용하여 `"속성이름":"값"` 쌍들을 구분을 위해 사용
  - 속성 값으로 객체 리터럴 가능

```js
var flight = {
  airline: "Oceanic",
  number: 815,
  departure: {
    IATA: "SYD",
    time: "2004-09-22 14:55",
    city: "Sydney"
  },
  arrival: {
    IATA: "LAX",
    time: "2004-09-23 10:42",
    city: "Los Angeless"
  }
};
```

## 2. 속성값 읽기
* 두가지 방법
  - `[]` 이용
  - `.` 이용: 유효한 이름이고 예약어가 아닐 경우 가능

  ```js
  stooge["first-name"]  // "Joe"
  flight.departure.IATA // "SYD"
  ```

* 존재하지 않는 속성을 읽으려고 하면 `undefined` 반환
  - `||` 연산자를 사용하여 기본값 지정 가능

```js
var middle = stooge["middle-name"] || "(none)";
var status = flight.status || "unknown";
```

* 존재하지 않는 속성(`undefined`)의 속성을 참조할 경우 `TypeError` 예외 발생
  - 방지를 위해 `&&` 연산자 사용

```js
flight.equipment                            // undefined
flight.equipment.model                      // throw "TypeError"
flight.equipment && flight.equipment.model  // undefined
```

## 3. 속성값 갱신
* 속성 이름이 이미 객체 안에 존재하면 속성의 값만 교체
* 존재하지 않을 경우 해당 속성을 객체에 추가

```js
stooge['first-name'] = 'Jerome';  // replace
stooge['middle-name'] = 'Lester'; // newly add
```

## 4. 참조
* 객체는 참조 방식으로 전달됨, 복사되지 않음

```js
var x = stooge;
x.nickname = 'Curly';
var nick = stooge.nickname;
    // x와 stooge가 모두 같은 객체를 참조하기 때문에,
    // 변수 nick의 값은 'Curly'
var a = {}, b = {}, c = {};
    // a, b, c는 각각 다른 빈 객체를 참조
a = b = c = {};
    // a, b, c는 모두 같은 빈 객체를 참조
```

## 5. 프로토타입 (`prototype`)
* 객체 리터럴로 생성되는 모든 객체는 자바스크립트의 표준 객체인 `Object`의 속성인 `prototype`(Object.prototype) 객체에 연결 됨
* 객체를 생성할 때 프로토타입이 될 객체를 선택할 수 있음
* `Object`객체에 `create`메소드 추가
  - 넘겨 받은 객체를 프로토타입으로 하는 새로운 객체를 생성하는 메소드

```js
if (typeof Object.create !== 'function') {
  Object.create = function (o) {
    var F = function () {};
    F.prototype = o;
    return new F();
  };
}
var another_stooge = Object.create(stooge);
```

* 객체를 변경하더라고 객체의 프로토타입에는 영향을 미치지 않음
* 프로토타입의 연결은 객체의 속성을 읽을 때만 사용
  - 객체의 특정 속정의 값을 읽으려 할때 해당 속성이 객체에 없는 경우 프로토타입에서 찾으려고 함
  - 이러한 시도는 프로토타입 체인(`prototype chain`)의 가장 마지막에 있는 `Object.prototype`까지 계속해서 이어짐
  - 프로토타입 체인 내에 어디에도 없을 경우 `undefined` 반환
  - 이러한 내부 동작을 위임(`delegation`)이라고 함

## 6. 리플렉션 (reflection)
* `typeof` 연산자를 통해 속성의 타입을 살펴보는데 유용
  - 이를 통해 속성의 존재 여부를 확인할 수 있음

  ```js
  typeof flight.number    // 'number'
  typeof flight.status    // 'status'
  typeof flight.arrival   // 'object'
  typeof flight.manifest  // 'undefined'
  ```

  - 때때로 객체의 속성이 아니라 프로토타입 체인 상에 있는 속성을 반환할 수 있음, 주의 필요

  ```js
  typeof flight.toString      // 'function'
  typeof flight.constructor   // 'function'
  ```

* 리플렉션 시 원하는 않는 속성을 배제하기 위한 두가지 방법이 있음
  - 함수값을 배제하는 방법
  - `hasOwnProperty` 메소드를 이용한 객체의 특정 속성의 존재 여부를 확인 하는 방법 (해당 메소드는 프로토타입 체인을 바라보지 않음)

  ```js
  flight.hasOwnProperty('number')       // true
  flight.hasOwnProperty('constructor')  // false
  ```

## 7. 열거 (Enumeration)
* `for in`구문을 사용하여 객체에 있는 모든 속성의 이름을 열거 가능
  - 함수 나 프로토타입에 있는 속성 등 모든 속성이 포함되기 때문에 원하지 않는 것들을 걸러내야 함
  - `hasOwnProperty` 메소드와 함수 배제를 위한 `typeof` 사용

```js
var name;
for (name in another_stooge) {
  if (typeof another_stooge[name] !== 'function') {
    document.writeln(name + ': ' + another_stooge[name]);
  }
}
```

* `for in` 구문 사용시 속성들이 이름순으로 나온다는 보장이 없음
  - 특정 순으로 열거되기를 원할 경우, 열거 순서를 배열로 지정하고 이를 이용하여 순서대로 열거 가능

```js
var i;
var properties = [
  'first-name',
  'middle-name',
  'last-name',
  'profession'
];
for (i = 0; i < properties.length; i += 1) {
  document.writeln(properties[i] + ': ' + another_stooge[properties[i]]);
}
```

  - 프로토타입 체인에 있는 속성들이 나오지 않을까 염려할 필요 없고, 원하는 순서대로 리플렉션 가능

## 8. 삭제
* `delete` 연산자를 사용하여 객체의 속성을 삭제 가능
  - 프로토타입 체인 상에 있는 객체들은 접근하지 않음

```js
another_stooge.nickname   // 'Moe'
  // another_stooge에서 nickname을 제거하면
  // 프로토타입에 있는 nickname이 나타남
delete another_stooge.nickname;
another_stooge.nickname   // 'Curly'
```

## 9. 최소한의 전역변수 사용
* 전역변수는 프로그램의 유연성을 약화시키기 때문에 가능하면 피하는것이 좋음
* 전역변수 사용을 최소화 하기 위한 방법
  - 전역변수 사용을 위한 전역변수를 하나 만드는 방법이 있음

  ```js
  var MYAPP = {};
  ```

  - 이 변수를 다른 전역변수를 위한 컨테이너로 사용

  ```js
  MYAPP.stooge = {
    "first-name": "Joe",
    "last-name": "Howard"
  };
  MYAPP.flight = {
    airline: "Oceanic",
    number: 815,
    departure: {
      IATA: "SYD",
      time: "2004-09-22 14:55",
      city: "Sydney"
    },
    arrival: {
      IATA: "LAX",
      time: "2004-09-23 10:42",
      city: "Los Angeless"
    }
  };
  ```

  - 이런 방법을 통해 어플리케이션이나 위젯, 라이브러리들과 연동할 때 발생하는 문제점을 최소화 가능
  - 또한 `MYAPP.stooge`가 명시적으로 전역변수라는 것을 나타내기 때문에 프로그램의 가동성이 향상됨
  - `closure`를 사용하여 전역변수 사용을 줄일 수도 있음 (4장에서 설명)


