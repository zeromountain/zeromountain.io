---
title: DOM이란?
date: 2022-02-22 10:02:13
category: DOM
thumbnail: { thumbnailSrc }
draft: false
---

# DOM

<img src="https://cheonmro.github.io/images/dom-tree.png">

- DOM(Document Object Model)
  - HTML 요소를 제어할 수 있는 API
  - HTML 문서의 계층적 구조와 정보를 표현
  - 프로퍼티와 메서드를 제공하는 트리 자료 구조

## 노드

`<div class="hello">Hello</div>` 이 HTML 코드를 DOM 구조로 분석해보면, 다음과 같다.

- element node: `div`
- attribute node: `class="hello"`
- text node: `Hello`

이렇게 node 구조로 분리되면 트리 구조 형태로 DOM을 구성하게 된다. 위의 코드들을 트리 구조로 나타내 보면 다음과 같다.

<img src="https://i.ibb.co/kM2hRJp/DOM-tree.jpg">

여기서 주목할 점은 `<div>` 태그의 하위에 존재하는 요소가 자식 요소로 포함된다는 것이다.

### 노드의 타입

- 문서노드
  - DOM 트리 최상위 루트 노드
  - document 객체
- 요소노드
  - HTML 요소
  - 어트리뷰트 노드와 형제 관계
- 어트리뷰트 노드
  - HTML 요소의 속성
  - 요소 노드와 형제 관계
- 텍스트 노드
  - HTML 요소의 텍스트
  - DOM 트리의 마지막 노드

## CREATE

### createElement

`document.prototype.createElement` 메서드는 함수의 인자로 입력한 태그 이름의 요소 노드를 생성해 반환한다.

```js
const $container = document.createElement('div')
```

### createTextNode

`document.prototype.createTextNode` 메서드는 함수의 인자로 입력된 문자열을 텍스트 노드로 생성해 반환하고, 생성된 텍스트 노드는 어떤 요소 노드와도 연결된 상태가 아니기 때문에 DOM 트리에 추가하는 작업이 필요하다.

```js
const textNode = document.createTextNode('텍스트 노드는 독립적이다.')
```

## APPEND

### append

`Element.append` 메서드는 노드 객체나 문자열을 요소 노드의 마지막 자식 요소로 삽입할 수 있다.

- 노드 객체 삽입

```js
const $parent = document.createElement('div')
const span = document.createElement('span')

$parent.append(span)
```

- 문자열 삽입

```js
const $parent = document.createElement('parent')

$parent.append('마지막 자식 요소로 문자열이 삽입됩니다.')
```

<img src="https://i.ibb.co/H21bD3j/2022-02-22-1-20-25.png">

### appendChid

`Node.appendChild` 메서드는 호출한 노드의 마지막 자식 요소로 함수 인자로 입력된 노드를 추가하고 추가된 노드를 반환한다.

```js
const $parent = document.createElement('div')
const textNode = document.createTextNode('텍스트 노드')

$parent.appendChild(textNode)
```

<img src="https://i.ibb.co/dbYmkbK/2022-02-22-1-42-23.png">

### append와 appendChild의 차이

|                        | append | appendChild |
| :--------------------: | :----: | :---------: |
|     노드객체 삽입      |  ⭕️   |     ⭕️     |
| 문자열(DOMString) 삽입 |  ⭕️   |     ❌      |
|         반환값         |   ❌   |     ⭕️     |
|     다중 입력 허용     |  ⭕️   |     ❌      |

## READ

요소 노드를 취득하기 위해서 가장 많이 사용되는 DOM API를 알아보겠다.

### getElementById

`Document.prototype.getElementById` 메서드는 HTML 요소의 속성인 id 값을 참조해 DOM 트리에서 요소 노드를 찾는 방법이다.

> 중복된 id값이 존재하면, 단 하나의 첫 번째 요소만 반환하고, 존재하지 않는 경우 null을 반환한다.

만약, id값이 header라는 요소의 정보를 취득하고 싶다면 다음과 같이 `getEelementById` 메서드를 사용할 수 있다.

```js
const $header = document.getElementById('header')
```

### querySelector

`Document.prototype.querySelector` 메서드는 CSS 선택자를 참조해 DOM 트리에서 요소 노드를 찾는 방법이다.
그렇기 때문에, CSS에서 선택자로 사용되는 id, class, attribute, 후손, 자식, 인접형제, 가상클래스 등의 선택자를 통해서 요소 노드를 찾을 수 있다.

해당 선택자를 만족하는 요소가 중복되는 경우 첫 번재 요소 노드만 반환하고, 존재하지 않는 경우 null을 반환한다.

> CSS 선택자가 유효하지 않는 경우 DOMException 에러 발생

- id가 foo인 요소 찾기

```js
const $foo = document.querySelector('#foo')
```

- class가 foo인 요소 찾기

```js
const $foo = document.querySelector('.foo')
```

- 태그 이름이 div인 요소 찾기

```js
const $div = document.querySelector('div')
```

- input type이 password인 요소 찾기

```js
const $passwordInput = document.querySelector('input[type=password]')
```

### querySelectorAll

`document.querySelectorAll` 메서드는 CSS 선택자를 만족하는 **모든** 요소 노드를 탐색해 NodeList 객체를 반환하고, 요소가 존재하지 않는 경우 빈 NodeList 객체를 반환한다.

> - NodeList 객체는 유사 배열 객체이며 이터러블하다.
>   - NodeList 객체는 forEach 메서드를 제공한다.
>   - Spread Syntax나 Array.from 메서드를 사용해 배열로 변환할 수 있다.
> - CSS 선택자가 유효하지 않는 경우 DOMException 에러 발생

## UPDATE

### innerHTML

`Element.innerHTML` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML이나 XML 마크업을 취득하거나 변경할 수 있다.

- GET

```js
const div = document.querySelector('div')

div.innerHTML
```

<img src="https://i.ibb.co/y0R28pd/2022-02-22-1-50-33.png">

- SET

```js
const div = document.createElement('div')

div.innerHTML = '<span>자식요소</span>'
```

<img src="https://i.ibb.co/nrVFkZH/2022-02-22-1-51-44.png">

### textContent

`Node.textContent` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 손 노드의 텍스트를 모두 취득하거나 변경할 수 있다.

- GET

```js
const div = document.querySelector('div')
div.textContent
```

<img src="https://i.ibb.co/3ctb1YB/2022-02-22-2-02-17.png">

> HTML 마크업은 무시하고 텍스트 노드만 가져온다.

- SET

```js
const div = document.createElement('div')

div.textContent = '텍스트 <span>자식 요소</span>'
```

<img src="https://i.ibb.co/QF151sm/2022-02-22-2-04-17.png">

## DELETE

### remove

`Element.remove` 메서드는 remove 메서드의 대상이 되는 요소를 DOM 트리에서 삭제한다. 여기서 주의해야 할 점은 DOM 트리에 속해 있는지의 여부이다. 다음 코드는 DOM 트리에 추가된 상태가 아니기 때문에 remove로 삭제가 되지 않는다.

```js
const div = document.createElement('div')
const textNode = document.createTextNode('문자열')

div.appendChild(textNode)
div.remove()
```

<img src="https://i.ibb.co/hd0PQp3/2022-02-22-2-33-26.png">

### removeChild

`Node.removeChild` 메서드는 함수의 입력값으로 입력된 노드를 DOM에서 삭제하며, 인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야 한다.

```js
const div = document.createElement('div')
const textNode = document.createTextNode('문자열')

div.appendChild(textNode)
div.removeChild(textNode)
```

<img src="https://i.ibb.co/PTqp1Pk/2022-02-22-2-38-17.png">
