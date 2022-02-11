---
title: Hacker-News chapter 3 리팩토링
date: 2022-02-11 16:02:18
category: practice
thumbnail: { thumbnailSrc }
draft: false
---

# 해커뉴스 웹 어플리케이션 제작하기 - (3)

## 리팩토링

지금까지 작성한 코드를 살펴보면, 중복된 코드들을 확인할 수 있습니다.

### 함수

```js
// 1
ajax.open('GET', NEWS_URL, false)
ajax.send()

const newsFeed = JSON.parse(ajax.response)
```

```js
// 2
ajax.open('GET', CONTENT_URL.replace('@id', id), false)
ajax.send()

const newsContent = JSON.parse(ajax.response)
```

위의 두 코드는 서버 API에 get 요청을 보내고, 서버에서 받은 응답을 값으로 변수에 저장하는 부분이 일치한다는 것을 알 수 있습니다.

1번과 2번의 코드 덩어리를 함수로 만들면 효율적으로 재사용할 수 있는 코드가 될 수 있을거 같습니다.

```js
function getData(URL) {
  ajax.open('GET', URL, false)
  ajax.send()

  return JSON.parse(ajax.response)
}
```

다른 부분들은 공통되지만 URL은 서로 다르기 때문에, 함수의 입력값으로 URL을 따로 받아서 서로 다른 URL로 요청을 보내서 응답을 받을 수 있도록 합니다.

getData 함수를 사용해 다음과 같이 함수를 값으로 사용해 변수에 할당할 수 있습니다.

```js
const newsFeed = getData(NEWS_URL)
const newsContent = getData(CONTENT_URL.replace('@id', id))
```

### DOM요소 적게 사용하기

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

위의 DOM API를 활용한 코드는 HTML 요소의 관계를 직관적으로 알기 어렵다는 단점을 가지고 있습니다.

그래서 DOM API의 사용을 최소화하고, HTML 태그를 문자열로 표현함으로서 요소끼리의 관계를 직관적으로 파악할 수 있습니다.

```js
for (let i = 0; i < newsFeed.length; i++) {
  const div = document.createElement('div')

  div.innerHTML = `
    <li>
      <a href="#${newsFeed[i].id}">
        ${newsFeed[i].title} (댓글: ${newsFeed[i].comments_count})
      </a>
    </li>
  `

  ul.appendChild(div.firstElementChild)
}
```

`div`라는 요소만 DOM API를 사용해 그릇으로 사용하고 최종적으로는 불필요한 `div`는 DOM 트리에 추가되지 않도록 첫번째 자식 요소만을 `ul` 태그에 자식 요소로 추가합니다.
