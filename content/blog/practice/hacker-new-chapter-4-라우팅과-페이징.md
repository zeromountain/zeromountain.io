---
title: Hacker-New chapter 4 라우팅과 페이징
date: 2022-02-12 18:02:46
category: practice
thumbnail: { thumbnailSrc }
draft: false
---

# 해커뉴스 웹 어플리케이션 제작하기 - (4)

## 동적 라우팅을 위한 함수 분리

이전까지의 작업을 통해서 페이지 전환의 기반을 만들었지만, 페이지가 동적으로 전환되지 않았습니다.

이번에는 어플리케이션에서 동적으로 URL을 파싱해서 페이지를 전환시켜주는 router 함수를 만들어 보겠습니다.

### router

`router` 함수의 기본적인 동작은 URL의 해시값을 분석해 상황에 따라서 `newsFeed` 함수와 `newsDetail` 함수를 실행해 줍니다.

```js
function router() {
  const routePath = location.hash;
  if (routePath === '') {
    // location.hash에 #만 있을 경우 빈 문자를 반환
    newsFeed();
  } else if (routePath.indexOf('#/page/') >= 0) {
    store.currentPage = +routePath.substr(7);
    newsFeed();
  } else {
    newsDetail();
  }
```

`router` 함수는 어플리케이션이 실행될 때 한번 실행되고, 해시값의 변화가 일어날 때 마다 실행되어야 하므로, 기존 `hashchange` 이벤트에서 실행되던 `newsFeed` 함수 대신 `router` 함수를 콜백 함수로 사용합니다.

```js
window.addEventListener('hashchange', router) // hashchange 이벤트
router() // 초기 렌더링
```

### newsFeed

```js
function newsFeed() {
  const news = getData(NEWS_URL)
  const newsList = []

  newsList.push('<ul>')
  for (let i = (store.currentPage - 1) * 10; i < store.currentPage * 10; i++) {
    newsList.push(`
    <li>
      <a href="#/show/${news[i].id}">
        ${news[i].title} (댓글: ${news[i].comments_count})
      </a>
    </li>
  `)
  }
  newsList.push('</ul>')
  newsList.push(`
    <div>
      <a href="#/page/${
        store.currentPage > 1 ? store.currentPage - 1 : 1
      }">이전 페이지</a>
      <a href="#/page/${
        store.currentPage * 10 < news.length
          ? store.currentPage + 1
          : store.currentPage
      }">다음 페이지</a>
    </div>
  `)
  container.innerHTML = newsList.join('')
}
```

`newsFeed` 함수는 API 서버로부터 응답받은 뉴스 목록을 10개 단위로 나누어 한 페이지에 뉴스의 타이틀의 목록으로 나열해 보여주는 역할을 합니다.

### newsDetail

```js
function newsDetail() {
  const id = location.hash.substr(7)

  const newsContent = getData(CONTENT_URL.replace('@id', id))

  container.innerHTML = `
    <h1>${newsContent.title}</h1>

    <div>
      <a href="#/page/${store.currentPage}">목록으로</a>
    </div>
  `
}
```

`newsDetail` 함수는 `newsFeed` 함수에서 생성된 뉴스 목록들 중 하나의 뉴스 타이틀을 선택했을 경우, 해당 뉴스에 대한 상세 페이지로 전환해주는 역할을 합니다.

## 페이징을 위한 상태 다루기

### store

`router`, `newsFeed`, `newsDetail` 함수에서 `currentPage`라는 변수의 참조가 필요하기 때문에, `currentPage`라는 변수는 세 함수가 참조할 수 있도록 전역에서 관리할 필요가 있습니다.

`currentPage` 이외에도 공통으로 사용될 수 있는 변수가 생길 수 있기 때문에, `store`라는 객체를 만들어 변수를 관리하면 조금 더 효율적일거 같습니다.

```js
const store = {
  currentPage: 1,
}
```

### 페이지 변화

`currentPage`는 이전 페이지와 다음 페이지를 클릭했을 때, 값을 1씩 추가하거나 1씩 감소시켜줘서 이전 페이지와 다음 페이지를 구현할 수 있을거 같습니다.

추가적으로 1 페이지의 이전 페이지는 존재하지 않기 때문에 `currentPage`가 1보다 큰 경우에만 1씩 감소하도록 해줍니다.

마찬가지로 게시물이 없는 마지막 페이지인 경우는, 현재 페이지부터 이전 페이지의 게시물의 개수가 서버 API에서 받아 온 뉴스 목록보다 작은 경우에만 1씩 추가해 `currentPage`에 변화를 줍니다.

```js
newsList.push(`
    <div>
      <a href="#/page/${
        store.currentPage > 1 ? store.currentPage - 1 : 1
      }">이전 페이지</a>
      <a href="#/page/${
        store.currentPage * 10 < news.length
          ? store.currentPage + 1
          : store.currentPage
      }">다음 페이지</a>
    </div>
  `)
```
