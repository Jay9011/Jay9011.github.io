---
title: auto와 decltype - 타입 추론
date: 2024-02-14 17:04:33 +0900
categories:
    - 풀스택 개발
    - C++
tags:
    - c
    - 변수
    - 자료형
    - 편의성
math: false
type: C++
keywords:
    - auto
    - decltype
    - 타입 추론
---

# auto

C++11에서 도입된 `auto`키워드는 [타입 추론(Type Inference)](/posts/%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0(type-inference)%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%A0%95%EB%A6%AC/){: target="_blank" } 키워드로, <span class="font_highlight">변수의 **타입을 자동으로 유추**하는 기능</span>을 제공합니다.
<br>
이는 코드의 간결성을 높이고 프로그래머에게 타입 선언에서의 편의성을 제공합니다.

## 사용법

### 1. 기본 자료형 타입으로 사용

`auto` 키워드를 사용하면 컴파일러가 변수의 타입을 해당 변수의 **초기화 값**으로부터 유추합니다.

```cpp
auto x = 10; // x는 int 타입으로 유추됨
auto y = 3.14; // y는 double 타입으로 유추됨
auto str = "hello"; // str은 const char* 타입으로 유추됨
```

#### 범위 기반 for 루프

`auto`를 범위 기반 for 루프에서 사용하면, 컨테이너의 요소 타입을 자동으로 유추하기 때문에, 범위 기반 for 루프 같은 곳에서 유용하게 쓸 수 있습니다.

```cpp
std::vector<int> vec = {1, 2, 3, 4, 5};
for (auto& element : vec) {
    element *= 2;
}
```

### 2. 익명 메서드 타입으로 사용

`auto` 키워드는 람다 표현식과 같은 익명 메서드에서 유용하게 사용할 수 있습니다.

```cpp
auto lambda = [](auto a, auto b) {
    return a + b;
};
```

### 3. 함수의 반환 타입으로 사용

C++14부터는 함수의 반환 타입에도 `auto`를 사용할 수 있습니다.

```cpp
auto add(int a, int b) -> int {
    return a + b;
}
```

## 주의 사항

### 1. 초기화

`auto` 변수는 초기화 되는 값에 근거하여 타입을 추론하기 때문에 초기화가 필요합니다. 초기화 없이는 사용할 수 없습니다.

```cpp
auto x; // 오류: 초기화 필요
```

### 2. 유추된 타입 확인

`auto` 키워드를 사용하여 코드를 작성했다면, 디버깅 과정에서 유추된 타입을 확인하는 것이 중요합니다. 예상치 못한 타입이 유추될 수 있기 때문입니다.

```cpp
auto x = {1, 2, 3}; // x의 타입은 std::initializer_list<int>
```

# decltype

`decltype` 키워드는 C++11에서 도입된 타입 추론 기능으로, <span class="font_highlight">주어진 **표현식의 타입**을 추론</span>합니다.
<br>
이를 통해 변수의 선언, 함수 반환 타입 지정 등에서 표현식의 타입을 명확하게 지정할 수 있습니다.

## 사용법

`decltype` 키워드는 주어진 표현식의 타입을 추론하여 그 타입을 반환합니다. 이를 통해 변수를 선언하거나 함수 반환 타입을 지정할 수 있습니다.

```cpp
int x = 5;
decltype(x) y = 10; // x 표현식을 통해 추론한 y의 타입은 int
```

`decltype`은 표현식 자체의 타입을 유추하기 때문에, 참조나 상수성(const) 등을 포함한 정확한 타입을 추론할 수 있습니다.

```cpp
int x = 5;
int& ref = x;
const int& const_ref = x;

decltype(x) a; // int
decltype(ref) b = x; // int&
decltype(const_ref) c = x; // const int&
```

# auto와 decltype

특히, `auto` 키워드와 `decltype`의 조합은 조금 강력하게 사용될 수도 있습니다.

## 1. `decltype(auto)`의 사용

C++14부터 도입된 `decltype(auto)`는 **반환 타입 추론**을 더 정밀하게 수행할 수 있게 합니다. 이는 특히, 반환 타입이 참조 타입인지 값 타입인지 정확하게 반영하는 데 유용합니다.

```cpp
int x = 5;
int& ref = x;

decltype(auto) a = x; // int
decltype(auto) b = ref; // int&
```

## 2. 함수 반환 타입 지정

`decltype`은 함수의 반환 타입을 지정하는 데 유용합니다. 특히, 반환 타입이 복잡하거나 템플릿 함수에서 유용하게 쓸 수 있습니다.

```cpp
template <typename T, typename U>
auto add(T t, U u) -> decltype(t + u) {
    return t + u;
}
```

위 예제에서 `decltype(t + u)`는 `t`와 `u`의 합의 타입을 추론하여 반환 타입으로 지정합니다.

## 3. 람다 표현식에서의 사용

람다 표현식도 함수이기 때문에 반환 타입을 지정할 수 있는데, 이때, `decltype`을 사용 할 수도 있습니다.

```cpp
auto lambda = [](int a, int b) -> decltype(a + b) {
    return a + b;
};
```

---

# 참고

[타입 추론(Type Inference)에 대한 간단한 정리](/posts/%ED%83%80%EC%9E%85-%EC%B6%94%EB%A1%A0(type-inference)%EC%97%90-%EB%8C%80%ED%95%9C-%EA%B0%84%EB%8B%A8%ED%95%9C-%EC%A0%95%EB%A6%AC/){: }