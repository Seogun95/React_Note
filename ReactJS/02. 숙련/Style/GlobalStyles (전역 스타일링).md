# GlobalStyles?

```ad-summary
title: 목표
1. Styled Component 중 GlobalStyles가 왜 필요한지에 대해, 그리고 사용방법에 대해 알 수 있다.
2. Sass가 무엇인지 알 수 있다.
3. css reset 기법의 필요성을 인지하고, 간단한 실습을 완료하게 된다.
```

## 1. 지역 스타일링
특정한 컴포넌트 안에서 쓰이는 스타일들을 [[Styled Components|지역 스타일링]]이라고 한다.

```jsx
// TestPage.jsx
import React from 'react';
import styled from 'styled-components';

//지역 스타일링
const Wrapper = styled.div`
  width: 100%;
  min-height: 100px;
  border: 1px solid ${(props) => props.borderColor};
  padding: 1rem;
  border-radius: 1rem;
  margin: 1rem;
`;

const Title = styled.h1`
  color: ${(props) => props.color};
  margin-bottom: 0.5rem;
`;

//이 p 태그 같은 경우에는 전역에서 사용하는 것이 좋다. (밑에서 다룸)
const Content = styled.p`
  line-height: 1.5;
  font-size: 1rem;
  margin: 0;
`;

const TestPage = ({ title, contents, textColor, borderColor }) => {
  return (
    <Wrapper borderColor={borderColor}>
      <Title color={textColor}>{title}</Title>
      <Content>{contents}</Content>
    </Wrapper>
  );
};

export default TestPage;

```

```jsx
//App.js
import React from 'react';
import TestPage from './components/TestPage';

function App() {
  return (
    <>
      <TestPage
        title={'안녕'}
        contents={'hello, wolrd'}
        borderColor={'Tomato'}
        textColor={'YellowGreen'}
      />
      <TestPage
        title={'어서오세요'}
        contents={'hello, There'}
        borderColor={'YellowGreen'}
        textColor={'tomato'}
      />
    </>
  );
}

export default App;

```

## 2. 글로벌 스타일링

만일 규모가 좀 큰 프로젝트라면, 공통적으로 적용되는 스타일링 부분은 빼줄 필요가 있다. 글꼴(font)과 line-height를 공통 요소라 가정하고 GlobalStyles을 적용 할 수 있다.

```ad-note
styled components는 컴포넌트 내에서만 활용할 수 있다. 하지만, 공통적으로 들어가야 할 스타일을 적용해야 할 경우 ‘전역적으로(globally)’ 스타일을 지정한다.
```

### 전역 스타일링 사용 방법

GlobalStyle.jsx 파일을 생성하고 아래와 같이 변수를 만들어 설정해준다.
```jsx
import { createGlobalStyle } from "styled-components";
const GlobalStyle = createGlobalStyle``
```

```ad-tip
title: 코드스니펫
`const GlobalStyle = createGlobalStyle`만 입력하면 자동으로 import 된다.
```

```jsx
//GlobalStyle.jsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
    body {
        font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        line-height: 1.5;
        font-size: 1rem;
    }
`;

export default GlobalStyle;
```

위와 같이 전역 스타일링을 컴포넌트로 빼줬으면, App.js의 return 최상단에 해당 컴포넌트를 사용해주면 전역에서 사용되게 된다.

```jsx
//App.js
import React from 'react';
import TestPage from './components/TestPage';
import GlobalStyle from './components/GlobalStyle.jsx';

function App() {
  return (
    <>
      <GlobalStyle />
      <TestPage
        title={'안녕'}
        contents={'hello, wolrd'}
        borderColor={'Tomato'}
        textColor={'YellowGreen'}
      />
    </>
  );
}

export default App;
```

![](https://i.imgur.com/U62QpNa.png)
