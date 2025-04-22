#javascript
# Chapter 5. 상속

* 코드 재사용 관점에서 중요하다.
* 자바스크립트는 프로토타입 기반 언어
* 객체가 다른 객체로 바로 상속된다.
(클래스 기반에서는 클래스가 다른 클래스로 상속)

## 1. 의사 클래스 방식(Pseudoclassical)

* 자바스크립트는 프로토타입 언어의 본질과 모순되는 점들이 있다.
* 객체에서 다른 객체로 직접 상속하는 방법을 갖는 대신
생성자 함수를 통해 객체를 생성(불필요한 간접 단계)
* 여기에 나오는 예제코드는 잘 이해가 안감.
* 상속을 prototype을 이용해서 구현하는 걸 보여주는 듯.
* 이렇게 구현 했을때 단점들
  - private 이 없고 모든 속성이 public
  - 부모 메소드로 접근 불가
  - 심각한 위험은 생성자 함수를 호출할때 new 연산자를 포함 안하면
  새로운 객체와 바인딩 되는것이 아니라 this는 전역객체와 연결되고
  이렇게 됨으로써 새로운 객체에 필요한 기능을 추가하게 되는것이 아니라
  전역변수에 영향을 미치게 된다.
  (new를 누락해도 컴파일 경고나 런타임 경고가 발생하지 않는다.)
* 결론적으로 이 방식은 좋은 방법이 아니다.

## 2. 객체를 기술하는 객체(Object Specifiers)

* 객체를 기술하는 객체는 만들어질 객체의 명세를 포함

```js
var myObject = maker(f, l, m, c, s);
```
이렇게 하는 대신
```js
var myObject = maker({
  first: f,
  last: l,
  state: s,
  city: c
});
```
이렇게 작성한다.
* 이렇게 하면 인수 순서를 꼭 맞출 필요가 없으며
기본값을 설정하고 있다면 인수를 생략할수도 있다.
* 이러한 방법은 JSON을 사용하여 작업하는 경우 부가적인 이점이 있다.
* 생성자에 JSON 객체를 넘기고 이 생성자가 필요한 기능을 갖춘 객체를 구성하여
반환하는 형식으로 처리 할수 있다.(??)

## 3. 프로토타입 방식

* 순수하게 프로토타입에 기반한 패턴에서는 클래스가 필요 없다.
* 프로토타입에 의한 상속은 기존 객체의 속성을 새로운 객체에서 상속받는 방식

```js
var myMammal = {
  name : 'Herb the Mammal',
  get_name : function() {
    return this.name;
  },
  says : function() {
    return this.saying || '';
  }
};
```
인스턴스에 메소드나 속성 추가
```js
var myCat = Object.create(myMammal);
myCat.name = 'Henrietta';
myCat.saying = 'meow ';
myCat.purr = function(n) {
  var i, s = '';
  for(i=0; i<n; i+=1) {
    if(s) {
      s += '-';
    }
    s += 'r';
  }
  return s;
};
myCat.get_name = function() {
  return this.says + ' ' + this.name + ' ' + this.says();
};
```

## 4. 함수를 사용한 방식

* 위에서 언급한 프로토타입에 의한 상속 패턴의 한가지 단점
-> private 속성을 가질 수 없다.(객체의 모든 속성은 public)

1. 새로운 객체를 생성한다.
2. 필요한 private 변수와 메소드를 정의
3. that에 새로운 객체를 할당하고 메소드를 추가
4. 새로운 객체 that을 반환

```js
var mammal = function(spec) {
  var that = {};
    that.get_name = function() {
      return spec.name;
    };
    that.says - function() {
      return spec.saying || '';
    };

    return that;
};

var myMammal = mammal({name: 'Herb'});
```
name과 saying은 private 속성이 되고 get_name과 says 메소드만이 접근 가능하다.
```js
var cat = function(spec) {
  spec.saying = spec.saying || 'meow';
  var that = mammal(spec);
  that.purr = function(n) {
    var i, s = '';
    for(i=0; i<n; i+=1) {
      if(s) {
        s += '-';
      }
      s += 'r';
    }
    return s;
  };
  that.get_name = function() {
    return that.says() + ' ' + spec.name + ' ' + that.says();
  };
  return that;
};

var myCat = cat({name: 'Henrietta'});
```
* 함수형 패턴은 유연성이 매우 좋다.
* 의사 클래스 패턴보다 작업량이 적고 캡슐화와 정보은닉
그리고 super 메소드에 접근할 수 있는 방법까지 제공

## 5. 클래스 구성을 위한 부속품

* 제품을 만들 때 부속품들을 가져다 조립을 하듯이
객체를 구성할때도 같은 방법으로 할 수 있다.

```js
var eventuality = function(that) {
  var registry = {};

  that.on = function(type, method, parameters) {
    var handler = {
      method: method,
      parameters: parameters
    };
    if(registry.hasOwnProperty(type)) {
      registry[type].push(handler);
    } else {
      registry[type] = [handler];
    }
    return this;
  }

  that.fire = function(event) {
    var array,
      func,
      handler,
      i,
      type = typeof event === 'string' ? event : event.type;

    if(registry.hasOwnProperty(type)) {
      array = registry[type];
      for(i=0; i<array.length; i+=1) {
        handler = array[i];
        func = handler.method;
        if(typeof func === 'string') {
          func = this[func];
        }

        func.apply(this, handler.parameters || [event]);
      }
    }
    return this;
  };

  return that;
}
```


