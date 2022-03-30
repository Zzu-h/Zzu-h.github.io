---
layout: post
title: "Android: TabLayout"
date: 2022-03-30
excerpt: "Android TabLayout"
tags: [UMC, TabLayout, Android]
comments: true
categories : Android-기초
---
<img src="https://media.geeksforgeeks.org/wp-content/uploads/20210821164559/Tabs.gif"
    width="400dp"/>

# TabLayout
- 탭으로 구분하는 화며에서 탭 버튼을 배치하는 레이아웃

## 기본 Tab 추가하기
- 기본 탭은 TabLayout의 newTab() 메소드를 통해 객체를 받을 수 있다.
- 생성된 객체를 TabLayout에 추가하면서 탭을 생성할 수 있다.
    - Example
        ```kotlin
        val tab1: TabLayout.Tab = tabLayout.newTab()
        tab1.text=”Tab1”
        tabLayout.addTab(tab1)

        val tab2: TabLayout.Tab = tabLayout.newTab()
        tab2.text=”Tab2”
        tabLayout.addTab(tab2)
        ```
- 위처럼 동적으로 만들 필요가 없다면, xml파일에서 정의할 수 있다.
    - Example
        ```xml
        <com.google.android.material.tabs.TabLayout
            android:id=”@+id/tabs”
            android:layout_width=”match_parent”
            android:layout_height=”wrap_content”>
                <com.google.android.material.tabs.TabItem
                android:layout_width=”wrap_content”
                android:layout_height=”wrap_content”
                android:text=”Tab1” />

                <com.google.android.material.tabs.TabItem
                android:layout_width=”wrap_content”
                android:layout_height=”wrap_content”
                android:text=”Tab2” />
        </com.google.android.material.tabs.TabLayout>
        ```

## Tab Event 처리
- 탭 버튼을 선택할 때 출력해야하는 내용은 이벤트 핸들러에 명시를 해주면서 우리가 원하는 기능을 수행할 수 있다.
```kotlin
 binding.albumContentTb.addOnTabSelectedListener(object : TabLayout.OnTabSelectedListener{
     // 탭 버튼을 선택할 때 이벤트
    override fun onTabSelected(tab: TabLayout.Tab?) {
        when(tab?.text){
            "Tab1" -> TODO("Not yet implemented")
            "Tab2" -> TODO("Not yet implemented")
        }
    }
    // 선택된 탭 버튼을 다시 선택할 때 이벤트
    override fun onTabUnselected(tab: TabLayout.Tab?) { TODO("Not yet implemented") }
    // 다른 탭 버튼을 눌러 선택된 탭 버튼이 해제될 때 이벤트
    override fun onTabReselected(tab: TabLayout.Tab?) { TODO("Not yet implemented") }
})
```

## ViewPager 연동하기
앞서 [ViewPager](./2022-03-29-ViewPager.md)에서 TabLayoutMediator에서 정리를 했다.
