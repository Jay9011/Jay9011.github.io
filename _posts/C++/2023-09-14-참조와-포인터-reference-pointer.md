---
title: 참조와 포인터 (Reference & Pointer)
date: 2023-09-14 22:58:56 +0900
categories:
  - C++
  - 풀스택 개발
tags:
  - c
  - 데이터
  - 메모리
  - 포인터
  - 변수
  - 참조
keywords:
  - Pointer
  - Reference
  - 참조
  - 포인터
---

# 포인터(Pointer)란?

<span class="keyword">**포인터(Pointer)**</span>란, <span class="font_highlight">**메모리의 주소를 저장하는 변수**</span>입니다.

즉, **포인터는 초기화되지 않아도 되며** (**`nullptr`** 가능), **필요한 주소를 그때그때 저장**할 수 있습니다.

또한, 포인터는 산술 연산을 통해 주소 값을 자료형의 크기만큼 더하거나 뺄 수도 있습니다.

## 특징

- 포인터는 **다른 변수의 주소를 저장**할 수 있습니다.
- 포인터는 `NULL` 값을 가질 수 있습니다.
- 포인터는 **산술 연산**이 가능합니다. (자료형의 크기만큼 주소를 더하거나 뺄 수 있습니다.)

## 사용법

```cpp
Type* pointerName;
```

## 주의점

- 초기화되지 않은 포인터를 사용하거나 객체가 삭제된 주소를 그대로 사용하면 [댕글링 포인터](/posts/%ED%97%88%EC%83%81-%ED%8F%AC%EC%9D%B8%ED%84%B0-dangling-pointer-%EB%B0%9C%EC%83%9D-%EC%9B%90%EC%9D%B8%EA%B3%BC-%EC%98%88%EB%B0%A9-%EB%B0%A9%EB%B2%95/){: target="_blank"}와 같은 문제가 발생할 수 있습니다.

---

# 참조 (Reference)란?

<span class="keyword">**참조(Reference)**</span>는 <span class="font_highlight">이미 존재하는 객체나 함수의 **별칭(alias)**</span>을 생성하는 C++의 기능입니다.

따라서, 참조를 초기화 할 때에는 반드시 **유효한 객체나 함수를 참조**하도록 해야하며, 참조자의 타입은 대상이 되는 **변수의 타입과 일치**해야 합니다.

참조는 **단 한 번만 초기화** 되며, **대상 변수와 같은 메모리 위치를 참조**합니다.

> C++에서 참조 변수를 선언할 때 사용되는 & 기호는 포인터의 참조와 같은 기호를 사용하고, 참조란 단어는 포인터에서도 "참조"와 "역참조"로 쓰여 헷갈릴 수 있습니다.
>
> 하지만, 포인터에서의 참조와 참조 변수의 참조는 조금 다릅니다.
> 
> [포인터 (Pointer)에서의 참조 역참조](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){:target="_blank"}
>
> 참조 변수에서의 "참조"는 **원본 변수를 "참조하는" 별칭을 만드는 것**을 의미합니다.
>
> 이는, 한글 단어에서의 참조로 사용해도 될 것 같기도 하고, 'reference'의 뜻인 가리키다, 언급, 참고와 같은 뜻으로도 사용해도 될 것 같습니다.
> 
{: .prompt-tip}

## 특징

- 참조는 **선언과 동시에 초기화** 되어야 합니다.
- 참조는 **한 번만 초기화** 할 수 있으며, 변경할 수 없습니다.
- 참조자의 타입은 대상이 되는 **변수의 타입과 일치**해야 합니다.
- 참조는 `NULL` 값을 가질 수 없습니다.
- 참조는 대상 변수와 같은 메모리 위치를 참조하여 원본을 가지고 작업할 수 있습니다.

## 사용법

```cpp
Type& referenceName = variable;
```

이때의 `&` 연산자는 주소 연산자가 아닌 데이터형 식별자로 사용된 것입니다.

---

# 참조 사용시 임시 변수에 의한 주의 사항

참조는 원래 **<span class="important">lvalue</span>**(변수 등을 가리키는 표현식)만을 대상으로 할 수 있습니다.

만약 **<span class="important">rvalue</span>**(임시 값이나 리터럴 등)를 참조하려고 하면, 컴파일러가 **<span class="important">임시 변수</span>**를 만들어 이를 참조하게 됩니다.

이런 경우, 원래 의도와 다르게 동작할 수 있습니다.

## 1. 비const 참조에 대한 초기 값은 lvalue여야 합니다.

예를 들어, 참조 타입 객체를 인자로 받아서 비 const 함수를 호출한다고 해봅시다.

```cpp
#include <iostream>
using namespace std;

class MyClass
{
public:
	void NonConstFunction(){}
};

void FunctionByReference(MyClass& obj)
{
	obj.NonConstFunction();
}

int main()
{
	MyClass myC = MyClass();
	FunctionByReference(myC); // 가능

	FunctionByReference(MyClass()); // 컴파일러 문제 발생
}
```

### 상황의 이해

1. 특정 **함수가 참조를 매개변수**로 받습니다.
2. 이 함수 내에서 해당 **참조 객체의 멤버 함수를 호출**합니다.
3. 이 **멤버 함수는 `const`가 아닙니다**. 즉, 객체를 변경할 수 있는 함수로 생각할 수 있습니다.

### • 문제 상황 1: 비const 일반 함수 호출

함수의 인자로 생성자를 바로 사용하는 경우, 컴파일러는 <span class="important">**임시 객체**</span>를 만들고 **인자로 전달**합니다. (<span class="important">**rvalue**</span>)

이때, 호출하려는 함수는 비const이고 매개 변수를 참조로 받아야 하는데, 인자가 임시 객체이므로 문제가 발생합니다.

왜냐하면, **임시 객체는 일반적으로 변경할 수 없다고 가정 되기 때문**입니다.

임시 객체는 일반적으로 함수 호출이 끝날 때 소멸됩니다. 따라서 임시 객체를 넘겨봤자 함수 호출이 끝난 다음에는 재사용 할 수 없기 때문에 이를 허용하지 않습니다.

따라서, 함수의 매개 변수를 <span class="important">**const 참조로 호출**</span>하면 이 함수를 호출할 수 있습니다.

```cpp
void FunctionByReference(const MyClass& obj)
{
	obj.NonConstFunction(); // 컴파일러 문제 발생
}
```

### • 문제 상황 2: 호환되지 않는 형식 한정자

이렇게 const 참조로 매개변수를 호출하면 임시 변수를 참조로 사용할 수도 있습니다.

그러나 비 const 멤버 변수를 호출하려고 하면 **호환되지 않는 형식 한정자**라며 컴파일 에러가 발생합니다.

이때, 같이 나오는 메시지가 아래와 같은데,

```bash
오류 C2662: 'void MyClass::NonConstFunction(void)': 'this' 포인터를 'const MyClass'에서 'MyClass &'(으)로 변환할 수 없습니다.
```

**`this`** 포인터는 객체 자신을 가리키는 포인터입니다.

현재 `FunctionByReference()` 내에서는 **`const MyClass`** 타입이지만, **`const`**가 아닌 멤버 함수를 호출하기 위해서는 const가 아닌 **`MyClass`** 타입이어야 한다는 뜻입니다.

이는 <span class="font_highlight">**`const`** 객체는 변경되지 않아야 한다는 규칙</span>을 지키기 위한 것입니다.

**`const`**가 아닌 멤버 함수는 객체의 상태를 변경할 가능성이 있기 때문에, **`const`** 객체에 대해 호출할 수 없습니다. 따라서 <span class="font_highlight">**`const`**인 **`this`** 포인터는 **`const`** 멤버 함수만 호출</span>할 수 있습니다.

정리하자면, <span class="font_highlight">**`const`** 객체는 변경할 수 없으므로, 이를 변경할 수 있는 **const가 아닌 멤버 함수**를 호출할 수 없습니다</span>.

### ▷ 최종 수정 결과

즉, 아래 코드와 같이 변경하면, **임시 변수로 `const 함수`**를 호출하게 됩니다.

```cpp
#include <iostream>
using namespace std;

class MyClass
{
public:
	void NonConstFunction() const
	{
		cout << "NonConst 함수 호출" << endl;
	}
};

void FunctionTakingReference(const MyClass& obj)
{
	obj.NonConstFunction();
}

int main()
{
	FunctionTakingReference(MyClass());
}
```

![NonConst 함수 호출](https://i.postimg.cc/bNHk06LM/Non-Const.png){: w="333" h="85"}

이와 같이 임시 변수를 참조로 사용하기 위해서는 const와 관련된 상황을 주의해야 합니다.

---

# 참조 사용시 주의 사항

- <span class="important">**임시 변수 문제**</span>: 참조는 원본을 전달하기 때문에, **참조가 가능한 lvalue가 아니거나, 데이터형이 달라 형변환을 하게 될 때, 임시 변수를 생성**할 수도 있어, 함수가 의도한 대로 동작하지 않을 수 있습니다.
- <span class="important">**함수 종료와 참조**</span>: **함수가 종료되면서 수명이 끝나는 메모리** 위치에 대한 참조를 리턴하지 않도록 해야 합니다.

---

# 참조와 포인터의 차이점

- **`NULL` 값**: 포인터는 `NULL` 값을 가질 수 있지만, 참조는 가질 수 없습니다.
- **변경 가능**: 포인터는 주소를 마음대로 변경할 수 있지만, 참조는 초기화 이후 변경할 수 없습니다.
- 로우 레벨에서 볼 때, 둘 다 메모리 주소를 사용해 객체에 접근하기 때문에 동작의 차이가 거의 없습니다.
    - 하지만, C++ 차원에서 참조는 "**안전성**"과 "**가독성**"을 높이기 위해 한 번 초기화 이후 대상을 바꿀 수 없도록 되어있습니다.



---

# 참고

[포인터 (Pointer)](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){:target="_blank"}

[허상 포인터 (Dangling pointer) 발생 원인과 예방 방법](/posts/%ED%97%88%EC%83%81-%ED%8F%AC%EC%9D%B8%ED%84%B0-dangling-pointer-%EB%B0%9C%EC%83%9D-%EC%9B%90%EC%9D%B8%EA%B3%BC-%EC%98%88%EB%B0%A9-%EB%B0%A9%EB%B2%95/){:target="_blank"}

[스마트 포인터 (unique_ptr, shared_ptr, weak_ptr, auto_ptr)](/posts/%EC%8A%A4%EB%A7%88%ED%8A%B8-%ED%8F%AC%EC%9D%B8%ED%84%B0-unique-ptr-shared-ptr-weak-ptr-auto-ptr){:target="_blank"}