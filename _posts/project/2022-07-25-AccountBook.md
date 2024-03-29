---
layout: post
title: "Account Book"
date: 2022-07-25
excerpt: "자신의 지출, 수입 내역을 보고 관리할 수 있으며 그래프를 통해 시각적으로 한 눈에 확인이 가능한 가계부 앱"
project: true
tags: [project]
comments: true
---

# Account Book
## 👀 Overview

| | |
|:---:|:---|
|한줄 소개|자신의 지출, 수입 내역을 보고 관리할 수 있으며 그래프를 통해 시각적으로 한 눈에 확인이 가능한 가계부 앱|
|진행기간|2022.07.25 ~ 2022.08.05|
|Skills|Jetpack Compose, Hilt, SQLite, Coroutine, LiveData|
|팀구성|Android 1|
| | |

## ❓ 개발 목표
해당 프로젝트는 아래의 내용을 증명하는 것을 목표로 함.

1. Compose의 사용법
2. Android View보다 Compose가 더 좋은 점
3. 혼자 하더라도 컨벤션 등을 지키는 프로젝트

## 🍀 서비스 내용
사용자는 개인적으로 자신의 지출, 수입 내역을 볼 수 있으며, 관리할 수 있습니다.
지출, 수입에 대한 내역을 볼 수 있으며, 시각적(원 그래프와 꺽은 선 그래프)로 한 눈에 확인이 가능합니다.

## 🛠 기술 스택
### Jetpack Compose
- 이번 프로젝트의 주요 기술

### Hilt
- Koin vs Hilt
    - 이전 프로젝트에서 Koin을 사용했고, 이번 프로젝트에선 Hilt를 사용해 이 둘을 직접 비교해보고자 함

### SQLite
- 수입 및 지출 내역을 Database에 저장 및 수정, 조회를 수행하기 위해 사용

### Coroutine
- 비동기 처리를 위해 사용

### LiveData
- 데이터의 변경 사항을 뷰에 즉각적인 변화를 주기 위해 사용

## 🖥 개발 내용
아래 화면들은 전체 Compose로 진행했습니다.

### 수입 및 지출 화면
- lazy column을 활용해 Database에 저장되어 있는 이력들을 불러와 화면에 리스트 형태로 보여주도록 개발했습니다.    
- 필요한 뷰들은 공통 컴포넌트로 분리해 뷰를 재사용할 수 있도록 했으며, 각 아이템은 롱클릭 리스너를 추가해 이들을 삭제할 수 있는 화면으로 넘어가도록 했습니다.    
- 삭제화면에서는 현재 선택된 아이템일 경우 체스표시와 백그라운드 색상이 변경되도록 현재 상태를 지속적으로 관찰하고 있으며, 선택된 것이 없을 때 삭제화면을 벗어나도록 했습니다.

### 수입 및 지출 수정/생성 화면
- history객체가 들어오면 수정, 아니면 생성화면으로 필수 정보 입력 시 버튼이 활성화 되도록, 그렇지 않을 경우 비활성화 되도록 구현되어 있습니다.
- 또한 카테고리를 추가하고 싶다면 카테고리 추가 화면으로 이동할 수 있으며 중복 프래그먼트가 생기지 않도록 관리를 했습니다.
- Custom Popup View

### 월별 요약 화면
- LazyVerticalGrid를 통해 캘린더를 직접 구현을 했습니다.
- 각 필요한 날짜들은 계산식을 통해 구현을 했으며, 오늘 날짜일 경우 다른 색상을 부여하여 오늘 날짜를 확인할 수 있습니다.

### 그래프 화면
- PieChart 라이브러리를 통해 Compose 그래프를 그렸으며, 각 통계를 계산해 그릴 수 있었습니다.

### 카테고리 관리 화면
- 다른 화면과 마찬가지로 LazyColumn을 통해 Database에서 카테고리 리스트를 불러와 화면에 적용했습니다.
- 새로 카테고리를 추가했을 경우 또는 변경했을 경우 화면에 적용하기 위해서 관리 화면이 onStart가 될 때 화면을 한번 리로드 하게 됩니다.
- 이는 flow를 이용했다면 더 효율적인 개발이 될 수 있지 않았을까 하는 아쉬움이 있습니다.

### Local API
현재는 Local Database를 이용해 데이터를 저장하고 있습니다.
- 하지만 서비스 확장으로 네트워크를 요구할 수 있기 때문에 API를 통해 접근한다는 추상적인 레이어를 하나 더 추가하게 된다면, 이후 프로젝트 마이그레이션을 수행할 때 더 효율적으로 작업할 수 있을 것입니다.

## 📈 성장 경험
### Jetpack Compose
- 처음으로 Compose를 학습하고 적용해보았습니다.
- 어떻게 사용하는지, 그리고 새로운 개념을 학습하는데 어떻게 수행하는지를 배울 수 있었습니다.
    - 안드로이드 샘플 코드는 생각보다 친절합니다.
        - 이를 통해 어떻게 사용하는지, 어떤 네이밍 컨벤션을 지키는지를 확인할 수 있었습니다.
    - 코드랩을 따라 기본 사용법을 익힐 수 있었습니다.
        - 기본적인 화면을 구현하는데 있어 어떤 기술들이 있었고, 어떻게 사용하는지를 익힐 수 있었습니다.
- 아직은 Android View가 편해 이를 사용하기에는 더 많은 시간이 소요되고 있습니다.
- 하지만, 뷰의 재사용성이나 list를 그리는데 있어는 Android View보다 더 좋은 점은 확실히 느낄 수 있었습니다.
- 어떤 프로젝트냐에 따라 이를 선택하는 것도 중요한 요소임을 깨달았습니다.

### 협업의 필요성
- 이전 프로젝트인 Repository 개발과 비교했을 때 시간이 부족함을 느꼈습니다.
    - 이전 개발에서는 각자의 역할을 나누고 각 화면에 쏟는 시간에 더 많기에 저희가 바라는 화면을 더 잘 그릴 수 있었습니다.
    - 하지만 이번 프로젝트에서는 혼자서 모든 화면을 구현해야 했기에 모든 시간이 분산이 되어 제가 바라는 완벽한 화면을 그릴 수가 없었습니다.
    - 왜 협업이 있어야 하며, 혼자서 개발하는 것은 한계가 있다는 것을 알 수 있었습니다.
- 규칙성이 깨집니다.
    - 같이 하게 되면 서로의 코드리뷰를 통해서 저가 짰던 코드에서 네이밍 컨벤션을 준수하지 못했거나 또는 분리될 수 있는 코드들을 깨닫고 수정할 수 있었습니다.
    - 하지만, 혼자서 하기 때문에 어디서 깨졌는지 또는 더 분리될 수 있는지를 놓치기 너무나도 쉬웠습니다.
    - 같이 한다는 것은 존재하는 규칙을 더 완벽히 지킬 수 있다는 것을 알 수 있었습니다.

### Hilt와 Koin의 차이
- 실제 사용해보고 겪은 차이는 다음과 같습니다.
- Koin
    - 사용하기가 매우 쉽다.
    - 어디서 에러가 나는지 찾기가 매우 힘들다.
- Hilt
    - 사용하기가 (귀찮)어렵다.
    - 어디가 문제인지 코인보다 쉽게 알 수 있다.
- 각각의 장단점은 있으나, 프로젝트 규모가 커질수록 에러 찾기는 분명 더 어려워질 것입니다.
    - 따라서, Hilt가 더 좋은 선택이 될 수 있음을 깨달았습니다..

## 추가
이번 프로젝트에서 혼자하니까 여러 규칙들을 어겼던게 마음이 많이 걸렸다.    
시간에 쫓기고 또 코드의 피드백을 그 누가 해주지 않기에 어긋난 방향으로 가더라도 잡아줄 사람이 없었다.    
물론 습관이 잡힌다면 크게 그렇지 않겠지만.. 아직 나는 주니어 개발자임을 다시 한번 깨닫게 해준 프로젝트였다.