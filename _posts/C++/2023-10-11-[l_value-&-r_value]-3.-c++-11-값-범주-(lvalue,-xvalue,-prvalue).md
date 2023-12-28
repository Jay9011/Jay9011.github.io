---
title: "[L_Value & R_Value] 3. C++ 11 값 범주 (Lvalue, Xvalue, Prvalue)"
date: 2023-10-11 22:59:54 +0900
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
    - Lvalue
    - Prvalue
    - Xvalue
    - 값 범주
---

xvalue(eXpiring Value)는 C++ 11 이후에 추가된 개념으로 기존의 값 범주에서 변화가 생겼습니다.

때문에 값 범주가 기존의 개념보다 이해하고 정리하기 더 어려운 개념이 되어버린 것 같습니다.

이 글에서 저는 이 새로운 값 범주에 대해 제가 이해한 과정 정도를 글로 표현하고 정리 해둘까 합니다.

# C++ 11 이후의 값 범주

C++ 11에서 **우측 값 참조(rvalue reference)**와 **이동 시멘틱(move semantics)**이 등장하면서, <span class="keyword">**xvalue(eXpiring value)**</span>라는 개념도 함께 도입되었습니다.

즉, 이동 시멘틱(move semantics)을 사용하기 위해 추가된 값 범주라고 볼 수 있습니다.

우선, MSDN 에서는 값 범주간의 관계를 보여주는 다이어그램을 이렇게 표현했습니다.

![값 범주](https://drive.google.com/uc?export=view&id=1BYv-eIgDYbkUkR88Oe-MOUwhM7lxnr29&usp=drive_fs){: w="400" h="333"}

여기에서 <span class="keyword">**glvalue**(Generalized LValue)</span>는 <span class="keyword">**lvalue**(Left Value, Locator Value)</span>와 <span class="keyword">**xvalue**(eXpiring Value)</span>를 합해서 말하고, 

<span class="keyword">**rvalue**(Right Value)</span>는 <span class="keyword">**xvalue**(eXpiring Value)</span>와 <span class="keyword">**prvalue** (Pure RValue)</span>를 합해서 말합니다.

따라서, 확연히 구분되는 범주는 **lvalue**, **xvalue**, **prvalue**라고 볼 수 있습니다.

lvalue와 prvalue는 이전에 설명한 글을 참조하면 되지만, lvalue의 경우에는 한가지 특성이 더 들어가면 좋을 것 같습니다.

- <span class="keyword">**lvalue**</span> : **표현식이 끝나고도 유지되어 다시 사용될 수 있는 값**을 말합니다. (다른 말로는 identity를 가진다고 합니다.) 즉, 메모리 상에 위치해있고, 그 위치를 **참조할 수 있는 값**을 말합니다.
    - 또한, **rvalue 참조에 바인딩 할 수 없는 값**입니다. <br> rvalue란, 일시적이거나 이름이 없는 값으로 곧 만료될 값을 뜻하는데, 때문에 “이동”해도 문제가 없을 것이라 판단되는 값입니다.
    하지만, lvalue는 표현식을 벗어나도 다시 참조할 수 있기 때문에 그대로 rvalue 참조에 바인딩 하지 못합니다.
- <span class="keyword">**prvalue**</span> : **표현식을 벗어나면 다시 사용할 수 없는 값**을 말합니다. 다른 말로는 identity를 가지지 않는다고 하는데, 간단히 말해 메모리 주소를 가지지 않는 값을 말합니다.

![lvalue는 rvalue 참조에 바로 바인딩 하지 못한다.](https://drive.google.com/uc?export=view&id=1BQStd3V_v10PxfK95khtNSedCq5nVMAY&usp=drive_fs){: w="680" h="148"}
_lvalue는 rvalue 참조에 바로 바인딩 하지 못한다._

## 새로 추가된 xvalue(eXpiring Value)

먼저, xvalue에 대한 이해를 돕기 위해 제가 추론한 내용을 간단하게 풀고 가겠습니다. 우선 xvalue가 왜 **glvalue**와 **rvalue**에 속하게 되었을까 생각해보았습니다.

xvalue의 등장은 먼저 이동 시멘틱의 등장에 있었습니다. 따라서 각 범주에 대해 등장 배경을 추론해보면,

- **glvalue의 특징**: rvalue는 identity를 가지지 않는 값입니다. 기본적으로 메모리 주소를 가지지 않기 때문에 일반적인 방법으로 참조하고, 이동할 수 없습니다.
    - 따라서, “이동”을 하기 위해서는 메모리에 위치할 수 있는 임시 객체로써 존재해야 되고, 이는 곧 lvalue의 특성에 가깝습니다.
- **rvalue의 특징**: lvalue를 “이동”하게 되면 더 이상 유효한 값이 아니게 되어버립니다. 하지만, lvalue를 그대로 “이동”시켜버리는 경우, 이후에 다시 그 lvalue를 접근하거나 수정하려는 시도를 하게 되면 문제가 발생할 수 있습니다.
    - 따라서, rvalue와 같이 표현식이 끝나면 만료되어야 하는 그 어떠한 값이나 표시가 필요했을 것입니다.

따라서, xvalue는 glvalue의 특성을 가지면서도 기존의 rvalue의 특성을 가질 필요가 있었던 것 같습니다.

---

# xvalue란?

즉, 만료되어 가는 값은 안전하게 “이동” 시켜도 괜찮다고 판단하고, “이동” 시키려면 identity를 가져 메모리 공간의 데이터에 접근할 수 있어야 하므로

<span class="keyword">**xvalue(eXpiring Value)**</span>란, 만료되어 가는 값으로, “**이동**” 할 수 있는 identity를 가지는 값.
{: .text-center}

이라고 할 수 있겠습니다.

이때, 만료되어 간다는 표현은 그 값이 이동 연산에 사용될 수 있으며 이후에는 그 상태가 불명확할 수 있다는 것을 의미하지, 그 값을 임의로 만료시키지는 않습니다.

예를 들어, lvalue를 xvalue로 캐스팅하는 코드가 있고, 이 코드가 어떤 함수를 호출한다고 합시다.

```cpp
void test(MyCharacter&& c)
{
	// 어떤 작업을 한다.
}

int main()
{
	MyCharacter c;
	test(std::move(c)); // std::move(c)는 c에 대한 xvalue를 생성한다.
}
```

여기에서 **`std::move`**는 **lvalue를 xvalue로 캐스팅해주는 함수**입니다.

이때, 당연히도 c가 xvalue가 되지는 않습니다. 단지 c에 대한 “이동 가능한 참조”를 생성하고 넘길 뿐입니다.

즉, 원래의 lvalue에 대한 이동 연산을 가능하게 하는 xvalue를 생성할 뿐이지 lvalue가 반드시 만료되어야 하는 것은 아닙니다.

예를 들어, swap을 “이동”으로 구현하는 경우에 두 lvalue 모두 만료되어서는 안될 것입니다.

단지, lvalue가 xvalue로 캐스팅되면 그 값의 리소스는 이동될 수 있으므로, 그 이후에는 그 값에 대한 액세스가 "만료"될 수 있으므로 주의해야 한다는 의미로 “만료되어 가는 값”이라는 표현을 쓴 것 같습니다.

# xvalue의 종류

사실, 이 부분이 난해하고 어렵습니다.

우선, xvalue는 “이동”을 위해서 만들어진 값 범주에 해당합니다. 즉, rvalue 참조(&&)와 관련있다는 것을 먼저 생각해두면 좋을 것 같습니다.

## 1. std::move()로 캐스팅

**`std::move()`**가 **lvalue를 xvalue로 변환**해 줍니다.

따라서, lvalue를 rvalue 참조의 인자로써 호출할 수 있게 해줍니다.

```cpp
void mySwap(MyCharacter&& x, MyCharacter&& y)
{
	MyCharacter tmp = std::move(x);
	x = std::move(y);
	y = std::move(tmp);
}

int main()
{
	MyCharacter x;
	MyCharacter y;
	mySwap(std::move(x), std::move(y));
}
```

## 2. rvalue 참조로 캐스팅

**rvalue 참조로 캐스팅**하면 캐스팅 결과는 **xvalue** 입니다.

```cpp
void rvalueRef(MyCharacter&& x)
{
}

int main()
{
	MyCharacter x;
	rvalueRef(static_cast<MyCharacter&&>(x));
}
```

## 3. 반환 유형이 rvalue 참조인 함수를 호출한 결과

**rvalue 참조 타입을 반환하는 경우**가 있을지는 잘 모르겠지만…

반환 유형이 rvalue 참조 타입인 경우에도 이동 가능한 **xvalue를 반환**하게 됩니다.

```cpp
MyCharacter&& returnRvalueRef()
{
	MyCharacter x;
	return std::move(x);
}

void rvalueRef(MyCharacter&& x)
{
}

int main()
{
	rvalueRef(returnRvalueRef());
}
```

단순히, 예제일 뿐이지 실제 이렇게 코딩하면 안됩니다. MyCharacter는 지역변수이기 때문에 함수가 종료되는 순간 이 객체는 소멸될 것입니다.

## 4. 객체의 표현식이 xvalue인 객체의 비정적 멤버로의 접근

**객체가 xvalue**라면, 이 객체의 **멤버로 접근한 결과도 xvalue** 입니다.

```cpp
class MyCharacter
{
public:
	int id;
};

void rvalueRef(int&& x)
{
}

int main()
{
	MyCharacter x;
	rvalueRef(std::move(x).id);
}
```

## 5. 임시 객체 구체화(Temporary materialization)

이 부분이 가장 난해한 부분입니다.

임시 구체화는 <span class="important">**임시 객체인 prvalue가 xvalue로 변환되는 것**</span>을 말합니다. prvalue 표현식에서 임시 객체를 초기화하고, 그 임시 객체를 나타내는 xvalue를 생성하게 되는 변환을 말합니다.

자세한 내용은 [Implicit conversions - cppreference.com](https://en.cppreference.com/w/cpp/language/implicit_conversion#Temporary_materialization){: target="_blank"} 을 보시는 것을 추천 드립니다.

### 1) prvalue를 참조에 바인딩할 때

```cpp
void test(const int& x) {}

int main() 
{
    test(42);  // 42는 prvalue
}
```

이 코드에서 42는 int형 리터럴인 prvalue 입니다.

그러나, test()에 참조로 매개변수를 받기 때문에, 참조할 수 있는 메모리 공간이 필요합니다.

따라서, 이 prvalue 표현식은 임시 객체를 생성하고 xvalue로 캐스팅하여 참조에 바인딩하게 됩니다.

이는 rvalue 참조도 마찬가지입니다.

```cpp
void test(int&& x) {}

int main() 
{
    test(42);  // 42는 prvalue
}
```

위 코드 또한 임시 객체를 생성하며 xvalue로 캐스팅하여 참조에 바인딩합니다.

### 2) prvalue 클래스로의 멤버 접근을 수행할 때

```cpp
class MyCharacter
{
public:
	int id;
};

int main()
{
	int id = MyCharacter().id;  // MyCharacter()는 prvalue이지만 멤버로 접근하기 위해 xvalue로 캐스팅되어야 한다.
}
```

이 코드에서 MyCharacter()는 prvalue 입니다.

하지만, 해당 임시 객체의 멤버로 접근하기 위해서는 참조할 수 있는 메모리 공간이 필요하게 됩니다.

따라서, 이 prvalue 표현식은 임시 객체를 생성하고 xvalue로 캐스팅하여 멤버로 접근할 수 있게 됩니다.

### 3) prvalue 배열에 배열 - 포인터 변환 또는 첨자 연산을 수행할 때

```cpp
std::array<int, 3> getArray() 
{
    return {1, 2, 3};
}

int main() 
{
    int x = getArray()[2];  // getArray()의 반환값은 prvalue
}
```

만약, 위와 같은 코드가 있다면, 이 코드는 prvalue 배열을 반환하게 됩니다.

하지만, 첨자 연산을 통해서 배열의 요소에 접근하기 위해서는 참조할 수 있는 메모리 공간이 필요합니다.

따라서, 이 prvalue 배열은 임시 객체를 생성하고 xvalue로 캐스팅하여 요소에 접근하게 됩니다.

### 4) std::initializer_list\<T\> 타입의 객체를 중괄호 초기화 목록에서 초기화할 때

`std::initializer_list<T>`는 C++11 이후로 도입된 타입으로, 중괄호 초기화 목록을 사용할 수 있습니다.

```cpp
#include <initializer_list>

void test(const std::initializer_list<int>& list)
{
}

int main()
{
    test({1, 2, 3, 4, 5});  // 중괄호 초기화 목록은 prvalue입니다.
}
```

위 코드에서 `{1, 2, 3, 4, 5}`는 int 타입의 prvalue 중괄호 초기화 목록입니다.

이 prvalue는 `std::initializer_list<int>` 타입의 객체를 초기화하는 데 사용됩니다. 이 과정에서 임시 객체가 생성되고 xvalue로 변환된다는 것 같습니다.

<br>

이 외에도 이렇게 임시 객체가 메모리 공간에 접근해야 할 필요가 있을 때, Temporary materialization이 발생할 수 있습니다.

---

# 각 값 범주들의 함수 오버로딩 접근

각 값 범주들은 똑같은 함수들에 시그니처를 달리했을 때, 각각 접근하는 범위가 다릅니다.

이는 “이동”과 “만료”와 관련이 있다고 생각하시면 편하게 이해하실 수 있습니다.

```cpp
void callOverloading(int& x)
{
	cout << "L-value Ref\n";
}
void callOverloading(int&& x)
{
	cout << "R-value Ref\n";
}
void callOverloading(const int& x)
{
	cout << "Const L-value Ref\n";
}

int main()
{
	int a = 123;
	callOverloading(a);	// L-value Ref
	callOverloading(123);	// R-value Ref
	callOverloading(std::move(a));	// R-value Ref

	const int b = 200;
	callOverloading(b);	// Const L-value Ref
}
```

- <span class="keyword">**lvalue**</span>: 우선 lvalue는 “만료”되면 안되는 값 범주입니다. 강제로 만료되어 가는 것(xvalue)처럼 하여 rvalue 참조에 바인딩 할 수 있지만, 기본적으로는 **L-value 참조**를 호출합니다.
- <span class="keyword">**prvalue**</span>: prvalue는 “만료”되어 가는 값입니다. 따라서, “이동”에 쓰일 수 있다고 판단하고 xvalue로 변환하여 **R-value 참조**를 호출합니다.
- <span class="keyword">**xvalue**</span>: `std::move()`는 lvalue를 “이동”에 쓰고 싶어서 강제로 xvalue로 캐스팅하는 함수입니다. 따라서 “만료”되어 가고 “이동”에 쓰일 수 있는 값으로 판단하고 **R-value 참조**를 호출합니다.
- <span class="keyword">**const lvalue**</span>: const lvalue는 **Const L-value 참조**에 바인딩 됩니다. <br> “이동”은 객체의 데이터를 다른 객체로 옮기는 것인데, 이 과정에서 객체의 상태가 변경될 수 있습니다. 그러나 const는 객체가 불변임을 나타내는 키워드이므로 rvalue 참조나 변경 가능성이 있는 lvalue 참조에 바인딩 되지 않습니다.

---

# 참고

[값 범주: Lvalues 및 Rvalues(C++) \| Microsoft Learn](https://learn.microsoft.com/ko-kr/cpp/cpp/lvalues-and-rvalues-visual-cpp?view=msvc-170&viewFallbackFrom=vs-2019)

[Move semantics and rvalue references: Modern C++ fundamentals \| by Šimon Tóth \| ITNEXT](https://itnext.io/move-semantics-and-rvalue-references-modern-c-fundamentals-cbbe38760c05)

[Value categories - cppreference.com](https://en.cppreference.com/w/cpp/language/value_category)

[Implicit conversions - cppreference.com](https://en.cppreference.com/w/cpp/language/implicit_conversion#Temporary_materialization)

[c++ - What expressions create xvalues? - Stack Overflow](https://stackoverflow.com/questions/11581903/what-expressions-create-xvalues)

[value categories - Temporary materialization conversion - Confusion about terminology and concepts - Stack Overflow](https://stackoverflow.com/questions/66474539/temporary-materialization-conversion-confusion-about-terminology-and-concepts)

---

[[L_Value & R_Value] 1. 기본적인 L-Value와 R-Value 구분하기](/posts/l_value-&-r_value-1.-%EA%B8%B0%EB%B3%B8%EC%A0%81%EC%9D%B8-l-value%EC%99%80-r-value-%EA%B5%AC%EB%B6%84%ED%95%98%EA%B8%B0/){:target="_blank"}

[[L_Value & R_Value] 2. 우측 값 참조와 이동(move semantics)](/posts/l_value-&-r_value-2.-%EC%9A%B0%EC%B8%A1-%EA%B0%92-%EC%B0%B8%EC%A1%B0%EC%99%80-%EC%9D%B4%EB%8F%99(move-semantics)/){:target="_blank"}
