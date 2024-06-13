---
title: C＃의 접근 제한자와 어셈블리(Assembly)
date: 2024-06-03 17:43:32 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 객체 지향
math: false
type: C#
keywords:
    - 어셈블리
    - 접근 제한자
    - Assembly
---

| 접근 지정자               | 설명                                    |
| -------------------- | ------------------------------------- |
| `public`             | 모든 코드에서 접근 가능                         |
| `protected`          | 동일 클래스 및 파생 클래스에서 접근 가능               |
| `private`            | 동일 클래스 내에서만 접근 가능                     |
| `internal`           | 동일 어셈블리 내에서 접근 가능                     |
| `protected internal` | 동일 어셈블리 내에서 또는 파생 클래스에서 접근 가능         |
| `private protected`  | 동일 클래스 내에서 또는 동일 어셈블리의 파생 클래스에서 접근 가능 |

# 동일 어셈블리란?

어셈블리(Assembly)는 .NET에서 코드와 리소스를 패키징한 단위입니다.
<br>
어셈블리는 `.exe` 또는 `.dll` 파일 형태로 존재합니다. 간단히 말해, C# 프로젝트를 빌드하면 생성되는 결과물(`.exe`나 `.dll` 파일)이 어셈블리입니다.
<br>
즉, 동일 어셈블리란, 같은 `.exe` 또는 `.dll` 파일 내의 코드를 의미합니다.

보통은 프로젝트 단위로 같은 결과물을 출력하기 때문에, 한 프로젝트 내에서는 테스트하기 어렵고, 한 솔루션 내에서 여러 프로젝트를 만들어 테스트 해볼 수 있습니다.

## 예시를 통한 이해

예를 들어, 두 개의 프로젝트(Assembly1, Assembly2)를 생성하고, 종속성을 설정해서 Assembly2가 Assembly1을 참조하도록 설정하면 다음과 같은 테스트를 해볼 수 있습니다.

#### 클래스 A가 있는 파일 (Assembly1)

```csharp
namespace Assembly1
{
    public class ClassA
    {
	    // 접근 제한자를 지정한 각 필드들...
        public int PublicField;         // 모든 곳에서 접근 가능
        private int PrivateField;       // ClassA 내에서만 접근 가능
        protected int ProtectedField;   // ClassA 및 상속받은 클래스에서만 접근 가능
        internal int InternalField;                    // Assembly1.dll 내에서 접근 가능
        protected internal int ProtectedInternalField; // Assembly1.dll 내에서 또는 상속받은 클래스에서 접근 가능
        private protected int PrivateProtectedField;   // Assembly1.dll 내에서 상속받은 클래스에서 접근 가능

        public void AccessFields()
        {
            PublicField = 1;
            PrivateField = 2;
            ProtectedField = 3;
            InternalField = 4;
            ProtectedInternalField = 5;
            PrivateProtectedField = 6;
        }
    }
}
```

#### 클래스 B가 있는 파일 (Assembly1)

같은 프로젝트(Assembly1)의 다른 .cs 파일에서 ClassB를 정의하면 같은 어셈블리입니다.

```csharp
namespace Assembly1
{
    public class ClassB
    {
        public void AccessFields()
        {
            ClassA a = new ClassA();
            a.InternalField = 4;          // 접근 가능 (동일 어셈블리)
            a.ProtectedInternalField = 5; // 접근 가능 (동일 어셈블리)
            // a.PrivateProtectedField = 6; // 상속 받지 않았으므로 접근 불가능
        }
    }
}
```

#### 클래스 C가 있는 파일 (Assembly2, 다른 어셈블리)

다른 프로젝트(Assembly2)에서 `ClassA`를 상속 받으면 다른 어셈블리이므로 공개되거나 상속과 관련된 접근 제한자에 접근할 수 있습니다.

```csharp
using Assembly1;

namespace Assembly2
{
    public class ClassC : ClassA
    {
        public void AccessFields()
        {
            PublicField = 1;          // 접근 가능
            // PrivateField = 2;      // 접근 불가능
            
            ProtectedField = 3;       // 접근 가능 (상속받은 클래스)
            // InternalField = 4;     // 접근 불가능 (다른 어셈블리)
            ProtectedInternalField = 5; // 접근 가능 (상속받은 클래스)
            // PrivateProtectedField = 6; // 접근 불가능 (다른 어셈블리)
        }
    }
}
```

위 예제에서 볼 수 있듯이, `internal` 접근 제한자는 동일 어셈블리(같은 `.dll` 또는 `.exe` 파일) 내에서는 접근이 가능하지만, 다른 어셈블리에서는 접근이 불가능합니다.
<br>
`protected internal`은 동일 어셈블리 내에서 접근이 가능하고, 다른 어셈블리라도 상속받은 클래스에서는 접근이 가능합니다.