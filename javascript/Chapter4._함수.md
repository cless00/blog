#javascript
# Chapter 4. 함수

* 모듈화의 근간
* 코드의 재사용이나 정보의 구성/은닉, 객체의 행위를 지정하는 등에 사용

## 1. 함수 객체

* 함수 = 객체
* 객체 리터럴로 생성되는 객체는 `Object.prototype`에 연결됨
* 함수 객체는 `Function.prototype`에 연결
* `Function`은 `Object.prototype`에 연결
* 함수는 `prototype.constructor`라는 속성이 있으며, 함수 자체를 값으로 가짐

## 2. 함수 리터럴

* 함수 객체는 함수 리터럴로 생성
  - 함수의 이름: 선택사항. 없을 경우 익명함수(`anonymous`)라 함
  - 함수 매개변수: 괄호내에 쉼표로 구분하여 열거. 호출시 넘겨진 인수로 초기화
  - 함수 바디: 중괄호. 호출시 실행

  ```js
  var add = function ( a, b ) {
    return a + b;
  }
  ```

* 함수 리터럴은 표현식이 나올 수 있는 곳 어디든 위치 가능
* 내부 함수는 자신의 변수 뿐만 아니라 자신을 포함하고 있는 함수의 변수에도 접근 가능
* 함수 리터럴로 생성한 함수 객체는 외부 문맥으로의 연결이 있음 (이를 `closure`라 함)

## 3. 호출

* 호출 시 현재 함수의 실행을 잠시 중단하고 제어를 매개변수와 함께 호출한 함수로 넘김
* 명시된 매개변수 외어 `this`와 `arguments`라는 추가적인 매개변수가 자동 할당됨
* 함수를 호출하는 네가지의 패턴이 있음
* 매개변수는 순차적으로 할당. 초과할 경우 무시, 모자랄 경우 `undefined` 할당

* 매소드 호출 패턴
  - 객체에 속성으로 저장된 함수를 호출하는 방법 (`.`이나 `[]`를 포함하고 있는 경우)
  - 객체에 저장된 함수를 메소드라 함
  - `this`에 메소드를 포함하고 있는 객체가 바인딩
  - 자신을 포함하는 객체의 속정을 접근하기 위해 `this`를 사용
  - 객체와 `this`의 바인딩은 호출시에 일어나며, 이러한 메소드를 public 메소드라 함

```js
var myObject = {
  value: 0,
  increment: function ( inc ) {
    this.value += typeof inc === 'number' ? inc : 1;
  }
};
myObject.increment( 2 ); // value = 2
```

* 함수 호출 패턴
  - 함수가 객체의 속성이 아닐 경우의 호출방법
  - `this`에 **전역객체** 가 바인딩 됨 (언어 설계단계에서의 실수)
  - 그렇기 때문에 내부함수를 호출 시에 호출한 외부함수에 접근할 수 없는 문제 발생
  - 문제 해결을 위해 (함수를 호출 하는) 메소드에 변수를 정의한 후 `this`를 할당하고 해당 변수를 통해 메소드에 접근

```js
myObject.double = function () { // method
  var that = this;
  var helper = function () { // function
    that.value = add( that.value, that.value );
  };
  helper();
};
myObject.double(); // value = 4
```

* 생성자 호출 패턴
  - 자바스크립트는 `prototype`에 의해 상속이 이루어지는 언어
  - 이는 (클래스가 아닌) 객체가 다른 객체로 상속이 가능하다는 의미
  - 함수를 `new`라는 전치 연산자와 함께 호출하는 방법
  - `new`와 함께 호출 시, 호출한 함수의 `prototype` 속성의 값에 연결되는 (숨겨진) 링크를 갖는 객체가 생성되고, 이 새로운 객체는 `this`에 바인딩됨
  - 생성자 호출 패턴을 사용하는 생성자 함수는 이니셜을 대문자로 표기
  - 생자 함수를 `new` 연산자 없이 호출할 경우 컴파일이나 실행시에 (`this` 변수의 값의 차이로 인한) 알 수 없는 결과를 초래할 수 있음
  - 권장하는 스타일은 아님

```js
var Quo = function (string) {
  this.status = string;
};
Quo.prototype.get_status = function () {
  return this.status;
};
var myQuo = new Quo( "confused" );
myQuo.get_status(); // confused
```

* apply 호출 패턴
  - 자바스크립트는 함수형 객체지향 언어이기 때문에 함수도 메소드를 가질 수 있음
  - `apply` 메소드는 함수를 호출 할때 사용할 인수들을 배열로 받아들이고, `this`의 값을 선택할 수 있도록 함
  - `apply` 메소드는 두개의 매개변수가 있음. 첫 번째는 `this`에 바인딩될 값, 두 번째는 매개변수들의 배열

```js
var array = [ 3, 4 ];
var sum = add.apply( null, array ); // sum = 7
var statusObject = {
  status: 'A-=OK'
};
var status = Quo.prototype.get_status.apply( statusObject ); // status = 'A-OK'
// statusObject라는 객체에 get_status라는 메소드가 없지만 apply 메소드를 통해
// statusObject를 this로 바인딩하게 하여 Quo의 get_sattus 메소드를 사용할 수 있음
```

## 4. 인수 배열 (aguments)

* 함수에 자동적으로 할당된 `argumens`라는 배열을 사용 가능
* 매개변수의 개수보다 더 많이 전달된 인수들도 모두 포함
* 전달된 인수의 개수에 따라 동적으로 동작하는 함수를 만들 수 있음

```js
var sum = function () {
  var i, sum = 0;
  for( i = 0; i < arguments.length; i++ ) {
    sum += arguments[i];
  }
  return sum;
};
sum( 4, 8, 15, 16, 23, 42 ); // 108
```

* `arguments`는 배열이 아니고, 배열 같은 객체
* 그렇기 때문에 배열이 가지는 메소드는 없음

## 5. 반환

* `return`문은 함수가 끝에 도달하기 전에 제어 반환 가능
* 함수는 항상 값을 반환함. 반환값을 지정하지 않을 경우 `undefined`가 반환됨
* `new` 전치 연산자와 함께 실행하고 반환값이 객체가 아닌경우, 반환값은 `this`(새로운 객체)가 됨 (`undefined`가 아님)

## 6. 예외

* 예외는 흐름을 방해하는 비정상적인 사고
* `throw`문은 함수의 실행을 중단하고 `name`과 `message` 속정을 가진 예외 객체를 반환해야 함

```js
var add = function ( a, b ) {
  if ( typeof a !== 'number' || typeof b !== 'number' ) {
    throw {
      name: 'TypeError',
      message: 'add needs numbers'
    };
  }
  return a + b;
};
```

* `throw`에서 반환한 예외 객체는 `try`문의 `catch` 절에 전달됨
* `try` 블럭 내에서 예외가 발생하면 `catch` 블록으로 이동함

```js
var try_it = function () {
  try {
    add( "seven" );
  } catch ( e ) {
    document.writeln( e.name + ': ' + e.message );
  }
};
try_it();
```

## 7. 기본 타입에 기능 추가

* 기본타입에 메소드 추가 가능
* 함수, 배열, 문자열, 숫자, 정규표현식, 불리언 등에 유효
* 아래와 같이 `Function.prototype`에 메소드를 추가하면 이후 모든 함수에서 사용가능

```js
Function.prototype.method = function (name, func) {
  this.prototype[name] = func;
  return this;
}
```

* 새로운 메소드를 추가하면 관련된 값들에 즉각 새로운 메소드가 반영됨
* 이때 기본타입의 프로토타입은 public 이기 때문에 주의 필요
* 아래와 같이, 존재하지 않을 경우에만 추가하는 방식으로 가능

```js
Function.prototype.method = function (name, func) {
  if (!this.prototype[mame]) {
    this.prototype[mame] = func;
  }
};
```

## 8. 재귀적 호출

* 자바스크립트는 꼬리 재귀(tail recursion) 최적화를 제공하지 않음
* 꼬리 재귀(tail recursion) 최적화: `return`문에서 자기 자신을 호출하는 경우, 반복실행으로 대체하여 속도를 매우 빠르게 향상시키는 방법

## 9. 유효범위 (Scope)

* 블록 유효범위(`{}`내로 범위를 한정하는 것)를 지원하지 않음
* 대신 함수 유효범위를 지원. 함수 내에서만 변수(매개변수 포함)가 유효함
* 내부에 정의된 변수는 함수 어느부분에서도 접근 가능
* 그렇기 때문에, 함수에서 사용하는 모든 변수를 함수 첫부분에서 정의하는 것이 최선

## 10. 클로저 (closure)

* 자바스크립트는 내부함수에서 외부함수의 변수를 접근할 수 있는 특성이 있음
* 외부함수 보다 내부함수가 더 오랬동안 유지될 경우 흥미로운 특징이 있음

```js
var myObject = function () {
  var value = 0;
  return {
    increment: function () {
      value == typeof inc === 'number' ? inc : 1;
    },
    getValue: function () {
      return value;
    }
  };
} ();
```

* `myObject`의 두개의 메소드에서는 `value`를 접근할 수있지만 외부에서는 `getValue` 메소드를 통해서만 value에 접근 가능

## 11. 콜백

* 비연속적인 이벤트를 다루는데 도움
* 서버로 요청을 비동기식으로 하고 서버로부터 응답이 왔을때 콜백함수를 호출되도록 하는 방식

## 12. 모듈

* 함수 유효범위와 클로저(closure)를 통해 모듈을 만들 수 있음
* 메소드 내부에 private 변수를 선언하고 `return`문에 선언한 변수를 접근할 수 있는 함수를 반환하도록 함

## 13. 연속 호출 (Cascade)

* 메소드 실행시 `this`를 반환하도록 하여, 연속해서 메소드를 호출 할 수 있도록 하는 것

```js
getElmentById('myBoxDiv').
  move(350, 0).
  width(100).
  height(200).
  color('black');
```

## 14. 커링 (Curry)

* 함수와 인수를 결합하여 새로운 함수를 만들 수 있음

```js
Function.method('curry', function () {
  var args = arguments, that = this;
  return function() {
    return that.apply(null, args.concat(arguments));
  };
}); // 뭔가 잘못된 점이 있음
```

```js
// curry 메소드를 이용하여 새로운 함수를 생성
var add1 = add.curry(1);
document.writeln(add1(6));  // 7
```

* curry 메소드는 커링할 원래 함수와 인수를 유지하는 클로저를 만드는 방식으로 동작
* curry 메소드를 호출할 때 받은 인수와 자신을 초출할 때 받게 되는 인수를 결합하여 curry를 실행한 원래 함수를 호출
* 위 curry 메소드에 문제가 있음
  - arguments는 배열이 아니기 때문에 concat 메소드가 없음
  - 이를 위해 arguments에 배열의 slice 메소드 적용

```js
Function.method('curry', function () {
  var slice = Array.prototype.slice,
      args = slice.apply(arguments),
      that = this;
  return function() {
    return that.apply(null, args.concat(slice.apply(arguments));
  };
});
```

## 15. 메모이제이션 (memoization)

* 불필요한 작업을 피하기 위해 이전에 연산한 결과를 저장하고 있는 객체를 사용하는 것
* 클로저를 통해 이전 연산 결과를 저장


