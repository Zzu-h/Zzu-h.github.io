---
layout: post
title: "Android: MediaPlayer"
date: 2022-04-13
excerpt: "Android MediaPlayer"
tags: [UMC, MediaPlayer, Android]
comments: true
categories : Android-기초
---

# MediaPlayer
- 다양한 일반 미디어 유형의 재생을 지원
- 오디오, 동영상, 이미지를 애플리케이션에 쉽게 통합
- 리소스에 저장된 미디어 파일, 파일 시스템의 독립형 파일 또는 네트워크를 통해 들어오는 데이터 스트림 모두
    - MediaPlayer API를 사용하여 오디오 또는 동영상을 재생

## MediaPlayer 클래스
- 최소한의 설정으로 오디오와 동영상을 가져오고 디코딩하며 재생
- 다음 미디어 소스를 지원
    - 로컬 리소스
    - Content Resolver에서 가져올 수 있는 것과 같은 내부 URI
    - 외부 URL(스트리밍)

### 상태
![img](https://developer.android.com/images/mediaplayer_state_diagram.gif)

### 기본 사용법
- 간단한 리소스 음원을 재생하는 예
    ```kotlin
    var mediaPlayer: MediaPlayer? = MediaPlayer.create(context, R.raw.sound_file_1)
    mediaPlayer?.start() // no need to call prepare(); create() does that for you
    ```
- 외장 메모리나 서버에 있을 때: setDataSource()
    ```kotlin
    mediaPlayer = MediaPlayer()
    mediaPlayer.setDataSource(this, Uri.parse(url))
    ```

### 음원 로딩
- 음원을 로드하는데 시간이 걸릴 수 있다.
    - 이때, 두 개의 메소드를 이용이 가능함
        - prepare()
            - 재생을 준비하는 동안 다른 작업을 수행하지 못함
        - prepareAsync()
            - 다른 작업을 함께 수행할 수 있음
- 음원이 로딩이 된 후에는 OnPreparedListener가 반응한다.

### 리스너
- OnCompletionListener
    - 미디어 재생 완료 시 이벤트
- OnErrorListener
    - 재생 에러 발생
- OnPreparedListener
    - 재생 가능한 상태로 로딩된 상태
- OnBufferingUpdateListener
    - 버퍼링 이벤트
- OnVideoSizeChangedListener
    - 비디오 크기 변경 이벤트

### 리소스 해제
```kotlin
mediaPlayer?.release()
mediaPlayer = null
```

## 제공 메소드
- start()
    - 재생 시작
- stop()
    - 정지
- prepare()
    - 준비
- pause()
    - 일시 정지
- release()
    - 메모리 해제
- seekTo()
    - 재생 위치 지정
- getCurrentPosition()
    - 재생 위치
- getDuration()
    - 재생 시간
- getVideoHeight()
    - 영상 높이값
- getVideoWidth()
    - 영상 너비값
- setLooping()
    - 반복 설정
- setVolumn()
    - 볼륨 설정