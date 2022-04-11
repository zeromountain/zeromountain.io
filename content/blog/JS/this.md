---
title: this
date: 2022-04-11 20:04:97
category: js
thumbnail: { thumbnailSrc }
draft: false
---

## INTRO

모의 면접을 진행하면서 this와 관련된 질문을 받았다.

this가 동적으로 바인딩 되는 부분은 이해하고 있었지만, 함수의 선언과 호출 그리고 스코프와 this의 관계에 대한 이해가 부족하다고 느껴져서 this의 개념에 대해서 정리하려고 한다.

## 자기 참조 변수 THIS

먼저, Javascript에서 함수의 스코프는 정적 스코프(lexical scope)를 따른다. 그래서 함수가 가지고 있는 스코프 영역의 변수들의 참조가 가능하다. 이와는 다르게, this의 참조는 함수의 호출 방법에 따라서 결정된다. this 참조를 변경하는 함수 호출 방법은 크게 네 가지가 있다.

- 일반 함수로서의 호출
- 객체의 메서드로서의 호출
- 생성자함수로서의 호출
- call, apply, bind에 의한 호출

### 일반 함수로서 호출

일반 함수로 호출할 경우, this는 전역 객체를 참조한다.

```js
function a() {
  console.log('a', this) // window
  function b() {
    console.log('b', this) // window
  }
  b()
}
a()
```

주의 할 점은 strict 모드에서는 일반 함수 호출할 경우, this는 undefined를 참조한다.

```js
'use strict'
function a() {
  console.log(this) // undefined
}

a()
```

위 예제들을 살펴보면, 함수로 호출을 할 경우 어떤 함수라도 this는 전역 객체를 참조한다는 것을 알 수 있다.

### 객체의 메서드로서 호출

객체의 메서드로 함수를 호출할 경우, this는 메서드를 호출한 객체를 참조한다.

```js
var person = {
  name: 'zeromountain',
  hello() {
    console.log(`Hello, my name is ${this.name}!`)
  },
}
person.hello() // Hello, my name is zeromountain!
```

이번에는 조금 심화된 코드를 살펴보자.

```js
var person = {
  name: 'zeromountain',
  hello() {
    console.log(`Hello, my name is ${this.name}!`)
  },
}
var anotherPerson = {
  name: 'Yeongsan',
}
anotherPerson.hello = person.hello

person.hello() // Hello, my name is zeromountain! ①
anotherPerson.hello() // Hello, my name is Yeongsan! ②

var hello = person.hello

hello() // Hello, ! ③
```

먼저 1번과 2번은 객체의 메서드로 함수를 호출했다. 이때, anotherPerson의 hello 메서드를 person의 메서드로 동적 할당 해주었기 때문에, this가 person을 참조하지 않을까 헷갈리 수 있지만 메서드를 **호출한** 객체에 this 바인딩 된다는 것을 기억해야 한다. 그래서, `anotherPerson.hello()`는 hello 메서드를 호출한 대상이 `anotherPerson` 객체이기 때문에, `hello` 함수의 this는 `anotherPerson`을 참조한다.

3번의 경우는 hello라는 변수(식별자)에 `person.hello` 메서드를 할당해 주었지만 이건 함수의 호출이 아니라는 것을 기억하자. `hello()`가 일반 함수로 호출 되었기 때문에, hello 함수의 this는 window 전역 객체를 참조한다.

### 생성자 함수로서 호출

생성자 함수로 호출할 경우, this는 new 연산자와 생성자 함수로 생성된 인스턴스 객체를 참조한다.

```js
function Person(name) {
  this.name = name
  this.hello = function() {
    console.log(`Hello, my name is ${this.name}!`)
  }
}

var zeromountain = new Person('zeromountain')
var yeongsan = new Person('Yeongsan')

zeromountain.hello() // Hello, my name is zeromountain!
yeongsan.hello() // Hello, my name is Yeongsan!
```

하지만, 생성자 함수를 new 연산자 없이 호출할 경우에는 일반 함수의 호출로 동작하므로 주의해야 한다.

```js
var pseron = Person('person')

person // undefined
name // person
hello() // Hello, my name is person!
```

생성자 함수를 일반 함수로 호출할 경우, 다음과 같이 프로퍼티와 메서드가 전역 객체에 등록된다.

### call / apply / bind 메서드에 의한 간접 호출

call과 apply는 this를 대상 객체에 바인딩하고 함수를 호출하는 기능을 가지고 있다.

```js
var obj = {
  name: 'zeromountain',
  hello() {
    console.log(`Hello, my name is ${this.name}!`)
  },
}

function sayName() {
  console.log(`Hello, my name is ${this.name}!`)
}

sayName() // Hello, my name is !
sayName.call(obj) // Hello, my name is zeromountain!
sayName.apply(obj) // Hello, my name is zeromountain!
```

call과 apply를 사용해 `sayName` 함수의 this가 `obj` 객체를 참조하도록 설정하고 `sayName` 함수를 호출해 실행할 수 있다.

또, call과 apply는 받을 수 있는 함수의 인자에 차이가 있다. call은 두번째 인자부터 여러개의 인자를 나열하는 방식으로 함수에 입력값을 전달한다. 반면에, apply는 두번째 인자에 배열이나 유사 배열 객체를 입력값으로 받을 수 있다.

- call과 apply의 두 번째 인자로 받은 입력값은 call, apply 메서드로 호출한 함수의 arguments로 참조할 수 있다.

```js
var obj = {
  name: 'zeromountain',
  hello() {
    console.log(`Hello, ${this.name}!`)
  },
}

function sayName() {
  console.log(`Hello, ${this.name}!`)
}

sayName.bind(obj)() // Hello, zeromountain!
```

bind 메서드는 this를 바인딩하지만 함수를 호출해주는 기능이 없기 때문에, 함수를 따로 호출해 주어야한다.

# OUTRO

this에 대해서 정리하면서 아직까지 너무 얇게 알고 있는 개념들이 많다는 것을 느꼈다.

100% 완벽할 수는 없겠지만 100에 수렴할 수 있도록 꾸준히 학습해야겠다.

> 참조 영상
>
> [[인간 JS 엔진 되기 1-6]this는 호출 때 결정된다고!!!](https://www.youtube.com/watch?v=pgo6URFz8tc&list=PLcqDmjxt30Rt9wmSlw1u6sBYr-aZmpNB3&index=6)
