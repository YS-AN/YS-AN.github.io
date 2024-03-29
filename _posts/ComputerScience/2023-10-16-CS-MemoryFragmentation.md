---
title : 메모리 단편화
author : YS_AN
date : 2023-10-16 14:29:08 +0900
categories : [Computer Science]
tags : [CS, 기술면접]
---

# 메모리 단편화 (Memory Fragmentation)
RAM에서 메모리의 공간이 작은 조각으로 나뉘어져 사용가능한 메모리가 충분히 존재하지만 할당(사용)이 불가능한 상태


### 내부 단편화 (Internal Fragmentation)
메모리를 할당할 때 프로세스가 필요한 양보다 더 큰 메모리가 할당되어서 프로세스에서 사용하는 메모리 공간이 낭비 되는 상황 
(예를 들어 프로그램을 실행하는데 OS가 10kb를 할당했지만, 실제로는 6kb만 사용 중이라면 4kb만큼은 내부 단편화가 생기는 것)

### 외부 단편화 (External Fragmentation)
메모리를 반복적으로 할당과 해제를 반복하면 메모리 중간 중간 사용하지 않는 메모리가 많아진다. 
총 메모리 양으로는 공간이 충분하지만 실제로 메모리 할당은 불가능한 상황


## 메모리 관리 기법

### 페이징 (Paging)
사용하지 않는 프레임을 페이지에 옮기고, 필요한 메모리를 페이지 단위로 프레임에 옮기는 기법. <br/>
⇒ 연속적이지 않은 공간도 활용할 수 있기 때문에 외부 단편화 문제를 해결할 수 있으나 내부 단편화 문제가 여전히 존재함. 페이지 단위를 작게하면 내부 단편화 문제도 해결할 수 있긴 하지만 Page Mapping 과정이 많아지기 때문에 효율이 떨어짐. (페이지를 적절히 나눠야 함) <br/>
* 페이지 : 보조기억장치를 이용한 가상메모리를 같은 크기의 블록으로 나눈 것
* 프레임 : RAM을 페이지와 같은 크기로 나눈 것
* 페이지와 프레임을 대응시키기 위해 page mapping과정이 필요해서 paging table이 필요함

### 세그멘테이션 (Segmentation)
가상 메모리를 서로 크기가 다른 논리적 단위은 세그먼트로 분할하고 메모리를 할당하여 실제 메모리 주소로 변환하는 기법. <br/>
	* 세그먼트 : 하나의 프로세스를 구성하는 주소 공간은 일반적으로 코드, 데이터, 스택 등의 의미있는 단위들로 구성된다. 세그먼트는 이와 같이 주소 공간을 기능 단위 또는 의미 단위로 나눈 것을 뜻함 <br/>
세그먼트는 연속적인 공간에 저장되어 있고, 세그먼트들의 크기가 다르기 때문에 미리 분할할 수 없음. 메모리에 적재될 때 빈 공간을 찾아 할당해야함. <br/>
프로세스가 필요한 메모리만큼 할당해주기 때문에 내부 단편화 문제는 일어나지 않지만 외부 단편화 문제는 여전히 존재함. <br/>

### 메모리 풀 (Memory Pool)
필요한 메모리 공간을 필요한 크기, 개수만큼 사용자가 직접 지정하여 미리 할당받아 놓고 필요할 때마다 사용하고 반납하는 기법 <br/>
미리 공간을 할당해놓고 가져다 쓰고 반납하면 할당괴 해제로 인한 외부 단편화가 발생하지 않고, 필요한 크기만큼 할당 해서 사용하기 대문에 내부 단편화가 일어나지 않음 <br/>
사용량보다 너무 큰 메모리 풀을 만들어 사용할 경우 메모리 단편화로 인한 메모리 낭비량보다 더 큰 낭비가 될 수 있으니 주의해야함. (메모리 누수 주의)