---
title: "[백준 16968번][Kotlin] 차량 번호판 1"
excerpt: "알고리즘, 백준, Kotlin"
toc: true
toc_sticky: true
toc_label: "페이지 목차"
header:
  teaser: /assets/images/20230125_baekjoon.png

categories:
  - Algorithm
tags:
  - algorithm
  - kotlin
last_modified_at: 2023-01-25
---

## 문제 링크
https://www.acmicpc.net/problem/16968
<br>
## 코드
```kotlin
fun main() {
    val readStr = readln()
    var total = 1
    val readStrLen = readStr.length
    var preStr = '0'    // 큰 따옴표 = String 타입, 작은 따옴표 = Char

    for (i in 0 until readStrLen) {
        if (preStr == readStr[i]) {
            if (readStr[i] == 'd')
                total *= 9
            else if (readStr[i] == 'c')
                total *= 25
        } else {
            if (readStr[i] == 'd')
                total *= 10
            else if (readStr[i] == 'c')
                total *= 26
        }
        preStr = readStr[i] // String의 요소는 Char 타입을 반환한다
    }
    println(total)
}
```

## 설명
입력 받은 문자열에서 한 문자씩 탐색한다.<br>
i번째 위치의 문자가 i-1번째 위치의 문자와 동일하다면 연속된 것이라 볼 수 있다. 연속되지 않는 위치라면 d든 c든 모든 경우의 문자가 올 수 있으므로 해당하는 경우 만큼(10 또는 26) 곱하고, 연속할 수 있는 위치라면 바로 앞의 문자는 오지 못하므로 올 수 있는 문자의 수에서 1을 뺀 수(9 또는 25)를 곱한다.<br>
dddc나 dddd 같이 3연속, 4연속 문자열이 입력으로 오게된다해도 마찬가지다. 우리는 번호판 문자의 2번 연속만 피하면 되므로 i번째 위치의 문자가 i-2번째 위치의 문자와 같다고해도 i-1번째 위치의 문자와 다르기만 하면 된다.
<br>
## 후기
처음에 전체 경우의 수에서 겹치는 수를 빼는 것에만 꽂혀서 시간을 꽤나 버렸었다. 막히면 여집합으로도 생각해보고 여러 경우의 수를 통해서 내가 도출한 알고리즘에 허점이 없는 지 계산 후에 코드로 옮기자.
