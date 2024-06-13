---
title: 클래스(Class)와 백킹 필드(backing field), 그리고 base와 this
date: 2024-06-02 16:11:48 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 객체 지향
    - 다형성
math: false
type: C#
keywords:
    - backing field
    - base
    - Class
    - 백킹 필드
    - 클래스
    - this
---

클래스(class)는 [객체 지향 프로그래밍(Object-Oriented Programming, OOP)](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: target="_blank"}의 핵심 개념 중 하나로, <span class="font_highlight">**데이터**와 해당 데이터를 처리하는 메서드(**기능**)를 하나로 묶는 방법</span>입니다.

클래스는 데이터(필드)와 해당 데이터에 작용하는 메서드(함수)의 집합을 정의합니다.

C#은 객체 지향 프로그래밍 패러다임을 강력하게 따르는 프로그래밍 언어로, 이 클래스를 **기본 단위**로 하여 프로그램의 동작들을 다루게 됩니다.

# 클래스 정의

클래스는 `class` 키워드를 사용하여 정의되며, 클래스 내부에는 필드, 메서드, 생성자, 소멸자, 프로퍼티, 이벤트 등이 포함될 수 있습니다.

```csharp
public class MyClass
{
    // 필드
    private int myField;
    // 프로퍼티
    public int MyProperty { get; set; }
    // 메서드
    public void MyMethod()
    {
        Console.WriteLine("Hello, World!");
    }
    // 생성자
    public MyClass(int initialValue)
    {
        myField = initialValue;
    }
	// 소멸자
    ~MyClass()
    {
        // 리소스 해제 작업
    }
}
```

## 1. 필드 (Fields)

- 필드는 <span class="font_highlight">클래스의 **데이터**를 저장하는 멤버</span>입니다.
- 클래스 내부에 선언되며, **객체의 상태**를 나타냅니다.
- 필드는 접근 제한자에 따라 공개(public), 비공개(private), 보호(protected) 등으로 설정할 수 있습니다.

### 멤버 접근 연산자 (`.`)

클래스의 멤버(필드, 메서드, 프로퍼티 등)에 접근할 때에는 멤버 접근 연산자 (`.`)를 사용합니다.
<br>
이 연산자를 통해 객체의 내부 데이터에 접근하거나, 객체가 제공하는 기능을 사용할 수 있습니다.

```csharp
BaseClass myClass = new DerivedClass();

myClass.Name = "MyClass";
Console.WriteLine($"Class Name: {myClass.Name}");   // Class Name: MyClass
```

위 예제는 멤버 접근 연산자로 `Name` 필드에 값을 대입하고, 출력시, `Name` 필드의 값을 가져와서 출력하는 모습입니다.

## 2. 프로퍼티 (Properties)

- 프로퍼티는 <span class="font_highlight">**필드에 대한 접근**을 제어하는 방법</span>을 제공합니다.
- 프로퍼티는 get 접근자와 set 접근자를 포함할 수 있으며, 자동 구현 프로퍼티를 통해 간단히 정의할 수도 있습니다.

### 백킹 필드(backing field)와 value 예약어

프로퍼티는 필드에 대한 접근을 제어하는 방법으로, 해당 필드를 읽어올 때, 해당 필드에 대입할 때, 각각 접근 제한자를 적용시킬 수 있습니다.

```csharp
// 읽기는 public, 쓰기는 protected
public int MyProperty { get; protected set; }
```

또한, 아래와 같이 동작을 정의할 수도 있기 때문에, C++에서 오신 분은 C++의 getter/setter 메서드와 헷갈릴 수 있습니다.

```csharp
public int MyProperty
{
    get { return MyProperty; }
    protected set { MyProperty = value; }
}
```

하지만, 위와 같이 사용하면 스택 오버플로우가 발생하게 됩니다.
<br>
왜냐하면, 프로퍼티 내에서 해당 프로퍼티를 직접 참조하면, 다시 해당 프로퍼티 메서드를 호출하는 형태로 무한 재귀 호출이 발생하기 때문입니다.

이를 해결하기 위해서 프로퍼티를 지원하는 실제 필드를 따로 사용해야 합니다.

#### 백킹 필드(Backing Field)

백킹 필드는 <span class="font_highlight">프로퍼티의 값을 실제로 저장하는 데 사용되는 **비공개 필드**</span>입니다.

이때, 프로퍼티는 데이터 캡슐화를 제공하며, 값에 대한 접근과 수정 시 추가 로직을 수행할 수 있게 하여 C++에서의 getter/setter를 만들 수 있습니다.

```csharp
// 백킹 필드
private int _myProperty;

// 프로퍼티
public int MyProperty
{
	get
	{
		// 백킹 필드에서 값을 가져옴
		return _myProperty;
	}
	protected set
	{
		_myProperty = value;
    }
}
```

#### value 예약어

`set` 프로퍼티를 보시면 따로 지정하지 않은 `value`라는 예약어가 쓰인 것을 보실 수 있게 됩니다.

`value`는 프로퍼티의 `set` 접근자 내에서 사용되는 예약어로, <span class="font_highlight">프로퍼티에 할당되는 값</span>을 나타내는, 숨겨진 매개변수 같은 것입니다.
<br>
즉, 대입 연산자 같은 것에 의해 `set` 접근자가 호출될 때, 할당하려는 값이 `value`를 통해 전달됩니다.

## 3. 메서드 (Methods)

- 메서드는 <span class="font_highlight">클래스의 **동작**을 정의하는 멤버</span>로, 특정 작업을 수행하는 코드 블록입니다.
- 메서드는 매개변수를 받을 수 있으며, 값을 반환할 수도 있습니다.
- **접근 제한자**에 따라 메서드의 접근 범위를 제어할 수 있습니다.

## 4. 생성자 (Constructors)

- 생성자는 <span class="font_highlight">클래스의 **인스턴스를 초기화**</span>하는 특별한 메서드입니다.
- 생성자는 **클래스명과 동일한 이름**을 가지며, **반환 타입이 없습니다**.
- 생성자는 객체가 생성될 때 **자동으로 호출**됩니다.

## 5. 소멸자 (Destructors)

- 소멸자는 <span class="font_highlight">**객체가 소멸**될 때 호출</span>되는 특별한 메서드입니다.
- 주로 리소스를 해제하는 데 사용되며, `~` 기호로 시작합니다.
- C#의 가비지 컬렉션은 자동으로 메모리를 관리하므로, 소멸자를 명시적으로 사용하는 경우는 드뭅니다.

# 접근 제한자

C#에서는 클래스 멤버에 대한 접근을 제어하기 위해 여러 접근 제한자를 제공합니다.

- `public`: 모든 코드에서 접근 가능
- `protected`: 동일 클래스 및 파생 클래스에서 접근 가능
- `private`: 동일 클래스 내에서만 접근 가능
- `internal`: 동일 어셈블리 내에서 접근 가능
- `protected internal`: 동일 어셈블리 내에서 또는 파생 클래스에서 접근 가능
- `private protected`: 동일 클래스 내에서 또는 동일 어셈블리의 파생 클래스에서 접근 가능

# 객체 생성

클래스의 인스턴스를 생성하기 위해 `new` 키워드를 사용합니다.

또한, 생성자를 통해 초기 상태를 설정할 수 있습니다.

```csharp
public class BaseClass
{
    public string Name;
    protected int MyProperty;

    // 생성자를 통해 필드 초기화
    public BaseClass()
    {
        Name = "BaseClass";
        MyProperty = 0;
    }
}
static void Main(string[] args)
{
    BaseClass myClass = new DerivedClass();
}
```

# 상속과 다형성

C#은 [상속](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: target="_blank"}을 통해 기존 클래스의 기능을 확장할 수 있습니다. 클래스 뒤에 `:` 기호를 사용하여 상속을 명시합니다.

파생 클래스는 기본 클래스의 모든 공용 및 보호 멤버를 상속받습니다.

```csharp
public class BaseClass
{
	public string Name;
	protected int MyProperty;

	public BaseClass()
	{
		Name = "BaseClass";
		MyProperty = 0;
	}
}

public class DerivedClass : BaseClass
{
	public DerivedClass()
	{
		Name = "DerivedClass";  // 기본 클래스의 멤버 사용 가능
		MyProperty = 1;  // 기본 클래스의 멤버 사용 가능
	}
}
```

위 예제에서는 `DerivedClass`가 `BaseClass`를 상속 받고, `BaseClass`의 필드인 `Name`과 `MyProperty`를 상속 받아 사용하는 모습입니다.

이를 통해 클래스의 기능을 점점 확장해 나갈 수 있습니다.

[다형성](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: target="_blank"}은 객체가 여러 형태를 가질 수 있도록 하며, 기본 클래스 타입으로 파생 클래스 객체를 참조할 수 있게 합니다.

이를 통해 메서드 오버라이딩을 활용하여 런타임 다형성을 구현할 수 있습니다.

```csharp
public class BaseClass
{
    public string Name;
    protected int MyProperty;

    public BaseClass()
    {
        Name = "BaseClass";
        MyProperty = 0;
    }

    public virtual void PrintInfo()
    {
        Console.WriteLine($"Name: {Name}, MyProperty: {MyProperty}");
    }
}
public class DerivedClass : BaseClass
{
    private string description;

    public DerivedClass()
    {
        Name = "DerivedClass";
        MyProperty = 1;
        description = "파생 클래스만의 멤버";
    }

    public override void PrintInfo()
    {
        Console.WriteLine($"Name: {Name}, MyProperty: {MyProperty}, Description: {description}");
    }
}
static void Main(string[] args)
{
    BaseClass myClass = new DerivedClass();
    myClass.PrintInfo();    // Name: DerivedClass, MyProperty: 1, Description: 파생 클래스만의 멤버
}
```

위 예제는 `DerivedClass`가 `BaseClass`를 상속 받아 `PrintInfo()`를 재정의 하고, `Main`에서 `DerivedClass`를 `BaseClass` 타입으로 다룰 수 있게 하여 재정의 된 `PrintInfo()`를 호출하는 모습입니다.

메서드 오버라이딩이 되어있기 때문에, `PrintInfo()`는 `DerivedClass`의 함수를 호출하고 있습니다.

## base 키워드

하지만, 오버라이딩 된 `PrintInfo()`를 호출하게 되면 `DerivedClass`의 함수를 호출하는데, 만약 부모 클래스에서 처리하고 와야 할 동작이 정의되어 있다면 어떻게 할까요?

예를 들어, `PrintInfo()`가 해당 클래스가 가지고 있는 정보를 출력하는 기능이라면, 다음과 같이 번거롭게 같은 코드를 반복해야 할까요?

```csharp
public class BaseClass
{
    public virtual void PrintInfo()
    {
        Console.WriteLine($"Class Name: {Name}");
        Console.WriteLine($"MyProperty: {MyProperty}");
    }
}
public class DerivedClass : BaseClass
{
    public override void PrintInfo()
    {
        Console.WriteLine($"Class Name: {Name}");
        Console.WriteLine($"MyProperty: {MyProperty}");
        Console.WriteLine($"Description: {description}");
    }
}
```

위 코드는 "확장"에 알맞지 않습니다.

부모 클래스의 기능에서 수정이 발생하면 상속 받은 클래스의 기능까지 전부 수정해야 하는 번거로움이 발생하게 됩니다.

이때, `base` 키워드를 사용하면 부모 클래스에 접근할 수 있습니다.

`base` 키워드는 C#에서 <span class="font_highlight">기본 클래스(Base Class)의 멤버에 접근할 때 사용</span>됩니다.

주로 **상속 관계**에 있는 클래스에서 부모 클래스 인스턴스의 생성자, 메서드, 필드 등에 접근하기 위해 사용됩니다.

```csharp
public class BaseClass
{
	public string Name;
	protected int MyProperty;

	public BaseClass()
	{
		// 기존 코드
	}
	public BaseClass(string name, int myProperty)
	{
		Name = name;
		MyProperty = myProperty;
	}

	public virtual void PrintInfo()
	{
		Console.WriteLine($"Class Name: {Name}");
		Console.WriteLine($"MyProperty: {MyProperty}");
	}
}
public class DerivedClass : BaseClass
{
	private string description;

	public DerivedClass()
	{
		// 기존 코드
	}
	public DerivedClass(string description) : base("DerivedClass", 1)   // 부모 클래스의 생성자 호출
	{
		this.description = description;
	}

	public override void PrintInfo()
	{
		base.MyProperty = 2;    // 부모 클래스의 protected 멤버에 접근 가능
		base.PrintInfo();       // 부모 클래스의 메서드 호출

		Console.WriteLine($"Description: {description}");
	}
}
```

위 코드를 보면, 매개변수를 받는 생성자의 이니셜라이저(Initializer)에서 `base()`를 사용해 부모 클래스의 생성자를 호출하고 있습니다.
<br>
또한, `PrintInfo()`에서는 `base` 키워드에 멤버 접근 연산자(`.`)를 사용해서 부모 클래스의 필드에 접근하거나 부모 클래스 함수를 호출하고 있습니다.



## 이니셜라이저(Initializer)와 this()

`base` 키워드가 상속 관계에서 부모 클래스 인스턴스를 가리키는 키워드라면,

`this` 키워드는 <span class="font_highlight">현재 클래스 인스턴스를 가리키는 키워드</span>입니다.

이때, 이니셜라이저(Initializer)에서 `base()`를 호출하던 것처럼 `this()`도 호출할 수 있는데, 이때의 `this()`는 현재 클래스의 다른 생성자를 호출하는 데 사용됩니다.

```csharp
public class MyClass
{
    public int Value1;
    public int Value2;

    // 기본 생성자
    public MyClass() : this(0, 0)
    {
        Console.WriteLine("Default constructor");
    }

    // 매개변수가 있는 생성자
    public MyClass(int value1) : this(value1, 0)
    {
        Console.WriteLine("Single parameter constructor");
    }

    // 두 매개변수가 있는 생성자
    public MyClass(int value1, int value2)
    {
        Value1 = value1;
        Value2 = value2;
        Console.WriteLine("Two parameter constructor");
    }
}
```

이 예제에서, 모든 생성자는 최종적으로 두 매개변수를 받는 생성자를 통해 초기화를 수행합니다. 이를 통해 코드 중복을 줄이고, 생성자 간의 일관성을 유지할 수 있습니다.

---

# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[[OOP] 상속성(Inheritance)](/posts/oop-%EC%83%81%EC%86%8D%EC%84%B1(inheritance)/){: }

[[OOP] 다형성(Polymorphism)](/posts/oop-%EB%8B%A4%ED%98%95%EC%84%B1(polymorphism)/){: }