---
title: redux-toolkit으로 로그인 구현하기
date: 2022-02-09 18:02:79
category: movester
thumbnail: { thumbnailSrc }
draft: false
---

공식문서를 참조하며 주관적으로(?) 작성된 코드로 잘못된 부분이 있을 수 있습니다.

## Flow

<img src="https://ko.redux.js.org/assets/images/ReduxAsyncDataFlowDiagram-d97ff38a0f4da0f327163170ccc13e80.gif">

## store 설정

store에 등록할 reducer를 설정합니다.

```js
// configureStore.js
import { configureStore } from '@reduxjs/toolkit'
import adminReducer from './admin/adminSlice'

const store = configureStore({
  reducer: {
    admin: adminReducer,
  },
})

export default store
```

- `configureStore()`: 스토어 설정

## store 적용

설정한 store를 App에 적용합니다.

```jsx
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import { BrowserRouter } from 'react-router-dom'
import { Provider } from 'react-redux'

import App from './App'
import store from './store/configureStore'

ReactDOM.render(
  <Provider store={store}>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </Provider>,
  document.getElementById('root')
)
```

## State Slice 설정

redux-toolkit의 `createSlice`는 내부적으로 immer를 사용해 state의 불변성을 지킵니다.

그리고 redux에서 reducer, action, action-creator를 따로 만들어 관리했다면, redux-toolkit에서는 `createSlice`를 통해서 함께 관리할 수 있습니다.

```js
// adminSlice.js
import { createSlice } from '@reduxjs/toolkit'
import fetchAdminLogin from './adminThunk'

const initialState = {
  isAuth: false,
  admin: null,
}

export const adminSlice = createSlice({
  name: 'admin',
  initialState,
  reducers: {
    logout(state, action) => {
      state.admin = null;
    }
  },
  extraReducers: builder => {
    builder
      .addCase(fetchAdminLogin.fulfilled, (state, action) => ({
        ...state,
        admin: action.payload.data,
      }))
      .addCase(fetchAdminLogin.rejected, state => state)
  },
})

export const {logout} = adminSlice.actions;

export default adminSlice.reducer
```

- `reducers`: 동기적인 action 처리, 내부적 요소
  - `logout`: state의 admin 속성의 값을 null 로 처리해 로그아웃 구현(동기적)
- `extraReducers`: 비동기적인 action 처리, 외부적 요소
  - **Promise** 객체의 상태에 따른 로직을 설정
    - pending, fullfilled, rejected

## thunk 설정

```js
// adminThunk.js
import { createAsyncThunk } from '@reduxjs/toolkit'
import axios from 'axios'

const fetchAdminLogin = createAsyncThunk(
  'admin/login',
  async (payload, thunkAPI) => {
    try {
      const response = await axios.post(
        'http://localhost:8000/api/admin/login',
        {
          email: payload.email,
          password: payload.password,
        },
        { withCredentials: true }
      )

      return response.data
    } catch (err) {
      return thunkAPI.rejectWithValue(err.response.data)
    }
  }
)

export default fetchAdminLogin
```

- `createAsyncThunk`: 파라미터 값으로 action-type과 콜백함수를 전달
- `rejectWithValue`: 내부적으로 error를 값으로 저장

## dispatch 설정

```jsx
const onClick = async () => {
  try {
    const originalPromiseResult = await dispatch(
      fetchAdminLogin({
        id: 'admin1',
        password: 'admin12',
      })
    ).unwrap()
    console.log(originalPromiseResult)
  } catch ({ error }) {
    setErr(error)
  }
}
```

- dispatch에 fetchAdminLogin 함수를 태워, fetchAdminLogin의 결과에 따라 extraReudcer에서 store의 상태를 변경
- `unwrap`: 디스패치된 thunk 함수가 반환한 promise 객체는 unwrap 속성을 가지고 있고, fullfilled 상태일때 payload와 rejected 상태일때 error를 추출
