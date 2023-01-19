---
title:  "사소하지만 소중한 행복: 행복저금통"
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

##  기간

2022/01/05 → 2022/02/22<br/>


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
  

<img src="/assets/images/20220317_happy_bottle_home.gif" width="200"/> <img src="/assets/images/20220317_happy_bottle_settings.jpeg" width="200"/>

- 받은 쪽지함(좌), 받은 쪽지 확인(중), 전송할 쪽지 쓰기(우) 화면
  

<img src="/assets/images/20220317_happy_bottle_send_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_check_send_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_write_send_msg.jpeg" width="200"/>

- 저금통(좌), 쪽지 리스트(중), 저장된 쪽지 확인 화면(우) 화면
  

<img src="/assets/images/20220317_happy_bottle_check_bottle.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_check_save_msg.jpeg" width="200"/> <img src="/assets/images/20220317_happy_bottle_confirm_save_msg.jpeg" width="200"/>


## 발생문제 및 해결방법

발생한 이슈와 대응했던 방법은 다음과 같습니다.<br/>

- 안드로이드 버전 12 업데이트 이후 발생된 푸시 알림를 클릭하면 앱 크러시 발생
  
  → 푸시 알림 클릭 시 액티비티 이동을 위해 PendingIntent를 사용할 때 Flag를 지정하지 않아서 발생한 이슈였습니다. 안드로이드 버전 12부터 PendingIntent 객체의 변경 가능 여부를 반드시 명시적으로 지정해야하기 때문에 Flag를 지정했습니다. 해당 앱의 Notification은 단순히 텍스트로 정보를 알리고 클릭 시 정보 화면으로 이동하는 것이 전부이기 때문에 PendingIntent.FLAG_IMMUTABLE를 지정하는 것으로 해결하였습니다. 
  
## 관련 링크

- [Google Play](https://play.google.com/store/apps/details?id=kr.co.yeeunlee.own.project1.mywriting&hl=ko)
- [Github](https://github.com/YeeunLee8245/HappyBottle-Android)
