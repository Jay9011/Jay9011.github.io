---
title: 정책(Policy)과 클레임(Claim)
date: 2024-09-08 20:05:26 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - 객체 지향
  - 권한
  - 데이터
  - 인증
math: false
fmContentType: aspnet
keywords:
  - Claim
  - Policy
  - 정책
  - 클레임
---

# 1. 정책과 클레임이란?

## 클레임(Claim)

클레임은 <span class="font_highlight">사용자에 대한 정보를 담고 있는 조각이자 객체</span>입니다. 예를 들어, 사용자의 이름, 이메일, 역할(role) 등이 클레임으로 표현될 수 있습니다.

클레임은 주로 **JWT(Json Web Token)** 또는 **쿠키**와 같은 인증 토큰에 포함됩니다.

## 정책(Policy)

정책은 <span class="font_highlight">특정 액션을 수행할 수 있는지 여부를 결정하는 규칙</span>입니다.

예를 들어, "관리자만 접근 가능"과 같은 정책을 정의할 수 있습니다.

정책은 주로 클레임을 평가하여 사용자가 특정 리소스에 접근할 수 있는지 판단합니다.

> **액션(Action)**<br>
> 액션은 사용자가 웹 애플리케이션에서 수행할 수 있는 특정 작업을 의미합니다.<br>
> 예를 들어, 페이지 보기, 데이터 수정, 파일 업로드 등이 액션에 해당합니다.<br>
> 정책은 이러한 액션에 대한 접근 권한을 제어하는 데 사용됩니다.<br>
> 
{: .prompt-info}

# 2. ASP.NET Core에서 클레임과 정책 설정하기

## 클레임 추가하기

ASP.NET Core에서 클레임을 추가하는 방법은 여러 가지가 있지만, 가장 일반적인 방법은 사용자가 로그인할 때 클레임을 추가하는 것입니다.

```csharp
public async Task<IActionResult> Login(string username, string password)
{
    // 사용자 인증 로직 (예: 데이터베이스 조회)
    var user = AuthenticateUser(username, password);
    if (user == null)
    {
        return Unauthorized();
    }

    var claims = new List<Claim>
    {
        new Claim(ClaimTypes.Name, user.Username),
        new Claim(ClaimTypes.Email, user.Email),
        new Claim(ClaimTypes.Role, "Administrator")
    };

    var claimsIdentity = new ClaimsIdentity(claims, "login");

    await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, new ClaimsPrincipal(claimsIdentity));

    return RedirectToAction("Index", "Home");
}
```

## 정책 설정하기

정책을 설정하려면 `Startup.cs` 또는 `Program.cs` 파일에서 정책을 정의하고 등록해야 합니다.

다음은 "관리자만 접근 가능" 정책을 설정하는 예제입니다.

### Startup.cs

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
        .AddCookie();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("AdminOnly", policy => policy.RequireClaim(ClaimTypes.Role, "Administrator"));
        options.AddPolicy("UserOnly", policy => policy.RequireClaim(ClaimTypes.Role, "User"));
        options.AddPolicy("GuestOnly", policy => policy.RequireClaim(ClaimTypes.Role, "Guest"));
    });

    services.AddControllersWithViews();
}
```

### Program.cs

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie();

builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly", policy => policy.RequireClaim(ClaimTypes.Role, "Administrator"));
    options.AddPolicy("UserOnly", policy => policy.RequireClaim(ClaimTypes.Role, "User"));
    options.AddPolicy("GuestOnly", policy => policy.RequireClaim(ClaimTypes.Role, "Guest"));
});

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();

app.Run();
```

## 정책 적용하기

정책을 적용하려면 컨트롤러나 액션 메서드에 `[Authorize]` 어트리뷰트를 사용하면 됩니다. 다음은 "AdminOnly" 정책을 적용하는 예제입니다.

```csharp
[Authorize(Policy = "AdminOnly")]
public IActionResult AdminDashboard()
{
    return View();
}
```

# 3. 클레임이란?

클레임은 <span class="font_highlight">**사용자**의 **속성** 또는 **특징**을 나타내는 **정보** 조각</span>으로, <span class="font_highlight">개발자가 쉽게 사용할 수 있도록 **미리 정의된 객체**</span>입니다.

(물론, 개념적으로 미리 정의된 객체일 뿐, 개발자가 필요에 따라 커스텀 클레임을 정의할 수 있습니다.)<br>
클레임은 주로 인증과 권한 부여 과정에서 사용되며, 사용자의 신원을 확인하고 권한을 부여하는 데 중요한 역할을 합니다.

커스텀 세션 정보(사용자 정보)를 기반으로 정책을 적용하거나 커스텀 정책 핸들러를 통해 정책을 적용하는 방법도 있을 수 있는데, 주로 ASP.NET에서 기본으로 정의해주는 `Claim`을 사용하는 편입니다.

즉, 정책을 지정할 때 클레임을 사용하는 것은 인증과 권한 부여를 쉽게 구현하기 위해 사용하는 방법 중 하나인 것입니다.

## 클레임의 구성 요소

클레임은 주로 다음과 같은 세 가지 구성 요소로 이루어집니다.

1. **클레임 유형(Claim Type)**: 클레임의 종류 혹은 키(key)를 나타내는 문자열입니다. 예를 들어, `ClaimTypes.Name`, `ClaimTypes.Email`, `ClaimTypes.Role` 등이 있습니다.
2. **클레임 값(Claim Value)**: 클레임의 실제 값(value)입니다. 예를 들어, 이름 클레임의 값은 "John Doe", 역할 클레임의 값은 "Administrator"일 수 있습니다.
3. **발행자(Issuer)**: 클레임을 발행한 주체를 나타냅니다. 예를 들어, 인증 서버가 될 수 있습니다.

## 클레임의 저장과 사용

클레임은 사용자에 대한 정보 조각으로, 이를 효과적으로 사용하기 위해, 클레임을 여러 방식으로 다룰 수 있습니다.

1. **인증 토큰 방식 (예: JWT)**
    - 사용자 정보(클레임)를 작은 암호화된 파일에 담아 브라우저에 저장합니다.
    - 비유: 휴대용 신분증
    - 사용: 웹사이트에 접속할 때마다 이 '신분증'을 제시합니다.
2. **쿠키(Cookie) 방식**
    - 사용자 정보를 암호화하여 브라우저의 쿠키에 저장합니다.
    - 비유: 놀이공원 입장 팔찌
    - 사용: 웹사이트 방문 시 자동으로 이 '팔찌'를 확인합니다.
3. **세션(Session) 방식**
    - 사용자 정보를 서버에 저장하고, 사용자에게는 고유 번호(세션 ID)만 줍니다.
    - 비유: 호텔 키카드 시스템
    - 사용: 사용자가 이 번호로 접근할 때마다 서버에서 정보를 확인합니다.

## 클레임의 활용

클레임은 주로 다음과 같은 목적으로 활용됩니다.

- **인증(Authentication)**: 사용자의 신원을 확인하는 데 사용됩니다. 예를 들어, 사용자가 로그인할 때 클레임을 통해 사용자의 정보를 확인합니다.
- **권한 부여(Authorization)**: 사용자가 특정 리소스에 접근할 수 있는지 여부를 결정하는 데 사용됩니다. 예를 들어, 관리자만 접근할 수 있는 페이지에 접근하려면 "Administrator" 역할 클레임이 필요합니다.
- **사용자 프로필(User Profile)**: 사용자의 프로필 정보를 저장하고 관리하는 데 사용됩니다. 예를 들어, 사용자의 이름, 이메일, 전화번호 등을 클레임으로 저장할 수 있습니다.

## 클레임의 관리

클레임은 <span class="font_highlight">개발자가 쉽게 사용할 수 있도록 **미리 정의된 객체**</span>입니다.

ASP.NET Core에서는 **`Claim`** 클래스를 사용하여 클레임을 관리할 수 있습니다. 클레임은 `ClaimsIdentity`와 `ClaimsPrincipal` 객체를 통해 사용자와 연결됩니다.

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, "John Doe"),
    new Claim(ClaimTypes.Email, "john.doe@example.com"),
    new Claim(ClaimTypes.Role, "Administrator")
};

var claimsIdentity = new ClaimsIdentity(claims, "login");
var claimsPrincipal = new ClaimsPrincipal(claimsIdentity);

// 클레임을 사용하여 사용자 인증
await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, claimsPrincipal);
```

위 예제에서 `var claims = new List<Claim> { ... };` 부분은 사용자의 정보를 나타내는 `Claim` 객체들의 리스트를 생성하는 부분입니다.

각각 Name, Email, Role을 생성하며, 각 값을 집어 넣었습니다.

`var claimsIdentity = new ClaimsIdentity(claims, "login");` 부분은 여러 개의 `Claim` 객체를 포함하는 사용자의 신원 및 인증 객체입니다.

`ClaimsPrincipal`은 현재 사용자를 나타내는 객체입니다. 위에서 설정한 하나 이상의 `ClaimsIdentity` 객체를 포함할 수 있습니다.

이 구조는 사용자가 여러 방식으로 인증될 수 있는 상황(예: 회사 계정과 소셜 미디어 계정을 모두 사용)을 지원하지만, 대부분의 어플리케이션에서는 하나의 `ClaimsPrincipal`이 하나의 `ClaimsIdentity`를 가지는게 일반적입니다.

`await HttpContext.SignInAsync(CookieAuthenticationDefaults.AuthenticationScheme, claimsPrincipal);` 부분은 전달된 `claimsPrincipal` 객체의 <span class="font_highlight">정보(**claims**)를 쿠키에 포함</span>시키고, 쿠키와 세션에 이 정보를 저장합니다.

# 4. 정책이란?

정책(Policy)은 <span class="font_highlight">특정 액션을 수행할 수 있는지 여부를 결정하는 **규칙**</span>입니다.

정책은 웹 애플리케이션에서 사용자 접근 제어를 보다 유연하고 강력하게 관리할 수 있도록 도와줍니다.

- **유연성**: 다양한 조건을 기반으로 접근 제어를 설정할 수 있습니다.
- **확장성**: 새로운 요구사항이 생길 때마다 정책을 추가하거나 수정할 수 있습니다.
- **보안성**: 세밀한 접근 제어를 통해 보안을 강화할 수 있습니다.

## 정책 추가와 평가

1. **정책명**: 정책을 추가할 때, 먼저 정책명을 지정합니다. 이 정책명은 나중에 `[Authorize]` 어트리뷰트에서 참조됩니다.
2. **정책 평가(람다 함수)**: 정책 평가를 위한 람다 함수는 정책이 만족해야 하는 조건을 정의합니다. 이 조건은 주로 클레임이나 역할을 기반으로 합니다.

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly", policy => 
        policy.RequireClaim(ClaimTypes.Role, "Administrator"));
    
    options.AddPolicy("UserOnly", policy => 
        policy.RequireClaim(ClaimTypes.Role, "User"));
    
    options.AddPolicy("AtLeast21", policy => 
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});
```

위 예제에서 “AdminOnly” 같은 문자열은 정책명을, 뒤의 람다식은 적책 평가로, ClaimTypes의 Role에서 일치 여부를 확인하고 있습니다.

## 정책 적용

1. **어트리뷰트에서 정책명 참조**: 특정 액션이나 컨트롤러에 `[Authorize]` 어트리뷰트를 사용하여 정책명을 참조합니다. 
이 어트리뷰트는 해당 액션이나 컨트롤러에 접근할 때 정책을 평가하도록 합니다.

```csharp
[Authorize(Policy = "AdminOnly")]
public IActionResult AdminDashboard()
{
    return View();
}
```

위 예제에서 `[Authorize]` 어트리뷰트는 위에서 추가한 정책의 람다식 중 정책명이 “AdminOnly”인 정책을 찾아 평가하게 됩니다.

과정으로 예를 들면

1. 사용자가 `AdminDashboard` 액션에 접근하려고 시도합니다.
2. `[Authorize(Policy = "AdminOnly")]` 어트리뷰트가 적용된 액션이므로, ASP.NET Core는 "AdminOnly" 정책을 찾습니다.
3. "AdminOnly" 정책은 ClaimTypes.Role 클레임이 "Administrator"인지 확인하는 조건(`policy.RequireClaim(ClaimTypes.Role, "Administrator")`)을 가지고 있습니다.
4. 사용자의 클레임을 평가하여 조건이 만족되면 접근을 허용하고, 그렇지 않으면 접근을 거부합니다.

# 마무리

ASP.NET Core의 클레임과 정책 시스템은 웹 애플리케이션의 인증(Authentication)과 권한 부여(Authorization)를 프로그래머가 쉽게 구현하고, 효과적으로 관리할 수 있게 해주는 강력한 도구입니다.

클레임은 사용자의 속성이나 특징을 나타내는 정보 조각으로, 사용자의 신원을 확인하고 권한을 부여하는 데 중요한 역할을 합니다.

정책은 이러한 클레임을 기반으로 사용자의 접근 권한을 결정하는 규칙을 정의합니다.

개발자는 이 시스템을 활용하여 복잡한 인증 및 권한 부여 요구사항을 효과적으로 구현할 수 있습니다. 그러나 시스템의 효과적인 사용을 위해서는 클레임과 정책의 개념을 정확히 이해하고, 애플리케이션의 요구사항에 맞게 적절히 설계하는 것이 중요합니다.