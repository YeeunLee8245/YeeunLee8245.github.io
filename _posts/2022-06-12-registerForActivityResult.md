---
title: "[Android]registerForActivityResult() 원리와 구현"
excerpt: "registerForActivityResult, ActivityResultLauncher, ActivityResultContracts"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/20220612_registerForActivityResult.png

categories:
  - Android
tags:
  - android
last_modified_at: 2022-06-12

---

## 목차

1. registerForActivityResult()란?
2. 활용
3. 주의할 점
4. 마무리<br>

## 1. registerForActivityResult()란?

A 액티비티에서 B 액티비티로 화면을 전환했다고 생각해보자. <br>

B 액티비티에서 해야할 작업을 끝내고 다시 A 액티비티로 돌아갔을 때 호출 되어야하는 함수가 필요할 수 있다.<br>

그렇다. 우리는 B 액티비티가 종료될 때 실행될 함수를 A 액티비티 안에서 registerForActivityResult()라는 메서드로 정의해줄 수 있다!<br>

❓기존에 있던 걸 사용하지 않고 왜❓

원래는 startActivityForResult와 OnActivityResult 메서드를 통해 이 작업을 구현하였는데 2020년 5월을 기준으로 deprecated 되고 메모리, 유지보수 측면에서 더 효율적인 **registerForActivityResult**가 제공되고있다. <br>

1. registerForActivityResult은 앞선 메서드로 구현했을 때와 달리, 메모리가 부족하여 B 액티비티를 보여주고 있는 상태에서 A액티비티가 종료됐을 때 A 액티비티에 정의된 Result CallBack를 유지시킨다(정확히는 **ActivityResultLauncher**로 인해).<br>

메모리 부족으로 인해 사라졌던 액티비티일지라도 ActivityResultLauncher는 registerForActivityResult 메서드가 다시 콜백을 등록하도록 요청한다는 것.<br>

2. 한 메서드에 request 파라미터를 받아 분기문으로 액티비티 별 작업을 전부 나누어줘야했던 것을 registerForActivityResult의 파라미터로 넘겨줘야하는 **ActivityResultContracts**를 통해 액티비티 별로 콜백 함수를 등록할 수 있게 한다.<br>

이로써 액티비티 종료 데이터를 받아오는 코드를 더 깔끔하고 객체지향적으로 짤 수 있게 됐다.<br>

<br>

## 2. 활용

▶️ A액티비티에서 B액티비티를 호출했다가 종료하기(A액티비티로 돌아감)

* ActivityA.kt

```kotlin
class ActivityA : AppCompatActivity() {
	private lateinit var textLauncher: ActivityResultLauncher<Intent>
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    // B액티비티 종료 콜백 정의
    textLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result: ActivityResult ->
      if (result.resultCode == Activity.RESULT_OK) {	// 액티비티B 정상종료 확인
       	val txt: String? = result.data?.getStringExtra("name")	// 값 추출
        txt?.let{
          Toast.makeText(this,"B액티비티에서 입력한 text:${txt}",Toast.LENGTH_SHORT).show()
        }
    	}                                                                                       
  	}
    
    val intentB = Intent(this, ActivityB::class.java)
    textLauncher.launch(intentB)	// B액티비티 호출
  }
}
```

<br>

* ActivityB.kt


```kotlin
class ActivityB : AppCompatActivity() {
  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    val txt:String = "이예은"
    val intentA = Intent(this, ActivityA::class.java).apply{
      putExtra("name", txt)
    }
    setResult(RESULT_OK, intentA)	// 결과 데이터(정상 종료, txt) 삽입
    finish()	// B액티비티는 생성과 동시에 종료
  }
}
```

<br>

## 3. 주의할 점

* registerForActivityResult는 반드시 Activity 또는 Fragment의 생명주기인 **onCreate**가 끝나기 이전에 정의되어야 한다. 

어느 블로그에서는 onStart까지도 정상 실행된다고 말한다. 되도록이면  프로퍼티 또는 onCreate 내부에서 정의하자.<br>

onCreate 메서드가 끝나고 난 뒤에 정의된다면 해당 Activity가 메모리 부족 등의 이유로 종료되고 나중에 다시 실행될 때 반환된 결과 값을 받아오는 종료 콜백 함수(registerForActivityResult)를 찾지 못할 수도 있기 때문이다.<br>

* 권한을 받아올 때 이전 권한 체크 여부를 받아오는 메서드를 직접 호출하지 않아도 된다.

이 블로거 분이 잘 정리해놓으셨다: [Permission 요청](https://modelmaker.tistory.com/18).<br>

<br>

## 마무리

개발하다가 deprecated된 코드를 마주칠 때면 매번 의아하지만 업데이트 된 Document를 보다보면 납득이 된다. 안드로이드 개발자가 우수한 코드를 작성할 수 있도록 Android API를 발전시켜주시는 개발자 분들께 감사하다.<br>
