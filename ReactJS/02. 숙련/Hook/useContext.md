# useContext
#useContext #hook

```ad-note
title: useContext
우선 context는 전역적으로 사용되는 어떠한 것을 표현할 때 보통 context라는 말을 사용한다. 
```

## useContext 의 필요성

이전 까지 일반적으로 부모컴포넌트 → 자식 컴포넌트로 데이터를 전달해 줄 때 [[08. props|props]]를 사용했었다. 

그러나 `부모` → `자식` → `그 자식` → `그자식의 자식` 이렇게 너무 깊어지게 되면  [[08. props#Prop drilling|prop drilling]] 현상이 일어나게 된다. 

### prop drilling의 문제점

1. 깊이가 너무 깊어지면 이 prop이 어떤 컴포넌트로부터 왔는지 파악이 어려움
2. 어떤 컴포넌트에서 오류가 발생할 경우 추적이 힘들어지니 대처가 늦게 된다.

![](https://i.imgur.com/8CsOJnj.png)
<p style="text-align: center">출처 : <a href="https://www.copycat.dev/blog.react-context">https://www.copycat.dev/blog.react-context</a></p>

그래서 등장한 것이 react context API라는 것이고, useContext hook을 통해 쉽게 `전역 데이터를 관리`할 수 있게 된다. 

## context API 필수 개념

- `createContext` : context 생성
- `Consumer` : context 변화 감지
- `Provider` : context 전달 (to 하위 컴포넌트)

## useContext를 사용하지 않을 때
useContext를 사용하지 않고 prop drilling 만을 이용해 코드를 보자면

<img height="200" src="https://i.imgur.com/YXa2zkW.png"/>

App.jsx
```jsx
import React from 'react';
import GrandFather from './components/GrandFather';

const App = () => {
  return <GrandFather />;
};

export default App;
```

GrandFather.jsx
```jsx
import React from 'react';
import Father from './Father';

// GF -> Ghild에게 정보를 전달하고 Child가 내용을 출력
const GrandFather = () => {
  const houseName = '스파르타';
  const pocketMoney = 10000;
  return <Father houseName={houseName} pocketMoney={pocketMoney} />;
};

export default GrandFather;

```

Father.jsx
```jsx
import React from 'react';
import Child from './Child';

const Father = ({ houseName, pocketMoney }) => {
  return <Child houseName={houseName} pocketMoney={pocketMoney} />;
};

export default Father;

```

Child.jsx
```jsx
import React from 'react';
import styled from 'styled-components';

const MySpan = styled.span`
  color: tomato;
  padding: 0 0.2rem;
  font-weight: bold;
`;

const Child = ({ houseName, pocketMoney }) => {
  console.log('Child:', houseName, pocketMoney);
  return (
    <div>
      <p>
        저는 <MySpan>{houseName}</MySpan>의 막내입니다. <br />
        할아버지가 아버지를 통해 저에게 <MySpan>{pocketMoney}원</MySpan>을
        용돈으로 주셨습니다.
      </p>
    </div>
  );
};

export default Child;

```

![](https://i.imgur.com/6l4wAfw.png)

이런식으로 `부모` → `자식` → `그 자식` → `그자식의 자식` 으로 prop drilling 시켜줬다. 이 과정에서 Father은 부모의 prop을 Child에 전달하는 그저 거쳐가기만 했다. (쓸모없는 과정 발생)

이것을 useContext로 해결할 수 있다.

## useContext 사용 했을 때

1. 우선 context 라는 폴더를 만들고 FamilyContext.jsx를 생성한다.

![](https://i.imgur.com/vqI6rAQ.png)

```jsx
import { createContext } from "react";

export const FamilyContext = createContext(null);
```

나중에 props로 주입하는 하위 컴포넌트에서 사용할 수 있는 FamilContext가 완성 되었는데, 용돈과 집안 이름은 할아버지로 부터 나왔기 때문에 GrandFather 컴포넌트에서 사용할 수 있다.

```ad-note
title: null의 의미?

createContext의 파라미터에는 context의 기본 값을 설정 할 수 있다. 여기서 사용한 ==null은 context를 쓸 때 값을 따로 지정하지 않을 경우 사용되는 기본 값이다.== 
```

2. GrandFather.jsx 코드 수정
```jsx
import React from 'react';
import Father from './Father';
import { FamilyContext } from '../context/FamilyContext';

const GrandFather = () => {
  const houseName = '스파르타';
  const pocketMoney = 10000;
  return (
    // Provider: context 전달 (to 하위 컴포넌트)
    <FamilyContext.Provider value={{ houseName, pocketMoney }}>
      <Father />
    </FamilyContext.Provider>
  );
};

export default GrandFather;

```

 3. Father.jsx 수정
 Father로 전달하는 `props`를 넘겨주지 않고 context만든것을 통해 외부로 접근할 수 있게 해준다.

```jsx
import React from 'react';
import Child from './Child';

const Father = ({ houseName, pocketMoney }) => {
  console.log('Father:', houseName, pocketMoney); // 스파르타 10000
  return <Child />;
};

export default Father;

```

4. Child.jsx 수정

자식 컴포넌트에 useContext, FamilyContext를 import해주고, 컴포넌트 내부에 data 라는 변수를 생성하고 useContext를 사용해 FamilyContext 데이터를 불러온다.

그리고, data.houseName과 같이 부모가 전달해준 값을 출력해주면 된다.

```ad-important
- 이 방식은 props로 내려준 값을 쓴것이 아니다. 
- context를 이용해 값을 받아온것!
```


```jsx
import React, { useContext } from "react";
import { FamilyContext } from "../context/FamilyContext";

function Child({ houseName, pocketMoney }) {
  const stressedWord = {
    color: "red",
    fontWeight: "900",
  };

  const data = useContext(FamilyContext);
  console.log("data", data);

  return (
    <div>
      나는 이 집안의 막내에요.
      <br />
      할아버지가 우리 집 이름은 <span style={stressedWord}>{data.houseName}</span>
      라고 하셨어요.
      <br />
      게다가 용돈도 <span style={stressedWord}>{data.pocketMoney}</span>원만큼이나
      주셨답니다.
    </div>
  );
}

export default Child;
```

## 주의사항
- `useContext`를 사용할 때, Provider에서 제공한 value가 달라지게 되면 useContext를 사용하고 있는 모든 하위 컴포넌트가 리렌더링이 되게 된다. 
- 그래서 상태관리 라이브러리로써는 조금 부족한 부분이 있다.
- 따라서 value 부분을 항상 신경을 써주어야한다. 

## 해결 방안 
- [[00. Redux 소개|Redux]]를 사용하면 상태관리면으로써 좋은 해결 방안이 된다.

