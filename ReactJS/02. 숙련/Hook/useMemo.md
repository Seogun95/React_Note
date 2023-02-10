# useMemo?

`useMemo`는 **함수가 반환하는 값 또는 값 그 자체를 캐싱**한다.

동일한 값을 반환하는 함수를 계속 호출해야하면 불필요한 리-렌더링이 계속 발생한다. 맨 처음에 반환해주는 값을 메모리에 저장해두고 가져다 쓰는 것이라면, `이미 저장한 값`을 가져와 사용하는 것이기 때문에 리렌더링이 되지 않는다. 

이러한 기법을 **캐싱을 한다.** 라고 표현하기도 함

## 사용 코드

```jsx
// as-is
const value = 반환할_함수();

// to-be
const value = useMemo(()=> {
	return 반환할_함수()
}, [dependencyArray]);
```

[[의존성 배열]]의 값이 변경될 때만 `반환할_함수()`가 호출 된다. 
**이 외의 경우에는 memoization 해놨던 값을 가져오기만 한다. **

## 적용해보기

### 1번째 예시
`HeavyComponent` 안에서는 `const value = heavyWork()` 를 통해서 value값을 세팅해주고 있어요. 만약 `heavyWork`가 엄청나게 무거운 작업이라면 다른 state가 바뀔 때 마다 계속해서 호출이 되겠죠! 하지만 `useMemo()`로 감싸주게 되면 그럴 걱정이 없다.

> `useMemo`는 무거운 작업(함수)들이 계속 리-렌더링 되는 것을 막기 위해 사용된다. 
> 값을 저장해두고 사용하기 위함..!
> 따라서 [[#주의 사항]]이 있다!

#### App.jsx

```jsx
import "./App.css";
import HeavyComponent from "./components/HeavyComponent";

function App() {
  const navStyleObj = {
    backgroundColor: "yellow",
    marginBottom: "30px",
  };

  const footerStyleObj = {
    backgroundColor: "green",
    marginTop: "30px",
  };

  return (
    <>
      <nav style={navStyleObj}>네비게이션 바</nav>
      <HeavyComponent />
      <footer style={footerStyleObj}>푸터 영역이에요</footer>
    </>
  );
}

export default App;
```

#### components > HeavyComponent.jsx

```jsx
import React, { useState, useMemo } from "react";

function HeavyButton() {
  const [count, setCount] = useState(0);

  const heavyWork = () => { // useMemo를 사용하지 않는다면 리렌더링 될 때마다 호출된다. 
    for (let i = 0; i < 1000000000; i++) {} // 연산 후 
    return 100; // 같은 값을 계속 사용하는 경우라면..!
  };

	// CASE 1 : useMemo를 사용하지 않았을 때
  const value = heavyWork(); // hevaWork()를 통해 값을 받아옴

	// CASE 2 : useMemo를 사용했을 때
  const value = useMemo(() => heavyWork(), []);

  return (
    <>
      <p>나는 {value}을 가져오는 엄청 무거운 작업을 하는 컴포넌트야!</p>
      <button
        onClick={() => { {/* useMemo를 사용하지 않으면 button을 누를 때마다 리렌더링 된다.  */}
          setCount(count + 1);
        }}
      >
        누르면 아래 count가 올라가요!
      </button>
      <br />
      {count}
    </>
  );
}

export default HeavyButton;
```

> 무거운 작업을 하고 나서 그 값을 계속 사용하는 경우라면 `useMemo`를 통해 최적화가 가능하다. 

### 2번째 예시

> 상황 : `me`라는 객체 내에서 `isAlive`의 값이 바뀔 때에만 `console.log("생존여부가 바뀔 때만 호출해주세요!")`를 출력하고 싶다. 하지만 아래 코드를 실행해서 `필요없는 숫자영역`의 `누르면 숫자가 올라가요` 부분을 클릭하게 되면 `useEffect`내의 코드가 실행된다. 의존성 배열에 `me`를 넣었음에도..!


```jsx
import React, { useEffect, useState } from "react";

function ObjectComponent() {
  const [isAlive, setIsAlive] = useState(true);
  const [uselessCount, setUselessCount] = useState(0);

  const me = {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };

  useEffect(() => {
    console.log("생존여부가 바뀔 때만 호출해주세요!");
  }, [me]);

  return (
    <>
      <div>
        내 이름은 {me.name}이구, 나이는 {me.age}야!
      </div>
      <br />
      <div>
        <button
          onClick={() => {
            setIsAlive(!isAlive);
          }}
        >
          누르면 살았다가 죽었다가 해요
        </button>
        <br />
        생존여부 : {me.isAlive}
      </div>
      <hr />
      필요없는 숫자 영역이에요!
      <br />
      {uselessCount}
      <br />
      <button
        onClick={() => {
          setUselessCount(uselessCount + 1);
        }}
      >
        누르면 숫자가 올라가요
      </button>
    </>
  );
}

export default ObjectComponent;
```

### 이유? 

그 이유는 [[12. 불변성 & 순수함수|불변성과 관련]]이 깊다. 
위 예제에서 버튼이 선택돼서 uselessCount state가 바뀌게 되면 
1.  리렌더링
2. 컴포넌트 함수가 새로 호출
3. `me` 객체도 다시 할당 (이 때, 다른 메모리 주소값을 할당)
4. useEffect의 dependency array에 의해 `me` 객체가바뀌었는지 확인해봐야 하는데 
5. 이전 것과 모양은 같은데 주소가 다르다! (`me`객체의 주소 변경)
6. 리액트 입장에서는 `me`가 바뀌었구나 인식하고 `useEffect` 내부 로직이 호출!

### 해결 방안

위와 같은 상황을 해결하기 위해서도 `useMemo`를 사용할 수 있다. 

```jsx
const me = useMemo(() => {
  return {
    name: "Ted Chang",
    age: 21,
    isAlive: isAlive ? "생존" : "사망",
  };
}, [isAlive]);
```

> `me`객체는 메모리에 저장이 되어 있기 때문에 영향을 받지 않게 된다. 

## 주의 사항 

`useMemo`를 남발하게 되면 별도의 메모리 확보를 너무나도 많이 하게 되는 것이기 때문에 오히려 성능이 더 악화될 수 있으므로 **꼭 필요한 상황에서만 사용**해야한다. 