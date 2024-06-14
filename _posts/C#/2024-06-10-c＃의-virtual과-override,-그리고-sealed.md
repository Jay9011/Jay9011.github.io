---
title: C＃의 virtual과 override, 그리고 sealed
date: 2024-06-10 17:31:58 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 다형성
    - 함수
    - 객체 지향
math: false
type: C#
keywords:
    - override
    - polymorphism
    - sealed
    - virtual
    - 가상 메서드
    - 가상 함수
    - 다형성
    - 상속
    - 재정의
---

C#의 `virtual`과 `override` 키워드는 [상속](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: target="_blank"}과 [다형성(polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: target="_blank"}을 구현하는데 핵심적인 역할을 합니다.

이 두 키워드를 사용하면 <span class="font_highlight">기본 클래스의 메서드를 파생 클래스에서 **재정의**</span>할 수 있으며, 이를 통해 코드의 **유연성**과 **재사용성**을 높일 수 있습니다.


# 다형성(Polymorphism)

다형성은 같은 인터페이스를 통해 서로 다른 데이터 타입의 객체를 동일하게 다룰 수 있는 [객체지향(OOP)](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: target="_blank"} 프로그래밍의 특성입니다.

C#에서는 가상 함수와 메서드 재정의를 통해 다형성을 구현할 수 있습니다.
<br>
그 중 가상 함수는 `virtual`과 `override` 키워드로 구현할 수 있습니다.

```csharp
public static void Main(string[] args)
{
    Animal myAnimal = new Animal();
    myAnimal.Speak(); // Animal에 정의된 Speak()를 사용
    // 출력: The animal makes a sound

    Dog myDog = new Dog();
    myDog.Speak(); // 같은 메서드명(Speak())을 사용함에도 다른 결과가 출력
    // 출력: The dog barks

    Animal myPolymorphicDog = new Dog(); // 심지어 Animal 타입으로 객체를 다루는데에도, 원래 객체는 Dog였으므로
    myPolymorphicDog.Speak();            // Dog에 대한 Speak()를 사용함.
    // 출력: The dog barks
}
```
위의 예제에서 `myPolymorphicDog`는 `Animal` 타입이지만 `Dog` 객체를 참조합니다.
<br>
이 경우 `Speak()` 메서드를 호출하면, 실제 객체 타입인 `Dog`의 `Speak()` 메서드가 실행됩니다.

이는 다형성의 한 예로, 메서드를 오버라이딩(Overriding)하여 실행 시간에 객체의 실제 타입에 따라 적절한 메서드가 호출되는 경우입니다.


# 가상 함수(Virtual Function)와 virtual 키워드

<span class="keyword">**가상 함수**</span>는 **상속 관계**에 있는 클래스들에서 <span class="font_highlight">기본 클래스의 메서드를 파생 클래스에서 **재정의할 수 있도록** 하는 메서드</span>입니다.

C#에서는 가상 함수를 정의하기 위해 `virtual` 키워드를 사용합니다.
<br>
기본 클래스에서 가상 함수로 선언된 메서드는 파생 클래스에서 `override` 키워드를 사용하여 재정의할 수 있습니다.

```csharp
public class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("The animal makes a sound");
    }
}
```

위의 코드에서 `Speak` 메서드는 `virtual`로 선언되어 있으며, 이는 이 메서드가 파생 클래스에서 재정의될 수 있음을 의미합니다.

# 메서드 재정의(Overriding)와 override 키워드

<span class="keyword">**오버라이딩(Overriding)**</span>은 <span class="font_highlight">파생 클래스가 기본 클래스의 가상 함수를 **재정의하는 것**</span>을 말합니다.

이를 통해 파생 클래스에서 기본 클래스의 메서드를 새로운 동작으로 대체할 수 있습니다. 이를 위해 메서드 앞에 `override` 키워드를 사용합니다.

```csharp
public class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("The dog barks");
    }
}
```

여기서 `Dog` 클래스는 `Animal` 클래스를 상속받으며, `Speak` 메서드를 `override` 키워드를 사용하여 재정의합니다.

# 가상 함수와 재정의의 규칙

1. **상속 관계**: 가상 함수와 재정의는 반드시 상속 관계에 있는 클래스들 간에 이루어져야 합니다.
2. **메서드 시그니처 일치**: 재정의되는 메서드는 기본 클래스의 메서드와 동일한 **시그니처(메서드 이름, 매개변수 목록, 반환 타입)**를 가져야 합니다.
3. **접근 제한자**: 재정의되는 메서드의 접근 제한자는 기본 클래스의 메서드와 **동일**하거나 **더 넓은** 접근 수준을 가져야 합니다.
4. **기본 클래스의 선언**: 기본 클래스의 메서드는 `virtual` 키워드로 선언되어야 하며, 파생 클래스의 메서드는 `override` 키워드를 사용하여 재정의되어야 합니다.

# sealed 키워드

## 메서드 재정의 방지

`sealed` 키워드를 메서드에 사용하면 그 메서드가 <span class="font_highlight">더 이상 파생 클래스에서 **재정의되지 않도록** 할 수 있습니다</span>.

`sealed` 메서드는 반드시 `override` 키워드와 함께 사용해야 합니다.

이는 재정의된 메서드를 다음부터 재정의 하지 못하도록 하는 키워드이기 때문입니다.

```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal sound");
    }
}
class Dog : Animal
{
    public sealed override void Speak() // sealed 사용
    {
        Console.WriteLine("Bark");
    }
}
class Bulldog : Dog
{
    // 더 이상 오버라이딩이 불가능하다.
    // public override void Speak() { Console.WriteLine("Bulldog Bark"); }
}
```
위 예제에서 `Dog` 클래스의 `Speak()` 메서드는 `sealed`로 선언되었기 때문에 `Bulldog` 클래스에서 더 이상 재정의할 수 없습니다.

## 클래스 상속 방지

`sealed` 키워드의 또 다른 사용법은 클래스의 상속을 막는 방법으로 사용하는 것입니다.

`sealed` 키워드를 클래스에 사용하면 <span class="font_highlight">해당 클래스를 다른 클래스가 **상속받지 못하게** 합니다</span>.

```csharp
sealed class SealedClass
{
    public void Display()
    {
        Console.WriteLine("This is a sealed class.");
    }
}

// 더 이상 상속 받지 못함
// class DerivedClass : SealedClass { }
```

---

# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](https://jay9011.github.io/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[[OOP] 상속성(Inheritance)](https://jay9011.github.io/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: }

[[OOP] 다형성(Polymorphism)](https://jay9011.github.io/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }

