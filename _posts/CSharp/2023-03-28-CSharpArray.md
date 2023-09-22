---
title : Casting
author : YS_AN
date : 2023-03-28 22:36:06 +0900
categories : [CSharp]
tags : [C#]
---

## 배열 (Array)

- 동일한 자료형의 요소들로 구성된 데이터 집합 <br>
- 인덱스를 통하여 각각의 배열 요소(Element)에 접근할 수 있음 <br>
    (인덱스 = 배열에서 각 위치를 가리키는 숫자) <br>
- 배열의 처음 요소의 인덱스는 0부터 시작함

### 배열의 선언

자료형 뒤에 [ ]괄호를 추가하여 배열로 사용함을 선언함 <br>
→ (자료형)[ ] (배열명); (ex. int[ ] iArr; )

### 다차원 배열

자료형뒤에 [ ]괄호를 추가하며, 추가하는 차원 수만큼 ‘<b>,</b>(콤마)’를 추가한다 <br>
ex. int [ , ] iArr = new int [10, 10]

[배열 예제]
```csharp
internal class ArrayType
{
	public void StartArrayType()
	{
		int[] iArr; //int배열 선언
		iArr = new int[200]; //int 데이터를 200개 가지는 배열을 선언함 

		//선언과 동시에 초기화
		int[] iArr2 = new int[200];
		int[] iArr3 = { 1, 2, 3, 4, 5 }; //각 배열의 요소 값까지 초기화 하는 방법. 

		//배열의 접근 - index
		iArr[19] = 200;
		iArr[3] = iArr[19];

		//다차원 배열의 선언
		int[,] iMat = new int[8, 8]; //2차원 배열
		int[,,] iCube = new int[3, 3, 3]; //3차원 배열
		
		//다차원 배열 접근
		iMat[5, 4] = 10;
		iCube[0, 1, 2] = 7;
	} 
}
```
