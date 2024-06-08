---
title: 선택적 매개변수(Optional Parameter)
date: 2024-05-21 21:50:04 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 변수
  - 함수
  - 다형성
math: false
type: C#
keywords:
  - 선택적 매개변수
  - Optional Parameter
---

C#에서 선택적 매개변수(optional parameter)는 메서드, 생성자, 인덱서, 대리자 등에서 <span class="font_highlight">**특정 매개변수에 기본값**을 제공</span>하여 <span class="font_highlight">호출자가 해당 매개변수를 **생략**할 수 있게 하는 기능</span>입니다.

이 기능은 코드의 유연성을 증가시키고, 메서드 오버로드를 줄여 코드의 간결성과 가독성을 향상시킵니다.

# 선언 방법

## 기본값 지정

선택적 매개변수를 사용하려면, 해당 매개변수에 **기본값**을 지정해야 합니다. 매개변수가 생략될 경우, 지정된 기본값이 사용됩니다.

아래 예제는 선택적 매개변수의 사용법입니다.

```csharp
public class Program
{
    // 선택적 매개변수를 포함한 메서드
    public static void PrintMessage(string message, int repeatCount = 1)
	{
	    for (int i = 0; i < repeatCount; i++)
	    {
	        Console.WriteLine(message);
	    }
	}

    public static void Main()
    {
        // 선택적 매개변수 없이 메서드 호출
        PrintMessage("안녕하세요."); // 안녕하세요.

        // 선택적 매개변수를 사용하여 메서드 호출
        PrintMessage("반갑습니다.", 3);
		/*  반갑습니다.
		 *  반갑습니다.
		 *  반갑습니다.
		 */
    }
}
```
이 예제에서 `PrintMessage` 메서드는 `repeatCount` 매개변수에 기본값 `1`을 제공합니다. 이를 통해 사용자는 이 매개변수를 생략할 수 있으며, 생략 시 기본값 `1`이 사용됩니다.

## 선택적 매개변수 위치

선택적 매개변수를 선언 할 때, 주의할 점이 있습니다.

바로 **선택적 매개변수**가 선언된 **이후의 매개변수**는 **모두 선택적**이어야 한다는 것입니다.

즉, 선택적 매개변수는 <span class="font_highlight">반드시 **매개변수 목록 뒤쪽**에 위치</span>해야 합니다.

```csharp
public void ExampleMethod(int requiredParam, int optionalParam = 10) // 올바른 선언

public void ExampleMethod(int optionalParam = 10, int requiredParam) // 오류 발생
```

# 선택적 매개변수와 오버로딩

선택적 매개변수를 사용하면 메서드 오버로딩(Overloading)을 줄이는 데 도움을 줄 수 있습니다.
<br>
메서드 오버로딩이란, **같은 이름**의 메서드를 다른 매개변수 **타입**이나 **개수**로 정의하는 것을 말합니다.

이런 선택적 매개변수를 사용함으로써 매개변수의 개수에 따라 다수의 메서드를 정의할 필요가 줄어듭니다.



다음은 오버로딩을 사용한 예제와 선택적 매개변수를 사용한 예제입니다.

### 오버로딩 사용 예제

```csharp
public void Display(string text)
{
    Display(text, 1);
}

public void Display(string text, int count)
{
    for (int i = 0; i < count; i++)
    {
        Console.WriteLine(text);
    }
}
```

### 선택적 매개변수 사용 예제

```csharp
public void Display(string text, int count = 1)  // 단 하나의 메서드로 두 가지 방법으로 호출할 수 있다.
{
    for (int i = 0; i < count; i++)
    {
        Console.WriteLine(text);
    }
}
```

선택적 매개변수를 사용하면 메서드 오버로딩을 줄이고 코드를 간소화하여 **가독성**을 높이고 **유지보수**를 쉽게 할 수 있게 합니다.

하지만, 모든 상황에서 오버로딩을 완전히 대체할 수는 없습니다. 때에 따라서는 오버로딩이 더 명확할 수 있습니다.

# 명명된 인수와 함께 사용

C#의 선택적 매개변수는 C++의 디폴트 매개변수는 비슷한 개념이지만 다른 점이 존재합니다.

바로, C#에서는 선택적 매개변수를 사용할 때 [명명된 인수(named argument)](/posts/%EB%AA%85%EB%AA%85%EB%90%9C-%EC%9D%B8%EC%88%98-(named-argument)/){: target="_blank" }를 함께 사용하여 원하는 매개변수만 선택해 사용할 수 있다는 점입니다.

어쩌면, 이 점에서 '선택적' 매개변수라는 이름이 붙었을지도 모르겠습니다.

```csharp
ublic static void PrintDetails(string name, int age = 0, string address = "")
{
	Console.WriteLine($"Name: {name}, Age: {age}, Address: {address}");
}

// 명명된 인수와 함께 사용
OptionalParam.PrintDetails("홍길동", address: "한국");   // Name: 홍길동, Age: 0, Address: 한국
```

이 예제에서는 `age` 매개변수를 생략하고 `address`만 지정하여 호출하는 모습을 보여줍니다.

> 명명된 인수란?
> 
> 메서드 호출 시 인수를 지정할 때 사용하는 방법 중 하나로, **인수의 이름을 명시적으로 지정**하여 값을 전달하는 것을 말합니다.
> <br>
> 인수에 매개변수 이름을 명시하면, **순서와 무관**하게 변경을 원하는 **특정 인수만 변경**할 수 있습니다.
{: .prompt-info}

---

# 참고

[명명된 인수 (Named Argument)](/posts/%EB%AA%85%EB%AA%85%EB%90%9C-%EC%9D%B8%EC%88%98-(named-argument)/){: }

[디폴트 매개변수 (Default parameters) - C++](/posts/%EB%94%94%ED%8F%B4%ED%8A%B8-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98-(default-parameters)/){: }