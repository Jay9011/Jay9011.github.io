---
title: 추상 함수와 추상 클래스(Abstract Class) 그리고 인터페이스(Interface)
date: 2024-02-02 21:19:29 +0900
categories:
  - 풀스택 개발
  - C++
tags:
  - c
  - 객체 지향
  - 다형성
  - 디커플링
  - 유연성
  - 유지보수
  - 추상화
  - 확장성
math: false
type: C++
keywords:
  - Abstract Class
  - Interface
  - 인터페이스
  - 추상 클래스
  - 추상 함수
  - Pure Virtual Function
---

# 순수 가상 함수(Pure Virtual Function)

순수 가상 함수는 구현이 없는 가상 함수로, <span class="font_highlight">파생 클래스에서 **반드시 재정의**해야 하는 함수</span>를 의미합니다.

순수 가상 함수는 클래스의 선언에서 `= 0` 으로 표시됩니다.

```cpp
class Base
{
public:
    virtual void pureVirtualFunction() = 0; // 순수 가상 함수
};
```

이 함수는 구현이 없으며, 이 클래스를 상속받은 파생 클래스에서 이 함수를 반드시 구현해야 합니다.

# 추상 클래스(Abstract Class)

추상 클래스는 <span class="font_highlight">**한 개 이상**의 **순수 가상 함수**를 포함하는 클래스</span>입니다.

추상 클래스는 그 자체로는 <span class="important">**인스턴스화할 수 없으며**</span>, 이 클래스를 상속받는 파생 클래스에서 **순수 가상 함수를 구현**해야 합니다.

단, 추상 클래스 타입의 포인터나 참조로는 사용할 수 있습니다.

(당연하게도 추상 클래스를 사용하는 이유 중 하나가, 공통적인 동작에 대한 다형적인 접근을 사용할 수 있게 하기 위해서도 있기 때문입니다.)

```cpp
class Derived : public Base
{
public:
    void pureVirtualFunction() override
    {
      // 순수 가상 함수 구현
    }
};

int main()
{
    Base* ptr = new Derived();
    ptr->pureVirtualFunction();
}
```

# 인터페이스(Interface)

C++에서는 인터페이스라는 개념이 명시적으로 존재하지 않지만, <span class="font_highlight">순수 가상 함수만을 가지는 추상 클래스</span>를 인터페이스라고 부릅니다.

실제로 **추상 클래스(Abstract Class)**는 순수 가상 함수(Pure Virtual Function)만을 가지는게 아니라, 구현 할 수 있는 가상 함수라던가, 멤버 변수 또한 가질 수 있습니다.

즉, 추상 클래스란, 공통적인 기능을 여러 클래스에 제공하면서도, 일부 메서드는 하위 클래스에서 반드시 구현하도록 강제할 때 유용합니다.

하지만, <span class="keyword">**인터페이스(Interface)**</span>는 다형성을 구현하고, <span class="font_highlight">클래스 간의 결합도를 낮추는데</span> 목적이 있습니다.

조금 다르게 말하자면, **인터페이스**는 **서로 다른** 클래스나 컴포넌트가 클래스의 **내부 구현을 외부에 노출하지 않고 <span class="font_highlight">공통된 방법으로 상호 작용</span>**할 수 있도록 합니다.

이는 정보 은닉과 캡슐화를 강화하여 **유지보수성**과 **확장성**을 높이고 다형성(polymorphism)을 가능하게 하여 코드의 **유연성**을 높입니다.

보다 쉽게 설명드리자면,
- 서로 다른 클래스나 컴포넌트가
- 공통된 방법(인터페이스에서 정의한 메서드)으로 상호작용 하게 하고 싶은데
- 어딘가에 특정(상속)되지도 않고, 어쩌면 떨어져 있어도 될 것 같은 기능이라면 인터페이스로 구현

예를 들어, 게임 내에서 플레이어, 몬스터, 건물 등 다양한 객체가 있을 수 있고, 이들 모두 다양한 방법으로 데미지를 받을 수 있습니다.

하지만, 데미지를 처리하는 기능을 상속으로 구현하기에는 "어딘가에 특정(상속)되지도 않고"에 해당하는 것 같습니다.

이때, 이러한 객체들이 데미지를 받는 행동을 공통된 인터페이스를 통해 처리할 수 있습니다.

1．데미지 인터페이스 정의

```cpp
class IDamageable
{
public:
    virtual void takeDamage(int amount) = 0;
};
```

2．각 클래스에서 IDamageable 구현

```cpp
class Player : public IDamageable {
private:
    int health;
public:
    Player(int h) : health(h) {}
    void takeDamage(int amount) override {
        health -= amount;
        cout << "Player가 " << amount << "의 데미지를 받았습니다. 남은 체력: " << health << endl;
    }
};

class Monster : public IDamageable {
private:
    int health;
public:
    Monster(int h) : health(h) {}
    void takeDamage(int amount) override {
        health -= amount;
        cout << "Monster가 " << amount << "의 데미지를 받았습니다. 남은 체력: " << health << endl;
    }
};

class Building : public IDamageable {
private:
    int durability;
public:
    Building(int d) : durability(d) {}
    void takeDamage(int amount) override {
        durability -= amount;
        cout << "Building이 " << amount << "의 충격을 받았습니다. 남은 내구도: " << durability << endl;
    }
};
```