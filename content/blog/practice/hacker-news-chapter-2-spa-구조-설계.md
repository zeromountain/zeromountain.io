---
title: Hacker-News chapter 2 SPA 구조 설계
date: 2022-02-10 13:02:30
category: practice
thumbnail: { thumbnailSrc }
draft: false
---

# 해커뉴스 웹 어플리케이션 제작하기 - (2)

## SPA

SPA(Single Page Application)은 어떤 웹 사이트의 전체 페이지를 하나의 페이지에 담아 동적으로 화면을 바꿔가며 표현하는 것을 말합니다.

## SPA 설계

### 링크

기존 Hacker News 애플리케이션에 링크를 추가함으로 간단한 SPA 구조를 만들어 보겠습니다.

```js
for (let i = 0; i < data.length; i++) {
  const li = document.createElement('li')
  li.innerHTML = newsFeed[i].title
  ul.appendChild(li)
}
```

위와 같이 기존에는 API 서버에서 받아온 타이틀을 li 요소에 추가하는게 전부였지만, 다음과 같이 링크 요소를 추가해 페이지 전환을 할 수 있도록 준비합니다.

```js
for (let i = 0; i < newsFeed.length; i++) {
  const li = document.createElement('li')
  const a = document.createElement('a')

  a.href = `#${newsFeed[i].id}`
  a.innerHTML = `${newsFeed[i].title} (댓글: ${newsFeed[i].comments_count})`

  li.appendChild(a)
  ul.appendChild(li)
}
```

- `a.href=#${newsFeed[i].id}`: `<a>` 요소의 href 속성을 API 서버에서 받은 데이터의 id 값으로 설정합니다.
  - hashchange 이벤트로 감지

<img src="https://i.ibb.co/8d37NHT/2022-02-09-3-41-31.png">

### 이벤트 설정

우리는 링크 요소를 클릭했을 때, 해당 제목의 상세 페이지로 이동하길 원합니다.

그래서 a 요소에 클릭 이벤트를 주어서 클릭할 때마다 새로운 돔 요소를 생성하고 싶지만, 모든 a 요소에 클릭 이벤트를 주는 것은 비효율적이므로 window에 hashchange 이벤트를 주어서 URL의 해시값이 바뀌면 새로운 돔 요소를 생성하도록 해보겠습니다.

```js
const content = document.createElement('div')
const CONTENT_URL = 'https://api.hnpwa.com/v0/item/@id.json'

window.addEventListener('hashchange', () => {
  const id = location.hash.substr(1)
  ajax.open('GET', CONTENT_URL.replace('@id', id), false)
  ajax.send()

  const newsContent = JSON.parse(ajax.response)
  const title = document.createElement('h1')

  title.innerHTML = newsContent.title

  content.appendChild(title)
})
```

위의 코드를 통해서 링크 요소를 클릭했을 때, 해당 링크의 해시값으로 CONTENT_URL의 @id 값을 대체한 주소로부터 받아온 데이터를 통해 아래 이미지와 같이 새로운 h1 요소를 생성할 수 있습니다.

<img src="https://i.ibb.co/BNGVPcV/2022-02-10-2-25-15.png">
