---
title: Hacker-News chapter 5 tailwindCSS를 활용한 디자인 UI
date: 2022-02-18 18:02:26
category: practice
thumbnail: { thumbnailSrc }
draft: false
---

# tailwindCSS

tailwind CSS는 HTML 파일, Javascript 컴포넌트 그리고 다른 템플릿의 클래스 이름을 분석함으로 동작한다. 클래스명에 해당 요소를 식별하기 위한 이름이 아닌, 스타일 속성을 작성함으로써 클래스명을 분석해 정적인 CSS 파일 형태로 변환해주는 역할을 한다.

## header

```html
<div class="bg-gray-600 min-h-screen">
  <div class="bg-white text-xl">
    <div class="mx-auto px-4">
      <div class="flex justify-between items-center py-6">
        <div class="flex justify-start">
          <h1 class="font-extrabold">Hacker News</h1>
        </div>
        <div class="items-center justify-end">
          <a href="#/page/{{__prev_page__}}" class="text-gray-500">
            Previous
          </a>
          <a href="#/page/{{__next_page__}}" class="text-gray-500 ml-4">
            Next
          </a>
        </div>
      </div>
    </div>
  </div>
</div>
```

<img src="https://i.ibb.co/rHtcy9W/2022-02-18-6-50-45.png">

- 클래스에 식별자를 사용하는 것이 아니라 tailwind CSS에서 정의한 스타일 속성을 작성하면 각 요소에 디자인을 할 수 있다.
- `{{__prev_page__}}`, `{{__next_page__}}`: 템플릿 리터럴 내부에서 데이터로 교체하기 위해 고유한 마킹을 표시했다.

## List

### List Wrapper

```html
<div class="p-4 text-2xl text-white">
  {{__news_feed__}}
</div>
```

### List

```html
<div
  class="p-6 ${
      newsFeed[i].read ? 'bg-violet-500' : 'bg-white'
    } mt-6 rounded-lg shadow-md transition-colors duration-500 hover:bg-green-100"
>
  <div class="flex">
    <div class="flex-auto">
      <a href="#/show/${newsFeed[i].id}" class="text-gray-500"
        >${ newsFeed[i].title }</a
      >
    </div>
    <div class="text-center text-sm">
      <div class="w-10 text-white bg-green-300 rounded-lg px-0 py-2">
        ${ newsFeed[i].comments_count }
      </div>
    </div>
  </div>
  <div class="flex mt-3">
    <div class="grid grid-cols-3 text-sm text-gray-500">
      <div><i class="fas fa-user mr-1"></i>${newsFeed[i].user}</div>
      <div><i class="fas fa-heart mr-1"></i>${newsFeed[i].points}</div>
      <div><i class="far fa-clock mr-1"></i>${newsFeed[i].time_ago}</div>
    </div>
  </div>
</div>
```

<img src="https://i.ibb.co/MBPmxBb/2022-02-18-7-03-15.png">

- 반복문을 순회하며 같은 UI를 가진 List 요소를 생성한다.

## tailwind CSS의 장점과 단점

tailwind CSS를 사용해보며 느꼈던 장점은 BEM과 같은 클래스 이름을 정의하는 컨벤션을 지키며 클래스 식별자 이름을 고민할 필요 없이 그저 클래스에 스타일 속성을 작성하면되기 때문에 스타일 속성을 빠르게 적용해 나가볼 수 있었다.

반면에, 위의 코드들과 같이 클래스에 많은 스타일 속성들이 적용되면서 코드가 길어지면서 코드가 더러워지는 단점도 분명 존재한다.

그럼에도, 개발씬에서 `tailwind CSS`를 선호하는 이유는 분명 단점보다 장점이 더 크기 때문이라고 생각하고, 커스터마이징을 잘 활용해 사용한다면 단점들을 최소화할 수 있을거라고 생각한다.
