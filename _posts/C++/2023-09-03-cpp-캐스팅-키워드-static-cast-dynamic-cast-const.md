---
title: C++ 캐스팅 키워드 (static_cast, dynamic_cast_, const_cast...)
date: 2023-09-03 21:17:02 +0900
categories:
  - C++
  - 풀스택 개발
tags:
  - c
  - const
  - 데이터
  - 변수
  - 자료형
  - 컴파일
keywords:
  - const_cast
  - dynamic_cast
  - reinterpret_cast
  - static_cast
  - 정의되지 않은 동작
  - undefined behavior
---

C++에서는 C보다 다양한 캐스팅 연산자를 제공하여 타입 변환을 수행할 수 있습니다.

이러한 캐스팅 연산자들은 각각 다른 목적과 사용법을 가지고 있으며, 잘못 사용하면 예상치 못한 문제를 일으킬 수 있습니다.

이 글에서는 **`static_cast`**, **`dynamic_cast`**, **`const_cast`**, **`reinterpret_cast`**에 대해 자세히 알아보고, 각각 어떤 상황에서 사용해야 하는지, 주의해야 할 점은 무엇인지 살펴보겠습니다.

---

# static_cast<>()

- <span class="keyword">**컴파일 타임**</span> 캐스팅: **컴파일러**가 타입을 검사하고 변환합니다.
- 주로 **기본 자료형간 형변환**이나, **상속 관계에 있는 클래스간의 형변환**에 사용할 수 있습니다.
- 다만, 형변환 시점이 컴파일 시점이기 때문에 런타임 타입 검사를 하지 않아, **상속 관계에 있는 클래스간의 형변환에 조심**히 사용해야 합니다.

## 주의 사항

- **`static_cast`**는 **런타임 검사를 수행하지 않으므로, 안전한 타입 변환이 보장되지 않습니다**.
- 예를 들어, **다운 캐스팅**을 할 때 실제 객체의 런타임 타입이 예상한 타입과 다르면 **정의되지 않은 동작(undefined behavior)**이 발생할 수 있습니다.

## 예제

```cpp
int a = 10;
float b = static_cast<float>(a);  // int에서 float로 형변환
```


> <span class="keyword">**정의되지 않은 동작(undefined behavior)**</span>이란, **프로그램이 어떻게 동작할지 예측할 수 없는 상태**입니다. <br>
> 즉, **잘못된 메모리 접근**이나 **오버플로우/언더플로우** 등이 있지만, **잘못된 결과**를 출력할 수 있습니다. <br>
>  <br>
> <span class="small">따라서, 다음과 같은 경우가 발생할 수 있습니다. </span><br>
> 1. **프로그램 크래시**: 가장 일반적으로 생각되는 결과로, 프로그램이 중단되고 오류 메시지가 출력될 수 있습니다. <br>
> 2. **잘못된 결과**: 프로그램이 계속 실행되지만, 예상과 다른 결과를 출력하거나 파일, 데이터베이스 등에 잘못된 데이터를 저장할 수 있습니다.
> 3. **무한 루프**: 프로그램이 끝나지 않고 계속 실행되는 상태에 빠질 수 있습니다. <br>
> 4. **메모리 누수**: 프로그램이 계속 실행되면서 점점 더 많은 메모리를 소비할 수 있습니다. <br>
> 5. **보안 취약점**: 가장 심각한 경우로, 악의적인 공격자가 이를 이용하여 시스템을 해킹할 수 있습니다. <br>
{: .prompt-info}

---

# dynamic_cast<>()

- <span class="keyword">**런타임**</span> 캐스팅: 런타임에 <span class="important">**런타임 타입 정보(RTTI)**</span>로 타입을 검사하고 변환합니다.
- 주로 **다형성을 가진 클래스**의 포인터나 참조를 안전하게 **다운 캐스팅**하기 위해 사용됩니다.
- <span class="important">**RTTI**</span> 정보를 런타임에 조회하는 중에 메모리 접근과 조건 분기 등의 동작이 발생할 수 있으므로, static_cast에 비해 **성능 오버헤드**가 있을 수 있습니다.

## 주의 사항

- 변환 실패 시 nullptr을 반환하므로, **반환 값을 체크**해야 합니다.
- <span class="important">**런타임 타입 정보(RTTI)**</span>는 **다형성을 가진 클래스**(즉, 하나 이상의 가상 함수를 가진 클래스)에 대해서만 생성됩니다. 만약 클래스에 가상 함수가 없다면, 그 클래스에 대한 RTTI 정보는 생성되지 않습니다.
- **RTTI**를 사용하므로, **컴파일러 옵션**에서 이를 비활성화하면 **`dynamic_cast`**와 **`typeid`** 같은 RTTI를 사용하는 연산자를 사용할 수 없게 됩니다.

## 예제

```cpp
Base* basePtr = new Derived();
Derived* derivedPtr = dynamic_cast<Derived*>(basePtr);  // 안전한 다운 캐스팅
```

---

# static_cast와 dynamic_cast의 안전한 형변환

**`static_cast`**는 컴파일 시점에 타입 변환을 수행하는 캐스팅 연산자입니다. 이는 런타임에 동적으로 타입을 검사하지 않는다는 것을 의미합니다.

예를 들어, 다음과 같은 상속 구조가 있다고 가정해봅시다.

```cpp
class Base { /* ... */ };
class Derived : public Base { /* ... */ };
```

컴파일러는 **`Derived`**가 **`Base`**의 상속을 받았으므로 컴파일을 허용하겠지만, 
실제로 **`base`**가 가리키는 객체는 **`Derived`** 타입이 아니므로, **`derived`** 포인터를 사용하면 **정의되지 않은 동작(undefined behavior)**이 발생할 수 있습니다.

```cpp
Base* base = new Base();
Derived* derived = static_cast<Derived*>(base);  // 문법적으로 허용되지만 위험함
```

반면에 **`dynamic_cast`**는 런타임에 타입 검사를 수행하기 때문에, 위와 같은 상황에서 안전하게 타입 변환을 수행할 수 있습니다.

```cpp
Base* base = new Base();
Derived* derived = dynamic_cast<Derived*>(base);  // 런타임에 타입 검사

if (derived) {
    // 안전하게 Derived 타입의 멤버에 접근 가능
} else {
    // 변환 실패, Derived 타입이 아님
}
```

따라서 **`static_cast`**가 런타임 검사를 수행하지 않으므로, 안전한 타입 변환이 보장되지 않는다는 말은, **`static_cast`**를 사용할 때는 프로그래머가 캐스팅의 유효성을 직접 검증해야 하며, 그렇지 않으면 위험한 상황이 발생할 수 있다는 것을 의미합니다.

---

# const_cast<>()

- **컴파일 타임** 캐스팅: 상수성(cv-qualifier: **`const`**, **`volatile`** 등)을 추가하거나 제거합니다.
- 주로 함수가 상수 포인터나 참조를 받아서 내부에서 수정할 필요가 있을 때 사용됩니다. 하지만 이를 남용하면 프로그램의 안정성을 해칠 수 있습니다.

## 주의 사항

- **`const_cast`**를 사용하여 **`const`** 객체를 **`non-const`**로 변환하여 수정하면 정의되지 않은 동작(undefined behavior)이 발생할 수 있습니다.
- **`volatile`** 객체도 마찬가지로 **`non-volatile`** 타입의 참조나 포인터로 변환하면 위험합니다.

## 예제

```cpp
const int constValue = 10;
int* modifiableValue = const_cast<int*>(&constValue);  // const 제거
```

---

# **reinterpret_cast<>()**

- **컴파일 타임** 캐스팅: 메모리를 재해석합니다. (**타입 간에 기본 비트 패턴을 재해석**하는 캐스팅 연산자입니다.)
- **서로 다른 타입의 포인터나 참조를 변환할 때 사용**됩니다.
다만, 이는 매우 위험한 작업이므로 이 캐스팅을 사용할 때는 코드의 동작을 정확히 이해하고 있어야 합니다.

## 사용 가능한 변환

- **포인터 간 변환**: 서로 다른 타입의 포인터로 변환할 수 있습니다.
- **포인터와 정수 간 변환**: 포인터를 정수로, 정수를 포인터로 변환할 수 있습니다.
- **함수 포인터 간 변환**: 다른 함수 포인터 타입으로 변환할 수 있습니다.

## 주의 사항

- **`reinterpret_cast`**는 **타입 안전성을 보장하지 않습니다**. 따라서, 잘못된 타입으로의 캐스팅은 정의되지 않은 동작(undefined behavior)이 발생할 수 있습니다.

## 예제

```cpp
int* intPtr = new int(10);
void* voidPtr = reinterpret_cast<void*>(intPtr);  // int 포인터를 void 포인터로 변환
```