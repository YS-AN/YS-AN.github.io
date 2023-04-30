---
title : Casting
author : YS_AN
date : 2023-03-28 22:36:06 +0900
categories : [CSharp]
tags : [C#]
---

## 형변환 (Casting)

- 데이터를 선언한 자료형에 맞는 형태로 변환하는 작업
- C#의 자료형은 엄격하게 관리되기 때문에 다른 자료형의 데이터를 저장 불가
- 다른 자료형의 데이터를 저장하기 위해선 형변환 과정을 거쳐야 하며, <br>
&nbsp;이 과정에서 보관할 수 없는 데이터는 버려짐.
- 변환할 데이터의 앞에 변환할 자료형을 괄호 안에 넣어 형변환을 함 <br>
&nbsp;[자료형 타입] [변수명] = (자료형 타입) 데이터; (ex. int iVal = (int)166.666666f;)
- 명시적으로 변환이 가능항 경우 자동으로 형변환이 진행됨

### 문자열 변환

- 문자열은 단순형변환이 불가능
- 각 자료형의 Parse를 이용하여 문자열에서 자료형으로 변환
- Parse를 이용하여 변환이 불가능한 경우 예외처리 발생
- TryParse를 이용하면 변환 가능 여부까지 확인이 가능하다

[형변환 예제]

```csharp
internal class TypeCasting
{
	public void StartTypeCasting()
	{
		int iVal = (int)166.666666f; //정수는 소수점을 저장할 수 없기 때문에 소수점자리 절사

		float fVal = 3;//부동소수점형 변수에 정수 데이터를 넣은 경우, 자동으로 형 변환이 됨. 
		double dVal = 1.2f; //double은 float를 포함하는 큰 범위라 자동으로 형 변환이 됨. 

		fVal = (float)iVal;
		//일반적으로 변수의 형변환 같은 경우 자동형변환이 가능하다 하더라도 형변환을 적어줌
		//명시적으로 적어주는 과정을 통해 의도적으로 형변환을 진행했음을 나타냄. 

		//문자열 변환 > Parse를 이용해야함.
		string text = "142";
		iVal = int.Parse(text); //int.Parse를 통해 string자료형을 int형 자료형으로 사용한다 

		text = "abc";
		//iVal = int.Parse(text); //형변환이 불가능한 문자열을 변환하려 하는 경우 예외처리 발생 
		bool success = int.TryParse(text, out iVal); // 변환 가능 여부를 반환. 변환값은 iVal에 들어감. 

		Console.WriteLine($"처리 결과 : {success}, 값 : {iVal}");

		//char의 경우에는 숫자로 변환이 가능함. 

		int iVal2 = (int)'한';
		Console.WriteLine(iVal2); //유니코드 값이 반환 됨.

		int iVal3 = (int)'A';
		Console.WriteLine(iVal3); //아스키코드 값이 반환 됨
	}
}
```