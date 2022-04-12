---
layout: post
title: "Android: SharedPreferences"
date: 2022-04-13
excerpt: "Android SharedPreferences"
tags: [UMC, SharedPreferences, Android]
comments: true
categories : Android-기초
---

# SharedPreferences
- 키-값 쌍이 포함된 파일
- 키-값 쌍을 읽고 쓸 수 있는 간단한 메서드를 제공
- 간단한 값 저장에 DB를 사용하기에는 복잡하기 때문에 SharedPreferences를 사용하면 적합함
- 어플리케이션에 파일 형태로 데이터를 저장
- 어플리케이션이 삭제되기 전까지 보존된다.

## SharedPreferences 객체 얻기
- SharedPreferences 객체는 두 개의 메소드를 통해서 얻을 수 있다.
    - getSharedPreferences()
        - 이름으로 식별되는 SharedPreferences 파일이 여러 개 필요한 경우 이 메서드를 사용
        - 이름은 첫 번째 매개변수로 지정함
        - 앱의 모든 Context에서 이 메서드를 호출할 수 있음
    - getPreferences()
        - 하나의 Activity에서만 사용하는 객체를 생성한다.
        - 이 메서드는 Activity에 속한 기본 SharedPreferences 파일을 검색하기 때문에 이름을 제공할 필요가 없습니다.

## SharedPreferences 데이터 저장
- 데이터를 저장하기 위해선 Editor를 얻어야 함
    - `val edit = sharedPreference.edit()`
- Editor를 통해서 작성하고 저장을 한다.
- 작성 후 apply 또는 commit을 하여 저장을 꼭 해야 한다.
    > - apply는 비동기처리 commit은 동기 처리
- 사용
    ```kotlin
    val sharedPreference = getSharedPreferences("song", MODE_PRIVATE)
    val edit = sharedPreference.edit()

    edit.putString("songData", gson.toJson(song))
    edit.apply()
    ```

## SharedPreferences 데이터 불러오기
- get~ 메소드를 통해서 불러온다.
    - 첫 번째 매개변수는 key
    - 두 번째 매개변수는 인자가 없을 경우 반환할 값
- 사용
    ```kotlin
    val sharedPreference = getSharedPreferences("song", MODE_PRIVATE)
    val songJson = sharedPreference.getString("songData", null)
    ```

## SharedPreferences 데이터 삭제
- 데이터 삭제도 Editor를 활용해야 한다.
- 삭제 후 데이터를 변경했기 때문에 이를 저장을 해야한다.
1. 특정 데이터 삭제
    ```kotlin
    val test = getSharedPreferences("test", MODE_PRIVATE)
    val edit = test.edit()
    edit.remove("test")
    edit.apply()
    ```
2. 모든 데이터 삭제
    ```kotlin
    val test = getSharedPreferences("test", MODE_PRIVATE)
    val edit = test.edit()
    edit.clear()
    edit.apply()
    ```