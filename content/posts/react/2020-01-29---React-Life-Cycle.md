---
title: React Life Cycle
date: '2020-01-29 10:35:00'
template: 'post'
draft: false
slug: 'react-life-cycle'
category: 'React'
tags:
  - 'React'
  - 'life cycle'
description: 'React 라이프 사이클 메서드'
socialImage: ''
---

라이프사이클 메서드는 **컴포넌트 상태에 변화가 있을 때마다 실행하는 메서드**이다. 이 메서드들은 클래스형 컴포넌트에서만 사용 가능하고 함수형 컴포넌트에서는 `Hooks`를 사용한다.
라이프사이클의 흐름은 마운트, 업데이트, 언마운트로 구분할 수 있다.

- 마운트: DOM이 생성되고 웹브라우저 상에 나타나는 것.
- 언마운트: 컴포넌트를 DOM에서 제거하는 것.
- 업데이트: 컴포넌트 정보를 업데이트하는 것이고 다음과 같은 경우에 발생한다.
  1. props가 바뀔 때
  2. state가 바뀔 때
  3. 부모 컴포넌트가 리렌더링될 때
  4. this.forceUpdate로 강제로 렌더링을 트리거할 때

컴포넌트를 업데이트할 때는 다음과 같은 라이프사이클 메서드를 호출한다.

### 라이프사이클 메서드

#### 1. render()

컴포넌트의 모양새 정의. 필수 메서드.
이 메서드 안에서 this.props와 this.state에 접근할 수 있으며, div 태그나 커스텀 컴포넌트 같은 리액트 요소를 반환한다. 아무것도 보여 주고 싶지 않다면 null 값이나 false 반환.

#### 2. constructor

컴포넌트의 생성자 메서드로 컴포넌트를 만들 때 처음으로 실행. 초기 state를 정할 수 있다.

#### 3. getDerivedStateFromProps

리액트 16.3 버전 이후에 새로 생긴 메서드. props로 받아 온 값을 state에 동기화시키는 용도로 사용. 컴포넌트가 마운트될 때와 업데이트될 때 호출.

#### 4. componentDidMount

컴포넌트를 다 만들고, 첫 렌더링을 다 마친 후 실행. 이 안에서 다른 자바스크립트 라이브러리 또는 프레임워크 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리하면 된다.

```jsx
componentDidMount() {
    this.timerID = setInterval(
        () => this.tick(),
        1000
    );
}
```

#### 5. shouldComponentUpdate

props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드. 이 메서드에서는 반드시 `true`나 `false` 값을 반환해야 한다. 이 메서드를 따로 생성하지 않으면 기본적으로 언제나 true 값을 반환한다. 만약 false 값을 반환한다면 업데이트 과정은 여기서 중지된다. 프로젝트 성능을 최적화할 때, 상황에 맞는 알고리즘을 작성하여 리렌더링을 방지할 때는 false 값을 반환하게 한다.

#### 6. getSnapshotBeforeUpdate

리액트 16.3 버전 이후에 새로 생긴 메서드. 이 메서드는 render에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출된다. 주로 업데이트하기 직전의 값을 참고할 일이 있을 때 활용.(예: 스크롤바 위치 유지)

#### 7. componentDidUpdate

리렌더링을 완료한 후에 실행한다. 업데이트가 끝난 후이므로 DOM 관련 처리를 해도 된다. 여기서는 prevProps, prevState를 사용하여 컴포넌트가 이전에 가졌던 데이터에 접근할 수 있다.

#### 8. componentWillUnmount

컴포넌트를 DOM에서 제거할 때 실행한다. componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 한다.

```jsx
componentWillUnmount() {
    clearInterval(this.timerID);
}
```

#### 9. componentDidCatch

리액트 16 버전에서 새롭게 도입된 메서드. 컴포넌트 렌더링 도중에 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI를 보여 줄 수 있게 해준다.

```jsx
componentDidCatch(error, info) {
    this.setState({
        error: true
    });
    console.log({error, info});
}
```

error는 파라미터에 어떤 에러가 발생했는지 알려주고, info는 어디에 있는 코드에서 오류가 발생했는지에 대한 정보를 준다. 이 메서드를 사용할 때는 컴포넌트 자신에게 발생하는 에러는 잡을 수 없고 자신의 this.props.children으로 전달되는 컴포넌트에서 발생하는 에러만 잡아낼 수 있다.

<hr>

**reference:**

##### 리액트 공식 문서

- 생명주기 메서드를 클래스에 추가하기  
  <https://ko.reactjs.org/docs/state-and-lifecycle.html#adding-lifecycle-methods-to-a-class>
