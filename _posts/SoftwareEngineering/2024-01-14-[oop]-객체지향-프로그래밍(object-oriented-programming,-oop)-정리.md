---
title: "[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리"
date: 2024-01-14 19:44:21 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 객체 지향
  - 다형성
  - 소프트웨어 공학
  - 유연성
  - 유지보수
  - 재사용성
  - 확장성
  - 추상화
math: false
type: SoftwareEngineering
keywords:
  - 객체지향
  - 상속성
  - 정보은닉
  - 추상화
  - 캡슐화
  - 다형성
---

객체지향 프로그래밍(Object-Oriented Programming, OOP)은 프로그래밍 패러다임 중 하나로, **데이터**와 그 데이터를 처리하는 **함수(메서드)** 를 하나의 **'객체'** 로 묶어서 프로그래밍하는 방식입니다.

이 패러다임은 <span class="font_highlight">**여러 개의 독립된 객체**들이 **상호작용**하면서 동작</span>하는 개발 방법을 제시하며, 하나의 객체가 다른 객체의 <span class="font_highlight">데이터에 직접 접근하기보다 **메소드를 이용하여 접근**하는 것</span>을 권장하고 있습니다.

# 객체란?

객체는 <span class="keyword">**데이터(속성)**</span>와 <span class="keyword">**기능(메서드)**</span>을 결합한 캡슐화된 모듈입니다.

이 모듈은 다른 객체에 서비스를 제공하며, 각 객체는 독립적으로 동작할 수 있는 자율성을 가집니다.

객체는 단순한 데이터 저장 수단을 넘어서 해당 데이터를 조작하는 메서드를 포함하므로, 단순한 데이터 구조체보다 더 복잡한 행동을 모델링 할 수 있습니다.

# 캡슐화(Encapsulation)

캡슐화는 객체의 <span class="font_highlight">**데이터**와 그 데이터를 처리하는 **메서드**를 **하나의 단위로 묶고**, 객체의 일부 구현 내용에 대한 직접적인 **접근을 제한**하는 기법</span>입니다.

이는 소프트웨어의 설계를 개선하고, 데이터의 **무결성**을 보호하며, **유지보수**를 용이하게 만들고, **재사용성**을 증가시키는 중요한 기법입니다.

## 캡슐화의 특징

- **코드의 재사용성** 증가: 캡슐화된 객체는 **독립적으로 동작**하기 때문에, 다른 프로젝트나 코드베이스에서도 재사용하기 쉽습니다.
- **유지보수성** 향상: 객체의 내부 구현이 변경되더라도, 외부 인터페이스는 그대로 유지될 수 있으므로, **코드 변경 시 다른 부분에 영향을 최소화**할 수 있습니다.
- **응집도(Cohesion)** 증가: **관련된 데이터와 메서드를 하나의 객체로 묶음**으로써, 코드의 응집도가 높아지고, 코드가 더 **이해**하기 쉽고 **유지보수**하기 쉬워집니다.


## 정보은닉

**정보 은닉(Information Hiding)** 이란, 결국 데이터에 쉽게 접근해서 발생한 데이터 보호 문제나 높은 결합도 문제를 해결하기 위한 방법입니다.

즉, <span class="font_highlight">객체의 내부 상태를 외부에서 **직접 접근할 수 없도록 함**</span>으로써, 객체의 내부 구현을 숨기고, 외부에서는 객체가 제공하는 기능을 통해서만 상호작용하게 하여 **데이터의 무결성**을 유지하게 합니다.

데이터의 무결성이 유지된다는 말은 외부에서 영향을 받거나 주지 않고 내구 구현을 변경할 수 있다는 말이 되고, 따라서 외부 코드에 영향을 최소화하여 **유지보수성**이 높아지게 됩니다.

[캡슐화와 정보 은닉 글 자세히 보러가기](/posts/oop-%EC%BA%A1%EC%8A%90%ED%99%94(encapsulation)/){: target="_blank" }

# 상속성(Inheritance)

상속성(Inheritance)은 기존 클래스의 특성과 동작을 새로운 클래스가 물려받아 <span class="keyword">**재사용**</span>하고 <span class="keyword">**확장**</span>할 수 있는 특징을 말합니다.

상속을 통해 코드의 재사용성을 높이고, 계층적 분류를 통한 복잡성 관리가 가능합니다.

## 상속성의 특징

1. **코드 재사용성**: 상속을 통해 **기존 클래스의 속성**과 **메서드**를 그대로 사용할 수 있어 코드 중복을 줄이고, **기존 클래스를 확장**하여 새로운 기능을 추가할 수도 있습니다.
2. **유지보수**의 용이성: 공통 기능을 상위 클래스에 정의하고, 이를 상속받는 하위 클래스에서 사용함으로써 유지보수를 용이하게 만들 수 있습니다.
	- 예를 들어, 상위 클래스의 기능을 수정하면 이를 상속받는 모든 하위 클래스에 자동으로 반영되므로, 일관성 있는 수정이 가능합니다.
3. **계층 구조**: 상속성을 통해 클래스 간의 계층 구조를 명확히 할 수 있습니다. 이를 통해 시스템의 구조를 체계적으로 설계하고, **객체 간의 관계**를 명확히 이해할 수 있습니다.

[상속성 글 자세히 보러가기](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: target="_blank" }

# 추상화(Abstraction)

추상화(Abstraction)란, 복잡한 시스템을 **중요한 부분만을 간추려** 인터페이스를 단순화하고 사용자에게는 필요한 정보만을 제공하는 과정입니다.

이때, <span class="important">**추상 클래스**</span>와 <span class="important">**인터페이스**</span>를 통해 **공통된 기능을 정의**함으로써, 여러 클래스에서 이 기능들을 **재사용**할 수 있습니다.

물론, 추상 타입에서는 **구현을 제공하지 않고**, 구체적인 구현은 해당 추상화를 구현하는 구체적인 클래스에서 구현하게 되므로, 구현 코드의 수정이 발생하든, 확장이 발생하든, 동작을 요청하는 부분은 변하지 않으므로 **유지보수**와 **확장성**을 크게 향상시킬 수 있습니다.

## 추상화의 장점

1. 코드 **재사용성** 향상: 추상 클래스와 인터페이스를 통해 공통된 기능을 정의함으로써, 여러 클래스에서 이 기능들을 재사용할 수 있습니다. 이 접근 방식은 중복된 코드를 줄이고, 개발 시간과 비용을 절감합니다.
2. **유지보수성**: 공통된 기능을 한 곳에서 관리함으로써 코드의 간결성이 증가합니다. 이는 코드 변경이 필요할 때 일관된 수정이 가능하게 하고, 결과적으로 **유지보수**가 용이해집니다.
3. **유연성**
	- 새로운 클래스가 추가될 때 기존 코드를 수정하지 않고도 쉽게 확장할 수 있습니다. 추상화를 통해 코드의 **유연성**과 **확장성**을 높일 수 있습니다.
	- 추상 타입을 사용하는 코드는 구현 세부 사항에 의존하지 않으므로, 새로운 기능을 추가하거나 변경할 때 기존 구성 요소를 쉽게 대체하거나 확장할 수 있습니다.

[추상화 글 자세히 보러가기](/posts/oop-%EC%B6%94%EC%83%81%ED%99%94(abstraction)/){: target="_blank" }

# 다형성(Polymorphism)

다형성은 객체지향 프로그래밍의 핵심적인 개념 중 하나로 <span class="font_highlight">**하나의 객체**가 **다양한 타입**으로 행동</span>할 수 있도록 하며, 이로 인해 코드의 **재사용성**과 **확장성**이 증가하고 **유지보수**가 용이해집니다.

이는 메서드 오버라이딩(상속받은 메서드를 다르게 구현)과 오버로딩(동일한 메서드 이름에 다른 매개변수를 사용)으로 구현됩니다.

- <span class="important">**오버라이딩(Overriding)**</span>: 오버라이딩은 다형성의 동적 형태로, **실행 시간(runtime)**에 **객체의 실제 타입**에 따라 적절한 메서드가 호출됩니다. 이는 같은 메서드 호출에 대해 다른 행동을 할 수 있도록 합니다.
- <span class="important">**오버로딩(Overloading)**</span>: 오버로딩은 정적 다형성의 예로, **컴파일 시간**에 **메서드를 선택**할 수 있게 합니다. 이는 동일한 메서드 이름에 다양한 매개변수를 사용할 수 있게 하여 메서드 사용의 유연성을 높입니다.

[다형성 글 자세히 보러가기](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: target="_blank"}

---
# 참고

[[OOP] 캡슐화 (Encapsulation)](/posts/oop-%EC%BA%A1%EC%8A%90%ED%99%94(encapsulation)/){:  }

[[OOP] 상속성(Inheritance)](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){:  }

[[OOP] 추상화(Abstraction)](/posts/oop-%EC%B6%94%EC%83%81%ED%99%94(abstraction)/){:  }

[[OOP] 다형성(Polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }