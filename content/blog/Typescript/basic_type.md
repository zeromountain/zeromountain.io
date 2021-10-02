---
title: basic_type
date: 2021-10-02 16:10:26
category: Typescript
thumbnail: { thumbnailSrc }
draft: false
---

타입스크립트에서 변수에 타입을 선언하는 가장 기본적인 방법에 대해 살펴보자면,

`var 변수명:타입=값`

위와 같이, 변수명에 값을 할당해주기 전에, 타입을 정해주면 됩니다.

### Number

- 변수의 타입이 숫자 형태이면 `number`를 사용해 타입을 지정해 줍니다.

```ts
const score: number = 90
```

### String

- 변수의 타입이 문자 형태이면 `string`을 사용해 타입을 지정해 줍니다.

```ts
const sentence: string = 'Hello, Typescript!'
```

### Boolean

- 변수의 타입이 진위 값인 경우에 `boolean`을 사용해 타입을 지정해 줍니다.

```ts
const isHandsome: boolean = false
```

### Null & Undefined

- 타입스크립트에서 null과 undefined는 타입스크립트의 환경 설정에 따라 할당이 가능하기 때문에 주류로 사용되는 타입은 아닙니다.

```ts
let u: undefined = undefined
let n: null = null
```

### Array

- 자바스크립트의 배열에서는 배열의 요소에 어떤 타입이와도 자유로웠지만, 타입스크립트에서는 배열의 모든 요소가 같은 타입인 것을 지향합니다.
- 기본 표현법

```ts
// 배열의 모든 요소가 number 타입
const arr: number[] = [1, 2, 3]
```

- 제네릭 표현법

```ts
const arr: Array<number> = [1, 2, 3]
```

### Object

- 원시 타입이 아닌, 나머지 타입들, 즉 참조 타입을 갖는 모든 변수를 object라 합니다.

- 빈 객체

```ts
const obj: object = {}
const arr: object = []
const func: object = function() {}
```

- 속성과 값이 있는 객체

```ts
const SON: { name: stirng; job: string; joined_club: string } = {
  name: '손흥민',
  job: '축구선수',
  joined_club: '토트넘 핫스퍼',
}
```

- 타입의 재사용을 위한 인터페이스 타입

```ts
interface Player {
  name: string,
  joined_club: string,
}

const SON: Player = {
  name: "손흥민",
  joined_club: "토트넘 핫스퍼"
}

const KDB: Player = {
  name: "케빈 더 브라위너"
  joined_club: "맨체스터시티"
}
```
