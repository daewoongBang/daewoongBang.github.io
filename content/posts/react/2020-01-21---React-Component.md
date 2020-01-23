---
title: React Component
date: '2020-01-21 18:00:00'
template: 'post'
draft: false
slug: 'react-component'
category: 'React'
tags:
  - 'React'
  - 'Component'
  - 'Props'
  - 'State'
description: 'Component, Props, State'
socialImage: ''
---

## Component가 뭘까?

**Component**의 사전적 의미는 **`(구성) 요소`, `부품`**이다.  
사전적 의미에서 알 수 있는 것처럼 React는 여러 가지 **Component**로 구성되어 있으며, 이러한 요소, 부품들이 모여 하나의 React 애플리케이션이 탄생한다.  
React를 사용하여 애플리케이션의 인터페이스를 설계할 때는 UI의 각 부분을 여러 개의 **Component** 단위로 나눌 수 있고, 여러 조각으로 나눠진 **Component**를 통해 UI를 재사용 가능하게 만들 수 있고, 각 조각을 개별적으로 살펴볼 수 있다.

예를 들어 아래와 같은 [**Todo List App**](https://daewoongbang.github.io/mashup-todolist/)을 만든다고 하면,

<img src="/media/react/mashup-todolist-ui.png" width="350" height="521" alt="mashup-todolist ui">

**Component**는 다음과 같이 분리할 수 있다.

1. **TodoTemplate**  
   Todolist의 레이아웃을 설정하는 컴포넌트.  
   페이지의 중앙에 그림자가 적용된 흰색 박스.  
   <br>

2. **TodoHead**  
   오늘 날짜와 요일을 보여주고, 앞으로 해야할 일이 몇 개 남았는지 표시.  
   <br>

3. **TodoList**  
   할 일에 대한 정보가 들어있는 todos 배열을 내장함수 map을 사용하여 여러개의 TodoItem 컴포넌트를 렌더링.  
   <br>

4. **TodoItem**  
   각 할 일에 대한 정보를 렌더링해주는 컴포넌트.  
   좌측에 있는 원을 누르면 할 일의 완료 여부 toggle 가능.  
   할 일이 완료되었을 땐 좌측에 체크 표시되고 텍스트 색상이 연해짐.  
   마우스 올리면 휴지통 아이콘이 나타나고 이를 누르면 항목 삭제.  
   <br>

5. **TodoCreate**  
   새로운 할 일을 등록할 수 있게 해주는 컴포넌트.  
   TodoTemplate의 하단부에 초록색 원 버튼을 렌더링해주고, 이를 클릭하면 할일을 입력할 수 있는 폼이 나타남.

**Component**는 데이터가 주어졌을 때 이에 맞추어 UI를 만들어 주는 것은 물론이고, 라이프사이클 API를 이용하여 **Component**가 화면에 나타날 때, 사라질 때, 변화가 일어날 때 주어진 작업들을 처리할 수 있으며, 임의 메서드를 만들어 특별한 기능을 붙여줄 수 있다.

Todo List App 코드는 아래 링크에서 확인 가능하다.  
<https://github.com/daewoongBang/mashup-todolist>

그렇다면 Component는 어떻게 작성할까?  
Component를 선언하는 방식은 **클래스형**과 **함수형** 두 가지가 있으며 다음과 같은 차이가 있다.

### 클래스형 Component

```javascript
class App extends Component {
  render() {
    const name = 'daewoong';
    return <div>{name}</div>;
  }
}
```

- state 기능 및 라이프사이클 기능을 사용할 수 있고 임의 메서드를 정의할 수 있다.
- render 함수가 꼭 있어야 하고, 그 안에서 보여주어야 할 JSX를 반환해야 한다.

### 함수형 Component

```javascript
const App = () => {
  const name = 'daewoong';
  return <div>{name}</div>;
};
```

- 메모리 자원을 클래스형 컴포넌트보다 덜 사용한다.
- 프로젝트 완성 후 빌드, 배포할 때 결과물의 크기가 클래스형보다 더 작다.
- state와 라이프사이클 API의 사용이 불가하다. (Hook으로 해결 가능)

리액트 공식 메뉴얼에서는 Component를 새로 작성할 때 함수형 Component와 Hooks를 사용하도록 권장하고 있다.

<hr>

## props

props는 `properties`를 줄인 표현으로 상위 Component가 하위 Component에게 값을 전달할 때 사용한다. React가 사용자 정의 Component로 작성한 엘리먼트를 발견하면 JSX Attribute를 해당 컴포넌트에 단일 객체로 전달하는데 이 객체를 `props` 라고 한다. Component 자신은 해당 props를 읽기 전용으로만 사용할 수 있다.

**App Component**에서 **name** 속성을 설정하여 **MyComponent**가 해당 값을 받는다고 하면 아래와 같이 작성할 수 있다.

```javascript
const App = () => {
  return <MyComponent name="daewoong" />;
};
```

```javascript
const MyComponent = props => {
  return (
    <div>
      Hello! My Name is <b>{props.name}</b>
    </div>
  );
};
```

name 값을 ES6의 **비구조화 할당** 문법을 사용하여 아래와 같이 받을 수도 있다.

```javascript
const MyComponent = ({ name }) => {
  return (
    <div>
      Hello! My Name is <b>{name}</b>
    </div>
  );
};
```

name 값을 지정하지 않았을 경우 **defaultProps**로 기본값을 설정할 수 있다.

```javascript
const MyComponent = props => {
  return (
    <div>
      Hello! My Name is <b>{props.name}</b>
    </div>
  );
};

// name 값이 전달되지 않았을 경우 기본 값 설정.
MyComponent.defaultProps = {
  name: 'anonymous'
};
```

Component 태그 사이의 내용을 보여주는 props를 **children** 이라고 한다.

```javascript
const App = () => {
  return <MyComponent>daewoong</MyComponent>;
};
```

```javascript
const MyComponent = props => {
  return (
    <div>
      Hello! My Name is <b>{props.children}</b>
    </div>
  );
};
```

Component의 이름은 항상 대문자로 시작한다. React는 소문자로 시작하는 컴포넌트를 DOM 태그로 처리한다. 예를 들어 `<div />`는 HTML div 태그를 나타내지만, `<MyComponent />`는 컴포넌트를 나타내며 범위 안에 MyComponent가 있어야 한다.

<hr>

## state

React에서 `state`는 Component 내부에서 바뀔 수 있는 값을 의미한다. props는 Component가 사용되는 과정에서 부모 Component가 설정하는 값이며, Component 자신은 해당 props를 읽기 전용으로만 사용할 수 있다. props를 바꾸려면 부모 Component에서 바꾸어 주어야 한다.

React에서는 두 가지 종류의 state가 있는데 하나는 클래스형 Component가 가지고 있는 state이고, 다른 하나는 함수형 Component에서 **useState**라는 함수를 통해 사용하는 state이다.

### 클래스형 Component에서 state 사용

```javascript
class Counter extends Component {
  state = {
    number: 0
  };

  render() {
    const { number } = this.state;
    return (
      <div>
        <h1>{number}</h1>
        <button
          onClick={() => {
            this.setState({ number: number + 1 });
          }}
        >
          + 1
        </button>
      </div>
    );
  }
}
```

### 함수형 Component에서 useState 사용

```javascript
const Say = () => {
  const [message, setMessage] = useState('');
  const onClickEnter = () => setMessage('HI!');
  const onClickLeave = () => setMessage('BYE!');

  return (
    <div>
      <button onClick={onClickEnter}>입장</button>
      <button onClick={onClickLeave}>퇴장</button>
      <h1>{message}</h1>
    </div>
  );
};
```

state 값을 바꾸어야 할 때는 `setState` 혹은 `useState`를 통해 전달받은 setter 함수를 사용해야 한다.

<hr>

참고: 리액트 공식 문서

- Components and Props  
  <https://ko.reactjs.org/docs/components-and-props.html>

- State and Lifecycle  
  <https://ko.reactjs.org/docs/state-and-lifecycle.html>
