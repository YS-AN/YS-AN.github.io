---
title : Coroutine
author : YS_AN
date : 2023-10-14 10:09:00 +0900
categories : [Unity, UnityInfo]
tags : [Unity, 기술면접]
---

# Coroutine
실행을 일시 중지하고 Unity에 제어 권한을 반환한 후 다음 프레임에서 중단했던 위치에서 계속할 수 있는 메서드이다. <br/>
작업을 다수의 프레임에 분산할 수 있다. → 단일 스레드 환경인 유니티에서 비동기 처리가 필요할 때 활용한다. <br/><br/>
**주의**   <br/>
<span style='background-color:#FBECDD'>비동기 처럼 동작하지만 비동기 방식은 아니다.</span> <br/>
멀티 스레딩 모델의 비동기 방식은 병렬로 처리되기 때문에 함수 A의 완료와 함수 B의 실행 시점이 일치하지 않지만, <br/>
코루틴에서는 순차적으로 처리된다. (코루틴에서 작업이 늦을수록 다음 작업도 딜레이되는 현상이 발생하는 이유이기도 하다) <br/><br/>

> 동기식(Synchronous) VS 비동기식(Asynchronous)
> * 동기식 : 먼저 시작된 하나의 작업이 끝날 때까지 다른 작업을 시작하지 않고, 기다렸다가 다 끝난 후 새로운 작업을 시작하는 방식
> * 비동기식 : 먼저 시작된 작업의 완료 여부와 상관없이 새로운 작업을 시작하는 방식

> 비동기식 VS 멀티스레딩  <br/>
> 비동기는 일(task)의 순서에 관한 것이고, 멀티스레드는 작업자(worker)에 관한 것이다.  <br/>
> 예를 들어, 할 일이 밥먹기, 청소하기가 있을 경우,  <br/>
> 비동기는 "음식을 주문하자마자 청소 시작 후 음식이 도착하면 밥을 먹는다"라면, 멀티스레드는 "할 일을 여러명에서 같이 한다"이다. <br/>
> 멀티스레이면서동기일 수 있고, 멀티스레드이면서 비동기일 수 있다. <br/>
> 멀티 스레드이면서 동기일 경우, 여러 공간에서 순새도로 작업을 하는 것이고, 멀티스레드 이면서 비동기이면 여러 공간에서 순서를 신경쓰지 않고 작업하는 것이다. 

### 코루틴 구현 
IEnumerator를 사용하여 작성되며 yield return으로 멈춘 시점에서 다시 시작한다. 
```C#
IEnumerator 함수이름()
{
	yield return // + 조건
    // 함수 내용
}

StartCoroutine(함수이름()); //코루틴 시작
StopCoroutine(함수이름); //코루틴 종료
StopAllCoroutines(); //모든 코루틴을 종료
```
* 반드시 IEnuemrator을 반환해야한다.
* yield return을 만나면 코루틴 호출 위치로 돌아가고, yield return 이후에 있는 구문은 조건에 맞게 대기 후 수행한다. 
* yield break를 만나면 코루틴이 종료된다.
<br/>

**[ yield return + 종류(조건) ]**

|종류(조건)|설명|
|---|---|
|null|다음 프레임까지 대기 (Update()가 끝날 때 호출)|
|new WaitForEndOfFrame()|모든 랜더링 작업이 끝날 때까지 대기 <br/>(한 프레임워크가 완전히 종료될 때 호출)|
|new WaitForFixedUpdate()|FixedUpdate가 끝날 때 호출|
|new WaitForSeconds(float)|지정한 초(float)만큼 대기 후 호출|
|new WaitForSecondsRealtime(float)|지정한 초(float)만큼 대기 후 호출 <br/>(단, Time.timeScale의 영향을 받지 않는 절대적인 시간만큼 대기함)|
|new startCoroutine(string)|다른 코루틴이 끝날 때까지 대기|
|new WaitWhile(bool)|update와 LateUpdate 사이에서 호출되며, 결과값이 만족하면 대기, <br/>만족하지 않으면 yield return 이후 구문이 실행됩니다.|
|new WaitUntil(bool)|update와 LateUpdate 사이에서 호출되며, WaitWhiler과 반대로 <br/>결과값이 만족하면 yield return 이후 구문이 실행됩니다.|

⇒ yield return을 사용하여 반환 위치를 기억하다가 다음 호출 때 반환 위치 이후부터 실행할 수 있도록 한다.

### 코루틴 장단점 
장점
* 비동기식 처리가 가능하다 
* 프레임 독립적: 코루틴은 프레임 독립적입니다. 필요한 순간에 언제든지 게임 로직을 시작하고 일시 중지할 수 있다. 

단점
* 메모리 낭비 : 코루틴을 실행할 때마다 IEnumerator 인터페이스에 대한 객체가 생성된다. 코루틴 생성마다 새로운 가비지(IEnumerator)를 만들어내어 과도한 사용은 메모리 부담을 초래할 수 있다. 
* 코드 복잡성 : 코루틴을 오용하면 코드가 복잡해질 수 있습니다. → 지나치게 중첩된 코루틴은 유지 관리와 디버깅을 어렵게 만들 수 있다.

