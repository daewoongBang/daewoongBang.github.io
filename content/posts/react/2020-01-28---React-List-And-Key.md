---
title: React List And Key
date: '2020-01-28 10:35:00'
template: 'post'
draft: false
slug: 'react-list-and-key'
category: 'React'
tags:
  - 'React'
  - 'list'
  - 'key'
description: '리스트와 key, 컴포넌트 반복'
socialImage: ''
---

### React는 반복적인 내용을 어떻게 효율적으로 보여줄까?

일단 자바스크립트 배열 객체의 내장 함수 중 하나인 **map** 이라는 함수에 대해 알아야 한다.

> arr.map(callback(currentValue[, index[, array]])[, thisArg])

#### 매개변수

- `callback`: 새로운 배열 요소를 생성하는 함수. 다음 세 가지 인수를 가진다.

  - `currentValue`: 처리할 현재 요소.
  - `index`(Optional): 처리할 현재 요소의 인덱스.
  - `array`(Optional): map()을 호출한 배열.

- `thisArg`(Optional): callback을 실행할 때 this로 사용되는 값.

#### 반환 값

배열의 각 요소에 대해 실행한 `callback`의 결과를 모은 새로운 배열.

**map**은 각각의 요소에 대해 한번씩 순서대로 불러 그 함수의 반환값으로 새로운 배열을 만든다. 사용법은 다음과 같다.

```javascript
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(number => number * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

여기서 중요한 건 **map**이 기존 numbers 값을 변경하지 않고 새로운 배열을 반환한다는 것이다. doubled 에는 [2, 4, 6, 8, 10] 이 담겨있지만 numbers는 그대로 [1, 2, 3, 4, 5]이다.  
리액트에서 상태를 업데이트할 때는 이와 같이 기존 상태를 그대로 두면서 새로운 값을 상태로 설정해야 하는데 이를 **_불변성 유지_**라고 한다. 불변성 유지를 해주어야 나중에 리액트 컴포넌트의 성능을 최적화할 수 있다. **concat**, **filter** 등의 배열 내장 함수도 새로운 배열을 반환함으로써 불변성을 유지할 수 있도록 해준다.

### 컴포넌트에 적용해보기

JSX에서 map을 사용하여 아래와 같이 데이터 배열을 표현할 수 있다.

```jsx
const IterationSample = () => {
  const names = ['daewoong', 'gildong', 'daeseoung', 'suji'];
  const nameList = names.map(name => <li>{name}</li>);
  return <ul>{nameList}</ul>;
};
```

해당 코드를 렌더링해보면 원하는 대로 렌더링이 된 것처럼 보인다. 하지만 크롬 개발자 도구 console을 열어보면 다음과 같은 경고 메시지가 떠있다.  
<span style="color:red">**Warning: Each child in a list should have a unique "key" prop.**</span>  
"key" prop이 없다는 경고 메시지이다. 그럼 key란 무엇일까?

#### Key

`key`는 엘리먼트 리스트를 만들 때 포함해야 하는 특수한 문자열 Attribute이다. 리액트에서 `key`는 컴포넌트 배열을 렌더링했을 때 어떤 항목을 변경, 추가, 삭제할지 식별하는 것을 돕는 역할을 한다. `key`가 없다면 Virtual DOM을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 찾아야하지만 `key`가 있다면 이 값을 사용하여 어떤 변화가 일어났는지 더욱 빠르고 효율적으로 감지할 수 있다.

위에서 경고 메시지를 출력했던 코드는 아래와 같이 수정할 수 있다.

```jsx
const IterationSample = () => {
  const names = ['daewoong', 'gildong', 'daeseoung', 'suji'];
  const nameList = names.map((name, index) => <li key={index}>{name}</li>);
  return <ul>{nameList}</ul>;
};
```

key는 **리스트의 다른 항목들 사이에서 해당 항목을 고유하게 식별할 수 있는 문자열**을 선택하여 값으로 넣어주면 된다. 하지만 위 코드처럼 index를 key 값으로 사용하는 경우는 고유한 값이 없을 때이다. index를 key로 사용하면 배열이 변경될 때 효율적으로 리렌더링하지 못한다. (index를 key로 사용할 경우 부정적인 영향에 대한 설명은 아래 Index as a key is an anti-pattern 링크를 참고)

아래 코드와 같이 항목을 고유하게 식별할 수 있는 id 값을 key로 사용하는 것이 가장 이상적이다.

```jsx
const posts = [
  { id: 1, title: 'Hello World', content: 'Welcome to learning React!' },
  { id: 2, title: 'Installation', content: 'You can install React from npm.' }
];
const content = posts.map(post => (
  <Post key={post.id} id={post.id} title={post.title} />
));
```

위 예제에서 또 하나 알아둘건 Post 컴포넌트는 props.id를 읽을 수 있지만 props.key를 읽을 수는 없다. React에서 key는 컴포넌트의 prop으로 전달되지는 않는다. 컴포넌트에서 key와 동일한 값이 필요하면 위 코드에서 key 외에 id 값을 별도로 준 것 처럼 다른 이름의 prop으로 명시적으로 전달한다.

<hr>

**reference:**

##### 리액트 공식 문서

- 리스트와 Key  
  <https://ko.reactjs.org/docs/lists-and-keys.html>
- 왜 Key가 필요한가  
  <https://ko.reactjs.org/docs/reconciliation.html#recursing-on-children>

##### MDN web docs

- 자바스크립트 Array map()
  <https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map>

##### etc

- Index as a key is an anti-pattern  
  <https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318>
