---
title: "[L_Value & R_Value] 1. 기본적인 L-Value와 R-Value 구분하기"
date: 2023-09-24 20:11:14 +0900
categories:
  - Full Stack 개발
  - C++
tags:
  - c
  - lvalue
  - rvalue
  - 값 타입
  - 데이터
  - 메모리
math: false
type: C++
keywords:
  - L-Value
  - R-Value
  - 값 범주
---

C++에서 “값 범주”라고도 불리는 이 <span class="keyword">**L-Value**</span>와 <span class="keyword">**R-Value**</span>는 **“이동”**의 개념이 생기면서 좀 더 세분화 되었습니다. 그리고 좀 더 복잡해졌습니다.

이 **세분화된 구분을 정리하기 전**, 먼저 가볍게 <span class="keyword">**L-Value**</span>와 <span class="keyword">**R-Value**</span>에 대해서 이해하고 넘어가도록 하겠습니다.

그리고 저는 “이동”이라는 용어를 먼저 사용하지 않고 정리하도록 하겠습니다. “이동”이라는 용어로 인해 좀 더 어려워진 느낌이 있기 때문에 우선 L-Value와 R-Value가 어떤 것인지 이해를 한 이후에 “이동”이라는 용어를 첨가하는 쪽이 더 좋다고 생각했습니다.

---

# L-Value 란?

<span class="font_highlight">**표현식이 끝나고도 유지되어 다시 사용될 수 있는 값**</span>입니다. 어려운 말로는 “<span class="important">**Identity를 가진다**</span>”고도 하는데, 쉽게 말해 메모리 상에 위치가 있고, 그 위치를 참조할 수 있는 값을 말합니다.

지금 당장은 어려울 수 있지만, 좀 더 이해하기 쉽게 풀어낸 후 다시 정리하겠습니다.

우선, 아래 표현식을 보겠습니다.

![lvalue](https://i.postimg.cc/cLw6W6tn/lvalue01.png){: w="244" h="139"}

여기에서 `a`가 <span class="keyword">**L-Value**</span> 입니다. 이때, int는 자료형일 뿐이고 int a가 L-Value가 아니라 a가 L-Value인 겁니다.

좀 더 이해하기 쉽게 보충하자면, `a = 20;`에서 `a`가 <span class="keyword">**L-Value**</span> 입니다. 즉, a라는 값의 메모리 주소를 참조할 수 있기 때문에 L-Value라는 **값 범주**에 해당합니다.

![lvalue 참조](https://i.postimg.cc/zfNvG5TP/lvalue02.png){: w="208" h="192"}

---

# R-Value 란?

기존의 R-Value란, <span class="font_highlight">**표현식을 벗어나면 다시 사용할 수 없는 값**</span>을 말합니다. 기존에는 이 값들은 Identity를 가지지 않아서, 해당 표현식을 벗어나면 다시 사용할 수 없는 값들로 리터럴이나 임시 객체, 열거형, 등을 말했습니다.

여기에서 왜 “기존”이라는 말을 덧붙였냐면 **C++11 이후**로 <span class="keyword">**X-Value(eXpiring Value)**</span>라는 개념이 도입되었기 때문입니다. 이는 이동 시맨틱스(move semantics)의 도입으로 인해 값 범주가 세분화 되었기 때문인데, 우선 이 어려운 개념을 생각하기 전에 기존의 R-Value를 먼저 이해하는 것이 중요합니다.

왜냐하면, L-Value의 개념은 읽으면 어느정도 이해할 수 있겠지만, R-Value를 공부하고 나서 이리저리 공부하다 보면 헷갈릴 수 있고, 이 개념이 흔들리면 X-Value도 어렵기 때문입니다.

따라서, 우선 “이동”이라는 용어를 사용하지 않고 R-Value를 이해해 보도록 하겠습니다.

![rvalue](https://i.postimg.cc/QxWdgVby/rvalue01.png){: w="262" h="138"}

이 식에서 <span class="keyword">**R-Value**</span>는 당연히 `10` 입니다. `10`은 **정수형 리터럴**이고, 이 표현식을 벗어나면 이 `10` 자체를 다시 사용할 수 없습니다.

![rvalue 구분](https://i.postimg.cc/NjqfdYCf/rvalue02.png){: w="331" h="167"}

이 두 표현식에서 두 `10`은 모두 <span class="keyword">**R-Value**</span> 지만, **10 자체를 재사용 한 것으로 볼 수 없습니다**. 실제로 10이라는 리터럴은 메모리에 위치한걸 가져온게 아니라 **10 그 자체를 대입한 것**이기 때문입니다.

![rvalue 리터럴의 어셈블리코드](https://i.postimg.cc/W3b40nWG/Rvalue.png){: w="581" h="130"}

이 두 표현식에서 `0Ah`는 **10을 16진수로 표현한 값**입니다. 따라서 두 10은 논리적으로는 같은 값이지만, <span class="font_highlight">**하나의 표현식에서 그 당시에만 유효한 값으로 해당 표현식을 벗어나면 다시 사용하지 못한 값**</span>입니다.

이것이 왜 중요하냐면, 앞으로 나올 표현식에서 L-Value와 R-Value를 헷갈리지 않게 하기 위함입니다.

핵심은, **<span class="keyword">R-Value</span>란, 표현식을 벗어나면 다시 사용할 수 없는 값**이라는 것입니다.

---

# L-Value 와 R-Value 구분하기

## 1. 비참조 타입을 반환하는 함수

비참조 타입을 반환하는 함수의 경우에는 <span class="keyword">**R-Value**</span> 입니다.

정확히는 함수가 아니라 **<span class="font_highlight">함수의 결과가 임시 객체로 반환</span> 되므로, 해당 표현식에서만 사용되고, 표현식을 벗어나면 다시 사용할 수 없습니다**.

```cpp
int rtnInt() { return 1; }
int main()
{
	rtnInt();
	int num = rtnInt();
}
```

즉, 위 코드에서 `rtnInt();`만 쓴 경우에는 반환 값이 다음 표현식으로 넘어갈 때, 다시 사용할 수 없습니다. 따라서 `int num = rtnInt();`와 같이 함수의 결과를 <span class="keyword">**R-Value**</span>로 쓰게 됩니다.

따라서 아래처럼 사용하려고 하면 다음과 같은 오류를 출력합니다.

![rvalue를 lvalue처럼 쓰면](https://i.postimg.cc/LsVXq7T4/rvalue-01.png){: w="343" h="146"}

## 2. 배열 요소

배열 요소는 당연히 <span class="keyword">**L-Value**</span> 입니다. 배열 요소는 메모리상에 저장 공간이 있고, 참조할 수 있으므로 L-Value 입니다.

```cpp
int arr[5];
arr[0] = 1;
int a = arr[0];
```

## 3. 포인터 역참조

포인터의 역참조(\*)도 <span class="keyword">**L-Value**</span> 입니다. 포인터의 역참조(\*)도 마찬가지로 메모리상에 저장 공간이 있고 역참조를 통해 주소에 있는 값을 얻어오거나 변경할 수 있습니다.

```cpp
int *ptr = &a;
*ptr = 30;
int b = *ptr;
```

## 다시 한 번, L-Value 란

이쯤에서 헷갈릴 수 있는 부분이 배열이나 포인터는 L-Value도 되고 R-Value도 될 수 있는게 아닌가 하는 부분일 수 있습니다.

L-Value를 대입연산자 왼쪽에 오는 것이고, R-Value를 대입연산자 오른쪽에 오는 것이라고 배웠다면 그렇게 생각할 수 있습니다.

하지만, <span class="keyword">**L-Value**</span>는 대입 연산자 왼쪽에도 올 수 있고 오른쪽에도 올 수 있습니다.

단지 **표현식이 끝나고도 유지되어 다시 사용될 수 있는 값**으로 **메모리 주소를 가지는 객체**를 의미할 뿐입니다.

즉, **배열의 요소(`[]`) 표현식**은 특정 메모리 위치를 역참조(`*(arr + 0)`)하는 것입니다.

**포인터의 역참조** 또한 포인터가 가리키는 메모리 주소를 역참조해 **주소에 있는 값을 얻거나 수정**합니다.

즉, 배열의 요소를 가리키거나 포인터를 역참조 하는 것 자체는 <span class="keyword">**L-Value**</span>로 봅니다.

[포인터에서 참조와 역참조의 뜻](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){:target="_blank"} 보기

디스어셈블리로 확인하면, 참조로 주소 값을 얻어온 후 해당 주소를 역참조 해 대입하는 모습을 볼 수 있습니다.

![포인터 역참조의 값 대입](https://i.postimg.cc/x1vj77BG/rvalue-02.png){: w="600" h="215"}

`*ptr = 30;`부분을 보면 `ptr` 변수를 참조해 64비트 주소(rax)를 가져오고, 이를 역참조해 30을 저장하고 있습니다. 즉, 역참조 자체는 메모리를 사용하는 <span class="keyword">**L-Value**</span>로 봅니다.

`int b = *ptr;`부분을 보게 되면, 역시 `ptr` 변수를 참조해 64비트 주소를 똑같이 rax 레지스터에 로드하고, 이를 역참조해 4바이트 값을 eax 레지스터에 저장한 다음, 변수 b에 저장하고 있습니다.


## 4. 포인터 참조

포인터 참조 연산자(&)는 변수의 주소를 가져오지만, 그 결과는 <span class="keyword">**R-Value**</span>입니다.

```cpp
int *ptr = &a;
```

즉, `a`는 <span class="keyword">**L-Value**</span>지만, `&a`는 **메모리 주소를 나타내는 임시 값**을 나타내는 <span class="keyword">**R-Value**</span> 입니다. 이는 [1. 비참조 값을 반환하는 함수](#1-비참조-타입을-반환하는-함수)와 같이 생각해도 좋을 것 같습니다.

다만, 이 값 자체는 주소이기 때문에, [역참조](#3-포인터-역참조)하게 된다면 이는 <span class="keyword">**L-Value**</span>처럼 쓸 수 있습니다.

예를 들어, 아래와 같은 코드로 쓰면 L-Value 변수처럼 다룰 수 있습니다.

```cpp
*&a = 10;
cout << a << endl; // 10
```

따라서, 주소 그 자체를 나타내는 임시 값으로 생각하면 R-Value 라고 생각하지만, 사람에 따라서는 참조 연산자(&)가 변수의 주소를 리턴하고 주소는 메모리 위치를 나타내므로, 이 주소 자체를 L-Value라고도 생각 할 수도 있습니다.

## 5. 배열의 이름

위에서 배열의 요소는 L-Value라고 했습니다. 그렇다면 배열의 이름은 어떨까요?

```cpp
int* p = arr; // int arr[5]
```

결과부터 말씀 드리자면, 배열의 이름은 주소로 사용되는 R-Value 입니다.

![배열의 이름은 rvalue](https://i.postimg.cc/JhKr7XrG/rvalue.png){: w="574" h="89"}

디스어셈블리로 보면, arr 이라는 변수의 주소를 lea로 연산해 가져오는 것을 볼 수 있습니다.

즉, C++에서 배열의 이름은 `arr`이라는 배열의 메모리의 주소를 나타내는 <span class="keyword">**R-Value**</span> 입니다.

하지만, 위에서 설명했듯이 [3. 포인터 역참조](#3-포인터-역참조) 연산자를 쓰는 것으로 <span class="keyword">**L-Value**</span>로 쓸 수 있습니다. 배열의 이름이 배열의 첫 번째 주소를 리턴하고 있기 때문입니다.

```cpp
*arr = 10;
cout << arr[0] << endl; // 10
```

따라서, 아래와 같은 연산을 사용할 수도 있고 이는 포인터의 요소에 접근하는 `[]`와 같은 역할을 합니다.

```cpp
*(arr + 2) = 10; // arr[2] = 10;
cout << arr[2] << endl; // 10;
```

## 6. 참조를 반환하는 함수와 포인터를 반환하는 함수

위에서 비참조를 반환하는 함수는 R-Value 라고 했습니다. 그렇다면 참조를 반환하는 함수는 어떨까요?

참조를 반환한다는 것은 해당 변수의 별칭을 반환, 즉, <span class="font_highlight">**해당 변수를 그 자체로 사용할 수 있는 객체를 반환**</span>한다는 말입니다.

즉, 참조의 반환 값은 임시 참조 변수라 R-Value로 생각할 수도 있지만, 

**원본 변수를 가리키는 별칭**으로 본다면, 임시 참조 변수를 원본 변수로 볼 수 있고, 이는 메모리상의 위치를 가리키기 때문에 <span class="keyword">**L-Value**</span>입니다.

(아직, R-Value 참조를 언급하기 전이기 때문에, 기존 변수의 참조는 L-Value 참조로 보겠습니다.)

[참조와 포인터 (Reference & Pointer)](/posts/%EC%B0%B8%EC%A1%B0%EC%99%80-%ED%8F%AC%EC%9D%B8%ED%84%B0-reference-pointer/){:target="_blank"}

따라서, <span class="font_highlight">메모리 주소를 가리키는 변수(원본 변수의 별칭)를 리턴</span>하므로 <span class="keyword">**L-Value**</span> 입니다.

이는, [포인터 참조 연산자(&)](#4-포인터-참조)와는 다른 <span class="keyword">**참조 변수**</span>입니다.

> 사실, **참조**란 단어는 한글로 사용하기에는 조금 어색할 수 있습니다.
>
> 포인터에서 사용되는 참조는 메모리 주소를 가져오는 것입니다.
>  
> 하지만, C++에서 도입된 **참조 변수**에서 사용된 "참조"는 **원본 변수를 "참조하는" 별칭을 만드는 것**을 의미하는 것으로, 원본 변수 자체라고 봐도 될 것 같습니다.
> 
> 즉, 여기에서 말하는 **참조를 반환하는 함수**는 해당 **원본 변수에 바로 접근할 수 있는 객체를 반환하는 함수**를 뜻합니다.
> 
> [포인터에서 참조와 역참조](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){: target="_blank"}
> 
> [참조와 포인터 (Reference & Pointer)](/posts/%EC%B0%B8%EC%A1%B0%EC%99%80-%ED%8F%AC%EC%9D%B8%ED%84%B0-reference-pointer/){: target="_blank"}
>
{: .prompt-tip}

```cpp
int& refInt()
{
	static int a = 10;
	return a;
}
int main()
{
	refInt() = 20;
}
```

반면에 **포인터를 반환하는 함수**는 <span class="font_highlight">메모리 주소를 가진 임시 객체를 리턴</span>하지만, 이는 메모리를 참조할 수 없는 임시 객체로, <span class="keyword">**R-Value**</span>로 봐야 합니다.

다만, 포인터의 역참조(\*)를 통해 <span class="keyword">**L-Value**</span>처럼 쓸 수 있습니다. 즉, 반환된 임시 포인터는 메모리 주소를 가지고 있으므로 이 주소를 사용해 다른 값을 할당할 수도 있습니다.

```cpp
int* rtnPtr()
{
	static int a = 10;
	return &a;
}
int main()
{
	*rtnPtr() = 20;
}
```

정리하자면, 

- **참조를 반환하는 함수**는 <span class="font_highlight">메모리 주소를 가리키는 변수(원본 변수의 별칭)를 리턴</span>하기 때문에 <span class="keyword">**L-Value**</span>이고,
- **포인터를 반환하는 함수**는 <span class="font_highlight">메모리 주소를 가진 임시 객체를 리턴</span>하므로 <span class="keyword">**R-Value**</span>로 봐야 합니다. (임시 객체 자체는 메모리 주소를 참조할 수 없습니다.)

아래 코드의 결과는 각각 20, 20으로 할당되어 출력됩니다.

```cpp
#include <iostream>
using namespace std;

int& refInt() {
	static int a = 10;
	return a;
}
int* rtnPtr()
{
	static int a = 10;
	return &a;
}

int main()
{
	refInt() = 20;
	*rtnPtr() = 20;
	cout << refInt() << endl;
	cout << *rtnPtr() << endl;
}
```

![참조를 반환하는 함수와 포인터를 반환하는 함수 결과](https://i.postimg.cc/jjqWhtCn/image.png){: w="285" h="89"}

## 7. 정리 및 나머지 종류

즉, <span class="important">**표현식의 결과에 따라**</span> 메모리상에 위치가 있다면 <span class="keyword">**L-Value**</span>, 메모리상에 위치 없이 해당 표현식에서만 사용되는 경우에는 <span class="keyword">**R-Value(PR-Value)**</span>라고 할 수 있습니다.

```cpp
int rtnInt() { return 1; }
int& refInt() {
  static int a = 10;
  return a;
}
int* rtnPtr()
{
  static int a = 10;
  return &a;
}

int main() {
  // 1. 변수: 변수는 메모리 상에 위치가 있고 참조할 수 있으므로 L-Value입니다.
  // 2. 리터럴: 리터럴은 컴파일 타임에 결정되는 상수이므로 메모리 상에 명확한 위치가 없는 pR-Value입니다.
  int a = 10;

  // 3. 배열 요소: 배열의 각 요소는 메모리 상에 위치가 있고 참조할 수 있으므로 L-Value입니다.
  int arr[5];
  arr[0] = 1;

  // 4. 포인터 참조: 포인터 참조 연산자(&)는 변수의 주소를 가져오는 R-Value입니다.
  // 5. 포인터 역참조: 포인터 역참조(*)는 메모리 상의 위치를 가리키는 L-Value입니다.
  int* ptr = &a;
  *ptr = 30;

  // 6. 비참조 타입을 반환하는 함수: 비참조 타입을 반환하는 함수의 반환값은 pR-Value입니다.
  int num = rtnInt();

  // 7. 참조를 반환하는 함수: 참조를 반환하는 함수의 반환값은 L-Value입니다.
  refInt() = 20;
  // 8. 포인터를 반환하는 함수: 포인터를 반환하는 함수의 반환값은 R-Value입니다.
  *rtnPtr() = 20;

  // 9. 멤버 접근 연산자(.): 객체의 멤버에 대한 접근도 L-Value입니다.
  struct MyStruct
	{
      int x;
  };
  MyStruct myStruct;
  myStruct.x = 50;

  // 10. 전위 증감 연산자: 전위 증감 연산자는 변수의 값을 증감시키고 그 값을 반환합니다. 따라서 변수 그 자체로 반환하면 되기 때문에 L-Value입니다.
  ++a = 10;
  --a = 20;

  // 11. 후위 증감 연산자: 후위 증감 연산자는 변수의 현재 값을 임시로 저장한 후, 변수의 값을 증가시키거나 감소시킵니다. 그리고 임시로 저장한 원래 값을 반환합니다.
  // 따라서, 임시로 저장한 값을 반환하기 때문에 R-Value입니다.
  arr[1] = a++;

  // 12. 람다 표현식: 람다 표현식은 이름이 없는 임시 함수 객체를 생성하므로 pR-Value입니다.
  auto lambda = []() { return 42; };

  // 13. 캐스팅 연산: 캐스팅 연산의 결과는 대부분 R-Value입니다.
  // 캐스팅 연산은 기존 변수의 값을 새로운 타입으로 변환하여 임시 값을 생성하고 새로운 변수에 저장하는 경우가 많기 때문입니다.
  double e = static_cast<double>(a);  // a의 값을 double로 변환하여 그 값을 e에 저장.
	// 다만, const_cast와 reinterpret_cast는 L-Value를 반환할 수 있습니다.

  // 14. 삼항 연산자: 삼항 연산자의 경우에는 결과에 따라 L-Value와 pR-Value가 혼재할 수 있습니다.
  int d = (a > 0) ? a : 42;  // (a > 0) ? a : 42는 pR-Value (a가 L-Value이지만 42가 pR-Value이므로)
  (a > 0) ? a : d = 42;      // (a > 0) ? a : d = 42는 어떤 결과가 나와도 L-Value (a와 d가 모두 L-Value이므로)
}
```

---
# 참고

[포인터 (Pointer)](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/)

[참조와 포인터 (Reference & Pointer)](/posts/%EC%B0%B8%EC%A1%B0%EC%99%80-%ED%8F%AC%EC%9D%B8%ED%84%B0-reference-pointer/)

---

[[L_Value & R_Value] 2. 우측 값 참조와 이동(move semantics)](/posts/l_value-&-r_value-2.-%EC%9A%B0%EC%B8%A1-%EA%B0%92-%EC%B0%B8%EC%A1%B0%EC%99%80-%EC%9D%B4%EB%8F%99(move-semantics)/){:target="_blank"}

[[L_Value & R_Value] 3. C++ 11 값 범주 (Lvalue, Xvalue, Prvalue)](/posts/l_value-&-r_value-3.-c++-11-%EA%B0%92-%EB%B2%94%EC%A3%BC-(lvalue,-xvalue,-prvalue)/){:target="_blank"}