# UseEffect?
#useEffect #hook 

```ad-note
title: useEffect?
- `useEffect()`는 리액트 컴포넌트가 렌더링이 될 때 마다 특정 작업을 수행하도록 설정할 수 있는 hook
- 쉽게 말해 ==어떤 컴포넌트가 화면에서 사라졌을 때 무언가를 실행하고 싶다면?== `useEffect()`를 사용
- useEffect는 화면이 처음 렌더됐을때도 동작하지만 화면에서 사라질때도 동작한다.
- 보통 API를 한 번만 불러오고 싶을 때 사용한다.
```



## useEffect 사용방법

```jsx
// src/App.js

import React, { useEffect } from "react";

const App = () => {

  useEffect(() => {
		// 이 부분이 실행된다.
    console.log("hello useEffect");
  });

  return <div>Home</div>;
}

export default App;
```

> 브라우저에서 우리가 App 컴포넌트를 눈으로 보는 순간, 즉 App 컴포넌트가 최초에 화면에 렌더링이 될 때 `useEffect`안에 있는 `console.log()`가 실행된다. 
> 
>  ==input 태그에 값이 입력 될 때마다 렌더링이 되고 있다== 가 바로 핵심 기능이다. 

## useEffect 과정

1. input에 값을 입력
2. value, 즉 state가 변경
3. state가 변경 되었기 때문에 ➜ App 컴포넌트가 리렌더링
4. 리렌더링 ➜ useEffect() 
5. 1~4 번 과정 반복

![](https://i.imgur.com/HU3qAYt.gif)

```jsx
import { React, useEffect, useState } from 'react';

function App() {
  const [value, setValue] = useState('');
  useEffect(() => {
    console.log('hello useEffect');
  });

  return (
    <div>
      <input type="text" value={value} onChange={(e) => setValue(e.target.value)} />
    </div>
  );
}
export default App;

```

## useEffect와 리렌더링(re-rendering)

 `useEffect`는 화면이 렌더링 될 때마다 실행된다. 따라서 `useEffect()`의 의존성 배열을 활용해서 우리가 원할 때만 코드가 실행될 수 있도록 설정할 수 있다. 

### 의존성 배열?

- [[의존성 배열|의존성 배열(dependency array)]]이란 쉽게 말해 **"이 배열에 값(`state`)을 넣었을 때만 `useEffect`내의 코드를 실행해줘"** 라는 의미

useEffect가 끝나는 부분 뒤에 두번째 인자로 의존성 배열을 넣어줘야 한다.

참고로 의존성 배열에는 원하는 State를 여러개 넣어줄 수 도 있다.

```jsx
// useEffect의 두번째 인자가 의존성 배열이 들어가는 곳 입니다.
useEffect(()=>{
	// 실행하고 싶은 함수
}, [의존성배열])
```

의존성 배열에 값이 **없을때**

![](https://i.imgur.com/TDZoTYL.gif)

의존성 배열에 값이 **있을때**

![](https://i.imgur.com/5O0TUst.gif)

- 비어있는 의존성 배열일 때는 **최초 렌더링이 될 때 단 한번만 실행**하게 된다.
	- 어떤 값을 입력하던 간에 의존성 배열에는 값이 없기 때문에 어떤 State가 변해도 화면이 처음 로딩 될때만 동작 한다!
- 의존성 배열에 값이 있을 때는 배열 안에 있는 값이 바뀔 때만, 위의 코드에서는 **`value`가 변경이 돼서 렌더링이 될 때에만 실행** 하게 된다.

아래 코드는 useEffect를 통해 작성된 콘솔로그는 의존성 배열이 없기 때문에 첫 렌더 시에만 실행되는데, 버튼이 클릭되어 카운트가 올라가 State가 변경되어도 useEffect 부분은 리-렌더 되지 않는다.

```jsx
import { useEffect, useState } from 'react';

function App() {
  const [count, setCount] = useState(0);
  const countUpHandler = () => {
    setCount((prev) => prev + 1);
    console.log('버튼이 클릭 될때마다 실행되는 콘솔');
  };

  useEffect(() => {
    console.log('첫 렌더가 일어날때만 실행되는 콘솔');
  }, []);

  return (
    <div>
      <p>{count}</p>
      <button onClick={countUpHandler}>증가</button>
    </div>
  );
}

export default App;

```

### useEffect 의존성 배열 예시

만약 button 핸들러와, Input 핸들러가 있다고 가정했을때, 각각의 State만 변화할 때만 콘솔로그가 찍히게 하고싶다면 useEffect의 클린업을 활용할 수 있다.

우선, 클린업을 사용하지 않을때를 보자면, 인풋에 변화가 감지되던 버튼이 감지되던 State가 바뀔때마다 전체가 리-렌더되는것을 확인할 수 있다.

```jsx
function App() {
  const [count, setCount] = useState(0);
  const [inputTitle, setInputTitle] = useState('');
  const countUpHandler = () => {
    setCount((prev) => prev + 1);
  };
  console.log('버튼클릭 --->', count);

  const inputChangeHandler = (e) => setInputTitle(e.target.value);
  console.log('Input 변화 --->', inputTitle);

  return (
    <div>
      <p>{count}</p>
      <button onClick={countUpHandler}>증가</button>
      <input value={inputTitle} onChange={inputChangeHandler} />
    </div>
  );
}
```

![](https://i.imgur.com/8opzaVD.gif)

그런데 만약 버튼을 클릭할때는 input의 State가 렌더되지 않게하려면 useEffect의 의존성 배열에 input의 state를 넣어주게 되면 된다.

```jsx
  const inputChangeHandler = (e) => setInputTitle(e.target.value);

  useEffect(() => {
    console.log('Input 변화 --->', inputTitle);
  }, [inputTitle]);

```


![](https://i.imgur.com/EYcM9t5.gif)

여기서 문제점이 있는데, 첫 렌더시 콘솔창을 보면 Input 변화 ---> 라는 콘솔이 찍히는것을 알 수 있다. Input에 아무런 값이 없는데도 말이다. 

이것을 방지하려면 if문을 통해 만약 InputTitle이 빈값이 아닐때를 판별해줄 수 있다.

```jsx
  useEffect(() => {
    if (inputTitle !== '') {
      console.log('Input 변화 --->', inputTitle);
    }
  }, [inputTitle]);
```

그리고 만약 inputTitle의 길이가 5이하일 때는 실행하지 않게 할 수 도 있다.

```jsx
  useEffect(() => {
    if (inputTitle !== '' && inputTitle.length > 5) {
      console.log('Input 변화 --->', inputTitle);
    }
  }, [inputTitle]);
```

![](https://i.imgur.com/6EEI2jt.gif)

추가로 의존성 배열에는 원하는 State를 여러개 넣어줄 수 도 있다.

아래 코드는 첫 렌더시 모든 콘솔이 찍히지만, 특정 State가 변할때만 해당 콘솔이 찍히도록 해주는 코드이다.
 
```jsx
  useEffect(() => {
    console.log('Dom이 첫 렌더링 되었습니다!');
  }, []);

  useEffect(() => {
    console.log('input값이 변경 되었다 --->', inputTitle);
  }, [inputTitle]);

  useEffect(() => {
    console.log('button값이 변경 되었다 --->', count);
  }, [count]);
```

![](https://i.imgur.com/yDnhTqd.gif)

의존성 배열에 여러 State를 넣어 준다면 아래처럼 해줄 수 있다.

```jsx
useEffect(() => {
	console.log(
`input값과 button값이 변경 되었다 input:${inputTitle} button:${count}`
	);
}, [inputTitle, count]);
```

![](https://i.imgur.com/G9mPaAI.gif)

## 클린업 (clean up)

```ad-note
title: 클린업
"==컴포넌트가 화면에서 사라졌을 때 무엇을 실행=="하고 싶다면 **clean up**을 사용하면 된다. 
- useEffect는 화면이 처음 렌더됐을때도 동작하지만 화면에서 사라질때도 동작하는데 이것이 바로 클린 업 이다.
```

### 클린 업 사용 방법

클린 업을 사용하려면, useEffect 안에서 return문을 함수형태로 작성하면 그안의 로직이 컴포넌트가 죽을때 작동하는 부분이다.

> 컴포넌트가 죽을때가 뭐지?
> - 우리는 React를 사용할때 보통 함수형 컴포넌트를 사용하는데, 함수형 컴포넌트는 State의 변화가 감지할때 리-렌더링을 하면서 위에서 부터 아래로 전부 리-렌더링이 되게 된다. 왜냐? Single Page Application이기 때문에.
> - 이것을 컴포넌트가 죽는다고 표현한것이다.

```jsx
// src/App.js
import { React, useEffect } from 'react';

function App() {
  useEffect(() => {
    // 화면에 컴포넌트가 나타났을(mount) 때 실행하고자 하는 함수

    return () => {
      // 화면에서 컴포넌트가 사라졌을(unmount) 때 실행하고자 하는 함수
    };
  }, []);

  return <div></div>;
}
export default App;
```

### 클린 업 동작 과정

`useEffect` 훅의 두번째 파라미터로 빈 배열이 들어갔기 때문에 리액트 컴포넌트가 제거되는 시점에 클린-업 함수가 실행된다. 명확하게 `componentWillUnmount` 메소드가 호출되는 동일한 시점에 클린-업 함수가 실행됩니다.

반면에 `useEffect` 훅의 두번째 파라미터에 아무 값도 넣어주지 않으면 다음과 같은 순서로 실행됩니다.

1.  props/state 업데이트
2.  컴포넌트 리-렌더링
3.  **이전 이펙트의 클린-업 함수**
4.  새로운 이펙트 실행

### 클린업 동작 예시 1

아래 코드는 버튼이 클릭 되었을때, Seogun 이라는 컴포넌트가 실행되게 하는 코드이다.

```jsx
const Seogun = () => {
  return <h1>서근개발노트!</h1>;
};
function App() {
  const [showing, setShowing] = useState(false);
  const btnHandler = () => {
    setShowing((prev) => !prev);
  };
  return (
    <div>
      <button onClick={btnHandler}>{showing ? '숨기기' : '보여주기'}</button>
      {showing ? <Seogun /> : null}
    </div>
  );
}
```

![](https://i.imgur.com/Zk2LEDG.gif)

위 버튼에 삼항연산자로 showing이 false일때와 true일때를 정해줬는데, fasle라면 null로 컴포넌트를 사라지게 했다. 결과물을 보면 showing이 true일때만 콘솔로그가 찍히는것을 확인할 수 있다.

```jsx
const Seogun = () => {
  useEffect(() => {
    console.log('showing이 true일때만 호출 되는 컴포넌트 입니다.');
  }, []);
  return <h1>서근개발노트!</h1>;
};
```

![](https://i.imgur.com/6XvlAxF.gif)

여기 까지가 useEffect를 사용하는 기본적인 방법인데, 그럼 클린업은 무엇일까?

위 코드에서 showing이 true일 때 즉, create될 때만 콘솔이 찍히게 했는데, 만약 컴포넌트가 죽을때(사라질때) 도 찍히게 하려면 이 클린업을 사용할 수 있다.

```jsx
const Seogun = () => {
  useEffect(() => {
    console.log('나타났다!!');
    return () => console.log('사라졌다!!');
  }, []);
  return <h1>서근개발노트!</h1>;
};
```

![](https://i.imgur.com/PlleQQB.gif)

위코드를 실질적으로 사용하는 방법은 함수를 사용하는것이다.

결국, 위 코드와 동일하게 작동 된다.

```jsx
const Seogun = () => {
  const creteFunc = () => {
    console.log('나타났다!!');
    return removeFunc;
  };
  const removeFunc = () => {
    console.log('사라졌다!!');
  };
  useEffect(creteFunc, []);
  return <h1>서근개발노트!</h1>;
};
```

### 클린업 동작 예시 2

아래 코드는 속세를 벗어나는 버튼 을 만들고, 버튼을 누르면 `useNavigate`에 의해서 `/todos`로 이동하면서 속세 컴포넌트를 떠나게 된다.

그러면서 화면에서 속세 컴포넌트가 사라질 것 이고, `useEffect`의 `return` 부분이 실행되는 로직이다.

```jsx
// src/SokSae.js
import React, { useEffect } from "react";
import { useNavigate } from "react-router-dom";

const SokSae = () => {
  const nav = useNavigate();

  useEffect(() => {
    return () => {
      console.log(
        "안녕히 계세요 여러분! 전 이 세상의 모든 굴레와 속박을 벗어 던지고 제 행복을 찾아 떠납니다! 여러분도 행복하세요~~!"
      );
    };
  }, []);

  return (
    <button
      onClick={() => {
        nav("/todos");
      }}
    >
      속세를 벗어나는 버튼
    </button>
  );
};

export default SokSae;
```

![](https://i.imgur.com/eWr237U.gif)

`/`에서 `/todos` 잘 이동했고, 그 과정에서 clean up이 실행되었다. 

clean up은 hook을 배워가면서 알아 갈 예정이다.

```ad-note
`const nav = useNavigate();` ← 이 기능은 나중에 우리가 배울 기능이다.다른페이지로 이동해서 컴포넌트가 사라지고, 그에 따라 클린 업이 실행되는 과정에만 이해하면 된다.
```

## 요약
- `useEffect`는 화면에 컴포넌트가 mount 또는 unmount 됐을 때 실행하고자 하는 함수를 제어하게 해주는 Hook
- 의존성 배열을 통해 함수의 실행 조건을 제어할 수 있고, 딱 한번만 실행하고자 할 때는 의존성 배열을 빈 배열로 둔다
