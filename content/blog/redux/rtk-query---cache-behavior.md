---
title: RTK Query - Cache Behavior
date: 2022-03-24 18:03:59
category: redux
thumbnail: { thumbnailSrc }
draft: false
---

> RTK Query 공식문서를 번역한 내용입니다.

# Cache Behvior (캐시 동작)

RTK 쿼리의 주요 기능은 캐시된 데이터의 관리입니다. 서버에서 데이터를 가져오면 RTK 쿼리는 Redux 저장소에 데이터를 '캐시'로 저장합니다. 동일한 데이터에 대해 추가 요청이 수행되면 RTK 쿼리는 서버에 추가 요청을 보내는 대신 기존에 캐시된 데이터를 제공합니다.

RTK 쿼리는 캐시 동작을 조작하고 필요에 맞게 조정할 수 있는 다양한 개념과 도구를 제공합니다.

## Default Cache Behavior (기본 캐시 동작)

RTK 쿼리에서 캐싱은 다음을 기반으로 합니다.

- API endpoint definitions (API 엔드포인트 정의)
- The serialized query parameters used when components subscribe to data from an endpoint (구성 요소가 끝점에서 데이터를 구독할 때 사용되는 직렬화된 쿼리 매개변수)
- Active subscription reference counts (활성 구독 참조 수)

구독이 시작되면 엔드포인트와 함께 사용되는 매개 변수가 직렬화되고 요청에 대한 `queryCacheKey`로 내부적으로 저장됩니다. 동일한 `queryCacheKey`를 생성하는 향후 요청(즉, 동일한 매개변수로 호출, 직렬화 인수분해)은 원본에 대해 중복 제거되며 동일한 데이터 및 업데이트를 공유합니다. 즉, 동일한 요청을 수행하는 두 개의 개별 구성 요소가 동일한 캐시된 데이터를 사용합니다.

요청이 시도될 때 데이터가 이미 캐시에 있는 경우 해당 데이터가 제공되고 새 요청이 서버로 전송되지 않습니다. 그렇지 않고 데이터가 캐시에 없으면 새 요청이 전송되고 반환된 응답은 캐시에 저장됩니다.

구독은 참조 횟수입니다. 동일한 엔드포인트 매개변수를 요청하는 추가 구독은 참조 횟수를 증가시킵니다. 데이터에 대한 활성 '구독'이 있는 한(예: 엔드포인트에 대한 useQuery 후크를 호출하는 구성요소가 마운트된 경우) 데이터는 캐시에 남아 있습니다. 구독이 제거되면(예: 데이터를 구독한 마지막 구성 요소가 마운트 해제될 때) 일정 시간(기본값 60초) 후에 데이터가 캐시에서 제거됩니다. 만료 시간은 [API 정의 전체에 대한](https://redux-toolkit.js.org/rtk-query/api/createApi#keepunuseddatafor) keepUnusedDataFor 속성을 사용하여 구성할 수 있을 뿐만 아니라 [엔드포인트별로](https://redux-toolkit.js.org/rtk-query/api/createApi#keepunuseddatafor-1) 구성할 수 있습니다.

### Cache lifetime & subscription example​ (캐시 수명 및 구독 예시​)

id를 쿼리 매개변수로 예상하는 엔드포인트와 이 동일한 엔드포인트에서 데이터를 요청하는 4개의 구성요소가 탑재되어 있다고 상상해 보십시오.

```jsx
import { useGetUserQuery } from './api.ts'

function ComponentOne() {
  // component subscribes to the data
  const { data } = useGetUserQuery(1)

  return <div>...</div>
}

function ComponentTwo() {
  // component subscribes to the data
  const { data } = useGetUserQuery(2)

  return <div>...</div>
}

function ComponentThree() {
  // component subscribes to the data
  const { data } = useGetUserQuery(3)

  return <div>...</div>
}

function ComponentFour() {
  // component subscribes to the *same* data as ComponentThree,
  // as it has the same query parameters
  const { data } = useGetUserQuery(3)

  return <div>...</div>
}
```

4개의 구성 요소가 끝점에 가입되어 있지만 끝점 쿼리 매개 변수의 고유한 조합은 3개뿐입니다. 쿼리 매개변수 1과 2에는 각각 단일 구독자가 있고 쿼리 매개변수 3에는 두 명의 구독자가 있습니다. RTK 쿼리는 세 가지 고유한 가져오기를 수행합니다. 엔드포인트당 고유한 쿼리 매개변수 세트마다 하나씩.

하나 이상의 활성 가입자가 해당 끝점 매개변수 조합에 관심이 있는 한 데이터는 캐시에 보관됩니다. 구독자 참조 횟수가 0에 도달하면 타이머가 설정되고 타이머가 만료될 때까지 해당 데이터에 대한 새 구독이 없으면 캐시된 데이터가 제거됩니다. 기본 만료 시간은 60초로, API 정의 전체와 엔드포인트별로 구성할 수 있습니다.

위의 예에서 'ComponentThree'가 마운트 해제되면 시간이 아무리 지나도 'ComponentFour'가 여전히 동일한 데이터를 구독하고 있기 때문에 데이터가 캐시에 남아 있고 구독 참조 횟수는 1이 됩니다. 그러나 한 번 'ComponentFour'가 마운트 해제되면 구독자 참조 횟수는 0이 됩니다. 데이터는 남은 만료 시간 동안 캐시에 남아 있습니다. 타이머가 만료되기 전에 새 구독이 생성되지 않은 경우 캐시된 데이터가 최종적으로 제거됩니다.

## Manipulating Cache Behavior​ (캐시 동작 조작하기​)

기본 동작 외에도 RTK 쿼리는 데이터가 유효하지 않은 것으로 간주되어야 하거나 그렇지 않으면 '새로고침'하기에 적합한 것으로 간주되는 시나리오에서 더 일찍 데이터를 다시 가져오는 여러 방법을 제공합니다.

### Reducing subscription time with keepUnusedDataFor​ (keepUnusedDataFor로 구독 시간 줄이기​)

기본 캐시 동작 및 캐시 수명 및 구독 예제에서 위에서 언급했듯이 기본적으로 데이터는 구독자 참조 수가 0에 도달한 후 60초 동안 캐시에 남아 있습니다.

이 값은 API 정의와 엔드포인트당 모두에 대해 `keepUnusedDataFor` 옵션을 사용하여 구성할 수 있습니다. 엔드포인트별 버전이 제공되는 경우 API 정의의 설정보다 우선 적용됩니다.

`keepUnusedDataFor`에 값을 초 단위로 제공하면 구독자 참조 횟수가 0에 도달한 후 데이터를 캐시에 보관해야 하는 기간을 지정합니다.

```js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/' }),
  // global configuration for the api
  // API에 대한 전역 설정
  keepUnusedDataFor: 30,
  endpoints: builder => ({
    getPosts: builder.query({
      query: () => `posts`,
      // configuration for an individual endpoint, overriding the api setting
      // api 설정을 재정의하는 개별 엔드포인트에 대한 설정
      keepUnusedDataFor: 5,
    }),
  }),
})
```

### Re-fetching on demand with refetch/initiate​ (refetch/initiate로 요청 시 다시 가져오기​)

데이터 다시 가져오기를 완벽하게 세부적으로 제어하기 위해 [useQuery](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usequery) 또는 [useQuerySubscription](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usequerysubscription) 훅에서 결과 속성으로 반환된 다시 가져오기 기능을 사용할 수 있습니다.

`refetch` 함수를 호출하면 연결된 쿼리를 강제로 다시 가져옵니다.

또는 동일한 효과에 대해 썽크 작업 작성자에게 `forceRefetch: true` 옵션을 전달하여 끝점에 대한 썽크 시작 작업을 디스패치할 수 있습니다.

```jsx
import { useDispatch } from 'react-redux'
import { useGetPostsQuery } from './api'

const Component = () => {
  const dispatch = useDispatch()
  const { data, refetch } = useGetPostsQuery({ count: 5 })

  function handleRefetchOne() {
    // force re-fetches the data
    // 강제로 데이터를 다시 가져옵니다.
    refetch()
  }

  function handleRefetchTwo() {
    // has the same effect as `refetch` for the associated query
    // 연결된 쿼리에 대한 'refetch'와 동일한 효과를 가집니다.
    dispatch(
      api.endpoints.getPosts.initiate(
        { count: 5 },
        { subscribe: false, forceRefetch: true }
      )
    )
  }
* [ ]
  return (
    <div>
      <button onClick={handleRefetchOne}>Force re-fetch 1</button>
      <button onClick={handleRefetchTwo}>Force re-fetch 2</button>
    </div>
  )
}
```

### Encouraging re-fetching with refetchOnMountOrArgChange​ (refetchOnMountOrArgChange로 다시 가져오기 권장)

쿼리는 [refetchOnMountOrArgChange](https://redux-toolkit.js.org/rtk-query/api/createApi#refetchonmountorargchange) 속성을 통해 평소보다 더 자주 다시 가져오도록 권장할 수 있습니다. 이는 엔드포인트 전체, 개별 후크 호출 또는 [초기](https://redux-toolkit.js.org/rtk-query/api/created-api/endpoints#initiate) 작업을 전달할 때 전달할 수 있습니다(작업 생성자의 옵션 이름은 forceRefetch임).

`refetchOnMountOrArgChange`는 기본 동작이 캐시된 데이터를 대신 제공하는 추가 상황에서 다시 가져오기를 권장하는 데 사용됩니다.

`refetchOnMountOrArgChange`는 부울 값이나 숫자를 초 단위의 시간으로 허용합니다.

이 속성에 대해 false(기본값)를 전달하면 위에서 설명한 기본 동작이 사용됩니다.

이 속성에 대해 true를 전달하면 쿼리에 대한 새 구독자가 추가될 때 끝점이 항상 다시 가져옵니다. API 정의 자체가 아닌 개별 후크 호출에 전달된 경우 이는 해당 후크 호출에만 적용됩니다. 즉, 훅을 호출하는 구성 요소가 마운트되거나 인수가 변경되면 엔드포인트 arg(인자) 조합에 대한 캐시된 데이터가 이미 존재하는지 여부에 관계없이 항상 다시 가져옵니다.

숫자를 초 단위 값으로 전달하면 다음 동작이 사용됩니다.

- 쿼리 구독이 생성될 때:
  - 캐시에 기존 쿼리가 있는 경우 해당 쿼리에 대해 현재 시간과 마지막으로 수행된 타임스탬프를 비교합니다.
  - 제공된 시간(초)이 경과하면 다시 가져옵니다.
- 쿼리가 없으면 데이터를 가져옵니다.
- 기존 쿼리가 있지만 마지막 쿼리 이후 지정된 시간이 경과하지 않은 경우 기존 캐시된 데이터를 제공합니다.

```js
// Configuring re-fetching on subscription if data exceeds a given time

import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/' }),
  // global configuration for the api
  refetchOnMountOrArgChange: 30,
  endpoints: builder => ({
    getPosts: builder.query({
      query: () => `posts`,
    }),
  }),
})
```

```jsx
// Forcing refetch on component mount

import { useGetPostsQuery } from './api'

const Component = () => {
  const { data } = useGetPostsQuery(
    { count: 5 },
    // this overrules the api definition setting,
    // forcing the query to always fetch when this component is mounted
    { refetchOnMountOrArgChange: true }
  )

  return <div>...</div>
}
```

### Re-fetching on window focus with refetchOnFocus​ (refetchOnFocus를 사용하여 window 포커스에서 다시 가져오기​)

[refetchOnFocus](https://redux-toolkit.js.org/rtk-query/api/createApi#refetchonfocus) 옵션을 사용하면 응용 프로그램 창이 다시 포커스를 얻은 후 RTK 쿼리가 모든 구독 쿼리를 다시 가져오려고 할 것인지 여부를 제어할 수 있습니다.

`skip: true`와 함께 이 옵션을 지정하면 skip이 false가 될 때까지 평가되지 않습니다.

이렇게 하려면 [setupListeners](https://redux-toolkit.js.org/rtk-query/api/setupListeners)가 호출되어야 합니다.

이 옵션은 [createApi](https://redux-toolkit.js.org/rtk-query/api/createApi)를 사용한 API 정의와 [useQuery](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usequery), [useQuerySubscription](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usequerysubscription), [useLazyQuery](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#uselazyquery) 및 [useLazyQuerySubscription](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#uselazyquerysubscription) 훅에서 모두 사용할 수 있습니다.

```js
// src/services/api.ts

import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/' }),
  // global configuration for the api
  refetchOnFocus: true,
  endpoints: builder => ({
    getPosts: builder.query({
      query: () => `posts`,
    }),
  }),
})
```

```js
// src/store.ts

import { configureStore } from '@reduxjs/toolkit'
import { setupListeners } from '@reduxjs/toolkit/query'
import { api } from './services/api'

export const store = configureStore({
  reducer: {
    [api.reducerPath]: api.reducer,
  },
  middleware: gDM => gDM().concat(api.middleware),
})

// enable listener behavior for the store
setupListeners(store.dispatch)
```

### Re-fetching on network reconnection with refetchOnReconnect​ (refetchOnReconnect를 사용하여 네트워크 재연결 시 다시 가져오기​)

`createApi`의 [refetchOnReconnect](https://redux-toolkit.js.org/rtk-query/api/createApi#refetchonreconnect) 옵션을 사용하면 네트워크 연결을 다시 얻은 후 RTK 쿼리가 구독된 모든 쿼리를 다시 가져오려고 할 것인지 여부를 제어할 수 있습니다.

`skip: true`와 함께 이 옵션을 지정하면 skip이 false가 될 때까지 평가되지 않습니다.

이렇게 하려면 `setupListeners`가 호출되어야 합니다.

이 옵션은 `createApi`를 사용한 API 정의와 `useQuery, useQuerySubscription, useLazyQuery` 및 `useLazyQuerySubscription` 훅에서 모두 사용할 수 있습니다.

```js
// src/services/api.ts

import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export const api = createApi({
  baseQuery: fetchBaseQuery({ baseUrl: '/' }),
  // global configuration for the api
  refetchOnReconnect: true,
  endpoints: builder => ({
    getPosts: builder.query({
      query: () => `posts`,
    }),
  }),
})
```

```js
// src/store.ts

import { configureStore } from '@reduxjs/toolkit'
import { setupListeners } from '@reduxjs/toolkit/query'
import { api } from './services/api'

export const store = configureStore({
  reducer: {
    [api.reducerPath]: api.reducer,
  },
  middleware: gDM => gDM().concat(api.middleware),
})

// enable listener behavior for the store
setupListeners(store.dispatch)
```

### Re-fetching after mutations by invalidating cache tags​ (캐시 태그를 무효화하여 뮤테이션 후 다시 가져오기​)

RTK 쿼리는 선택적 [캐시 태그](https://redux-toolkit.js.org/rtk-query/usage/automated-refetching#cache-tags) 시스템을 사용하여 변형 엔드포인트의 영향을 받는 데이터가 있는 쿼리 엔드포인트에 대한 다시 가져오기를 자동화합니다.

이 개념에 대한 자세한 내용은 [자동 다시 가져오기](https://redux-toolkit.js.org/rtk-query/usage/automated-refetching)를 참조하세요.

## Tradeoffs​

### No Normalized or De-duplicated Cache​ (정규화 또는 중복 제거된 캐시 없음​)

RTK 쿼리는 여러 요청에서 동일한 항목을 중복 제거하는 캐시를 의도적으로 구현하지 않습니다. 여기에는 몇 가지 이유가 있습니다.

- 완전히 정규화된 쿼리 간 공유 캐시는 해결하기 어려운 문제입니다.
- 지금 당장 해결할 시간, 자원 또는 관심이 없습니다.
- 많은 경우 무효화되었을 때 데이터를 다시 가져오는 것만으로도 잘 작동하고 이해하기 쉽습니다.
- 최소한 RTKQ는 많은 사람들에게 큰 골칫거리인 "일부 데이터 가져오기"의 일반적인 사용 사례를 해결하는 데 도움이 될 수 있습니다.

예를 들어 getTodos 및 getTodo 엔드포인트가 있는 API 슬라이스가 있고 구성 요소가 다음 쿼리를 수행한다고 가정해 보겠습니다.

- `getTodos()`
- `getTodos({filter: 'odd'})`
- `getTodo({id: 1})`

이러한 각 쿼리 결과에는 `{id: 1}`과 같은 Todo 개체가 포함됩니다.

완전히 정규화된 중복 제거 캐시에서는 이 Todo 개체의 단일 복사본만 저장됩니다. 그러나 RTK 쿼리는 각 쿼리 결과를 캐시에 독립적으로 저장합니다. 따라서 Redux 저장소에 이 Todo의 3개의 개별 사본이 캐시됩니다. 그러나 모든 엔드포인트가 동일한 태그(예: {type: 'Todo', id: 1})를 일관되게 제공하는 경우 해당 태그를 무효화하면 일치하는 모든 엔드포인트가 일관성을 위해 데이터를 다시 가져오게 됩니다.

Redux 문서는 ID로 항목을 쉽게 찾고 `store`에서 업데이트할 수 있도록 데이터를 [정규화된 조회 테이블](https://redux.js.org/usage/structuring-reducers/normalizing-state-shape)에 보관할 것을 항상 권장했으며 [RTK의 createEntityAdapter](https://redux-toolkit.js.org/api/createEntityAdapter)는 정규화된 상태를 관리하는 데 도움이 되도록 설계되었습니다. 이러한 개념은 여전히 ​​가치가 있으며 사라지지 않습니다. 그러나 RTK 쿼리를 사용하여 캐싱 데이터를 관리하는 경우 직접 데이터를 조작할 필요가 없습니다.

여기에 도움이 될 수 있는 몇 가지 추가 사항이 있습니다.

- 생성된 쿼리 후크에는 구성 요소가 쿼리 결과에서 개별 데이터 조각을 읽을 수 있도록 하는 [selectFromResult 옵션](https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#selectfromresult)이 있습니다. 예를 들어, `<TodoList>` 구성 요소는 `useTodosQuery()`를 호출할 수 있고 각 개별 `<TodoListItem>`은 동일한 쿼리 후크를 사용할 수 있지만 결과에서 선택하여 올바른 할 일 개체를 가져올 수 있습니다.
- [transformResponse](https://redux-toolkit.js.org/rtk-query/api/createApi#transformresponse) 엔드포인트 옵션을 사용하여 가져온 데이터를 수정하여 다른 모양으로 저장되도록 할 수 있습니다(예: `createEntityAdapter`를 사용하여 캐시에 삽입하기 전에 이 응답에 대한 데이터를 정규화하는 것)

### Further information​

- [Reddit: discussion of why RTKQ doesn't have a normalized cache, and tradeoffs](https://www.reddit.com/r/reactjs/comments/my9vrq/redux_toolkit_v16_alpha1_rtk_query_apis/gvxi5t7/)

## Examples

### Cache Subscription Lifetime Demo​ (캐시 구독 생명 주기 데모​)

이 예제는 구독자 참조 수와 `keepUnusedDataFor` 값이 서로 상호 작용하는 방식에 대한 라이브 데모입니다. `Subscriptions` 및 `Queries`(캐시된 데이터 포함)는 시각화할 수 있도록 데모에 표시됩니다(Redux Devtools Extension에서도 볼 수 있음).

동일한 엔드포인트 쿼리(`useGetUsersQuery(2)`)가 있는 두 구성 요소가 각각 탑재됩니다. 구성 요소를 끌 때 구독자 참조 수가 감소하는 것을 관찰할 수 있습니다. 구독자 참조 횟수가 0이 되도록 두 구성 요소를 모두 끈 후 `Queries` 섹션 아래의 캐시된 데이터가 5초 동안 지속되는 것을 관찰할 수 있습니다(이 데모에서 끝점에 대해 제공한 `keepUnusedDataFor` 값). 구독자 참조 수가 전체 기간 동안 0으로 유지되면 캐시된 데이터가 저장소에서 제거됩니다.

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";
import { FullUser, User } from "./types";
import { cacher } from "../rtkQueryCacheUtils";

export const api = createApi({
  baseQuery: fetchBaseQuery({
    baseUrl: "https://jsonplaceholder.typicode.com/"
  }),
  // global configuration for the api
  keepUnusedDataFor: 30,
  tagTypes: [...cacher.defaultTags, "User"],
  endpoints: (builder) => ({
    getUsers: builder.query<User[], number>({
      query: (count) => `users?_start=0&_end=${count}`,
      providesTags: cacher.providesList("User"),
      // configuration for an individual endpoint, overriding the api setting
      keepUnusedDataFor: 5,
      transformResponse: (response: FullUser[]) => {
        const simpleUsers: User[] = response.map(({ id, name }) => ({
          id,
          name
        }));
        return simpleUsers;
      }
    })
  })
});

export const { useGetUsersQuery } = api;
```

[reference](https://codesandbox.io/s/rtk-query-cache-subscription-lifetime-example-77tn4?from-embed)
