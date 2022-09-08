---
layout: post
title: "ShoppingMall Clone Coding"
date: 2022-02-02
excerpt: "Android에 적응을 위한 쇼핑몰 클론코딩"
project: true
tags: [project]
comments: true
---

# FILOT SHOPPING MALL - Shopping mall Clone Coding
## 👀 Overview

| | |
|:---:|:---|
|한줄 소개|Android에 적응을 위한 쇼핑몰 클론코딩|
|진행기간|2022.02.02 ~ 2022.04.02|
|Skills|Glide, Retrofit2, Coroutine|
|팀구성|Android 1 / Spring 1|
| | |

<!-- - (선택) 관련활동 -->

## ❓ 개발 의도 및 개발 목표
### 기획 의도
1. <Android with Kotlin> 에 대해 학습한 애용을 종합적으로 검증하기 위하여
2. 사용자로써 익숙한 쇼핑몰을  개발자 관점으로써 필요기능을 인식하고 설계하여 구현하는 일련의 프로세스를 이해하고 적용해보자는 목적으로 기획 

### 개발 목표
해당 프로젝트는 아래의 내용을 증명하는 것을 목표로 함.

1. 웹 및 프로그래밍 언어에 대한 전반적인 이해 수준
2. 보편적인 개발 원칙에 따라 개발하는 능력
3. 목적에 따라 특정 기능을 구현하고 설계하는 응용 능력
4. 프로젝트 전반에 대한 책임을 지고 신기술에 적응하는 개발자의 태도

## 🍀 서비스 내용
간편하게 옷들을 확인하고 옷을 장바구니에 담아 옷을 주문하는 앱!

위 서비스에서 제공하는 핵심 기능은 2가지입니다.

1. 원하는 옷을 수량과 함께 장바구니에 담기
2. 주문한 앱에 한해서 별점과 리뷰를 메길 수 있음

## 🛠 기술 스택
- Glide
    - 비동기 이미지 처리를 간편하게 수행해주며, 이미지 캐싱처리를 도와 사용자의 부담을 줄일 수 있습니다.
- Retrofit2
    - 네트워크 통신을 더 간편하게 이용 가능
- Coroutine
    - 비동기 처리를 위해 사용

## 🖥 개발 내용
### 회원가입 및 로그인
#### 회원가입 메일 인증
메일이 중복인지 체크를하도록 중복 체크 기능을 구현했으며, 메일 인증을 구현했습니다.    
백엔드와 세션을 유지해 메일로 인증번호를 받고 유효성 검사를 수행합니다.    
유효성 검사가 정상적으로 통과가 된다면, 회원가입에 성공하게 되며 이 이메일을 통해 로그인을 할 수 있습니다.

#### 자동 로그인
한번 로그인이 성공되면, 해당 토큰은 디바이스에 저장을 해두고 만료될 때까지 이 토큰을 사용할 수 있습니다.    

#### 회원 정보 찾기
앞서 메일 인증을 구현했던 것을 토대로 사용자 이메일의 메일 인증을 통해 사용자 인증을 받고, 개인 정보 보호를 위해 비밀번호 업데이트를 수행합니다.

### 메인 화면
#### Main 화면과 DrawerLayout
서버에서 카테고리 종류를 받아와 TabLayout과 ViewPager를 이용해 각 카테고리별 상품 리스트들을 불러옵니다.    
이때, 사이드 메뉴의 카테고리도 같이 갱신을 해주며 이를 통해서도 접근이 가능하도록 구현했습니다.    

#### 상품 상세 페이지
상품을 클릭했을 때, 이 페이지로 넘어오며 상품에 대한 상세 정보를 확인할 수 있습니다.
색상과 사이즈를 선택하면 가격 등이 변경되고, 장바구니에 담을 수 있습니다.     
하단에 해당 상품에 대한 리뷰를 확인할 수 있으며, 자신의 것은 다른 색상으로 구분해 확인할 수 있습니다.     
또한 자신의 것은 이를 변경하거나 삭제할 수 있는 CRUD의 기능을 구현했습니다.

### 관리자 페이지
#### 배너 이미지 관리
새로운 배너로 등록하고 싶을 때, 갤러리에 이미지를 불러와 설정을 눌러주면 서버로 데이터를 보내 갱신할 수 있도록 합니다.

#### 회원 정보 관리
TableLayout을 활용해 사용자의 이메일, 이름, 등급을 확인할 수 있으며, 상세히 보기 위해 해당 아이템을 롱클릭을 수행합니다.    
사용자가 회원가입 시 등록했던 정보들을 불러오며 수정하여 저장할 수 있습니다.    

#### 상품 추가 
상품에 필요한 필수 정보들을 담고, 제공될 사이즈들을 체크박스로 저장합니다.    
썸네일에 들어갈 이미지를 저장하고, 이를 서버에 보내면 상품 상세 설명에 들어갈 이미지들을 추가적으로 받을 수 있도록 합니다.    
여기서 이미지들을 저장하고 삭제할 수 있습니다.

#### 상품 수정
TableLayout을 활용해 상품들을 관리할 수 있으며, 롱클릭 시 상세내용을 수정할 수 있습니다.
해당 화면은 상품 추가 화면을 재사용했으며, 이전 데이터들을 모두 불러와 화면에 뿌려줍니다.    
변경하기를 누르면, 상품 상세 이미지들을 수정할 수 있도록 구현을 했으며, 여기까지 진행하면 정상적으로 변경된 것을 확인할 수 있습니다.

<!-- ## 성장 경험 -->

## 📱 서비스 화면
<img width="1160" alt="MainPage" src="https://user-images.githubusercontent.com/72387349/188935260-8243d3b4-01a6-441a-bcd7-3502e2aeba39.png">
<img width="1126" alt="signup" src="https://user-images.githubusercontent.com/72387349/188935356-dcc37548-9f7f-44a2-b3c5-853e66eb66b6.png">
<img width="1126" alt="signupsuccess" src="https://user-images.githubusercontent.com/72387349/188935467-c57413d6-1dfa-452d-8b07-7054ef9c2bc3.png">
<img width="1114" alt="findUser" src="https://user-images.githubusercontent.com/72387349/188935591-098d09e9-60b9-4798-b9f7-303b14fcc7d3.png">
<img width="1080" alt="Main1" src="https://user-images.githubusercontent.com/72387349/188935656-2a9bffab-b13d-46a8-b6ae-3bda0f69cf94.png">
<img width="1146" alt="Main2" src="https://user-images.githubusercontent.com/72387349/188935710-cac45067-5a3d-4774-a24c-9d84a2c89ec0.png">
<img width="1100" alt="Main3" src="https://user-images.githubusercontent.com/72387349/188935773-37dd3291-df51-4f71-a04f-a0018854c4dd.png">
<img width="1173" alt="review" src="https://user-images.githubusercontent.com/72387349/188935901-28465d10-ba79-4cf9-829a-ea693633beb2.png">
<img width="1112" alt="admin" src="https://user-images.githubusercontent.com/72387349/188935934-674ea931-8788-4608-b33b-c20288400746.png">
<img width="1165" alt="productEdit" src="https://user-images.githubusercontent.com/72387349/188935986-d0070342-2b83-4b71-8cdb-b8c60a573053.png">
<img width="1171" alt="productAdd" src="https://user-images.githubusercontent.com/72387349/188936109-e931cf1b-57fb-46e6-b6d4-3f3ceb026e8d.png">
<img width="1169" alt="productAdd2" src="https://user-images.githubusercontent.com/72387349/188936141-3e6fe023-003d-4900-a233-92def6a69d77.png">
<img width="1173" alt="updateBanner" src="https://user-images.githubusercontent.com/72387349/188936212-a2b17803-9145-4fab-bf88-2d8992ac50cb.png">
<img width="1098" alt="updateCategory" src="https://user-images.githubusercontent.com/72387349/188936244-7f3b5113-bfd6-48a5-9a59-eefe63ae0581.png">

<!-- ## 추가 -->
<!-- ## 관련 링크 -->