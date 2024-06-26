---
title: 리플렉션 (Reflection)
date: 2024-06-24 16:31:57 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 메타데이터
math: false
type: C#
keywords:
  - 리플렉션
  - Reflection
---

리플렉션(Reflection)이라는 용어는 "반사", "(거울에 반사되어 비친) 상", "반영", "묘사(설명)" 등의 뜻이 있습니다.
<br>
프로그래밍에서 리플렉션은 프로그램이 런타임에 자신을 반사 혹은 묘사하여 **자신의 구조와 동작**에 대한 **정보를 얻고**, 이를 바탕으로 **동적**으로 동작을 **변경**할 수 있는 기능을 의미합니다.

즉, <span class="keyword">**리플렉션(Reflection)**</span>은 <span class="font_highlight">프로그램이 실행 시간(runtime)에 자신의 메타데이터(데이터에 대한 데이터)를 확인하고 조작할 수 있는 능력</span>을 나타냅니다.

C#의 리플렉션(Reflection)은 프로그램이 런타임에 어셈블리, 모듈, 타입, 메서드, 속성 등의 메타데이터에 접근하고 이를 검사 및 조작할 수 있도록 하는 기능입니다.

리플렉션은 주로 `System.Reflection` 네임스페이스에서 제공됩니다.

리플렉션의 핵심은 런타임 시점에서 코드의 메타데이터를 확인하고, 객체의 타입을 검사하며, 해당 타입의 인스턴스를 생성하거나, 메서드를 호출하고, 속성 값을 읽거나 설정하는 기능을 제공하는 것입니다.

# 리플렉션의 기능

C#의 리플렉션은 다음과 같은 기능들을 할 수 있습니다.

## 어셈블리나 모듈 정보 확인 및 조작

C#의 `Assembly` 클래스는 어셈블리를 나타내며, 어셈블리의 메타데이터 및 타입 정보를 얻고 조작할 수 있는 다양한 기능을 제공합니다.

어셈블리는 C#에서 코드의 기본 배포 단위이며, 단일 파일 또는 여러 파일로 구성될 수 있습니다.
<br>
[접근 제한자와 어셈블리에 대한 글은 링크](/posts/c-%EC%9D%98-%EC%A0%91%EA%B7%BC-%EC%A0%9C%ED%95%9C%EC%9E%90%EC%99%80-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC(assembly)/){: target="_blank"}를 참고해 주세요.

```csharp
namespace _15_ReflectionAndAttribute
{
    public static class ReflectionClass
    {
        public static void Run()
        {
            Assembly assembly = Assembly.GetExecutingAssembly();

            foreach (Type type in assembly.GetTypes())
            {
                Console.WriteLine(type.Name);
            }
            
	        Module[] modules = Assembly.GetExecutingAssembly().GetModules();
			foreach (Module module in modules)
			{
			    Console.WriteLine(module.Name);
			}
        }
    }
}
```

```csharp
namespace _15_ReflectionAndAttribute
{
    internal class Program_TestClass
    {
        private int PrivateInt;
        protected double ProtectedDouble;
        public string PublicString;
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            ReflectionClass.Run();
            /* -- 실행 결과 --
             * EmbeddedAttribute        ──┐
             * NullableAttribute          │
             * NullableContextAttribute   │  _15_ReflectionAndAttribute 어셈블리 파일
             * Program_TestClass          │
             * Program                    │
             * ReflectionClass          ──┘
             * _15_ReflectionAndAttribute.dll
             */
        }
    }
}
```

위 예제는 `.cs`파일을 두 개로 나누어서 테스트 해본 결과입니다.
<br>
중간에 `Assembly.GetTypes()`으로 타입에 대한 메타데이터를 가져온 것을 볼 수 있습니다.
<br>
기존에 만들어져있는 것으로 보이는 타입들과 `.cs`파일에 직접 만든 `Program_TestClass` 클래스, `Program` 클래스, `ReflectionClass` 클래스가 출력된 것을 볼 수 있습니다.

모듈에 대한 정보는 `GetModules()`로 가져올 수 있습니다.

### Assembly.LoadFrom()

`Assembly` 클래스에는 정말 다양한 기능을 제공하지만, 간단하게 어셈블리를 동적으로 로드할 수 있는 `LoadFrom()`만 보도록 하겠습니다.

```csharp
namespace _14_Predicate
{
    static class Utilities
    {
        public static int Count<T>(IEnumerable<T> items, Predicate<T> predMethod)
        {
            int count = 0;
            foreach (var item in items)
            {
                if (predMethod(item))
                {
                    count++;
                }
            }
            return count;
        }
    }
	internal class Program
    {
        static void Main(string[] args)
        {
        }
    }
}
```

```csharp
namespace _15_ReflectionAndAttribute
{
    public static class ReflectionClass
    {
        public static void Run()
        {
            string baseDirectory = AppDomain.CurrentDomain.BaseDirectory;
            string solutionDirectory = Directory.GetParent(baseDirectory).Parent.Parent.Parent.Parent.FullName;
            string path = solutionDirectory + @"\_14_Predicate\bin\Debug\net6.0\_14_Predicate.dll";
            
            Assembly predicateAssembly = Assembly.LoadFrom(path);
            
            foreach (Type type in predicateAssembly.GetTypes())
            {
                Console.WriteLine(type.Name);
            }
        }
    }
}
```

```csharp
namespace _15_ReflectionAndAttribute
{
    internal class Program_TestClass
    {
        private int PrivateInt;
        protected double ProtectedDouble;
        public string PublicString;
    }
    internal class Program
    {
        static void Main(string[] args)
        {
            ReflectionClass.Run();
	        /*
			 * EmbeddedAttribute        ──┐
			 * NullableAttribute          │
			 * NullableContextAttribute   │  Predicate 어셈블리 파일
			 * Utilities                  │
			 * Program                    │
			 * <>c                      ──┘
			 */
        }
    }
}
```

위 예제는 서로 다른 dll(어셈블리) 파일을 로드해서 다른 어셈블리의 타입에 대한 메타데이터를 가져온 모습입니다.
<br>
현재 어셈블리에 해당하는 타입을 제외하고, Predicate 어셈블리에 존재하는 `Utilities`와 `Program`이 출력되는 것을 볼 수 있습니다.

## 특정 타입 정보 확인

어셈블리 내에 정의된 모든 `Type`의 정보를 가져올 수도 있습니다.

어셈블리에서 `GetTypes()`를 해서 가져온 `Type`이나, 특정 객체에  `GetType()`을 해서 가져온 타입에 대해 다양한 메타데이터를 가져올 수 있습니다.

```csharp
internal class Program_TestClass
{
    private int PrivateInt;
    protected double ProtectedDouble;
    public string PublicString;
}
internal class Program
{
	// 타입 접근 제한자를 문자열로 리턴하는 메서드
    static string GetAccessName(FieldInfo field)
    {
        if (field.IsPublic)
        {
            return "Public";
        }
        else if (field.IsPrivate)
        {
            return "Private";
        }
        else if (field.IsFamily)
        {
            return "Protected";
        }
        else
        {
            return "Internal";
        }
    }
    static void TypeCheck(Type type)
    {
        // 해당 타입의 모든 필드 정보를 얻어온다.
        FieldInfo[] fields = type.GetFields(
	        BindingFlags.Public 
	        | BindingFlags.NonPublic 
	        | BindingFlags.Static 
	        | BindingFlags.Instance);
        
        foreach (FieldInfo field in fields)
        {
            Console.WriteLine($"{GetAccessName(field)} {field.FieldType.Name} {field.Name}");
        }
    }

    static void Main(string[] args)
    {
        Program_TestClass test = new Program_TestClass();
        // 해당 객체에 대한 타입 정보를 가져온다.
        TypeCheck(test.GetType());
        /*
         * Private Int32 PrivateInt
         * Protected Double ProtectedDouble
         * Public String PublicString
         */
    }
}
```

위 예제는 특정 객체에 `GetType()`를 하여 `Type`을 가져온 뒤, `GetFields()`로 모든 필드에 대한 `FieldInfo`를 저장하고, 그 내용을 출력하는 모습입니다.

## 동적으로 인스턴스 생성

`Activator` 클래스를 사용하여 런타임에 동적으로 객체를 생성할 수도 있습니다.

```csharp
object? DynamicInstance = Activator.CreateInstance(typeof(Program_TestClass));
Console.WriteLine($"{DynamicInstance.GetType().Name}");
/* -- 실행 결과 --
 * Program_TestClass
 */
```

## 동적으로 메서드 호출

`Type`에 `GetMethod()`로 특정 타입의 메서드 정보를 런타임에 가져오고, `MethodInfo`를 통해 메서드를 호출할 수 있습니다.

```csharp
namespace _15_ReflectionAndAttribute
{
    internal class Program_TestClass
    {
        private int PrivateInt;
        protected double ProtectedDouble;
        public string PublicString;

        private void PrivateMethod()
        {
            Console.WriteLine("Program Private Method 호출");
        }
    }
    
    internal class Program
	{
		static void Main(string[] args)
		{
			object? DynamicInstance = Activator.CreateInstance(typeof(Program_TestClass));

            MethodInfo? method = DynamicInstance.GetType().GetMethod("PrivateMethod", BindingFlags.NonPublic | BindingFlags.Instance);
            method?.Invoke(DynamicInstance, null);
            /* -- 실행 결과 --
             * Program Private Method 호출
             */
        }
    }
}
```

위 예제는 `Activator.CreateInstance()`로 런타임에 인스턴스를 만들고, 해당 객체의 타입에서 `"PrivateMethod"`라는 이름의 메서드를 가져온 후, `MethodInfo`의 `Invoke`로 가져온 메서드를 동적으로 호출하고 있습니다.

## 어트리뷰트 정보 가져오기

클래스, 메서드, 속성 등에 적용된 **어트리뷰트**를 `GetCustomAttributes()` 메서드를 사용하여 읽고 검사할 수 있습니다.

커스텀 어트리뷰트는 `System.Attribute`를 상속 받아서 만들 수 있습니다.

```csharp
namespace _15_ReflectionAndAttribute
{
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
        private int PrivateInt;
        protected double ProtectedDouble;
        public string PublicString;

        private void PrivateMethod()
        {
            Console.WriteLine("Program Private Method 호출");
        }
    }

	internal class Program
	{
		static void Main(string[] args)
		{
			Type type = typeof(Program_TestClass);
			object[] attributes = type.GetCustomAttributes(typeof(CustomAttribute), true);
			foreach (CustomAttribute attribute in attributes)
			{
			    Console.WriteLine(attribute.Description);
			}
			/* -- 실행 결과 --
			 * Reflection 어셈블리 파일의 TestClass
			 */
		}
	}
}
```

위 예제는 `System.Attribute`를 상속 받은 `CustomAttribute`를 만들고, `Program_TestClass` 클래스 위에 어트리뷰트를 적용한 뒤, `GetCustomAttributes()`로 어트리뷰트를 가져와서 출력하고 있습니다.

## 리플렉션 표

| 기능         | 키워드 및 클래스              | 설명                                                                                   |
|--------------|-------------------------------|----------------------------------------------------------------------------------------|
| 어셈블리      | `Assembly`                    | 어셈블리의 메타데이터를 담고 있는 클래스입니다.                                             |
| 어셈블리 로드 | `Assembly.Load`, `Assembly.LoadFrom` | 어셈블리를 로드하여 어셈블리의 메타데이터에 접근할 수 있습니다.                      |
| 모듈          | `Module`                    | 모듈의 메타데이터를 담고 있는 클래스입니다.                                                 |
| 모듈 정보 얻기 | `Assembly.GetModules()`     | 모듈의 정보를 얻기 위한 메서드입니다.                                                       |
| 형식 정보     | `Type`                      | 형식(Type)의 메타데이터를 담고 있는 클래스입니다.                                             |
| 형식 정보 얻기 | `typeof`, `GetType`        | 형식(Type)의 정보를 얻기 위한 키워드 및 메서드입니다. `typeof`는 컴파일 타임, `GetType`은 런타임에 사용합니다. |
| 메서드 정보   | `MethodInfo`, `Type.GetMethods`, `Type.GetMethod` | 형식의 메서드 정보를 조사하고 특정 메서드를 호출할 수 있습니다.                      |
| 필드 정보     | `FieldInfo`, `Type.GetFields`, `Type.GetField` | 형식의 필드 정보를 조사하고 특정 필드에 접근할 수 있습니다.                          |
| 속성 정보     | `PropertyInfo`, `Type.GetProperties`, `Type.GetProperty` | 형식의 속성 정보를 조사하고 특정 속성에 접근할 수 있습니다.                  |
| 생성자 정보   | `ConstructorInfo`, `Type.GetConstructors`, `Type.GetConstructor` | 형식의 생성자 정보를 조사하고 인스턴스를 생성할 수 있습니다.          |
| 이벤트 정보   | `EventInfo`, `Type.GetEvents`, `Type.GetEvent` | 형식의 이벤트 정보를 조사하고 이벤트를 다룰 수 있습니다.                             |
| 특성 정보     | `Attribute`, `Type.GetCustomAttributes`, `MemberInfo.GetCustomAttributes` | 형식이나 멤버에 적용된 특성(어트리뷰트) 정보를 조사할 수 있습니다.   |
| 동적 생성     | `Activator.CreateInstance`   | 런타임에 인스턴스를 동적으로 생성할 수 있습니다.                                     |
| 메서드 호출   | `MethodInfo.Invoke`, `Delegate` | 메서드를 동적으로 호출하거나 대리자를 사용하여 호출할 수 있습니다.                   |


<br>
리플렉션은 그 기능도 다양하고 많아서 정말 강력한 도구이지만, 저는 다양하게 많이 사용해보지는 못한 것 같습니다.
<br>
대부분 타입 정보를 확인하거나 어트리뷰트를 가져와서 사용하는 정도로만 써본 것 같습니다.
<br>
정말 기능이 다양해서 필요한 기능을 찾아가며 사용해 보는게 좋을 것 같습니다.

---

# 참고

[C＃의 접근 제한자와 어셈블리(Assembly)](/posts/c-%EC%9D%98-%EC%A0%91%EA%B7%BC-%EC%A0%9C%ED%95%9C%EC%9E%90%EC%99%80-%EC%96%B4%EC%85%88%EB%B8%94%EB%A6%AC(assembly)/){: }

[Attribute(어트리뷰트)와 AttributeTargets](/posts/attribute-(%EC%96%B4%ED%8A%B8%EB%A6%AC%EB%B7%B0%ED%8A%B8)%EC%99%80-attributetargets/){: }