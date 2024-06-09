---
title: C＃의 박싱과 언박싱(Boxing and Unboxing)
date: 2024-05-30 19:19:42 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 값 타입
  - 자료형
  - 참조 타입
math: false
type: C#
keywords:
  - Boxing
  - Unboxing
  - 박싱
  - 언박싱
---

# Boxing과 Unboxing이란?

Boxing과 Unboxing은 C#에서 [값 타입(Value Type)](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: target="_blank"}과 [참조 타입(Reference Type)](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: target="_blank"} 간의 변환에 대한 개념입니다.
<br>
이 변환은 유연성과 타입 안정성을 제공하면서도 성능에 영향을 미칠 수 있습니다

간략하게, <span class="keyword">**박싱(Boxing)**</span> 이란, <span class="font_highlight">값 타입(Value Type)을 참조 타입(Reference Type)으로 변환</span>하는 것을 의미합니다.
<br>
<span class="keyword">**언박싱(Unboxing)**</span> 이란, <span class="font_highlight">Boxing 했던 참조 타입(Reference Type)을 다시 값 타입(Value Type)으로 변환(복귀)하는 것</span>을 의미합니다.

이를 가능하게 하는 것이 object 타입이라는 최상위 부모 객체가 존재하기 때문입니다.

## object 타입이란?

C#에서 `object` 타입은 <span class="font_highlight">모든 데이터 타입의 **최상위 클래스**</span>이며 `System.Object` 클래스의 별칭입니다.

모든 클래스와 데이터 타입은 `object`로부터 상속받습니다. 따라서 C#의 모든 타입은 `object` 타입으로 참조될 수도 있습니다. 

```csharp
object myObject = "Hello, World!";
object anotherObject = 123;
object yetAnotherObject = new DateTime(2023, 1, 1);
```

## Boxing

Boxing은 <span class="font_highlight">값 타입(Value Type)을 참조 타입(Reference Type)으로 변환</span>하는 과정입니다.

이 과정은 값 타입의 데이터를 **힙(Heap) 메모리**에 **복사**하여 객체를 생성하고, 생성된 객체의 참조를 `object` 타입 변수에 저장하는 방식으로 이루어집니다.

```csharp
int num = 123;
object boxedNum = num; // Boxing
```

1. 값 타입 데이터를 복사하여 힙에 할당.
2. 힙에 할당된 객체의 주소를 `object` 참조에 저장.

이 과정에서 새로운 힙 객체를 생성하고 값 타입 데이터를 복사하는 비용이 발생합니다.

## Unboxing

Unboxing은 <span class="font_highlight">참조 타입(Reference Type)을 다시 값 타입(Value Type)으로 변환(복귀)</span>하는 과정입니다.

이때 참조 타입 변수가 실제로 **원래의 값 타입과 호환**되어야 합니다.

Unboxing은 `object` 타입 변수에서 시작하여, 저장된 힙 객체의 값을 원래의 **값 타입 변수**로 **복사**합니다. 이 과정은 명시적 형 변환이 필요합니다.

```csharp
object boxedNum = 123;
int num = (int)boxedNum; // Unboxing
```

1. 참조 타입 변수에서 힙 객체의 주소를 가져옴.
2. 힙 객체의 값을 값 타입 변수에 복사합니다.

이 과정에서 또한 데이터 복사 비용이 발생합니다.

# 성능에 미치는 영향

- **메모리 할당:** Boxing은 힙에 새로운 객체를 할당하므로 메모리 할당 비용이 증가합니다.
- **값 복사:** Boxing과 Unboxing은 값 복사를 포함하므로, 특히 큰 값 타입의 경우 상당한 오버헤드를 초래할 수 있습니다.
- **가비지 컬렉션:** 힙에 할당된 객체는 가비지 컬렉션의 대상이 되므로, 추가적인 성능 부담이 증가할 수 있습니다.

따라서, Boxing과 Unboxing의 빈번한 사용은 성능 저하의 원인이 될 수 있습니다.

# Boxing과 Unboxing이 발생하는 이유

Boxing과 Unboxing은 C#에서 값 타입과 참조 타입 간의 유연한 변환을 가능하게 하며, 이로인해 다양한 방법으로 Boxing을 사용하게 될지도 모릅니다.

#### 1. 컬렉션

`ArrayList`와 같은 비제네릭 컬렉션은 `object` 타입을 사용하므로, 값 타입을 저장할 때마다 Boxing이 발생합니다.

```csharp
ArrayList list = new ArrayList();
int num = 123;
list.Add(num); // Boxing 발생
```

#### 2. 인터페이스

`struct`가 인터페이스를 구현할 수 있기 때문에, 값 타입인 struct를 인터페이스 타입으로 변환될 때도 Boxing이 발생합니다.

```csharp
interface IExample { }
struct ExampleStruct : IExample { }

ExampleStruct example = new ExampleStruct();
IExample interfaceExample = example; // Boxing 발생
```

#### 3. 그 외

그 외에도 `object`타입을 사용하면 다양한 타입을 저장 할 수 있다는 편의성 때문에, `object` 타입을 이용하기도 합니다.

```csharp
object[] array = new object[3];
array[0] = 123;
array[1] = "test";
array[2] = false;
```

# 제네릭 컬렉션 사용

Boxing과 Unboxing을 피하기 위해 제네릭 컬렉션을 사용하는 방법을 고려할 수 있습니다.

예를 들어, `ArrayList` 대신 `List<T>`를 사용하면 Boxing과 Unboxing을 피할 수 있습니다.

```csharp
List<int> list = new List<int>();
list.Add(123); // Boxing 발생하지 않음
int num = list[0]; // Unboxing 발생하지 않음
```

> 제네릭 컬렉션이란, 내부에서 사용할 데이터 타입을 외부에서 결정하여 컴파일시에 해당 데이터 타입의 컬렉션을 만들어 주는 것을 말합니다.
{: .prompt-tip }

---

# 참고

[C＃의 값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: }

[메모리 구조 (Memory Structure)](/posts/%EB%A9%94%EB%AA%A8%EB%A6%AC-%EA%B5%AC%EC%A1%B0-Memory-Structure/){: }