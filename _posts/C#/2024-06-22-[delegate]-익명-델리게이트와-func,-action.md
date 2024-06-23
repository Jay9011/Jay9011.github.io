---
title: "[delegate] 03. 익명 델리게이트와 Func, Action"
date: 2024-06-22 18:07:43 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
    - 델리게이트
    - 동적
    - 참조
    - 함수
    - 포인터
math: false
type: C#
keywords:
    - 델리게이트
    - delegate
    - Func
    - Action
---

# 익명 델리게이트(Anonymous Delegate)

델리게이트에 **메서드 참조**를 저장하기 위해 함수의 식별자(이름)을 제공하지만, 가끔씩은 특정 함수 내에서만 사용되어 함수를 따로 만들어 둘 필요가 없을 때도 있습니다. (이름이 필요 없는 일회용 함수를 사용하고 싶을 때)
<br>
이럴 때, 이름이 명시되지 않은 델리게이트인 익명 델리게이트를 사용할 수 있습니다.

<span class="keyword">**익명 델리게이트**</span>는 델리게이트 키워드를 사용하여 선언되며, 이후에 <span class="font_highlight">실행할 메서드의 **본문이 바로 정의**</span>됩니다.

이렇게 함으로써 별도의 메서드를 정의할 필요 없이 코드를 간결하게 만들 수 있습니다.

```text
delegate(매개변수){ // 실행할 코드 };
```

예를 들어, 어떤 숫자 배열에서 짝수나 홀수의 개수를 세는 동작을 만든다고 해봅시다.

```csharp
public delegate bool Predicate<T>(T x);

static class Utilities
{
    public static int Count<T>(IEnumerable<T> items, Predicate<T> predMethod)
    {
        int count = 0;
        foreach (var item in items)
        {
            if (predMethod(item))
            {
                count++;
            }
        }
        return count;
    }
}

internal class Program
{
	static void Main(string[] args)
	{
		var numbers = new List<int> {1, 2, 3, 4, 5, 6, 7, 8, 9};
		
		int oddNumbers = Utilities.Count(numbers, delegate(int n) { return n % 2 != 0; });
		Console.WriteLine(oddNumbers); // 5
		
		int evenNumbers = Utilities.Count(numbers, delegate(int n) { return n % 2 == 0; });
		Console.WriteLine(evenNumbers); // 4
	}
}
```

위 예제처럼, 어떤 수를 세기 위한 유틸리티로 범용성을 높이기 위해 타입 `T`에 맞는 컬렉션과 사용자 정의 델리게이트를 받아옵니다.
<br>
이때, 각각 홀수와 짝수를 판단하기 위한 메서드는 이 `Utilities.Count`를 호출하는 위치에서 각각 알맞는 함수를 넘겨 받으면 되기 때문에, 굳이 이름을 갖는 메소드를 따로 정의하지 않고 이름 없이 인라인 메서드로 넘겨줄 수 
있습니다.

이렇게 이름 없이 **본문이 바로 정의**된 메서드 델리게이트를 **익명 델리게이트**라고 합니다.

# Func, Action 델리게이트

하지만, 위에서처럼 익명 델리게이트를 사용하는데 있어서 굳이 델리게이트 선언부(`public delegate bool Predicate<T>(T x);`)가 필요할까?하는 생각이 들게 됩니다.

이럴때 쓸 수 있는게 `Func<>`나 `Action<>`입니다.

C#의 `Func`와 `Action` 델리게이트는 익명 메서드(익명 델리게이트나 람다식 등...)를 정의하고 사용하는 데 매우 유용한 기능입니다.

기존 델리게이트는 명시적으로 정의해야 하며, 이를 매개변수로 사용할 때도 명시적으로 타입을 지정해야 하지만,
<br>
`Func<>`와 `Action<>`은 <span class="font_highlight">기존의 델리게이트보다 **간결**하고 델리게이트 정의 없이 **간편**하게 사용</span>할 수 있는 제네릭 델리게이트입니다.

```csharp
static class Utilities
{
    public static int Count<T>(IEnumerable<T> items, Func<T, bool> predMethod)
    {
        int count = 0;
        foreach (var item in items)
        {
            if (predMethod(item))
            {
                count++;
            }
        }
        return count;
    }
}
// ...
var numbers = new List<int> {1, 2, 3, 4, 5, 6, 7, 8, 9};

int oddNumbers = Utilities.Count(numbers, delegate(int n) { return n % 2 != 0; });
Console.WriteLine(oddNumbers); // 5

int evenNumbers = Utilities.Count(numbers, delegate(int n) { return n % 2 == 0; });
Console.WriteLine(evenNumbers); // 4
```

위의 예제는 명시적인 델리게이트 선언 없이, `Func<T, bool>`로 바로 익명 델리게이트를 받아오는 모습을 볼 수 있습니다.

# Func 델리게이트

<span class="keyword">**Func 델리게이트**</span>는 <span class="font_highlight">**반환 값**을 가지는 메서드</span>를 나타내는 데 사용됩니다.

최대 16개의 매개변수를 받을 수 있으며, <span class="font_highlight">**마지막 타입** 매개변수가 **반환 값**의 타입</span>을 나타냅니다.

```csharp
internal class Program
{
	static void Main(string[] args)
	{
		Func<int, int, int> add = delegate(int x, int y) { return x + y; };
		Console.WriteLine( add(3, 4) ); // 7
	}
}
```

위의 예에서 `Func<int, int, int>`는 두 개의 `int` 매개변수를 받아 `int`를 반환하는 델리게이트를 정의합니다.
<br>
이때, 델리게이트를 명시적으로 선언 할 필요가 없어, 함수 내부에서 직접 만들어 사용할 수도 있습니다.

# Action 델리게이트

<span class="keyword">**Action 델리게이트**</span>는 <span class="font_highlight">**반환 값이 없는** 메서드</span>를 나타내는 데 사용됩니다.

`Action`도 최대 16개의 매개변수를 받을 수 있습니다.

```csharp
internal class Program
{
	static void Main(string[] args)
	{
		Action<int> print = delegate(int x) { Console.WriteLine(x); };
		print(10); // 10
	}
}
```

위의 예에서 `Action<int>`은 `int` 타입의 매개변수를 받고 바로 출력하는 델리게이트를 정의합니다.
<br>
이 역시, 델리게이트를 명시적으로 선언 할 필요가 없어, 함수 내부에서 직접 만들어 사용할 수도 있습니다.

# Func와 Action을 델리게이트로 사용

그렇다면, `Func<>`와 `Action<>`은 익명 델리게이트를 받는데에만 사용하는가? 하면 그렇지는 않습니다.

위에서 간단하게 만든 예제에서 볼 수 있듯이 익명 델리게이트를 바로 바로 만들어서 사용할 수도 있지만, 기존의 델리게이트처럼 혹은 이벤트처럼 필드에 저장하고 호출하는 방식으로도 쓸 수 있습니다.

```csharp
class TestClass
{
    private Action ActionDelegate;
    private Func<int, int, int> AddDelegate;
    public void AddAction(Action action)
    {
        ActionDelegate += action;
    }
    public void OnAction()
    {
        ActionDelegate?.Invoke();
    }
}
internal class Program
{
	private static void ActionTest()
	{
	    Console.WriteLine("ActionTest");
	}
	static void Main(string[] args)
	{
		TestClass testClass = new TestClass();
		testClass.AddAction(ActionTest);
		testClass.OnAction(); // ActionTest
	}
}
```

위 예제에서 보듯이 `event`처럼 델리게이트 없이 필드에 델리게이트를 저장하는 필드를 만들 수 있고, `+=` 연산자로 명명된 메서드(이름이 있는 메서드)를 연결한 뒤 호출할 수도 있습니다.

<br>
이번 글에서는 익명 델리게이트에 대해 설명하고, Func와 Action을 다루는데, 익명 델리게이트를 사용했습니다.
<br>
그러나 현대의 C# 개발 환경에서는 이러한 익명 델리게이트보다 더 간결하고 효율적인 람다 표현식을 더 자주 사용합니다.
<br>
람다에 대해서는 다음에 다루도록 하겠습니다.

---

# 참고

[C＃의 람다(Lambda)와 캡처(Capture), 클로저(closure)](/posts/c-%EC%9D%98-%EB%9E%8C%EB%8B%A4(lambda)%EC%99%80-%EC%BA%A1%EC%B2%98(capture),-%ED%81%B4%EB%A1%9C%EC%A0%80(closure)/){: }


[[delegate] 01.델리게이트의 정의와 정체, 체이닝, 사용 목적](/posts/delegate-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A0%95%EC%B2%B4,-%EC%B2%B4%EC%9D%B4%EB%8B%9D,-%EC%82%AC%EC%9A%A9-%EB%AA%A9%EC%A0%81/){: }

[[delegate] 02. event와 EventArgs (EventHandler)](/posts/delegate-event%EC%99%80-eventargs-(eventhandler)/){: }
