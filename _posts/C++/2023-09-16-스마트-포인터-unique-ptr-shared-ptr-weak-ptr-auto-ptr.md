---
title: 스마트 포인터 (unique_ptr, shared_ptr, weak_ptr, auto_ptr)
date: 2023-09-16 23:20:15 +0900
categories:
  - C++
  - 풀스택 개발
tags:
  - c
  - 데이터
  - 메모리
  - 변수
  - 포인터
  - 스마트 포인터
keywords:
  - auto_ptr
  - Pointer
  - shared_ptr
  - unique_ptr
  - weak_ptr
  - 스마트 포인터
  - 포인터
  - RAII
---

기존 C/C++ 에서는 동적으로 할당 받은 메모리는 사용이 끝난 경우 반드시 해제해줘야 합니다. 만약, 메모리를 해제해주지 않으면 <span class="important">**메모리 누수(memory leak)**</span>가 발생할 수 있습니다.

<span class="keyword">**스마트 포인터(smart pointer)**</span>는 이런 프로그래머의 부담을 줄여줄 수 있는, 포인터처럼 동작하는 클래스 템플릿으로, **메모리 관리를 자동화하여 메모리 누수나 해제되지 않은 메모리에 대한 접근 등의 문제를 최소화하기 위해 사용**합니다.

# 스마트 포인터의 공통 동작

스마트 포인터는 클래스 템플릿입니다.

따라서, 사용할 때 **함수(스택 프레임)에서 지역 변수로 클래스 객체를 생성**하고, 포인터로 사용할 클래스 객체를 템플릿으로 넘겨주면 해당 객체를 동적으로 할당해 줍니다.

그리고, **함수(스택 프레임)을 벗어나게 될 때**, 함수 내에서 생성된 **객체의 소멸자가 호출 되면서** 할당된 메모리를 <span class="important">**자동으로 해제**</span>해 줍니다.

```cpp
int main()
{
	스마트포인터<class> 변수명 = make_func<class>(); // 스마트 포인터의 동적 메모리 할당
} // 스코프를 벗어나면서 메모리 해제
```

# 스마트 포인터의 장점

- <span class="important">**메모리 누수 최소화**</span>: 스마트 포인터는 **RAII**의 패턴을 구현해 사용합니다. RAII는 **자원을 할당하고 초기화하는 동시에 그 자원의 수명을 관리하는 프로그래밍 패러다임**입니다. <br>
스마트 포인터 객체는 일반적으로 스택 메모리에 생성되는데, 이 객체가 스코프를 벗어나면, 스택에서 팝되고 해당 스마트 포인터의 **소멸자가 호출**되어 **스마트 포인터가 가지고 있던 객체를 자동으로 해제**합니다.
- <span class="important">**예외 안전성**</span>: 메모리 누수가 되는 경우는 또 하나 있습니다. 기존의 방식으로 포인터를 할당하게 되면, 만약 **`delete`를 호출하기 전에 예외가 발생하는 경우**, 이 `delete`를 호출하지 못할 수 있습니다.<br>
스마트 포인터는 RAII의 패턴을 사용하기 때문에, **예외로 인해 스코프를 벗어나면 자동으로 객체를 해제**해 줍니다.
- <span class="important">**메모리 접근 문제 최소화**</span>: 스마트 포인터의 가장 중요한 장점 중 하나는 포인터의 접근에 안전하다는 것입니다.<br>
예를 들어, 기존 C++ 스타일로 메모리를 관리하다보면 `delete` 이후에 `nullptr` 로 초기화 시켜주지 않은 경우 메모리 주소가 그대로 남게 됩니다.<br>
스마트 포인터는 **메모리 직접 해제하지 않기 때문에 이러한 메모리 접근 문제를 최소화** 시킬 수 있습니다.

![포인터 예외](https://i.postimg.cc/c1kVxmfp/포인터_예외.png){: w="700" h="308"}

---

# unique_ptr

**`unique_ptr`**은 <span class="font_highlight">**특정 객체의 소유권을 하나의 포인터에만 두는 것**</span>으로, <span class="important">**객체의 소유권을 명확히 하는 포인터**</span>입니다.

- **소유권 이전**: 따라서, 포인터 객체를 복사할 수 없지만, 소유권의 이동 연산은 가능합니다. (**`std::move`** 사용)
- **RAII 패턴을 따름**: 하나의 소유권만을 가지기 때문에, **`unique_ptr`** 객체가 스코프를 벗어나면, 자동으로 소유하고 있는 메모리를 해제합니다.
- **명시적으로 해제**: **`unique_ptr`** 객체가 명시적으로 **`reset`** 메서드를 호출하는 것으로도 메모리를 해제할 수 있습니다.

## 사용법

```cpp
std::unique_ptr<int> ptr1(new int(5)); // C++11 스타일
std::unique_ptr<int> ptr2 = std::make_unique<int>(5); // C++14 이후 스타일
```

기본적으로는 C++14 이후 스타일이 권장됩니다.

## C++11 스타일 사용시 문제

기존의 방식에서 문제는 인자로 new로 할당된 메모리 주소를 넘겨줄 수 있기 때문인데, 만약, 아래와 같은 코드를 사용하게 되면 여러 문제가 발생할 수 있습니다.

```cpp
int* temp = new int(5);
std::unique_ptr<int> ptr(temp);
```

1. 첫 번째 문제는 **두 코드 사이에 예외가 발생했을 경우**입니다. <br>스마트 포인터를 쓰는 이유는 메모리 관리를 자동화하여 메모리 누수를 방지하기 위함인데, 중간에 예외가 발생하게 되면 스마트 포인터에 할당되기 전이기 때문에 메모리 누수가 발생할 수 있습니다.
2. 두 번째 문제는 **소유권 문제**입니다. <br>스마트 포인터의 사용 이유에 메모리에 대한 접근 등의 문제를 최소화하기 위한 것도 있는데, 만약 위와 같이 코드를 짠다면 **`temp`**와 **`ptr`** 두 개의 포인터가 동일한 메모리를 가리키게 됩니다. <br>이 경우, **`std::unique_ptr`**의 소멸자가 호출될 때 메모리가 해제되지만, **`temp`** 포인터는 여전히 그 메모리를 가리키고 있을 것입니다. 이 상태에서 **`temp`**를 통해 메모리에 접근하려고 하면 정의되지 않은 동작(Undefined Behavior)이 발생할 수 있습니다.

따라서, **`make_unique<>`** 방식을 추천합니다.

---

# shared_ptr

**`shared_ptr`**은 <span class="keyword">**참조 카운팅(reference counting)**</span>을 사용해 <span class="font_highlight">**여러 포인터가 같은 메모리 주소를 공유할 수 있는 포인터**</span>입니다.

- **다중 소유**: 따라서, 여러 객체나 함수가 동일한 메모리를 안전하게 공유할 수 있습니다.
- **참조 카운팅**: 내부적으로 참조 카운터를 유지하여, 몇 개의 **`shared_ptr`**이 같은 메모리를 가리키고 있는지 추적할 수 있습니다.
- **RAII 패턴의 응용**: 객체를 할당할 때, 참조 카운팅을 늘리고, 스코프를 벗어나면 참조 카운딩을 줄이는 것으로, 마지막 **`shared_ptr`**이 소멸될 때 자동으로 소유하고 있던 메모리를 해제합니다.

## 사용법

```cpp
std::shared_ptr<int> ptr1(new int(5)); // C++11 스타일
std::shared_ptr<int> ptr2 = std::make_shared<int>(5); // C++14 이후 스타일
std::shared_ptr<int> ptr3(ptr2); // 복사 생성자를 이용한 초기화
std::shared_ptr<int> ptr4 = ptr3; // 대입을 통한 초기화

std::cout << ptr1.use_count() << std::endl;
std::cout << ptr2.use_count() << std::endl;
std::cout << ptr3.use_count() << std::endl;
std::cout << ptr4.use_count() << std::endl;
```

![SharedPtr](https://i.postimg.cc/D0hm8hVz/Shared-Ptr.png){: w="323" h="131"}

기본적으로는 C++14 이후 스타일이 권장됩니다.

기본적인 문제는 [unique_ptr의 문제](#c11-스타일-사용시-문제)와 같습니다.

하지만, **`shared_ptr`**은 **`unique_ptr`**과 다르게 복사가 가능하므로, 복사 생성자와 복사 대입 연산자를 사용할 수 있습니다.

하지만, 이로 인해 발생할 수 있는 문제가 있습니다.

## 순환 참조 문제

바로 순환 참조 문제입니다.

만약, 복사 대입 연산자를 통해서 아래와 같은 코드를 작성했다고 가정해 봅니다.

```cpp
#include <iostream>
#include <memory>

class TestClass
{
public:
	TestClass(int n) { num = n; }
public:
	std::shared_ptr<TestClass> parent;
	int num = 0;
};

void test()
{
	std::shared_ptr<TestClass> ptr1 = std::make_shared<TestClass>(3); // ptr1 참조 카운트: 1
	std::shared_ptr<TestClass> ptr2 = std::make_shared<TestClass>(5); // ptr2 참조 카운트: 1

	std::cout << "ptr1 Count : " << ptr1.use_count() << ", ptr2 Count : " << ptr2.use_count() << std::endl;

	ptr1->parent = ptr2; // ptr2 참조 카운트: 2
	ptr2->parent = ptr1; // ptr1 참조 카운트: 2

	std::cout << "ptr1 Count : " << ptr1.use_count() << ", ptr2 Count : " << ptr2.use_count() << std::endl;

} // ptr1과 ptr2 소멸자 호출, 참조 카운트 감소하지만 0이 되지 않음. 메모리 누수 발생

int main()
{
	test();
}
```

![SharedPtr1](https://i.postimg.cc/mkSk9nRy/Shared-Ptr1.png){: w="331" h="96"}

즉, **`test()`** 함수가 종료되어도 두 객체의 참조 카운트는 1로 남고, **`shared_ptr`**의 참조 카운트가 0이 되지 않기 때문에 메모리 누수가 발생하게 됩니다.

ptr1과 ptr2가 가리키고 있던 메모리는 그대로 남아 있는데, 함수(스택 프레임)이 종료되면서 ptr1과 ptr2가 사라지면서, 메모리 주소를 잃어버리기 때문입니다.

---

# weak_ptr

shared_ptr의 순환 참조 문제는 **`weak_ptr`**를 사용하여 해결 할 수 있습니다.

**`weak_ptr`**은 **`shared_ptr`와 함께 사용**되며, <span class="font_highlight">**참조 카운트에 영향을 주지 않는 스마트 포인터**</span>입니다.

- **용도**: 따라서, **`weak_ptr`**은 **객체를 가리키지만, 객체의 소유권을 갖지 않아, 순환 참조 문제를 해결**합니다.
- **메모리 접근**: 하지만, **실제 객체에 접근하기 위해서**는 **`weak_ptr`**를 **`shared_ptr`**로 업그레이드해야 하며, **이 때 참조 카운트가 증가**합니다.
- **메모리 해제**: **`weak_ptr`** 자체는 메모리를 해제하지 않습니다. 대신, 연관된 **`shared_ptr`**이 모두 소멸되면 메모리가 해제됩니다.


## 사용법

```cpp
std::shared_ptr<int> ptr1 = std::make_shared<int>(100); // shared_ptr 생성
std::weak_ptr<int> weak_ptr = ptr1; // 참조 카운트 변화 없음

std::shared_ptr<int> ptr2(weak_ptr); // 복사 생성자를 사용해 직접 변환, 참조 카운트 증가
std::shared_ptr<int> WtoSptr = weak_ptr.lock() // weak_ptr에 lock()함수를 사용해서 변환, 참조 카운트 증가
```

## 순환 참조 문제 해결

**`weak_ptr`**은 **객체를 약하게 참조**하는 것으로, 객체의 수명을 제어하지 않으면서도 **`shared_ptr`**과 같이 **객체를 여러 위치에서 공유**할 수 있게 합니다.

**`shared_ptr`**의 카운팅에 관여하지 않기 때문에 순환 참조 문제를 해결할 수 있습니다.

실제 객체에 접근해서 사용할 필요가 있을 때, **`shared_ptr`**로 변환하여 사용하기에 실제 사용되는 곳에서 객체의 수명을 제어하게 됩니다.

이때, `lock()` 함수를 사용하면, **`shared_ptr`**의 실제 객체가 할당 해제되었다면 `nullptr`을 반환됩니다. 따라서 **메모리에 대한 접근 문제**도 해결할 수 있습니다.

```cpp
#include <iostream>
#include <memory>

int main()
{
    std::shared_ptr<int> ptr1 = std::make_shared<int>(100); // 참조 카운트: 1
    std::shared_ptr<int> ptr2 = ptr1; // 참조 카운트: 2

    std::weak_ptr<int> weak_ptr = ptr1; // 참조 카운트 변화 없음

    std::cout << *ptr1 << std::endl; // 실제 객체 출력. // 100
    //std::cout << *weak_ptr << std::endl; // 실제 객체에 바로 접근할 수 없음.

    if (std::shared_ptr<int> WtoSptr = weak_ptr.lock())
    {
        std::cout << *WtoSptr << std::endl; // 실제 객체 출력. // 100
    }
    else
    {
        std::cout << "객체가 해제되어 포인터로 변환할 수 없음" << std::endl;
    }

    ptr1.reset(); // 참조 카운트: 1
    ptr2.reset(); // 참조 카운트: 0, 메모리 해제

    if (std::shared_ptr<int> WtoSptr = weak_ptr.lock())
    {
        std::cout << *WtoSptr << std::endl;
    }
    else
    {
        std::cout << "객체가 해제되어 포인터로 변환할 수 없음" << std::endl; // 메모리가 해제되었으므로 실제 객체에 대한 접근을 막는다.
    }
}
```

![WeakPtr](https://i.postimg.cc/G3PH64YT/WeakPtr.png){: w="390" h="119"}

---

# auto_ptr

- C++11 이전에는 **`auto_ptr`**이라는 스마트 포인터가 있었으나 C++11부터 사용하지 않기를 권고하고 있습니다. (C++17 부터는 아예 삭제되었습니다.)
- **`unique_ptr`**과 같이 하나의 소유권을 가지며 대입 연산자를 사용하면 소유권이 이전됩니다.
- 하지만, 이 복사 대입 연산자를 사용해 소유권이 이전 되기 때문에, 코드만으로 명확히 이해하기 어렵고 실제로 소유하고 있는지 헷갈릴 수 있기 때문에 **`unique_ptr`**로 대체된 것으로 알고 있습니다.

기존 auto_ptr의 소유권 이전
{: .small .text-center .text-muted .mb-0}
```cpp
std::auto_ptr<int> p1(new int(42));
std::auto_ptr<int> p2 = p1; // p1이 p2로 복사 된건지 이동 된건지 코드를 보다보면 헷갈릴 수 있음.
```

<br>

unique_ptr에서 복사 대입 연산자를 삭제함으로 명확히 이동만 되는 것을 볼 수 있음
{: .small .text-center .text-muted .mb-0}
```cpp
std::unique_ptr<int> p1(new int(42));
std::unique_ptr<int> p2 = std::move(p1); // move를 통해 명확히 이동된 것을 알아볼 수 있음.
```

---

# 참고

[포인터 (Pointer)](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){:target="_blank"}

[허상 포인터 (Dangling pointer) 발생 원인과 예방 방법](/posts/%ED%97%88%EC%83%81-%ED%8F%AC%EC%9D%B8%ED%84%B0-dangling-pointer-%EB%B0%9C%EC%83%9D-%EC%9B%90%EC%9D%B8%EA%B3%BC-%EC%98%88%EB%B0%A9-%EB%B0%A9%EB%B2%95/){:target="_blank"}

[참조와 포인터 (Reference & Pointer)](/posts/%EC%B0%B8%EC%A1%B0%EC%99%80-%ED%8F%AC%EC%9D%B8%ED%84%B0-reference-pointer/){:target="_blank"}