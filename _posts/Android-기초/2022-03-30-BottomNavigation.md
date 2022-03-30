---
layout: post
title: "Android: BottomNavigation"
date: 2022-03-30
excerpt: "Android BottomNavigation"
tags: [UMC, BottomNavigation, Android]
comments: true
categories : Android-기초
---

![BottomNavigation](https://media.geeksforgeeks.org/wp-content/uploads/20200908000701/Screenshot10-300x240.png)

# BottomNavigation
- 위와 같이 하단 Navigation으로 activity나 fragment를 전환하기 위한 View로 사용된다.

## 왜 Bottom Navigation Bar가 필요한가?
- 사용자가 서로 다른 activity나 fragment들을 손쉽게 전환할 수 있다.
- 사용자가 앱에서 다른 화면들을 인식할 수 있게 한다.
- 사용자가 현재 어느 화면에 있는지 확인할 수 있다.

## Material 권장 사항
- 아래의 상황에서는 사용하는 것을 권하지 않음
    - 단일 Task
    - 사용자 기본 설정 또는 Settings

- 아래의 상황에서 사용하는 것을 권함
    - 앱 내에서 접근 가능한 Top-level View
    - 3~5개의 Menu
    - 모바일 또는 태블릿 app

## BottomNavigationView 사용
- BottomNavigationView는 크게 2가지 View 정의가 필요
    - BottomNavigationView에서 사용할 menu.xml을 정의
    - BottomNavigationView을 사용할 xml에 layout resource에 BottomNavigationView을 정의

### menu.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:tools="http://schemas.android.com/tools"
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:id="@+id/homeFragment"
        android:icon="@drawable/ic_bottom_home_no_select"
        app:showAsAction="always"
        android:enabled="true"
        android:title="홈"
        tools:ignore="AlwaysShowAction" />
    <item
        android:id="@+id/lookFragment"
        android:icon="@drawable/ic_bottom_look_no_select"
        app:showAsAction="always"
        android:title="둘러보기" />
    <item
        android:id="@+id/searchFragment"
        android:icon="@drawable/ic_bottom_search_no_select"
        app:showAsAction="always"
        android:title="검색" />
</menu>
```

### layout.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <FrameLayout
        android:id="@+id/main_frm"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintBottom_toTopOf="@id/main_bnv"
        app:layout_constraintTop_toTopOf="parent" />
    <com.google.android.material.bottomnavigation.BottomNavigationView
        android:id="@+id/main_bnv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:itemIconSize="20dp"
        app:itemIconTint="@drawable/btm_color_selector"
        app:itemTextColor="@drawable/btm_color_selector"
        app:labelVisibilityMode="labeled"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:menu="@menu/menu"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```

### Click Listener 설정
```kotlin
bottomNavigationView.setOnNavigationItemSelectedListener{ item ->
    when (item.itemId) {
        R.id.homeFragment -> {
            supportFragmentManager.beginTransaction()
                .replace(R.id.main_frm, HomeFragment())
                .commitAllowingStateLoss()
            return@setOnItemSelectedListener true
        }
        R.id.lookFragment -> {
            supportFragmentManager.beginTransaction()
                .replace(R.id.main_frm, LookFragment())
                .commitAllowingStateLoss()
            return@setOnItemSelectedListener true
        }
        R.id.searchFragment -> {
            supportFragmentManager.beginTransaction()
                .replace(R.id.main_frm, SearchFragment())
                .commitAllowingStateLoss()
            return@setOnItemSelectedListener true
        }
    }
    false
}
```

- 설정할 때 반환 값은 boolean 형식이다.
