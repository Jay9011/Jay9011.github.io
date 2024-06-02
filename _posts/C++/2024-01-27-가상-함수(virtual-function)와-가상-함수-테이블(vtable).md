---
title: 가상 함수(virtual function)와 가상 함수 테이블(vtable)
date: 2024-01-27 18:19:01 +0900
categories:
  - 풀스택 개발
  - C++
tags:
  - c
  - 다형성
math: false
type: C++
keywords:
  - virtual function
  - 가상 함수
  - 가상 함수 테이블
  - vtable
---

# 가상 함수(virtual function)란

C++ 프로그래밍 언어는 객체 지향 프로그래밍(OOP)을 지원하며, 그 중 다형성(polymorphism)이 중요한 개념 중 하나입니다.

다형성을 구현하는 한 방법으로, 가상 함수는 **유연성**과 **확장성**을 크게 향상시킵니다.

<span class="keyword">**가상 함수(virtual function)**</span>는 기본 클래스에서 선언되고, <span class="font_highlight">파생 클래스에서 **재정의**할 수 있는 멤버 함수</span>입니다.

이를 통해, 파생 클래스의 객체를 가리키는 기본 클래스의 포인터를 사용하여 재정의된 함수를 호출할 수 있습니다.

가상 함수의 주된 이점은 실행 시간에 결정되는 함수 호출을 통해, 소프트웨어의 다양한 컴포넌트 간에 더 유연한 상호 작용을 가능하게 하는 것입니다.

<br>

가상 함수는 `virtual` 키워드를 사용하여 선언하며, C++11 이후 버전에서는 `override` 키워드를 사용하여 재정의함으로써, 오버라이딩의 정확성을 컴파일러가 검증할 수 있게 합니다.

```cpp
class Base
{
public:
    virtual void display()
    {
        cout << "Display Base" << endl;
    }
};

class Derived : public Base
{
public:
    void display() override // 파생 클래스에서는 override 키워드를 사용하고, virtual 키워드 생략 가능
    {
        cout << "Display Derived" << endl;
    }
};
```

# 가상 함수(virtual function)의 동작 원리와 가상 테이블(vtable)

C++의 가상 함수는 런타임 다형성을 구현하기 위해 <span class="keyword">**가상 함수 테이블(vtable)**</span>이라는 구조를 사용합니다.

가상 함수 테이블은 <span class="font_highlight">각 클래스에 대한 **가상 함수 포인터들**을 저장</span>하는 배열입니다.

클래스의 각 인스턴스는 <span class="important">**가상 함수 테이블을 가리키는 포인터(vptr)**</span>를 가지고 있으며, 이를 통해 런타임에 어떤 함수를 호출할지 결정합니다.

1. 클래스 정의 시 vtable 생성
  - <span class="font_highlight">가상 함수가 하나라도 존재</span>하는 클래스에 대해 컴파일러가 가상 함수 테이블을 생성합니다.
  - 이 테이블에는 해당 클래스의 각 가상 함수에 대한 포인터가 저장됩니다.
2. `vptr` 초기화
  - 인스턴스가 생성될 때, `vptr`은 해당 클래스의 vtable을 가리키도록 초기화됩니다.
3. 가상 함수 호출
  - 가상 함수 호출 시, `vptr`을 통해 적절한 vtable을 찾아가고, 그 안의 함수 포인터를 사용하여 함수를 호출합니다.

# 가상 함수 테이블(vtable)의 구조

가상 함수 테이블은 가상 함수를 포함하는 모든 클래스에 대해 생성되며, 파생 클래스에서 가상 함수를 <span class="font_highlight">**재정의**하면, 해당 함수의 포인터가 새롭게 정의된 함수를 가리키도록 업데이트</span>됩니다.

즉, 다음 코드와 같이 가상 함수를 만들었다면 이미지와 같은 가상 함수 테이블이 생성됩니다.
```cpp
class Base
{
public:
    virtual void VirtualFunc()
    {
        cout << "Base::VirtualFunc" << endl;
    }
    virtual void VirtualFunc2()
    {
        cout << "Base::VirtualFunc2" << endl;
    }
    virtual ~Base() {}
};

class Derived : public Base
{
public:
    virtual void VirtualFunc() override
    {
        cout << "Derived::VirtualFunc" << endl;
    }
    virtual ~Derived() {}
};
```

이때, `Derived`는 `VirtualFunc2`를 재정의 하지 않았으므로, 가상 함수 포인터를 업데이트 하지 않고 `Base::VirtualFunc2` 포인터를 그대로 가지고 있습니다.

즉, 재정의되지 않은 함수는 기본 클래스의 포인터를 유지하게 됩니다.

![vtable 구조](https://i.postimg.cc/G2MS5mBW/vtable.webp){: w="700" h="346"}

# 성능

가상 함수와 가상 함수 테이블의 사용은 **편의성**과 **유연성**을 제공하지만, 이에는 <span class="important">**약간의 성능 오버헤드**</span>가 따릅니다.

가상 함수를 호출할 때마다 `vptr`을 통해 vtable을 참조해야 하므로, 직접 함수 호출보다 느릴 수 있습니다.

따라서, 가상 함수는 주로 런타임 다형성이 필요한 경우에만 사용하고, 불필요한 경우에는 피하는 것이 좋습니다.