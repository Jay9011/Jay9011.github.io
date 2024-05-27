---
title: "[OOP] 다형성(Polymorphism)"
date: 2024-01-12 20:04:02 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 객체 지향
  - 소프트웨어 공학
  - 유지보수
  - 재사용성
  - 확장성
  - 다형성
math: false
type: SoftwareEngineering
keywords:
  - Overloading
  - Overriding
  - Polymorphism
  - 객체지향
  - 다형성
  - 오버라이딩
  - 오버로딩
  - 시그니처
---

다형성(Polymorphism)은 객체지향 프로그래밍(OOP)의 핵심적인 개념 중 하나로, <span class="font_highlight">'**여러 형태**(Poly 많은, morph 형태)'를 가질 수 있는 특성</span>을 말합니다.

이는 <span class="font_highlight">**하나의 객체**가 **다양한 타입**으로 행동</span>할 수 있도록 하며, 이로 인해 코드의 **재사용성**과 **확장성**이 증가하고 **유지보수**가 용이해집니다.

# 타입의 다형성

타입의 다형성은 <span class="font_highlight">**한 객체**가 **여러 타입**의 역할을 수행할 수 있는 능력</span>을 말합니다.

즉, 하나의 클래스가 상속 받은 **부모 클래스 타입**으로 취급 될 수 있고, 구현한 **인터페이스 타입**으로 취급 될 수도 있습니다.

따라서, 한 객체가 여러 타입의 기능을 제공하며, 코드를 더 유연하게 만들고 다양한 환경에 적용 가능하게 합니다.

```c#
public class Animal
{
    public virtual void Move()
    {
        Console.WriteLine("This animal moves in some way.");
    }
}
public interface IFlyable
{
    void Fly();
}
// Bird 클래스는 Animal을 상속받고, IFlyable 인터페이스 구현 함
public class Bird : Animal, IFlyable
{
    // 부모 메서드 Overriding
    public override void Move()
    {
        Console.WriteLine("Flaps its wings.");
    }
    // IFlyable의 Fly 메서드 구현
    public void Fly()
    {
        Console.WriteLine("The bird flies in the sky.");
    }
}
public class Program
{
    public static void Main()
    {
        Bird myBird = new Bird();
        // Animal 타입으로 변환하여 Move 메서드 호출
        Animal animal = myBird;
        animal.Move();
        // IFlyable 타입으로 변환하여 Fly 메서드 호출
        IFlyable flyer = myBird;
        flyer.Fly();
    }
}
```

위 예제에서 `Bird` 클래스는 `Animal` 클래스를 상속받고 `IFlyable` 인터페이스를 구현하여, `Animal`과 `IFlyable`의 기능을 모두 사용할 수 있습니다.

이는 다형성을 통해 다양한 타입의 기능을 하나의 객체에서 구현할 수 있음을 보여줍니다.

# 기능의 다형성

기능의 다형성에는 메서드의 오버라이딩과 오버로딩이 있습니다.

기 두 기능을 헷갈려 하시는 분이 많은데, `Over`를 제외하고 뒤의 문자만 기억하면 구분하기 쉽습니다.

`loading`은 부를 때, `riding`은 어떤 것을 타는 것이므로, `Overloading`은 같은 이름의 메서드를 `부를 때` 여러 형태로 부르는 것이고, `Overriding`은 부모 클래스의 메서드를 타고(?) 재정의 하는 것이라고 생각하면 좀 더 구분하기 쉽지 않을까요?

## 오버라이딩 (Overriding)

오버라이딩은 <span class="font_highlight">자식 클래스가 부모 클래스에서 **상속받은 메서드를 재정의하는 것**</span>을 의미합니다.

이를 통해 자식 클래스는 부모 클래스의 메서드를 자신의 필요에 맞게 수정하여 사용할 수 있습니다.

메서드 오버라이딩은 <span class="keyword">**동적 다형성**</span>의 예로, <span class="font_highlight">**실행 시간(runtime)**에 **객체의 실제 타입**에 따라 적절한 메서드가 호출</span>됩니다.

이는 같은 메서드 호출에 대해 다른 행동을 할 수 있도록 합니다.

## 오버로딩 (Overloading)

같은 클래스 내에서 **같은 이름**의 메서드를 메서드의 **시그니처를 다르게** 하여 **여러 개 정의**하는 것을 말합니다.

메서드의 <span class="important">**시그니처(Method Signature)**</span>란, 프로그래밍 언어에서 메서드를 고유하게 식별할 수 있는 정보를 제공하는 요소입니다.
<br>
메서드의 시그니처는 메서드의 이름과 파라미터 목록을 포함하며, 언어에 따라 리턴 타입까지 시그니처에 포함될 수 있습니다.

즉, 메서드 오버로딩이란, <span class="font_highlight">같은 클래스 내에서 **같은 이름**의 메서드를 **파라미터 목록(매개변수의 타입, 매개변수의 개수, 등...)을 다르게** 하여 **여러 개 정의**하는 것</span>입니다.

메서드 오버로딩은 컴파일 시간에 결정되는 <span class="keyword">**정적 다형성**</span>의 예로, <span class="font_highlight">**컴파일 타임**에 메서드 이름과 시그니처가 같은 메서드를 호출</span>하도록 결정합니다.

# 다형성의 장점

- 코드 <span class="important">**재사용성**</span> 증가: 다형성을 통해 기존 클래스를 **상속**받아 새로운 기능을 추가하거나 변경할 수 있어, 비슷한 기능을 하는 여러 클래스를 중복해서 작성할 필요가 줄어듭니다.
- <span class="important">**유지보수**</span> 용이: 코드를 **일반화**하여 다양한 객체에서 사용할 수 있기 때문에, 수정이 필요할 경우 관련된 모든 객체에 대해 일괄적으로 수정할 수 있어 유지보수가 용이합니다.
- <span class="important">**확장성**</span> 향상: 새로운 클래스나 기능을 추가하고자 할 때 기존 코드를 크게 변경하지 않고 확장할 수 있어, 시스템의 유연성과 확장 가능성이 크게 향상됩니다.
- <span class="important">**동적 바인딩**</span>: 실행 시간에 메서드를 동적으로 바인딩함으로써, 실제 객체의 타입에 따라 그에 맞는 메서드가 호출됩니다. 이는 프로그램의 **유연성**을 더욱 증가시키고, 객체의 실제 타입을 **추상화**하는 데 도움을 줍니다.



---
# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[[OOP] 캡슐화 (Encapsulation)](/posts/oop-%EC%BA%A1%EC%8A%90%ED%99%94(encapsulation)/){:  }

[[OOP] 상속성(Inheritance)](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){:  }

[[OOP] 추상화(Abstraction)](/posts/oop-%EC%B6%94%EC%83%81%ED%99%94(abstraction)/){:  }