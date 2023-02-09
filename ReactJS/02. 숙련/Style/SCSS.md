
# SCSS 스타일링

`CSS`를 전통적인 방법보다 효율적으로 사용하기 위해 만들어진 언어이다.

`CSS`는 웹 프로젝트 규모가 커지면 커질수록 코드가 복잡해지고 유지보수도 불편해지는데, 계속해서 동일한 코드를 복사하고 붙여넣는 과정을 반복해야 하기 때문!

코드의 재사용성을 높이고, 가독성 또한 향상시켜줄 수 있는 방법이 바로 [[Scss  & Styled Components#1. SCSS|SASS]] !

## SCSS 특징
1. 변수 사용 가능
```scss
//변수화
$color: #4287f5;
$borderRadius: 10rem;

div {
	background: $color;
	border-radius: $borderRadius;
}
```

2. 중첩 가능
```scss
label {
      padding: 3% 0px;
      width: 100%;
      cursor: pointer;
      color: $colorPoint;

      &:hover {
        color: white;
        background-color: $color;
      }
}
```

3. 다른 style 파일을 import 가능
* common.scss
```scss
$color1: #ed5b46; 
$color2: #f26671; 
$color3: #f28585; 
$color4: #feac97;
```
*  style.scss
```scss
//style.scss
@import "common.scss";
.box {
	background-color: $color3;
}
```

## CSS reset
웹브라우저에서 기본적으로 제공하는 defalut css style을 css reset을 통해 제거해줄 수 있다.

```css
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed, 
figure, figcaption, footer, header, hgroup, 
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
	margin: 0;
	padding: 0;
	border: 0;
	font-size: 100%;
	font: inherit;
	vertical-align: baseline;
}
/* HTML5 display-role reset for older browsers */
article, aside, details, figcaption, figure, 
footer, header, hgroup, menu, nav, section {
	display: block;
}
body {
	line-height: 1;
}
ol, ul {
	list-style: none;
}
blockquote, q {
	quotes: none;
}
blockquote:before, blockquote:after,
q:before, q:after {
	content: '';
	content: none;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}
```