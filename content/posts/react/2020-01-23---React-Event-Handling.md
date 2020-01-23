---
title: React Event Handling
date: '2020-01-23 11:16:00'
template: 'post'
draft: false
slug: 'react-event-handling'
category: 'React'
tags:
  - 'React'
  - 'Event Handling'
description: 'React Event 처리하기'
socialImage: ''
---

사용자가 웹 브라우저에서 DOM 요소들과 상호 작용하는 것을 **Event** 라고 한다. 예를 들어 버튼을 클릭했을 때는 `onclick` 이벤트를 실행하고 Form 요소는 값이 바뀔 때 `onchange` 이벤트를 실행한다.

React에서 Event를 처리하는 방식은 HTML DOM Element에서 Event를 처리하는 방식과 매우 비슷하지만 몇 가지 문법적인 차이가 있다.

아래 코드를 보면,

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

- React의 이벤트는 소문자 대신 _**camelCase를 사용한다**_.  
  ( onclick -> `onClick` )
- JSX를 사용하여 문자열이 아닌 _**함수로 Event Handler를 전달한다**_.  
  ( `onClick={onClickLeave}` )
- false를 반환해도 event의 기본 동작을 방지할 수 없다. _**반드시 `preventDefault`를 명시적으로 호출해야 한다**_.

```javascript
const ActionLink = () => {
  const handleClick = e => {
    // e 객체는 합성 이벤트(SyntheticEvent)로 웹 브라우저의 네이티브 이벤트를 감싸는 객체
    // 네이티브 이벤트와 인터페이스가 같으므로 순수 자바스크립트 HTML Event를 다룰 때와 똑같이 사용 가능하다.
    e.preventDefault(); // 기본 동작 방지
    console.log('The link was clicked.');
  };

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
};
```

- _**DOM 요소에만 이벤트를 설정할 수 있다**_. div, button, input, form, span 등의 DOM 요소에는 이벤트를 설정할 수 있지만, 우리가 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없다.

참고: 리액트 공식 문서

- 이벤트 처리하기  
  <https://ko.reactjs.org/docs/handling-events.html>

- 합성 이벤트(SyntheticEvent)  
  <https://ko.reactjs.org/docs/events.html>
