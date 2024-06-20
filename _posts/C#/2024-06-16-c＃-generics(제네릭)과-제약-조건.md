---
title: C＃ Generics(제네릭)과 제약 조건
date: 2024-06-16 17:02:47 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - Generic
    - 유연성
    - 유지보수
    - 재사용성
math: false
type: C#
keywords:
    - Generics
    - 제네릭
    - 제네릭 메서드
    - 제네릭 클래스
    - 제약 조건
    - 제네릭 인터페이스
---

C# 제네릭(Generic)은 **타입 안전성**과 **코드 재사용성**을 극대화하기 위해 도입된 기능입니다.

제네릭을 알아보기 전, 기존 C# 1.0에서는 기본 형식(Primitive types)으로 컬렉션 객체를 사용했습니다.
<br>
예를 들어, `ArrayList`의 경우에는 모든 타입의 컬렉션을 받을 수 있도록 `object`타입을 사용합니다.
<br>
따라서, 모든 기본 타입을 받을 수 있게 설계되었지만, `object`타입은 [참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/#%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85-reference-types){:target="_blank"}이기 때문에 [값 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/#%EA%B0%92-%ED%83%80%EC%9E%85-value-types){:target="_blank"}을 할당 할 경우 [방식/언박싱](/posts/c-%EC%9D%98-%EB%B0%95%EC%8B%B1%EA%B3%BC-%EC%96%B8%EB%B0%95%EC%8B%B1(boxing-and-unboxing)/){: target="_blank"}이 발생합니다.

> 박싱(Boxing)/언박싱(Unboxing)은 값 타입(Value Type)과 참조 타입(Reference Type)간의 변환 과정에서 발생하는 일을 말합니다.
> <br>
> 
> **박싱**은 **값 타입**을 **참조 타입**으로 변환하는 과정에서 힙에 새로운 **메모리 할당**과 **값 복사**가 발생하므로 추가적인 성능 오버헤드가 생깁니다.
> <br>
> **언박싱**은 **참조 타입**을 **값 타입**으로 변환하는 과정에서 **런타임 타입 검사**와 **값 복사**가 필요하므로 추가적인 성능 오버헤드가 있습니다.
> <br>
> 
> 이러한 변환이 빈번하게 발생하면 가비지 컬렉션 부담이 증가하고, 프로그램의 실행 속도가 느려질 수 있습니다. 따라서 박싱과 언박싱은 필요할 때만 사용하고, 가급적 피하는 것이 성능 최적화에 중요합니다.
> <br>
{: .prompt-info}

이 문제를 해결하려면 `ArrayList`가 한가지 타입만을 다루도록 해야 하는데, 이렇게 타입을 고정하면 각 타입마다 `ArrayList`를 구현해야 하는 단점이 생깁니다.

이를 해결하기 위해 도입된 문법이 제네릭(Generic)입니다.

# 제네릭이란?

제네릭(Generic)은 클래스, 인터페이스, 메서드, 델리게이트 등을 정의할 때 <span class="font_highlight">데이터 **타입을 미리 지정하지 않고**, 이를 사용하는 시점에 구체적인 타입을 지정할 수 있도록 하는 기능</span>입니다.

예를 들어, 제네릭 컬렉션 타입 중 하나인 `List<T>`타입을 보면, `T`의 위치에 사용하고자 하는 타입을 지정하면, **컴파일 타임**에 해당 타입에 대한 코드가 생성되어 <span class="font_highlight">박싱/언박싱 없이 원하는 타입에 대한 컬렉션을 사용</span>할 수 있습니다.

```csharp
int n = 10;
List<int> list = new List<int>();
list.Add(n);
```

즉, 제네릭을 사용하면 <span class="font_highlight">데이터 타입에 독립적인 코드를 작성</span>할 수 있으며, 이를 통해 보다 유연하고 효율적인 프로그래밍이 가능합니다.

# 제네릭을 사용하는 이유

1. **코드 재사용성**: 다양한 데이터 타입에 대해 동일한 코드를 작성할 수 있으므로, 코드 중복을 줄이고 유지 보수성을 향상시킵니다.
2. **성능 향상**: 제네릭을 사용하면 박싱(Boxing)과 언박싱(Unboxing)을 피할 수 있어 성능이 향상됩니다. 이는 특히 값 타입(Value Type) 데이터를 다룰 때 유용합니다.
3. **타입 안전성**: 제네릭을 사용하면 컴파일 타임에 데이터 타입을 검증할 수 있습니다. 이는 잘못된 타입 사용으로 인한 런타임 오류를 방지합니다.

# 제네릭 클래스

제네릭 문법은 간단하게 설명하면, <span class="font_highlight">각괄호(`<>`)안에 1개 이상의 타입 매개변수(혹은 형식 매개변수)를 지정하는 것</span>이라고 할 수 있습니다.

제네릭 클래스를 정의할 때는 <span class="font_highlight">클래스 이름 뒤에 타입 매개변수를 지정</span>합니다.

그리고 해당 타입 매개변수를 평범한 타입처럼 사용하면 됩니다.

```text
class 클래스명<타입매개변수[,...]>
{
	// 타입 매개변수를 타입처럼 사용
}
```

예를 들어, `T`라는 타입 매개변수를 사용하는 제네릭 클래스를 정의해보겠습니다.

```csharp
public class GenericList<T>
{
    private T[] items;
    private int count;

    public GenericList(int capacity)
    {
        items = new T[capacity];
        count = 0;
    }

    public void Add(T item)
    {
        if (count < items.Length)
        {
            items[count] = item;
            count++;
        }
    }

    public T GetItem(int index)
    {
        if (index >= 0 && index < count)
        {
            return items[index];
        }
        return default(T);
    }
}
```

위 예에서는 타입 매개변수 1개를 `T`라는 이름으로 주었고, 이 `T`를 배열의 타입, 매개 변수, 리턴 타입으로 사용하는 모습을 보여줍니다.

# 제네릭 메서드

제네릭은 클래스 뿐만 아니라 메서드에도 지정할 수 있습니다.

이를 제네릭 메서드라고 하는데, 제네릭 메서드는 <span class="font_highlight">**메서드 수준**에서 제네릭 타입 매개변수를 사용</span>할 수 있게 합니다.

```text
class 일반_클래스
{
	반환_타입 메서드명<타입매개변수[,...]>(타입명 매개변수명)
	{
		// 타입 매개변수를 타입처럼 사용하여 메서드 정의
	}
}
```

예를 들어, `T`라는 타입 매개변수를 사용하는 제네릭 메서드를 정의해보겠습니다.

```csharp
public static class Utilities
{
    public static T DefaultIfNull<T>(T input, T defaultValue)
    {
        return input == null ? defaultValue : input;
    }
}
```

위 예는 메서드에서 제네릭 타입 `T`를 사용하여 같은 타입의 매개 변수를 받고, `T` 타입의 값을 반환하는 예제입니다.

이때, 제네릭 메서드의 타입 매개변수 `T`는 메서드의 범위 내에서만 유효합니다.

# 제네릭 인터페이스

제네릭 인터페이스도 제네릭 클래스처럼 인터페이스 내부에서 사용 할 <span class="font_highlight">제네릭 타입 매개변수를 인터페이스 이름 뒤에 지정하여 사용</span> 가능합니다.

제네릭 클래스와 사용 방법이 비슷하니 간단하게 예제만 만들어 보겠습니다.

```csharp
public interface IRepository<T>
{
    void Add(T item);
    T Get(int id);
    void Delete(int id);
}
```

# 제네릭 제약 조건

제네릭을 사용할 때, 어떤 사용 환경에서는 제네릭 타입 매개변수가 특정 조건을 만족하도록 만들고 싶을 때가 있습니다.

예를 들어, 다음과 같은 코드를 만든다고 한다면 `CompareTo`에서 문제가 발생하는 것을 볼 수 있습니다.

```csharp
public static class Utilities
{
    public static T Max<T>(T a, T b)
    {
        return a.CompareTo(b) >= 0 ? a : b;
    }
}
```

즉, `T`로 대체될 타입이 모두 `CompareTo` 메서드를 지원하는 것이 아니기에 미리 오류를 발생시켜 잘못된 사용을 막는 것입니다.

이때, <span class="font_highlight">where를 사용하여 제네릭 타입 매개변수에 **특정 제약 조건**을 부여</span>할 수 있습니다.

이를 통해 제네릭 타입이 특정 인터페이스를 구현하거나 특정 기본 클래스를 상속받도록 강제할 수 있습니다.

```text
where 타입매개변수 : 제약조건[,...]
```

`where`는 제약 조건을 걸고 싶은 타입 매개 변수마다 사용할 수 있으며, <span class="font_highlight">1개 이상의 제약 조건을 사용</span>할 수 있습니다.

예를 들어, 2개의 타입 매개 변수에 제약조건을 걸고 싶다면 다음과 같이 쓸 수 있습니다.

```csharp
public class Dict<K, V> where K : ICollection
                        where V : IComparable
```

따라서, 처음의 예제는 `CompareTo` 메서드를 구현하도록 강제하는 `IComparable<T>` 인터페이스를 구현하도록 타입 매개변수 `T`에 제약 조건을 추가할 수 있습니다.

```csharp
public static class Utilities
{
    public static T Max<T>(T a, T b) where T : IComparable<T>
    {
        return a.CompareTo(b) >= 0 ? a : b;
    }
}
```

## 제네릭 타입 매개 변수에 대한 특별 제약 조건

| 제약 조건                    | 설명                                                                                            |
| ------------------------ | --------------------------------------------------------------------------------------------- |
| `where T : struct`       | 타입 매개변수 `T`는 **값 타입**만 가능합니다. 여기에는 모든 기본 값 타입(int, double 등)과 사용자 정의 구조체(struct)가 포함됩니다.      |
| `where T : class`        | 타입 매개변수 `T`는 **참조 타입**만 가능합니다. 여기에는 모든 클래스, 인터페이스, 델리게이트, 배열 등이 포함됩니다.                        |
| `where T : new()`        | 타입 매개변수 `T`는 매개변수가 없는 **기본 생성자**를 가져야 합니다.<br>제네릭 클래스 또는 메서드 내에서 `T`의 인스턴스를 생성할 때 유용합니다.      |
| `where T : 특정 클래스/인터페이스` | 타입 매개변수 `T`는 **특정 클래스**의 파생 클래스이거나, **특정 인터페이스**를 구현해야 합니다. 이를 통해 특정 기능이나 멤버를 사용할 수 있게 보장합니다. |
| `where T : U`            | 타입 매개변수 `T`는 다른 타입 매개변수 `U`의 **하위 타입**이어야 합니다. 여기서 `U`는 클래스, 인터페이스 또는 다른 제네릭 타입 매개변수일 수 있습니다. |

여기에서 이해하기 어려울 수도 있는 두 가지 경우에 대해서 설명드리겠습니다.

우선, `where T : new()`의 경우에는 `T` 타입의 객체가 기본 생성자를 가져야만 합니다.

```csharp
public static T AllocateIfNull<T>(T item) where T : class
{
	if (item == null)
	{
		item = new T();
	}
	return item;
}
```

위 코드는 `where T : class` 제약조건을 통해 참조 타입만 가져오기 때문에 얼핏 보면 `new T()`를 통해 객체를 생성하고 반환하는 동작에 문제가 없어 보이기도 합니다.

그러나, 과연 모든 타입이 **기본 생성자**를 가지고 있다고 장담할 수 없습니다.

따라서, `new()` 제약조건을 통해 기본 생성자를 가진 타입만 정의가 가능하다고 제약 조건을 걸 수 있습니다.

`where T : U`의 경우에는 조금 어려워 보일 수도 있지만, `where T : 특정 클래스/인터페이스`와 비슷하다고 생각하시면 됩니다.

```csharp
public class BaseClass
{}
public class DerivedClass : BaseClass
{}

public static class Utilities
{
	// where T : U 제약 조건 제네릭 함수
    public static B Allocate<B, D>() where D : B, new()
    {
        return new D();
    }
}

internal class Program
{
    static void Main(string[] args)
    {
        BaseClass myClass = Utilities.Allocate<BaseClass, DerivedClass>();
    }
}
```

위의 예제의 경우, 다형성을 이용한 인스턴스 할당 예제입니다.

`where D : B`의 경우 `D`가 `B`의 하위 타입인 경우에만 오게하여 `D` 객체를 생성하고, 이를 `B` 타입으로 반환해 줍니다.

간단히 말하면, 하위 타입만 올 수 있도록 제약 조건을 걸 수 있습니다.

# 정리

C# 제네릭(Generic)은 **타입 안전성**과 **코드 재사용성**을 극대화하여, 더욱 강력하고 유연한 프로그래밍을 가능하게 합니다.

제네릭을 통해 타입에 독립적인 코드를 작성할 수 있으며, 컴파일 타임에 타입을 검증함으로써 런타임 오류를 줄일 수 있습니다. 또한, 박싱과 언박싱을 피할 수 있어 성능 면에서도 이점을 제공합니다.

제네릭 클래스, 제네릭 메서드, 제네릭 인터페이스를 적절히 활용하면 코드의 유지 보수성과 가독성을 크게 향상시킬 수 있습니다. 제네릭 제약 조건을 통해 타입 매개변수에 특정 조건을 부여할 수 있어, 더욱 안전하고 효율적인 코드를 작성할 수 있습니다.

---

# 참고

[C＃의 값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: }

[C＃의 박싱과 언박싱(Boxing and Unboxing)](/posts/c-%EC%9D%98-%EB%B0%95%EC%8B%B1%EA%B3%BC-%EC%96%B8%EB%B0%95%EC%8B%B1(boxing-and-unboxing)/){: }