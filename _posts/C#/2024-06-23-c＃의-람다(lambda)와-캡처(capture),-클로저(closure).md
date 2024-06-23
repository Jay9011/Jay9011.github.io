---
title: C＃의 람다(Lambda)와 캡처(Capture), 클로저(closure)
date: 2024-06-23 16:03:37 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 유연성
  - 함수
  - 편의성
math: false
type: C#
keywords:
  - 람다
  - 클로저
  - 캡처
  - Capture
  - closure
  - Lambda
---

람다(lambda)는 일반적으로 <span class="important">**익명 함수(anonymous function)**</span> 혹은 이름이 없는 작은 함수로, 다른 함수 내에서 **인라인**으로 정의되거나, **간단한 표현식**을 실행하기 위해 사용됩니다.

# 일반적인 람다 함수의 특징

1. **익명성**: 람다 함수는 굳이 이름을 붙이지 않고, 즉석에서 정의하고 사용할 수 있습니다.
2. **간결함**: 람다 함수는 간단한 표현식을 실행하기 위해 주로 사용되며, 코드의 간결성을 높여줍니다.
3. **고차 함수 지원**: 람다 함수는 다른 함수의 인자로 전달되거나, 다른 함수에서 반환될 수 있습니다. 이를 통해 함수형 프로그래밍의 패턴을 지원합니다.

C#의 <span class="keyword">**람다식(Lambda Expression)**</span>은 메서드나 대리자를 간결하게 표현할 수 있는 구문으로, 익명 메서드와 비슷한 기능을 합니다.
<br>
이는 델리게이트로 표현될 수 있으며, 리턴 값이 없는 람다식은 [Action](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/#action-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8){: target="_blank"}, 리턴 값이 있는 람다식은 [Func](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/#func-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8){: target="_blank"} 델리게이트에 해당합니다.

# 람다식 정의

람다식은 `=>` 연산자를 사용하여 입력 매개변수와 식 본문을 구분하여 정의합니다.

```text
(매개변수) => 식
```

식에는 간단한 Statement(한 줄짜리 표현식) 혹은 블록(중괄호)를 사용한 여러 Statement(여러 줄 코드)를 사용할 수 있습니다.
<br>
이 종류에 따라 <span class="keyword">**식 람다(Expression Lambda)**</span> 혹은 <span class="keyword">**문 람다(Statement Lambda)**</span>라고 부르기도 합니다.

또한, 이 <span class="important">**'식'이 리턴 값**</span>이 됩니다.

## 예제

```csharp
// Action을 받기 위한 테스트 메서드
static void ActionTest(Action action)
{
	action();
}
// Func를 받기 위한 테스트 메서드
static void FuncTest<T>(Func<T, T, bool> func, T a, T b)
{
	if (func(a, b))
		Console.WriteLine("Func Test True");
	else
		Console.WriteLine("Func Test False");
}
static void Main(string[] args)
{
	// 매개변수가 없는 식람다
	ActionTest(   () => Console.WriteLine("ActionTest")   ); // 결과: ActionTest

	// 매개변수가 있는 식람다
	FuncTest(     (int a, int b) => a > b           , 1, 2); // 결과: Func Test False

	// 매개변수가 없는 문람다
	Action action = () =>
	{
		Console.WriteLine("Statement Lambda");
		Console.WriteLine("ActionTest");
	};
	ActionTest(action); // 결과: Statement Lambda\r\n ActionTest

	// 매개변수가 있는 문람다
	Func<int, int, bool> func = (a, b) =>
	{
		if (a > b)
		{
			return false;
		}
		else
		{
			return true;
		}
	};
	FuncTest(func, 1, 2); // 결과: Func Test True
}
```

위 예는 다양한 형태로 람다식을 사용하는 모습을 보여줍니다.

## 정리

1. 람다식의 매개변수는 있을 수도, **없을 수도** 있습니다.
2. 람다식의 매개변수의 자료형은 컴파일러가 유추할 수 있는 경우 **생략**할 수 있습니다.
3. 람다식의 식은 보통 **리턴 값**이 되며, 블록(중괄호)으로 감쌀 수도 있습니다.
4. 리턴 값이 없는 람다식은 `Action`, 리턴 값이 있는 람다식은 `Func` 델리게이트에 해당합니다.

```text
() => 리턴    // 리턴 값은 void(없음)이 될 수 있음
(매개변수) => 리턴    // 매개 변수의 자료형은 유추가 가능한 경우 생략 가능
() => { 리턴 }
(매개변수) => { 리턴 }
```

최대한 다양한 형태의 람다식을 보여준다고 예제를 만들었는데, 잘 전달 되었을지 모르겠습니다.

# 람다식의 장점

1. **간결함**: 코드가 짧고 간결하여 읽기 쉽습니다.
2. **가독성**: 코드의 의도를 명확하게 표현할 수 있습니다.
3. **유연성**: 대리자나 익명 메서드 대신 사용할 수 있어 코드를 더 유연하게 작성할 수 있습니다.

# 캡처(Capture)

아마 C++을 배우고 오셨다면, 람다식의 대괄호(`[]`) 안에 복사나 참조로 람다식 외부의 값을 람다식 내부에서 사용할 수 있게 하는 방법에 대해서 보셨을 수도 있을 것 같습니다.

캡처란, 람다식이 선언된 시점의 <span class="font_highlight">**주변 환경(context)**에 있는 변수들을 **"캡처"**하여, 나중에 람다식이 실행될 때도 그 변수들에 **접근**할 수 있게 하는 메커니즘</span>을 의미합니다.

C#의 람다 표현식에도 C++의 람다와 유사한 캡처 기능이 있습니다.
<br>
C#에서 람다 표현식은 주변 스코프의 변수들을 캡처하여 사용하는데, 이를 **클로저(closure)**라고 부릅니다. <span class="font_highlight">C#에서는 캡처된 변수는 기본적으로 **참조로 캡처**</span>되므로, 외부 변수의 값이 변경되면 람다 표현식에서도 변경된 값이 반영됩니다.

## 클로저(closure)

클로저를 쉽게 설명하자면, 무명 메서드나 람다식이 자신의 스코프 **외부의 메서드에서 정의된 변수**를 사용할 때, 그 무명 메서드나 람다식을 클로저라고 부릅니다.

즉, 클로저는 무명 메서드나 람다식이 <span class="font_highlight">자신의 스코프 외부에서 정의된 변수를 캡처하여, 그 변수를 유지하고 사용할 수 있는 메서드</span>입니다

## 예제

```csharp
static void Main(string[] args)
{
	int capture = 10;
	
	// 람다식을 사용한 클로저
	Action<int> AddTest = x => capture += x;
	AddTest(5);
	Console.WriteLine(capture); // 결과: 15
	
	Func<int, int> multiplier = CreateMultiplier(2);
	// 즉, 람다가 생성될 때의 a의 값이 2로 고정됨
	Console.WriteLine(multiplier(10)); // 결과: 20
}

static Func<int, int> CreateMultiplier(int a)
{
    // 반환 값으로 람다식을 사용
    // 이때, a는 람다식 외부의 변수 a를 참조하고 있으므로 클로저가 생성됨
    return x => a * x;
}
```

위 예제는 람다식이 외부 변수를 사용할 때, 그 변수를 캡쳐해서 클로저가 만들어지는 예제입니다.
<br>
`x => capture += x`에서는 람다식 외부에서 정의된 `int capture`를 캡처하고, 매개변수로 전달된 값을 캡처된 변수에 더하는 모습입니다.
<br>
C#에서는 캡처된 변수는 기본적으로 **참조로 캡처**되기 때문에, 이후 따로 `capture`를 출력해보면 `15`가 출력되는 모습을 볼 수 있습니다.

<br>
`Func<int, int> CreateMultiplier(int a)`에서는 람다식을 반환해 주는 메서드를 만들었습니다.
<br>
람다식 혹은 델리게이트는 <span class="keyword">**일급 객체**</span>로, 다음과 같은 특징을 갖습니다.

1. **변수에 할당**될 수 있습니다.
2. **함수의 인자**로 전달될 수 있습니다.
3. **함수의 반환값**으로 사용될 수 있습니다.

즉, 람다식을 반환 값으로 사용할 수 있으므로, 메서드 내에서 익명 메서드를 만들어 반환해 줄 수 있습니다.

위 예제의 함수 호출(`CreateMultiplier(2);`)로 람다식을 만들어 줄 때, 람다식은 함수의 매개변수(`int a`)를 캡처하게 됩니다.
<br>
즉, 람다가 생성될 때의 a의 값이 2로 고정되고, 이후 람다식을 호출(`multiplier(10)`) 해보면 `a`가 `2`로 고정되어 `20`을 출력하는 모습을 볼 수 있습니다.

---

# 참고

[[delegate] 03. 익명 델리게이트와 Func, Action](/posts/delegate-%EC%9D%B5%EB%AA%85-%EB%8D%B8%EB%A6%AC%EA%B2%8C%EC%9D%B4%ED%8A%B8%EC%99%80-func,-action/){: }

[람다 대수 - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/%EB%9E%8C%EB%8B%A4_%EB%8C%80%EC%88%98#%ED%95%B5%EC%8B%AC_%EA%B0%9C%EB%85%90)

[람다대수 - 나무위키 (namu.wiki)](https://namu.wiki/w/%EB%9E%8C%EB%8B%A4%EB%8C%80%EC%88%98#s-2.1.1)