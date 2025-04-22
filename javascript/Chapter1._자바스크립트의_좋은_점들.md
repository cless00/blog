#javascript
# Chapter 1. 자바스크립트의 좋은 점들

## 1. 왜 자바스크립트 인가?
* 언어 자체에 대해 많이 모르거나 심지어는 프로그래밍에 대해 잘 모르더라도 원하는 작업을 할 수 있다는 것 (표현력이 강한 언어)


## 2. 자바스크립트 분석
* 자바스크립트에서 좋은 점
  - 함수, 느슨한 타입 체크, 동적 객체, 표현적인 객체 리터럴 표기법
* 자바스크립트에서 나쁜 점
  - 전역변수에 기초한 프로그래밍 모델
* JSLint
  - 자바스크립트 프로그램을 분석하여 포함된 나쁜 점들을 알려주는 자바스크립트 파서
  - 엄격한 수준의 분석 결과 제공함

## 3. 예제 테스트를 위한 간단한 준비
* program.html

```html
<html><body><pre><script src="program.js">
</script></pre></body></html>
```

* program.js

```js
document.writeln('Hello, world!');
```

* Method 정의를 위한 method 메소드

```js
Function.prototype.method = function (name, func) {
    this.prototype[name] = func;
    return this;
};
```


