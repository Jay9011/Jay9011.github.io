---
title: Nullable과 null 병합 연산자, null 조건 연산자
date: 2024-06-08 17:17:29 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 값 타입
  - 변수
  - 자료형
  - 참조 타입
math: false
type: C#
keywords:
  - GetValueOrDefault
  - null 병합 연산자
  - null 조건 연산자
  - Nullable
  - 제네릭 구조체
---

`null`이란, "값이 없는" 혹은 "어떤 객체도 참조하지 않은" 참조형 변수를 말합니다.
<br>
C#에서 `null`을 가질 수 있는 타입은 [참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/#%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85-reference-types){: target="_blank"}입니다. [값 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/#%EA%B0%92-%ED%83%80%EC%9E%85-value-types){: target="_blank"}은 `null`일 수 없습니다.
<br>
다시 말해, C#에서 <span class="font_highlight">null은 "어떤 객체도 참조하지 않은 참조형 변수의 디폴트 값"</span>이라고 할 수 있습니다.

값 형식에서의 디폴트 값(초기화 하지 않을 때 가지는 값)은 "0"입니다.
<br>
정수형은 "0", bool 타입은 "false(0)", char 타입은 '\\0'을 가지지만, 이건 값이 없는 상태라고 할 수는 없습니다.

`Nullable<T>` 타입은 이런 값 형식(예: `int`, `double`, `bool` 등)도 `null` 값을 가질 수 있게 해주는 기능입니다.

# Nullable 타입 정의와 사용법

## Nullable 타입 정의

nullable 타입 혹은 `Nullable<T>` 타입은 **제네릭 구조체(Generic Struct)** 입니다.

`Nullable<T>`는 <span class="font_highlight">값 타입(Value Type)인 'T'가 'null' 값을 가질 수 있도록 확장</span>해주는 구조체입니다.

```csharp
public partial struct Nullable<T> where T : struct
{
    private readonly bool hasValue; // Do not rename (binary serialization)
    internal T value; // Do not rename (binary serialization) or make readonly (can be mutated in ToString, etc.)
    //...
}
```

실제 `Nullable<T>`는 **hasValue**와 **value** 두 개의 필드를 가지고 있습니다.

- bool **hasValue**: 값이 있는지 여부를 나타내는 불리언 필드입니다. 이것으로 `null` 값을 가질 수 있는 것처럼 동작합니다.
- T **value**: 실제 값을 저장하는 필드입니다. 만약 `hasValue`가 `false`인 경우 `value`에 접근하면 `InvalidOperationException`이 발생합니다.

## 사용법

사용 방법은 다른 제네릭을 쓰듯이 `T`대신 원하는 자료형을 사용하고, 실제 `null` 변수를 다루듯이 다루면 됩니다.

```csharp
Nullable<int> i = null;

if (i == null)
{
    Console.WriteLine("int i 가 null 입니다.");  // int i 가 null 이므로 이 문자 출력
}
```

또한, `Nullable<T>` 표기의 축약형으로, 값 형식 뒤에 `?` 기호를 함께 붙이는 것으로 Nullable 타입을 정의할 수 있습니다.

예를 들어, `int?`는 `Nullable<int>` 의 축약형 입니다.

```csharp
int? nullableInt = null;
double? nullableDouble = null;
bool? nullableBool = null;
```

# Nullable 타입 반환

## GetValueOrDefault()

Nullable에 할당 된 값을 Nullable이 아닌 값 타입에 넣으려고 하면 "암시적으로 'int?' 형식을 'int' 형식으로 변환할 수 없습니다."라는 말을 합니다.
<br>
그렇다고 `value`에 그냥 접근해서 가져오는 것도 위험할 수 있습니다.(null일 때, value 필드에 접근하면 `InvalidOperationException`을 발생시킵니다.)

이때, 사용할 수 있는 메서드가 `GetValueOrDefault()`입니다.

```csharp
int? nullableInt = 42;

// int num1 = nullableInt; // 암시적으로 'int?' 형식을 'int' 형식으로 변환할 수 없습니다.
int num2 = nullableInt.GetValueOrDefault(); // null이면 0을 반환

Console.WriteLine(num2); // 출력: 42
```

`GetValueOrDefault()` 메서드는 <span class="font_highlight">값이 있으면 'value'를 반환하고, 값이 없으면 해당 값 타입('T')의 기본값을 반환</span>합니다.

## ?? (Null 병합 연산자)

또 다른 방법으로는 null 병합 연산자라고도 불리는 연산자(`??`)를 사용하는 것입니다.

해당 연산자는 `피연산자1 ?? 피연산자2`의 형태로 사용하며, <span class="font_highlight">피연산자1이 null이 아니라면 피연산자1 값 그대로, 'null'이면 피연산자2의 값을 반환</span>하는 연산자입니다.

```csharp
string? nullableStr = null;
Console.WriteLine($"str : {nullableStr ?? "Null"}"); // str : Null
```

# Nullable 타입 비교

Nullable 타입을 비교할 때는 일반 값 타입과 동일하게 비교할 수 있습니다.
<br>
Nullable 타입이 null일 경우 다른 Nullable 타입이 null인지 확인하거나, null이 아닌 값과 같은지 비교할 수 있습니다.

```csharp
int? a = 5;
int? b = null;

Console.WriteLine(a == b); // 출력: False
Console.WriteLine(a == 5); // 출력: True
Console.WriteLine(b == null); // 출력: True
```

# Null 조건 연산자 (?.)

다만, 다음과 같이 `null`의 멤버에 접근하려고 하면, `NullReferenceException`이 발생합니다.

```csharp
string? nullableStr = null;

if (nullableStr.Length > 0) // NullReferenceException
{
    Console.WriteLine("nullableStr.Length > 0");
}
```

보통은 이 문제를 해결하기 위해 `null`의 가능성이 있는 참조형 앞에 `null` 검사를 붙여줍니다.

```csharp
if (nullableStr != null && nullableStr.Length > 0)
```

하지만, C# 6.0부터는 null 조건 연산자 (`?.`)를 사용할 수 있습니다.

null 조건 연산자는 <span class="important">멤버 접근 연산자</span>(`.`)나 <span class="important">인덱스 연산자</span>(`[]`)와 같이 사용되어 피연산자의 값이 <span class="font_highlight">null이면 null을 리턴</span>합니다.

```csharp
string? nullableStr = null;
int? length = nullableStr?.Length;  // nullableStr이 null이면 null을 반환
Console.WriteLine($"length : {length}");    // length :
Console.WriteLine($"length?? : {length ?? -1}"); // length?? : -1

if(nullableStr?.Length > 0) // false
{
    Console.WriteLine("nullableStr.Length > 0");
}
else
{
    Console.WriteLine("nullableStr.Length <= 0");
}
```

# 정리

- `Nullable<T>` 타입은 값 타입에 null을 할당할 수 있게 해주는 제네릭 구조체입니다.
	- `T?` 형태의 nullable 축약형으로도 정의할 수 있습니다.
	- `value`가 null이면 `InvalidOperationException`이 발생합니다.
- `GetValueOrDefault()` 메서드로 nullable의 값 혹은 기본 값을 반환합니다.
- `??`(Null 병합 연산자)로 피연산자가 null 일 때, 다른 형태로 반환할 수 있습니다.
- `?.`(Null 조건 연산자)로 피연산자의 멤버로 접근하기 전에 null 체크를 할 수 있습니다.

---

# 참고

[C＃의 값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: }