---
title: C＃의 Predicate
date: 2024-06-23 16:39:29 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 델리게이트
math: false
type: C#
keywords:
  - Predicate
  - 델리게이트
---

 <span class="keyword">**Predicate**</span> 델리게이트는 `Func<>`나 `Action<>`과 같은 미리 정의된 [델리게이트](/posts/delegate-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A0%95%EC%B2%B4,-%EC%B2%B4%EC%9D%B4%EB%8B%9D,-%EC%82%AC%EC%9A%A9-%EB%AA%A9%EC%A0%81/){: target="_blank"} 형식입니다.

`Predicate<T>` 델리게이트는 <span class="font_highlight">특정 조건에 맞는지를 검사하는 메서드를 캡슐화하는 데 사용되는 델리게이트</span>로, 하나의 매개변수를 갖고 리턴 값이 `bool`인 델리게이트입니다.

```csharp
public delegate bool Predicate<in T>(T obj);
```

### 사용 예제

`Predicate<T>` 델리게이트는 **입력 값이 하나**이고, **특정 조건**에 맞는지 검사하는 메서드를 정의할 때 사용합니다.

이전에 [익명 델리게이트에 대한 글](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/){: target="_blank"}을 읽어보셨다면, delegate로 만들었던 Predicate를 볼 수 있었을 수도 있습니다.
<br>
이전 예제를 가져오면, 아래와 같이 Predicate를 임의로 만들어 정의하고 있었습니다.

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

위 예제에서 임의로 만들었던 `Predicate<T>` 델리게이트를 제거하고, 익명 델리게이트도 람다로 바꿔보겠습니다.

```csharp
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
        var numbers = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
        
        Console.WriteLine(Utilities.Count(numbers, n => n % 2 != 0)); // 5
        Console.WriteLine(Utilities.Count(numbers, n => n % 2 == 0)); // 4
    }
}
```

훨씬 깔끔하고 보기 좋게 만들 수 있었습니다.

<br>
이 `Predicate<T>`는 기존의 컬렉션 매개변수에서도 자주 볼 수 있는 델리게이트입니다.

예를 들어, `List<T>`의 `FindAll()`이라는 메서드도 이 델리게이트를 사용합니다.

```csharp
public List<T> FindAll(Predicate<T> match)
{}
```
```csharp
static void Main(string[] args)
{
  // FindAll()에 람다식을 넘겨 줌
  var result = numbers.FindAll(n => n % 2 == 0);
  Console.WriteLine(string.Join(", ", result)); // 2, 4, 6, 8
}
```
FindAll 외에도 Exists, Find, FindLast, RemoveAll, 등에서도 사용됩니다.

이렇게, 메서드들에서 `Predicate<T>`라는 매개변수를 발견하게 된다면, 특정 조건에 맞는지 검사하는 메서드를 정의하고 이를 넘겨주면 되겠습니다.

---

# 참고

[[delegate] 01.델리게이트의 정의와 정체, 체이닝, 사용 목적](/posts/delegate-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A0%95%EC%B2%B4,-%EC%B2%B4%EC%9D%B4%EB%8B%9D,-%EC%82%AC%EC%9A%A9-%EB%AA%A9%EC%A0%81/){: }

[[delegate] 03. 익명 델리게이트와 Func, Action](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/){: }

[C＃의 람다(Lambda)와 캡처(Capture), 클로저(closure)](/posts/c-%EC%9D%98-%EB%9E%8C%EB%8B%A4(lambda)%EC%99%80-%EC%BA%A1%EC%B2%98(capture),-%ED%81%B4%EB%A1%9C%EC%A0%80(closure)/){: }