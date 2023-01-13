---
title:  "행복저금통"
excerpt: "대학교 3학년 겨울 방학 기간에 진행한 개인 앱 프로젝트"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/20220317_happ_bottle_app.webp

categories:
  - Project
tags:
  - project
last_modified_at: 2022-03-17
---

## 소개

사소하지만 소중한 행복을 되새길 수 있도록 도와주는 행복저금통 서비스<br/>

## 참여 기간

2022/09/01 → 2022/12/31<br/>

## 주요 역할 및 담당

기획, 안드로이드 앱 개발<br/>

## 사용기술

- Android
  
- Kotlin
  

## 주요 개발 스킬

- Firebase for Database
  
- MVVM for Design Pattern
  
- Coroutine for Asynchronous
  
  
## 결과

- 로그인(좌), 회원가입(우) 화면
  

 <img src="/assets/images/20220317_happy_bottle_login.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_sign_in.jpeg" width="200"/>

- 홈(좌), 설정(우) 화면
  

<img src="/assets/images/20220317_happy_bottle_home.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_settings.jpeg" width="200"/>

- 받은 쪽지함(좌), 받은 쪽지 확인(중), 전송할 쪽지 쓰기(우) 화면
  

<img src="/assets/images/20220317_happy_bottle_send_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_check_send_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_write_send_msg.jpeg" width="200"/>

- 저금통(좌), 쪽지 리스트(중), 저장된 쪽지 확인 화면(우) 화면
  

<img src="/assets/images/20220317_happy_bottle_check_bottle.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_check_save_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_confirm_save_msg.jpeg" width="200"/>


## 발생문제 및 해결방법

발생한 이슈와 대응했던 방법은 다음과 같습니다.<br/>

- Bottom Navigation 탭을 통해 화면 이동 시, 과거에 로딩 되었던 Paging3의 PagingSource 데이터가 유지되지않고 재로딩 되는 이슈 발생
  
  → 화면의 ViewModel 클래스에서 LiveData를 통해 화면으로 보여줄 PagingSource의 Flow를 저장합니다. 그 후, 화면이 다시 보여질 때 observe를 통해 기존의 LiveData의 데이터를 사용하는 것으로 해결하였습니다.
 
