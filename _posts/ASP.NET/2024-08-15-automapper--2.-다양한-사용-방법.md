---
title: AutoMapper / 2. 다양한 사용 방법
date: 2024-08-15 10:08:47 +0900
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

AutoMapper는 단순히 DTO와 모델 간의 값 이동만을 하기 위해 사용하는것이 아닙니다.

기본적으로 DTO와 모델 간의 변환을 간편하게 해주지만, 매핑을 위해 조건을 건다거나, 원하는 형태로 변환해서 매핑을 하는 등, 다양한 기능을 지원해줍니다.

# 조건부 매핑

조건부 매핑은 매핑 작업을 수행할 때 특정 조건이 충족될 때만 매핑을 수행하도록 하는 방법입니다.

이를 통해 원하는 조건에 따라 매핑을 유연하게 처리할 수 있습니다.

조건부 매핑을 설정하려면 `ForMember` 메서드와 함께 `Condition` 메서드를 사용합니다. `Condition` 메서드에는 매핑 조건을 지정하는 람다 식을 전달합니다.

문서: [https://docs.automapper.org/en/stable/Conditional-mapping.html](https://docs.automapper.org/en/stable/Conditional-mapping.html){: target="_blank"}

### 예제

예를 들어, `Condition` 메서드를 사용하여 나이가 18세 이상인 경우에만 매핑되도록 할 수 있습니다.

먼저, `Source`와 `Destination` 모델 클래스를 정의합니다.

```csharp
public class Source
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}

public class Destination
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string AdultContent { get; set; }
}
```

조건부 매핑을 설정하는 프로필을 정의합니다. 여기서 `Age` 속성이 18세 이상인 경우에만 `AdultContent` 속성이 매핑되도록 설정합니다.

```csharp
using AutoMapper;

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source, Destination>()
            .ForMember(dest => dest.AdultContent, opt => opt.Condition(src => src.Age >= 18))
            .ForMember(dest => dest.AdultContent, opt => opt.MapFrom(src => "This is adult content"))
            .ForMember(dest => dest.Name, opt => opt.MapFrom(src => src.Name));
    }
}
```

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        // AutoMapper 설정
        var config = new MapperConfiguration(cfg => cfg.AddProfile<MappingProfile>());
        var mapper = config.CreateMapper();

        // 예제 데이터 생성
        var source1 = new Source { Id = 1, Name = "John", Age = 20 };
        var source2 = new Source { Id = 2, Name = "Doe", Age = 16 };

        // 매핑 수행
        var destination1 = mapper.Map<Destination>(source1);
        var destination2 = mapper.Map<Destination>(source2);

        // 결과 출력
        Console.WriteLine($"Destination1 - Id: {destination1.Id}, Name: {destination1.Name}, AdultContent: {destination1.AdultContent}");
        Console.WriteLine($"Destination2 - Id: {destination2.Id}, Name: {destination2.Name}, AdultContent: {destination2.AdultContent}");
    }
}
```

```
Destination1 - Id: 1, Name: John, AdultContent: This is adult content
Destination2 - Id: 2, Name: Doe, AdultContent: 
```

그럼, `source1` 객체는 `Age` 속성이 20이므로 `AdultContent` 속성이 `destination1` 객체에 매핑되지만, `source2` 객체는 `Age` 속성이 16이므로 `AdultContent` 속성이 매핑되지 않았습니다.

이와 같이 조건부 매핑을 사용하여 특정 조건(예: 나이가 18세 이상)에 따라 매핑을 제어할 수 있습니다.

# Custom Value Resolver 사용

Custom Value Resolvers는 AutoMapper에서 매핑할 때 특정 속성의 값을 변환하거나 계산하는 데 사용되는 사용자 정의 로직을 제공하는 기능입니다.

기본적인 매핑 규칙으로 해결할 수 없는 복잡한 매핑 요구 사항을 처리할 때 유용합니다.

문서: [https://docs.automapper.org/en/stable/Custom-value-resolvers.html](https://docs.automapper.org/en/stable/Custom-value-resolvers.html){: target="_blank"}

### 예제

휴대폰 번호의 짝수 번째 숫자를 '*'로 치환하여 매핑 해보도록 하겠습니다.

먼저, `Source`와 `Destination` 모델 클래스를 정의합니다.

```csharp
public class Source
{
    public int Id { get; set; }
    public string PhoneNumber { get; set; }
}

public class Destination
{
    public int Id { get; set; }
    public string MaskedPhoneNumber { get; set; }
}
```

휴대폰 번호의 짝수 번째 숫자를 '*'로 치환하는 Custom Value Resolver 클래스를 정의합니다.

```csharp
using AutoMapper;

public class PhoneNumberMaskResolver : IValueResolver<Source, Destination, string>
{
    public string Resolve(Source source, Destination destination, string destMember, ResolutionContext context)
    {
        if (string.IsNullOrEmpty(source.PhoneNumber))
            return source.PhoneNumber;

        char[] phoneChars = source.PhoneNumber.ToCharArray();
        for (int i = 1; i < phoneChars.Length; i += 2)
        {
            if (char.IsDigit(phoneChars[i]))
            {
                phoneChars[i] = '*';
            }
        }

        return new string(phoneChars);
    }
}
```

이제, Custom Value Resolver를 사용하는 매핑 규칙을 프로필에 정의하면,

```csharp
using AutoMapper;

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source, Destination>()
            .ForMember(dest => dest.MaskedPhoneNumber, opt => opt.MapFrom<PhoneNumberMaskResolver>());
    }
}
```

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        // AutoMapper 설정
        var config = new MapperConfiguration(cfg => cfg.AddProfile<MappingProfile>());
        var mapper = config.CreateMapper();

        // 예제 데이터 생성
        var source = new Source { Id = 1, PhoneNumber = "1234567890" };

        // 매핑 수행
        var destination = mapper.Map<Destination>(source);

        // 결과 출력
        Console.WriteLine($"Destination - Id: {destination.Id}, MaskedPhoneNumber: {destination.MaskedPhoneNumber}");
    }
}
```

```
Destination - Id: 1, MaskedPhoneNumber: 1*3*5*7*9*
```

위와 같이 `Source` 객체의 `PhoneNumber` 속성에서 짝수 번째 숫자를 '*'로 치환하여 `Destination` 객체의 `MaskedPhoneNumber` 속성에 매핑되었습니다.

이처럼 Custom Value Resolvers를 사용하면 복잡한 변환 로직을 별도의 클래스에 정의하여 매핑 과정에서 사용할 수 있습니다.

# NULL 처리

AutoMapper에서 `null` 값을 처리하는 방법은 여러 가지가 있습니다.

특정 속성이 `null`인 경우 기본값을 사용하거나, `null` 체크를 수행하고 적절한 처리를 할 수 있습니다.

## 1. NullSubstitute

문서: [https://docs.automapper.org/en/stable/Null-substitution.html](https://docs.automapper.org/en/stable/Null-substitution.html){: target="_blank"}

`NullSubstitute`를 사용하면 소스 속성이 `null`인 경우에 대체할 값을 지정할 수 있습니다.

```csharp
using AutoMapper;

public class Source
{
    public string Name { get; set; }
}

public class Destination
{
    public string Name { get; set; }
}

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source, Destination>()
            .ForMember(dest => dest.Name, opt => opt.NullSubstitute("Default Name"));
    }
}
```

위 코드에서 `Source.Name`이 `null`인 경우 `Destination.Name`에 `"Default Name"`이 매핑됩니다.

## 2. Condition, Resolver

그 외에도 위에서 언급한 조건부나 Resolver를 사용 할 수도 있습니다.

```csharp
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Source, Destination>()
            .ForMember(dest => dest.Name, opt => opt.Condition(src => src.Name != null));
    }
}
```

```csharp
public class NullHandlingResolver : IValueResolver<Source, Destination, string>
{
    public string Resolve(Source source, Destination destination, string destMember, ResolutionContext context)
    {
        return source.Name ?? "Default Name";
    }
}
```

이 외에도 상당히 많은 내용들이 공식 문서에 존재하니 한 번씩 찾아 보시는 것을 추천 드립니다.

---

# 참조

[AutoMapper ─ AutoMapper documentation](https://docs.automapper.org/en/stable/index.html){: }

[AutoMapper / 1. AutoMapper와 초기화](/posts/automapper-1.-automapper%EC%99%80-%EC%B4%88%EA%B8%B0%ED%99%94/){: }