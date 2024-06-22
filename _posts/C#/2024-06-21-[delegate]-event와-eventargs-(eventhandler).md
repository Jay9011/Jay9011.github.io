---
title: "[delegate] 02. event와 EventArgs (EventHandler)"
date: 2024-06-21 17:55:15 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
    - 델리게이트
    - 동적
    - 참조
    - 함수
    - 포인터
    - 이벤트
math: false
type: C#
keywords:
    - 델리게이트
    - event
    - delegate
    - EventArgs
    - EventHandler
---

<span class="keyword">**event**</span>는 <span class="font_highlight">[delegate](/posts/delegate-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A0%95%EC%B2%B4,-%EC%B2%B4%EC%9D%B4%EB%8B%9D,-%EC%82%AC%EC%9A%A9-%EB%AA%A9%EC%A0%81/){: target="_blank"}의 사용 패턴을 좀 더 일반화 해서 제공하는 예약어</span>입니다.
<br>
예를 들어, 콜백 패턴 혹은 이벤트 패턴이나 옵저버 패턴 등을 구현할 때, 구현하기 쉽도록 다양한 기능을 제공합니다.

# 제약 조건과 제공 기능

`event`는 다음과 같은 제약 조건과 기능을 제공합니다.

1. **캡슐화**
	- 이벤트를 통해 구독자와 발생자 간의 상호작용이 캡슐화되어, 객체 간의 느슨한 결합을 유지할 수 있습니다. 이벤트를 발생시키는 객체는 구독자가 누구인지 알 필요가 없고, 구독자는 이벤트를 발생시키는 방법을 알 필요가 없습니다.
	- 이벤트는 <span class="font_highlight">**클래스 내부에서 선언**</span>됨으로써 응집력을 높이고, 클래스 외부에서는 이벤트를 발생시킬 수 없도록 캡슐화합니다.
2. **외부 호출 불가**
	- 이벤트는 이벤트가 정의된 클래스 외부에서 직접 호출할 수 없습니다. 이는 <span class="font_highlight">이벤트를 발생시킬 수 있는 권한이 클래스 내부에만 있음</span>을 보장합니다.
3. **델리게이트 직접 할당 불가**
	- 이벤트는 <span class="font_highlight">할당 연산자(`=`)를 통해 델리게이트를 직접 할당하거나 대입할 수 없습니다</span>. 이는 기존에 등록된 메서드 리스트를 안전하게 관리할 수 있도록 보장합니다.
4. **구독자 관리**
	- 이벤트는 <span class="font_highlight">`+=`와 `-=` 연산자를 통해서만 구독자를 추가하거나 제거</span>할 수 있습니다.
	- 이벤트가 발생하면 모든 구독자에게 알림을 보냅니다.

즉, `event`는 다음과 같은 특징을 가진 패턴을 구현할 때, `event` 예약어를 사용하면 빠르고 쉽게 패턴을 구현하며, 코드를 줄일 수 있습니다.

1. 클래스에서 이벤트(콜백)을 제공한다.
2. 클래스 외부에서 이벤트(콜백)을 구독하거나 해지하게 하고 싶다.
3. 이벤트 발생은 오직 클래스 내부에서만 가능하게 하고 싶다.
4. 이벤트가 발생하면 구독자에게 메시지를 보내고 싶다.

# event 정의

이벤트는 다음과 같은 형태로 정의할 수 있습니다.

```text
class 클래스_내부에서
{
	접근제한자 event 델리게이트 이벤트명;
}
```

만약, 특별한 델리게이트 없이 사용하고 싶다면 다음과 같이 `EventHandler`를 사용할 수 있지만, 기본적으로 이 핸들러에는 매개변수가 존재합니다.

- `object sender`: sender 매개변수는 <span class="important">**이벤트를 발생시킨 객체**</span>에 대한 참조를 전달합니다. 이를 통해 구독자들은 어떤 객체에서 발생한 이벤트인지를 구분할 수 있습니다.
- `EventArgs e`: e 매개변수는 이벤트와 관련된 <span class="important">**추가 정보**</span>를 전달합니다. 기본적으로 `EventArgs` 타입을 사용하며, 추가 데이터가 필요할 때는 이를 상속하여 커스텀 이벤트 아규먼트를 만들 수 있습니다.

```csharp
class 클래스명
{
	public event EventHandler 이벤트명;
}
```

## 이벤트 구독 및 구독 취소

이벤트를 구독(subscribe)하거나 구독을 취소(unsubscribe)하는 방법은 `+=`와 `-=` 연산자를 사용하면 됩니다.

```csharp
public void Subscribe(MyClass obj)
{
	obj.MyEvent += HandleEvent;
}

public void Unsubscribe(MyClass obj)
{
	obj.MyEvent -= HandleEvent;
}
```

# 델리게이트 대신 이벤트를 사용하는 경우

`event`를 사용하면 `delegate`를 사용하는 다양한 디자인 패턴을 더 안전하고 쉽게 구현할 수 있습니다.
<br>
예를 들어, 이전에 사용했던 `Player`의 `Action` 버튼시 행동에 대한 코드를 작성해 봅니다.

```csharp
public delegate bool ActionBtnDelegate();

internal class Player
{
    public ActionBtnDelegate actionBtnDelegates;
}
```

`delegate`를 선언하고, 이를 이용해 `Player` 내에서 액션 버튼이 눌렸을 때, 동작을 저장할 `actionBtnDelegates`를 만듭니다.
<br>
하지만, 외부에서도 델리게이트를 등록하고 제거 할 수 있어야 하니 접근 제한자를 `public`으로 만들게 되었습니다.

예상하실 수 있겠지만, 이렇게 코드를 작성하면 나중에 예기치 못한 버그나 문제가 발생할 수 있습니다.

예를 들어, 어떤 행동을 할 때마다 스테미나가 달고, 문을 여는 행동을 저장하고자 합니다.

```csharp
internal class Stamina
{
    public bool UseStamina()
    {
        Console.WriteLine("스태미너를 사용합니다.");
        return true;
    }
}
internal class Door
{
    public bool Open()
    {
        Console.WriteLine("문을 엽니다.");
        return true;
    }
}
internal class Program
{
	static void Main(string[] args)
	{
		Player player = new Player();
		Stamina stamina = new Stamina();
		player.actionBtnDelegates = stamina.UseStamina;
		
		Door door = new Door();
		player.actionBtnDelegates = door.Open;
		
		player.actionBtnDelegates.Invoke(); // 문을 엽니다.
	}
}
```

그럼, 어딘가에서 플레이어의 스테미나를 관리하는 델리게이트를 설정했을테고, 또 다른 어딘가에서 문을 여는 동작을 델리게이트에 할당합니다.
<br>
이때, 추가하는 `+=` 연산자가 아니라 대입 연산자(`=`)를 사용해버리면, 기존에 등록되어 있던 동작들이 **전부 초기화** 됩니다.
<br>
따라서, 스테미너 소모 없이 동작을 행할 수 있는 버그가 발생했습니다.
<br>
또한, `public`으로 공개되어 있기 때문에 아무데서나 해당 델리게이트를 호출할 수 있기 때문에, **의도치 않은 동작**이 발생할 수도 있습니다.

이를 해결하기 위해, 캡슐화를 하고 은닉성을 부여해 봅시다.

```csharp
public delegate bool ActionBtnDelegate();

internal class Player
{
	private ActionBtnDelegate actionBtnDelegates;

	public void AddAction(ActionBtnDelegate actionBtnDelegate)
	{
		actionBtnDelegates += actionBtnDelegate;
	}
	public void RemoveAction(ActionBtnDelegate actionBtnDelegate)
	{
		actionBtnDelegates -= actionBtnDelegate;
	}
	public void OnAction()
	{
		actionBtnDelegates?.Invoke();
	}
}
// ...
{
	Player player = new Player();
	Stamina stamina = new Stamina();
	player.AddAction(stamina.UseStamina);
	
	Door door = new Door();
	player.AddAction(door.Open);
	player.OnAction();
	// 스태미너를 사용합니다.
	// 문을 엽니다.
	
	player.RemoveAction(door.Open);
	player.OnAction();
	// 스태미너를 사용합니다.
}
```

`Player`의 델리게이트를 `private`로 보호하고, 델리게이트를 추가하거나 제거하는 메서드를 제공했습니다.
<br>
`OnAction()` 메서드의 경우에는 예제로 사용하기 위해 `public`으로 공개해뒀지만, 필요에 따라 `private`를 할 수도 있습니다.
<br>
이렇게, 캡슐화와 은닉성 제공을 통해 기존 코드보다는 안정적인 코드를 만들었습니다.

이때, 이 코드에 `event`를 사용한다면 이 코드를 줄일 수 있게 됩니다.

```csharp
public delegate bool ActionBtnDelegate();

internal class PlayerWEvent
{
    public event ActionBtnDelegate actionBtnDelegates;

    public void OnAction()
    {
        actionBtnDelegates?.Invoke();
    }
}
// ...
{
	PlayerWEvent playerWEvent = new PlayerWEvent();
	Stamina stamina = new Stamina();
	playerWEvent.actionBtnDelegates += stamina.UseStamina;
	
	Door door = new Door();
	playerWEvent.actionBtnDelegates += door.Open;
	playerWEvent.Action();
	// 스태미너를 사용합니다.
	// 문을 엽니다.
	
	playerWEvent.actionBtnDelegates -= door.Open;
	playerWEvent.Action();
	// 스태미너를 사용합니다.
}
```

`event`를 `public`으로 공개했음에도 제약사항으로 인해 외부에서 이벤트를 호출 할 수 없고, 대입 연산자를 사용할 수 없기 때문에, 제공해야 할 메서드의 양이 줄었고, 결과적으로 코드가 줄어든 것을 볼 수 있습니다.

즉, 특정 패턴을 구현하는데 있어, 제약사항과 제공 기능을 통해 좀 더 일반화 된 패턴을 구현할 수 있습니다.

<br>
다만, 가끔 `event`가 좀 더 안전하고 편한 것 같은데 `delegate`를 쓸 이유가 있을까? 하는 의문이 들수도 있지만, `delegate`는 좀 더 자유로운 코딩을 가능하게 합니다.

`delegate`는 특정 메서드 시그니처를 참조할 수 있는 형식을 제공함으로써, 매개변수로 전달하거나 리턴 타입으로 반환 할 수도 있습니다.

이때, `EventHandler`와 `event`만으로도 코드를 구성할 수 있다고 하지만, 가끔은 폭 넓은 타입을 제공하는 형식 보다, 제한된 형태의 형식을 제공함으로써 타입 안전한 방법으로 사용하는게 좋은 경우도 있습니다. (우리가 클래스를 
캡슐화하고 은닉성을 부여하는 것처럼...)

# EventArgs

위에서 잠깐 언급하고 넘어갔던 `EventArgs`는 C#에서 <span class="font_highlight">이벤트 핸들러에 전달되는 데이터의 기본 형식</span>을 정의하는 클래스입니다.

기본적으로 `EventArgs`는 데이터 없이 이벤트를 전달할 때 사용되는 빈 클래스지만, 이벤트 핸들러가 추가적인 정보를 필요로 할 때는 <span class="font_highlight">이 클래스를 **상속**하여 커스텀 이벤트 아규먼트 클래스를 만들 수 있습니다</span>.

이런 커스텀 `EventArgs`를 사용하여 <span class="font_highlight">이벤트가 발생할 때 **추가적인 데이터**를 전달</span>할 수 있습니다.

이때, 특정 커스텀 `EventArgs`로 이벤트 핸들러를 지정할 수 있는 제네릭 델리게이트를 제공합니다.

```csharp
public event EventHandler<MyEventArgs> MyEvent;
```

`EventHandler<TEventArgs>`은 제네릭 델리게이트로, `TEventArgs` 타입의 추가 데이터를 전달할 수 있습니다. 이때, `TEventArgs`는 `EventArgs`를 상속해야 합니다.

```csharp
internal class MyEventArgs : EventArgs
{
	//...
}
```

#### 예제

비록 좋은 예제를 만들지는 못했지만, 대략 이런식으로 `EventArgs`를 이용할 수 있다는 것만 봐주시면 좋을 것 같습니다.

```csharp
// 커스텀 EventArgs
internal class MyEventArgs : EventArgs
{
    public MyEventArgs(int n)
    {
        RemainStamina = n;
    }
    public int RemainStamina { get; set; }
}

internal class PlayerWEvent
{
    public event EventHandler<MyEventArgs> actionBtnDelegates;
	// 스테미나를 Player가 가지고 있도록 바꿈
    private StaminaWArgs stamina;

    public PlayerWEvent()
    {
        stamina = new StaminaWArgs();
        actionBtnDelegates += stamina.UseStamina;
    }

    public void Action()
    {
	    // 구독자에게 이벤트를 전달할 때, MyEventArgs를 넘겨줌
        actionBtnDelegates?.Invoke(this, new MyEventArgs(stamina.NowStamina));
    }
}
internal class StaminaWArgs
{
    public int NowStamina { get; set; } = 10;
    // 구독하기 위해서 EventHandler의 형태에 맞춰 (sender, e)를 매개변수로 받아 줌
    public void UseStamina(object? sender, MyEventArgs e)
    {
        NowStamina -= 10;
        Console.WriteLine("스태미너를 사용합니다.");
    }
}
internal class DoorWArgs
{
	// 구독하기 위해서 EventHandler의 형태에 맞춰 (sender, e)를 매개변수로 받아 줌
    public void Open(object? sender, MyEventArgs e)
    {
        if (e.RemainStamina < 10)
        {
            Console.WriteLine("스태미너가 부족합니다.");
            return;
        }
        Console.WriteLine("문을 엽니다.");
        return;
    }
}
// ...
static void Main(string[] args)
{
	PlayerWEvent playerWEvent = new PlayerWEvent();

	DoorWArgs door = new DoorWArgs();
	playerWEvent.actionBtnDelegates += door.Open;
	
	playerWEvent.Action();
	// 스태미너를 사용합니다.
	// 문을 엽니다.
	    
	playerWEvent.Action();
	// 스태미너를 사용합니다.
	// 스태미너가 부족합니다.
}
```

위 예제는 기존 예제에서 `Stemina`를 `Player`가 가지고 있도록 하고, 어떤 행동을 할 때마다 남은 스테미너를 커스텀 `EventArgs`에 담아서 보낼 수 있도록 구조를 변경한 것입니다.
<br>
이벤트를 호출할 때, 인자로 이벤트를 발생한 객체와 남은 스테미너를 담고있는 EventArgs를 함께 전달했습니다.
<br>
따라서, `playerWEvent.Action();`을 호출할 때, `MyEventArgs`에 담긴 스테미너 정보를 가지고 행동 여부를 판단할 수 있습니다.

# 정리

`event`는 델리게이트로 구현되는 다양한 패턴을 더 **안전**하고 **쉽게** 사용할 수 있도록 돕습니다.
<br>
이를 통해 느슨한 결합을 유지하면서 객체 간의 상호작용을 쉽게 구현할 수 있습니다.
<br>
이벤트는 **외부에서 직접 호출될 수 없으며**, **구독자는 이벤트를 통해 알림**을 받을 수 있습니다.

델리게이트 대신 `EventHandler`를 통해 **일반화된 호출**을 사용할 수 있는데, `EventHandler`는 시그니처를 일관되게 유지하고 있으며, 매개변수로는 `(object sender, EventArgs e)`를 가지고 있습니다.
<br>
이는 이벤트 핸들러의 호출을 **일관되게** 할 수 있으며, 필요시 `EventArgs`를 상속하고 **확장**하여 다양한 이벤트에서 필요한 데이터를 쉽게 전달할 수 있습니다.

---

# 참고

[[delegate] 01.델리게이트의 정의와 정체, 체이닝, 사용 목적](/posts/delegate-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A0%95%EC%B2%B4,-%EC%B2%B4%EC%9D%B4%EB%8B%9D,-%EC%82%AC%EC%9A%A9-%EB%AA%A9%EC%A0%81/){: }

[[delegate] 03. 익명 델리게이트와 Func, Action](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/){: }
