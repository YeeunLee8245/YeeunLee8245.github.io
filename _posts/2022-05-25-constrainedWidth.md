|      | ---                                                          |
| ---- | ------------------------------------------------------------ |
|      | title: "ConstraintLayout: layout_constrainedWidth/Height 알아보기" |
|      | excerpt: "ConstraintLayout, layout_constrainedWidth, layout_constrainedHeight" |
|      | toc: true                                                    |
|      | toc_sticky: true                                             |
|      | toc_label: "페이지 목차"                                     |
|      | header:                                                      |
|      | teaser: /assets/images/20220524_width01.png                  |
|      |                                                              |
|      | categories:                                                  |
|      | - Android                                                    |
|      | tags:                                                        |
|      | - android, kotlin                                            |
|      | last_modified_at: 2022-05-25                                 |
|      | ---                                                          |

## 목차

1. layout_constrainedWidth/Height의 역할
2. 활용
3. 마무리<br>

## 1. layout_constrainedWidth/Height의 역할

안드로이드의 layout을 개발하다보면 초반에는 비교적 간단한 LinearLayout에 손이 많이 가지만 속성을 통해 더 자유로운 개발이 가능한 ConstraintLayout이 편해질 때가 온다.<br>

이번 글은 ConstraintLayout의 속성 중 하나인 layout_constrainedWidth/Height에 대한 것이다.<br>

우선 왜 필요할까?<br>

특정 View의 최대 크기를 부모 Layout(ConstraintLayout)에 맞춰 지정해주는 것이다.<br>

예를 들어 View가 텍스트 수에 따라 길이가 달라질 때, 텍스트 수가 화면 가로 길이를 벗어날 정도로 길다면  

layout_constrainedWidth을 true로 설정하여 View가 부자연스럽게 잘리는 것을 방지해준다.<br>

## 2. 활용

* app:layout_constrainedWidth="false"

  <img src="/assets/images/20220524_width02.png"><br>

  default는 false로 지정되어있기 때문에 속성을 정의하지 않아도 false한 것과 같다.<br>

  View의 최대 크기를 지정해주지 않으면 화면에서 View가 부자연스럽게 잘리는 것을 볼 수 있다.<br><br><br>

* app:layout_constrainedWidth="true"

  <img src="/assets/images/20220524_width01.png"><br>

​	View의 최대 크기를 부모 Layout(ConstraintLayout)에 맞춰 지정했기 때문에 텍스트 수가 길어지더라도 부모 Layout 너비에 맞춰 크기가 조절된다.<br>

텍스트 뒤에 말줄임표(...)는 android:ellipsize="end" 속성을 정의했기 때문이다.<br>

## 3. 마무리

width만 서술했지만 height도 동일한 원리로 사용하면 된다.<br>

본문에 사용한 코드는 아래 링크에서 확인할 수 있다.<br>

[app:layout_constrainedWidth="true"](https://github.com/YeeunLee8245/Holo-AndroidApp/blob/ba5baf738aec8871c8fd64738f3caaf801462def/app/src/main/res/layout/item_chat_list_recycler.xml)<br>

ConstraintLayout 속성은 정말 다양한 것 같다. 제대로 익혀서 능수능란하게 사용하고싶다.<br>

