---
title: object 타입과 var 키워드와의 비교
date: 2024-06-01 17:51:07 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 객체 지향
    - 다형성
    - 데이터
    - 변수
    - 유연성
    - 자료형
math: false
type: C#
keywords:
    - object
    - var
    - 다형성
    - 유연성
---

C#은 강력한 정적 타입 언어로, 변수의 타입을 엄격하게 정의합니다. 그러나 코드의 유연성과 간결성을 높이기 위해 `object`와 `var` 키워드를 제공합니다.

이 두 키워드는 얼핏 보면 다양한 타입을 추상적으로 사용한다는 점에서 비슷해 보일 수 있으나, 실제로는 서로 다른 목적과 사용 방법을 가지고 있습니다.

# `object` 키워드

`object`는 C#의 <span class="font_highlight">**모든 타입**의 최상위 **기본 클래스**</span>입니다.

모든 데이터 타입은 `object`로부터 [상속](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: target="_blank"}받으며, 이를 통해 다양한 데이터 타입을 유연하게 다룰 수 있습니다.

```csharp
object number = 10;    // number는 실제로 int 타입의 값을 가리키는 최상위 클래스 object 입니다.
object text = "Hello"; // text는 실제로 string 타입의 값을 가리키는 최상위 클래스 object 입니다.
```

- **유연성**: `object` 타입을 사용하면 하나의 변수에 **다양한 타입**의 값을 할당할 수 있습니다.
- **다형성**: `object` 타입은 다형성을 지원하여, 메소드의 매개변수나 리턴 타입으로 사용할 수 있습니다.

즉, 상속을 통해 다양한 타입을 담을 수 있도록 만든 `class`이기 때문에 **[다형성](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: target="_blank"}**을 사용할 수 있습니다.

예를 들어, 두 객체가 같은지 여부를 확인하는 `Equals(object obj)`라던가, 객체를 문자열로 반환하는 `ToString()` 등을 오버라이딩해 사용할 수 있습니다.

```csharp
class MyClass
{
    public int Value;
    public string Name;

	// Value만 비교해서 같으면 True를 리턴하도록 한다.
    public override bool Equals(object? obj)
    {
        if (obj == null || GetType() != obj.GetType())
        {
            return false;
        }

        return this.Value == ((MyClass)obj).Value;
    }
}

internal class Program
{
    static void Main(string[] args)
    {
        MyClass myClass1 = new MyClass();
        myClass1.Value = 1;
        myClass1.Name = "MyClass1";

        MyClass myClass2 = new MyClass();
        myClass2.Value = 1;
        myClass2.Name = "MyClass2";

        Console.WriteLine(myClass1.Equals(myClass2)); // True
    }
}
```

# `object` 타입과 `var` 키워드의 차이

`object`와 `var`는 서로 다른 용도로 사용되며, 여러 가지 차이점이 있습니다.

`var` 키워드 컴파일 시점에 초기화 표현식의 타입을 기반으로 컴파일러가 변수의 타입을 추론하는 타입 추론 키워드입니다.

반면, `object`는 모든 타입의 최상위 기본 클래스로, 변수에 어떤 타입의 값이라도 할당할 수 있으며, 추상화 하여 사용할 수 있습니다.

<span class="small">[var 키워드 글](/posts/var-%EC%A0%95%EC%A0%81-%ED%83%80%EC%9D%B4%ED%95%91-%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-%ED%82%A4%EC%9B%8C%EB%93%9C/){: target="_blank"} 보러가기</span>

## 타입 추론과 상속

### 타입 추론 (var)

`var` 키워드는 변수 선언 시 <span class="font_highlight">**컴파일러**가 **변수의 타입을 자동으로 추론**</span>합니다.

이 변수는 초기화 시점에 타입이 명확해야 하며, 한 번 결정된 타입은 변경할 수 없습니다.

```csharp
var number = 10;  // 컴파일러가 number의 타입을 int로 추론
var text = "Hello"; // 컴파일러가 text의 타입을 string으로 추론
```

- **정적 타이핑**: `var`는 여전히 정적 타입으로, 변수의 타입이 컴파일 시점에 결정되며, 이후 변경할 수 없습니다.
- **간결성**: 코드의 가독성을 높이고, 반복적인 타입 선언을 줄여줍니다.
- **제한사항**: `var`는 초기화 시점에서 타입이 명확해야 하며, 재할당 시 동일 타입의 값만 사용할 수 있습니다.

### 상속 (object)

`object` 타입은 <span class="font_highlight">모든 타입의 최상위 클래스</span>입니다.

이는 모든 데이터 타입이 `object`로부터 상속받기 때문에, `object` 변수에 어떤 타입의 값이라도 할당할 수 있습니다.

다만, [값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: target="_blank"}의 형태에 따라 메모리의 변수를 복사하는 등의 [박싱과 언박싱](/posts/c-%EC%9D%98-%EB%B0%95%EC%8B%B1%EA%B3%BC-%EC%96%B8%EB%B0%95%EC%8B%B1(boxing-and-unboxing)/){: target="_blank"}이 발생할 수 있습니다.
<br>
(박싱과 언박싱이 발생하면 성능적인 오버헤드가 발생할 수 있으므로, 불필요한 박싱과 언박싱은 피하는 것이 좋습니다.)

```csharp
object number = 10;    // int 값 할당
object text = "Hello"; // string 값 할당
```

- **유연성**: 다양한 타입을 하나의 변수로 다룰 수 있습니다.
- **다형성**: 메소드 인자나 리턴 타입으로 사용할 때 유용합니다.
- **박싱과 언박싱**: 값 타입을 `object`에 할당하면 박싱이 발생하고, 다시 언박싱해야 실제 타입으로 사용할 수 있습니다.

# 정리

C#에서 `object`와 `var`는 모두 타입을 추상적으로 사용할 수 있도록 하지만, 각각 다른 목적과 특성을 가지고 있습니다.

`object`는 다형성을 지원하고 다양한 타입을 다룰 수 있는 유연성을 제공하지만, 박싱과 언박싱의 오버헤드가 있습니다.

반면, `var`는 타입 추론을 통해 코드의 간결성을 높이며, 정적 타입의 유지를 보장합니다.

두 키워드 모두 다양한 타입을 지원하는 키워드라고 생각하고 아무렇게나 사용하는 것 보다는, 상황에 맞게 적절하게 사용하는 것이 좋습니다.

---

# 참고

[var - 정적 타이핑 타입 추론 키워드](/posts/var-%EC%A0%95%EC%A0%81-%ED%83%80%EC%9D%B4%ED%95%91-%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0-%ED%82%A4%EC%9B%8C%EB%93%9C/){: }

[클래스(Class)와 백킹 필드(backing field), 그리고 base와 this](/posts/%ED%81%B4%EB%9E%98%EC%8A%A4(class)%EC%99%80-%EB%B0%B1%ED%82%B9-%ED%95%84%EB%93%9C(backing-field),-%EA%B7%B8%EB%A6%AC%EA%B3%A0-base%EC%99%80-this/){: }

[[OOP] 상속성(Inheritance)](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: }

[[OOP] 다형성(Polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }

[C＃의 값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: }

[C＃의 박싱과 언박싱(Boxing and Unboxing)](/posts/c-%EC%9D%98-%EB%B0%95%EC%8B%B1%EA%B3%BC-%EC%96%B8%EB%B0%95%EC%8B%B1(boxing-and-unboxing)/){: }