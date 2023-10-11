---
title : 그래픽스 API 초기화
author : YS_AN
date : 2023-07-19 20:24:13 +0900
categories : [Unity, 게임그래픽스]
tags : [Unity, 기술면접, 최적화]
---

# 그래픽스 API 초기화

GPU를 사용하기 위해서는 사전 작업이 필요하다.

GPU는 CPU에 비해 상대적으로 느리기 때문에 CPU가 요청하는 그래픽 작업을 바로 대응할 수 없다. 
→ CPU 처리 속도에 맞춰 일을 계속 요청하게 되면 병목현상이 발생하게 된다. 

병목현상을 해결하기 위해 CPU의 요청들을 커맨드 큐에 넣어두고, GPU는 큐에서 요청을 꺼내어 처리한다. 

여기서 CPU가 GPU에게 그래픽 작업을 수행하도록 <span style='background-color: #FBECDD'>커맨드 큐에 요청을 넣는 행위를 드로우 콜(Draw Call)</span>이라고 한다.
(처리할 양이 많아졌다 = 드로우 콜이 많다.)
![드로우콜 이미지](../../assets/img//post/Unity/GameGraphics/071901_DrawCall.PNG)


## 드로우 콜

### 드로우 콜이 발생하는 경우

- 하나의 오브젝트에 Mesh가 여러 개인 경우
- 하나의 오브젝트에 여러 개의 Material이 있는 경우
- 하나의 쉐이더에 멀티 패스가 정의되어 있는 경우 <br/> (멀티패스 : 두 번 이상 렌더링을 거치는 경우)


### 드로우 콜을 줄이기 위한 Batching 시스템

드로우콜 횟수가 크만 성능에 영향을 끼칠 수 있다.
유니티에서 드로우콜을 줄이기 위해서 Batching 기술을 활용한다. 

**Static Batching (정적 배칭)**
 * 정적인 오브젝트를 위핸 배칭 기법으로 주로 배경 오브젝트들에 활용된다. 
 * 오브젝트의 인스펙터 창에서 Static을 체크해줘야한다. <br/>
   체크 시 static batching 대상으로 인정받아 로딩 타임에 자동으로 배칭 처리가 된다. <br/>
   로딩 타임에 배칭 처리를 하기 때문에 처음부터 씬에 존재해야 하지만, 나중에 추가된 오브젝트도 StaticBatchingUtility.Combine()을 통해 Static Batching에 추가할 수 있다. <br/>
   (런타임 상에서도 정적인 오브젝트 추가가 가능하다) <br/>
   하지만, Static Batching에 추가하기 위한 시간이 필요하기 때문에 되도록 자제하는 것이 좋다.
 * 런타임에서 수행할 Vertex 연산이 없기 때문에 Dynamic Batching 효율적이다.
 
**Dynamic Batching (동적 배칭)**
 * Static과 인스펙터 창에서 Static이 체크되어 있지 않은 동적인 오브젝트들에 활용된다. 
 * 동적인 오브젝트 중 동일한 Material을 사용하고, 특정 조건을 만족하는 오브젝트들을 대상으로 배칭처리를 하는 기능이다. 
 * 런타임상에서 배칭처리가 진행된다. 
 * 매 프레임 씬에서 동적인 오브젝트들의 Vertex를 모아서 합쳐주는 과정을 거친다. <br/>
   모은 Vertex들을 Dynamic Batching에 쓰이는 Vertex Buffer와 Index Buffer에 담으면 GPU가 가져가서 렌더링한다. <br/>
   결과적으로 매번 데이터 구축과 갱신이 발생하기 때문에 매 프레임마다 오버헤드가 발생한다. <br/>
   (배칭 오버헤드와 드로우 콜 시간을 비교해 더 빠른쪽으로 진행해야 한다.) <br/>
   오버헤드가 발생하기 때문에 제약 사항이 많다. → 일반적으로 Static Batching보다 잘 쓰이지 않는다. 
 * 만약 특정 오브젝트의 배칭 오버헤드가 더 커서 배칭을 쓰지 않도록 하고 싶다면 쉐이더 태그에서 DisableBatching 플래그를 True로 설정해주면 된다.
 
**2D Sprite Batching**
 * Static이나 Dynamic Batching처럼 체크하지 않아도 자동으로 배칭이 이뤄진다.
 * Sprite들을 하나의 아틀라스로 합치면 단일 텍스처를 호출함으로써 Batch를 줄일 수 있다. <br/>
   스프라이트 아틀라스(Sprite Atlas) 는 여러 개의 텍스처를 단일 텍스처로 결합하는 에셋

**GPU Instancing**
 * 인스턴싱이란 동일한 메시의 복사본을 만드는 것으로 <br>
   단일 드로우 콜에서 동일한 재질의 메시 사본을 여러 개 렌더링하는 방법이다. 
 * Batching과 달리 Mesh 정보를 합치는 과정이 없기 때문에 상대적으로 오버헤드가 적다.