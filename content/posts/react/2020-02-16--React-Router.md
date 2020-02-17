---
title: React Router
date: '2020-02-16 22:05:00'
template: 'post'
draft: false
slug: 'react-router'
category: 'React'
tags:
  - 'React'
  - 'Router'
description: 'React Router로 페이지 전환하기'
socialImage: ''
---

## React Router란 뭘까?

Router에 대해서 이야기하기 전에 **SPA**가 무엇인지에 대해 알아야 한다. **SPA**는 **Single Page Application**의 약어로 한 개의 페이지로 이루어진 애플리케이션이다. 이 말이 의미하는 바는 뭘까?

전통적인 웹 페이지는 한 개가 아닌 여러 페이지로 구성되어 있다. 여러 페이지로 구성되어 있다는 것은 사용자가 다른 페이지로 이동을 할 때마다 새로운 html을 받아 온다는 것이고, 페이지를 로딩할 때마다 서버에서 리소스를 전달받아 해석한 뒤 화면에 보여준다는 것이다.

요즘은 웹에서 제공되는 정보가 너무 많기 때문에 새로운 화면을 보여 주어야 할 때마다 서버 측에서 모든 뷰를 준비해야 한다면 성능상의 문제가 생길 수 있다. 사용자와의 인터랙션이 자주 발생하는 모던 웹 애플리케이션에서는 다음과 같은 문제가 생길 수 있다.

- 트래픽이 많이 나올 수 있다.
- 서버에 높은 부하가 걸릴 수도 있다.
- 화면 전환이 일어날 때마다 html을 계속 서버에 요청 시 사용자의 인터페이스에서 사용하고 있던 상태를 유지하기 번거롭다.
- 바뀌지 않는 부분까지 새로 불러와서 보여주어야 하기 때문에 불필요한 로딩이 있어 비효율적이다.

또한 SPA는 앱의 규모가 커지면 자바스크립트 파일도 커진다는 단점이 있다. 페이지 로딩 시 사용자가 실제로 방문하지 않을 수도 있는 페이지의 스크립트까지 불러오기 때문이다. ( 이 문제는 **코드 스플리팅**을 사용하여 라우트별로 파일들을 나누어서 트래픽과 로딩 속도를 개선할 수 있다. )

이 같은 문제를 개선하고자 리액트 같은 라이브러리 혹은 프레임워크를 사용하여 **뷰 렌더링을 사용자의 브라우저가 담당하도록 하고, 애플리케이션을 브라우저에 불러와서 실행시킨 후에 사용자와의 인터랙션이 발생하면 필요한 부분만 자바스크립트를 사용하여 업데이트 해준다.** 만약 새로운 데이터가 필요하다면 서버 API를 호출하여 필요한 데이터만 새로 불러와 애플리케이션에서 사용할 수도 있다.

리액트 라우터처럼 브라우저에서 자바스크립트를 사용하여 라우팅을 관리하는 것은 자바스크립트를 실행하지 않는 일반 크롤러에서는 페이지의 정보를 제대로 수집해 가지 못한다. 또한, 자바스크립트가 실행될 때까지 페이지가 비어 있기 때문에 자바스크립트 파일이 로딩되어 실행되는 짧은 시간 동안 빈 페이지가 나타날 수 있다. 이러한 문제점들은 **서버 사이드 렌더링**을 통해 모두 해결할 수 있다.

**다른 주소에 다른 화면을 보여 주는 것을 라우팅이라 하고 React Router는 클라이언트 사이드에서 URL 값에 따른 View를 제공하기 위한 라이브러리이다.** 리액트 라이브러리 자체에 이 기능이 내장되어 있는 것은 아니고 별도 라이브러리를 사용하여 작업을 쉽게 구현할 수 있다. 리액트 라우팅 라이브러리는 react-router, reach-router, Next.js 등 여러가지가 있다.

리액트 라우터는 클라이언트 사이드에서 이루어지는 라우팅을 아주 간단하게 구현할 수 있도록 도와준다.

## 프로젝트에 리액트 라우터 적용

`yarn add react-router-dom`

### BrowserRouter

BrowserRouter 컴포넌트는 웹 애플리케이션에 HTML5의 History API를 사용하여 페이지를 새로고침하지 않고도 주소를 변경하고, 현재 주소에 관련된 정보를 props로 쉽게 조회하거나 사용할 수 있도록 해준다.

**1. BrowserRouter 컴포넌트로 감싸기**

index.js

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById('root')
);
```

**2. 페이지 만들기**

Home.js (Home Component)

```jsx
const Home = () => {
  return (
    <div>
      <h1>Home</h1>
    </div>
  );
};
```

About.js (About Component)

```jsx
const About = () => {
  return (
    <div>
      <h1>About</h1>
      <p>React Router Sample</p>
    </div>
  );
};
```

**3. Route 컴포넌트로 특정 주소에 컴포넌트 연결**

```jsx
<Route path='주소' component={component}>
```

path에 url path를 입력하고 component에는 해당 주소에 연결할 component를 import 한 후 넣어준다.

App.js

```jsx
import React from 'react';
import { Route } from 'react-router-dom';
import Home from './page/Home';
import About from './page/About';

function App() {
  return (
    <div>
      <Route path="/" component={Home} />
      <Route path="/about" component={About} />
    </div>
  );
}

export default App;
```

`/` 경로로 들어가면 연결되어 있는 Home Component가 잘 뜨는 것을 확인 할 수 있다.

<img src="/media/react/router/router-sample-home.png" width="494" height="360" alt="mashup-todolist ui">

하지만 `/about` 경로로 들어가보면 About 컴포넌트만 나오기를 기대했지만, Home과 About 모두 나타난다.

<img src="/media/react/router/router-sample-about-not-exact.png" width="572" height="560" alt="mashup-todolist ui">

`/about` 경로가 `/` 규칙에도 일치하기 때문에 발생한 현상으로 해당 문제를 수정하려면 Home Route 컴포넌트에 **`exact`** props를 true로 설정하면 된다.

```jsx
const App = () => {
  return (
    <div>
      <Route path='/' component={Home} exact={true} />
      <Route path='/about' component={About}>
    </div>
  );
};
```

<br>

### Link Component를 사용하여 다른 주소로 이동하기

Link Component는 클릭하면 다른 주소로 이동시켜 주는 컴포넌트이다. 일반 웹 애플리케이션에서는 a 태그를 사용하여 페이지를 이동하였지만 리액트에서는 a 태그를 직접 사용하면 안된다. 리액트에서 a 태그를 사용할 경우 페이지를 전환하는 과정에서 페이지를 새로 불러오기 때문에 애플리케이션이 들고 있던 상태들이 모두 날아가게 된다. 렌더링된 컴포넌트들도 모두 사라지고 다시 처음부터 렌더링된다.

Link Component를 사용하여 페이지를 전환하면 페이지 새로고침없이 애플리케이션은 그대로 유지한 채로 HTML5 History API를 사용하여 페이지의 주소만 변경해 준다. Link Component 에는 페이지 전환을 방지하는 기능이 내장되어 있다.

```jsx
<Link to="주소">내용</Link>
```

to에 url path를 입력한다.

```jsx
import React from 'react';
import { Route, Link } from 'react-router-dom';
import Home from './page/Home';
import About from './page/About';

function App() {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" component={Home} exact={true} />
      <Route path="/about" component={About} />
    </div>
  );
}

export default App;
```

<br>

### Route 하나에 여러 개의 path 설정하기

path props 값을 url이 담겨 있는 배열로 설정해주면 하나의 Route에 여러 url path로 연결할 수 있다.

```jsx
import React from 'react';
import { Route, Link } from 'react-router-dom';
import Home from './page/Home';
import About from './page/About';

function App() {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/info">Info</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" component={Home} exact={true} />
      <Route path={['/about', '/info']} component={About} />
    </div>
  );
}

export default App;
```

<br>

### URL 파라미터와 쿼리

페이지에 값을 전달하는 방식은 파라미터와 쿼리로 나눌 수 있다.

- 파라미터: /members/daewoong
- 쿼리: /members?active=true

일반적으로 파라미터는 특정 아이디 혹은 이름을 사용하여 조회할 때 사용하고, 쿼리는 어떤 키워드를 검색하거나 페이지에 필요한 옵션을 전달할 때 사용한다.

**URL 파라미터 사용**

예를 들어 `/member/daewoong` 으로 `username` 값을 전달한다고 할 때 해당 값을 props로 받아와서 조회하는 방법은 아래와 같다.

Member.js

```jsx
import React from 'react';

const data = {
  daewoong: {
    name: '방대웅',
    email: 'daewoong.bang@gmail.com'
  },
  gildong: {
    name: '홍길동',
    email: 'gildong@naver.com'
  }
};

const Member = ({ match }) => {
  const { username } = match.params;
  const member = data[username];
  if (!member) {
    return <div>존재하지 않는 사용자.</div>;
  }
  return (
    <div>
      <h3>
        {username}({member.name})
      </h3>
      <p>{member.email}</p>
    </div>
  );
};

export default Member;
```

URL 파라미터를 사용할 때는 라우터로 사용되는 컴포넌트에서 받아 오는 match라는 객체 안의 params 값을 참조한다. path를 `/member/:username` 이라고 주면 Member Component에서 match.params.username으로 현재 username 값을 조회할 수 있다.

App.js

```jsx
function App() {
  return (
    <div>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/info">Info</Link>
        </li>
        <li>
          <Link to="/member/daewoong">daewoong</Link>
        </li>
      </ul>
      <hr />
      <Route path="/" component={Home} exact={true} />
      <Route path={['/about', '/info']} component={About} />
      {/* Member Component에서 match.params.username 으로 현재 username 조회 가능 */}
      <Route path="/member/:username" component={Member} />
    </div>
  );
}
```

<br>

**URL 쿼리 사용**

- 쿼리는 location 객체에 들어 있는 search 값에서 조회할 수 있다.
- location 객체는 라우트로 사용된 컴포넌트에게 props로 전달되며, 웹 애플리케이션의 현재 주소에 대한 정보를 지니고 있다.

예를 들어 `http://localhost:3000/about?detail=true` 로 접속했을 때 location 형태는 아래와 같다.

**location 형태**

```json
{
  "pathname": "/about",
  "search": "?detail=true",
  "hash": ""
}
```

URL 쿼리를 읽을 때는 위 객체가 지닌 값 중에서 search 값을 확인해야 한다. 이 값은 문자열 형태로 되어있다. URL 쿼리는 `?detail=true&number=1`과 같이 여러가지 값을 설정할 수 있다. search 값에서 여러가지 값 중 특정 값을 읽어 오기 위해서는 이 문자열을 객체 형태로 변환해 주어야 한다.

쿼리 문자열을 객체로 변환할 때는 qs라는 라이브러리를 사용한다.

`yarn add qs`

About Component에서 location.search 값에 있는 detail 값이 true인지 아닌지에 따라 추가 정보를 보여준다고 하면 코드는 아래와 같이 작성할 수 있다.

```jsx
import React from 'react';
import qs from 'qs';

const About = ({ location }) => {
  // 쿼리는 location 객체에 들어 있는 search 값에서 조회할 수 있다.
  // location 객체는 라우트로 사용된 컴포넌트에게 props로 전달되며, 웹 애플리케이션의 현재 주소에 대한 정보를 지니고 있다.
  const query = qs.parse(location.search, {
    ignoreQueryPrefix: true // 이 설정을 통해 문자열 맨 앞의 ? 생략
  });

  const showDetail = query.detail === 'true';

  return (
    <div>
      <h1>About</h1>
      <p>React Router Sample</p>
      {showDetail && <p>detail 값은 true!</p>}
    </div>
  );
};

export default About;
```

쿼리를 사용할 때는 쿼리 문자열을 객체로 파싱하는 과정에서 결과 값은 언제나 문자열로 나온다. `?number=1`로 숫자를 전달한다고 해서 해당 값이 숫자로 넘어오는 것이 아니라 `"1"`과 같이 문자열 형태로 넘어온다. 숫자로 처리해야하는 상황이면 parseInt 함수를 통해 꼭 숫자로 변환해 준다.

<br>

### 서브 라우트

서브 라우트는 라우트 내부에 또 라우트를 정의하는 것을 의미한다. 작성은 그냥 라우트로 사용되고 있는 컴포넌트 내부에 Route 컴포넌트를 또 사용하면 된다.

Member.js

```jsx
const Member = () => {
  return <div>사용자 목록</div>;
};
```

### 리액트 라우터 부가 기능

**history**

history 객체는 라우트로 사용된 컴포넌트에 match, location과 함께 전달되는 props 중 하나로, 이 객체를 통해 컴포넌트 내에 구현하는 메서드에서 라우터 API를 호출할 수 있다. 예를 들어 뒤로 가거나, 로그인 후 화면을 전환하거나, 다른 페이지로 이탈하는 것을 방지해야 할 때 history를 활용할 수 있다.

**withRouter**

withRouter 함수는 HoC(Higher-order Component)이다. 라우트로 사용된 컴포넌트가 아니어도 match, location, history 객체를 접근할 수 있게 해준다.

**Switch**

Switch 컴포넌트는 여러 Route를 감싸서 그 중 일치하는 단 하나의 라우트만을 렌더링시켜 준다. Switch를 사용하면 모든 규칙과 일치하지 않을 때 보여줄 Not Found 페이지도 구현할 수 있다.

**NavLink**

NavLink는 Link와 비슷하다. 현재 경로와 Link에서 사용하는 경로가 일치하는 경우 특정 스타일 혹은 CSS 클래스를 적용할 수 있는 컴포넌트이다.

<hr>

**reference:**

- REACT ROUTER Quick Start  
  <https://reacttraining.com/react-router/web/guides/quick-start>
