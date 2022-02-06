---
title: Hacker-News chapter 1 데이터 가져오기
date: 2022-02-06 19:02:43
category: practice
thumbnail: { thumbnailSrc }
draft: false
---

> 김민태 강사님 강의를 학습한 내용을 기록한 내용입니다.

# 해커뉴스 웹 어플리케이션 제작하기 - (1)

## API 사용하기

### 요청 URL 알아보기

[해커뉴스 API 문서](https://github.com/tastejs/hacker-news-pwas/blob/master/docs/api.md)를 통해서 뉴스 관련 데이터를 가져오는 URL이 'https://api.hnpwa.com/v0/news/1.json' 형식임을 알 수 있고, URL에서 사용된 1은 페이지 값을 나타내며, API 문서에 따르면 최대 10페이지가 제공되는 것을 알 수 있습니다.

### 요청 URL 사용하기

위의 과정에서 알아본 URL을 가지고 다음과 같이 데이터 요청을 초기화 할 수 있습니다.

```js
const ajax = new XMLHttpRequest()
const NEWS_URL = 'https://api.hnpwa.com/v0/news/1.json'

ajax.open('GET', NEWS_URL, false)
ajax.send()
```

- XHLHttpRequest는 Ajax 통신을 위해서 브라우저에서 제공하는 Web API 입니다.
  - HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공합니다.

### 데이터 출력하기

xhr 객체를 통해서 HPNWA API 서버에서 응답받은 데이터를 출력하는 과정은 다음과 같습니다.

```js
const data = JSON.parse(ajax.response)

for (let i = 0; i < data.length; i++) {
  console.log(data[i].title)
}
```

위의 코드는 응답을 받은 데이터 배열 전체의 길이만큼 순회하며 서버에서 응답 받아온 30개의 객체에서 title 프로퍼티 값을 콘솔창에 출력합니다.

코드를 실행하면 콘솔창에서 아래 이미지와 같은 결과를 확인할 수 있습니다.

<img src="https://i.ibb.co/NVs7kgq/2022-02-06-8-22-44.png" />

### 돔 요소로 추가하기

서버에서 받아온 데이터를 웹 페이지에 렌더링하기 위해서 DOM을 조작해야 합니다.

[DOM](https://developer.mozilla.org/ko/docs/Glossary/DOM)(Document Object Model)이란 HTML 문서의 요소를 트리 형태로 구조화한 객체 모델을 말합니다.

이 DOM을 통해서 Javascript로 HTML 요소를 생성, 추가, 삭제가 가능하며, HPNWA API에서 가져온 데이터를 리스트 형식으로 DOM에 추가하는 과정은 다음과 같습니다.

```js
const ul = document.createElement('ul')

for (let i = 0; i < data.length; i++) {
  const li = document.createElement('li')
  li.innerHTML = newsFeed[i].title
  ul.appendChild(li)
}

document.getElementById('root').appendChild(ul)
```

위의 코드를 실행하면 아래 이미지와 같이 리스트 형태로 각 뉴스의 제목이 페이지에 렌더링되는 것을 확인할 수 있습니다.

<img src="https://i.ibb.co/Jyhz5Jf/2022-02-06-8-39-13.png" />
