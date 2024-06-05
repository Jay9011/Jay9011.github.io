---
title: C++의 StringStream
date: 2024-02-11 17:23:24 +0900
categories:
    - 풀스택 개발
    - C++
tags:
    - c
    - 데이터
    - 문자
math: false
type: C++
keywords:
    - sstream
    - StringStream
    - 문자열
---

C++의 `#include <sstream>` 헤더는 메모리에 있는 문자열을 사용하여 데이터를 **다른 타입으로 파싱**하거나, 데이터를 **문자열로 변환**하는 기능을 제공하는 String Stream 클래스들을 포함하고 있습니다.

# IStringStream

`istringstream` 클래스는 **문자열을 입력 스트림으로 사용**할 수 있게 합니다.

이 클래스를 사용하여 문자열 데이터를 읽고 파싱할 수 있습니다.

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::istringstream iss("test 123 aaa 456");
    std::string s1, s2;
    int i1, i2;
    iss >> s1 >> i1 >> s2 >> i2; // 공백을 기준으로 문자열을 파싱하고, 변수 형식에 맞게 변환

    std::cout << s1 << '\n';
    std::cout << i1 << '\n';
    std::cout << s2 << '\n';
    std::cout << i2 << '\n';
}
```
```text
결과
test
123
aaa
456
```

# OStringStream

`ostringstream` 클래스는 **문자열을 출력 스트림으로 사용**할 수 있게 합니다.

이 클래스를 통해 다양한 데이터를 문자열로 조합하고 특정 형식으로 변환하여 저장할 수 있습니다.

```cpp
#include <iostream>
#include <sstream>
#include <string>

int main()
{
    std::ostringstream oss;
    std::string s1 = "abc", s2 = "gjw";
    int i1 = 19234;
    double d1 = 3.591;
    oss << s1 << "\n" << i1 << "\n" << s2 << "\n" << d1; // 문자열 조합

    std::cout << oss.str() << std::endl; // str()을 사용하여 조립된 문자열 출력
}
```
```text
결과
abc
19234
gjw
3.591
```

# StringStream

`stringstream` 클래스는 `istringstream`과 `ostringstream`의 기능을 모두 가지고 있습니다. 이를 통해 문자열 데이터를 쉽게 읽고 쓸 수 있습니다.

## 사용법

### 문자열 초기화 및 입력

`stringstream`은 `str()` 함수를 통해 문자열을 스트림에 저장하거나 현재 스트림의 내용을 얻을 수 있습니다.

```cpp
#include <iostream>
#include <sstream>

int main()
{
    int n;
    std::string str = "1 23 456";
    std::stringstream ss;
    ss.str(str); // 스트림에 저장
    std::cout << ss.str() << std::endl; // 현재 스트림의 내용 출력
    ss.str(""); // 스트림 초기화
}
```
```text
결과
1 23 456
```

### 데이터 파싱

`stringstream`은 `std::istream`을 상속 받았기 때문에 `std::cin`처럼 사용할 수 있으며, `>>` 연산자를 이용해 원하는 자료형으로 데이터를 입력 받을 수 있습니다.

스트림의 끝은 `eof()` 메서드로 확인할 수 있습니다.

```cpp
#include <iostream>
#include <sstream>

int main()
{
    int n;
    std::stringstream ss("1 2 345 67 89");
    while (!ss.eof()) // 스트림의 끝에서 종료
    {
        ss >> n; // int형을 반복해서 스트림에 저장
        std::cout << n << std::endl;
    }
}
```
```text
결과
1
2
345
67
89
```