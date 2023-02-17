# Styled Components?
#styledComponents

```ad-summary
title: 목표
1. CSS-in-JS를 이해할 수 있습니다. 
2. Styled Components를 배워보고, 활용할 수 있습니다.
```

```ad-note
title: styled-componets란?
* styled-components는 우리가 리액트에서 CSS-in-JS 방식으로 컴포넌트를 꾸밀수 있게 도와주는 패키지 이다. 
* styled-components외에도 다양한 패키지가 있지만, styled-components는 예전부터 꾸준히 많은 개발자들에게 선택받은 패키지이다.
* CSS-in-JS란, 자바스크립트로 CSS코드를 작성하는 방식을 말한다.
* props를 통해서 부모 컴포넌트로부터 값을 전달받고, 조건문을 이용해서 조건부 스타일링을 할 수 있다.
```

## 리액트에서 CSS

리액트에서 css를 꾸미는 대표적인 방법
1. emotion
2. **styled-components**
3. tailwind CSS

![](https://i.imgur.com/fFbjEO0.png)


## Css-in-js
CSS-in-JS방식이란, 단어 그대로 자바스크립트 코드로 CSS 코드를 작성하여 컴포넌트를 꾸미는 방식입니다. 순수한 CSS코드를 우리가 아니라, 자바스크립트를 이용해서 CSS코드를 만들어내는 것이죠. CSS-in-JS 방식을 사용하기 위해 우리는 새로운 패키지를 사용할 것 입니다.

## 1. Styled Componets 준비

준비과정은 아래와 같다.

1. VScode 플러그인 설치
![](https://i.imgur.com/hNtfdSM.png)

2. yarn으로 vscode에 styled-components [설치](https://marketplace.visualstudio.com/items?itemName=styled-components.vscode-styled-components) #styledComponents설치 

```bash
yarn add styled-components
```


## 2. Styled Componets 사용방법

**기본구조**
```jsx
const 스타일컴포넌트명 = styled.태그`
   //스타일 코드 작성
`
```

중요하게 봐야할 부분은 `const 스타일컴포넌트명 = styled.div``;` 이 부분!
html 태그명 뒤에 백틱을 사용하여 css를 작성할 수 있다.

div ➜ `styled.div`
span ➜ `styled.span`
button ➜ `styled.button`

```jsx
import React from "react";
// styled-components에서 styled 라는 키워드를 import
import styled from "styled-components";

const StBox = styled.div`
// 스타일 코드를 작성
	display: flex;
	justify-content: center;
	align-items: center;
	width: 200px;
	height: 100px;
	border: 1px solid tomato;
	border-radius: 1rem;
	margin: 1rem;
`;

const App = () => {
  return <StBox>박스</StBox>;
};

export default App;
```

### 조건부 스타일링
classname을 사용해서 구현하기는 조금 까다로운 조건부 스타일링을 styled-components를 이용하면 간편하게 할 수 있다. 이 안에서 삼항연산자, if문, switch 문을 통해 스타일링을 해줄 수 있다.

우선, 박스 색을 배열 변수로 넣어두고,  props를 사용하여 각 컬러등을 특정한 값으로 사용해 줄 수 있다.

스타일드 박스에 props로 넘겨주려면 아래와 같이 화살표 함수를 사용하면 된다.

```jsx
const StBox = styled.div`
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: 100px;
  border-radius: 1rem;
  margin: 1rem;
  border: 1px solid ${(props) => props.borderColor};
  color: ${(props) => props.color};
  background-color: ${(props) => props.backgroundColor};
`;
```

```jsx
function App() {
  return (
    <>
      <StBox color="white" borderColor="blue" backgroundColor="Darkturquoise">
        파란 박스
      </StBox>
      <StBox color="white" borderColor="green" backgroundColor="Yellowgreen">
        초록 박스
      </StBox>
      <StBox color="white" borderColor="red" backgroundColor="tomato">
        빨간 박스
      </StBox>
    </>
  );
```

<img src="https://i.imgur.com/D74Wl6v.png" height="300">

### Switch문과 map을 사용해 변수 사용
위에서 구현한 코드를 자바스크립트의 map과 switch문을 이용해서 리팩토링 방법

1. 배열
```jsx
const boxColorArr = ['Darkturquoise', 'Yellowgreen', 'Tomato'];
```

```jsx
const getColorTxt = (color) => {
  switch (color) {
    case 'Darkturquoise':
      return '파란색';
    case 'Yellowgreen':
      return '노란색';
    case 'Tomato':
      return '빨간색';
    default:
      return '검은색';
  }
};
```

```jsx
{boxColorArr.map((item) => {
  return (
	<StBox color='white' backgroundColor={item}>
	  {getColorTxt(item)}
	</StBox>
  );
})}
```

2. 객체
```jsx
const boxColorObj = [
	{ color: 'white', backgroundColor: 'Darkturquoise' },
	{ color: 'black', backgroundColor: 'Yellowgreen' },
	{ color: 'yellow', backgroundColor: 'Tomato' },
];
```

```jsx
{boxColorObj.map((item) => {
  return (
	<StBox color={item.color} backgroundColor={item.backgroundColor}>
	  {getColorTxt(item)}
	</StBox>
  );
})}
```

## 공통된 스타일 컴포넌트로 생성 방법

공통으로 사용되는 스타일을 컴포넌트화 해서 사용 가능하다.

```
📦src  
 ┣ 📂styled  
 ┃ ┗ 📜ShareStyle.js  
```

### ShareStyle.js 생성

src/styled/ShareStyle.js
```jsx
import styled from 'styled-components';

export const DivFlex = styled.div`
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: ${(props) => props.direction};
`;

export const MyH1 = styled(DivFlex.withComponent('h1'))`
  font-size: 3rem;
  color: tomato;
`;

```

DivFlex라는 Div 컴포넌트를 생성하고, 만약 MyH1 등에서도 이 Div를 가져오고 싶다면, styled(DivFlex)와 같이 사용 가능하다.

그리고, HTML 타입이 다르다면 withComponent를 사용해 괄호 안에 태그를 입력해주면 된다.

다른 컴포넌트에서 사용가능하도록 export 해주는것을 잊으면 안된다.

### ShareStyle import

이제 공동 스타일을 사용하고자 하는 컴포넌트에 import 해준다.

import 해줄때는 `import * as Some from '경로'` 처럼 해줘야 한다.

그리고 아래와 같이 컴포넌트를 사용하면 된다.

```jsx
import React from 'react';
import * as S from '../styled/ShareStyle';

const About = () => {
  return (
    <>
      <S.DivFlex direction={'row'}>
        <S.MyH1>About 페이지 입니다.</S.MyH1>
      </S.DivFlex>
    </>
  );
};

export default About;

```


[[GlobalStyles (전역 스타일링)]]
theme provider
[CSS Tools Online](https://codebeautify.org/css-tools)