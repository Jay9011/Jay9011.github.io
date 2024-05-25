---
title: (상수) const 와 constexpr
date: 2023-09-05 22:06:42 +0900
categories:
  - Full Stack 개발
  - C++
tags:
  - c
  - const
  - constexpr
  - 자료형
  - 변수
keywords:
  - const
  - constexpr
  - 상수
  - 클래스
  - 포인터
  - 함수
---

**`const`**는 <span class="font_highlight">**상수를 선언하는 제한자(qualifier)**</span>입니다. 즉, 한 번 초기화된 **`const`** 변수의 값은 변경할 수 없습니다.

`#define`으로 상수처럼 사용할 수 있으나, **`const`**로 선언하는 경우에는 데이터형을 명시적으로 지정 할 수 있고, 활동 범위 규칙으로 특정 함수나 파일에서만 사용할 수 있도록 제한할 수 있습니다.

```cpp
const int x = 10;  // x는 10으로 초기화되고 이후에 변경할 수 없습니다.
```

const 키워드는 포인터, 레퍼런스, 멤버 변수, 멤버 함수, 지역 변수에 붙을 수 있습니다.

---

# 포인터에서의 const

포인터와 함께 사용될 때 **`const`**의 위치에 따라 의미가 달라집니다.

<span>(<span class="important">**const**</span>) int (<span class="important">**const**</span>) * (<span class="important">**const**</span>) ptr;</span>
{: .text-center}


## 1. **포인터가 가리키는 값이 상수인 경우**

<span class="important">**const int**</span> * ptr, <span class="important">**int const**</span> * ptr
{: .text-center}

**별 앞에 const**가 붙는 경우, 상수형 데이터를 가리키는 포인터로, <span class="font_highlight">**포인터가 가리키는 값을 변경할 수 없습니다**</span>. <span class="small">(**const int** 와 **int const** 는 같습니다.)</span>

<span class="important">단, 포인터 주소는 변경 가능합니다.</span>

```cpp
const int* ptr = &x;  // ptr은 상수를 가리키는 포인터입니다.
*ptr = 20;            // 불가능: ptr이 가리키는 값은 변경할 수 없습니다.
ptr = &y;             // 가능: ptr의 메모리 주소를 변경할 수 있습니다.
```

## 2. **포인터 자체가 상수인 경우**

**int** <span class="important">*** const**</span> ptr
{: .text-center}

**별 뒤에 const**가 붙는 경우, 포인터(int*)를 상수화 시키므로 <span class="font_highlight">**포인터가 가리키는 메모리 위치를 변경할 수 없습니다**</span>.

<span class="important">단, 데이터 값은 변경할 수 있습니다.</span>

```cpp
int x = 5, y = 10;
int* const ptr = &x;  // ptr은 상수 포인터입니다.
*ptr = 20;            // 가능: ptr이 가리키는 값은 변경할 수 있습니다.
ptr = &y;             // 불가능: ptr의 메모리 주소를 변경할 수 없습니다.
```

## 3. 포인터와 가리키는 값 모두 상수인 경우

<span class="important">**const int * const**</span> ptr
{: .text-center}

const는 포인터의 앞과 뒤에 두 개 올 수 있습니다. 이 때에는 상수형 데이터를 가리키는 포인터를 상수화 시킨 것으로, <span class="font_highlight">**주소와 값 둘 다 변경할 수 없습니다**</span>.

```cpp
**const int* const ptr = &x;**
```

---

# 멤버 함수에서의 const

function () <span class="important">**const**</span>
{: .text-center}

멤버 함수 뒤에 **`const`**를 붙이면 <span class="font_highlight">**해당 함수 내에서 멤버 변수를 수정할 수 없게 됩니다**</span>. 이를 통해 **읽기 전용 멤버 함수**를 만들 수 있습니다. 

이 때, 선언부와 정의부 둘 다 **`const`**가 써있어야 합니다.

---

# constexpr 이란?

**`constexpr`**는 C++11부터 도입된 키워드로, <span class="font_highlight">**컴파일 타임에 상수 표현식**</span>을 계산할 수 있게 해줍니다.

# constexpr 변수

**`constexpr`**은 컴파일 타임에 계산되는 표현식이므로, 변수 선언과 동시에 초기화 될 수 있는 <span class="important">**리터럴 타입(LiteralType)**</span>이어야 합니다.

```cpp
constexpr int x = 10 * 10; // 컴파일 타임에 100으로 계산됩니다.
```

## 클래스에서의 사용

조금 특이한 것은 **클래스 타입**도 **리터럴 타입**이 될 수 있다는 것입니다.

리터럴 타입 클래스의 조건은 조금 복잡할 수 있지만, 정리하자면 다음과 같습니다.

- **`constexpr 생성자`**를 가지고 있어야 합니다.
- **소멸자**가 컴파일러에 의해 **자동**으로 생성되거나, **디폴트(default) 소멸자**여야 합니다.
- **모든 멤버 변수**는 **리터럴 타입**이어야 합니다.
- 상속 받지 않아야 합니다.

이런 조건을 만족한다면, 클래스에도 constexpr을 붙일 수 있습니다.

## constexpr의 특이한 사용

**`constexpr`**은 **컴파일 타임 상수**이고, 클래스에도 constexpr을 붙일 수 있으므로 다음과 같은 특이한 코드도 가능하게 됩니다.

```cpp
#include <iostream>
using namespace std;

class MyClass
{
public:
	int x; // 리터럴 타입
	constexpr MyClass(int val) : x(val) {} // constexpr 생성자
	~MyClass() = default; // 디폴트(default) 소멸자
};

int main()
{
	constexpr MyClass obj1(10); // 컴파일 타임에 생성
	int arr1[obj1.x] = { 0 }; // 컴파일 타임에 크기 결정

	MyClass obj2(20); // 런타임에 생성
	// int arr2[obj2.x] = { 0 }; // obj2는 런타임에 생성되므로, 컴파일 타임 상수로 사용할 수 없습니다.

	cout << "\n컴파일 타임 상수로 배열 크기 결정\n";
	for (auto& n : arr1)
	{
		cout << n << ' ';
	}
	cout << "\n\n";
}
```

![constexpr의 특이한 사용](https://i.postimg.cc/cC3MN0R4/constexpr.png){: w="324" h="139"}

---

# constexpr 함수

**`constexpr`**는 함수에서도 사용할 수 있습니다. 그 경우, 해당 함수는 컴파일 타임에 실행 가능한 함수여야 합니다.

```cpp
constexpr int square(int x)
{
    return x * x;
}
```

## constexpr의 특이한 사용

다만, **`constexpr`** 함수를 사용하려는 경우, 관련된 선언에도 **`constexpr`** 지정자가 있어야 합니다.

그리고 **`constexpr`**은 **컴파일 타임 상수**이기 때문에, 그 반환 값도 **컴파일 타임 상수**가 되고, 따라서 다음과 같은 사용도 가능합니다.

```cpp
#include <iostream>
using namespace std;

constexpr int square(int a)
{
	return a * a;  // 컴파일 타임에 계산됩니다.
}

int main()
{
	constexpr int a = 1;
	constexpr int b = 2;
	int arr[square(a * b + 10)] = { 0 };  // 컴파일 타임에 계산됩니다.

	cout << "\n컴파일 타임 상수로 배열 크기 결정\n";
	int i = 0;
	for (auto& n : arr)
	{
		if (i > (a * b + 10))
		{
			i = 0;
			cout << '\n';
		}
		cout << n << ' ';
		i++;
	}
	cout << "\n\n";
}
```

![constexpr의 특이한 사용2](https://i.postimg.cc/m2xyy14c/constexpr-2.png){: w="325" h="321"}