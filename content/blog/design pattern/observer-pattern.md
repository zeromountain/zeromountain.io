---
title: observer pattern
date: 2022-04-10 20:04:91
category: design pattern
thumbnail: { thumbnailSrc }
draft: false
---

## INTRO

![](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/1708px-Observer.svg.png)

최근에 옵저버 패턴을 구현하는 기업 과제를 진행하게 되었다.

옵저버 패턴이라는 것을 책이나 강의를 통해서 스쳐가면서 들었던 기억이 있어서, 옵저버 패턴이 이벤트 시점을 감시하는 감시자 패턴이라는 것은 알고 있었지만 구체적으로 어떻게 구현해야 할지는 막막했다.

결국에는 옵저버 패턴 구현을 깔짝 거리다 과제를 마무리했고, 과제가 끝나고서 옵저버 패턴을 어떻게 구현할 수 있는지에 대해서 학습한 내용을 기록하려고 한다.

## 옵저버패턴

옵저버 패턴이이란 객체의 상태 변화를 관찰하는 관찰자들, 즉 옵저버들의 목록을 객체에 등록하여 상태 변화가 있을 때마다 메서드 등을 통해 객체가 직접 목록의 각 옵저버에게 통지하도록 하는 디자인 패턴이다.

- 주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다.
- 발행/구독 모델로 알려져 있기도 하다.

([이상 위키백과 정의](https://ko.wikipedia.org/wiki/%EC%98%B5%EC%84%9C%EB%B2%84_%ED%8C%A8%ED%84%B4))

위키 백과 정의를 인용해 봤는데, 음...좀 뭔가 확 와닿지 않는다. 실례를 들어서, 수학 선생님과 각 반의 반장들의 관계를 들어서 설명해 보겠다.

수학 선생님은 학생들을 위해서 두 가지 이벤트를 준비했다. 미적분 과제와 중간 고사 시험 공지이다. 이렇게 선생님으로부터 이벤트가 발생하면, 선생님은 각 반의 반장들에게 이벤트가 발생했다는 것을 알리는 이러한 구조를 옵저버 패턴이라고 한다.

어느 정도 감이 잡힌거 같으니, 어떻게 구현할 수 있을지에 대해서 생각해보자.

### 구현

먼저 선생님의 역할을 하는 이벤트 발행자에 대한 구현 목록은 다음과 같다.

- 구독자를 관리하는 리스트
- 구독자를 등록하는 함수
- 구독자 등록을 해제하는 함수
- 이벤트가 발생했음을 구독자에게 알리는 함수

```js
class Teacher {
  constructor() {
    this.students = []
  }

  subscribe(student) {
    this.students.push(student)
  }

  unsubscribe() {
    this.students.pop()
  }

  notify() {
    this.students.forEach(student => student.update())
  }
}
```

학생의 역할을하는 구독자에 대한 구현 목록은 다음과 같다.

- 발행자로부터 알림을 받았을 때 처리할 로직 함수

```js
class Observer {
  update() {}
}

class Student1 extends Observer {
  update() {
    console.log('애들아, 미적분 과제랑 중간 고사 범위 공지 떴다.')
  }
}

class Student2 extends Observer {
  update() {
    console.log('애들아, 확률과 통계 과제랑 중간 고사 범위 공지 떴다.')
  }
}
```

위에서 구현한 클래스를 통해서 인스턴스를 생성해 적용해서 `notify` 함수를 실행하면, 다음과 같은 결과를 확인할 수 있다.

```js
var teacher = new Teacher()
var student1 = new Student1()
var student2 = new Student2()

teacher.subscribe(student1)
teacher.subscribe(student2)

teacher.notify()
```

```md
애들아, 미적분 과제랑 중간 고사 범위 공지 떴다.
애들아, 확률과 통계 과제랑 중간 고사 범위 공지 떴다.
```

## OUTRO

옵저버 패턴을 학습하면서, 옵저버 패턴도 MVC 패턴과 크게 다르지 않다고 느껴졌다.

- M(model-observable) V(view-observer) C(controller)

지금이야 간단한 예제를 통해서 적용해 보았지만, 관리해야할 구독자가 많아지고 이벤트가 발생했을 때, 처리해야 할 로직들이 많아진다면 지금보다 더 구현하기 어려워 질거라는 생각을 했다.

디자인 패턴에 대해서는 크게 관심을 가지고 있지 않았는데, 이번에 기업 과제를 진행하면서 디자인 패턴이 프로그램을 설계하는데 많은 영향을 끼친다는 것을 느꼈고, 앞으로 꾸준히 공부해서 역량을 다져야 할 영역이라고 생각됐다.

> 참고 영상
>
> [디자인패턴, Observer Pattern, 옵저버 패턴 - 코드없는 프로그래밍](https://www.youtube.com/watch?v=1dwx3REUo34)
>
> [{컴퓨터공학:자막필수} 옵저버패턴, MVC 패턴, 콜백함수 그리고 구현 요령 - 시니어코딩
> ](https://www.youtube.com/watch?v=yNvD0pQrYZw)
