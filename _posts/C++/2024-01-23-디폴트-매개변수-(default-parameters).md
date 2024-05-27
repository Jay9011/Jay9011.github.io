---
title: 디폴트 매개변수 (Default parameters)
date: 2024-01-23 01:45:11 +0900
categories:
  - 풀스택 개발
  - C++
tags:
  - c
math: false
type: C++
keywords:
  - 디폴트 매개변수
  - Default parameters
---

디폴트 매개변수(default parameters)는 프로그래밍에서 <span class="font_highlight">함수를 선언할 때 매개변수에 미리 **기본값**을 설정</span>하는 것으로써, 함수 호출 시 <span class="font_highlight">**해당 매개변수를 생략**할 수 있게 해주는 기능</span>입니다.

이는 함수의 사용성을 향상시키고 코드의 간결성을 높여주며, 여러 함수 오버로드를 만드는 것보다 간편할 수 있습니다.

디폴트 매개변수는 주로 함수 **선언 시에 지정**되며, 함수 정의에서는 일반적으로 생략됩니다.

디폴트 값은 **왼쪽에서 오른쪽으로 차례대로 적용**되므로, <span class="font_highlight">한 매개변수에 디폴트 값을 제공하면 그 오른쪽에 있는 모든 매개변수도 디폴트 값을 가져야 합니다</span>.

```cpp
void printDetails(string name, int age = 30, string country = "South Korea")
{
    cout << "Name: " << name << ", Age: " << age << ", Country: " << country << endl;
}
int main()
{
    printDetails("John");  // John, 30, South Korea
    printDetails("Alice", 25);  // Alice, 25, South Korea
    printDetails("Mike", 35, "USA");  // Mike, 35, USA
}
```

# 디폴트 매개변수의 주의점

- **정의와 선언의 분리**: 일반적으로 디폴트 매개변수는 함수의 선언에서만 지정해야 하며, 함수의 정의에서는 디폴트 값을 제공하지 않는 것이 좋습니다. 이렇게 함으로써, 디폴트 값이 여러 번 지정되어 발생할 수 있는 혼란을 방지할 수 있습니다.
- **함수 오버로딩과의 충돌**: 디폴트 매개변수와 함수 오버로딩을 함께 사용할 때 주의해야 합니다. 디폴트 매개변수가 있는 함수와 매개변수의 수가 다른 오버로드된 함수가 있을 경우, 호출 시 모호성이 발생할 수 있습니다.
- **프로그램의 가독성**: 디폴트 매개변수를 사용할 때는 함수의 호출이 간결해지지만, 때로는 코드의 가독성이 떨어질 수 있습니다. 함수 호출만 보고는 호출되는 함수의 행동을 정확히 예측하기 어려울 수 있으므로, 디폴트 매개변수의 사용은 신중하게 결정해야 합니다.
