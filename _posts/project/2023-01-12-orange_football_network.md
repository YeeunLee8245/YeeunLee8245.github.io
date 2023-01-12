---
title:  "Orange Football Network: OFN"
excerpt: "project, 인턴"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/20230112_ofn_app.png

categories:
  - Project
tags:
  - project
last_modified_at: 2023-01-12
---

## 소개

유럽 축구 전문가들의 노하우를 전 세계로 전달하는 서비스<br/>

## 참여 기간

2022/09/01 → 2022/12/31<br/>

## 주요 역할 및 담당

안드로이드 앱 개발<br/>

## 사용기술

- Android
  
- Kotlin
  

## 주요 개발 스킬

- Retrofit/OkHttp for networking
  
- MVVM for Design Pattern
  
- Coroutines/Flow for asynchronous
  
- Glide for Loading images
  
- Jetpack Navigation, Paging3
  

## 결과

> 제가 구현한 부분의 이미지 또는 영상을 올리는 것에 대해 사전 동의를 얻었음을 밝힙니다.

- 전화번호 인증
  

 <img src="/assets/images/20230112_ofn_phone_number_ready.jpeg" width="200"/> <img src="/assets/images/20230112_ofn_phone_number_send.png" width="200"/>

- 프로필 설정
  

<img src="/assets/images/20230112_ofn_edit_profile.png" width="200"/>

- 검색
  

<img src="/assets/images/20230112_ofn_search.gif" width="200"/>

- 알림 화면
  

<img src="/assets/images/20230112_ofn_notification.gif" width="200"/>
- 강사(좌), 프로그램(우) 화면
  

 <img src="/assets/images/20230112_ofn_instructor.gif" width="200"/> <img src="/assets/images/20230112_ofn_program.gif" width="200"/>

- 로그인(좌), 홈(우) 화면
  

 <img src="/assets/images/20230112_ofn_login.gif" width="200"/> <img src="/assets/images/20230112_ofn_home.gif" width="200"/>

## 발생문제 및 해결방법

각각의 이슈와 대응했던 방법은 다음과 같습니다.<br/>

1. Bottom Navigation 탭을 통해 화면 이동 시, 과거에 로딩 되었던 화면의 데이터가 유지되지않고 재로딩 되는 이슈 발생
  
  → 화면의 ViewModel 클래스에 LiveData를 통해 보여줄 데이터를 저장하도록 하고, 화면이 다시 보여질 때 observe를 통해 LiveData의 데이터를 사용자에게 보여지게 함
  
  
2. 한 화면에서 무한 스크롤이 가능한 각각의 RecyclerView 스크롤을 통일하기 위해 NestedScrollView 사용시 메모리 이슈 발생
  
  → 스크롤이 RecyclerView에 도달하였을 때 View를 생성하기 위해 ConcatAdapter를 사용하여 표시할 RecyclerView를 결합하고 RecyclerView에서 무한 스크롤이 가능할 수 있도록 Jetpack Paging3를 사용하여 구현
