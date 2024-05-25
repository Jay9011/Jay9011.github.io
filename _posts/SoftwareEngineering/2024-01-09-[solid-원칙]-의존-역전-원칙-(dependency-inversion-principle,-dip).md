---
title: "[SOLID 원칙] 의존 역전 원칙 (Dependency Inversion Principle, DIP)"
date: 2024-01-09 17:25:20 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 객체 지향
  - 소프트웨어 공학
  - 유연성
  - 유지보수
  - 재사용성
math: false
type: SoftwareEngineering
keywords:
  - SOLID
  - 객체지향
  - 의존 역전 원칙
---

의존 역전 원칙은 다음 두 가지의 내용을 담고 있습니다.

1. 고수준 모듈(High-level Module)은 저수준 모듈(Low-level Module)에 의존해서는 안 되며, <span class="font_highlight">둘 다 **추상화**에 의존</span>해야 한다.
2. 추상화는 세부 사항에 의존해서는 안 되며, <span class="font_highlight">세부 사항은 **추상화**에 의존</span>해야 한다.

조금 어렵게 느껴질 수 있지만, 쉽게 말하자면, 두 클래스가 서로의 동작을 직접적으로 사용하는 관계가 되어서는 안된다는 것입니다.

만약 두 클래스가 **종속(dependency)**되거나 **결합(coupling)**된다면, 이후 수정 사항이 발생할 때, 상당히 많은 부분을 수정하게 되어 **유지보수성**을 해칠 수 있습니다.

말로만 하면 어렵기 때문에 간단한 코드로 보겠습니다.

# 의존 역전 원칙 이해하기

## DIP를 적용하지 않은 예시

```cpp
class Light
{
public:
    void TurnOn()
    {
        cout << "Light is on" << endl;
    }
    void TurnOff()
    {
        cout << "Light is off" << endl;
    }
};
class Switch
{
public:
	// Light에 의존
    void Operate(Light& light, const string& action)
    {
        if (action == "on")
        {
            light.TurnOn();
        }
        else if (action == "off")
        {
            light.TurnOff();
        }
    }
};
int main()
{
    Light light;
    Switch sw;
    sw.Operate(light, "on");
    sw.Operate(light, "off");
}
```

위 예제에서 `Switch` 클래스는 `Light` 클래스에 직접 의존하고 있습니다.

이 경우, `Switch` 클래스가 현재 의존하고 있던(`Light`) 클래스 대신 다른 클래스로 변경하려면 기존 코드를 변경해야 하며, 심지어는 `Light` 외의 기능을 추가(확장)하려고 할 때도 기존 코드를 수정해야 합니다.

## DIP를 적용

```cpp
// 추상화된 인터페이스
class Switchable
{
public:
    virtual void TurnOn() = 0;
    virtual void TurnOff() = 0;
};
// 저수준 모듈
class Light : public Switchable
{
public:
    void TurnOn() override
    {
        cout << "Light is on" << endl;
    }
    void TurnOff() override
    {
        cout << "Light is off" << endl;
    }
};
// 고수준 모듈
class Switch {
public:
    void Operate(Switchable& device, const string& action)
    {
        if (action == "on")
        {
            device.TurnOn();
        }
        else if (action == "off")
        {
            device.TurnOff();
        }
    }
};
int main()
{
    Light light;
    Switch sw;
    sw.Operate(light, "on");
    sw.Operate(light, "off");
}
```

이 코드에서는 `Switchable`이라는 <span class="font_highlight">**추상화 된 인터페이스**를 이용</span>했습니다.

따라서, `Switch` 클래스가 `Light` 클래스 대신 다른 클래스를 사용한다고 하더라도, `Switchable` 인터페이스를 구현한 다른 클래스를 받기만 하면 되기 때문에, 코드 수정 없이 동작을 변경할 수 있습니다.

또한, `Switch` 클래스는 `Switchable` 인터페이스에 의존하여, 다른 `Switchable` 구현체가 추가되더라도 `Switch` 클래스는 변경할 필요가 없게 되었습니다.

### 고수준 모듈 (High-level Module)

- 고수준 모듈은 시스템의 **주요 로직**이나 **규칙**을 정의하는 모듈입니다. 이 모듈은 시스템의 중요한 기능과 동작을 처리하며, 일반적으로 **추상적이고 포괄적인 역할**을 합니다.
- 고수준 모듈은 **시스템의 전체적인 동작과 흐름을 제어**하며, 저수준 모듈에 의존하여 구체적인 작업을 수행합니다.
- 따라서, 고수준 모듈은 가능한 한 구체적인 구현에 대한 의존성을 피하고 **추상화된 인터페이스에 의존**해야 합니다.

### 저수준 모듈 (Low-level Module)

- 저수준 모듈은 **구체적**이고 **세부적인 작업**을 수행하며, **고수준 모듈의 요청을 처리**합니다.

# 목적

- <span class="important">**유연성**</span> 증가: 고수준 모듈과 저수준 모듈 간의 **결합도가 낮아져**서 **변경에 유연하게 대응**할 수 있습니다.
- <span class="important">**재사용성**</span> 향상: 추상화를 통해 모듈 간의 **의존성을 줄임**으로써, 모듈을 다른 프로젝트나 컨텍스트에서 **쉽게 재사용**할 수 있습니다.
- <span class="important">**테스트 용이성**</span>: DIP를 적용하면 모듈을 **독립적으로 테스트**하기 쉬워집니다. 구체적인 구현 대신 인터페이스나 추상 클래스에 의존하므로, 테스트 시 모의 객체(mock object)를 쉽게 사용할 수 있습니다.

# 의존 역전이란?

의존 역전이란, <span class="important">**추상화의 역전**</span>이라고 봐도 좋을 것 같습니다.

기존에는 고수준 모듈이 저수준 모듈의 **구체적인 구현**에 의존적이었습니다.
<br>
그러나, DIP를 적용하면, 오히려 고수준 모듈이 **추상화된 인터페이스**에 의존하게 됩니다.

즉, '구체적인 것(추상적이지 않은 것)' → '추상적인 것'에 의존하게 된 것이므로 추상화의 역전이라고 봐도 좋을 것 같습니다.

또한, 그렇게 추상적인 것을 중간에 추가하게 되면, **저수준 모듈** 또한 **추상화된 인터페이스에 의존**하게 됩니다.

즉, 저수준 모듈의 입장에서 보면 아래 그림과 같이 의존성이 역전된 것이므로 의존성이 역전되었다고 볼 수 있습니다.

![중간에 추상화 계층이 끼어 화살표 한쪽이 역전된다.](https://i.postimg.cc/DZsr9f2c/SOLID.webp){: w="500" h="276"}

# 마무리

의존 역전 원칙과 개방-폐쇄 원칙은 둘 다 모두 추상화를 활용하기 때문에 얼핏 보면 비슷한 원칙처럼 보일 수 있습니다.
그러나 DIP는 **모듈 간의 <span class="keyword">결합도</span>를 낮추는 것**에 초점을 맞추며, <span class="keyword">**유연성**</span>과 <span class="keyword">**재사용성**</span>을 높이는 데 중점을 둡니다.

---
# 참고

[[SOLID 원칙] 객체지향 설계 5가지 원칙](/posts/solid-%EC%9B%90%EC%B9%99-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%EC%84%A4%EA%B3%84-5%EA%B0%80%EC%A7%80-%EC%9B%90%EC%B9%99/){: }

[[SOLID 원칙] 개방-폐쇄 원칙 (Open-Closed Principle, OCP)](/posts/solid-%EC%9B%90%EC%B9%99-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99-(open-closed-principle%2C-ocp)/){: }

[[SOLID 원칙] 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)](/posts/solid-%EC%9B%90%EC%B9%99-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99-(liskov-substitution-principle%2C-lsp)/){: }