# useRef?
#useRef #hook

```ad-note
title: useRef란?
- DOM 요소에 접근할 수 있도록 하는 hook
```

## useRef hook 소개

JavaScript에서는 DOM 요소에 접근하기 위해서 아래와 같은 코드를 사용했었다. 

```javascript
// (1) getElementById 이용
const divTag = document.getElementById('#myDiv');

// (2) querySelector 이용
const divTag2 = document.querySelector('#myDiv');
```

리액트에서도 DOM을 선택해야 할 상황이 생기기 마련인데, 예를 들어 화면이 렌더링 되자마자 특정 `input` 태그가 focusing이 돼야 하는 경우가 있다. 

이때 DOM 요소에 접근할 수 있도록 하는 React hook이 `useRef`이다. 

## useRef 사용 방법

```jsx
import { React, useRef } from 'react';

function App() {
  const ref = useRef('초기값');
  console.log('ref', ref); // {current: '초기값'} 출력

  return (
    <div>
      <p>useRef</p>
    </div>
  );
}

export default App;

```

![](https://i.imgur.com/N4shvRi.png)

초기값을 변경 할 수 도 있다.

```jsx
import "./App.css";
import { useRef } from "react";

function App() {
  const ref = useRef("초기값");
  console.log("ref 1", ref); // {current: "초기값"}

  ref.current = "바꾼 값";
  console.log("ref 1", ref); // {current: '바꾼 값'}

  return (
    <div>
      <p>useRef에 대한 이야기에요.</p>
    </div>
  );
}

export default App;
```

![](https://i.imgur.com/hKrq8h1.png)

```ad-important
title: 중요
이렇게 설정된 ref 값은 ==컴포넌트가 계속해서 렌더링 되어도 unmount 전까지 값을 유지하게 됨==
```

위와 같은 특징 때문에 useRef의 사용용도는 다음과 같다.

## useRef 사용 용도

### 1. 저장공간에서 사용

1. `state`와 비슷한 역할을 한다. **다만** `state`는 변화가 일어나면 다시 렌더링이 일어나고 내부 변수들은 초기화가 된다.
	* 함수형에서 State가 변화되면 함수가 다시 그려져 내부 변수가 초기화 됨!
2. ==ref 에 저장한 값은 렌더링을 일으키지 않는다==. 즉, `ref`의 값 변화가 일어나도 렌더링으로 인해 내부 변수들이 초기화 되는 것을 막을 수 있다.
3. 컴포넌트가 100번 렌더링 되어도 → `ref`에 저장한 값은 유지

- 정리하자면
	- **State**는 리렌더링이 꼭 필요한 값을 다룰 때 쓰면 된다
	- **ref**는 리렌더링을 발생시키지 않는 값을 저장할 때 사용한다.

### 2. DOM에서 사용

- 렌더링이 되자마자 특정 input이 focusing 돼야 한다면 `useRef`를 사용할 수 있다. 

## Focusing 사용 코드

```jsx
import { React, useEffect, useRef, useState } from 'react';

function App() {
  const idRef = useRef('');
  const pwRef = useRef('');

  const [id, setId] = useState('');

  const changeIdInputHandler = (event) => {
    setId(event.target.value);
  };

  // 렌더링이 될 때 id input에 자동 focusing
  useEffect(() => {
    idRef.current.focus();
  }, []);

  // useEffect 안에 조건!
  // id input에 길이가 10개가 넘어가면 자동으로 pw input으로 focusing
  useEffect(() => {
    if (id.length >= 10) {
      pwRef.current.focus();
    }
  }, [id]);

  return (
    <>
      <div>
        <lable>아이디 : </lable>
        <input
          type="text"
          ref={idRef}
          value={id}
          onChange={changeIdInputHandler}
        />
      </div>
      <div>
        <lable>비밀번호 : </lable>
        <input type="password" ref={pwRef} />
      </div>
    </>
  );
}

export default App;

```

```ad-note
title: useEffect()이 아닌 changeIdInputHandler 내부에 조건을 달면
- 그렇게 된다면 `id.length = 11`이 되었을 때 조건이 발동 될 것이다. 
- 왜냐하면 위의 코드에서 `setId`는 [[DOM? virtual DOM ?#Batch Update|배치 업데이트]]가 되고 있기 때문이다.
```

## input 태그 자동 focusing

useRef를 사용하여 자동 포커스 해주는 방법

```jsx
const inputRef = useRef():
useEffect(() => {
	inputRef.current.focus();
}, [todoList.title])

...

<ToDoInput ref={inputRef} onChange={setToDoText} value={toDoText.title} />
```

```ad-note
보통 값을 저장할 때는 useState를 사용할 텐데, 리액트 컴포넌트는 state가 변할 때 마다 다시 렌더링 되면서 컴포넌트 내부가 초기화 되지만, ==useRef는 값을 저장하기 때문에 Ref안의 값이 변경되어도 다시 렌더링 되지 않고 그대로 유지된다==. 변경 시 렌더링 발생을 막아야 하는 값을 다룰 때 편리하다.
```

