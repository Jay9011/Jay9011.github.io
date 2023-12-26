---
title: 허상 포인터 (Dangling pointer) 발생 원인과 예방 방법
date: 2023-09-12 17:49:30 +0900
categories:
    - 프로그래밍
    - C++
tags:
    - c
    - 포인터
    - 메모리
keywords:
    - pointer
    - 포인터
    - 허상 포인터
    - Dangling pointers
---

<span class="keyword">**Dangling pointers**</span>는 <span class="font_highlight">**유효한 객체를 가리키지 않는 포인터**</span>를 말합니다.

**객체를 가리키고 있던 메모리가 해제된 후에 그 메모리 주소를 계속 참조하는 경우**와 같은 경우로, 이 메모리 주소를 역참조하는 경우 정의되지 않은 동작(undefined behavior)이 발생할 수 있습니다.


> **정의되지 않은 동작(undefined behavior)**이란, **프로그램이 어떻게 동작할지 예측할 수 없는 상태**입니다. <br>
> 정의되지 않은 동작에 의해 다음과 같은 경우가 발생할 수 있습니다. <br>
> 1. **프로그램 크래시**: 가장 일반적으로 생각되는 결과로, 프로그램이 중단되고 오류 메시지가 출력될 수 있습니다. <br>
> 2. **잘못된 결과**: 프로그램이 계속 실행되지만, 예상과 다른 결과를 출력하거나 파일, 데이터베이스 등에 잘못된 데이터를  저장할 수 있습니다.
> 3. **무한 루프**: 프로그램이 끝나지 않고 계속 실행되는 상태에 빠질 수 있습니다. <br>
> 4. **메모리 누수**: 프로그램이 계속 실행되면서 점점 더 많은 메모리를 소비할 수 있습니다. <br>
{: .prompt-warning .small}

# 발생 원인

<span class="keyword">**Dangling pointers**</span>는 **객체가 삭제되거나 할당 해제 될 때** 발생하는 경우가 가장 흔합니다.

참조를 가지고 있는 객체가 삭제되거나 할당 해제되면, 포인터의 값이 수정되지 않아 **메모리 위치를 계속 가리키고 있는데**, 이 메모리가 다른 곳에서 사용되고 있다면 문제가 발생할 수 있습니다.

## 1. 메모리 해제 후 재참조

가장 일반적인 경우로, **`delete`**나 **`free`**를 사용하여 **메모리를 해제한 후**, 해당 메모리를 가리키는 **포인터가 아직 해당 주소를 가리키고 있는 경우**.

```cpp
int *ptr = new int(10);
delete ptr;
// ptr은 이제 Dangling pointer입니다.
*ptr = 20;  // Undefined Behavior
```

## 2. 스택 메모리의 포인터 반환

함수가 스택에 할당된 로컬 변수의 주소를 반환하면, **함수가 종료되고 스택 프레임이 해제될 때**, 스택에 할당된 주소를 가리키던 포인터는 Dangling pointer가 됩니다.

```cpp
int* func() 
{
    int x = 10;
    return &x;  // x는 스택에 있으므로 이 포인터는 Dangling pointer가 됩니다.
}
```

## 3. 객체의 생명주기가 끝난 후 참조

객체가 **스코프를 벗어나거나 명시적으로 삭제된 후**, 그 객체의 멤버를 가리키는 포인터가 있을 경우입니다.

```cpp
void func()
{
	int *ptr;
	{
	    int x = 10;
	    ptr = &x;
	}
	// x는 스코프를 벗어났으므로 ptr은 이제 Dangling pointer입니다.
}
```

## 4. 이중 해제

**이미 해제된 메모리를 다시 해제하려고 할 때**, 다른 포인터가 그 메모리를 참조하고 있을 경우 Dangling pointer가 될 수 있습니다.

```cpp
int *ptr1 = new int(10);
int *ptr2 = ptr1;  // ptr2는 ptr1과 같은 주소를 가리킵니다.
delete ptr1;
delete ptr2;  // ptr2는 이제 dangling pointer입니다.
```

실제 한 함수 내에서 같은 메모리의 주소를 두 번 참조하는 경우는 드물지만, 서로 다른 함수에서 같은 메모리 주소를 참조할 수 있으므로, 이 경우도 드물지 않게 발생하기도 합니다.

# 문제점

1. 현재 메모리가 다른 데이터로 채워져 있을 수 있습니다.
<br> 이 데이터를 임의로 수정하려고 하면 정의되지 않은 동작(undefined behavior)이 발생할 수 있습니다.
2. 메모리가 다른 프로세스에 재할당 되고, 허상 포인터를 역참조하려고 하면 segmentation faults가 발생할 수 있습니다.
3. 프로그램이 시스템 메모리를 잘못 수정하게 되면, 이로 인해 시스템이 불안정해질 수 있습니다.



> **Segmentation Fault (세그멘테이션 폴트) 란?**
> 
> Segmentation Fault는 **프로세스가 자신에게 할당되지 않은 메모리에 접근하려고 할 때 발생하는 오류**입니다.
> <br> 이는 운영체제가 프로세스의 메모리를 보호하기 위한 메커니즘의 일종으로, Segmentation Fault가 발생하면 프로세스는 즉시 종료됩니다.
> 
> <span class="small">Segmentation Fault의 주요 발생 상황으로는 다음과 같은 경우가 있습니다.</span>
> 1. **Null Pointer Dereferencing**: **`nullptr`** 또는 초기화되지 않은 포인터를 역참조할 때
> 2. **Array Index Overflow**: 배열의 범위를 벗어나는 인덱스에 접근할 때
> 3. **Dangling Pointer**: 메모리가 해제된 후에 그 메모리 주소를 계속 참조할 때
> 4. **Stack Overflow**: 재귀 함수에서 종료 조건이 없거나 스택에 너무 많은 데이터를 할당할 때
{: .prompt-info}

# 예방 방법

다른 언어에서는 **가비지 컬렉션**을 통해 허상 참조를 방지하고, C++에서는 <span class="important">**스마트 포인터**</span>와 같은 참조 카운팅 또는 추적을 통해 어떤 객체가 더 이상 참조되지 않는지 확인해야 합니다.

또한, 메모리를 해제한 후에는 관련된 모든 포인터를 **`nullptr`**로 설정하는 것이 좋습니다.


---

# 참고

[포인터 (Pointer)](/posts/%ED%8F%AC%EC%9D%B8%ED%84%B0-pointer/){:target="_blank"}

[참조와 포인터 (Reference & Pointer)](/posts/%EC%B0%B8%EC%A1%B0%EC%99%80-%ED%8F%AC%EC%9D%B8%ED%84%B0-reference-pointer/){:target="_blank"}

[스마트 포인터 (unique_ptr, shared_ptr, weak_ptr, auto_ptr)](/posts/%EC%8A%A4%EB%A7%88%ED%8A%B8-%ED%8F%AC%EC%9D%B8%ED%84%B0-unique-ptr-shared-ptr-weak-ptr-auto-ptr){:target="_blank"}