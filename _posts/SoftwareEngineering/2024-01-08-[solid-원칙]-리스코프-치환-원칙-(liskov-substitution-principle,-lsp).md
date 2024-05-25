---
title: "[SOLID 원칙] 리스코프 치환 원칙 (Liskov Substitution Principle, LSP)"
date: 2024-01-08 19:53:05 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 객체 지향
  - 소프트웨어 공학
  - 유지보수
  - 다형성
math: false
type: SoftwareEngineering
keywords:
  - SOLID
  - 객체지향
  - 리스코프 치환 원칙
---

리스코프 치환 원칙은, <span class="font_highlight">**파생 클래스는 기본 클래스를 대신할 수 있어야 한다**</span>라는 원칙입니다.

좀 더 원문에 가깝게 말하면, '프로그램의 기능이나 동작을 바꾸지 않고도 상위 타입(`T`)을 하위 타입(`S`)으로 대체할 수 있어야 한다'는 원칙입니다.

> 컴퓨터 프로그램에서 자료형 𝑆가 자료형 𝑇의 서브타입(하위 타입)이라면 필요한 프로그램의 속성(정확성, 수행하는 업무 등)의 변경 없이 자료형 𝑇의 객체를 자료형 𝑆의 객체로 교체(치환)할 수 있어야 한다는 원칙이다.
> 
> 출처: [리스코프 치환 원칙 - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99){: target="_blank"}

사실, 이렇게만 설명하면 조금 어렵습니다.

<span class="text-muted small">심지어는 설명이 헷갈려 상위 타입과 하위 타입을 바꿔서 읽다가 더 헷갈리게 만들기도 하고, 어쩌다 이해하다가도 바꿔서 이해하면 '상속을 하면 기능을 확장한다는 건데, 어떻게 파생 클래스 대신 기본 클래스를 써도 괜찮게 만들라는 거지?'라는 오해를 하기도 합니다.</span>

간략히 설명하자면, <span class="font_highlight">객체 지향(OOP)의 **상속을 제대로 사용하자는 것**</span>에 해당합니다.

# LSP 위반 사례와 함께 이해하기

파생 클래스가 기본 클래스를 대신할 수 있어야 한다는 말은, <span class="font_highlight">기본 클래스에서 제공하던 기능을 **파생 클래스에서도 일관되게 동작하도록** 해야 한다</span>는 말이 됩니다.

기본 클래스에서 제공하던 기능을 파생 클래스에서 사용하지 못하게 하거나, 엉뚱한 기능으로 바꿔 놓는다면, <span class="keyword">**다형성**</span>에 대한 여러 문제가 발생할 수 있게 됩니다.

예를 들어, 모든 새가 날 수 있다고 생각하고 코드를 작성했는데, 펭귄이 날 수 없어서 예외가 발생하는 등의 문제가 발생합니다.

따라서, 코드의 신뢰성을 잃게 되고, 기능의 변경이나 코드의 변경이 필요할 때 기존 클래스를 수정해야 하는 문제 등이 발생할 수 있기 때문에 <span class="font_highlight">파생 클래스가 기본 클래스의 기능을 망가뜨리지 않고 확장할 수 있도록 설계</span>해야 합니다.

플레이어가 동물(`Animal`) 중 새(`Bird`)를 조종할 수 있도록 프로그램을 작성한다고 가정합시다.

```cpp
#include <iostream>
using namespace std;

class Animal{};
class Bird : public Animal
{
public:
	virtual void Fly() = 0;
	virtual void SetAltitude(int altitude) = 0;
};
class Eagle : public Bird
{
public:
	void Fly() override
	{
		cout << "Eagle is flying" << endl;
	}
	void SetAltitude(int altitude) override
	{
		cout << "Eagle is flying at " << altitude << " meters" << endl;
	}
};

class BirdController
{
public:
	virtual void Input(Bird* bird, int x, int y)
	{
		bird->Fly();
		bird->SetAltitude(y);
	}
};
```

모든 `Bird`는 날 수 있다고 생각해서 `Fly()`와 `SetAltitude()`를 만들고, 이를 상속 받는 `Eagle`을 만들었습니다.

이후 플레이어의 입력을 받을 수 있도록 `BirdController`를 만들고, 입력 받으면(`Input()`) 적당히 움직일 수 있게끔 만들었습니다.

나쁘지 않게 동작하는 것 같습니다.



하지만 만약, 조종할 수 있는 `Animal`에 펭귄이 추가된다면 어떨까요?

`Controller`를 담당하지 않은 사람이 펭귄 클래스를 구현하는데 사용하지 않는 가상 함수가 강제되어 있으니, 아마 다음과 같은 코드를 만들지도 모릅니다.

```cpp
class Penguin : public Bird
{
public:
	void Fly() override 
	{
		throw "Penguins can't fly";
	};
	void SetAltitude(int altitude) override
	{
		throw "Penguins can't fly";
	};
};
```

이때, <span class="important">**다형성**</span>에 의해 `bird->Fly()`를 사용하던 코드에서 문제가 발생하게 됩니다.


리스코프 치환 원칙의 목적은 <span class="font_highlight">**상속**과 **다형성**을 올바르게 사용하여 프로그램의 일관성과 안정성을 유지시키는 원칙</span>입니다.

이 원칙은 프로그램을 **안정적**으로 동작 시키고 **유지보수**성이 좋은 코드를 만들 수 있도록 합니다.

# 리스코프 치환 원칙 이해하기

위키 백과에서 리스코프 치환 원칙의 원칙 부분에서 이렇게 설명합니다.

> 리스코프의 원칙은 새로운 객체 지향 프로그래밍 언어에 채용된 **시그니처**에 관한 몇 가지 표준적인 요구사항을 강제한다.
> - 하위형에서 메서드 인수의 **반공변성**
> - 하위형에서 반환형의 **공변성**
> - 하위형에서 메서드는 상위형 메서드에서 던져진 **예외의 하위형**을 제외하고 새로운 예외를 던지면 안된다.
> 
> 여기에 더하여 하위형이 만족해야하는 **행동 조건** 몇 가지가 있다. 이것은 계약이 상속에 대해 어떻게 상호작용하는지에 대한 제약조건을 유도하는 계약에 의한 설계 방법론과 유사한 용어로 자세히 설명되어있다.
> - 하위형에서 **선행조건**은 강화될 수 없다.
> - 하위형에서 **후행조건**은 약화될 수 없다.
> - 하위형에서 상위형의 **불변조건**은 반드시 유지되어야 한다.
> 
> 출처: [리스코프 치환 원칙 - 위키백과](https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99){: target="_blank"}

## 공변성과 반공변성

우선, 어려운 단어인 <span class="important">'공변성'</span>과 <span class="important">'반공변성'</span>에 대해 설명해야 할 것 같습니다.

프로그래밍 언어에서의 공변성이란, <span class="font_highlight">**서브타입 관계**가 유지되는 것</span>을 의미합니다.

무슨 말이냐면, 타입 `S`가 타입 `T`의 서브타입일 때, `S*`도 `T*`의 서브 타입이고, `I<S>`도 `I<T>`의 서브 타입인 관계가 유지된다는 말입니다.
<br>
즉, 타입 `S`가 타입 `T`의 서브타입이기 때문에, `S*`도 `T*`로 타입 변환이 가능하고, `I<S>`도 `I<T>`로 타입 변환이 가능합니다.


반공변성은 이러한 <span class="font_highlight">**서브타입 관계가 반대로 적용**되는 것</span>을 의미합니다.
<br>
타입 `S`가 타입 `T`의 서브타입일 때, `T*`가 `S*`로 타입 변환이 가능해집니다.

이해하기 어렵다면, 공변성이란, 단순히 타입 `S`가 타입 `T`의 서브타입일 때, 서브타입이기 때문에 `S*`가 `T*`로 <span class="important">순서에 맞게 타입 변환</span>이 가능하다고 이해하시고 넘어가도 될 것 같습니다.

## 하위형에서 메서드 인수의 반공변성

인자 타입에 대한 반공변성은 Scala 언어가 지원합니다.

![공변성과 반공변성 지원 언어](https://i.postimg.cc/QCD807Pb/image.webp)
_출처: [공변성과 반공변성 (컴퓨터 과학) - 위키백과)](https://ko.wikipedia.org/wiki/%EA%B3%B5%EB%B3%80%EC%84%B1%EA%B3%BC_%EB%B0%98%EA%B3%B5%EB%B3%80%EC%84%B1_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))_

```scala
class Contravariant[-T] {}

val animal = new Contravariant[Animal]
val bird = new Contravariant[Bird]
val eagle = new Contravariant[Eagle]

// 반공변성 테스트
def say(user: Contravariant[Bird]) = println(user)

say(Animal) // 기본 클래스인 Animal을 Bird로 변환할 수 있다
say(Bird)
// say(Eagle) // Compile Error! // 파생 클래스 Eagle을 기본 클래스인 Bird로 변환이 허락되지 않았다
```

## 하위형에서 반환형의 공변성

하위 클래스의 메서드는 상위 클래스의 메서드와 비교하여 <span class="font_highlight">반환형을 하위 클래스로만 변경</span>할 수 있습니다.

```cpp
class Animal
{
public:
	virtual Animal* GetInstance()
	{
		cout << "Animal instance created" << endl;
		return new Animal();
	}
};
class Bird : public Animal
{};
class Eagle : public Bird
{
public:
  // 기존 Animal로 되어있던 반환형을 바꿀 수 있다.
	Eagle* GetInstance() override
	{
		cout << "Eagle instance created" << endl;
		return new Eagle();
	}
};

int main()
{
	Animal* animal = new Eagle();
	Animal* animal2 = animal->GetInstance();	// Eagle instance created

	delete animal;
	delete animal2;
}
```

하지만 만약, 반환형의 반공변성이 허용된다면 다음과 같은 문제가 발생할 수 있습니다.

1. <span class="important">**타입 안전성**</span> 문제: 반환형의 반공변성을 허용하면, 하위 클래스의 메서드가 상위 클래스의 메서드보다 **더 일반적인 상위 타입을 반환**할 수 있습니다. 이는 프로그램의 타입 안전성을 깨뜨릴 수 있습니다.
2. <span class="important">**다형성 깨짐**</span> 문제: 반환형의 반공변성을 허용하면, 상위 클래스의 메서드를 기대하고 사용하는 코드가 하위 클래스의 **메서드를 호출할 때 일관성**이 깨질 수 있습니다.

글로 정리하려니 조금 어려워서, 아래 코드를 보며 설명하겠습니다.

반공변성의 문제를 보여주기 위해 `Animal`의 상위 클래스로 `Derived`를 추가하고, `Derived`의 상위 클래스로 `Base` 클래스를 추가하겠습니다.

```cpp
class Base {};
class Derived : public Base 
{
public:
    void DerivedOnly()
    {}
};

class Animal
{
public:
    virtual Derived* create()
    {
        return new Derived();
    }
};
class Bird : public Animal
{
public:
    Base* create() override {  // 반환형의 반공변성 허용
        return new Base();
    }
};

void processAnimal(Animal* animal)
{
    Derived* d = animal->create();  // Derived 타입을 기대했으나, 타입 변환이 이루어지지 않을 수 있음
    // Derived 타입을 기반으로 한 작업을 기대했으나, Base 타입으로 인해 동작 문제가 발생할 수 있음
    d->DerivedOnly();
}

int main()
{
    Bird bird = new Bird();
    processAnimal(&bird);  // 문제가 발생
}
```

위 코드는 `Animal`이 `Derived` 타입을 반환하는 것으로 기대하여 `processAnimal()`에 관련 코드를 작성하였으나, `Bird`가 `Base` 타입을 반환할 수 있게 되면서 문제가 발생합니다.

`Derived` 타입을 기대했으나, 타입 변환이 제대로 이루어지지 않는 문제가 발생하거나, 이후 `Derived` 타입을 기반으로 한 작업(`DerivedOnly()` 메서드를 호출)을 기대했으나, `Base` 타입에 존재하지 않는 동작으로 인해 문제가 발생할 수도 있습니다.

## 하위형에서 메서드는 상위형 메서드에서 던져진 예외의 하위형을 제외하고 새로운 예외를 던지면 안된다.

하위 클래스의 메서드는 <span class="font_highlight">상위 클래스 메서드가 던지는 예외와 **동일하거나** 그 예외의 **하위 타입만**을 던져야 합니다</span>.

즉, 기존에 `catch`해서 사용할 수 있게 안전한 프로그램을 만들었는데, 사전에 약속되지 않은 <span class="font_highlight">**새로운 예외**를 만들어 던지면 기존 코드에서 이 예외를 캐치하지 못하게 됩니다.</span>

```cpp
class IOException : public exception
{
public:
	const char* what() const noexcept override
	{
		return "I/O Exception";
	}
};
class FileNotFoundException : public IOException
{
public:
	const char* what() const noexcept override
	{
		return "File Not Found Exception";
	}
};

int main()
{
	try
	{
		throw FileNotFoundException();
	}
	catch (const IOException& e)
	{
		cout << e.what() << endl;
	}
}
```

어쩌면, '모든 예외를 처리할 수 있게 하면 되지 않나?'하고 생각하실 수도 있을 것 같습니다.

하지만, 클라이언트 코드에서 던져질 예외들을 예상하고 그에 대한 처리를 작성해 놓아, 그 코드에서 예외가 발생하더라도 문제없이 프로그램이 동작하도록 만들어졌다면 어떨까요?

하위형에서 새로운 예외를 던진다면, 클라이언트 코드는 예기치 않은 예외를 처리해야 하는 상황에 놓이게 됩니다.

즉, <span class="important">**예측 가능성**</span>을 떨어뜨리고 <span class="important">**안정성**</span>도 떨어질 수 있습니다.

## 하위형에서 선행조건은 강화될 수 없다.

하위 클래스가 선행조건을 강화할 수 없다는 말은 하위 클래스의 메서드가 <span class="font_highlight">상위 클래스 메서드의 실행 조건보다 **엄격하게 만들지 말라**</span>는 뜻입니다.

메서드의 조건을 강화하는 경우 다형성에 의해 동작하던 코드가 일관되지 않고 예측 불가능하게 되어버리기 때문입니다.

```cpp
class Bird : public Animal
{
public:
	virtual void Fly() = 0;
	virtual void SetAltitude(int altitude)
	{
		if (altitude <= 0)
		{
			throw "Altitude must be greater than 0";
		}
	}
};
class Penguin : public Bird
{
public:
	void SetAltitude(int altitude) override
	{
		if (altitude != 0)
		{
			throw "Penguins can't fly";
		}

		cout << "Penguin is swimming" << endl;
	};
};

int main()
{
	Bird* birds[2] = { new Eagle(), new Penguin() };
	for (size_t i = 0; i < 2; i++)
	{
		birds[i]->SetAltitude(100);
	}

	delete[] birds;
}
```

위 예처럼 모든 `Bird`의 `SetAltitude()`의 값을 0 이상으로 기대했는데, `Penguin`에서 이 조건을 0이 아니면 안되게 만든다면, 모든 `Bird`를 대상으로 하는 동작에서 문제가 발생 할 것입니다.

## 하위형에서 후행조건은 약화될 수 없다.

후행조건이란, 메서드 호출 후 결과 값이 유효한 값으로 존재하게끔 하는 조건을 말합니다.

즉, <span class="font_highlight">상위 클래스의 메서드가 **보장하는 결과**를 하위 클래스가 더 **완화**하여 만들면 안된다</span>는 뜻입니다.


예를 들어, 양수만을 반환하는 `GetAltitude()`가 하위형에서 음수를 반환할 수도 있게 만들면 문제가 발생할수도 있습니다.

```cpp
class AnimalCalculator
{
public:
	virtual int GetAltitude(int input)
	{
		if (input < 0)
		{
			return 0;
		}

		return input;
	}
};
class BirdCalculator : public AnimalCalculator
{
public:
	int GetAltitude(int input) override
	{
		// 후행 조건을 완화시킨 경우, 양수를 기대하는 프로그램에서 문제가 발생할 수 있다.
		return input;
	}
};
```

## 하위형에서 상위형의 불변조건은 반드시 유지되어야 한다.

상위형의 불변조건이란, <span class="font_highlight">상위 클래스에서 설정한, **변하면 안되는 조건**은 하위 클래스에서도 유지되어야 한다</span>는 뜻입니다.

한 줄에 말하기 어려워서 조금 설명하자면,
<br>
상위 클래스에서 `int hp`라는 데이터가 존재한다고 가정합시다.

이때, 이 `int hp`에 대한 조건으로 '음수가 될 수 없다.'는 조건이 있고, 이 조건대로 상위 클래스를 설계했다면, 하위 클래스에서도 이 조건을 따라야 한다는 뜻입니다.

```cpp
class Character
{
public:
	virtual void Hit(int damage)
	{
		hp -= damage;

		if (hp <= 0)
		{
			hp = 0;
		}
	}
protected:
	int hp; // hp는 음수가 될 수 없다.
};
class Undead : public Character
{
public:
	void Hit(int damage) override
	{
		// 불변 조건을 지키지 않음
		hp -= damage;
	}
};
```

이 예가 적절할지 모르겠지만, `Character`에서 지정한 규칙이 `Undead`에서 깨지게 됩니다.

이후 이 `hp`를 조건으로 사용하는 어떤 코드에서 문제가 발생할지 알 수 없게 되어버립니다.

따라서, 객체의 상태 변경을 그 객체의 메서드를 통해서만 이루어지게 한다던가 하는 **캡슐화**를 잘 활용해야 할 것입니다.


# 목적

결국, 리스코프 치환 원칙의 목적은 <span class="font_highlight">**상속**과 **다형성**을 올바르게 사용</span>하여 프로그램의 일관성과 안정성을 유지시키는 원칙입니다.

- <span class="important">**코드의 일관성**</span>과 <span class="important">**예측 가능성**</span>을 유지: 기본 클래스에 대한 기능과 동작이 **파생 클래스에서도 일관 되도록 유지**하여, 이후 상속을 이어나가거나 기존 코드를 변경하더라도 **예측 가능하도록** 만들어야 한다.
- <span class="important">**유지보수성**</span> 향상: 기본 클래스의 인스턴스를 파생 클래스로 교체하더라도 프로그램이 예상대로 동작하도록 하여 코드 수정을 최소화 한다.
- <span class="important">**다형성**</span>의 효과적인 사용: **기본 클래스의 참조**를 통해 다양한 파생 클래스의 객체에 대한 동작을 처리할 수 있게 한다.

---
# 참고

[[SOLID 원칙] 객체지향 설계 5가지 원칙](/posts/solid-%EC%9B%90%EC%B9%99-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%EC%84%A4%EA%B3%84-5%EA%B0%80%EC%A7%80-%EC%9B%90%EC%B9%99/){: }

[[SOLID 원칙] 개방-폐쇄 원칙 (Open-Closed Principle, OCP)](/posts/solid-%EC%9B%90%EC%B9%99-%EA%B0%9C%EB%B0%A9-%ED%8F%90%EC%87%84-%EC%9B%90%EC%B9%99-(open-closed-principle%2C-ocp)/){: }

[[SOLID 원칙] 의존 역전 원칙 (Dependency Inversion Principle, DIP)](/posts/solid-%EC%9B%90%EC%B9%99-%EC%9D%98%EC%A1%B4-%EC%97%AD%EC%A0%84-%EC%9B%90%EC%B9%99-(dependency-inversion-principle%2C-dip)/){: }

[리스코프 치환 원칙 - 위키백과)](https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99)

[공변성과 반공변성 (컴퓨터 과학) - 위키백과)](https://ko.wikipedia.org/wiki/%EA%B3%B5%EB%B3%80%EC%84%B1%EA%B3%BC_%EB%B0%98%EA%B3%B5%EB%B3%80%EC%84%B1_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))

[공변성과 반공변성, 무공변성 (tistory.com)](https://partnerjun.tistory.com/78)