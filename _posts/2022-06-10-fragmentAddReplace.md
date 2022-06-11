---
title: "[Android]Fragment의 add()와 replace() 차이점"
excerpt: "fragment, add, replace"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/20220610_fragmentAddReplace.png

categories:
  - Android
tags:
  - android
last_modified_at: 2022-06-10
---

## 목차

1. add? replace?
2. 활용
3. 주의할 점
4. 마무리<br>

## 1. add()? replace()?

add()와 replace()는 Fragment를 교체하는 방법이다. <br>

FragmentManager의 beginTransaction()을 통해 호출될 때 두 메서드의 프래그먼트 생명주기는 **onAttach ▶️ onCreate ▶️ onCreateView ▶️ onStart ▶️ onResume**으로 같지만, 교체되는 시점에서 차이가 발생한다.<br>

* add()

기존에 보여지고 있는 프래그먼트를 **제거하지 않고** 그 위에 다른 프래그먼트를 **추가**하는 것이다.<br>

따라서 새로운 프래그먼트가 추가되었다고 한들 기존 프래그먼트의 생명주기에는 영향을 주지 않는다.<br>

```mbox
<1. 프래그먼트 A add()>
Fragment A: onAttach() ▶️ onCreate() ▶️ onCreateView() ▶️ onStart() ▶️ onResume()
  
<2. 프래그먼트 B add()>
Fragment A: (유지)
Fragment B: onAttach() ▶️ onCreate() ▶️ onCreateView() ▶️ onStart() ▶️ onResume()
```

<br>

* replace()

기존에 보여지고 있는 프래그먼트를 **제거하고** 그 자리를 다른 프래그먼트로 **교체**하는 것이다.<br> 

따라서 add()로 추가되었던 프래그먼트가 여러개일지라도 여태까지 추가된 프래그먼트들은 전부 제거하고 replace()를 통해 새 프래그먼트로 교체된다. <br>

```mbox
<1. 프래그먼트 A add()>
Fragment A: onAttach() ▶️ onCreate() ▶️ onCreateView() ▶️ onStart() ▶️ onResume()
  
<2. 프래그먼트 B replace()>
Fragment A: onPause() ▶️ onStop() ▶️ onDestroyView( ▶️ onDestroy()  ▶️ onDetach()
Fragment B: onAttach() ▶️ onCreate() ▶️ onCreateView() ▶️ onStart() ▶️ onResume()
```

<br>

* +추가) hide(), show()

추가된(add()) 프래그먼트는 hide()를 호출해 숨기거나 show()를 호출해 다시 보여줄 수 있다. <br>

둘다 데이터는 유지시킨 채 가시성(visibility)을 설정하는 것이라 생각하면 되겠다.<br>

<br>

## 2. 활용

* add()를 이용한 Fragment 재사용

```kotlin
fun chageAFragment(){
  val tran = supportFragmentManager.beginTransaction()
  if (fragmentA == null){
    	fragmentA = FragmentA()
    	tran.add(R.id.fragment, fragmentA).commit()
  }
  
  if (fragmentA != null) // fragmentA를 add()했었다면 show()
  	tran.show(fragmentA).commit()
  if (fragmentB != null)
  	tran.hide(fragmentB).commit()
}

fun chageBFragment(){
  val tran = supportFragmentManager.beginTransaction()
  if (fragmentB == null){
    	fragmentB = FragmentB()
    	tran.add(R.id.fragment, fragmentB).commit()
  }
  
  if (fragmentA != null) 
  	tran.hide(fragmentA).commit()
  if (fragmentB != null) // fragmentB를 add()했었다면 show()
  	tran.show(fragmentB).commit()
}
```

fragmentA와 fragmentB를 데이터가 유지된 채로 추가하는 코드다. <br>

<br>

* replace를 이용한 새 Fragment 생성

```kotlin
fun chageAFragment(){
  val tran = supportFragmentManager.beginTransaction()
  tran.replace(R.id.fragment, fragmentA).commit()
}
```

새로 화면에 보여질 때마다 데이터를 초기화시켜야할 때 replace()를 사용할 수 있겠다.<br>

<br>

## 3. 주의할 점

* 같은 container id상에서 같은 fragment 객체를 add()로 중복 호출하면 에러가 발생한다.

add()는 기존 프래그먼트 위에 똑같은 프래그먼트를 올리는 것이기 때문에 발생하는 에러다. 해결하려면 remove() 호출을 통해 기존 프래그먼트를 지우고 호출하면 된다(이렇게 할 바엔 처음부터 replace()를 호출하는 것을 추천).<br>

또는 hide와 show를 사용하여 기존 프래그먼트와 다른 프래그먼트를 올릴 때는 hide()를 호출하고 다시 기존 프래그먼트를 올리고싶을 때는 show() 호출하면 된다. 위의 방법과 달리 데이터를 유지할 수 있다.<br>

<br>

* Fragment transaction의 commit()은 **비동기(asynchronous)**이다.

이 사실을 간과한 채 프래그먼트 교체 코드를 짰다가 테스트할 때 프래그먼트가 뒤죽박죽 생성되었던 경험이 있다.<br>

프래그먼트를 commit()할 조건(ex. A 프래그먼트가 아직 화면에 보이고 있는지 분기문으로 체크)을 설정하는 것을 추천한다.<br>

<br>

* ViewModel를 통해 LiveData를 가지고 있는 fragment를 같은 객체(fragment)인 상태에서 replace하지 않기

문장만 보면 무슨 말인지 이해할 수 없을 것 같다. 코드를 보자.<br>

```kotlin
// Fragment A's ViewModel
class FragmentA: ViewModel() {
    private var _userLocation = MutableLiveData<String>()
    val userLocation: LiveData<String> = _userLocation
}
```

```kotlin
// Fragment A
...
fragmentAViewModel.userLoacation.observe(viewLifeCycleOwner){
  ...	
}
...
```

```kotlin
// Activity
...
fun chageAFragment(){
  val tran = supportFragmentManager.beginTransaction()
  tran.replace(R.id.fragment, fragmentA).commit()
}
...
```

observe는 LiveData가 가지고 있는 value가 바뀔 때 사용자가 정의한 함수를 호출해준다.<br>

하지만 기존 fragment에서 다른 fragment로 add()/replace() 되었다가 다시 replace() 된다면  기존 fragment와 연결되어 있던 ViewModel의 변수 역시 초기화 된다. 그러므로 LiveData의 값 변동(초기화)이 생긴 것으로 간주한 observe는 replace()와 동시에 의도치 않은 함수를 호출할 수 있다.<br>

이 문제는 기본적으로 기존 fragment의 객체가 같을 때 발생하기 때문에 애초에 새로운 frament 객체를 만들어서 replace() 한다면 문제가 생기지는 않는다.<br>

<br>

## 마무리

addToBackStack()을 호출하면 add()와 replace()로 더 다양한 액션이 가능하다(뒤로가기 버튼을 눌렀을 때).<br>

처음에 이 개념을 배울 때 replace()가 필요한 이유는 알겠지만 add()는 굳이? 라는 생각을 했었다. 하지만 데이터를 유지(show()와 hide()와 함께)해야하거나 addToBackStack()과 함께 쓰일 때 필요하다는 것을 이젠 안다.<br>

addToBackStack()과 add()/replace()가 어떻게 쓰이는지 알고싶다면 다음 게시글을 클릭해보시라. 잘 정리해놓으셨다: [addToBackStack() 알아보기](https://zzandoli.tistory.com/55).<br>
