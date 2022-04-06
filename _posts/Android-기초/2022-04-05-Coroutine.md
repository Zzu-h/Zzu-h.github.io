---
layout: post
title: "Android: Coroutine"
date: 2022-04-05
excerpt: "Android Coroutine"
tags: [UMC, Coroutine, Android]
comments: true
categories : Android-기초
---

> 아래 영상을 참고하여 학습했습니다.

<iframe width="722" height="406" src="https://www.youtube.com/embed/xSgZS9e3qCU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# 🎯 Coroutine
- 탭으코루틴은 비동기적으로 실행되는 코드를 간소화하기 위해 Android에서 사용할 수 있는 동시 실행 설계 패턴
    - 비동기 경량 스레드
- 기존 Android에서는 AsyncTask를 이용해서 비동기 프로그래밍을 구현할 수 있었다. 
    - 하지만, 메모리 누수 등 메모리 관련 문제로 인해 API 30부터 Deprecated가 되었다.
- 위에 대한 대체제로 코루틴을 지원하기 시작했으며, 안드로이드 공식문서에서도 이를 권장하고 있다.
- 코루틴은 단순 안드로이드만의 개념이 아닌 프로그래밍 언어에서 구현되어 제공되어지고 있다.

## 루틴
- 프로그램의 흐름 그 자체
- 루틴은 크게 다음과 같이 나눌 수 있다.
    - 메인 루틴
        - 프로그램의 실행에 있어 메인이 되는 루틴
    - 서브 루틴
        - 메인 루틴 안에서 실행되는 개별 함수에 의해 수행되는 흐름

## 코루틴의 장점
[공식 문서](https://developer.android.com/kotlin/coroutines?hl=ko&gclid=CjwKCAjw0a-SBhBkEiwApljU0tHstXN8UF_r4wcV_lF5C12tvAWvs5odkcXDztnuQbr1ISsER72w-hoCoSoQAvD_BwE&gclsrc=aw.ds)에서는 다음의 장점이 있다고 한다.
- 경량: 코루틴을 실행 중인 스레드를 차단하지 않는 정지를 지원하므로 단일 스레드에서 많은 코루틴을 실행할 수 있습니다. 정지는 많은 동시 작업을 지원하면서도 차단보다 메모리를 절약합니다.
- 메모리 누수 감소: 구조화된 동시 실행을 사용하여 범위 내에서 작업을 실행합니다.
- 취소 지원: 실행 중인 코루틴 계층 구조를 통해 자동으로 취소가 전달됩니다.
- Jetpack 통합: 많은 Jetpack 라이브러리에 코루틴을 완전히 지원하는 확장 프로그램이 포함되어 있습니다. 일부 라이브러리는 구조화된 동시 실행에 사용할 수 있는 자체 코루틴 범위도 제공합니다.

## 코루틴 특징
- 쓰레드와는 달리 작업을 제어할 수 있다.
    - 루틴이 이미 실행되었더라도 잠시 suspend를 시킬 수 있으며
    - Resume을 통해 다시 재시작을 하는 흐름 제어가 가능하다.
- ANR 문제를 해결할 수 있다.
- 메모리 경량
    - 쓰레드는 각 쓰레드마다 Stack영역을 할당받게 되며, 이는 적지 않은 공간을 차지한다.
    - 하지만, 코루틴은 공유된 Heap 영역을 사용하기에 메모리 부담이 훨씬 덜한다.
- 오버헤드 경량
    - 쓰레드는 처리해야할 쓰레드로 교체하는 Context Switching이 발생하게 된다.
    - 하지만 코루틴은 같은 메모리를 두고 있기에 Context Switching 자체가 필요없다.


## 코루틴과 스레드의 차이
### 메모리 구조의 차이
- 쓰레드
    - 메모리 영역 중 Stack의 공간을 차지한다.
- 코루틴
    - 프로세스에 할당된 Heap 메모리 영역을 공유한다.
- 위의 차이로 인해 코루틴은 하나의 쓰레드로 보기는 어렵다.
    - 하나의 함수로 보는 것이 더 가까움

### 수행방식의 차이
- 쓰레드
    - 선점형
- 코루틴
    - 비선점형
    - 다만, 스위칭이 매우 빠르게 동작한다.
        - 이로인해 **동시성은 있으나, 병행성은 없다.**

## 코루틴 구조
- Coroutine Scope
    - 코루틴이 어떤 범위에서 동작하는지 규정
    - Global Scope
        - Coroutine Scope 중 한 종류
        - Android에서는 App LifeCycle에 따라 이를 비추천한다.
            - 코루틴은 최상위 레벨, SingleTone에서 동작하게 되는데 Global Scope는 이를 위반하기 때문
- Coroutine Context
    - 코루틴이 어떤 쓰레드에서 동작할지 결정
- Coroutine Builder
    - 다양한 요구사항에 맞게 개별적인 Coroutine(코루틴)을 만드는 방법
### Coroutine Context
Context는 Dispatchers와 Job으로 구성되어 있음
- Dispatchers: 코루틴이 실행되는 쓰레드를 지정
    - Default
        - CPU 연산을 많이 사용하는 작업
    - IO
        - File IO, Network IO 등에서 많이 사용되는 작업
    - Main
        - UI 쓰레드, UI 관련 작업
    - Unconfined
- Job & Deferred(결과값을 가지는 Job)
    - 코루틴에서는 하나의 흐름을 Job & Deferred라는 Object로 만들어서 다룸

### Coroutine Builder
- launch
    - 일반적으로 자주 사용
    - Job 객체 반환
- async
    - Deferred 객체 반환
    - async 내부에서 작업하고 마지막에 남은 결과값을 반환한다.
- runBlocking
    - 이는 사용하지 않음
- withContext
    - Dispatcher를 전환싵킴
    - Main으로 실행시킨 Coroutine의 일부를 IO로 전환하여 작업하고 다시 돌아올 수 있다.

## 코루틴 지연
- delay
    - 일정 시간동안 코루틴 처리가 중단
    - 쓰레드 sleep과 유사하지만 delay는 숫자를 세면서 대기를 하는 상태임
- join
    - await과 사용하는 Builder가 다름
    - launch로 실행한 코루틴에 대해서 Job의 실행이 끝날 때까지 대기시킬 수 있음
- await
    - join과 사용하는 Builder가 다름
    - async로 실행한 코루틴에 대해서 Job의 실행이 끝날 때까지 대기시킬 수 있음

## 코루틴 취소
- cancel
    - Job을 Cancelling State로 전환
- cancelAndJoin
    - Cancelling State로 전환하고 Cancelled까지 기다림
- withTimout
    - 일정 시간동안 기다리며 초과 시 Exception을 던짐
- withTimoutOrNull
    - 일정 시간동안 기다리며 초과 시 Null을 던짐

## ANR 문제(Activity not response)
![anr](https://developer.android.com/topic/performance/images/anr-example-framed.png?hl=ko)

- 액티비티가 응답하지 않는 오류 상황
- 액티비티로 구성된 화면은 사용자 이벤트에 5초 이내로 반응하지 않으면 오류가 발생된다.
- 메인 스레드가 오래 걸리는 작업이라고 하더라도 ANR 문제가 발생하지 않는다.
    - 사용자가 이벤트를 발생시켰을 때 발생함
- 본래 Thread를 통해 이를 해결할 수 있다.
    - 하지만 앞서 간단한 IO 작업 등과 같이 단순 작업에도 메모리 부하와 오버헤드가 든다.
    - 이를 더욱 간편하게 해결할 수 있는 것이 Coroutine이다.