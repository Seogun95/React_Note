# React.memo?

React.memo는 [[리-렌더링 및 최적화#리-렌더링의 발생 조건|리-렌더링 조건]]과 [[리-렌더링 및 최적화#최적화|최적화]]에 대해 사전에 숙지 하고 있어야 한다.

==리-렌더링 발생 조건==
1.  컴포넌트에서 State가 바뀌었을 때
2.  컴포넌트가 내려받은 props가 변경 되었을때
3.  부모 컴포넌트가 리-렌더링 된 경우 자식 컴포넌트는 모두 리렌더링

## React.memo

React.memo 란
- 리-렌더링의 발생 조건 중 3번째 경우. 즉, ==부모 컴포넌트가 리렌더링 되면 자식컴포넌트는 모두 리렌더링 된다는 것==은 아래 그림으로 쉽게 확인 가능하다.

<img src="https://i.imgur.com/XyFFYq7.png" height="200px">

-   1번 컴포넌트가 리렌더링 된 경우, 2~7번이 모두 리렌더링 된다.
-   4번 컴포넌트가 리렌더링 된 경우, 6, 7번이 모두 리렌더링 된다.
라는 의미과 같다.

자녀 컴포넌트의 입장에서는 “**나는 바뀐게 없는데 왜 다시 렌더링 돼야하지?**”라고 할 수 있다. 이 부분을 돕는 도구가 바로 ==React.memo==

## 코드로 살펴보기

### 디렉토리 구성

<img src="https://i.imgur.com/H9JKMS8.png" height="200px">

### 구현할 화면

<img src="https://i.imgur.com/Jiag0Xr.gif" height="200px" />

App.js
```jsx
import React, { useState } from 'react';
import styled from 'styled-components';
import Box1 from './components/Box1';
import Box2 from './components/Box2';
import Box3 from './components/Box3';

const FlexDiv = styled.div`
  display: flex;
  gap: 1rem;
  margin: 1rem;
`;

const App = () => {
  console.log('App 컴포넌트가 렌더링 되었다.');
  const [count, setCount] = useState(0);
  const countUpBtnHandler = () => {
    setCount(count + 1);
  };

  const countDownBtnHandler = () => {
    setCount(count - 1);
  };

  return (
    <>
      <h1>서근 카운트</h1>
      <p>현재 카운트: {count}</p>
      <button onClick={countUpBtnHandler}>+</button>
      <button onClick={countDownBtnHandler}>-</button>
      <FlexDiv>
        <Box1 />
        <Box2 />
        <Box3 />
      </FlexDiv>
    </>
  );
};

export default App;

```

Box1.jsx
```jsx
import React from 'react';
import styled from 'styled-components';

const MyBox = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  color: white;
  border-radius: 1rem;
  background-color: tomato;
`;

const Box1 = () => {
  console.log('Box1 컴포넌트가 렌더링 되었다.');
  return <MyBox>Box1</MyBox>;
};

export default Box1;

```

Box2.jsx
```jsx
import React from 'react';
import styled from 'styled-components';

const MyBox = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  color: white;
  border-radius: 1rem;
  background-color: gray;
`;

const Box2 = () => {
  console.log('Box2 컴포넌트가 렌더링 되었다.');
  return <MyBox>Box2</MyBox>;
};

export default Box2;

```

Box3.jsx
```jsx
import React from 'react';
import styled from 'styled-components';

const MyBox = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  color: white;
  border-radius: 1rem;
  background-color: blueviolet;
`;

const Box3 = () => {
  console.log('Box3 컴포넌트가 렌더링 되었다.');
  return <MyBox>Box3</MyBox>;
};

export default Box3;

```

## 버튼 클릭을 통해 리-렌더링 

이제, 카운트가 올라가는 버튼을 클릭해 콘솔로그를 통해 어떻게 리-렌더링이 되었는지 확인해보자.

카운트 화면을 보면, count State만 변경했는데, 그 하위에 있는 자식 컴포넌트인 Box 컴포넌트가 전부 리-렌더링이 발 생하는것을 확인할 수 있다.

즉, 모든 하위 컴포넌트가 리렌더링 되고 있는것! 실제로 변한 것은 부모컴포넌트, App.jsx 뿐인데도 말이다.

<img src="https://i.imgur.com/SXXutPq.gif" width="250" />

## memo를 통한 해결 방법

```ad-note
title: memo를 통한 해결 방법
- 이렇게 불필요한 부분까지 리-렌더링 되는것을 막아줄 수 있는 것이 바로 `memo` 이다.
- 간단하게 React.memo를 이용해 컴포넌트를 메모리에 저장해두고 필요할 때 갖다 쓰기만 하면 된다.
- 이렇게 하면 부모 컴포넌트의 state의 변경으로 인해 **props가 변경이 일어나지 않는 한 컴포넌트는 리렌더링 되지 않는다.** 이것을 컴포넌트 `memoization` 이라고 한다.
```

1. Box 컴포넌트의 export 부분에 다음과 같이 React.memo를 추가
```jsx
export default React.memo(Box1); 
export default React.memo(Box2); 
export default React.memo(Box3);
```

2. 이렇게 되면 첫 렌더이외에는 App.js 컴포넌트의 state가 변경되어도 자식 컴포넌트는 리-렌더링 되지 않는다.

<img src="https://i.imgur.com/GUKcCNL.gif" width="250" />

