---
title: 확장 메서드 (Extension Method)
date: 2024-06-25 15:18:51 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 함수
  - 객체 지향
math: false
type: C#
keywords:
  - 확장 메서드
  - Extension Method
---

확장 메서드는 이미 정의된 클래스에 새로운 메서드를 추가할 수 있게 해주는 기능입니다.
<br>
이 기능은 기존 클래스의 내부 코드를 수정하지 않고 마치 새로운 인스턴스 메서드를 정의하는 것처럼 추가할 수 있습니다.

일반적으로 기존 클래스의 메서드를 확장하는 방법으로는 상속이 있는데, `sealed`로 봉인된 클래스는 상속 받을 수 없고, 새로운 기능으로 확장하기 위해서는 새로운 클래스명을 써야하는데, 마치 기존 클래스의 기능인 것처럼 사용하고 싶을 때가 있습니다.

예를 들어, `sealed` 클래스인 `string`은 상속 받을 수 없습니다.

```csharp
class NewString : String    // sealed 형식 'string'에서 파생될 수 없습니다.
{
}
```

이때, 확장 메서드를 사용하면, 마치 새로운 인스턴스 메서드를 정의하는 것처럼 추가할 수 있습니다.

# 확장 메서드 정의

확장 메서드는 <span class="important">`static` 클래스</span>로 정의되며, <span class="important">확장할 메서드도 `static`</span>이어야 합니다. 또한, <span class="important">**첫 번째 매개변수**는 **확장될 클래스**의 인스턴스를 나타내야 하며, 앞에 `this` 키워드</span>가 붙습니다.

```csharp
static class ExtendString
{
    public static void CustomMethod(this string str)
    {
        Console.WriteLine($"Custom: {str}");
    }
}

internal class Program
{
    static void Main(string[] args)
    {
        "Hello".CustomMethod(); // Custom: Hello
    }
}
```

그렇게 정의된 `static` 메서드는 `this`로 지정된 클래스 인스턴스의 인스턴스 메서드처럼 호출할 수 있습니다.
<br>
(물론, 확장 메서드가 다른 네임스페이스에 정의되어 있다면, 확장 메서드를 사용하는 네임스페이스를 `using` 지시문으로 포함해야 합니다.)

즉, <span class="font_highlight">static 메서드 호출을 인스턴스 메서드를 호출하듯이 문법적인 지원을 해주는 것</span>이 확장 메서드입니다.
<br>
문법적으로 인스턴스 메서드를 호출하는 것처럼 지원하지만, 사실 확장 메서드는 **컴파일 타임에 바인딩**되며, 실제로는 **정적 메서드 호출**로 변환됩니다.

# 정리

1. **정적 클래스와 정적 메서드**: 확장 메서드를 정의하는 클래스는 반드시 `static`이어야 하며, 확장 메서드 자체도 `static`이어야 합니다.
2. **첫 번째 매개변수**: 확장할 클래스의 인스턴스를 첫 번째 매개변수로 받으며, `this` 키워드를 사용해야 합니다.
3. **네임스페이스**: 확장 메서드를 사용하려면 해당 메서드가 정의된 네임스페이스를 `using` 지시문으로 포함해야 합니다.
4. **컴파일 타임**: 확장 메서드는 컴파일 타임에 바인딩되며, 실제로는 정적 메서드 호출로 변환됩니다.