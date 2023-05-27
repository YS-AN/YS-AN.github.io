---
title : Coroutine
author : YS_AN
date : 2023-05-15 16:05:06 +0900
categories : [Unity]
tags : [Unity]
---

### Coroutine
코(Co-, 협업) + 루틴 = 같이 흘러가는 루틴

- 작업을 다수의 프레임에 분산할 수 있는 비동기식 작업 <br>
(비동기식 작업 = 안 보고 있어도 알아서 하고 있있는 작업)
- 반복가능한 작업을 분산하여 진행하며, 실행을 일시정지하고 중단한 부분부터 다시시작할 수 있음
- 단, 코루틴은 스레드가 아니며 코루틴의 작업은 여전히 메인 스레드에서 실행 <br>
→ 게임은 동기화 문제 때문에 멀티 스레드를 그렇게 좋아하지 않음. 그래서 코루틴은 그냥 `메인 루틴에서 분산 작업`을 함 (시분할 시스템)



코루틴 작성 방법
* 반복 작업이기 때문에 반환형은 `IEnumerator`로 설정함 
* 작업 중간 멈춰야 하는 부분에 `yield`키워드로 반환함. 

코루틴 작성 시 많이 활용하는 함수
WaitForSeconds() : N초 기다림
WaitForEndOfFrame() : 프레임 끝까지 기다림

코루틴의 시작 : StartCorouine () 
코루틴의 중지 
    * 작업 종료 시 자동으로 중지됨
    * 강제 중지 시 StopCoroutine()
    * 전체 중지 시 StopAllCoroutine() → MonoBehaviour에서 시작시킨 모든 코루틴 중지

총알을 