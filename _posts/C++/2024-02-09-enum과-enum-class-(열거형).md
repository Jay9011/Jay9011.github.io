---
title: enum과 enum class (열거형)
date: 2024-02-09 16:48:31 +0900
categories:
    - 풀스택 개발
    - C++
tags:
    - c
math: false
type: C++
keywords:
    - enum
    - enum class
    - 열거형
---

# Enum (열거형)

열거형(Enum)은 C++에서 **상수**를 정의하는 방법 중 하나로, <span class="font_highlight">**관련된 이름 있는 상수**들을 하나의 묶음으로 관리하기 위해 사용</span>됩니다.
<br>
이러한 열거형을 사용하면 코드의 가독성을 높이고, 특정 값들에 의미를 부여할 수 있습니다.
<br>
일반적으로 열거형의 각 상수는 **고유한 이름**과 **값**을 가집니다.

```cpp
enum Color {
    RED = 0,
    GREEN,
    BLUE
}
```
이 코드에서 `RED`, `GREEN`, `BLUE` 라는 이름의 상수가 생겨났고, 이 이름들에는 각각 `0`, `1`, `2`라는 값이 부여되었습니다.

기본적으로 열거형의 기본 형식은 `int`이며, 각 열거형 멤버는 0부터 시작하는 정수 값으로 자동 할당됩니다. 그러나 시작 값을 지정하거나 각각 특정 값을 할당할 수도 있습니다.

## 장점

- **안전성**
  - 열거형을 사용하면 코드에서 **의미있는 이름**을 사용할 수 있게 되기 때문에, 잘못된 상수 값을 사용하는 실수를 줄일 수 있습니다.
- **가독성**
  - 상수 값에 **의미 있는 이름**을 붙임으로써 코드의 가독성을 높입니다.
  - 예를 들어, 숫자 `0` 대신 `RED`를 사용하면 의미를 더 쉽게 이해할 수 있습니다.
- **유지보수성**
  - 열거형을 사용하면 새로운 상수를 추가하거나 기존 상수를 변경하는 작업이 더 간편해집니다.
  - 예를 들어, 각 코드에서 `0`, `1`, `2`라는 상수를 사용하다가 앞에 '아무 색도 선택하지 않는 `NONE`이라는 개념'을 추가하려고 하면 기존에 사용하던 모든 코드들을 일일이 수정해야 하는 반면, `enum`을 사용하면 `enum Color`의 상수 목록 앞에 `NONE`을 추가하기만 하면 됩니다.

## 단점

그러나 이런 `enum`에도 문제가 있습니다.

- `enum`은 열거형 상수가 선언된 스코프에서 전역으로 사용되기 때문에, 이름을 중복해서 가질 수 없고,
- 기본 형식은 `int`형인 전역 상수이기 때문에, 암시적인 형변환을 지원합니다.

예를 들어, 다음과 같이 말도 안되는 비교가 가능할 수 있습니다.
```cpp
enum Color
{
	RED,
	GREEN,
	BLUE
};
enum Day
{
	MONDAY,
	TUESDAY,
	WEDNESDAY
};
int main()
{
	if (RED == MONDAY)
	{
		std::cout << "RED == MONDAY" << '\n';
	}
	else
	{
		std::cout << "RED != MONDAY" << '\n';
	}
}
```

# Enum class (Scoped Enumeration)

`enum class` (Scoped Enumeration)는 C++11에서 도입된 새로운 형태의 열거형(Enum)으로, 기존의 `enum`에 비해 몇 가지 개선 사항을 가지고 있습니다.
<br>
이런 개선 사항들은 주로 타입 안전성과 이름 충돌 방지이며 그로인해 코드의 명확성 및 가독성이 높아졌습니다.

## 특징

1. **스코프**: 열거형 상수는 `enum class`의 이름 공간 안에 존재합니다. 따라서 **이름 충돌을 방지**할 수 있습니다.
2. **타입 안전성**: `enum class` 상수는 암묵적으로 정수형으로 변환되지 않습니다. **명시적 변환**이 필요합니다.
3. **강한 타입 검사**: 다른 열거형과 혼동해서 사용할 수 없게 되었습니다.

정리하자면, C++11에서 도입된 스코프가 있는 열거형(Scoped Enumeration) 타입은 **타입 안전성**이 강화되고 **이름 충돌을 방지**하도록 개선된 `enum`입니다.

### 1. 스코프

기존의 enum은 열거형 상수가 선언된 스코프에서 **전역으로 사용**됩니다.
<br>
이는 같은 이름을 가진 열거형 상수가 서로 다른 열거형에 포함될 때, 이름 충돌 문제를 발생 시킵니다.

```cpp
enum Color
{
	RED,
	GREEN,
	BLUE
};

enum TrafficLight
{
	RED,		// 이름 충돌 발생
	YELLOW,
	GREEN		// 이름 충돌 발생
};
```

`enum class`는 열거형 상수를 <span class="font_highlight">**해당 열거형의 스코프 내에 제한**</span>함으로써, 이러한 이름 충돌을 방지합니다.
<br>
대신, 열거형 상수를 사용할 때는 <span class="font_highlight">반드시 **열거형의 이름을 함께 사용**</span>해야 합니다.

```cpp
enum class Color
{
	RED,
	GREEN,
	BLUE
};

enum class TrafficLight
{
	RED,
	YELLOW,
	GREEN
};

int main()
{
	Color color = Color::RED;

	switch (color)
	{
		case Color::RED:
			std::cout << "Red\n";
		break;
		case Color::GREEN:
			std::cout << "Green\n";
		break;
		case Color::BLUE:
			std::cout << "Blue\n";
		break;
	}
}
```

### 2. 타입 안전성

기존의 enum은 열거형 상수가 정수형(int)으로 암묵적으로 변환될 수 있어 타입 안전성이 부족합니다.
<br>
이로 인해, 서로 다른 열거형 타입의 상수를 혼동하여 사용할 위험이 있으며, 이는 프로그램의 버그를 유발할 수 있습니다.

예를 들어, 컬러를 매칭시키는 코드를 작성하는데, 실수로 다른 타입의 `enum`을 비교하는데 사용하더라도, 이를 눈치채기 어려울 수 있습니다.
```cpp
enum Color
{
	RED,
	GREEN,
	BLUE
};
enum Day
{
	MONDAY,
	TUESDAY,
	WEDNESDAY
};
int main()
{
  Color c = RED;
  Day d = MONDAY;

  // 기타 코드들...

	if (c == d)
	{
		std::cout << "컬러가 일치합니다!" << '\n';
	}
	else
	{
		std::cout << "컬러가 불일치합니다." << '\n';
	}
}
```

이러한 문제를 해결하기 위해 `enum class`는 암묵적인 형 변환을 허용하지 않습니다. 이는 코드의 타입 안전성을 크게 향상시킵니다.

따라서, 만약 `enum class` 상수를 다른 타입으로 변환하려면 명시적인 형 변환이 필요합니다.

```cpp
enum class Color
{
	RED,
	GREEN,
	BLUE
};
enum class Day
{
	MONDAY,
	TUESDAY,
	WEDNESDAY
};
int main()
{
	Color c = Color::RED;
	Day d = Day::MONDAY;

	// 기타 코드들...

	if (c == d)	// 에러 발생
	{
		std::cout << "컬러가 일치합니다!" << '\n';
	}
	else
	{
		std::cout << "컬러가 불일치합니다." << '\n';
	}
}
```

## 장점

이처럼 `enum class`는 기존 `enum`의 장점은 그대로 가져오면서 문제점을 보완한 열거형입니다.

- **안전성**
  - 열거형을 사용하면 코드에서 **의미있는 이름**을 사용할 수 있게 되기 때문에, 잘못된 상수 값을 사용하는 실수를 줄일 수 있습니다.
  - (+) 타입 안전성으로 다른 열거형과 혼동해서 사용하지 않게 하고, 각종 논리적 문제를 줄일 수 있습니다. 다만, 열거형 상수를 사용할 때, 열거형 타입의 이름을 붙여야 합니다.
- **가독성**
  - 상수 값에 **의미 있는 이름**을 붙임으로써 코드의 가독성을 높입니다.
  - (+) 상수의 앞에 **열거형 타입의 이름**이 붙음으로써 더욱 명확히 어디에 속한 상수인지 알 수 있습니다.
- **유지보수성**
  - 열거형을 사용하면 새로운 상수를 추가하거나 기존 상수를 변경하는 작업이 더 간편해집니다.
  - 예를 들어, 각 코드에서 `0`, `1`, `2`라는 상수를 사용하다가 앞에 '아무 색도 선택하지 않는 `NONE`이라는 개념'을 추가하려고 하면 기존에 사용하던 모든 코드들을 일일이 수정해야 하는 반면, `enum`을 사용하면 `enum Color`의 상수 목록 앞에 `NONE`을 추가하기만 하면 됩니다.
