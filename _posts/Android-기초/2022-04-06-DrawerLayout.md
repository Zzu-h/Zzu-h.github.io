---
layout: post
title: "Android: DrawerLayout"
date: 2022-04-06
excerpt: "Android DrawerLayout"
tags: [UMC, DrawerLayout, Android]
comments: true
categories : Android-기초
---

![img](https://developer.android.com/images/topic/libraries/architecture/navigation-drawer.png)

# DrawerLayout
- 액티비티 화면에 보이지 않던 내용이 왼쪽이나 오른쪽에서 나오는 기능
- Layout xml파일에서 Drawer 메뉴가 출력되어야 하는 부분의 태그를 DrawerLayout으로 선언한다.
- 이는 꼭 루트 태그일 필요는 없으나, 기본적으로 기본 화면 위에 표시하므로
    - 대부분 루트태그로 작성하게 된다.
- 이를 사용하면 뷰 2개를 생성해야 한다.
    - 첫번째 뷰는 기본으로 보이는 화면
    - 두번째 뷰는 옆에 끌려나오는 화면

## Drawer 구성
```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Use DrawerLayout as root container for activity -->
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">

    <!-- Layout to contain contents of main body of screen (drawer will slide over this) -->
    <androidx.fragment.app.FragmentContainerView
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:id="@+id/nav_host_fragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />

    <!-- Container for contents of drawer - use NavigationView to make configuration easier -->
    <com.google.android.material.navigation.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true" />

</androidx.drawerlayout.widget.DrawerLayout>
```

## 토글 버튼
- 기본적으로 Layout xml만으로 Drawer Layout을 이용할 수 있다.
    - 또한, 토글 버튼을 통해 레이아웃을 표시할 수 있다.
- 이 토글 버튼은 ActionBarDrawerToogle 클래스에서 제공한다.
    ```kotlin
    lateinit var toggle: ActionBarDrawerToggle

    override fun onCreate(savedInstanceState: Bundle?) {
        (... 생략 ...)
        // ActionBarDrawerToggle 버튼 적용
        toggle = ActionBarDrawerToggle(this, binding.drawer, R.string.drawer_opened, R.string.drawer_closed)
        supportActionBar?.setDisplayHomeAsUpEnabled(true)
        toggle.syncState()
    }
    ```
- `ActionBarDrawerToggle(this, binding.drawer, R.string.drawer_opened, R.string.drawer_closed)`
    - 각 매개변수는 다음을 의미한다.
        - binding.drawer: 토글 버튼으로 여닫는 드로어 객체
        - R.string.drawer_opened: 드로어가 열렸을때의 상태를 표현한 문자열
        - R.string.drawer_closed: 드로어가 닫혔을때의 상태를 표현한 문자열
- `supportActionBar?.setDisplayHomeAsUpEnabled(true)`
    - 토글 버튼으로 사용할 아이콘이 출력되게 함
- `toggle.syncState()`
    - Drawer를 열기 위해 제공하는 내비게이션 아이콘을 출력한다.
- 토글 이벤트 동작 메소드: 이를 같이 적용해주어야 토글 버튼이 정상적으로 작동한다.
    ```kotlin
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        // 이벤트가 토글 버튼에서 발생하면
        if (toggle.onOptionsItemSelected(item)) {
        return true
        }
        return super.onOptionsItemSelected(item)
    }
    ```