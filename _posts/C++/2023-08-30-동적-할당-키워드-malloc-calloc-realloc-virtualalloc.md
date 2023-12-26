---
title: 동적 할당 키워드 (new, malloc, calloc, realloc, virtualAlloc)
date: 2023-08-30 20:18:53 +0900
categories:
    - 프로그래밍
    - C++
tags:
    - c
    - 데이터
    - 메모리
    - 동적 할당
keywords:
    - calloc
    - malloc
    - new
    - realloc
    - 동적 할당
    - 메모리
    - virtualAlloc
---

# **동적 할당 (Dynamic Allocation)**

<span class="keyword">**동적 할당**</span>은 <span class="font_highlight">**프로그램 실행 중에 필요한 메모리 공간을 할당**</span>하는 프로그래밍 기법입니다.

정적 할당과 달리, 동적 할당은 실행 시간에 메모리의 크기와 위치를 결정하므로 더 유연하게 메모리를 관리할 수 있습니다.

동적 할당의 주요 장점은 **메모리의 효율적인 사용**입니다.
필요한 만큼의 메모리를 할당하고 해제할 수 있으므로, 불필요한 메모리 낭비를 줄일 수 있습니다. <br> 이때, 동적으로 할당된 메모리는 <span class="keyword">**힙(heap)**</span> 영역에 저장됩니다.

C++에서는 **`malloc`**, **`calloc`**, **`realloc`**, **`new`** 등의 함수를 사용하여 동적 할당을 수행할 수 있으며, 할당된 메모리는 **`free`** 또는 **`delete`**로 해제해야 합니다.

---

# malloc

- 메모리의 힙 영역에 **바이트 단위**로 할당하고, 할당된 메모리의 **시작 주소**를 반환합니다.
- 할당된 메모리는 **초기화 되지 않은 상태**입니다.
- 메모리 해제는 `free`

```bash
malloc(원하는 크기)
```

```cpp
#include <stdlib.h>

int main() 
{
    int* arr = (int*)malloc(10 * sizeof(int)); // 10개의 int 크기만큼 메모리 할당
    //...
    free(arr); // 메모리 해제
}
```

---

# calloc

- malloc과 마찬가지로 힙 영역에 **바이트 단위**로 할당하고, 할당된 메모리의 **시작 주소**를 반환합니다.
- 할당을 원하는 메모리 크기를 **자료형의 크기**와 **개수** 두 개의 인자로 받습니다.
- **0으로 초기화** 해줍니다. (초기화 과정이 포함되므로 malloc 보다는 느릴 수 있습니다.)
- 메모리 해제는 `free`

```bash
calloc(원하는 개수, 원하는 크기)
```

```cpp
#include <stdlib.h>

int main() 
{
    int* arr = (int*)calloc(10, sizeof(int)); // 10개의 int 크기만큼 메모리 할당
    //...
    free(arr); // 메모리 해제
}
```

---

# realloc

- 이미 할당된 **메모리의 크기를 바꾸어 재할당**할 때 사용합니다. 이때, 크기가 **변경된 메모리의 시작 주소**를 반환합니다. (메모리 재할당시 메모리 주소가 변경될 수 있습니다.)
- 재할당 받고 싶은 **메모리 주소**와 **크기**를 인자로 받습니다.
- 메모리를 재할당 받을 위치에 충분한 공간이 있다면 바로 이어서 추가 메모리 공간을 할당해주고, **충분하지 않다면 새로운 공간을 할당**해줍니다.
- 따라서 크기 변경 과정에서 **메모리 복사**가 일어날 수 있으므로 비효율적일 수 있습니다.

```bash
realloc(메모리 주소, 원하는 메모리 크기)
```

```cpp
#include <stdlib.h>

int main() 
{
    int* arr = (int*)malloc(5 * sizeof(int)); // 5개의 int 크기만큼 메모리 할당
    //...
    arr = (int*)realloc(arr, 10 * sizeof(int)); // 메모리 크기 재조정 (10개의 int 크기로 확장)
    //...
    free(arr); // 메모리 해제
}
```

---

# new

- 힙 영역의 메모리를 **자료형 단위**로 할당합니다. (**클래스 단위**도 포함한 자료형 단위)
- 새로 메모리를 할당할 때, **생성자**를 호출하므로 **초기화 과정**이 자동으로 이루어집니다.
- 메모리 해제는 `delete`

```bash
new 자료형
```

```cpp
int main()
{
    int* num = new int; // int 타입의 동적 객체 생성
    //...
    delete num; // 객체 해제
}
```

---

# virtualAlloc

- Windows 운영체제에서 <span class="keyword">**가상 메모리(Virtual Memory)**</span>에 공간을 할당하고 관리합니다.
- **페이지 단위**로 메모리를 할당할 수 있으며, 특정 주소에 메모리를 할당할 수 있습니다.
(보통 예약된 페이지가 아니라면 **`NULL`**로 넘겨 비어있는 충분한 공간을 할당 받습니다.)
- 연속적으로 메모리를 할당하고 해제하는 과정에서 발생하는 **<span class="important">메모리 단편화</span>를 극복**할 수 있습니다.
- **대용량의 메모리**가 필요한 경우 유용합니다.
- 메모리에 대한 **예약**이 가능하고, 예약 상태 및 **엑세스 권한** 지정이 가능합니다.
- **메모리 예약** 플래그는 다음과 같습니다.
    - **`MEM_COMMIT`**: 물리 메모리를 할당하고 페이지가 **물리 메모리에 매핑**되며, 초기 값은 0이 됩니다.
    - **`MEM_RESERVE`**: **가상 주소 공간을 예약**하지만 물리 메모리를 할당하지 않습니다. <br> 따라서, **`MEM_RESERVE`**만 사용하면 해당 페이지에 액세스할 수 없으며, **`MEM_COMMIT`**을 사용해야 페이지에 액세스할 수 있습니다.
- **엑세스 권한**은 다음과 같습니다.
    - **`PAGE_NOACCESS`**: 페이지에 대한 모든 액세스를 차단합니다.
    - **`PAGE_READWRITE`**: 페이지에 대한 읽기 및 쓰기 액세스를 허용합니다.
    - **`PAGE_READONLY`**: 페이지에 대한 읽기 액세스만 허용합니다.
- 메모리 해제는 `virtualfree`

```bash
VirtualAlloc(NULL(메모리 시작 지점), 4KB 단위의 크기, COMMIT 또는 RESERVE, 엑세스 권한)
```

```cpp
#include <windows.h>

int main() {
    LPVOID pMemory = VirtualAlloc(NULL, 4096, MEM_COMMIT, PAGE_READWRITE); // 4096 바이트 크기의 가상 메모리 할당
    //...
    VirtualFree(pMemory, 0, MEM_RELEASE); // 메모리 해제
}
```

---

# 참고

[VirtualAlloc (가상 메모리 할당 및 예약)](/posts/virtualalloc-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC-%ED%95%A0%EB%8B%B9-%EB%B0%8F-%EC%98%88%EC%95%BD/){:target="_blank"}