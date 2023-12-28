---
title: "[L_Value & R_Value] 2. 우측 값 참조와 이동(move semantics)"
date: 2023-09-30 12:01:15 +0900
categories:
    - 프로그래밍
    - C++
tags:
    - c
    - lvalue
    - rvalue
    - 값 타입
    - 데이터
    - 메모리
    - 참조
    - 참조 타입
math: false
type: C++
keywords:
    - Move Semantics
    - R-value Reference
    - 우측 값 참조
    - 이동
    - 참조 축약 규칙
    - 완벽한 전달
---

저도 우측 값 참조(rvalue reference)를 어느 정도 이해했다고 생각했는데, 글로 표현하기 위해 다시 공부하고 정리하면서도 아직 어렵고 난해한 것 같습니다.

따라서 저는 제가 이해한 정의만 나열하기보다 우선 제가 이해한 과정이나 정도만을 글로 표현하고 정리 해둘까 합니다.

먼저, 우측 값 참조(rvalue reference)를 이해하기 위해서는 먼저 “이동”에 대해 알아보도록 하겠습니다.

# 이동(move semantics)

먼저, <span class="keyword">**이동**</span>에 대해서 찾다 보면 다음과 같은 설명을 볼 수 있습니다.

![복사와 이동](https://drive.google.com/uc?export=view&id=12JO2J_ubCnOe_jMha5zpyZMiPz86bFqb&usp=drive_fs){: w="500" h="339"}

<span class="important">**복사**</span>의 경우 **데이터를 복사하고 새로운 객체에 새롭게 생성된 데이터를 가리키**게 하고, 

<span class="important">**이동**</span>의 경우에는 **데이터를 복사하지 않으면서 가리키는 포인터만 바꾸는 것**입니다.

## 얕은 복사의 문제

만약, 아래와 같은 코드를 만든다고 해보겠습니다.

```cpp
#include <iostream>
#include <vector>
using namespace std;

class MyCharacter
{
public:
	MyCharacter()
	{
		data = new char[10];
		strcpy_s(data, 10, "Hello");
		
		cout << "Constructor: " << static_cast<void*>(data) << '\n';
	}

	~MyCharacter()
	{
		delete[] data;
		cout << "Destructor: " << static_cast<void*>(data) << '\n';
	}

public:
	char* data;
};

int main()
{
	vector<MyCharacter> characters;
	characters.push_back(MyCharacter());

	cout << characters[0].data << '\n';
}
```

위 코드는 단순히 생성자에서 data를 동적으로 생성하고 저장하는 클래스입니다.

이때, 이 클래스들을 관리하는 vector가 정의되어 있는데, 내가 원하는 동작은 “캐릭터를 하나 새로 생성해서 vector에 넣어 관리하고 싶다.”라고 합시다.

그래서 **임시 객체**인 `MyCharacter()`를 `vector`에 `push_back()`해준다면 어떤 문제가 발생할까요?

![얕은 복사 문제](https://drive.google.com/uc?export=view&id=1AsvJRgtfWzq23iAcDWoNubgQtA7uRBLX&usp=drive_fs){: w="317" h="127"}

위의 경우에는 이미 소멸자가 호출되어 실제 데이터를 출력하지 못하고 소멸됩니다.

더 큰 문제는 소멸 후에도 해당 위치를 다시 한 번 삭제하기 위해 `delete[] data;` 를 호출한다는 것입니다.

즉, 임시 객체는 `push_back()` 함수가 종료된 직후에 소멸되어 vector 안에는 올바른 데이터가 들어가지 못한다는 뜻입니다.


> `static_cast<void*>`는 `char*`를 `void*`로 형변환하여, **포인터가 가리키는 메모리의 주소를 출력**하게 합니다.
> 
> 만약, `&data`를 사용하게 되면 `data` 포인터 공간의 주소를 출력하기 때문에, 실제 동적 할당된 `new data[10]` 의 주소를 출력하는게 아닙니다.
>
{: .prompt-info}

## 깊은 복사가 필요하다

따라서 위 코드에 깊은 복사를 재정의 하면 이 문제는 해결될 수 있습니다.

```cpp
MyCharacter(const MyCharacter& other)
{
	data = new char[10];
	strcpy_s(data, 10, other.data);

	cout << "Copy Constructor: " << static_cast<void*>(data) << '\n';
}
```

![깊은 복사 결과](https://drive.google.com/uc?export=view&id=1AuNdGLHu70muiXzh3PSO0_QyPcgkcOgw&usp=drive_fs){: w="326" h="153"}

위 코드는 복사 생성자를 재정의 해서 깊은 복사를 하는 코드입니다.

이 코드만 추가하면, **`vector`**는 복사 생성자를 호출하고, **<span class="font_highlight">새로운 메모리 공간</span>을 만들고 임시 객체 데이터를 그대로 복사해서 사용**하기 때문에 **임시 생성자가 소멸될 때의 문제를 해결**할 수 있었습니다.

이는 위 이미지에서 copy의 동작과 같습니다.

![복사 동작](https://drive.google.com/uc?export=view&id=15KdU-LjSp6_QoCbNEMNuaBKFxXTYTOi9&usp=drive_fs){: w="465" h="75"}

## 이동의 필요성을 느끼다

하지만, 깊은 복사만으로 괜찮을까요? 만약, data의 크기가 커진다면? 그리고 매번 생성하고 복사하고 옮기는 작업이 반복된다면?

**큰 데이터의 깊은 복사는 성능에 영향**을 미칠 수 밖에 없을 겁니다.

이때, 쓰고자 하는 것이 <span class="keyword">**이동(move semantics)**</span>입니다.

아래와 같이 코드를 추가하고, 소멸자를 수정해 봅시다.

```cpp
MyCharacter(MyCharacter&& other) noexcept
{
	data = other.data;
	other.data = nullptr;

	cout << "Move Constructor: " << static_cast<void*>(data) << '\n';
}

~MyCharacter()
{
	if (data != nullptr)
	{
		delete[] data;
		cout << "Destructor: " << static_cast<void*>(data) << '\n';
	}
	else
	{
		cout << "Destructor: nullptr\n";
	}
}
```

![이동 후 결과](https://drive.google.com/uc?export=view&id=1B4aCTQOuk9VZLU8VNwIxl4InREIuFNGi&usp=drive_fs){: w="332" h="154"}
_임시 객체의 경우 nullptr로 이미 소멸되었지만, 이후 소멸자에서는 처음 생성한 객체의 주소가 그대로 이동되었다._

![이동 동작](https://drive.google.com/uc?export=view&id=1257WmjtEDlwARdzvDzxMdLDiAxYJmFNS&usp=drive_fs){: w="469" h="80"}

위 코드는 **이동 생성자를 호출**하고, 동적 할당된 원본 데이터를 망가트리지 않으면서, 큰 데이터의 경우 **복사 비용 없이 빠르게 데이터를 옮겨 줄 수 있습니다.**

사실은 <span class="font_highlight">데이터를 이동한다기 보다 해당 데이터를 가리키는 포인터를 옮겨(move)주는 것</span>에 해당합니다.


## 자동으로 이동하지 않는다?

그런데, 한 가지 의문이 들것이라고 생각됩니다.

<span class="serif">*왜 이동된 data에 굳이 nullptr을 지정해줘야 하는거지?*</span> 라고 생각할 수 있습니다.

사실, nullptr을 지정해주는 부분을 빼면 다음과 같은 문제가 또 발생하게 됩니다.

![자동으로 이동하지 않는 모습](https://drive.google.com/uc?export=view&id=1B6e1pcoAZigjD53Odbr05WpBgvvI1L5q&usp=drive_fs){: w="327" h="144"}
_이동 생성자로 이동했음에도 동적 할당된 메모리가 그대로 소멸되었다..._

분명히 이동 생성자가 호출되었는데에도, 임시 객체가 소멸될 때, 얕은 복사와 같은 문제가 발생합니다.

이는, <span class="keyword">**이동 시맨틱(move semantics)**</span>은 데이터를 이동시켜주는 새로운 코드가 아니라, <br> <span class="font_highlight">**객체의 리소스를 효율적으로 이동시킬 수 있는 방법을 제시하며, 실제 이동의 여부는 프로그래머가 작성하는 것**</span>에 달려있다고 볼 수 있습니다.

---

# 우측 값 참조(rvalue reference)

저는 이동(move semantics)을 위와 같이 정리했습니다.

그리고 이제 이 이동(move semantics)을 사용하기 위해 나오게 된 <span class="keyword">**rvalue 참조**</span>에 대해서 알아보겠습니다.

## 우측 값 참조는 왜 있는 걸까?

이 부분은 아마 이동 시맨틱(move semantics)을 오버로딩 하기 위해 필요한 조치가 아니었을까? 생각합니다.

우선, 이번에는 객체가 아니라 간단한 함수로 테스트를 해보겠습니다.

```cpp
#include <iostream>
using namespace std;

void fn(int& x)
{
	cout << "Lvalue reference\n";
}
void fn(const int& x)
{
	cout << "Rvalue\n";
}

int main()
{
	int x = 10;

	fn(x); // Lvalue reference
	fn(10); // Rvalue
	fn(x + 10); // Rvalue
}
```

![const lvale 참조 매개변수를 호출](https://drive.google.com/uc?export=view&id=1BKJTgboy2V-FGo9FzNtMWjZ0yUOsQJ7X&usp=drive_fs){: w="313" h="117"}

만약, rvalue 참조가 없다면 다음과 같이 rvalue는 `const int&` 형태의 시그니처를 가진 함수를 호출하게 됩니다.

즉, rvalue는 기본적으로 수정 불가능한 형태로 호출됩니다. 아니면 아래와 같이 값 복사를 사용해야 합니다.

```cpp
#include <iostream>
using namespace std;

void fn(int x)
{
	cout << "value\n";
}
void fn(int& x)
{
	cout << "Lvalue reference\n";
}

int main()
{
	int x = 10;

	//fn(x); // 오버로딩 될 함수 목록이 두개라서 문제가 발생.
	fn(10); // value
	fn(x + 10); // value
}
```

하지만, 저희가 원한건 rvalue를 “**이동**”하는 것이니 **rvalue를 수정할 수 있는 어떠한 형태의 다른 타입**이 필요했을 겁니다.

그래서 생긴 것이 <span class="keyword">**우측 값 참조(rvalue reference)**</span>라고 생각합니다.

기존의 lvalue 참조는 **`int&`** 형태였으니, rvalue를 이동할 수 있게 참조한다는 의미를 담아 rvalue 참조를 **`int&&`** 형태로 만든 것이 아닐까 생각합니다.

때문에 놀랍게도, 다음과 같은 코드를 추가하면 <span class="important">**rvalue**</span>는 기존의 **`const int&`** 보다 <span class="font_highlight">**rvalue 참조의 시그니처**를 우선적</span>으로 부르게 됩니다.

```cpp
#include <iostream>
using namespace std;

void fn(int& x)
{
	cout << "Lvalue reference\n";
}
void fn(const int& x)
{
	cout << "Rvalue\n";
}
void fn(int&& x)
{
	cout << "Rvalue reference\n";
}

int main()
{
	int x = 10;

	fn(x); // Lvalue reference
	fn(10); // Rvalue reference
	fn(x + 10); // Rvalue reference
}
```

![rvalue 참조를 호출하는 모습](https://drive.google.com/uc?export=view&id=1BKyJKqE0V56-s-d5xOXfHtajTRz0GTqF&usp=drive_fs){: w="313" h="117"}

## 우측 값 참조의 다양한 사용법

rvalue 참조는 함수의 매개변수로만 사용되는 것이 아닙니다.

rvalue 참조는 이동을 위해 만들어진 **새로운 타입**이기 때문에 기존의 타입처럼 **함수의 반환 값**이나 **변수 타입**처럼 사용할 수 있습니다.

아래 예제가 의미가 있을지는 모르겠지만, 다양한 사용법에 대한 코드를 보여드리겠습니다.

```cpp
MyCharacter&& rvalueRef(MyCharacter&& x)
{
	return std::move(x);
}

int main()
{
	MyCharacter x;
	MyCharacter&& y = rvalueRef(std::move(x));
	cout << static_cast<void*>(y.data) << '\n';
}
```

아직, **`std::move()`**에 대해서는 언급하지 않았지만, 중요한 것은 <span class="font_highlight">rvalue 참조 또한 타입</span>이라는 것입니다.

위의 코드는 단순히 객체 x를 만들어서 rvalue 참조의 매개변수로 사용하고, rvalue 참조로 리턴한 후, rvalue 참조 변수에 저장하는 코드입니다.

## 우측 값 참조의 잠깐 정리

따라서 rvalue 참조를 잠깐 정리해보면 다음과 같을 것 같습니다.

- 임시 객체(rvalue)에 대한 참조를 가능하게하여 rvalue의 소유권을 이전(move)할 수 있게 하는 함수를 오버로딩 하는데 사용한다.
- rvalue 참조는 타입에 해당한다. 따라서 매개변수, 리턴 타입, 변수 타입으로도 쓸 수 있다.

---

# 강제적으로 이동 시맨틱 사용하기(std::move)

이 **이동**은 우측 값 참조(rvalue reference)를 이용해 값을 복사 비용 없이 이동시키기 위함입니다.

하지만, 만약 **좌측 값(lvalue)를 이동 시키고 싶다면** 어떨까요?

![lvalue는 rvalue 참조 타입에 바인딩 되지 않는다.](https://drive.google.com/uc?export=view&id=1BQStd3V_v10PxfK95khtNSedCq5nVMAY&usp=drive_fs)
_lvalue는 rvalue 참조 타입에 바인딩 되지 않는다._

## lvalue를 rvalue처럼 쓸 수 있게 해주는 함수 std::move()

다행히 **`std::move()`**라는 **C++11** 표준 라이브러리 함수가 있습니다.

원래는 lvalue를 함수 호출 인자로 넣으면, rvalue 참조를 가진 함수 시그니처를 호출 할 수 없지만, **`std::move()`**를 사용하는 것으로 <span class="font_highlight">**lvalue를 임시적으로 rvalue(정확히는 xvalue)로 캐스팅**</span> 할 수 있습니다.

```cpp
void mySwap(MyCharacter&& x, MyCharacter&& y)
{
	MyCharacter&& tmp = x; // 문제 발생
	x = y;
	y = tmp;
}

int main()
{
	MyCharacter x;
	MyCharacter y;
	cout << "x: " << static_cast<void*>(x.data) << ", y: " << static_cast<void*>(y.data) << '\n';

	mySwap(std::move(x), std::move(y));

	cout << "x: " << static_cast<void*>(x.data) << ", y: " << static_cast<void*>(y.data) << '\n';
}
```

하지만, 이 코드에는 문제가 있습니다.

## 우측 값 참조는 사실 lvalue 이다?

C++에서 <span class="keyword">**rvalue 참조(&&)**</span>는 이동 시맨틱을 가능하게 하기 위한 것이며, 사실 <span class="font_highlight">**rvalue 참조 변수 자체는 lvalue**</span>입니다.

lvalue와 rvalue에 대한 정보는 [[L_Value & R_Value] 1. 기본적인 L-Value와 R-Value 구분하기](/posts/l_value-&-r_value-1.-%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-l-value%EC%99%80-r-value-%EA%B5%AC%EB%B6%84%ED%95%98%EA%B8%B0/){: target="_blank"} 에서 볼 수 있으며

여기에서, 주소가 있고 참조가 가능한 것은 lvalue입니다.

![Untitled](https://drive.google.com/uc?export=view&id=1BRx4unhog_ZIDTiEMdLDyWN_q7eE8h1p&usp=drive_fs){: w="853" h="188"}
_rvalue 참조 매개변수를 rvalue 참조 변수에 담으려 하면 lvalue로 인한 바인딩 문제가 발생한다._

만약, 아래와 같은 코드로 바꿔본다면, x와 y는 각각의 주소가 있고 참조를 할 수 있다는 사실을 알 수 있습니다.

```cpp
void mySwap(MyCharacter&& x, MyCharacter&& y)
{
	cout << &x << ", " << &y << '\n'; // 000000ABA11CF868, 000000ABA11CF888
	/*MyCharacter&& tmp = x;
	x = y;
	y = tmp;*/
}
```

즉, rvalue 참조는 rvalue 로만 오버로딩 되고, 대입이 되지만, 변수로 사용되는 rvalue 참조 자체는 lvalue 라는 것입니다.

## 우측 값 참조와 std::move()를 사용해서 값 이동 시키기

따라서, 아래와 같이 코드를 수정하면 lvalue인 rvalue 참조를 다시 rvalue로 만들면서 rvalue 참조를 통해 이동 시멘틱을 구현할 수 있습니다.

```cpp
class MyCharacter
{
//...
// 복사 대입 연산자
MyCharacter& operator=(const MyCharacter& other)
{
	if (this != &other)
	{
		delete[] data;

		data = new char[10];
		strcpy_s(data, 10, other.data);
	}

	cout << "Copy Assignment: " << static_cast<void*>(data) << '\n';

	return *this;
}
// 이동 대입 연산자
MyCharacter& operator=(MyCharacter&& other) noexcept
{
	if (this != &other)
	{
		delete[] data;

		data = other.data;
		other.data = nullptr;
	}

	cout << "Move Assignment: " << static_cast<void*>(data) << '\n';

	return *this;
}
//...
}
void mySwap(MyCharacter&& x, MyCharacter&& y)
{
	MyCharacter&& tmp = std::move(x);
	x = std::move(y);
	y = std::move(tmp);
}
```

이로써 이동 대입 연산자를 호출해서, 복사 없이 이동을 구현할 수 있지만, 한 가지 문제가 있습니다.

tmp를 “참조”로 사용해버렸기에 생기는 문제입니다.

![Untitled](https://drive.google.com/uc?export=view&id=1BTKT-7_5qtKKm2PYATl6uIO2aTeZyNyt&usp=drive_fs){: w="383" h="211"}
_rvalue 참조는 참조 타입이다._

위 코드대로 하면, 이동 대입 연산자를 호출하지만, 

1. “참조”이기 때문에 **`MyCharacter&& tmp = std::move(x);`**에서 **`tmp`**는 **`x`**를 그대로 가리키게 되고, 
2. 그런 다음 **`x = std::move(y);`**를 수행하면 **`x`**는 **`y`**를 가리키면서 **`y`**가 **`nullptr`**이 되고, 
3. 마지막으로 **`y = std::move(tmp);`**를 수행하면 **`y`**가 기존 **`tmp``(x)`**가 가리키던 값을 가리키면서 **`tmp``(x)`**가 가지고 있던 값을 **`nullptr`**로 만들어버려 
4. 최종적으로 **`x`**는 **`nullptr`**이 됩니다.

설명 자체는 복잡했지만, 결론은 <span class="important">**x와 tmp가 같은 값을 가리키면서 문제가 생긴 것**</span>입니다.

따라서, 아래와 같이 코드를 수정하면 rvalue 참조를 통해서 이동 생성자와 이동 대입 연산자를 호출하는 것으로 이동 시멘틱을 구현할 수 있습니다.

```cpp
void mySwap(MyCharacter&& x, MyCharacter&& y)
{
	MyCharacter tmp = std::move(x); //참조하지 않고 이동 생성자로 객체를 새로 만든다. 이때, x는 이동 생성자로 인해 data가 nullptr을 가리킨다.
	x = std::move(y);
	y = std::move(tmp);
}

int main()
{
	MyCharacter x;
	MyCharacter y;
	cout << "x: " << static_cast<void*>(x.data) << ", y: " << static_cast<void*>(y.data) << '\n';
	mySwap(std::move(x), std::move(y));
	cout << "x: " << static_cast<void*>(x.data) << ", y: " << static_cast<void*>(y.data) << '\n';
}
```

![rvalue참조와 move로 이동](https://drive.google.com/uc?export=view&id=1BYp6tCgK4I8UWkqCmNwiybm1KM8KjXsE&usp=drive_fs){: w="382" h="201"}

---

# 그 밖의 내용들

## 참조 축약 규칙

조금 더 공부하면서, 이동 시멘틱을 위해 추가한 타입의 모양이 왜 rvalue 참조(&&)모양인지를 좀 더 이해할 수 있었던 부분이 어쩌면 이 부분입니다.

C++ 11에서 <span class="keyword">**참조 축약 규칙(reference collapsing)**</span>이라는 것이 생겼습니다.

참조 축약 규칙은 참조자 여러개가 겹쳤을 때, 즉, lvalue 참조와 rvalue 참조를 템플릿과 결합할 때 참조 축약 규칙이 적용됩니다.

규칙은 아래와 같습니다.

- T**& &nbsp;&nbsp;&nbsp;& &nbsp;&nbsp; → T&**   : lvalue 참조 + lvalue 참조 → lvalue 참조
- **T& &nbsp;&nbsp;&nbsp;&& → T&**   : lvalue 참조 + rvalue 참조 → lvalue 참조
- **T&& &nbsp;& &nbsp;  → T&**   : lvalue 참조 + rvalue 참조 → lvalue 참조
- **T&& &nbsp;&& → T&&** : rvalue 참조 + rvalue 참조 → rvalue 참조

이 참조 축약 규칙은 **특수 템플릿 인자 유추 규칙(special template argument deduction rule)**에서 <span class="important">**완벽한 전달(perfect forwarding)**</span>을 구현하는데 사용된다고 합니다.

간단히 이해한 바로는, Perfect Forwarding이란, **함수 템플릿에서 전달된 인자를 또 다른 함수로 전달할 때 그 인자의 타입과 값 범주(lvalue, rvalue)를 그대로 유지**하는 기술이라는 것 같습니다.

이를 통해 인자가 원래 가지고 있던 모든 정보(예를 들어, 수정 가능성, 수명 등)를 손상시키지 않고 다른 함수로 그대로 전달하는게 완벽한 전달인 것 같습니다.

따라서 아래와 같이 구현하게 되면, lvalue는 lvalue로, rvalue는 rvalue로 전달되게 됩니다.

```cpp
#include <iostream>
#include <utility>

void foo(int& x) { std::cout << "lvalue ref: " << x << std::endl; }
void foo(int&& x) { std::cout << "rvalue ref: " << x << std::endl; }

template <typename T>
void wrapper(T&& arg) 
{
    foo(std::forward<T>(arg));
}

int main() {
    int a = 5;
    wrapper(a);  // Output: lvalue ref: 5
    wrapper(10); // Output: rvalue ref: 10
}
```

## 이동과 예외

이동 시멘틱을 보다보면 <span class="keyword">**noexcept**</span> 키워드를 붙이는 것을 권장합니다.

이 키워드는 해당 함수에서 예외를 던지지 않을 것이라고 명시적으로 표시하는 키워드입니다.

컴파일러는 noexcept가 지정된 함수가 예외를 던지지 않을 것이라고 가정하므로, 예외 처리에 대한 추가적인 코드를 생성하지 않고, 이로 인해 실행 시간이나 메모리 사용량이 줄어들 수 있습니다.

따라서, <span class="font_highlight">**해당 키워드를 붙이려면 예외가 발생하지 않도록 코드를 작성**</span>해야 합니다. (단순한 이동만 구현한다면 예외가 발생할 일이 없을 것입니다.)

<span class="font_highlight">**만약, 이 키워드를 붙이지 않는다면, std::vector와 같은 것들이 “이동”을 수행하지 않는다**</span>고 합니다.

## 컴파일러 최적화와 이동 시멘틱

컴파일러는 <span class="keyword">**반환 값 최적화(Return Value Optimization, RVO)**</span>를 이용해 성능을 향상 시키는데, 만약, 이 값이 복사가 일어난다고 생각하고 **`std::move()`**를 사용하게 되는 순간 이 최적화가 발생하지 않고 오히려 속도가 느려질 수 있습니다.

예를 들어, 아래와 같은 코드가 있습니다.

```cpp
MyClass foo() {
    MyClass temp;
    // temp로 어떤 작업을 한다.
    return temp;
}

int main() {
    MyClass obj = foo();
}
```

이때, 스택 프레임이나 임시 객체라는 것을 떠올려서 foo() 함수의 스택 프레임 내에서 MyClass를 생성하고, 리턴 될 때, 호출자(caller)의 스택 프레임에 이 객체가 복사될 수 있다고 생각할 수 있습니다.

하지만, 컴파일러는 이 상황에서 반환 값 최적화(Return Value Optimization, RVO)를 할 수 있습니다.

컴파일러 최적화를 사용하면 <span class="font_highlight">**컴파일러는 이러한 복사를 피하기 위해 반환되는 객체를 호출자(caller)의 스택 프레임에 직접 생성**</span>합니다. 즉, 복사 단계를 건너뛰고 객체를 "이동"시킵니다.

이 최적화로 인해 복사 생성자나 이동 생성자의 호출이 줄어들고, 프로그램의 성능이 향상될 수 있습니다.

하지만 아래와 같이 **`std::move()`**를 적용하게 된다면 **이동 시멘틱을 사용하기 적합한 상태**가 되기 때문에 **이동 생성자가 호출될 가능성이 높아져** 리턴 값 최적화보다 느리게 동작하는 것 같습니다.

```cpp
MyClass foo() {
    MyClass temp;
    // temp로 어떤 작업을 한다.
    return std::move(temp);
}
```

---

# 정리

## 우측 값 참조(R-value reference)

- <span class="keyword">**rvalue 참조**</span>는 **rvalue의 소유권을 이전(move)**할 수 있게 하는 이동 시멘틱을 호출하기 위한 새로운 <span class="important">**참조 타입**</span>입니다.
- **lvalue**를 rvalue 참조를 통해 이동 시멘틱을 호출하고 싶다면, **`std::move()`**를 통해 lvalue를 **rvalue(정확히는 xvalue)로 캐스팅**할 수 있습니다.
- **참조 축약 규칙**을 사용하여 템플릿 인자 유추 규칙에서 **완벽한 전달**을 구현할 수 있습니다. <br> (rvalue 참조 + lvalue 참조 → lvalue 참조, rvalue 참조 + rvalue 참조 → rvalue 참조)

## 이동 시멘틱(move semantics)

- 큰 데이터의 경우 **복사 비용 없이 빠르게 데이터를 옮겨(move)** 줄 수 있도록 하는 것.
- 객체의 **리소스를 효율적으로 이동시킬 수 있는 방법을 제시하는 것**으로, 실제 이동의 구현은 프로그래머가 작성하는 것에 달려있습니다.
- 이동 시멘틱을 구현할 때, 해당 함수에 예외가 발생하지 않도록 구현해야 하며, <span class="keyword">**noexcept**</span> 키워드를 붙여야 합니다.

---

<br>

제가 잘 이해했는지, 또 제가 이해한대로 글을 잘 작성 했는지, 잘 모르겠습니다.
{: .text-muted .small}

제가 이해한 내용을 기록하기 위해 정리하고 글로 표현해 봤는데, 생각했던 것 보다 글이 길어진 것 같습니다.
{: .text-muted .small}

제가 정리한 내용이 다른 글을 이해하는데 도움이 되었으면 좋겠습니다.
{: .text-muted .small}

---

# 참고

[Move semantics and rvalue references: Modern C++ fundamentals](https://itnext.io/move-semantics-and-rvalue-references-modern-c-fundamentals-cbbe38760c05)

[씹어먹는 C++ 토막글 ① - Rvalue(우측값) 레퍼런스에 관해](https://modoocode.com/189)

---

[[L_Value & R_Value] 1. 기본적인 L-Value와 R-Value 구분하기](/posts/l_value-&-r_value-1.-%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-l-value%EC%99%80-r-value-%EA%B5%AC%EB%B6%84%ED%95%98%EA%B8%B0/){:target="_blank"}

[[L_Value & R_Value] 3. C++ 11 값 범주 (Lvalue, Xvalue, Prvalue)](/posts/l_value-&-r_value-3.-c++-11-%EA%B0%92-%EB%B2%94%EC%A3%BC-(lvalue,-xvalue,-prvalue)/){:target="_blank"}