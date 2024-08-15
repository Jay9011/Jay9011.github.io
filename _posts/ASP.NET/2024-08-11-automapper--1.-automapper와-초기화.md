---
title: AutoMapper / 1. AutoMapper와 초기화
date: 2024-08-11 10:36:52 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - DAL/DAO
  - Model
  - 객체 지향
math: false
type: aspnet
keywords:
  - AutoMapper
  - ORM
---

ORM (Object-Relational Mapping) 라이브러리와 AutoMapper는 서로 다른 목적과 기능을 가진 도구들입니다.

먼저 이들의 차이점을 간단히 설명한 후, AutoMapper에 대해 자세히 알아보겠습니다.

# ORM과 AutoMapper

## ORM (Object-Relational Mapping) 라이브러리

ORM은 <span class="font_highlight">**데이터베이스**와 **객체 지향 프로그래밍** 언어 간의 상호 변환을 자동화하는 도구</span>입니다.

주요 기능은 다음과 같습니다.

- **데이터베이스와의 상호작용을 추상화:** ORM을 사용하면 SQL 쿼리를 직접 작성할 필요 없이 객체 지향적인 방식으로 데이터베이스와 상호작용할 수 있습니다.
- **객체와 데이터베이스 테이블 간의 매핑:** 객체 모델을 데이터베이스 테이블에 매핑하여 데이터베이스의 레코드를 객체로 변환하고, 객체의 속성을 데이터베이스 필드로 변환합니다.
- **데이터 조작:** CRUD (Create, Read, Update, Delete) 작업을 객체 지향 방식으로 수행할 수 있습니다.

대표적인 ORM 라이브러리로는 Entity Framework (C#), Hibernate (Java), Django ORM (Python) 등이 있습니다.

## AutoMapper

AutoMapper는 <span class="font_highlight">**객체 간의 매핑**을 자동화하는 도구</span>입니다.

주요 기능은 다음과 같습니다.

- **객체 간 매핑 자동화:** 서로 다른 타입의 객체 간에 속성을 복사하거나 매핑할 때 수동으로 코드를 작성할 필요 없이 자동으로 매핑을 수행합니다.
- **중복 코드 감소:** 수동으로 매핑 코드를 작성하는 과정에서 발생하는 중복 코드를 줄여줍니다.
- **유지보수 용이성:** 매핑 규칙을 정의하는 방식으로 매핑을 관리할 수 있어 유지보수가 쉽습니다.

# AutoMapper란?

AutoMapper는 <span class="font_highlight">**객체 간의 매핑**을 자동화하는 도구</span>입니다.

주로 **DTO (Data Transfer Object)**와 **도메인 모델** 간의 매핑을 쉽게 해주기 위해 사용됩니다.

## 주요 기능

1. **자동 매핑:** 속성 이름이 같은 경우 자동으로 매핑됩니다.
2. **커스텀 매핑:** 속성 이름이 다르거나 복잡한 변환이 필요한 경우 커스텀 매핑 규칙을 정의할 수 있습니다.
3. **유형 변환**: 속성 간의 타입 변환도 지원합니다.
4. **컬렉션 매핑**: 리스트와 같은 컬렉션 타입도 매핑할 수 있습니다.
5. **컨디셔널 매핑**: 조건부 매핑을 설정할 수 있습니다.
6. **Null 값 처리**: Null 값을 처리하는 다양한 옵션을 제공합니다.

## 설치

AutoMapper는 NuGet 패키지로 제공되므로, 다음 명령어로 설치할 수 있습니다.

```bash
Install-Package AutoMapper
```

# 사용 방법 1: `MapperConfiguration`과 `AddProfile`

이 방법은 여러 매핑 설정을 중앙 집중화하고 구조화하는 데 유용합니다.

특히, 대규모 프로젝트에서 각 도메인에 맞는 매핑 설정을 따로 관리할 수 있어 유지보수가 용이합니다.

## **1. 프로필 클래스 생성**

매핑 구성을 정의하는 `Profile` 클래스를 생성합니다.

이 클래스의 생성자에서 `CreateMap` 메서드를 사용하여 소스와 대상 객체 간의 매핑을 정의합니다.

```csharp
public class Source1
{
    public string Value1 { get; set; }
}

public class Destination1
{
    public string Value1 { get; set; }
}

public class Source2
{
    public string Value2 { get; set; }
}

public class Destination2
{
    public string Value2 { get; set; }
}

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source1, Destination1>();
        CreateMap<Source2, Destination2>();
    }
}
```

## **2. MapperConfiguration과 IMapper 초기화**

프로그램 시작 시 `MapperConfiguration`을 사용하여 매핑 구성을 초기화합니다.`AddProfile` 메서드를 통해 정의한 `Profile` 클래스를 추가합니다.

```csharp
var configuration = new MapperConfiguration(cfg => {
    cfg.AddProfile<MappingProfile>();
});

IMapper mapper = configuration.CreateMapper();
```

## **3. 매핑 수행**

매퍼를 사용하여 소스 객체를 대상 객체로 변환합니다.

```csharp
var source1 = new Source1 { Value1 = "Hello" };
var destination1 = mapper.Map<Destination1>(source1);
Console.WriteLine(destination1.Value1); // "Hello"

var source2 = new Source2 { Value2 = "World" };
var destination2 = mapper.Map<Destination2>(source2);
Console.WriteLine(destination2.Value2); // "World"
```

## 장단점

### 장점

1. **구조화된 매핑 설정**: 매핑 설정을 별도의 프로필 클래스로 분리하여 코드 구조를 깔끔하게 유지할 수 있습니다.
2. **재사용성**: 여러 매핑 구성을 모듈화하여 재사용할 수 있습니다.
3. **유지보수 용이성**: 매핑 설정이 분리되어 있어 유지보수가 쉽습니다.
4. **확장성**: 대규모 프로젝트에서도 쉽게 확장할 수 있습니다.

### 단점

1. **복잡성 증가**: 작은 프로젝트나 단순한 매핑 작업에서는 다소 복잡할 수 있습니다.
2. **초기 설정 필요**: 매핑을 사용하기 전에 초기 설정을 해야 합니다.

# 사용 방법 2: `MapperConfiguration`에서 `CreateMap`을 직접 사용하는 방법

이 방법은 `MapperConfiguration` 객체를 사용하여 매핑 구성을 설정하되, `Profile` 클래스를 사용하지 않고 직접 `CreateMap` 메서드를 사용하여 매핑을 정의하는 방식입니다.

주로 작은 프로젝트나 단순한 매핑 작업에 적합합니다.

## **1. MapperConfiguration과 IMapper 초기화**

`CreateMap` 메서드를 사용하여 직접 매핑 구성을 정의합니다.

```csharp
var configuration = new MapperConfiguration(cfg => {
    cfg.CreateMap<Source, Destination>();
    cfg.CreateMap<AnotherSource, AnotherDestination>(); // 여러 매핑 설정
});

IMapper mapper = configuration.CreateMapper();

```

## **2. 매핑 수행**

매퍼를 사용하여 소스 객체를 대상 객체로 변환합니다.

```csharp
// 첫 번째 매핑 수행
var source = new Source { Value = "Hello" };
var destination = mapper.Map<Destination>(source);
Console.WriteLine(destination.Value); // "Hello"

// 두 번째 매핑 수행
var anotherSource = new AnotherSource { Value = "World" };
var anotherDestination = mapper.Map<AnotherDestination>(anotherSource);
Console.WriteLine(anotherDestination.Value); // "World"
```

## 장단점

### 장점

1. **간단한 설정**: 작은 프로젝트나 단순한 매핑 작업에서는 빠르고 간단하게 설정할 수 있습니다.
2. **적은 코드량**: 매핑 설정이 비교적 적은 경우에는 코드량이 적고 간단합니다.
3. **유연성**: `CreateMap` 메서드를 통해 필요한 만큼 매핑 설정을 추가할 수 있습니다.

### 단점

1. **구조화 부족**: 매핑 설정이 중앙 집중화되지 않아서 대규모 프로젝트에서는 관리가 어려울 수 있습니다.
2. **확장성 제한**: 매핑 설정이 증가할수록 코드가 복잡해지고 유지보수가 어려워질 수 있습니다.

# 사용 방법 3: 동적 및 확장 객체 매핑

동적 객체 매핑은 명시적인 매핑 설정 없이도 객체 간 변환을 수행하는 방법입니다.

이 기능은 다양한 상황에서 유용하게 사용할 수 있으며, 주로 작은 프로젝트나 빠르게 변환 작업을 수행해야 하는 경우에 적합합니다.

단지, 다음과 같은 특징이 존재합니다.

1. **기본 구성**
    - 이 방식은 AutoMapper의 기본 설정만을 사용하겠다는 의미입니다. 별도의 사용자 정의 매핑 규칙을 정의하지 않습니다.
2. **규약 기반 매핑**
    - AutoMapper는 기본적으로 "규약 기반 매핑"을 지원합니다. 이는 소스와 대상 타입의 속성 이름이 일치할 경우 자동으로 매핑을 수행한다는 의미입니다.
3. **동적 매핑**
    - 이후에 `CreateMap` 메서드를 사용하여 동적으로 매핑을 추가할 수 있습니다.
4. **최소 구성**
    - 복잡한 매핑 규칙이 필요 없는 간단한 프로젝트에서 사용될 수 있습니다.

```csharp
public class Foo {
    public int Bar { get; set; }
    public int Baz { get; set; }
    public Foo InnerFoo { get; set; }
}
dynamic foo = new MyDynamicObject();
foo.Bar = 5;
foo.Baz = 6;

var configuration = new MapperConfiguration(cfg => {});

var result = mapper.Map<Foo>(foo);
result.Bar.ShouldEqual(5);
result.Baz.ShouldEqual(6);

dynamic foo2 = mapper.Map<MyDynamicObject>(result);
foo2.Bar.ShouldEqual(5);
foo2.Baz.ShouldEqual(6);
```

위 코드는 [공식 사이트](https://docs.automapper.org/en/stable/Dynamic-and-ExpandoObject-Mapping.html){: target="_blank"}의 코드로, 런타임에 속성이 동적으로 추가된 `foo` 객체를 별도의 매핑 구성 없이 정적 객체인 `Foo` 타입으로 매핑합니다.

이때, AutoMapper는 `Bar`와 `Baz` 속성의 이름이 일치하므로 값을 올바르게 매핑합니다.

그 반대로, `result` 객체(`Foo` 타입)를 `MyDynamicObject` 타입으로 다시 매핑하는 모습도 볼 수 있습니다.

## 사전(Dictionary) 객체와 정적 객체 매핑

좀 특별하게 볼 부분은 사전(Dictionary) 객체와 정적 객체간의 매핑입니다.

```csharp
var result = mapper.Map<Foo>(new Dictionary<string, object> { ["InnerFoo.Bar"] = 42 });
result.InnerFoo.Bar.ShouldEqual(42);
```

예제에서는 사전(Dictionary) 객체를 생성하고, `Foo` 타입으로 매핑하는 것을 볼 수 있는데, 그 반대로 `Foo` 타입 객체에서 사전(Dictionary) 객체를 생성 할 수도 있습니다.

이는 나중에 다음과 같이 DTO에서 Dictionary 자료구조로 데이터를 가져온 후, 데이터베이스와 매핑 할 때에도 쓸 수 있습니다.

```csharp
var configuration = new MapperConfiguration(cfg => {});

Dictionary<string, object> dc = mapper.Map<DTO, Dictionary<string, object>>(dto);
// 프로시저 호출 시 파라미터 자동 매핑 할 때에도 쓰임
```

---

# 참고

[AutoMapper -- AutoMapper documentation](https://docs.automapper.org/en/stable/index.html){: }

[AutoMapper / 2. 다양한 사용 방법](/posts/automapper-2.-%EB%8B%A4%EC%96%91%ED%95%9C-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95/){: }