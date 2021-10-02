---
title: arrow_function
date: 2021-10-02 16:10:31
category: JS
thumbnail: { thumbnailSrc }
draft: false
---

# 🔍 Arrow function(화살표 함수)

## 1. 정의

화살표를 활용해 함수로 표현한 식을 말합니다.

## 2. 선언

```js
const 함수명 = (인자1, 인자2, ..., 인자N) => {statements}
```

## 3. 표현식 비교

### ✔️ 함수 표현식

```js
const adder = function(x) {
  return function(y) {
    return x + y
  }
}
```

### ✔️ 화살표 함수

```js
const adder = x => {
  return y => {
    return x + y
  }
}
```

화살표 함수는 여기서 더 축소해서 표현이 가능하다.

➡️ statement가 return값만있다면
➡️ 함수의 인자가 한개라면

```js
const adder = x => y => x + y
```

같은 함수지만 함수를 한줄로 끝낼 수 있다.
마른 자바스크립트에 한 줄기 빛 같은 존재이다.

## 4. 일반 함수와의 차이

➡️ 화살표 함수는 인스턴스를 생성할 수 없다.

```js
const Foo = () => {}
// 화살표 함수는 생성자 키워드로 호출 불가
new Foo() // 타입에러 발생
```

- 인스턴스를 생성할 수 없기때문에 prototype 프로퍼티가 없고 생성하지 않는다.

```js
const Foo = () => {}
Foo.hasOwnProperty('prototype') // false
```

➡️ 중복된 매겨변수 이름 선언 불가

```js
const arrow = (a, a) => a + a
// SyntaxError: Duplicate ~
```

➡️ 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다
