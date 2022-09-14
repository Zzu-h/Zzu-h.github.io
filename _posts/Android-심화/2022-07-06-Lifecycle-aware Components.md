---
layout: post
title: "Android: Lifecycle-aware Components"
date: 2022-07-06
excerpt: "Lifecycle-aware Components"
comments: true
categories : Android-심화
---

# Lifecycle-aware Components
## Lifecycle
<img width="771" alt="lifecycle" src="https://user-images.githubusercontent.com/72387349/189464752-15883ef1-7e3e-47d0-ae10-557eaad49497.png">

- Activity, Fragment와 같은 컴포넌트의 Lifecycle 정보를 다른 객체가 관찰이 가능하다.
- Event
    - Initialized에서 Created로 상태가 변경될 때처럼 상태가 변경될 때 callback이 호출된다.
- State
    - 말 그대로 현재의 상태

- Lifecycle은 타입으로도 존재함
> Event-Driven Development: 상태 변경에 따라 처리해야 할 작업들을 적재적소 적용하는 방식의 프로그래밍

## LifecycleOwner
- LifecycleOwner는 lifecycle을 반환하는 단일 메서드 인터페이스이다.
- Lifecycle을 제공하는 제공자를 반환한다.
- 코드를 한번 확인할 필요가 있음