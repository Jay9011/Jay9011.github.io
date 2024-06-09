---
title: RAII와 스마트 포인터 (Resource Acquisition Is Initialization)
date: 2023-09-18 23:19:48 +0900
categories:
  - 풀스택 개발
  - C++
tags:
  - c
  - 객체 지향
  - 동적
  - 메모리
  - 스마트 포인터
  - 포인터
keywords:
  - RAII
  - Resource Acquisition Is Initialization
  - 스마트 포인터
  - 객체 지향
---

<span class="keyword">**RAII(Resource Acquisition Is Initialization)**</span>는 주로 <span class="font_highlight">**객체 지향 프로그래밍에서 자원을 효과적으로 관리하기 위한 패러다임**</span>입니다.

[MS Learn](https://learn.microsoft.com/ko-kr/cpp/cpp/object-lifetime-and-resource-management-modern-cpp?view=msvc-170){: target="_blank"}에 적혀있는 글을 읽어보면 MS에서는 <span class="text-muted serif">*“개체가 리소스를 소유한다는 원칙을 ‘리소스 획득은 초기화’ 또는 RAII라고도 합니다.”*</span> 라고 적혀있는 것을 볼 수 있습니다.

즉, <span class="keyword">**RAII**</span>는 <span class="important">**객체가 자원을 소유한다는 원칙의 패러다임**</span>이고 Resource Acquisition Is Initialization 라는 영어를 해석해보면, <span class="important">**자원의 획득은 초기화 단계에서 이루어진다**</span>고 볼 수 있습니다.

그리고 <span class="text-muted serif">*“소유 개체 자체는 스택에 선언됩니다.”*</span>라는 말이 있고, <span class="text-muted serif">*“리소스 소유 스택 개체가 scope 나가면 소멸자를 자동으로 호출합니다. 이러한 방식으로 C++의 가비지 수집은 개체 수명과 밀접한 관련이 있으며 결정적입니다. 리소스는 항상 제어할 수 있는 프로그램의 알려진 지점에서 해제됩니다.”*</span>라고 하는 말을 정리해 보면, **<span class="font_highlight">객체를 스택 Scope (함수)에 선언하면 소멸자를 자동으로 호출</span>하기 때문에 <span class="font_highlight">자원의 소멸을 객체의 소멸자에 두는 것으로 객체의 수명과 함께 자동화 시킬 수 있다</span>**는 말로 정리할 수 있을 것 같습니다.


> [MS Learn](https://learn.microsoft.com/ko-kr/cpp/cpp/object-lifetime-and-resource-management-modern-cpp?view=msvc-170){: target="_blank"}에서 또 눈여겨 볼 부분이 있습니다. <br>
> <span class="serif">*”C++의 디자인은 개체가 scope 나갈 때 제거되도록 합니다. 즉, 블록이 종료되면 역순으로 파괴됩니다.”*</span> <br>
> 이 부분에서, 스택 블록이 종료되면 스택 영역에 할당된 객체들은 **역순으로 파괴**된다는 것을 알 수 있습니다.<br>
> 어떻게 보면 당연한 말이지만, 이는 **종속성을 가진 객체들이 올바른 순서로 파괴**될 수 있도록 합니다. <br>
> 더 많은 내용은 “[스택 프레임](/posts/%EC%8A%A4%ED%83%9D-%ED%94%84%EB%A0%88%EC%9E%84(stack-frame)%EA%B3%BC-%EC%BD%9C%EC%8A%A4%ED%83%9D(call-stack)/){: }”으로 찾아볼 수 있습니다. <br>
{: .prompt-tip}

## 정리

정리하자면 <span class="keyword">**RAII**</span>는 <span class="font_highlight">**객체 지향 프로그래밍에서, 자원의 획득과 소멸을 객체의 수명과 밀접하게 두어 자원의 관리를 자동화**</span>하여 메모리 누수를 방지할 수 있는 패러다임입니다.

- **객체가 생성**될 때, **자원을 할당**합니다.
- **객체가 소멸**될 때, **자원을 해제**합니다.

RAII는 자원의 생명 주기를 관리해야 할 때 주로 사용하는 패러다임입니다. 즉, 모든 클래스에 적용할 필요도 없고, 자원 관리가 필요한 모든 클래스에서 RAII를 따를 필요는 없습니다.

RAII는 자원의 생명 주기를 객체의 생명 주기와 연관되도록 만들고 싶을 때 사용하면 됩니다.

이런 패러다임과 밀접하게 관련되어 있는 클래스가 바로 스마트 포인터입니다.

# RAII와 스마트 포인터

[C++의 스마트 포인터](/posts/%EC%8A%A4%EB%A7%88%ED%8A%B8-%ED%8F%AC%EC%9D%B8%ED%84%B0-unique-ptr-shared-ptr-weak-ptr-auto-ptr/){: target="_blank"}는 RAII 패러다임을 따릅니다.

**객체의 생성을 스마트 포인터라는 클래스 객체와 수명을 밀접하게 연결**시켜 자원의 수명을 자동으로 관리하는 포인터입니다.

`unique_ptr`이나 `shared_ptr`은 `make_unique()`나 `make_shared()` 함수를 사용하여 객체 생성과 동시에 메모리가 할당되고, 스마트 포인터가 소멸될 때 자동으로 메모리를 해제합니다.

## UniquePtr 처럼 만들어보기

실제 클래스 템플릿인 스마트 포인터의 구현은 다르겠지만, **RAII 패러다임**을 사용해서 스마트 포인터처럼 만들면 다음과 같이 만들어 볼 수 있을 것 같습니다.

```cpp
#include <iostream>
#include <utility>
using namespace std;

template <typename T>
class CustomSmartPointer
{
public:
	// 생성자에서 자원을 할당합니다.
	CustomSmartPointer(T* ptr = nullptr) : ptr(ptr)
	{
		cout << *ptr << "번 자원을 할당합니다." << endl;
	}
	// 소멸자에서 자원을 해제합니다.
	~CustomSmartPointer()
	{
		if(ptr != nullptr)
		{
			cout << *ptr << "번 자원을 해제합니다." << endl;
			delete ptr;
		}
	}

	// 복사 생성자와 대입 연산자를 삭제합니다.
	CustomSmartPointer(const CustomSmartPointer&) = delete;
	CustomSmartPointer& operator=(const CustomSmartPointer&) = delete;

	// 이동 생성자와 이동 대입 연산자로 소유권 이전을 정의합니다.
	CustomSmartPointer(CustomSmartPointer&& other)
		: ptr(other.ptr)
	{
		other.ptr = nullptr;
	}
	CustomSmartPointer& operator=(CustomSmartPointer&& other)
	{
		if (this != &other)
		{
			delete ptr;
			ptr = other.ptr;
			other.ptr = nullptr;
		}
		return *this;
	}

	// 포인터처럼 사용할 수 있도록 * 연산자와 -> 연산자를 오버로딩합니다.
	T& operator*() { return *ptr; }
	T* operator->() { return ptr; }

private:
	T* ptr;
};

void test()
{
	CustomSmartPointer<int> p1(new int(1)); // p1이 자원을 할당합니다.
	
	CustomSmartPointer<int> p2(move(p1)); // p2로 소유권을 이전합니다.
	// CustomSmartPointer<int> p3 = p2; // 컴파일 에러! 복사 불가능
	CustomSmartPointer<int> p3(new int(2));

	cout << *p2 << endl; // 포인터처럼 사용.
}

int main()
{
	test();
}
```

![RAII와 스마트포인터_01](https://i.postimg.cc/HxXVYxhK/RAII-01.png){: w="336" h="146"}

실제로 자원의 해제는 클래스 템플릿의 소멸자에서 정의한 것이 전부입니다.

사용자의 입장에서 보면 자원의 할당에만 신경 쓰고 스택의 소멸자에서 자동으로 자원을 해제 해주고 있는 모습을 볼 수 있습니다.

더불어 함수를 빠져나갈 때, 할당된 순서의 역순으로 해제되는 것도 볼 수 있습니다.

### 자원의 생성과 소멸

자원의 생성과 소멸은 각각 객체의 생성자와 소멸자에서 해주었습니다.

객체 생성자의 Initializer에서 할당을 받아 저장하고, 소멸자에서 `nullptr`이 아니라면 자원도 같이 해제해 줍니다.

```cpp
// 생성자에서 자원을 할당합니다.
CustomSmartPointer(T* ptr = nullptr) : ptr(ptr)
{
	cout << *ptr << "번 자원을 할당합니다." << endl;
}
// 소멸자에서 자원을 해제합니다.
~CustomSmartPointer()
{
	if(ptr != nullptr)
	{
		cout << *ptr << "번 자원을 해제합니다." << endl;
		delete ptr;
	}
}
```

`shared_ptr`은 객체의 복사 생성자와 복사 대입 연산자를 열고, 객체에 **참조 카운트(reference count)**를 두어, 복사될 때 참조 카운트를 증가시키고, 소멸될 때 참조 카운트를 감소시키다가 0이 되면 자원을 해제하도록 만들면 됩니다.

이렇게 만들면 아마 순환 참조 문제도 재현해볼 수 있을 겁니다.

---

# 마무리

이처럼 <span class="keyword">**RAII**</span>는 **자원을 안전하고 효율적으로 관리할 수 있게 도와주는 중요한 패러다임**입니다.

이 패러다임은 객체 지향 프로그래밍에서 객체의 생명 주기와 자원의 생명 주기를 밀접하게 연관시켜, 자원의 할당 및 해제를 자동화 해줄 수 있습니다.

이로인해 사용자 입장에서는 자원 관리를 단순하게 할 수 있고, 메모리 누수와 같은 문제를 예방하면서 코드의 안정성을 높일 수 있습니다.

---

# 참고

[스마트 포인터 (unique_ptr, shared_ptr, weak_ptr, auto_ptr)](/posts/%EC%8A%A4%EB%A7%88%ED%8A%B8-%ED%8F%AC%EC%9D%B8%ED%84%B0-unique-ptr-shared-ptr-weak-ptr-auto-ptr/){:target="_blank"}