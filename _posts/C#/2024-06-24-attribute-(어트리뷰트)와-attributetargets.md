---
title: Attribute(어트리뷰트)와 AttributeTargets
date: 2024-06-24 19:42:28 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 메타데이터
math: false
type: C#
keywords:
    - Attribute
    - AttributeTargets
    - 어트리뷰트
---

어트리뷰트(Attribute)는 코드 요소에 대한 <span class="font_highlight">메타데이터를 추가하는 방법</span>입니다.

어트리뷰트는 클래스, 메서드, 프로퍼티, 필드, 이벤트 등 다양한 코드 요소에 적용될 수 있으며, 주로 런타임 시 또는 컴파일 타임에 특정 동작을 변경하거나 추가 정보를 제공하기 위해 사용됩니다.

이 추가 정보는 런타임에 [리플렉션](/posts/%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98-(reflection)/){: target="_blank"}을 통해 접근할 수 있습니다.

어트리뷰트는 대괄호 `[]`를 사용하여 코드 요소 위에 적용됩니다.

# 어트리뷰트 사용법

어트리뷰트는 대괄호 안에 어트리뷰트 클래스 이름을 쓰고, 필요에 따라 매개변수를 전달하여 사용합니다.

```text
[어트래뷰트 이름(어트리뷰트 매개변수)]
public class TestClass()
{
    // ...
}
```

# 어트리뷰트 클래스 정의

어트리뷰트는 사용자가 원하는대로 만들어 메타데이터를 추가할 수 있는데,
<br>
사용자 정의 어트리뷰트를 만들기 위해서는 `System.Attribute`를 상속하는 클래스를 정의해야 합니다.

```csharp
public class CustomAttribute : System.Attribute
{
    public string Description { get; set; }
    public CustomAttribute(string description)
    {
        Description = description;
    }
}

[Custom("Reflection 어셈블리 파일의 TestClass")]
internal class Program_TestClass
{
	// ...
}
```

# 주요 기본 어트리뷰트

C#에서 기본적으로 제공되는 어트리뷰트들이 존재합니다.

| 어트리뷰트            | 설명                                           | 예시                                                  |
| ---------------- | -------------------------------------------- | --------------------------------------------------------------------------- |
| `Obsolete`       | 해당 요소가 더 이상 사용되지 않음을 나타내며, 경고 또는 오류를 발생시킵니다. | `[Obsolete("Use NewMethod instead")]`<br>public void OldMethod()<br>{ }   |
| `Serializable`   | 클래스를 직렬화 가능하게 만듭니다.                          | `[Serializable]`<br>public class MySerializableClass<br>{ }                  |
| `NonSerialized`  | 직렬화하지 않을 필드를 나타냅니다.                          | \[Serializable\]<br>public class MyClass<br>{<br>`\[NonSerialized\]`<br>public int Temp;<br>}      |
| `DllImport`      | 외부 DLL 함수를 가져올 때 사용됩니다.                      | `[DllImport("user32.dll")]`<br>private static extern int MessageBox(int hWnd, string text, string caption, uint type);    |
| `Conditional`    | 특정 조건에서만 메서드를 호출하도록 합니다.                     | `[Conditional("DEBUG")]`<br>public void DebugOnlyMethod() <br>{ }                        |
| `Flags`          | 열거형이 비트 필드(비트 마스크)로 처리됨을 나타냅니다.              | `[Flags]`<br>public enum MyEnum<br>{ }                                              |
| `AttributeUsage` | 사용자 정의 어트리뷰트의 사용 대상을 지정합니다.                  | `[AttributeUsage(AttributeTargets.Class \| AttributeTargets.Method)]`<br>public class CustomAttribute : System.Attribute<br>{ } |
| `StructLayout`   | 구조체의 메모리 레이아웃을 개발자가 제어하도록 합니다. (언매니지드)       | `[StructLayout(LayoutKind.Sequential)]`<br>public struct SequentialLayoutStruct<br>{<br>    public int i;<br>    public float f;<br>}   |
| `FieldOffset`    | 구조체 멤버의 언매니지드 메모리 레이아웃(메모리 위치)을 직접 제어합니다.    | `[StructLayout(LayoutKind.Explicit)]`<br>public struct Union<br>{ <br>`[FieldOffset(0)]` public int i;<br>`[FieldOffset(0)]` public float f;<br>} |

# 어트리뷰트 적용 범위

`System.AttributeUsage`는 어트리뷰트를 적용할 수 있는 범위를 제어하는데 사용됩니다.

이 적용될 수 있는 범위를 `AttributeTargets`이라고 하는데, 이를 통해 어트리뷰트가 적용될 수 있는 코드 요소를 클래스부터 모듈까지 다양한 범위와 조합(`|`)으로 적용시킬 수 있습니다.

`AttributeTargets` 열거형의 종류를 표로 정리해 보았습니다.

| AttributeTargets 값 | 범위       |
| ------------------ | -------- |
| `All`              | 모든 요소    |
| `Enum`             | 열거형      |
| `Struct`           | 구조체      |
| `Class`            | 클래스      |
| `Interface`        | 인터페이스    |
| `Field`            | 필드       |
| `Property`         | 프로퍼티     |
| `Delegate`         | 대리자      |
| `Event`            | 이벤트      |
| `Constructor`      | 생성자      |
| `Method`           | 메서드      |
| `Parameter`        | 메서드 매개변수 |
| `GenericParameter` | 제네릭 매개변수 |
| `ReturnValue`      | 메서드 반환 값 |
| `Assembly`         | 어셈블리     |
| `Module`           | 모듈       |

---

# 참고

[리플렉션 (Reflection)](/posts/%EB%A6%AC%ED%94%8C%EB%A0%89%EC%85%98-(reflection)/){: }

[AttributeTargets Enum (System) | Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/api/system.attributetargets?view=net-8.0#fields)