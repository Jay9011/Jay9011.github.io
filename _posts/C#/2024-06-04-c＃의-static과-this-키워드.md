---
title: C＃의 Static과 this 키워드
date: 2024-06-04 19:43:58 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 정적
    - 함수
math: false
type: C#
keywords:
    - static
    - this
---

C#에서 `static` 키워드는 특정 클래스, 메서드, 필드, 속성, 생성자, 또는 다른 멤버가 <span class="font_highlight">클래스 수준에서 정의되도록 하기 위해 사용</span>됩니다.

즉, `static` 멤버가 클래스의 특정 인스턴스에 속하지 않으며, <span class="font_highlight">클래스 자체에 속하게 하여 **인스턴스 없이 사용** 가능</span>하도록 합니다.

# Static Class (정적 클래스)

Static Class는 클래스 수준에서 다룰 멤버들을 정의하기 위한 클래스. 즉, <span class="font_highlight">**인스턴스 없이 사용**하기 위한 클래스를 정의</span>합니다.
<br>
따라서, 인스턴스화할 수 없으며, 오직 `static` 멤버만 가질 수 있습니다.

주로 유틸리티 클래스나 확장 메서드 집합을 정의할 때 사용됩니다.

```csharp
public static class MathUtilities
{
    public static int Add(int a, int b)
    {
        return a + b;
    }
}
static void Main(string[] args)
{
    // MathUtilities util = new MathUtilities();  // 정적 클래스는 인스턴스화 불가.
}
```

또한, 정적 클래스의 멤버는 **클래스 이름을 통해 직접 접근**합니다.

```csharp
int result = MathUtilities.Add(5, 3);
```

# Static Field (정적 필드)

정적 필드는 <span class="font_highlight">클래스의 모든 인스턴스가 공유</span>하는 단일 변수입니다.

주로 클래스 수준에서 공유되어야 하는 데이터를 저장할 때 사용됩니다.

```csharp
public class StaticClass
{
    public static int counter = 0;
}
```

정적 필드 또한 멤버는 **클래스 이름을 통해 직접 접근**합니다.

```csharp
int counter = StaticClass.counter;
```

# Static Method (정적 메서드)

정적 메서드는 클래스의 인스턴스가 아닌 <span class="font_highlight">클래스 자체에 속하는 메서드</span>입니다.
<br>
클래스의 특정 인스턴스와 관련이 없는 작업을 수행하거나 정적 멤버에 대한 동작을 정의할 때 사용됩니다.

이때, 인스턴스와 함께 쓰이게 될 때 주의 깊게 봐야하는게 있습니다.

```csharp
public class StaticClass
{
    public static int counter = 0;
    public string Name;

    public static void IncrementCounter()
    {
        counter++;
        // Name = counter;   // Name은 인스턴스의 멤버이기 때문에 정적 메서드에서 사용할 수 없다.
    }

    public void Print()
    {
        Console.WriteLine($"Name: {Name}, counter: {counter}");
    }
}
```

위 예제처럼, 클래스에 **정적 멤버**와 **인스턴스 멤버**(static이 아닌 멤버)가 존재할 경우, Static Method는 인스턴스 멤버에 접근할 수 없습니다.

메모리를 보면 정적 멤버와 인스턴스 멤버는 다음과 같이 존재하게 됩니다.

![정적 멤버](https://i.postimg.cc/d0WtFvj1/static.webp){: w="500" h="259"}

즉, 각 인스턴스는 서로 독립적인 인스턴스 멤버를 가지고 있고, 정적 멤버는 클래스 단위로 존재하게 됩니다.

따라서, Static Method는 인스턴스 멤버를 알 수 없기도 하고, 인스턴스 없이 호출되어야 하므로 인스턴스 멤버를 사용할 수 없습니다.

다만, 인스턴스 메서드에서는 클래스 단위로 접근 할수도 있기 때문에 정적 멤버에 접근할 수 있습니다.

## this 키워드

`this` 키워드는 <span class="font_highlight">**클래스의 인스턴스**를 참조</span>하는 데 사용되는 특수한 참조 변수입니다.

인스턴스 메서드 내에서, `this`는 현재 메서드를 호출한 객체를 가리킵니다. 이를 통해 인스턴스의 멤버(필드, 속성, 메서드 등)에 접근할 수 있습니다.

## 정적 메서드와 this 키워드의 관계

정적 메서드는 클래스의 **인스턴스가 없어도 호출되어야 하기 때문에**, 정적 메서드 내에서는 `this` 키워드를 사용할 수 없습니다.

따라서, `this`는 오직 인스턴스 메서드 내에서만 유효합니다.

```csharp
public class StaticClass
{
    public static int counter = 0;
    public string Name;

    public static void IncrementCounter()
    {
        counter++;
        // this.Name = "Name" // this 키워드를 사용할 수 없다.
    }

    public void Print()
    {
        this.Name = "Name"; // this 키워드를 사용하여 인스턴스 멤버에 접근할 수 있다.
        Console.WriteLine($"Name: {Name}, counter: {counter}"); // static 멤버에도 접근 가능
    }
}
```

위 예제에서 `Print()`는 인스턴스 메서드이기 때문에 `this` 키워드를 사용하여 인스턴스 필드와 정적 필드 모두 접근할 수 있습니다.

반면, `IncrementCounter()`는 정적 메서드이기 때문에 `this` 키워드를 사용할 수 없고, 오직 정적 필드에만 접근할 수 있습니다.

# Static Constructor (정적 생성자)

정적 생성자는 <span class="font_highlight">클래스가 처음으로 로드될 때 실행되는 생성자</span>입니다.

여기에서 로드라는 표현을 쓴 이유는 '정적 멤버를 처음 호출한 경우'나 '인스턴스 생성자를 통해 객체를 만들기 위해 클래스가 로드 된 경우' 등에 그 어떤 생성자보다 먼저 실행되는 생성자이기 때문입니다.

**매개변수를 가질 수 없고**, **직접 호출될 수도 없습니다**.

주로 클래스 수준의 초기화를 수행할 때 사용되며, <span class="important">**딱 한 번 호출**</span>됩니다.

```csharp
public class StaticClass
{
    public static int counter = 0;

	// 정적 생성자
    static StaticClass()
    {
        Console.WriteLine("Static constructor");
    }
}

static void Main(string[] args)
{
    int counter = StaticClass.counter;  // 출력: Static constructor
    StaticClass C1 = new StaticClass(); // 출력 없음
}
```

# 요약

- **정적 클래스**는 인스턴스화할 수 없으며, 오직 정적 멤버만 가질 수 있습니다.
- **정적 필드**는 클래스의 모든 인스턴스가 공유하는 단일 변수입니다.
- **정적 메서드**는 특정 인스턴스가 아닌 클래스 자체에 속하는 메서드입니다.
	- 정적 메서드에서 인스턴스 멤버에 접근할 수 없고, `this` 키워드도 사용할 수 없습니다.
- **정적 생성자**는 클래스가 처음으로 로드될 때 실행되는 생성자입니다.

---

# 참고

[클래스(Class)와 백킹 필드(backing field), 그리고 base와 this](/posts/%ED%81%B4%EB%9E%98%EC%8A%A4(class)%EC%99%80-%EB%B0%B1%ED%82%B9-%ED%95%84%EB%93%9C(backing-field),-%EA%B7%B8%EB%A6%AC%EA%B3%A0-base%EC%99%80-this/){: }