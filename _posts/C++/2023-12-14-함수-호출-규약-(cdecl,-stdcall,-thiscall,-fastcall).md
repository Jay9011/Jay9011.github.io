---
title: 함수 호출 규약 (cdecl, stdcall, thiscall, fastcall)
date: 2023-12-14 20:53:47 +0900
categories:
  - 풀스택 개발
  - C++
tags:
  - c
  - 메모리
  - 스택
  - 함수
  - 운영체제
math: false
type: C++
keywords:
  - cdecl
  - fastcall
  - stdcall
  - thiscall
  - 함수 호출 규약
---

<span class="keyword">**함수 호출 규약(Function Calling Convention)**</span>이란, **함수를 호출할 때 <span class="font_highlight">인자를 어떻게 전달</span>하고, 함수가 반환될 때 <span class="font_highlight">어떻게 스택을 정리</span>하는지에 대해 정의한 규칙**입니다.

이 규약은 프로그래밍 언어, 컴파일러, 운영 체제, 아키텍처에 따라 다를 수 있습니다.

# 함수 호출 규약(Function Calling Convention)

사실 이전 포스트인 [스택 프레임(Stack Frame)과 콜스택(Call Stack)](/posts/%EC%8A%A4%ED%83%9D-%ED%94%84%EB%A0%88%EC%9E%84(stack-frame)%EA%B3%BC-%EC%BD%9C%EC%8A%A4%ED%83%9D(call-stack)/){: target="_blank"}에서 매개 변수가 어떻게 처리되는지 말하지 않았습니다.

사실, 매개 변수의 처리는 이 **함수 호출 규약**에 따르게 됩니다.

## 매개 변수 처리

1. 함수 실행 전, 스택을 통해 매개 변수를 전달합니다.
2. 함수 종료시 스택에 저장된 매개 변수를 처리해야 합니다. 이때, 매개 변수의 크기 만큼 ESP 값을 증가(이전 위치로 이동하게 끔)해서 매개 변수들을 무효한 값으로 처리합니다. (매개 변수는 삭제되지 않습니다.)

---

# __cdecl

<span class="keyword">**cdecl(C Declaration)**</span>은 주로 C언어에서 사용하는 방식으로, **<span class="font_highlight">Caller(호출자)</span>에서 스택을 정리**하는 방식입니다.

![push를 통해 매개변수를 전달하고, add를 동해 esp를 옮겨 매개변수를 정리하는 모습](https://i.postimg.cc/qRFV5XwH/cdecl.png){: w="517" h="177"}
_push를 통해 매개변수를 전달하고, add를 동해 esp를 옮겨 매개변수를 정리하는 모습_

visual studio x86에서 실행하게 되면, 위와 같이 push를 통해 두 매개변수를 저장하고, 함수 호출 후, 종료 후에 ESP의 값을 더해주는 모습을 볼 수 있습니다.

따라서, 매개 변수를 집어 넣기 전의 위치로 돌아오게 됩니다.

(x64 아키텍처에서는 레지스터를 사용하는 방식이기 때문에 `add esp`를 보기 위해서는 x86으로 실행해야 합니다.)

## 장점

**`cdecl`**의 방식은 **<span class="font_highlight">Caller</span>가 매개 변수의 정보를 알고 있을 때, 유리**합니다.

가변 길이 매개변수를 가진 함수(printf와 같이)는 **매개변수의 정확한 수를 컴파일 시간에 알 수 없습니다**. 함수 내에서 특별한 값이나 첫 번째 매개변수를 통해 추론하는 수 밖에 없습니다.

따라서 이런 <span class="font_highlight">가변 길이 매개변수를 처리</span>할 때, **`cdecl`** 호출 규약이 유리합니다.

**`cdecl`**규칙에서 함수는 매개변수의 정확한 수를 알 필요가 없습니다. Caller가 매개 변수의 정보를 알고 있기 때문에 Caller가 함수 호출이 끝난 후, 매개 변수를 정리해줄 수 있기 때문입니다.

# __stdcall

<span class="keyword">**stdcall(STandard call)**</span>은 **<span class="font_highlight">Callee(피호출자)</span>에서 스택을 정리**하는 방식입니다.

![Caller에서는 push로 매개 변수만 넣어줄 뿐, esp를 조작하는 동작은 안보인다.](https://i.postimg.cc/zf01n1RB/stdcall-01.png){: w="477" h="171"}
_Caller에서는 push로 매개 변수만 넣어줄 뿐, esp를 조작하는 동작은 안보인다._

![리턴 후 esp를 8만큼 증가시킨다는 의미.](https://i.postimg.cc/MGWSMGHN/stdcall-02.png){: w="383" h="96"}
_리턴 후 esp를 8만큼 증가시킨다는 의미._

이전의 `_cdecl`과는 달리 **Caller**에서는 매개 변수만 넣어 줄 뿐, ESP를 조작하는 동작은 보이지 않습니다.

다만, **Callee**에 가면 ret 옆에 못 보던 숫자가 추가된 것이 보이는데, 어딘가 익숙한 숫자입니다.

바로 여기 `ret`에서 리턴시 ESP를 증가시켜 본인이 사용하던 매개 변수 값을 무효화 시키는 것입니다.

## 장점

이러한 방식의 장점에는 **<span class="font_highlight">Callee</span>가 본인이 사용한 만큼의 매개 변수의 길이 만큼** ESP를 옮겨주면 되기 때문에, **Caller가 함수 호출 후에 매개 변수를 정리하는 코드를 일일이 추가하지 않아도 된다**는 점입니다.

즉, <span class="font_highlight">전체 코드의 길이</span>가 줄어들 수 있다는 것입니다.

예를 들어, 다음과 같은 코드가 있다면…

```cpp
int sum(int a, int b)
{
	return a + b
}
int main()
{
	int a = sum(1, 2);
	int b = sum(1, 2);
	int c = sum(1, 2);
}
```

`_cdecl` 방식은 각각의 sum 호출 다음에 `ADD ESP 8` 이라는 코드가 세 번 들어갔을 겁니다.

하지만, `_stdcall` 방식을 쓰게 되면 어차피 `RET 8` 코드 하나만 있다면, 세 번의 호출에도 정상적으로 매개 변수를 처리 할 수 있으니 더 효율적으로 쓸 수 있다는 뜻입니다.

# __fastcall

<span class="keyword">**fastcall**</span>은 기본적으로 _stdcall 방식이지만, **함수의 첫 번째, 두 번째 매개 변수를 레지스터(`ECX`, `EDX`)를 사용**해 **더 빠르게 처리**할 수 있는 호출 방식입니다. (이후의 인자들은 전통적인 방식대로 스택을 통해 전달됩니다.)

다만, **`ECX`**와 **`EDX`** 레지스터는 사용되는 아키텍처에 따라 존재하지 않을 수 있기 때문에, 이식성과 호환성이 제한적입니다.

# __thiscall

<span class="keyword">**thiscall**</span>은 주로 C++에서 사용되는 함수 호출 규약으로, 기본적으로 _stdcall 방식이지만, **멤버 함수(클래스의 메서드) 호출에서 사용되는 방식**입니다.

C++에서 멤버 함수를 호출하면, **`ECX`** 레지스터를 통해 <span class="font_highlight">**객체의 포인터(this)**</span>가 전달되고, 멤버 함수(Callee)가 나머지 스택을 정리합니다.

물론, thiscall은 **비정적(non-static) 멤버 함수**에만 적용됩니다. 정적 멤버 함수나 일반 함수는 this 포인터가 필요 없기 때문에 다른 호출 규약을 사용합니다.

예를 들어, 다음과 같은 멤버 함수가 존재한다면,

```cpp
class MyClass {
public:
    void myMethod(int param);
};
```

**`myMethod()`** 함수를 호출할 때, **`this`** 포인터는 **`ECX`** 레지스터를 통해 전달되고, **`param`**은 **`스택`**을 통해 전달됩니다.

---

# 참고

[스택 프레임(Stack Frame)과 콜스택(Call Stack)](/posts/%EC%8A%A4%ED%83%9D-%ED%94%84%EB%A0%88%EC%9E%84(stack-frame)%EA%B3%BC-%EC%BD%9C%EC%8A%A4%ED%83%9D(call-stack)/)