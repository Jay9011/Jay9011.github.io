---
title: 인터페이스(Interface)와 추상 클래스(Abstract Class)
date: 2024-06-19 16:54:54 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 다형성
  - 유연성
  - 인터페이스
  - 재사용성
  - 추상 클래스
  - 추상화
  - 객체 지향
math: false
type: C#
keywords:
  - Interface
  - 인터페이스
  - 추상 클래스
  - Abstract Class
---

# 인터페이스(Interface)

**인터페이스**는 [객체 지향 프로그래밍](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: target="_blank"}에서 중요한 개념으로, <span class="font_highlight">메서드 시그니처만을 정의</span>하고, <span class="font_highlight">구현은 제공하지 않는</span> 일종의 **청사진**입니다.

인터페이스는 주로 <span class="font_highlight">[다형성](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: target="_blank"}을 구현하고, 클래스 간의 **결합도를 낮추는**데 목적</span>이 있습니다. 그로 인해 <span class="font_highlight">**서로 다른 클래스**들이 **동일한 인터페이스**를 구현함으로써 **상호 작용**</span> 할 수 있게 합니다.

즉, 인터페이스를 잘 활용하면 코드의 유연성과 재사용성을 극대화할 수 있게 됩니다.

## 사용 법

인터페이스는 `interface` 키워드를 사용하여 선언됩니다.

인터페이스는 접근 지정자를 포함할 수 있으며, 그 내부에는 구현되지 않은 메서드, 속성, 이벤트, 인덱서 등의 멤버만 선언할 수 있습니다.

또한, 클래스나 구조체 이름 뒤에서 `:` 기호를 사용하여 인터페이스를 구현할 수 있습니다. 이때, 인터페이스를 구현하는 클래스는 인터페이스에 정의된 모든 멤버를 구현해야 합니다.

그리고 클래스는 여러 개의 인터페이스를 동시에 구현할 수도 있습니다.

```csharp
public interface IMyInterface
{
    void MyMethod();
    string MyProperty { get; set; }
    delegate void MyDelegate();
    event MyDelegate MyEvent;
    int this[int index] { get; set; }
}

public class MyoncreteClass : IMyInterface
{
    public void MyMethod()
    {
        // 메서드 구현
    }
    public string MyProperty { get; set; }
    public event IMyInterface.MyDelegate MyEvent;
    public int this[int index]
    {
        // 인덱서 구현
    }
}
```

이렇게 인터페이스를 구현하면 **다형성**을 구현할 수 있습니다.

클래스에서 상속을 통해 다형성을 구현하는 것 처럼, 인터페이스 타입의 변수를 통해 인터페이스를 구현한 객체를 참조할 수 있습니다.

또한, 인터페이스도 다른 인터페이스를 상속할 수 있습니다. 이 경우 하위 인터페이스는 상위 인터페이스의 모든 멤버를 상속받습니다.

# 추상 클래스(Abstract Class)

추상 클래스는 <span class="font_highlight">하나 이상의 **추상 메서드**를 포함하는 클래스</span>입니다.

(추상 메서드는 메서드의 시그니처(이름, 반환 타입, 매개변수)만 정의되고, 본체가 구현되지 않은 메서드입니다.)

따라서, 추상 클래스는 <span class="font_highlight">직접 인스턴스를 생성할 수 없으며</span>, 이를 상속받은 하위 클래스에서 구현되어야 합니다.

(추상 메서드는 구현이 존재하지 않으므로, 추상 클래스 자체로 직접 인스턴스를 생성하게 된다면, 구현이 없는 메서드를 호출할 때 오류를 발생시켜야 할 것입니다.)

**인터페이스**는 다형성을 구현하고, 클래스 간의 결합도를 낮추는데 목적이 있습니다만, **추상 클래스**는 <span class="font_highlight">공통적인 기능을 여러 클래스에 제공하면서도, 일부 메서드는 하위 클래스에서 반드시 구현하도록 강제할 때 유용</span>합니다.

따라서, 추상 클래스는 인터페이스와 달리, 필드(멤버 변수), 속성, 메서드(구현된 메서드 및 추상 메서드), 이벤트 등 다양한 멤버를 포함할 수 있습니다.

## 사용법

추상 클래스는 `abstract` 키워드를 사용하여 선언합니다.

다양한 멤버를 구성할 수 있으며, 하나 이상의 추상 메서드를 포함합니다.

추상 클래스만으로 인스턴스를 생성할 수 없으므로, 하위 클래스에서 상속 받아 사용해야 합니다.

상속은 클래스 이름 뒤에서 `:` 기호를 사용하여 상속 받을 수 있습니다.

```csharp
public abstract class Animal
{
    // 멤버 변수(필드)
    private string name;
    // 속성
    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    // 추상 메서드
    public abstract void MakeSound();

    // 일반 메서드
    public void Sleep()
    {
        Console.WriteLine($"{Name} is sleeping...");
    }
}
```

# 추상 클래스와 인터페이스의 비교

| **특징**          | **인터페이스**                                 | **추상 클래스**                          |
|-------------------|-------------------------------------------------|-----------------------------------------|
| **정의**          | 모든 메서드가 추상 메서드로 선언되며, 구현은 제공하지 않음  | 일부 메서드만 구현되고 추상 메서드는 하위 클래스에서 구현해야 하는 클래스 |
| **선언 키워드**   | `interface`                                      | `abstract class`                        |
| **접근 제어자**   | 멤버는 항상 `public`                              | 모든 접근 지정자 사용 가능               |
| **메서드 구현 여부** | 구현을 제공하지 않음                            | 구현된 메서드와 추상 메서드 모두 포함 가능 |
| **생성자**        | 포함 불가                                         | 생성자를 가질 수 있음                     |
| **필드**          | 포함 불가                                         | 필드를 가질 수 있음                       |
| **인스턴스화**    | 불가능                                            | 불가능                                   |
| **다중 상속**     | 다중 상속 가능                                    | 단일 상속만 가능                          |
| **목적**          | 다형성과 유연성을 높이는 데 사용                  | 공통된 기본 동작 제공 및 하위 클래스의 구체적 동작 정의  |

---

# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[[OOP] 다형성(Polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }