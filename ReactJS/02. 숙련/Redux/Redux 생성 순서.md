# Redux 생성 순서 코드스니펫

1. `Redux` 설치하기

```bash
yarn add redux react-redux
```

2. `src` 폴더에 `redux` 폴더 구성하기

```
📦src  
 ┣ 📂redux  
 ┃ ┣ 📂config  
 ┃ ┃ ┗ 📜configStore.js  
 ┃ ┣ 📂modules
 ┃ ┃ ┗ 📜todoReducer 
 ┃ ┗ 📜.DS_Store 
 ┣ 📂pages  
 ┃ ┗ 📜Home.jsx  
 ┣ 📂shared  
 ┃ ┗ 📜Router.js  
 ┣ 📜.DS_Store  
 ┣ 📜App.css  
 ┣ 📜App.js  
 ┗ 📜index.js
```

3. `configStroe.js` 연결하기 

`src/redux/config/configStore.js`

```jsx
// 중앙 데이터 관리소(store)를 설정하는 부분
import { createStore } from "redux";
import { combineReducers } from "redux";

const rootReducer = combineReducers({
 //이곳에 Modules 폴더에 넣어놓은 state의 묶음들을 넣을 예정
 //즉, 리듀서
}); 
const store = createStore(rootReducer); 

export default store; 
```

4. `index.js`에 스토어 뿌려주기 

`./index.js`

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
// Provider import
import { Provider } from 'react-redux'; 
//주의: 반드시 config에서 가져와야 한다. 정확한 경로 필수
//tip: export default로 작성했을때는 import시 { store }로 작성하면 안된다.
import store from './redux/config/configStore';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  //Provider:
  //스토어에서 만들어놓은 중앙데이터 관리소를 App 컴포넌트 하부에서 스토어를 가능하게 해줌
  <Provider store={store}>
    <App />
  </Provider>
);
```

5. 리듀서 모듈 생성하기

`src/redux/modules/리덕스모듈이름.jsx`

```jsx
// 1. Actions
const CREATE = "todo/CREATE";

// 2. Action Creators
//create
export function addTodo(title, desc) {
  return {
    type: CREATE,
    title,
    desc,
  };
}

// 3. 초기 상태값
const initialState = {

};

// 4. Reducer
export default function reducer(state = initialState, action = {}) {
  switch (action.type) {
    case CREATE:
      return 
    default:
      return state;
  }
}
```

6. `react-router-dom` 설치하기

```bash
yarn add react-router-dom 
```

7. `Router` 파일 생성

`src/shared/Rounter.js`

```jsx
import React from "react";
// 1. react-router-dom을 사용하기 위해서 아래 API들을 import 한다.
import { BrowserRouter, Route, Routes } from "react-router-dom";

// 2. Router 라는 함수를 만들고 아래와 같이 작성한다.
//BrowserRouter를 Router로 감싸는 이유는, 
//SPA의 장점인 브라우저가 깜빡이지 않고 다른 페이지로 이동할 수 있게 만들어준다!
const Router = () => {
  return (
    <BrowserRouter>
      <Routes>
      </Routes>
    </BrowserRouter>
  );
};

export default Router;
```

8. `App.js`에 `Router.js` import 해주기

`./App.js`

```jsx
import React from "react";
import Router from "./shared/Router";

function App() {
  return <Router />;
}

export default App;
```

9. `page` 폴더 내의 `Home.jsx` 뼈대 구성

`src/pages/페이지이름.js`

```jsx
import React from 'react'

const Home = () => {
  return (
			<>
    	</>
  )
}

export default Home
```

10. 스타일드 컴포넌트 설치

```bash
yarn add styled-components
```

