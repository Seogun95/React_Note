# Scss? Styled?

## 1. SCSS

```ad-note
title: scss?
* 코드의 재활용성 및 가독성을 높여주고 css의 단점을 보완해 효율성을 높이기 위해 나온 문법.
* css in css 문법. js파일과는 분리되어 있다.
* css 전처리기로서, css 언어 이외에 variables, mixins, nesting 등 복잡한 형태의 스타일 시트를 작성하기 더 쉽게 돕는 기능이 탑재되어 있다.
```

**기능 정리**
1. `variable` - 웹사이트에서 가장 중요한 색 혹은 가장 중요한 `styles` 등을 저장할 때 사용
2. `mixins` - 스타일시트에서 재사용이 가능한 스타일을 정의할 수 있도록 하는 문법
3. `nesting` - `target`하는 `element`를 더 명확하게 나타내어 준다.

### CSS 단점
* 선택자 (Selector)를 만드는 관점에서 불필요한 부모 선택자를 매번 적어야함
* 규모가 큰 프로젝트의 경우 자동화가 어렵고 수동으로 모든 것들을 수정해야 함

### SCSS 장점
* 선택자의 중첩을 통해 반복되는 부모 요소 사용을 현저하게 줄일 수 있다.
* 선택자를 중첩, 속성을 중첩을 통해 상위 요소를 참조하는 것이 가능하다
* 조건문, 반복문을 사용해 동적인 css 관리 가능
* 큰 프로젝트에서도 CSS를 유지 가능하며 조직적인 형태로 관리할 수 있다.
-  변수를 지정해 놓고 바로 사용이 가능하고, 디자인 컨셉 변경 등 컬러코드 속성을 대대적으로 변경할 때에도, 변수 선언 부만 수정하면 되니 유지보수성도 뛰어나다.

### SCSS 단점
* 잘못 관리되는 경우, 복잡하고 가독성이 떨어지는 구조의 코드로 짜여질 수 있다는 위험성이 있다.
* .css 확장자는 브라우저가 이해할 수 있지만, .scss는 이해하지 못한다.
* 그렇기 때문에 SCSS는 CSS로 컴파일을 거쳐야 하는 번거로움이 있다.

## 2. [[Styled Components|Styled Components]]

```ad-note
title: Styled Compoents
* 자바스크립트 라이브러리의 일종.
* 리액트 컴포넌트에 스타일을 더하기 위한 라이브러리
* JS 컴포넌트 파일 안에 CSS code를 입력할 수 있는 방식인 만큼, 컴포넌트의 스타일과 로직을 동시에 관리할 수 있다는 점에서 메리트를 갖는다.
```

### Styled Components 장점
* 컴포넌트의 스타일링과 로직 관리를 한 번에 할 수 있다는 장점!
* 코드 재사용성이 좋다.
* JS 변수/함수 기반인 만큼 동적인 스타일링 구현에 적합하다.
* 스타일에 대한 고유 클래스명을 생성한다.

### Styled Components 단점
-   잘못 관리된다면 파일이 너무 비대해질 수 있다는 문제가 있음
-   JS 기반인만큼 해당 언어에 익숙하지 않다면 배우는 과정에 어려움이 있을 수 있다.

## 3. Tailwind CSS
```ad-note
title: Tailwind CSS
* HTML 요소를 빠르고 쉽게 스타일링 할 수 있도록 돕는 미리 디자인된 CSS 클래스 집합을 제공하는 Utility-First 컨셉을 가진 CSS 프레임워크다. 
* 최소한의 설정을 토대로, 사용자 정의 위주로 되어 있어 신속한 프로토타이핑 및 개발에 널리 사용되기 시작했다.
* 부트스트랩과 비슷하게 `m-1`, `flex`와 같이 미리 세팅된 유틸리티 클래스를 활용하는 방식으로 HTML 코드 내에서 스타일링을 할 수 있다.
```

### Tailwind CSS 장점
-   HTML 요소를 꾸미는 빠르고 효율적이다.
-   필요한 설정이 많지 않다.
-   스타일 코드도 HTML 코드 안에 있기 때문에 HTML와 CSS 파일을 별도로 관리할 필요가 없다.

### Tailwind CSS 단점
-   Tailwind가 제공하는 미리 제공된 틀을 벗어난 스타일링을 구현하는 데에 어려움이 있다.
-   특정 컴포넌트나 요소에 스타일 범위를 지정하는 수단이 갖춰져 있지 않아 큰 프로젝트 진행 시 naming 충돌이 일어날 수 있다.