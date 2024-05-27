---
title: "[SOLID 원칙] 개방-폐쇄 원칙 (Open-Closed Principle, OCP)"
date: 2024-01-08 12:48:47 +0900
categories:
    - 컴퓨터 공학
    - 소프트웨어 공학
tags:
    - 객체 지향
    - 소프트웨어 공학
    - 유지보수
    - 확장성
    - 추상화
math: false
type: SoftwareEngineering
keywords:
    - SOLID
    - 개방-폐쇄 원칙
    - 객체지향
---

개방-폐쇄 원칙이란, 객체(클래스, 모듈, 함수 등)가 <span class="font_highlight">**확장에는 열려** 있어야 하고 **변경에는 닫혀(폐쇄)** 있어야 한다</span>는 것입니다.

즉, 기능을 추가(확장)할 때 <span class="font_highlight">**기존 코드를 변경하지 않고 확장**할 수 있어야 한다</span>는 뜻입니다.

# 목적

**확장에는 열려**있어야 한다.
- 객체는 <span class="important">**확장성**</span>을 가지고 있어야 한다. (언제든지 확장할 수 있다.)
- 새로운 변경사항이 발생한 경우, 유연하게 코드를 추가하는 것으로 <span class="important">**유지 보수**</span>에 좋아야 한다.

**변경에는 닫혀**있어야 한다.
- 기존에 동작하고 있던 **객체를 직접 수정하지 않고** 변경사항을 적용할 수 있어야 한다.

# OCP 위반 사례와 수정

마법을 사용하는 객체를 만들었다고 해봅시다.
<br>
`class Magic`은 마법을 관리하는 객체이며, `CastSpell()`을 통해 마법을 사용한다고 가정해 봅시다.

## 위반 사례

```cpp
class Magic
{
public:
	void CastSpell(const string& spell)
	{
		if (spell == "fireball")
		{
			CastFireball();
		}
		else if (spell == "frostbolt")
		{
			CastFrostbolt();
		}
		else
		{
			cout << "Unknown spell: " << spell << endl;
		}
	}

private:
	// 구현
	void CastFireball()
	{
		cout << "Casting Fireball!" << endl;
	}
	void CastFrostbolt()
	{
		cout << "Casting Frostbolt!" << endl;
	}
};

int main()
{
	Magic magic;
	magic.CastSpell("fireball");
	magic.CastSpell("frostbolt");
}
```

위 코드는 원하는 동작을 수행하지만, 새로운 마법들을 추가하려면 어떻게 될까요?

```cpp
// 만약, 다른 마법을 추가한다면?
magic.CastSpell("magic missile");
magic.CastSpell("lightning");
```

이 경우, 해당 클래스를 **직접 변경**해야만 할 것입니다. 이는 번거롭고, 수정 과정에서 코드가 잘못되면 **기존에 잘 동작하던 부분도 망가질 수 있습니다.**

```cpp
class Magic
{
public:
	void CastSpell(const string& spell)
	{
		if (spell == "fireball")
		{
			CastFireball();
		}
		else if (spell == "frostbolt")
		{
			CastFrostbolt();
		}
		// 기능을 확장하기 위해서는 클래스 내부 구성을 일일히 수정해야 하는 번거로움이 생긴다.
		else if (spell == "magic missile")
		{
			cout << "Casting Magic Missile!" << endl;
		}
		else if (spell == "lightning")
		{
			cout << "Casting Lightning!" << endl;
		}
		else
		{
			cout << "Unknown spell: " << spell << endl;
		}
	}

private:
	// 구현
	void CastFireball()
	{
		cout << "Casting Fireball!" << endl;
	}
	void CastFrostbolt()
	{
		cout << "Casting Frostbolt!" << endl;
	}
	// 단순 기능을 확장일 뿐인데도...
	void CastMagicMissile()
	{
		cout << "Casting Magic Missile!" << endl;
	}
	void CastLightning()
	{
		cout << "Casting Lightning!" << endl;
	}
};
```

## 원칙에 따라 수정

이때, 클래스를 <span class="important">**추상화**</span>하고 이를 상속, 확장하는 형태로 구성하면 **확장에는 열려**있지만 **변경에는 닫혀**있는 형태의 클래스를 만들 수 있습니다.

1. 확장 시킬 클래스들을 나눈다. (이 경우, 각 마법을 각 클래스로 나눈다.)
2. 이 클래스들이 공통으로 사용 될 부분을 **추상화**(추상 클래스 or 인터페이스)하여 작성한다.
3. 구현에 의존하기 보다, **추상화에 의존**하도록 한다.

```cpp
// Spell 인터페이스
class Spell
{
public:
	// 추상화 메서드
	virtual void Cast() = 0;
};
// 각 확장 가능한 클래스
class Fireball : public Spell
{
public:
	void Cast() override
	{
		cout << "Casting Fireball!" << endl;
	}
};
class Frostbolt : public Spell
{
public:
	void Cast() override
	{
		cout << "Casting Frostbolt!" << endl;
	}
};
// Magic 클래스
class Magic
{
public:
	void AddMagic(string magicName, Spell* spell)
	{
		magicList.insert(make_pair(magicName, spell));
	}

	void CastSpell(string magicName)
	{
		auto magic = magicList.find(magicName);
		if (magic != magicList.end())
		{
			// 추상화에 의존(Cast() 추상 메서드)
			magic->second->Cast();
		}
		else
		{
			cout << "Unknown spell!" << endl;
		}
	}

private:
	// 추상화에 의존(Spell)
	unordered_map<string, Spell*> magicList;
};

```

이렇게 위와 같이 구성하게 되면, 새로운 `magic missile`이나 `lightning`을 추가(확장) 한다고 해도 `class Magic`은 변경 없이 확장 가능하게 됩니다.

# 주의 사항

OCP 설계 원칙에 따라 설계 할 때에는 추상화(추상 클래스 or 인터페이스) 설계를 주의 할 필요가 있습니다.

OCP를 준수하려고 추상화와 상속 구조를 잘못 설계하면, 다른 SOLID 원칙, 특히 리스코프 치환 원칙(LSP)과 인터페이스 분리 원칙(ISP)을 위반하게 될 수 있습니다.

상속 구조에서 파생 클래스가 기본 클래스의 가상 메서드를 구현하지 못 할 수도 있고, 너무 많은 기능을 하나의 인터페이스에 통합하면 사용하지 않는 메서드에 의존하게 될 수도 있습니다.

# 마무리

개방-폐쇄 원칙과 의존 역전 원칙은 둘 다 모두 추상화를 활용하기 때문에 얼핏 보면 비슷한 원칙처럼 보일 수 있습니다.
그러나 OCP는 주로 <span class="keyword">**확장성**</span>에 초점을 맞추며, 소프트웨어 시스템이 새로운 기능을 추가할 때 기존 코드를 수정하지 않도록 설계할 수 있도록 합니다.

---
# 참고

[[SOLID 원칙] 객체지향 설계 5가지 원칙](/posts/solid-%EC%9B%90%EC%B9%99-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%EC%84%A4%EA%B3%84-5%EA%B0%80%EC%A7%80-%EC%9B%90%EC%B9%99/){: }

[[SOLID 원칙] 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)](/posts/solid-%EC%9B%90%EC%B9%99-%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84-%EC%B9%98%ED%99%98-%EC%9B%90%EC%B9%99-(liskov-substitution-principle%2C-lsp)/){: }

[[SOLID 원칙] 의존 역전 원칙 (Dependency Inversion Principle, DIP)](/posts/solid-%EC%9B%90%EC%B9%99-%EC%9D%98%EC%A1%B4-%EC%97%AD%EC%A0%84-%EC%9B%90%EC%B9%99-(dependency-inversion-principle%2C-dip)/){: }