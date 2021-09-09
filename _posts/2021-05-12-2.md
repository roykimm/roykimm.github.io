---
title: React 함수형 컴포넌트 정의
tags: react
comments: true
---

출처 : https://velopert.com/2994

React 에서 컴포넌트를 정의할때는 ecmascript6에 도입된 클래스 문법을 사용.

```
import React, {Component} from react;

class Test extends React.Component() {
    render() {
        return (
            <div> Hello { this.props.name } </div>
        )
    }
}
export default Test;
```

라이프사이클API도 사용하지 않고, state도 사용하지 않고 그냥 props만 전달받아 뷰를 렌더링 해주는 역할이라면 함수형 컴포넌트로 컴포넌트를 정의 할수 있음.

```
import React from 'react';

function Test(props){

    return (
        <div> hello {props.name}</div>
    )
}
export default Test;

```

es6의 화살표 함수를 사용한 경우

```
import React from 'react;

const Test = () => {
    return (
        <div> Hello {props.name} </div>
    )
}
export default Test;

```