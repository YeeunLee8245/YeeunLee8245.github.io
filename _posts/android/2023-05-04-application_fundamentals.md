---
title: "작성중)[안드로이드 공식 문서 읽기] 안드로이드 기본(android fundamentals)"
excerpt: "apk, abb, activity"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/android_title.jpeg

categories:
  - Android
tags:
  - android
last_modified_at: 2023-05-04
---

## 목차

1. 앱 컴파일
2. 안드로이드 보안 특징
3. 앱 간 데이터 공유와 앱에서 System Service 접근
4. 앱 컴포넌트
5. Activity<br>

## 1. 앱 컴파일
* Android SDK
	* APK, App Bundle로 코드를 컴파일함
* .apk 파일
	* 안드로이드 패키지
	* 런타임에 필요한 안드로이드 앱 구성요소 포함
	* 디바이스에 안드로이드 앱 설치 가능

* .aab 파일
	* 안드로이드 앱 번들
	* 런타임에 필요없는 메타데이터와 런타임에 필요한 안드로이드 앱 프로젝트 구성요소 포함
	* 퍼블리시(게시) 형식이며 디바이스에 설치 불가능
	* apk 생성과 서명을 뒤로 미룬 것(서명은 Google Play가 대리로 서명해줌)
	* aab는 apk를 완성해주는 요소를 담은 패키지(디바이스 별로 apk를 만들 수 있게 해줌)
	* apk 형태로 배포하는 것보다 앱 크기가 평균 15% 줄어듦

* Google Play에 앱을 배포하면 Google Play 서버는 앱이 설치되는 특정 디바이스에 필요한 resource와 코드만 포함하여 APK를 최적화함

* Google Play는 aab로 앱을 배포하는 것을 강제함
	* 서명을 Google Play가 하기 때문에 구글 입장에선 인앱 결제 수수료를 떼가는 것을 통제하기 좋다고,,

## 2. 안드로이드 보안 특징
* Android OS는 각 앱이 곧 다른 유저인 다중 사용자 Linux System
* VM 위에서 수행되며 앱 코드는 다른 앱과 독립적으로 수행됨
* 모든 앱은 Linux Process에서 디폴트로 수행되며, 안드로이드 시스템은 앱의 구성요소가 실행이 필요할 때 시작되고 더 이상 시스템이 필요하지 않거나 **다른 앱을 위해 메모리를 복구 해야할 때 프로세스를 종료함**
* 이 시스템은 각 앱에 고유한 Linux User ID를 할당함
	* ID는 시스템 내에서만 사용되며 앱에서는 알 수 없음
	* 앱에 할당된 해당 ID만 시스템에 액세스할 수 있도록 시스템은 앱의 모든 파일에 대한 권한을 설정함
	* 각 앱은 기본적으로 작업을 수행하는데 필요한 구성요소에만 액세스할 수 있음

## 3. 앱 간 데이터 공유와 앱에서 System Service 접근
* 동일한 Linux User ID 간 공유
	* 파일끼리의 접근을 통해 가능
	* 시스템 리소스 절약을 위해 동일한 ID를 가진앱은 동일한 Linux 프로세스에서 실행되고 동일한 VM을 공유 가능(두 앱은 동일한 인증서로 서명되어야 함)
* 디바이스 권한 요청(ex. 카메라, 위치, 블루투스 연결)
	* 권한을 명시적으로 부여해야함
	* [Permissions on Android](https://developer.android.com/training/permissions)

## 4. 앱 컴포넌트
* 각 컴포넌트는 시스템과 유저가 앱에 접속할 수 있는 entry point(진입점)
* 종류
	* Activity
	* Service
	* Broadcast receiver
	* Content provider
* 각 컴포넌트는 구별되는 목적과 lifecycle을 가짐

## 5. Activity
* 유저와 상호작용하는 entry point
* UI와 함께 single screen을 표시함
* 용이성
	* 시스템이 activity를 호스팅하는 프로세스를 유지할 수 있도록 사용자 화면에 있는 것을 추적함
	* 이전에 사용했던 프로세스에 사용자가 되돌아올 수 있는 activity(parse 상태) 활동이 포함되어 있는지 파악하여 사용 가능 상태를 유지함
		* 돌아갈 프로세스가 존재하면 해당 프로세스의 우선순위를 더 높게 지정
	* 유저가 이전 상태가 복원된 activity로 돌아갈 수 있도록 앱이 activity 프로세스를 종료처리하는 것을 도움
	* 앱이 서로 간의 user flow를 구현하고 시스템이 user flow를 조정하는 방법을 제공함
		* ex. 공유
	* 다른 activity를 생성할 수 있음
* 한 앱이 다른 앱을 호출하는 것은 실질적으로 다른 앱의 activity를 호출하는 것과 같음
* 앱은 다중 activity를 포함할 수 있으며 앱의 시작 화면은 특정 activity에 종속되는 것이 아님
	* 앱이 시작할 때 시작 환경에 따라 다른 activity로 시작할 수 있음(단, activity 간 의존은 최소한으로)
* 작은 창을 띄우는 화면, 위나 아래에 고정된 화면 모두 activity로 구현 가능
	* 하지만 보통 activity는 window 전체를 덮는 화면으로 사용됨
### 5-1. Manifest 등록
* 앱 매니페스트에 해당 액티비티 정보를 등록해야 액티비티 lifecycle를 관리할 수 있음
* activity 선언
```xml
<manifest ... > 
	<application ... >     
		<activity android:name=".ExampleActivity" android:icon="@drawable/app_icon"> 
			<intent-filter>
				 <action android:name="android.intent.action.SEND" />
				 <category android:name="android.intent.category.DEFAULT" />
				 <data android:mimeType="text/plain" />
		     </intent-filter>
		</activity> 
		... 
	</application ... > 
	...
</manifest >
```
	* android:name은 activity 클래스 이름을 지정함
	* intent-filter: 유저가 작업을 수행하는 데 사용할 앱을 묻는 경우 작동됨
		* 명시적(explicit)
			* ex. Gmail(앱을 특정 함) activity를 통해 이메일 전송
		* 암시적(implicit)
			* ex. 메일을 전송할 수 있는 어떤 activity든 실행하여 이메일 전송
	* <action>은 android:name="android.intent.action.SEND"로 정의되어 activity가 데이터를 전송하도록 지정함 
	* <data>는 activity가 전송할 수 있는 데이터 타입을 지정함
		* android:mimeType="text/plain"로 정의되었기 때문에 text/plain 타입으로 전송
	* <category>는 intent가 처리해야하는 구성요소 종류에 대한 추가 정보를 포함하는 문자열
		* 암시적 인텐트를 받을 때 앱의 intent-filter와 category가 동일하지 않으면 암시적 인텐트가 통과 안됨(= 정의된 카테고리가 다른 암시적 인텐트는 받을 수 없음)
		* 대부분의 intent에는 카테고리가 필요하지 않음 => 카테고리를 정의하지 않으면 안드로이드에서 자동으로 android.intent.category.DEFAULT를 추가해줌
		* 

### 5-2. 생명주기
* onCreate()
	* activity 생성 시, 첫번째로 실행됨
	* activity의 필수 구성요소를 초기화해야함
		* view와 data를 바인딩 해야함
	* setContentView()를 통해 UI에 대한 레이아웃을 호출해야함
* onStart()
	* onCreate()가 종료된 후 activity가 시작된 상태
	* activity가 사용자에게 표시됨
	* 사용자와 상호작용하기 위한 최종 구현 코드를 작성해야함
* onResume()
	* activity가 사용자와 상호작용하기 직전(포그라운드로 올라왔을 때)에 시스템이 호출함
	* 모든 사용자 입력을 캡처함
	* 이 시점에서 activity는 activity stack의 최상단에 있음
	* 앱의 핵심 기능을 작성해야함
	* onPause()는 항상 onResume()를 호출함(onPause()의 다음 작업으로 Resume()를 호출할 수 있음)
* onPause()
	* activity가 포커스를 잃으면 시스템이 호출
		* ex. 유저가 뒤로가기 버튼 누를 때
	* 이후에 activity가 중지되거나 재개(onResume) 됨
	* 유저가 UI 업데이트를 예상할 것 같은 경우 UI를 계속 업데이트 할 수 있음
		* 하지만 유저 데이터 네트워크 호출, DB 트랜잭션을 실행하는데 사용하면 안됨
	* 이후 onStop()과 onResume() 둘중 하나가 호출됨
	* 주의점
		* **onPause()는 포커스만 뺐겼을 뿐 아직 포그라운드 상태**
		* activity가 무엇인가에 가려질 때 처음 호출되는 콜백함수이기 때문에 이 activity가 유저에게 포커스를 잃기 전 꼭해야할 것을 미리 저장(ex. onSaveInstanceState()에 저장)해놓거나 처리해놓는게 좋음
* onStop()
	* activity가 유저에게 아예 보여지지 않을 때 호출
		* 새로운 activity 실행 또는 해당 activity 종료 직전
	* 백그라운드 상태가 됨
	* 백그라운드에서 포그라운드로 activity를 올리는 상황이라면 onRestart()를 호출함
	* activity가 완전히 종료되는 경우 onDestory() 호출
* onRestart()
	* onStop() 상태의 activity가 다시 시작될 때 호출
	* onStop()된 시간부터 상태를 복원함
	* 이 콜백 다음은 항상 onStart() 호출
* onDestroy()
	* activity가 완전히 소멸되기 직전에 최종적으로 호출
	* activity를 포함하는 프로세스가 소멸될 때 activity의 모든 resource가 해제되도록 보장하기 위해 구현해야함
