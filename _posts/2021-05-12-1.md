---
title: React 기본 컴포넌트
tags: react
comments: true
---

출처 : https://medium.com/little-big-programming

ui 를 구성하기 위해서는 화면에 컴포넌트를 
그리고 (Mounting)
갱신하고 (Updating)
지워야(Unmounting) 한다.

컴포넌트는 각 프로세스가 진행될 때에 따라 Lifecycle함수로 불리는 특별한 함수가 실행됨.
- <img src="/assets/img/reactlifecycle.jpeg" />

React 컴포넌트의 props와 state
: 컴포넌트는 두가지 인스턴스 속성인 props와 state를 가지고 있다.
: props는 컴포넌트의 mounting, updating프로세스 시점에 값이 할당될뿐 컴포넌트 내부에서는 값을 변경할 수가 없음. 
: 상황에 따라 변경되어야 하는 값들은 state를 사용하여야함.
