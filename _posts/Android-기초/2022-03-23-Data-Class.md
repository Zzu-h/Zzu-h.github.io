---
layout: post
title: "Android: Data Class"
date: 2022-03-23
excerpt: "Data Class"
comments: true
categories : Android-기초
---

# Data Class
- toString(), hashCode(), equals(), copy()메소드를 자동으로 만들어주는 클래스
- [참고](https://kotlinlang.org/docs/data-classes.html)
## 특징
- 상속 받을 수 없음
- val 또는 var으로 선언해야 함
- abstract, open, sealed, inner를 붙일 수 없음
- 1개 이상의 프로퍼티를 가지고 있어야 함

## 예제
- data class
    ```kotlin
    data class Person(val name: String) {
        var age: Int = 0
    }
    ```
- 사용
    ```kotlin
    val person1 = Person("John")
    val person2 = Person("John")
    person1.age = 10
    person2.age = 20
    ```
- copy method
    ```kotlin
    val jack = User(name = "Jack", age = 1)
    val olderJack = jack.copy(age = 2)
    ```
- class 구조 해제
    ```kotlin
    val jane = User("Jane", 35)
    val (name, age) = jane
    ```