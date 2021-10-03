---
title: flex
date: 2021-10-04 00:10:98
category: CSS
thumbnail: { thumbnailSrc }
draft: false
---

> ## display
>
> dsiplay 속성은 박스 바깥쪽 속성과 박스 안쪽 속성으로 나눌 수 있다
>
> 박스 바깥쪽 속성으로는 `block` `inline` `inline-block` 속성이 있고
>
> 박스 안쪽 속성으로는 `flex`와 `grid` 속성이 있다
>
> 레거시 코드에서는 외부 속성과 내부 속성을 동시에 사용할 때, 스페이싱을 활용했지만 CSS3에서는 `-`를 사용한다
>
> `display: inline flex` 👉 `display: inline-flex`

# FLEX

`flex`는 아이템들의 1차원 정렬을 표현하는 방법으로서, 부모 컨테이너에 사용할 수 있는 속성과 자식 아이템에 사용할 수 있는 속성으로 구분할 수 있다

- **flex container**: 아이템들을 포함하고 있는 부모 컨테이너
- **flex item**: 부모 컨테이너에 담겨 있는 자식 요소
- **main axis**: 메인 축
- **cross axis**: 교차 축

## Container

### flex-direction

- `flex` 컨테이너의 주축 방향을 설정하는 속성
  - **row**(→) **row-reverse**(←) **column**(↓) **column-reverse**(↑)

> **row**
>
> <img width="827" alt="row" src="https://user-images.githubusercontent.com/54147313/133017527-6fdfafac-55a2-4698-8914-6e1ffa07f9cb.png">

> **row-reverse**
>
> <img width="362" alt="row-reverse" src="https://user-images.githubusercontent.com/54147313/133017561-39720841-fc3a-4fad-962b-8a38900ee77a.png">

> **column**
>
> <img width="94" height="200px" alt="column" src="https://user-images.githubusercontent.com/54147313/133017629-a51aefad-a1e8-4c8b-b859-2dfc6bbd397b.png">

> **column-reverse**
>
> <img width="83" height="200px" alt="column-reverse" src="https://user-images.githubusercontent.com/54147313/133017659-4a618290-e8e9-43fa-8154-a70484019179.png">

**SYNTAX**

```css
/* 한 줄의 글을 작성하는 방향대로 */
flex-direction: row;

/* <row>처럼, 대신 역방향 */
flex-direction: row-reverse;

/* 글 여러 줄이 쌓이는 방향대로 */
flex-direction: column;

/* <column>처럼, 대신 역방향 */
flex-direction: column-reverse;
```

---

### flex-wrap

- 아이템의 본래 크기를 유지하기 위해서 컨테이너를 아이템으로 wrapping할 할지 설정
  - 한 줄로 표현할지 여러 줄로 표현할지
  - 한 줄(nowrap): 아이템 요소 축소 (강제성이 있음)
  - 여러 줄(wrap, wrap-reverse): 아이템 요소 크기 유지하고 다음 행에 배치

> ****nowrap****
>
> <img width="490" alt="no-wrap" src="https://user-images.githubusercontent.com/54147313/133020639-d476af9a-9395-456f-b6f1-690050ce63c0.png">

> **wrap**
>
> <img width="493" height="200px" alt="wrap" src="https://user-images.githubusercontent.com/54147313/133020654-99f04054-4f65-4966-9a97-8f511f3f9104.png">

> **wrap-reverse**
>
> <img width="498" height="200px" alt="wrap-reverse" src="https://user-images.githubusercontent.com/54147313/133020674-52613b18-81cf-41ac-9855-3fc9747cb8e4.png">

**SYNTAX**

```css
flex-wrap: nowrap; /* Default value */
flex-wrap: wrap;
flex-wrap: wrap-reverse;
```

---

### flex-flow

- `flex-direction`과 `flex-wrap` 속성을 단축속성으로 사용할 수 있다

**SYNTAX**

```css
/* flex-flow: <'flex-direction'>과 <'flex-wrap'> */
flex-flow: row nowrap;
flex-flow: column wrap;
flex-flow: column-reverse wrap-reverse;
```

---

### justify-content

- 주축을 기준으로 아이템을 정렬하는 방법

**SYNTAX**

```css
/* Positional alignment */
justify-content: center; /* Pack items around the center */
justify-content: flex-start; /* Pack flex items from the start */
justify-content: flex-end; /* Pack flex items from the end */

/* Distributed alignment */
justify-content: space-between; /* Distribute items evenly
                                   The first item is flush with the start,
                                   the last is flush with the end */
justify-content: space-around; /* Distribute items evenly
                                   Items have a half-size space */
```

**비교**

```css
.container {
  border: 5px dashed orange;
  display: flex;
  margin: 20px;
}
.item {
  width: 50px;
  height: 50px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;
  font-size: 30px;
}

.container:nth-child(1) {
  justify-content: space-between;
}
.container:nth-child(2) {
  justify-content: space-around;
}
```

<img width="526" alt="between around" src="https://user-images.githubusercontent.com/54147313/133022175-a0b7b915-e848-45e0-92ba-2495eaaad8f4.png">

- btween: 아이템 사이의 여백 👉 모든 아이템들 사이에 동일한 여백이 적용(양끝 아이템 margin-right, margin-left 🔥)
- around: 컨테이너와 아이템 사이의 여백 👉 컨테이너와 모든 아이템에 동일한 여백이 적용 (모든 아이템 풀마진 🔥)

---

### align-items

- 교차축의 아이템을 정렬하는 방법
  - 한 줄(행)에 대한 정렬

> **flex-start** (`justify-content: space-between`)
>
> <img width="535" height="200px" alt="align-items-flex-start" src="https://user-images.githubusercontent.com/54147313/133022260-6752937b-d8b5-4e79-9c2c-9a6a7f8e1202.png">

> **center**
>
> <img width="633" height="200px" alt="align-items-center" src="https://user-images.githubusercontent.com/54147313/133022321-54e1325b-d810-4cdf-a6fe-128a214954c5.png">

> **flex-end**
>
> <img width="535" height="200px" alt="스크린샷 2021-09-12 오후 7 51 51" src="https://user-images.githubusercontent.com/54147313/133022374-7d5ac6d9-1f36-4317-8ef6-9e23add2054c.png">

> **stretch**(height 속성 없는 경우 🤔)
>
> <img width="537" alt="스크린샷 2021-09-12 오후 7 52 06" src="https://user-images.githubusercontent.com/54147313/133022489-afabb187-e8e9-4bcd-b387-92b37f2d9e2d.png">

**SYNTAX**

```css
/* Basic keywords */
align-items: normal;
align-items: stretch;

/* Positional alignment */
/* align-items does not take left and right values */
align-items: center; /* Pack items around the center */
align-items: flex-start; /* Pack flex items from the start */
align-items: flex-end; /* Pack flex items from the end */
```

---

### align-content

- 여러 줄에 대한 교차축 정렬

> **flex-start**
>
> <img width="493" alt="align-content-flex-start" src="https://user-images.githubusercontent.com/54147313/133023714-19f646ac-4276-4d54-8251-6e55b3cf2be6.png">

> **flex-end**
>
> <img width="491" alt="align-content-flex-end" src="https://user-images.githubusercontent.com/54147313/133023723-9343763f-9c71-4652-985c-8393d0bf58cf.png">

> **center**
>
> <img width="491" alt="align-content-center" src="https://user-images.githubusercontent.com/54147313/133023741-1b42f035-eff9-498c-b665-3d1e5034d585.png">

> **space-between & space-around**
>
> ![스크린샷 2021-09-13 오후 1 25 35](https://user-images.githubusercontent.com/54147313/133023918-8f27533d-f052-41a7-b207-a853fb39f9f7.png)

**SYNTAX**

```css
/* Basic positional alignment */
/* align-content does not take left and right values */
align-content: center; /* Pack items around the center */
align-content: flex-start; /* Pack flex items from the start */
align-content: flex-end; /* Pack flex items from the end */

/* Distributed alignment */
align-content: space-between; /* Distribute items evenly
                                 The first item is flush with the start,
                                 the last is flush with the end */
align-content: space-around; /* Distribute items evenly
                                 Items have a half-size space
                                 on either end */
```

## Item

### order

- 현재 아이템의 배치 순서 지정
  - value 값이 작을수록 선배치
  - 기본값은 0이며, 마크업 순서를 따른다
    - 같은 값을 가진 경우도 마크업 순서로 배치

**SYNTAX**

```css
/*  
  <integer>: 음의 정수, 0, 양의 정수
*/
order: <integer>;
```

**에제**

```css
.container {
  height: 200px;
  border: 5px dashed orange;
  display: flex;
}

.item {
  width: 50px;
  height: 50px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;
}

.item:nth-child(1) {
  order: -1;
}
.item:nth-child(2) {
  order: 2;
}
.item:nth-child(3) {
  order: -2;
}
.item:nth-child(4) {
  order: 6;
}
```

<img width="493" alt="order" src="https://user-images.githubusercontent.com/54147313/133024285-a2a6ddce-7614-4103-a426-0dea4e4e71a7.png">

---

### flex-shrink

- 아이템이 차지할 수 있는 범위를 지정 👉 축소
  - **전제조건** 아이템 요소들의 크기가 컨테이너 요소의 크기보다 클 경우
  - 줄어든 영역만큼 나눠 갖아서 줄어든다
    - 만약, 3개의 아이템 요소가 있고 컨테이너의 크기가 120px 줄었다면, 아이템 요소들은 각각 40px 만큼 줄어든다

**SYNTAX**

```css
/*
  <number>: 0, 양의정수, 소수
*/
flex-shrink: <number>;
```

**예제**

```css
.container {
  height: 500px;
  border: 5px dashed orange;
  display: flex;
}

.item {
  width: 200px;
  height: 100px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;
}

.item:nth-child(3) {
  flex-shrink: 0;
}

.item:nth-child(5) {
  flex-shrink: 2;
}
```

![스크린샷 2021-09-13 오후 1 40 50](https://user-images.githubusercontent.com/54147313/133025043-ed409d16-e9bb-4f0e-ba7f-51367ceb1308.png)

- `flex-shrink`의 기본값은 1이기 때문에 3번째 아이템과 5번재 아이템을 제외한 요소들에는 기본값이 적용되고
- 3번째 요소는 `flex-shirink` 값이 0이기 때문에 최대한 요소의 크기를 유지하고
- 5번째 요소는 `flex-shirink` 값이 2이기 때문에 컨테이너가 줄어드는 길이만큼 1:2의 비율로 나머지 요소들과 줄어든 컨테이너 크기의 2/3 만큼 줄어든다

---

### flex-grow

- 요소가 차지할 수 있는 범위를 지정 👉 확대
  - 컨테이너의 남는 공간을 각 아이템이 나누어 갖는다
  - 컨테이너의 남는 공간의 길이만큼 나누어서 늘어남

**SYNTAX**

```css
/*
  <number>: 0, 양의정수, 소수
*/
flex-grow: <number>;
```

**예제**

```css
.container {
  height: 500px;
  border: 5px dashed orange;
  display: flex;
  flex-wrap: wrap;
}

.item {
  width: 100px;
  height: 100px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;
}

.item:nth-child(3) {
  flex-grow: 1;
}
.item:nth-child(5) {
  flex-grow: 2;
}
```

![스크린샷 2021-09-13 오후 1 58 21](https://user-images.githubusercontent.com/54147313/133026437-d1613309-7845-4fb9-b6dc-eb2f6021929d.png)

- `flex-grow`의 기본값은 0이기 때문에 3번째 요소와 5번째 요소를 제외한 나머지 요소들은 기본값이 적용되고
- 3번째 요소는 `flex-grow` 값이 1이기 때문에 컨테이너 영역의 늘어나는 길이의 1/3 만큼 길이가 늘어나며
- 5번째 요소는 `flex-gorw` 값이 2이기 때문에 컨테이너 영역의 늘어나는 길이의 2/3 만큼 길이가 늘어난다

---

### flex-basis

- 아이템의 초기 크기를 지정
  - box-sizing을 지정하지 않으면 콘텐츠 박스의 크기를 변경
  - 절대길이와 상대길이는 다르게 적용된다
    - 절대길이를 100px로 설정했다면, 100px이 가능한 컨테이너 영역의 크기만큼 버틴다
    - 상대길이는 줄어든 컨테이너 영역의 길이에 영향을 받는다

**SYNTAX**

```css
/*
  <width>: <length>, <percentage>, auto
*/
flex-basis: <width>;
```

**예제**

**절대길이**

```css
.container {
  height: 500px;
  border: 5px dashed orange;
  display: flex;
}

.item {
  height: 100px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;

  flex-basis: 100px;
}

.item:nth-child(1) {
  flex-grow: 5;
}

.item:nth-child(2) {
  flex-grow: 1;
}
.item:nth-child(3) {
  flex-grow: 3;
}
```

![flex-basis](https://user-images.githubusercontent.com/54147313/133027646-9c8baa96-16c9-441f-a284-a9041f891d59.gif)

**상대길이**

```css
.container {
  height: 500px;
  border: 5px dashed orange;
  display: flex;
}

.item {
  height: 100px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;

  flex-basis: 10%;
}

.item:nth-child(1) {
  flex-grow: 5;
}

.item:nth-child(2) {
  flex-grow: 1;
}
.item:nth-child(3) {
  flex-grow: 3;
}
```

![flex-basis 10](https://user-images.githubusercontent.com/54147313/133028007-edc21207-7bd2-43ba-b46b-ffaacbcd5ba6.gif)

> 절대길이는 아이템의 크기를 유지하려고 하는 반면에 상대길이는 컨테이너 길이에 영향을 받아서 줄어드는 것을 확인할 수 있다

---

### flex

- `flex-grow` `flex-shrink` `flex-basis`를 단축속성으로 사용
  - `flex-basis` 값을 입력하지 않으면 초기값인 auto가 적용되지 않고 0이 적용된다
    - `flex-basis` auto 값은 아이템 요소의 width 속성을 따른다

**SYNTAX**

```css
/*
value 1개 👉 flex-grow 값으로 적용, flex-shrink 기본값 1, flex-basis는 0으로 적용
value 2개 👉 첫번째 값 flex-grow 적용
             두번째 값이 <number> 👉 flex-shrink 적용
             두번째 값이 <width> 👉 flex-basis 적용

value 3개 👉 grow shrink basis
*/

flex: <number> <number & width> <width>
flex: initial
flex: auto
flex: none
```

- initial: `flex: 0 1 auto`
- auto: `flex: 1 1 auto`
- none: `0 0 auto`

---

### align-self

- 아이템 자신에 대한 정렬 방법
  - align-items와 같은 키워드 사용

**SYNTAX**

```css
/* Positional alignment */
/* align-self does not take left and right values */
align-self: center; /* Put the item around the center */
align-self: flex-start; /* Put the flex item at the start */
align-self: flex-end; /* Put the flex item at the end */
align-self: stretch; /* Stretch 'auto'-sized items to fit the container */
```

**예제**

```css
.container {
  height: 500px;
  border: 5px dashed orange;
  display: flex;
  flex-wrap: wrap;

  align-items: center;
}

.item {
  width: 200px;
  height: 50px;
  margin: 5px;
  background-color: paleturquoise;
  border: 3px solid blue;
}

.item:nth-child(4) {
  height: auto;
  align-self: stretch;
}
```

![스크린샷 2021-09-13 오후 2 40 43](https://user-images.githubusercontent.com/54147313/133029715-d86a299b-fb5f-422d-868e-b1fba44a0621.png)
