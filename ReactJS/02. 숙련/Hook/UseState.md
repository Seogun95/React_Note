# useState?
#useState #hook 

```ad-summary
title: UseState란?
 - useState는 가장 기본적인 [[Hook|hook]]이며, 함수 컴포넌트에서 가변적인 상태를 가지게 해준다.
 - React에서 정의하는 `state`를 Fuction Component에서 사용하게 해주는 hook 이다.
 - 클래스형 컴포넌트에서 `this.state`와 `this.setState`를 대체 한다.
```

## UseState 기본 형태

```jsx
const [value, setValue] = useState(0);
```

- `value`, `setValue` 
	-  `value`  : 관리할 value(변수 선언)
	* `setValue` : value를 관리, value를 재선언 할 수 있다/
- 주의
	* `value` 가 원시 데이터타입이 아닌 객체 데이터 타입 (주소를 참조하는)인 경우에는 [[12. 불변성 & 순수함수#1. 불변성 이란?|불변성]]을 유지 해주어야 한다.

## UseState 리렌더링 방식

기존 업데이트 방식과 함수형 업데이트 방식을 간단히 알아보자면 다음과 같다.

```jsx
// 기존에 사용하던 방식 
setState(number + 1); 
// 함수형 업데이트 
setState(() => {});
// 현재 number의 값을 가져와서 그 값에 +1을 더하여 반환하는 방법
setState((currentNumber)=>{ return currentNumber + 1 });
```

### 기존의 업데이트 방식

```jsx
import { React, useState } from 'react';

function App() {
  const [counter, setCounter] = useState(0);

  const addCountHandler = () => {
    //3번의 setState를 사용했지만, 1씩만 추가 된다.
    setCounter(counter + 1);
    setCounter(counter + 1);
    setCounter(counter + 1);
  };
  return (
    <>
      <div>
        <p> 버튼을 누른 횟수 {counter}</p>
        <button onClick={addCountHandler}>카운트</button>
      </div>
    </>
  );
}
export default App;
```

이때 기대되는 값은 `setNumber`가 3번이 실행이 되었으므로 `number`가 **3** 이 될 것이라고 예상할 수 있지만, 값은 1씩만 추가된다.

![](https://i.imgur.com/y0UFQjE.gif)

### 함수형 업데이트 방식

```jsx
// src/App.js

import { useState } from "react";

function App() {
  const [counter, setCounter] = useState(0);

  const addCountHandler = () => {
    setCounter((currentNum) => currentNum + 1);
    setCounter((currentNum) => currentNum + 1);
    setCounter((currentNum) => currentNum + 1);
  };
  return (
    <>
      <div>
        <p> 버튼을 누른 횟수 {counter}</p>
        <button onClick={addCountHandler}>카운트</button>
      </div>
    </>
  );
}

export default App;
```

함수형 업데이트 방식으로 `setState((currentState) => currentState + 1)` 처럼 버튼에 함수를 넣어주면 아래 결과화면 처럼 +3 씩 카운트가 올라가는것을 확인할 수 있다.

![](https://i.imgur.com/SgpngFS.gif)

### 다르게 동작하는 이유

**일반 업데이트 방식**은 버튼을 클릭했을 때 첫번째 부터 세번째줄의 있는 setCounter가 각각 실행되는 것이 아니라, #배치성(batch)로 처리한다.

> [!note] 배치성? 
> 데이터를 실시간으로 처리하는것이 아닌, 일괄적으로 모아 처리하는 작업

**즉,  onClick을 했을 때 setCounter 라는 명령을 세번 내리지만, 리액트는 그 명령을 하나로 모아 최종적으로 한번만 실행**을 시킨다!

그래서 setCounter을 3번 명령하던, 100번 명령하던 1번만 실행하게 되는 것이다.

반면,  **함수형 업데이트 방식**은 **3번을 동시에 명령을 내리면, 그 명령을 모아 순차적으로 각각 1번씩 실행**시킨다. 

0에 1더하고, 그 다음 1에 1을 더하고, 2에 1을 더해서 3이라는 결과가 리렌더링 된다.

### 왜 리액트는 위 방식으로 동작할까?

```ad-info
title: 공식문서
* 리액트는 성능을 위해 단일 업데이트(batch update)로 한꺼번에 처리함.
* 즉, 불필요한 리-렌더링을 방지(렌더링 최적화)를 위해서 한꺼번에 state를 모아놓고 같은 state를 같은 방식으로 처리한다면 딱 한번만 실행하게 된다.
* 하지만 함수형 업데이트를 사용하면 `setState()`에서 인자를 받을 때 이전 값(`previousState`)을 받고 그 값에 대한 것을 실행 하기 때문이다.
```

## 요약

1. useState의 업데이트 방식은 기존방식과 함수형 방식으로 총 2가지이며 각각 다르게 동작한다.
2. useState로 원시 데이터가 아닌 참조형 데이터를 변경할 때는 불변성을 신경써야한다. 

