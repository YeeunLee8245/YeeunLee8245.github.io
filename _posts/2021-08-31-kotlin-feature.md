---
title:  "코틀린의 특징 5가지"
excerpt: "Null 안정성, 함수 확장, 함수형 프로그래밍, data 클래스, Immutable 변수"
header:
  teaser: /assets/images/kotlin.png

categories:
  - Kotlin
tags:
  - kotlin
  - basic
last_modified_at: 2021-08-31
---

## 특징

코틀린에만 국한된 특징은 아니지만 코틀린이 보다 효율적인 언어가 될 수 있었던 특징들을 살펴보겠다.    

   

* Null 안정성(Null safety)

  코틀린에서는 ?와 같은 특수문자를 이용해 Null exception를 간결하게 처리할 수 있다.   

  변수 정의와 동시에 Null 허용/ 불허용으로 지정할 수 있기 때문에 Null에 관련한 오류를 사전에 제어할 수 있다.

  

* 함수 확장

  객체지향 프로그래밍에서는 상속을 통해 상위 클래스의 객체를 하위 클래스에서 이용할 수 있다.  코틀린 역시 객체지향 프로그래밍에 속하기 때문에 상속을 이용할 수 있지만 과도하게 상속을 이용하면 코드 해석과 유지보수 측면에서 좋지않다.      

  따라서 코틀린에서는 이러한 단점을 해결하기 위해 함수 확장을 제공한다.     

  함수 확장을 통해 기존 클래스의 기능을 쉽게 추가할 수 있다.

  

* 함수형 프로그래밍

  함수형 프로그래밍은 대부분의 단위들를 함수로 나누어 사용할 수 있도록 해준다. 이는 프로그램의 유지보수에 효율적이다.      

  코틀린에서는 람다식과 고차함수 등을 제공하여 함수형 프로그래밍을 편리하게 사용할 수 있도록 한다.     

  

* data 클래스

  data 클래스는 이름에서부터 짐작할 수 있듯이 문제 해결을 위한 알고리즘을 가지고 있지 않고 단순 data들을 다루는 클래스를 말한다. 다시말해 이는 data들의 단순 나열이기 때문에 코드를 작성함에 있어서 귀찮고 코드를 지저분하게 만드는데 일조한다.      

  코틀린에서는 이를 짧게 작성할 수 있도록 data 클래스를 제공한다.

  

* Immutable 변수

  처음에 초기화한 값이 바뀌지 않아야 하는 변수가 필요할 때가 있다.    

  코틀린에서는 변수를 선언할 때 해당 변수의 mutable(가변)/immutable(불변)을 반드시 정해야한다.    
  단, 상수변수인 것은 아니다. 코틀린에서 모든 변수는 프로퍼티이기 때문에 값은 변경할 수 없지만 출력되는 값은 다르게 할 수 있다.   
  즉, 상수변수가 아닌 읽기 전용 변수라고 하는 것이 정확하다.    
  코틀린에서의 상수변수는 const 예약어로 선언할 수 있다. 
