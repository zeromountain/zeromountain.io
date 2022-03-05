---
title: React의 동작 원리
date: 2022-03-05 11:03:23
category: react
thumbnail: { thumbnailSrc }
draft: false
---

> 리액트를 사용하면서 리액트가 어떻게 동작하는지에 대해서는 크게 생각하지 않고 사용한 듯 하다.
>
> 그래서 리액트가 어떻게 동작하는지 한번 정리해 보는 시간을 가지려고 한다.
>
> 제목은 거창하게 React의 동작 원리라고 적었지만, 거창한 제목을 내용으로 잘 채워나갈수 있을지 걱정이 되지만,,, 시작해 보겠다.

# React 어떻게 동작할까?

먼저 React가 어떻게 동작하는지 이해하기 위해서는 React 이전에 Javascript를 사용해 웹페이지를 그렸는지 이해할 필요가 있다.

## DOM 직접 접근

> 브라우저 렌더링 과정
> ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTCvCK4ZZ4tDDzYc1AcgVOpRCMgZ-vuVAzyQA&usqp=CAU)

Javascript를 사용해 웹을 구성할 때, DOM에 직접 접근해 DOM을 추가하는 방식을 사용한다.

```js
const ul = document.createElement('ul')

for (let i = 0; i < data.length; i++) {
  const li = document.createElement('li')
  li.innerHTML = newsFeed[i].title
  ul.appendChild(li)
}

document.getElementById('root').appendChild(ul)
```

위의 코드처럼 Javascript는 돔에 직접 접근해서 DOM을 생성한다.

하지만, DOM에 직접 접근할 경우 가장 큰 문제점은 동적인 UI를 구현하는데 한계가 있다.

### DOM의 문제점

**동적 UI**

웹의 규모가 작은 경우라면 괜찮지만, 많은 데이터를 사용해 사용자와 상호 작용하는 웹이라면 DOM을 생성하고 수정하는 일이 많아진다는 문제점이 있다.

브라우저는 DOM트리와 CSSOM트리를 바탕으로 Render Tree를 생성해 Layout과 Paint라는 과정을 통해서 화면을 렌더링하기 때문에 데이터가 바뀌어 DOM이 수정되면 DOM이 다시 그려지게 되고 브라우저 렌더링 과정을 다시 반복한다.

**이벤트 핸들러**

DOM에 직접 이벤트 핸들러를 등록하는 일은 다음과 같은 문제점을 가지고 있다.

1. 시간 ↑
2. 에러 발생 ↑
3. 유지 보수의 용이성 ↓

## Virtual DOM

React는 DOM에 직접 접근하는 문제점을 Virtual DOM이라는 추상화 모델을 가지고 DOM을 최소한으로 조작할 수 있도록 구현해냈다.

1. 데이터 업데이트 → 전체 UI를 Virtual DOM에 리렌더링
2. previous Virtual DOM VS current Virtual DOM 비교
3. 변경된 요소만 계산해 실제 DOM에 적용

이처럼 React는 DOM에 접근을 최소화 함으로써 사용자에게 더 나은 웹 애플리케이션 환경을 제공하고, 개발자에게는 개발 비용을 최소화할 수 있도록 했다.

## JSX

React는 기존 Javascript에서 DOM 요소를 생성하는 `createElement` 메서드를 대체할 수 있는 JSX라는 문법을 제공한다.

JSX를 사용하면 다음과 같이 복잡한 과정을 쉽게 구현할 수 있다.

- Javascript

```js
const ul = document.createElement('ul')

for (let i = 0; i < data.length; i++) {
  const li = document.createElement('li')
  li.innerHTML = newsFeed[i].title
  ul.appendChild(li)
}
```

- React JSX

```jsx
return (
  <ul>
    {data.map(item => (
      <li>item.title</li>
    ))}
  </ul>
)
```

위의 코드에서도 알 수 있듯이, JSX는 HTML 태그를 사용하면서 Javascript의 문법까지 사용할 수 있는 Javascript 확장 문법이다.

JSX를 사용하면 기존 Javascript로 DOM API를 사용해 DOM 요소를 그려주던 방식에서 HTML 태그와 Javascript 문법을 사용함으로써 더 직관적이고 쉽게 UI를 만들 수 있게 되었다.
