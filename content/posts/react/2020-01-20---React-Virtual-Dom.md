---
title: React Virtual DOM
date: '2020-01-20 14:18:00'
template: 'post'
draft: false
slug: 'react-virtual-dom'
category: 'React'
tags:
  - 'React'
  - 'DOM'
  - 'Virtual DOM'
description: 'DOM과 React Virtual DOM의 기본 개념'
socialImage: ''
---

---

> We build React to solve one problem:  
> building large applications with data that  
> changes over time.

> 우리는 지속해서 데이터가 변화하는  
> 대규모 애플리케이션을 구축하기 위해 리액트를 만들었다.

---

리액트의 주요 특징 중 하나는 `Virtual DOM`을 사용하는 것이다.  
그렇다면 `Virtual DOM`이란 뭘까?  
`Virtual DOM`을 얘기하기 전에 먼저 `DOM`이 무엇인지에 대해서 알아야 한다.

`DOM`이란 `Document Object Model`의 약어로서, HTML 문서의 요소를 제어하기 위해 웹 브라우저에 처음 지원되었다.  
`DOM`은 XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스로 문서의 내용, 구조, 스타일에 접근하고 변경하는 수단이며 트리형태로 되어있어 특정 노드를 찾거나 수정하거나 제거하거나 원하는 곳에 삽입할 수 있다.

DOM API를 수많은 플랫폼과 웹 브라우저에서 사용하는데 이 DOM에는 한 가지 문제점이 있다. 예를 들어 요즘 흔히 접하는 대규모의 애플리케이션의 경우 스크롤을 내릴수록 수많은 데이터가 로딩이 되는데 이 때 각 데이터를 표현하는 요소가 많아질수록 DOM에 직접 접근하여 변화를 주다 보면 속도가 느려지는 등의 성능 이슈가 발생하기 시작한다.

DOM 자체의 성능은 빠르다.  
하지만 웹 브라우저 단에서 DOM에 변화가 일어나면 웹 브라우저가 CSS를 다시 연산하고, 레이아웃을 다시 구성하고 페이지를 리페인트하게 된다.  
즉, 이 과정에서 DOM을 조작할 때마다 엔진이 웹 페이지를 새로 그리기 때문에 업데이트가 너무 잦으면 성능이 저하될 수밖에 없다.  
이를 해결하려면 DOM을 최소한으로 조작하여 작업을 처리하는 방식으로 개선을 해야한다.  
이를 해결하기 위해 **React**는 `Virtual DOM` 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하였다.

### Virtual DOM

`Virtual DOM`을 사용하면 데이터에 변화가 생길 경우 웹 브라우저의 실제 DOM에 접근하여 조작하는 대신, 가상 DOM에 업데이트 한다.  
React에서 데이터가 변하여 실제 DOM을 업데이트할 경우 절차는 다음과 같다.

1. 데이터가 업데이트되면 전체 UI를 Virtual DOM에 리렌더링한다.
2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교한다.
3. 바뀐 부분만 실제 DOM에 적용한다.

react and the virtual dom 에 관한 짧은 영상 (한글자막 제공)  
<https://www.youtube.com/watch?v=muc2ZF0QIO4&feature=youtu.be>
