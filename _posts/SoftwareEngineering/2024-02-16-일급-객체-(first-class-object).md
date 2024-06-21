---
title: 일급 객체 (First-class object)
date: 2024-02-16 18:17:25 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 소프트웨어 공학
  - 객체 지향
math: false
type: SoftwareEngineering
keywords:
  - 일급 객체
  - First-class object
---

일급 객체(First-class object)란, 프로그래밍 언어에서 특정 개체가 다른 개체와 동일하게 취급될 수 있는 속성을 의미합니다.

일급 객체는 다음과 같은 속성을 가집니다.

1. <span class="important">**변수에 할당**</span>될 수 있습니다.
2. <span class="important">**함수의 인자**</span>로 전달될 수 있습니다.
3. <span class="important">**함수의 반환값**</span>으로 사용될 수 있습니다.

일급 객체의 개념은 주로 함수형 프로그래밍에서 강조되지만, 객체 지향 프로그래밍과 같은 다른 패러다임에서도 중요하게 사용됩니다.

예를 들어, C++의 함수 포인터와 콜백 함수는 일급 객체의 좋은 예입니다. 특정 **함수의 주소를 저장**하고, 함수의 **인자로 전달**될 수 있으며, **반환** 할수도 있는 함수 포인터는 일급 객체입니다.

```csharp
#include <iostream>
int Add(int a, int b)
{
    return a + b;
}

int Multiply(int a, int b)
{
    return a * b;
}

// 함수 포인터를 반환하는 함수
int (*GetOperation(char op))(int, int)
{
    if (op == '+') 
    {
        return Add;
    }
    else if (op == '*') 
    {
        return Multiply;
    }

    return nullptr;
}

int main()
{
    int(*operation)(int, int) = GetOperation('+');

    if (operation != nullptr) 
    {
        std::cout << "결과: " << operation(5, 3) << std::endl; // Add 함수 호출
    }

    operation = GetOperation('*');
    if (operation != nullptr) 
    {
        std::cout << "결과: " << operation(5, 3) << std::endl; // Multiply 함수 호출
    }
}
```
![일급 객체와 콜백 함수 결과](https://i.postimg.cc/pTrMC1DB/image.png)