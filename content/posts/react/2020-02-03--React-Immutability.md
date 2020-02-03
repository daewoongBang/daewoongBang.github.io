---
title: React Immutability
date: '2020-02-03 09:05:00'
template: 'post'
draft: false
slug: 'react-immutability'
category: 'React'
tags:
  - 'React'
  - 'Immutability'
description: 'immer를 사용하여 React 불변성 쉽게 유지하기'
socialImage: ''
---

React에서 **불변성을 지킨다**는 것은 기존의 값을 수정하지 않으면서 새로운 값을 만들어 내는 것을 말한다. 불변성이 지켜지지 않으면 객체 내부의 값이 새로워져도 기존의 값과 비교할 수가 없으므로 바뀐 것을 감지하지 못한다. 아래는 불변성을 지키는 간단한 예시 코드이다.

```javascript
// 기존 배열
const array = [0, 1, 2, 3, 4, 5];

const nextArray = array; // 배열을 복사하는 것이 아닌 똑같은 배열을 가리킨다.
nextArray[0] = 7;
console.log(array === nextArray); // 완전히 같은 배열이기 때문에 true

const nextArray2 = [...array]; // 배열 내부의 값을 모두 복사
nextArray2[0] = 7;
console.log(array === nextArray2); // 다른 배열이기 때문에 false

// 기존 객체
const object = {
  value: 1
};

const nextObject = object; // 객체가 복사되지 않고 똑같은 객체를 가리킨다.
nextObject.value = nexrObject.value + 1;
console.log(object === nextObject); // 같은 객체이기 때문에 true

const nextObject2 = {
  ...object, // 기존에 있던 내용 모두 복사
  value: object.value + 1 // 새로운 값
};
console.log(object === nextObject2); // 다른 객체이기 때문에 false
```

하지만 **전개 연산자(...문법)**를 사용한다고 해서 객체나 배열 내부의 값을 완전히 복사하지 못한다. 이 것을 **얕은 복사**라고 하는데 즉, 내부의 값이 완전히 새로 복사되는 것이 아니라 가장 바깥 쪽에 있는 값만 복사가 된다. 따라서 내부의 값이 배열 혹은 객체가 포함되어 있다면 이 값 또한 따로 복사해 주어야 한다. 아래는 예시 코드이다.

```javascript
const todos = [{ id:1, checked: true},{ id:2, checked: false}];
const nextTodos = [...todos];

nextTodos[0].checked = false;
console.log(todos[0] === nextTodos); // 얕은 복사이므로 똑같은 객체를 가리키고 있어 true

nextTodos[0] = {
    nextTodos[0],
    checked: false
};
console.log(todos[0] === nextTodos); // 새로운 객체를 할당했기 때문에 true
```

구조가 간단하다면 이렇게 불변성을 유지하는데 크게 어려움이 없겠지만 구조가 복잡해질수록 이렇게 불변성을 유지하면서 업데이트하는 것이 정말 까다로워진다. 이때 immer라는 라이브러리를 사용하면 더 쉽게 불변성을 유지할 수 있다.

우선 프로젝트에 immer를 적용하고 싶을 경우 immer를 설치해야 한다.

`yarn add immer`

아래는 immer를 사용하여 불변성을 유지하는 예시 코드 이다.

```jsx
import produce from 'immer';

const originalState = [
  {
    id: 1,
    todo: 'React 공부하기',
    checked: true
  },
  {
    id: 2,
    todo: 'Blog 포스팅',
    checked: false
  }
];

const nextState = produce(originalState, draft => {
  // id가 2인 항목의 checkd 값을 true로 변경
  const todo = draft.find(t => t.id === 2);
  todo.checked = true;

  // 새로운 데이터 추가
  draft.push({
    id: 3,
    todo: 'immer 사용해보기',
    checked: true
  });

  // id가 1인 항목 제거
  draft.splice(
    draft.findIndex(t => t.id === 1),
    1
  );
});
```

**produce** 라는 함수는 두 가지 파라미터를 받는다. 첫 번째는 **수정하고 싶은 상태**이고, 두 번째는 **상태를 어떻게 업데이트할지 정의하는 함수**이다. 두 번째 파라미터로 전달되는 함수 내부에서 원하는 값을 변경하면, produce 함수가 불변성 유지를 대신해주면서 새로운 상태를 생성해준다. 코드만 보면 불변성 관리를 하지 않는 것처럼 보이지만 복잡한 데이터도 불변성 관리를 제대로 해준다.

아래는 컴포넌트에 immer를 적용한 코드이다.

```jsx
import React, { useRef, useCallback, useState } from 'react';
import produce from 'immer';

function FormUseImmer() {
  const nextId = useRef(1);
  const [form, setForm] = useState({ name: '', username: '' });
  const [data, setData] = useState({
    array: [],
    uselessValue: null
  });

  // input 수정
  const onChange = useCallback(
    e => {
      const { name, value } = e.target;
      // 기존 코드 주석 처리
      //   setForm({
      //     ...form,
      //     [name]: [value]
      //   });
      // immer를 적용한 코드
      setForm(
        produce(form, draft => {
          draft[name] = value;
        })
      );
    },
    [form]
  );

  // form 등록
  const onSubmit = useCallback(
    e => {
      e.preventDefault();
      const info = {
        id: nextId.current,
        name: form.name,
        username: form.username
      };

      // array에 새 항목 등록
      // 기존 코드 주석 처리
      //   setData({
      //     ...data,
      //     array: data.array.concat(info)
      //   });
      // immer를 적용한 코드
      setData(
        produce(data, draft => {
          draft.array.push(info);
        })
      );

      // form 초기화
      setForm({
        name: '',
        username: ''
      });
      nextId.current += 1;
    },
    [data, form.name, form.username]
  );

  // 항목 삭제
  const onRemove = useCallback(
    id => {
      // 기존 코드 주석 처리
      //   setData({
      //     ...data,
      //     array: data.array.filter(info => info.id !== id)
      //   });
      // immer를 적용한 코드
      setData(
        produce(data, draft => {
          draft.array.splice(
            draft.array.findIndex(info => info.id === id),
            1
          );
        })
      );
    },
    [data]
  );

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          placeholder="ID"
          value={form.username}
          onChange={onChange}
        />
        <input
          name="name"
          placeholder="NAME"
          value={form.name}
          onChange={onChange}
        />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map(info => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.username} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default FormUseImmer;
```

immer를 사용하면 객체 안에 있는 값을 직접 수정하거나, 배열에 직접적인 변화를 일으키는 push, splice 등의 함수를 사용해도 괜찮다. 하지만 immer를 사용한다고 해서 무조건 코드가 깔끔해지는 것은 아니다. onRemove의 경우 filter를 사용하는 것이 더 간단할 수 있으므로 immer는 불변성을 유지하기가 너무 복잡할 경우에만 사용해도 충분하다.

### useState의 함수형 업데이트와 immer 함께 쓰기

immer와 useState의 함수형 업데이트를 함께 사용하면 코드를 더욱 깔끔하게 만들 수 있다.

우선 함수형 업데이트는 함수가 계속 만들어지는 상황을 방지하기 위해 사용한다. 이런 상황을 방지하는 방법은 두 가지이다. 하나는 방금 이야기한 useState의 함수형 업데이트 기능을 사용하는 것이고, 두 번째는 useReducer를 사용하는 것이다. 아래 코드는 useState의 함수형 업데이트 예시이다.

```jsx
const [number, setNumber] = useState(0);
// prevNumber는 현재 number값을 가리킨다.
const onIncrease = useCallback(
  () => setNumber(prevNumber => prevNumber + 1),
  []
);
```

`setNumber(number + 1)` 이라고 하지 않고 어떻게 업데이트할지 정의해주는 업데이트 함수를 넣어주었다. 이렇게 정의할 경우 useCallback을 사용할 때 두 번째 파라미터 배열에 number를 넣지 않아도 된다. 아래는 FormUseImmer 컴포넌트를 함수형 업데이트로 변경한 코드이다.

```jsx
import React, { useRef, useCallback, useState } from 'react';
import produce from 'immer';

function FormUseImmerFU() {
  const nextId = useRef(1);
  const [form, setForm] = useState({ name: '', username: '' });
  const [data, setData] = useState({
    array: [],
    uselessValue: null
  });

  // input 수정
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    //   setForm(
    //     produce(form, draft => {
    //       draft[name] = value;
    //     })
    //   );
    // 함수형 업데이트로 변경
    setForm(
      produce(draft => {
        draft[name] = value;
      })
    );
  }, []);

  // form 등록
  const onSubmit = useCallback(
    e => {
      e.preventDefault();
      const info = {
        id: nextId.current,
        name: form.name,
        username: form.username
      };

      setData(
        // 함수형 업데이트로 변경
        produce(draft => {
          draft.array.push(info);
        })
      );

      // form 초기화
      setForm({
        name: '',
        username: ''
      });
      nextId.current += 1;
    },
    [form.name, form.username]
  );

  // 항목 삭제
  const onRemove = useCallback(id => {
    setData(
      // 함수형 업데이트로 변경
      produce(draft => {
        draft.array.splice(
          draft.array.findIndex(info => info.id === id),
          1
        );
      })
    );
  }, []);

  return (
    <div>
      <form onSubmit={onSubmit}>
        <input
          name="username"
          placeholder="ID"
          value={form.username}
          onChange={onChange}
        />
        <input
          name="name"
          placeholder="NAME"
          value={form.name}
          onChange={onChange}
        />
        <button type="submit">등록</button>
      </form>
      <div>
        <ul>
          {data.array.map(info => (
            <li key={info.id} onClick={() => onRemove(info.id)}>
              {info.username} ({info.name})
            </li>
          ))}
        </ul>
      </div>
    </div>
  );
}

export default FormUseImmerFU;
```

immer는 컴포넌트의 불변성을 유지하기가 까다로운 경우에 사용한다면 생산성을 크게 높일 수 있다. 그렇지 않을 경우에는 immer를 사용하지 않는 경우가 더 편한 경우도 있으므로 사용 전 충분히 고려해보도록 하자.

<hr>

**reference:**

- immer docs  
  <https://immerjs.github.io/immer/docs/introduction>
