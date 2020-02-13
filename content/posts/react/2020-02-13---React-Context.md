---
title: React Context
date: '2020-02-13 14:05:00'
template: 'post'
draft: false
slug: 'react-context'
category: 'React'
tags:
  - 'React'
  - 'Context'
description: 'context API를 사용하여 데이터 전역으로 관리하기'
socialImage: ''
---

React 프로젝트에서 사용자 정보와 같은 전역적으로 필요한 데이터를 관리해야 할 때는 어떻게 해야 할까? 일반적인 React 애플리케이션에서 데이터는 부모에서 자식으로 props를 통해 전달되지만 애플리케이션 안의 여러 컴포넌트들에 전달해줘야 하는 props의 경우 컴포넌트 여기저기에 전달하기에는 이 과정이 번거로울 수 있고 컴포넌트가 많아질 수록 더욱 복잡해지고 유지 보수하기가 힘들어질 수 있다.

이런 경우 **context API**를 이용하면 컴포넌트들에 일일이 props를 넘겨주지 않고도 컴포넌트 트리 전체에 데이터를 제공할 수 있다. context는 React 16.3 버전 이후 부터 많이 개선되어 전보다 사용하기가 훨씬 쉬워졌다.

## API

### React.createContext

```jsx
const MyContext = React.createContext(defaultValue);
```

- **createContext** 함수를 통해 Context를 만들 수 있다.
- Context 객체를 구독하고 있는 컴포넌트를 렌더링할 때 React는 트리 상위에서 가장 가까이 있는 Provider로부터 현재 값을 얻는다.
- 적절한 Provider를 찾지 못했을 경우 defaultValue 값을 사용한다.

**context 만들기 샘플코드 (color.js)**

```jsx
import { createContext } from 'react';

const ColorContext = createContext({ color: 'black' });

export default ColorContext;
```

<br>

### Context.Consumer

```jsx
<MyContext.Consumer>
  {value => /* context 값을 이용한 렌더링 */}
</MyContext.Consumer>
```

- context 변화를 구독하는 React 컴포넌트
- 함수 컴포넌트 안에서 context를 읽기 위해서 쓸 수 있다.
- Context.Consumer의 children은 함수여야 한다.
- 이 함수는 context의 현재 값을 받고 React 노드를 반환한다.
- 이 함수가 받는 value 값은 해당 context의 Provider 중 상위 트리에서 가장 가까운 Provider의 value prop과 동일하다.
- 상위에 Provider가 없다면 value 매개변수 값은 createContext()에 보냈던 defaultValue 값이다.

**Consumer 사용하기 샘플코드(ColorBox.js)**

```jsx
import React from 'react';
import ColorContext from '../contexts/color';

const ColorBox = () => {
  // Render Props
  // ColorContext 안에 있는 Consumer를 통해 context의 색상을 조회한다.
  return (
    <ColorContext.Consumer>
      {value => (
        <div
          style={{ width: '64px', height: '64px', background: value.color }}
        />
      )}
    </ColorContext.Consumer>
  );
};

export default ColorBox;
```

**\* Consumer 대신 useContext 사용하여 값 받아오기**

```jsx
import React, { useContext } from 'react';
import ColorContext from '../contexts/color';

const ColorBox = () => {
  const { state } = useContext(ColorContext);
  return (
    <>
      <div
        style={{
          width: '64px',
          height: '64px',
          background: state.color
        }}
      />
      <div
        style={{
          width: '32px',
          height: '32px',
          background: state.subcolor
        }}
      />
    </>
  );
};

export default ColorBox;
```

<br>

### Context.Provider

```jsx
<MyContext.Provider value={/* 어떤 값 */}>
```

- Provider를 사용하면 Context의 value 값을 변경할 수 있다.
- context를 구독하는 컴포넌트들에게 context의 변화를 알리는 역할을 한다.
- Provider는 value prop를 받아서 이 값을 하위에 있는 컴포넌트에게 전달한다.(값을 전달받을 수 있는 컴포넌트 수에 제한은 없다.)
- Provider 하위에 또 다른 Provider를 배치하는 것도 가능하며, 이 경우 하위 Provider의 값이 우선시된다.
- Provider 하위에서 context를 구독하는 모든 컴포넌트는 Provider의 value prop가 바뀔 때마다 다시 렌더링된다.
- createContext 함수의 defaultValue는 Provider를 사용하지 않았을 때만 사용된다. 만약 Provider를 사용했는데 value를 명시하지 않았다면 에러가 발생한다.

**Provider 사용하기 샘플코드 (App.js)**

```jsx
import React from 'react';
import ColorBox from './components/ColorBox';
import ColorContext from './contexts/color';

function App() {
  return (
    <ColorContext.Provider value={{ color: 'blue' }}>
      <div>
        <ColorBox />
      </div>
    </ColorContext.Provider>
  );
}

export default App;
```

<hr>

## 값이 변하는 context

theme 값에 따라 변하는 button style

**theme-context.js**

```jsx
import { createContext } from 'react';

export const themes = {
  light: {
    foreground: '#000000',
    background: '#eeeeee',
    color: 'black'
  },
  dark: {
    foreground: '#ffffff',
    background: '#222222',
    color: 'white'
  }
};

export const ThemeContext = createContext(themes.dark);
```

**themed-button.js**

```jsx
import React, { useContext } from 'react';
import { ThemeContext } from '../contexts/theme-context';

const ThemedButton = props => {
  const { background, color } = useContext(ThemeContext);
  return <button {...props} style={{ background: background, color: color }} />;
};

export default ThemedButton;
```

**App.js**

```jsx
import React, { useState } from 'react';
import { ThemeContext, themes } from './contexts/theme-context';
import ThemedButton from './components/themed-button';

function App() {
  const [theme, setTheme] = useState(themes.dark);

  const toggleTheme = () => {
    setTheme(theme === themes.light ? themes.dark : themes.light);
  };

  return (
    <ThemeContext.Provider value={theme}>
      <ThemedButton onClick={toggleTheme}>Change Theme</ThemedButton>
    </ThemeContext.Provider>
  );
}

export default App;
```

<hr>

**reference:**

- Context  
  <https://ko.reactjs.org/docs/context.html#when-to-use-context>
