---
layout: post
title: "Android: ApplicationClass"
date: 2022-05-25
excerpt: "Android ApplicationClass"
tags: [UMC, ApplicationClass, Android]
comments: true
categories : Android-기초
---

# ApplicationClass
- 어플리케이션 컴포넌트들 사이에엇 공동으로 멤버들을 사용할 수 있게 해주는 공유 클래스
- 공통되게 사용하는 내용을 작성하면 어디서든 context를 이용한 접근이 가능하다.

## 사용

### manifest.xml
```xml
<application
    android:name=".ApplicationClass"
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:usesCleartextTraffic="true"
    android:theme="@style/Theme.Udemy_android_template">
...
</application>
```
### ApplicationClass.kt
```kotlin
package com.example.udemy_android_template

import android.app.Application
import android.content.Context
import android.content.SharedPreferences
import com.example.udemy_android_template.config.XAccessTokenInterceptor
import okhttp3.OkHttpClient
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import java.util.concurrent.TimeUnit

class ApplicationClass : Application() {
    companion object{
        const val X_ACCESS_TOKEN: String = "X-ACCESS-TOKEN"         // JWT Token Key
        const val TAG: String = "TEMPLATE-APP"                      // Log, SharedPreference
        const val APP_DATABASE = "$TAG-DB"

        const val DEV_URL: String = "";       // 테스트 서버 주소
        const val PROD_URL: String = ""    // 실서버 주소
        const val BASE_URL: String = DEV_URL

        lateinit var mSharedPreferences: SharedPreferences
        lateinit var retrofit: Retrofit
    }

    override fun onCreate() {
        super.onCreate()

        val client: OkHttpClient = OkHttpClient.Builder()
            .readTimeout(30000, TimeUnit.MILLISECONDS)
            .connectTimeout(30000, TimeUnit.MILLISECONDS)
            .addNetworkInterceptor(XAccessTokenInterceptor()) // JWT 자동 헤더 전송
            .build()

        retrofit = Retrofit.Builder()
            .baseUrl(BASE_URL)
            .client(client)
            .addConverterFactory(GsonConverterFactory.create())
            .build()

        mSharedPreferences = applicationContext.getSharedPreferences(TAG, Context.MODE_PRIVATE)
    }
}
```