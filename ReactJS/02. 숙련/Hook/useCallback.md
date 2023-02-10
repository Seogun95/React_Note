# useCallback?
#useCallback

```ad-note
title: useCallback?
- [[React.memo]]는 컴포넌트를 메모이제이션 했다면, `useCallback`은 인자로 들어오는 함수 그 자체를 메모리에 저장(memoization)한다. 
```

## 코드로 살펴보기

➜ [[React.memo#코드로 살펴보기|React.memo에서 사용한 코드 사용]]

React.memo에서 사용한 코드를 그대로 가져와, ==초기화== 라는 버튼을 생성하고 이 버튼은 App.js 에서만 관리하도록 한다. 

App.js안에 count State가 있기 때문에 App.js 에서 관리하는 것!

App.js
```jsx
import React, { useState } from 'react';
...

const App = () => {
  console.log('App 컴포넌트가 렌더링 되었다.');
	...
  const initCount = () => {
    setCount(0);
  };

  return (
    <>
      <h1>서근 카운트</h1>
      <p>현재 카운트: {count}</p>
      <button onClick={countUpBtnHandler}>+</button>
      <button onClick={countDownBtnHandler}>-</button>
      <FlexDiv>
        <Box1 initCount={initCount} />
        <Box2 initCount={initCount} />
        <Box3 initCount={initCount} />
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
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100px;
  height: 100px;
  color: white;
  border-radius: 1rem;
  background-color: tomato;
`;

const Box1 = ({ initCount }) => {
  console.log('Box1 컴포넌트가 렌더링 되었다.');
  return (
    <>
      <MyBox>
	<button onClick={() => initCount()}>초기화</button>
		Box1
      </MyBox>
    </>
  );
};

export default React.memo(Box1);
```

<img src="https://i.imgur.com/O2dCAUK.gif" width="250" />

분명 React.memo를 사용했는데 아래 조건 모두에서 리-렌더링이 발생한다.
- `+ 버튼`이나, `- 버튼`을 누를 때 
- `초기화` 버튼을 누를 때 

![](https://i.imgur.com/cpguIPC.png)

### 메모이제이션을 했는데도 리-렌더링이 되는 이유?

- 함수형 컴포넌트를 사용하기 때문에 App.jsx가 리-렌더링이 되면서 코드가 다시 만들어지기 때문.
- 자바스크립트에서는 ==함수도 객체의 한 종류==이기 때문에 저장 될 때 메모리에 직접적으로 저장 되는것이 아니라 별도의 공간에 저장되고, 그 별도의 공간을 바라보고 있는 그 주소값을 저장한다. 
- 즉, State가 변경되면서 ==함수가 새로운 주소값을 갖게 되면서== 다시 return 되는것! 
- 이에 따라 하위 컴포넌트인 Box1.jsx는 props가 변경됐다고 리액트에서 인식하게 된다.
- 위의 코드에서 예를 들자면 App.jsx에 있는 initCount도 App.jsx가 리렌더링 될 때 다시 만들어지게 된다.
- 따라서 Box1.jsx에서는 props(initCount)가 변경 됐다고 인식하는 것이다.
- 이를 해결하기 위해 **useCallback를 통해 함수 자체를 메모이제이션 하는 방법**이 필요하다.

## useCallback을 통한 함수 메모이제이션

```jsx
import React, { useCallback, useState } from 'react';

// 변경 전
const initCount = () => {
  setCount(0);
};

// 변경 후
const initCount = useCallback(() => {
setCount(0);
}, []);
```

이렇게 되면 Box1.jsx 컴포넌트는 리렌더링이 되지 않는다. 
1. App.jsx가 처음 렌더링이 될 때, 메모리 공간에 `initCount`함수를 메모리공간에 그래도 저장한다. 
2. [[의존성 배열]]안에 아무것도 없기 때문에 끝까지 변하지 않고 setCount 그 자체로 그대로 있다.
3. 새로 함수가 리-렌더링이 되더라도 주소값이 새롭게 갱신되는 것이 아니라 메모이제이션 된 상태로 남아있다. 최초의 props만 계속 내려 받게 된다.

<img src="https://i.imgur.com/3dErrox.gif" width="250" />

## 또 다른 문제

count를 초기화 할 때, 콘솔을 찍어보자면, 아래와 같은 오류를 볼 수 있다.

![](https://i.imgur.com/0QaQXzi.png)

```jsx
...
const initCount = useCallback(() => {
	console.log(`값이 ${count}에서 0으로 초기화 되었다`)
	setCount(0);
}, []); 
...
```

위 코드의 예상 결과는 count를 증가, 감소 시킨만큼 그대로 찍힐 것 같지만 아무리 해도 `0`만 출력될 것이다. 

```ad-note
- 위와 같은 현상이 발생하는 이유는 useCallback이 count가 0일 때의 시점을 기준으로 메모리에 함수를 저장했기 때문! 
- 그렇기에 [[의존성 배열|의존성 배열(dependency array)]]이 필요하게 된다. 
- 기존 코드의 dependency array에 count를 넣으면, count가 변경이 될 때마다 새롭게 함수를 할당하게 된다. 
```

- 이 현상을 방지하려면, count가 변경 될 때 만큼은 useCallback으로 인해 함수가 다시 저장되어야 하는것이다. 
	- 왜냐? count가 변경 될 때 콘솔도 변경되야 되니까 이때 의존성 배열에 값이 들어가야 하는것이다.

```jsx
...
const initCount = useCallback(() => {
	console.log(`값이 ${count}에서 0으로 초기화 되었다`)
	setCount(0);
}, [count]); 
...
```

<img src="https://i.imgur.com/hGIVwtz.gif" width="250" />
