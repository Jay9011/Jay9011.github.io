---
title: Query String과 Action, 모델 바인딩
date: 2024-07-07 22:41:10 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - Controller
  - Model
  - URL
  - 데이터
math: false
type: aspnet
keywords:
  - Action
  - Query String
  - 모델 바인딩
---

# Query String

Query String은 URL의 일부분으로 클라이언트와 서버 간에 <span class="font_highlight">데이터를 전달하는 매우 간단한 방법</span>(문자열)입니다.

```
http://example.com/page?name=John&age=30
```

Query String은 URL의 끝에 추가되며 `?` 문자로 시작하고, `key=value` 형태의 쌍으로 구성됩니다. 또한, 여러 개의 파라미터를 추가하려면 `&` 문자로 구분하여 추가할 수 있습니다.

### Query String의 주요 특징

1. **간단한 데이터 전달 방법**: Query String은 매우 간단한 방식으로 데이터를 전달할 수 있습니다.
2. **데이터 <span class="important">노출</span>**: 모든 Query String 데이터는 URL에 노출되므로 보안에 민감한 데이터는 Query String을 통해 전달하지 않는 것이 좋습니다.
3. **데이터 <span class="important">길이 제한</span>**: 대부분의 브라우저는 URL 길이에 제한이 있기 때문에 전달할 수 있는 데이터 양이 제한적입니다.

# Action에서 Query String 데이터를 받는 방법

## 매개변수에서 직접 받는 방법

이 방법은 컨트롤러 메서드의 매개변수로 Query String 파라미터를 직접 선언하여 사용하는 방식입니다. 

이 경우 매개변수 이름과 Query String의 키 값이 일치하거나, `FromQuery` 어트리뷰트를 사용하여 명시적으로 매핑할 수 있습니다.

```csharp
public IActionResult GetUser(string name, int age)
{
    // name과 age는 Query String에서 전달된 값
    return Ok(new { Name = name, Age = age });
}
```

```csharp
public IActionResult GetUser([FromQuery(Name = "userName")] string name, int age)
{
    // name은 'userName' 키로 전달된 값
    return Ok(new { Name = name, Age = age });
}
```

## Request.Query 또는 Request.QueryString을 사용하는 방법

`Request.QueryString` 또는 `Request.Query`는 ASP.NET에서 Query String 파라미터에 접근하는 방법 중 하나입니다.

`Request.QueryString` 은 ASP.NET에서 사용되는,  `System.Collections.Specialized.NameValueCollection` 타입의 컬렉션이고,

`Request.Query` 는 ASP.NET Core에서 사용되는, `IQueryCollection` 타입의 컬렉션입니다.

둘 다 키-값 쌍으로 데이터를 관리하고, [”키”]로 접근할 수 있습니다.

```csharp
string name = Request.QueryString["name"];
string age = Request.QueryString["age"];
```

```csharp
string name = Request.Query["name"];
string age = Request.Query["age"];
```

## 일반 객체를 사용하는 방법 (모델 바인딩)

**모델 바인딩(Model Binding)**이란, 전송된 데이터를 C#의 모델 클래스에 담아서 액션 메서드에 전달하는 기법을 말합니다.

클래스객체를 사용하여 Query String 파라미터를 매핑하려면, 해당 객체의 속성들이 Query String의 키 값과 일치해야 합니다.

### 모델 클래스 정의

```csharp
public class UserQuery
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string City { get; set; }
}
```

### 컨트롤러 메서드에서 FromQuery 사용

```csharp
public IActionResult GetUser([FromQuery] UserQuery query)
{
    string name = query.Name;
    int age = query.Age;
    // query 객체를 사용한 로직 구현
    return Ok();
}
```

`FromQuery` 어트리뷰트는 ASP.NET Core에서 모델 바인딩을 사용하여 Query String 파라미터를 특정 타입의 객체로 변환할 때 사용됩니다.

이는 Query String의 각 파라미터를 모델의 속성에 매핑합니다.

이때, Query String에 일부 요소만 전달되는 경우, `FromQuery` 어트리뷰트는 모델 클래스에서 해당 요소에만 값을 할당하고, 나머지 요소들은 기본값을 유지합니다.

기본값은 보통 `null`(참조 타입) 또는 `0`(값 타입)입니다.

## 배열을 사용하는 방법

Query String에서 배열을 사용하는 경우, 동일한 키를 여러 번 사용하거나 인덱스 형식을 사용할 수 있습니다.

```csharp
public IActionResult GetUser([FromQuery] string[] names)
{
    // names 배열은 Query String에서 전달된 모든 'names' 값들을 포함
    return Ok(names);
}
```

```
http://example.com/user?names=John&names=Jane&names=Smith
```

이 경우 `names` 매개변수는 `["John", "Jane", "Smith"]` 배열을 갖게 됩니다.