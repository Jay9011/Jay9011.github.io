---
title: "[STL] vector"
date: 2023-11-25 22:21:54 +0900
categories:
  - 컴퓨터 공학
  - 자료구조
tags:
  - 시퀀스 컨테이너
  - 자료구조
  - c
math: true
type: DataStructure
keywords:
  - push_back
  - sequence container
  - vector
  - 시퀀스 컨테이너
  - emplace_back
---

# 개요

- 헤더 파일: `<vector>`
- STL 시퀀스 컨테이너 (sequence container): 선형 자료구조
- 탐색 시간: $O(1)$ (임의 접근)
- 요소 추가/삭제
    - $O(1)$ (재할당 하는 경우를 제외하고 배열의 마지막 위치에 요소를 추가하거나 삭제하는 경우)
    - $O(N)$ (중간에 삽입/삭제를 하는 경우 요소를 이동 시켜야 함, 혹은 재할당을 해야하는 경우)

<span class="keyword">**std::vector**</span>는 C++ STL의 일부로, **<span class="font_highlight">크기가 동적으로 변경</span>될 수 있는 동적 배열**입니다.

<span class="important">**동적 배열**</span>이란, **프로그램 실행 중에도 크기가 변할 수 있는 배열**을 말합니다.

따라서, 동적 배열인 벡터는 더 큰 크기의 배열이 필요할 때, 내부적으로 새 배열을 할당하고 모든 요소를 이동시키는 과정이 자동으로 이루어지는 배열입니다.

---

# 메모리 사용과 할당 전략

동적 배열인 벡터는 결국 물리적으로 뭔가 새로운 혁신적인 기능이 아니라, 프로그래머가 일일이 새롭게 할당하고 복사하던 과정을 자동으로 해주는 객체입니다.

따라서, 배열의 크기가 늘어나야 할 때, 새 배열을 할당하고 모든 요소를 이동시키는 과정의 비용은 결국 비싼편입니다.

이를 해결하기 위해(성능을 높이기 위해) 벡터는 요소가 추가될 때마다 메모리를 재할당 하기보다, 미리 사용 공간이 커질 것으로 예상되는 만큼 충분한 메모리 공간을 미리 예약(할당)합니다.

이를 <span class="important">**capacity**</span>라고 하는데, 실제 할당된 공간이 필요한 것보다 클 수 있습니다. 즉, 메모리 공간을 어느 정도 희생하고 성능을 높이는 편입니다.

이 과정에서 capacity는 기존 용량의 **약 1.5배에서 2배** 정도로 증가하는 것이 일반적입니다

![capacity와 size](https://i.postimg.cc/ZnLPqjM1/vector-01.png){: w="690" h="398"}

# 특징

1. <span class="important">**동적 배열**</span>: 벡터의 저장 공간은 **프로그램 실행중에 자동으로 관리**되며, 필요에 따라 확장됩니다. 물론, 배열과 마찬가지로 요소에 대한 임의 접근이 가능합니다.
2. <span class="important">**메모리 사용과 성장 전략**</span>: 벡터는 내부적으로 벡터의 크기가 성장할 가능성을 염두에 두고 **충분한 메모리 공간을 미리 예약(할당)**합니다.
    - 물론, 삽입 삭제가 빈번하여 재할당을 반복하는 경우 성능이 좋을 수 없습니다. 따라서, 삽입 삭제가 빈번한 경우, 사용 용도에 따라 다른 자료구조를 사용하는 것이 더 좋을 수 있습니다.
3. <span class="important">**컨테이너** </span>특성: 벡터는 STL 시퀀스 컨테이너이며, 따라서 **STL의 이터레이터와 다양한 알고리즘을 사용**할 수 있습니다.

## 장단점

### 장점

- <span class="important">**동적 배열**</span>: 저장 공간이 자동으로 관리되며, 필요에 따라 확장됩니다. `array`에 비해 유연합니다.
- <span class="important">**임의 접근**</span>: 벡터는 `list`와는 다르게 인덱스를 통한 접근이 가능합니다.
- <span class="important">**캐시 효율**</span>: `list`나 `deque`에 비해 메모리가 연속적이므로 캐시 효율이 좋습니다.

### 단점

- 성능을 위해 요소가 추가 할당될 경우를 대비하여 <span class="important">**저장 공간을 크게 할당**</span>하기 때문에, 메모리의 낭비가 있을 수 있습니다.
    - {: .small } 물론, 사용할 최대 크기를 미리 예상할 수 있다면 메모리 낭비를 줄일 수 있습니다.
- **중간 위치에 요소를 삽입/삭제** 해야 하는 경우, `list`와 비교하여 <span class="important">**데이터를 옮겨줘야 하는 단점**</span>이 있습니다.
    - {: .small} 또한, <span class="important">**메모리 재할당**</span>이 이루어질 수 있습니다. `deque`와 비교하여, `deque`는 새로운 블록을 새로 할당하기만 하면 될 경우(특히, 양 끝에서 추가/삭제하는 경우)가 있어 `deque`보다 비효율적일 수 있습니다.

---

# 사용법

### 생성자

`std::vector`는 템플릿 매개변수에 자료형을 넣는 것이 기본 형태입니다. 여기에 미리 크기를 지정하거나 초기 값을 지정해줄 수 있습니다.

`std::vector`의 생성자는 여러가지 형태의 오버로딩이 존재하지만 다음 4개의 형태만 기억하셔도 괜찮을 것 같습니다.

```cpp
std::vector<typename _Ty> v();
std::vector<typename _Ty> v(size_t _Size);
std::vector<typename _Ty> v(size_t _Size, _Ty& _Val);
std::vector<typename _Ty> v(vector& _Right)
```

### 다차원 배열

벡터 또한 배열이기 때문에, 기존에 쓰던 다차원 배열을 생성할 수 있습니다.

기존의 배열 $arr[2][arr[3][arr[4]]]$는 요소 ‘4’개짜리 1차원 배열을 ‘3’개짜리 2차원 배열로 만들고, 이 요소로 ‘2’개짜리 3차원 배열을 만듭니다.

이를 `std::array`에서 만들었을 때, 이렇게 만들었습니다.

```cpp
std::array<std::array<std::array<int, 4>, 3>, 2>
```

`std::vector`도 비슷합니다.

이번에는 범위 기반 for문으로 간단하게 만들어 보겠습니다.

```cpp
#include <iostream>
#include <vector>

int main()
{
	std::vector<std::vector<std::vector<int>>> v(2, std::vector<std::vector<int>>(3, std::vector<int>(4)));

	// 배열의 각 요소에 값을 할당하는 부분
	int value = 0;
	for (auto& dim1 : v)
	{
		for (auto& dim2 : dim1)
		{
			for (auto& dim3 : dim2)
			{
				dim3 = value++;
			}
		}
	}

	// 배열의 각 요소를 출력하는 부분
	for (auto& dim1 : v)
	{
		for (auto& dim2 : dim1)
		{
			for (auto& dim3 : dim2)
			{
				std::cout << dim3 << " ";
			}
			std::cout << std::endl;
		}
		std::cout << std::endl;
	}
}
```

![다차원 vector 결과](https://i.postimg.cc/sggmMdyK/array-01.png){: w="161" h="145"}

위 코드를 설명하자면, 우선 자료구조 부분은 다차원 배열 array와 마찬가지로 `std::vector<int>`를

자료형으로써 중첩시킨 모습입니다.

이후, 초기화 부분은 `_Ty& _Val`를 사용해 `std::vector<int>(_Size, _Val)`을 값으로 중첩시킨 모습입니다.

---

# push_back()과 emplace_back()

멤버 함수 편에서 함수를 다시 정리하겠지만, 벡터에는 **`push_back()`**과 **`emplace_back()`**이라는 요소 삽입 멤버 함수가 존재합니다.

둘의 차이는 매개변수의 형식도 존재하지만, 무엇보다 **`emplace_back()`**은 **perfect forwarding**을 이용하여 객체를 벡터 내에서 직접 생성한다는 점입니다.

(perfect forwarding 혹은 `std::forward<_Valty>(_Val)`에 대한 내용은 이 포스트에서 자세히 다루지 않습니다.)
{: .small}

여기에서 중요한 것은 **`emplace_back()`**은 **perfect forwarding**을 이용하여 <span class="font_highlight">객체를 벡터 내에서 직접 생성</span>하기 때문에, **`emplace_back()`**에는 내가 정의한 벡터의 Type인 `typename _Ty`의 생성자 형식에 맞춰야 한다는 점입니다.

자세한 내용은 아래 이어가겠습니다.

```cpp
void push_back(const _Ty& _Val)
void push_back(_Ty&& _Val)
decltype(auto) emplace_back(_Valty&&... _Val)
```

## push_back()

`push_back`의 두 오버로드 중 하나는 l-value 참조를 위한 것이고, 다른 하나는 r-value 참조를 위한 것입니다.

만약, r-value 참조를 사용하는 경우에는 다음과 같이 동작합니다.

1. **임시 MyType 객체 생성**: `vec.push_back(MyType(10));` 같은 표현식에서 MyType의 임시 객체를 생성합니다.
2. **이동 생성자 호출**: `push_back`은 이 임시 MyType 객체를 r-value 참조로 받습니다. MyType에 이동 생성자가 정의되어 있다면, 이동 생성자가 호출되어 임시 객체의 데이터가 벡터의 새 요소로 이동됩니다.
    - 만약 이동 생성자가 없거나 삭제된 경우, 복사 생성자가 대신 사용됩니다.
3. **벡터에 요소 추가**: 이동 생성자(또는 복사 생성자)를 통해 생성된 MyType 객체가 벡터에 추가됩니다.
4. **임시 객체 소멸**: `push_back`에서 빠져나온 후, 임시 객체는 소멸됩니다.

## emplace_back()

`emplace_back` 멤버 함수는 **perfect forwarding**을 사용합니다.

이 멤버 함수는 <span class="font_highlight">**가변 인자 템플릿**을 사용하여 임의의 수의 인자를 받아, 이를 직접 객체의 생성자에 전달</span>합니다.

즉, 벡터의 데이터 타입 생성자에 전달하는 매개변수 역할을 한다는 점입니다.

예를 들어, 다음과 같은 코드가 있다고 생각해 보겠습니다.

```cpp
#include <iostream>
#include <vector>

class MyType
{
public:
	MyType() {}
	MyType(int n) : num(n) {}
	MyType(int n, std::string str) : num(n), name(str) {}
	~MyType() {}

	int getNum() const { return num; }

private:
	int num;
	std::string name;
};

int main()
{
	std::vector<MyType> v2(3, 0);
	v2.emplace_back(10); // emplace_back의 사용법
}
```

1. **멤버 함수 호출**: `v2.emplace_back(10);`은 기본 데이터 타입의 인자를 전달합니다. (호출 비용 감소)
2. **직접 생성자 호출 및 벡터에 요소 추가:** 멤버 함수 내에서 벡터의 타입에 맞는 객체를 직접 생성하고 벡터에 추가합니다. 이 과정에서 복사나 이동이 발생하지 않습니다.

## 몇 가지 주의점

두 함수의 차이는 외부에서 객체를 생성해 전달(이동)하느냐, 내부에서 생성하느냐의 차이입니다.

즉, 이동 생성자가 비용이 많이 드는 객체를 다룰 때, 이동 생성자를 한 번 더 호출하는 편 보다 **`emplace_back()`**의 성능이 좋을 수 있다는 것입니다.
(만약, 이동 생성자의 비용이 적으면 **`push_back`**과 **`emplace_back`** 사이의 성능 차이는 미미할 수 있습니다.)

이 특징에서 몇 가지 주의점이 발생합니다.

### 1. 객체를 생성해 전달하는 경우

우선, `push_back()`보다 `emplace_back()`이 더 빠르다고 할 수 있지만, 아래와 같이 사용하면 차이가 없을 수 있습니다.

```cpp
v2.emplace_back(MyType(10));
// 또는
MyType m(10);
v2.emplace_back(m);
```

위에서 말했듯이 `emplace_back()`은 객체를 내부에서 생성하기 때문에 빠른 편입니다.

하지만, 위 예시처럼 외부에서 객체를 생성하는 경우에는 아래 코드와 비슷해지기 때문에, 객체를 생성해서 넘겨주기보다는 직접 생성할 수 있도록 인자를 넘겨주는 편이 좋습니다.

```cpp
MyType m(MyType(10)); // 이런 식으로 생성하는 것과 비슷하다
```

### 2. 인자를 잘못 전달하는 경우

우선, `emplace_back()`은 가변 인자 템플릿을 사용하여 **임의의 수의 인자**를 받아, 이를 직접 객체의 생성자에 전달합니다.

따라서, `emplace_back(_Ty);` 안에는 어떤 인자든 들어갈 수 있지만, 벡터 자료형의 생성자와 일치하지 않는 인자를 넘기는 경우에는 오류가 발생할 수 있습니다.

```cpp
// ...
// MyType의 생성자가 다음과 같은 두 경우만 있다고 한다면...
MyType(int n) : num(n) {}
MyType(int n, std::string str) : num(n), name(str) {}
// ...
std::vector<MyType> v2(3, 0);
v2.emplace_back(1, 2);
v2.emplace_back(3, 4, 5);
```

![emplace_back을 잘못 쓴 경우](https://i.postimg.cc/RZpRfsZm/vector-03.png){: w="519" h="76"}

즉, **<span class="font_highlight">생성자 매개변수의 타입에 맞춰</span> 써야합니다**.

```cpp
std::vector<MyType> v2(3, 0);
v2.emplace_back(1, "One");

for (auto& elem : v2)
{
	std::cout << elem.getNum() << ' ' << elem.getName() << std::endl;
}
```

![정상적으로 emplace_back()을 쓴 결과](https://i.postimg.cc/3RmCs3Cr/vector-02.png){: w="142" h="83"}
_정상적으로 `emplace_back()`을 쓴 결과_

---

# 멤버 함수

| 멤버 함수 | 설명 |
| --- | --- |
| vec.at(i) | i번째 원소에 대한 참조를 반환합니다. <br>범위 검사를 수행합니다. |
| vec[i] | i번째 원소에 대한 참조를 반환합니다. <br>범위 검사를 수행하지 않기 때문에 약간 더 빠를 수 있겠지만, 위험할 수 있습니다. |
| vec.reserve(n) | 최소한 n 개의 원소를 저장할 수 있도록 벡터의 용량을 늘립니다. |
| vec.size() | 벡터에 저장된 원소의 개수를 반환합니다. |
| vec.capacity() | 벡터가 할당한 전체 메모리 용량을 반환합니다. |
| vec.empty() | 벡터가 비어있는지 여부를 반환합니다. (비어있다면 `true` 반환) |
| vec.shrink_to_fit() | 메모리 사용을 현재 벡터 크기에 맞게 줄입니다 (C++11 이후). |
| vec.push_back(x) | 벡터의 끝에 x 값을 가진 원소를 추가합니다. |
| vec.emplace_back(x) | 벡터의 끝에 x 값을 가진 원소를 직접 추가합니다. <br>`push_back`과 유사하지만, 요소를 직접 생성하기 때문에 추가적인 복사나 이동이 없습니다. |
| vec.pop_back() | 벡터의 마지막 원소를 제거합니다. |
| vec.front() | 벡터의 첫 번째 원소에 대한 참조를 반환합니다. |
| vec.back() | 벡터의 마지막 원소에 대한 참조를 반환합니다. |
| vec.clear() | 벡터의 모든 원소를 제거합니다. (메모리는 그대로 유지됩니다.) |
| vec.assign(n, x) | 기존 요소들을 삭제하고, n 개의 새로운 요소로 대체합니다. <br>이 새로운 요소들은 모두 x 값으로 초기화됩니다. |
| vec.begin() | 벡터의 시작을 가리키는 이터레이터를 반환합니다. |
| vec.end() | 벡터의 끝(마지막 원소 다음)을 가리키는 이터레이터를 반환합니다. |
| vec.insert(iter, x) | iter가 가리키는 위치에 x 값을 삽입하고, 삽입된 위치의 이터레이터를 반환합니다. |
| vec.emplace(iter, x) | iter가 가리키는 위치에 새로운 요소를 직접 구성합니다. <br>`insert`와 비슷하지만, 요소를 직접 생성하기 때문에 추가적인 복사나 이동이 없습니다. |
| vec.erase(iter) | iter가 가리키는 위치의 원소를 제거하고, 다음 위치의 이터레이터를 반환합니다. |