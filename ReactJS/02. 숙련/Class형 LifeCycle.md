# LifeCycle
#lifeCycle

클래스형 컴포넌트에서의 라이프사이클 이해

```ad-summary
title: 클래스형 컴포넌트
우리는 함수형 컴포넌트를 사용할것이기 때문에 클래스형 컴포넌트는 간단하게 이해만 하고 넘어가도 된다.
```

## 01. 생명 주기

```ad-note
title: 리액트 생명 주기란?
- 리액트 컴포넌트는 각각 **[Mount]** ➜ **[Update]** ➜ **[Unmount]**의 과정을 거친다. 사람처럼 태어나고, 변화하고 죽는 것!
- 리액트 생명주기(라이프사이클)란, 컴포넌트 중심 라이브러리의 집합체라고 보면 된다. 
- 모든 컴포넌트에는 각각의 생명주기가 존재하고 각 생명주기에 맞는 메서드 들이 있다.
```

![](https://i.imgur.com/YjFqpo0.png)

## 02. Mount

```ad-note
title: Mount
컴포넌트가 생성될 때를 뜻함. 
```

Mount 메서드 종류:
- `constructor()`
- `getDerivedStateFromProps(nextProps, prevState)`
- `render()`
- `componentDidMount()`

Mount 메서드 종류 디테일:
1. `constructor`
   - 컴포넌트가 맨 처음 만들어 질 때 호출
   - 생성자
2.  `getDerivedStateFromProps`
   - 부모 컴포넌트로부터 props를 전달받을 때, state에 값을 일치시키는 역할을 하는 메서드
   - 마운트 될 때, 업데이트(리렌더링) 될 때도 호출
3. `render`
   - 최초 mount가 준비완료 되면 호출되는, 즉 렌더링 하는 메서드
   - 컴포넌트를 DOM에 마운트하기 위해 사용
4.  `componentDidMount`
   - 컴포넌트가 브라우저에 표시가 된 후 호출되는 메서드

## 03. Update

```ad-note
title: Update
컴포넌트가 갱신될 때를 뜻함. 
- State / Props / 부모 컴포넌트가 변경 되었을 때
```

Update 메서드 종류:
- `getDerivedStateFromProps(nextProps, prevState)`
- `shouldComponentUpdate()`
- `render()`
- `getSnapshotBeforeUpdate()`
- `componentDidUpdate()`

Update 메서드 종류 디테일:
1. `getDerivedStateFromProps(nextProps, prevState)`
   -  Mount 과정에서도 동일하게 호출되었던 메서드.
   - 부모 컴포넌트로부터 props를 전달받을 때, state에 값을 일치시키는 역할을 하는 메서드
2. `shouldComponentUpdate()`
   - 리렌더링 여부 판단(함수 호출 결과 : true / false)
      - true인 경우 : 리렌더링 진행'
      - false인 경우 : 리렌더링 하지 않음
   - 함수형 컴포넌트에서 memo, useMemo, useCallback이 역할을 대신한다.
3. `render()`
   - 변경사항 반영이 다 되어 준비완료 되면 호출되는, 즉 렌더링 하는 메서드
   -  컴포넌트를 DOM에 마운트하기 위해 사용
4. `getSnapshotBeforeUpdate()`
   - 컴포넌트에 변화가 일어나기 직전 DOM의 상태를 저장
   - componentDidUpdate 함수에서 사용하기 위한 스냅샷 형태의 데이터
5. `componentDidUpdate()`
   - 컴포넌트 업데이트 작업 완료 후 호출


## 03. Unmount

```ad-note
title: Unmount
포넌트가 DOM에서 제거되는 시점 뜻함. 
```

Unmount 메서드 종류:
- `componentWillUnmount`

Unmount 메서드 종류 디테일:
1. `componentWillUnmount`
   - 컴포넌트가 사라지기 전 호출되는 메서드
   - useEffect의 return과 동일