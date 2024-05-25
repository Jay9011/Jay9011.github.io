---
title: 구조체 패딩 (Structure Padding)
date: 2023-12-28 23:29:45 +0900
categories:
  - Full Stack 개발
  - C++
tags:
  - c
  - 구조체
  - CPU
math: false
type: C++
keywords:
  - Structure Padding
  - 구조체 패딩
  - 패딩 비트
  - 네트워크
---

**구조체 패딩(Structure Padding)** 은 **CPU가 데이터를 읽을 때, 읽기 쉬운 위치에 배치하기 위해** 컴파일러가 구조체 **멤버 사이에 추가 공간**을 삽입하는 기법을 말합니다.

즉, 메모리 공간을 일부 희생하더라도 <span class="font_highlight">**CPU가 더 쉽고 빠르게 데이터를 읽을 수 있도록** 빈 공간을 만드는 것</span>입니다.

예를 들어, 다음과 같이 구조체를 만들고 결과를 보도록 합시다.

```cpp
struct Struct01
{
	char char1;		// 1byte
	int int1;		// 4byte
	float float1;	// 4byte
	double double1;	// 8byte
	int int2;		// 4byte
	// 총 21byte로 예상
	// 하지만, 1 + 3(padding) + 4 + 4 + 4(padding) + 8 + 4 + 4(padding) = 32 byte
};

int main()
{
	printf("구조체 1의 크기: %d byte\n", sizeof(Struct01));	// 32byte
}

```

![구조체 패딩 결과](https://i.postimg.cc/NM3zDXVg/2.webp){: w="228" h="34"}

멤버들의 자료형 크기 총 합은 21byte지만, 그 결과는 32byte로 출력되는 것을 확인할 수 있습니다.

# 구조체 패딩의 필요성

CPU는 메모리에서 데이터를 읽을 때 한 번에 특정 크기만큼 읽을 수 있습니다. <span class="small">(32bit는 4byte, 64bit는 8byte)</span> <br>
만약, 구조체가 패딩이 되지 않았다면 어떨지 한 번 생각해 보겠습니다.

예를 들어, `int`는 4byte로, 32bit 컴퓨터가 한 번에 읽을 수 있어서 매우 빠르게 읽을 수 있는 기본 자료형입니다.<br>
하지만 위와 같이 구조체가 저장되어 있다면, `int`를 읽기 위해 총 2번의 접근이 필요하게 될 것입니다.

![패딩이 안 된 경우](https://i.postimg.cc/jdK8jRYk/2.webp){: w="899" h="140"}

하지만, 아래와 같이 패딩을 추가하면 그 만큼의 <span class="font_highlight">**메모리 공간을 희생**하겠지만, CPU가 메모리에 접근하는 수가 줄어 **성능을 올릴 수**</span> 있습니다.

![패딩이 된 경우](https://i.postimg.cc/j2j4zSqs/image.webp){: w="906" h="142"}

## 효율적으로 공간 설계하기

구조체는 **제일 큰 자료형을 기준**으로 패딩을 합니다.

실제로 Visual Studio를 사용해 확인해보면 다음과 같이 두 결과를 볼 수 있습니다.

```cpp
struct Struct01
{
	char char1;
	int int1;
	float float1;
	double double1;	// 8byte
	int int2;
};
struct Struct03
{
	// 제일 큰 자료형이 int인 경우
	char char1;
	int int1;		// 4byte
	float float1;
	int int2;
};
```

![visual studio에서](https://i.postimg.cc/prXYvX3w/13.webp){: w="950" h="157"}

위처럼 각 구조체의 패딩은 제일 큰 자료형을 기준으로 하는 것을 볼 수 있습니다. <br>
하지만, `Struct01`을 보니 `int2`가 `float1`옆에 들어가면 좋을 것 같다는 것을 알 수 있습니다.

따라서, 다음과 같이 정렬을 하면 `32byte`를 `24byte`로 줄일 수 있는 것을 볼 수 있습니다.

```cpp
struct Struct02
{
	// 정렬된 구조체
	char char1;		// 1byte
	int int1;		// 4byte
	int int2;		// 4byte
	float float1;	// 4byte
	double double1;	// 8byte
	// 1 + 3(padding) + 4 + 4 + 4 + 8 = 24byte
};
```

![정렬된 구조체](https://i.postimg.cc/7LKZcHMG/2.webp){: w="609" h="135"}

즉, 개발자는 **구조체** 혹은 **클래스**를 정의할 때, 멤버 순서에 유의하여 **패딩 비트를 최소한**으로하고 **공간을 최대한 효율적**으로 설계하는 것이 필요합니다.

# 패딩 비트 줄이기

만약 **네트워크**를 통해 구조체를 전송한다면 어떨까요?

구조체의 크기가 크고, 패딩 비트가 많고, 대부분의 데이터가 이 구조체로 이루어져 있다면 네트워크를 통해 요구되는 데이터도 크고, 그에따라 **데이터 소모량**도 커지게 될 것입니다.

즉, 구조체 패딩을 포함하면 불필요한 데이터를 전송하게 되어 **네트워크 대역폭을 낭비**할 수 있기 때문에, 패딩을 제거하고 필요한 데이터만을 전송하는 것도 중요합니다.

구조체 패딩을 제거하는 것으로 아마 다음과 같은 효과를 얻을 수 있을 것입니다.

- **네트워크 대역폭** 절약: 데이터 크기를 줄여 네트워크 대역폭을 절약할 수 있을 것입니다.
- **전송 속도** 향상: 마찬가지로, 데이터 크기가 줄어들면 전송 시간 또한 단축 될 것입니다.

## `#pragma pack` 지시어 사용

`#pragma pack` 지시어를 사용하면 컴파일러에게 **구조체 정렬 크기**(경계 크기)를 지정할 수 있습니다.

```cpp
#pragma pack(4) // 4byte 경계로 정렬

struct Struct01
{
	char char1;		// 1byte
	int int1;		// 4byte
	float float1;	// 4byte
	double double1;	// 8byte
	int int2;		// 4byte
	// 하지만, 1 + 3(padding) + 4 + 4 + 8 + 4 = 24 byte
};
```

![pragma pack 적용](https://i.postimg.cc/1RrvhjYQ/pragma-pack.webp){: w="346" h="211"}

## `#pragma pack` 사용법

- `#pragma pack(n)`: 구조체 멤버들이 `n` byte 경계로 정렬되도록 요청합니다.
- `#pragma pack(push, n)`: 현재 패킹 규칙을 스택에 저장하고, 새로운 패킹 규칙 `n`을 적용합니다.
- `#pragma pack(pop)`: 이전에 저장된 패킹 규칙을 스택에서 꺼내 적용합니다.

---

# 참고

[구조체(struct)](/posts/%EA%B5%AC%EC%A1%B0%EC%B2%B4(struct)/)