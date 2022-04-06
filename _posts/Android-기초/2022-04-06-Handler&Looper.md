---
layout: post
title: "Android: Handler And Looper"
date: 2022-04-06
excerpt: "Android Handler And Looper"
tags: [UMC, Handler, Looper, Android]
comments: true
categories : Android-기초
---

# Handler & Looper
- Handler와 Looper를 따로 정리하기엔 복잡해서 한번에 정리하고자 한다.
- MainThread에는 내부적으로 Looper를 가지고 있고, 그 안에는 MessageQueue를 포함하고 있다.
- Looper가 MessageQueue에 message나 Runnable 객체를 FIFO로 보관하고, 차례로 꺼낸다.
- 그리고 Handler는 Looper로 부터 전달 받은 message나 Runnable를 처리하거나 다른 Thread로 부터 받은 message를 MessageQueue에 넣는다.

![img](https://miro.medium.com/max/1000/1*cPvR6xzW8oSMhcUCaJNZ4w.png)

## 🎯 Looper
- Looper는 하나의 스레드만을 담당
- 한 스레드도 오직 하나의 Looper만을 가짐
- Looper는 MessageQueue가 비어있는 동안은 아무 행동도 안함
    - 메시지가 들어오면 해당 메시지를 꺼내 Handler로 전달

### Message
- '하나의 작은 작업 단위' 
- Message 객체는 내용물이 두 가지로 나눌 수 있다.
    - Runnable
    - Message 
- Looper 객체가 메세지 큐에서 메세지를 하나 꺼낼 때,
    - Runnable 객체가 담겨져있으면 Handler에 메세지를 전달하지 않고 run()을 수행
        - 해당 Runnble 작업을 바로 시작
    - Runnable 객체가 없을 경우
        - Message 객체 내부에 명시돼있는 Handler 의 handleMessage()를 수행

## 🎯 Handler
- 특정 메세지를 Looper의 MessageQueue에 넣거나, Looper가 MessageQueue에서 특정 메세지를 꺼내어 전달하면 이를 처리하는 기능

### Looper 로 메세지를 전달하는 경우
- Message 객체를 생성하여 이를 전달하는 방식으로 구현한다.
    - sendMessage() 메소드를 통해 메세지 큐에 Message 객체를 적재할 수 있다.
    - post로 시작하는 메소드들을 통해 Runnable 객체를 직접 적재할 수 있다.

### Looper 로부터 메세지를 전달받는 경우
- Looper 가 메세지 큐에서 메세지 하나를 딱 꺼냈을 때,
    - Runnable 객체가 담겨있다면
        - 해당 Runnable의 run() 메소드를 호출하여 작업을 실행할 수 있다.
    - Message 객체가 담겨있다면
        - 해당 메세지 내부의 Handler가 갖고 있는 handleMessage() 메소드를 호출함으로써 해당 Handler 가 메세지를 전달받을 수 있다

## 생성하기
```java
class LooperThread extends Thread {
       public Handler mHandler;
 
       public void run() {
           Looper.prepare();
 
           mHandler = new Handler(Looper.myLooper()) {
               public void handleMessage(Message msg) {
                   // process incoming messages here
               }
           };
 
           Looper.loop();
       }
   }
```
- Looper.prepare()
    - 해당 쓰레드에 종속되는 Looper와 MessageQueue를 준비
- mHandler = new Handler
    - Handler를 생성
    - 이 때, Handler와 Looper가 연결
    - 기존에는 Handler()에 Looper를 담지 않아도 암시적으로 Looper를 선택하게 되었다.
        - 하지만, 이 과정에서 특정 작업의 손실 등의 문제로 Deprecated가 되었다.
        - 따라서, 위처럼 명시를 해주어야 한다.
- Looper.loop()
    - Message 전달을 기다리는 동작을 시작