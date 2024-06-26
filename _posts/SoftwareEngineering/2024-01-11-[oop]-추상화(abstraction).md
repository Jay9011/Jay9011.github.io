---
title: "[OOP] 추상화(Abstraction)"
date: 2024-01-11 19:00:24 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 객체 지향
  - 소프트웨어 공학
  - 유연성
  - 유지보수
  - 재사용성
  - 추상화
math: false
type: SoftwareEngineering
keywords:
  - Abstraction
  - 객체지향
  - 추상 클래스
  - 추상화
  - 인터페이스
---

객체지향 프로그래밍(OOP)에서 추상화(Abstraction)란, <span class="font_highlight">복잡한 시스템을 이해하고 관리하기 쉽게 **단순화된 개념**이나 **의미있는 표현**으로 정의</span>하는 과정을 말합니다.

이 과정을 통해, 우리는 필수적인 특성만을 강조하고 불필요한 세부사항은 숨김으로써, 보다 체계적이고 효율적인 소프트웨어 설계를 가능하게 합니다.



# 추상화란?

추상화(Abstraction)는 복잡한 문제를 단순화하여 모델링하는 방법입니다.

복잡한 시스템을 설계할 때, 모든 세부 사항을 한번에 다루려고 하면 **이해**와 **관리**가 어려워집니다.

예를 들어, '자동차'를 모델링할 때 엔진, 바퀴, 운전대와 같은 **기본 요소를 중심으로 개념화**하면, 운전자는 각 부품이 어떻게 작동하는지 상세히 알 필요 없이 자동차를 이해하고 사용할 수 있습니다.

이처럼 추상화는 복잡성을 관리하고, 사용자에게는 필요한 정보만을 제공함으로써 인터페이스를 단순화합니다.


## 절차적 프로그래밍 (Procedural)의 문제

절차적 프로그래밍은 프로그램을 절차(Procedure) 또는 함수(Function) 단위로 나누어 작성하는 방식입니다.
<br>
이 방법은 복잡한 소프트웨어 개발이 진행되면서 절차적 프로그래밍의 몇 가지 문제점이 드러나게 되었습니다.



절차적 프로그래밍에서는 데이터와 함수를 분리된 형태로 관리하기 때문에 함수들이 동일한 데이터를 직접 조작하게 됩니다.
<br>
따라서, 데이터 구조가 변경되면 관련된 모든 함수를 수정해야 하는 문제가 발생합니다.
<br>
이는 코드의 유연성과 확장성을 저해합니다.



또한, 비슷한 기능을 여러 곳에서 사용해야 할 때, 이를 복사-붙여넣기 방식으로 처리하면 코드 중복이 발생합니다. 또 비슷하지만 조금식 세부 구현이 다른 경우에도 함수로 만들어두기 어렵고, 이 또한 일부 중복 코드가 발생하게 됩니다.
<br>
이는 유지보수를 어렵게 만듭니다.

# 두 가지 방식의 추상화 구현

OOP에서 추상화는 주로 클래스와 객체를 통해 구현됩니다.

그중 두 가지 주요 구성 요소는 **추상 클래스**와 **인터페이스**입니다.

## 추상 클래스(Abstract Class)

추상 클래스는 <span class="font_highlight">하나 이상의 **추상 메서드**를 포함하는 클래스</span>입니다.

**추상 메서드**는 <span class="font_highlight">**구현되지 않은 메서드**로, 하위 클래스에서 반드시 구현</span>해야 합니다.

**추상 클래스** 자체는 인스턴스화할 수 없으며, 주로 **공통된 속성**과 **메서드**를 정의하여 하위 클래스가 이를 <span class="keyword">**상속**</span>받아 사용할 수 있도록 합니다.

## 인터페이스(Interface)

인터페이스는 <span class="font_highlight">클래스가 **특정 기능**을 수행할 수 있도록 보장</span>하는 일종의 계약입니다.

인터페이스는 **메서드의 시그니처만을 정의**하며, 실제 구현은 이를 <span class="keyword">**구현**</span>하는 클래스에서 이루어집니다.

인터페이스는 한 클래스에 여러개를 구현할 수 있으며, 이를 통해 다양한 기능을 **유연**하게 추가하고 **확장**할 수 있습니다.

# 추상화 방법

추상화 방식은 위와 같이 '추상 클래스'를 만드는 방법과 '인터페이스'를 만드는 방법이 있습니다.

그렇다면, 어떻게 추상화를 해야 할까요?

추상화란, <span class="font_highlight">복잡한 시스템을 이해하고 관리하기 쉬운 **개념**</span>으로 만드는 것입니다.
<br>
즉, 여러 구현 클래스들의 <span class="font_highlight">**공통적인 특징**</span>들을 뽑아 구현 클래스들을 대표하는 상위 타입으로 추상화 할 수 있습니다.


예를 들어, 메시징 시스템을 구현한다고 가정해 보겠습니다. 여기서 클라이언트가 SMS, Kakao로 기능을 만들어 달라고 요구할 수 있습니다.
<br>
이 시스템을 구체적인 구현사항으로 만들면, 이후 클라이언트가 LINE과 같은 추가 사항을 요청했을 때, 관련된 코드들을 전부 수정해야 하는 문제가 발생합니다.
<br>
하지만, 메시징 서비스의 공통적인 기능을 `ISender` 인터페이스에 정의하면, 새로운 메시징 서비스를 추가할 때 기존 코드를 변경하지 않고도 확장할 수 있습니다.

![추상화를 하면 확장이 쉬워진다](https://i.postimg.cc/zvCsRQBM/OOP.webp){: w="512" h="189"}

추상화는 어떤 개념들에서 공통적인 개념을 뽑아 <span class="font_highlight">**공통적인 기능을 제공하는 타입**</span>을 새로 만드는 것입니다.

이때, 어떻게 구현 할 것가는 아직 모르기 때문에, 추상 타입에서는 **구현을 제공하지 않고**, 구체적인 구현은 해당 추상화를 구현하는 구체적인 클래스에서 구현하게 됩니다.

```cpp
ISender sender = Manager::GetSender();
sender.send(message);
```

이때, 구현 코드의 수정이 발생하든, 확장이 발생하든, 동작을 요청하는 부분은 변하지 않으므로 **유지보수**와 **확장성**을 크게 향상시킬 수 있습니다.

# 추상화의 장점

1. 코드 <span class="important">**재사용성**</span> 향상: 추상 클래스와 인터페이스를 통해 공통된 기능을 정의함으로써, **여러 클래스에서 이 기능들을 재사용**할 수 있습니다. 이 접근 방식은 중복된 코드를 줄이고, 개발 시간과 비용을 절감합니다.
2. <span class="important">**유지보수성**</span>: 공통된 기능을 한 곳에서 관리함으로써 코드의 간결성이 증가합니다. 이는 코드 변경이 필요할 때 일관된 수정이 가능하게 하고, 결과적으로 **유지보수**가 용이해집니다.
3. <span class="important">**유연성**</span>
	- 새로운 클래스가 추가될 때 기존 코드를 수정하지 않고도 쉽게 확장할 수 있습니다. 추상화를 통해 코드의 **유연성**과 **확장성**을 높일 수 있습니다.
	- 추상 타입을 사용하는 코드는 구현 세부 사항에 의존하지 않으므로, 새로운 기능을 추가하거나 변경할 때 기존 구성 요소를 쉽게 대체하거나 확장할 수 있습니다.


---
# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[[OOP] 캡슐화 (Encapsulation)](/posts/oop-%EC%BA%A1%EC%8A%90%ED%99%94(encapsulation)/){:  }

[[OOP] 상속성(Inheritance)](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){:  }

[[OOP] 다형성(Polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }