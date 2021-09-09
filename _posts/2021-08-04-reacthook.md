---
title: React Hook
tags: react
comments: true
---

### React Hook ###

hook은 class를 작성하지 않고도 state와 react의 다른기능을 사용하게 함.   

hook counter 예제   

```
import React , { useState } from 'react'

const App = () => {

    const [count, setCount] = useState(0);
    const [age, setAge] = useState(42);
    const [fruit, setFruit] = useState('banana');
    const [todos, setTodos] = useState([{text : 'learn hooks'}]);

    return (
        <div>
            <p>you clicked { count } times</p>
            <button onClick={() => setCount(count + 1)}>
                click me!
            </button>
        </div>
    )
}

export default App

```

Hook? 함수 컴포넌트에서 React state와 생명주기 기능을 연동 할수 있게 해주는 함수   
hook은 class안에서는 동작하지 않는 대신 class 없이 react를 사용할수 있게 해줌   


### Effect Hook ###
useEffect 는 함수 컴포넌트 내에서 이런 side effects를 수행할 수 있게 해줌.   
componentDidMount, componentDidUpdate, componentWillUnmount 와 같은 목적으로 제공되지만 하나의 api로 통합된것.

useEffect 함수를 사용하면 dom을 바꾼뒤에 effect 함수를 실행하는 것.   
useState와 마찬가지로 컴포넌트 내에서 여러개의 effect 를 사용할수 있다.

```
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

### custom hook ###
```
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}

function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}

```

### 다른 내장 hook ###

useContext   
컴포넌트 중청 없이 react context 를 구독 할 수 있게 해준다.
```
function Example() {
  const locale = useContext(LocaleContext);
  const theme = useContext(ThemeContext);
  // ...
}
```

useReducer는 복잡한 컴포넌트들의 state 를 reducer로 관리할수 있게 해준다. 
```
function Todos() {
  const [todos, dispatch] = useReducer(todosReducer);
  // ...
```