---
title: 가상 소멸자 (Virtual Destructor)
date: 2024-02-01 17:28:16 +0900
categories:
    - 풀스택 개발
    - C++
tags:
    - c
    - 다형성
math: false
type: C++
keywords:
    - 가상 소멸자
    - Virtual Destructor
---

기본적으로 C++에서 소멸자는 클래스 인스턴스가 소멸될 때 호출되어 자원을 해제하는 역할을 합니다. 하지만 **상속 관계**에 있는 클래스들에서는 조금 더 신경 써야 할 부분이 있습니다.

예를 들어, 다음과 같은 클래스 구조를 생각해 봅시다.

```cpp
class Base
{
public:
    Base()
    {
	    cout << "Base 생성자 호출" << endl;
    }
    ~Base()
	{
        cout << "Base 소멸자 호출" << endl;
    }
};

class Derived : public Base
{
public:
    Derived()
    {
	    cout << "Derived 생성자 호출" << endl;
    }
    ~Derived()
	{
        cout << "Derived 소멸자 호출" << endl;
    }
};

int main()
{
	Base* base = new Derived();

	delete base;
}
```

위 코드에서는 `base`가 `Derived` 객체를 가리키고 있지만, `delete` 연산자는 `Base` 클래스의 소멸자만 호출하게 됩니다.

따라서 `Derived` 클래스의 소멸자가 호출되지 않아 `Derived` 객체가 올바르게 정리되지 않습니다.

![일반 소멸자 호출](https://i.postimg.cc/cL8J6ScK/virtual-01.webp)

이는 자원 누수나 예기치 않은 동작을 초래할 수 있습니다.

# 자식 소멸자

가상 소멸자를 보기 전에 우선, 자식 소멸자를 호출해 봅시다.

```cpp
int main()
{
	Derived* derived = new Derived();

	delete derived;
}
```

간단하게, `Base` 타입 포인터를 `Derived` 타입 포인터로 바꿔봤습니다.

![가상 소멸자](https://i.postimg.cc/nLWYB7jF/virtual-02.webp)

이때, 우리가 원하는대로 자식 소멸자와 부모 소멸자 모두 호출 된 것을 확인할 수 있습니다.



이를 봐서, C++에서 상속을 사용할 때, <span class="font_highlight">자식 클래스의 소멸자가 호출되면 부모 클래스의 소멸자도 자동으로 호출</span>되는 것을 알 수 있습니다.

이는 프로그래머가 직접 명시적으로 부모 클래스의 소멸자를 호출할 필요가 없도록 설계된 부분인데, 비슷하게 생성자도 암시적으로 부모 생성자를 호출하는 부분이 존재합니다.

다만, 소멸자는 생성자와는 반대로, 자식 클래스의 소멸자가 먼저 실행되고 나서 부모 클래스의 소멸자가 실행됩니다.

# 가상 소멸자

즉, 상속 관계에서 소멸자 호출 문제를 해결하기 위해, 기본 클래스의 소멸자를 가상 함수로 선언하면 해결할 수 있을 것 같다는 생각이 듭니다.

```cpp
class Base
{
public:
    Base()
    {
	    cout << "Base 생성자 호출" << endl;
    }
    virtual ~Base() // virtual 선언
	{
        cout << "Base 소멸자 호출" << endl;
    }
};

class Derived : public Base
{
public:
    Derived()
    {
	    cout << "Derived 생성자 호출" << endl;
    }
    ~Derived() override // override로 다형성
	{
        cout << "Derived 소멸자 호출" << endl;
    }
};
```

이제 `Base` 클래스의 소멸자가 가상 소멸자이므로, 다형성의 원칙에 따라 런타임에 파생 클래스에서 재정의된 소멸자가 호출되고, 이는 `Derived` 클래스의 소멸자를 호출한 다음 암시적으로 `Base` 클래스의 소멸자를 호출하게 됩니다.

![가상 소멸자](https://i.postimg.cc/nLWYB7jF/virtual-02.webp)

## 작동 원리

C++에서는 가상 함수를 구현하기 위해 가상 함수 테이블(Virtual Table, vtable)을 사용합니다.

가상 소멸자도 예외가 아니며, vtable에 포함됩니다.

따라서, 객체가 소멸될 때, vtable을 참조하여 재정의된 `Derived`의 소멸자가 호출되고, 이후 소멸자의 암시적인 호출로 인해 부모 소멸자가 차례대로 호출되게 되는 것입니다.

---

# 참고

[가상 함수(virtual function)와 가상 함수 테이블(vtable)](/posts/%EA%B0%80%EC%83%81-%ED%95%A8%EC%88%98(virtual-function)%EC%99%80-%EA%B0%80%EC%83%81-%ED%95%A8%EC%88%98-%ED%85%8C%EC%9D%B4%EB%B8%94(vtable)/){: }