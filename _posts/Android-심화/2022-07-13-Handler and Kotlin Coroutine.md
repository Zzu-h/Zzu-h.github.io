---
layout: post
title: "Android: Handler & Kotlin Coroutine"
date: 2022-07-13
excerpt: "Handler and Kotlin Coroutine"
comments: true
categories : Android-심화
---

# Handler and Kotlin Coroutine

## Responsiveness
- ANR
    - Android에서는 Activity Manager 및 Window Manager 시스템 서비스에서 모니터링을 수행한다.
    - 다음 2개의 조건 중 하나를 감지하면 ANR문제가 야기되었음을 알린다.
        1. 입력 이벤트에 5초 이내 응답 없음
        2. BroadcastReceiver가 10초 내에 실행을 완료하지 못함
- 해결 방법
    - AsyncTask
        - Deprecated됨
    - background Thread / HandlerThread
    - communicating back with UI
## 유형별 Background Service
![background](https://developer.android.com/static/images/guide/background/background.svg)
- [참고](https://developer.android.com/guide/background)

## Process와 Thread
### Process
- Linux 프로세스가 안드로이드 앱을 실행하며, 시스템이 그 프로세스의 lifecycle을 관리한다.
    - 주요 구성요소가 프로세스에 미치는 영향을 고려해야 함
        - 주요 구성요소: Activity, Service, etc..
- Process type
    - foreground process 
    - visible process 
    - service process 
    - cached process 
        - onStop 상태에서 오랫동안 머무르는 프로세스
    - [참고](https://developer.android.com/guide/components/activities/process-lifecycle)
- application 내의 구성 요소가 별도의 process에서 싫행되도록 지정할 수 있음
    - manifest에 process를 직접 지정가능

### Thread
- 프로세스는 최소한 하나의 스레드를 포함
    - Main Thread ++
- 프로세스가 실행할 일련의 명령
    - app은 main thread를 생성해 실행
    - background / workthread

#### Background -> UI
- method
    - runOnUiThread
    - post
    - postdelayed

## Handelr, Message, Runnable
[참고](https://zzu-h.github.io/Handler&Looper/)
### Runnable
> SEM(Single Extract Method): 하나의 메소드만을 제공하는 인터페이스

## 동시성과 병렬성
![image](https://user-images.githubusercontent.com/72387349/189482013-fc00bd11-9228-41a9-ae8a-ca4acbf63f44.png)

- 병렬성은 동시성을 의미하지만, 동시성은 병렬성없이도 발생할 수 있다.

### Race condition
- 코드를 동시성으로 작성했지만, 특정한 순서로 실행될 것으로 가정하는 경우
- 동시성 코드가 제대로 작동하기 위해 일정한 순서로 완료돼야하는 경우

## Kotlin Coroutine
- suspend
    - 해당 스레드를 차단하지 않고 실행을 일시 중지할 수 있는 연산
    - 일시 중단된 스레드를 다시 시작하기 전까지, 다른 연산에서 사용할 수 있음
- launch / async

### Dispatchers