---
title: Controller와 Action, 그리고 라우팅
date: 2024-07-06 17:32:21 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - Controller
  - URL
math: false
type: aspnet
keywords:
  - Action
  - Controller
  - 라우팅
---

# Controller

Controller는 사용자 입력을 처리하고 모델을 업데이트하거나 뷰를 반환하는 역할을 합니다.

ASP.NET MVC에서 컨트롤러는 일반적으로 “Controllers” 폴더 아래에 위치하며, `System.Web.Mvc.Controller` 클래스를 상속하여 구현한 **클래스**입니다.

일반적으로 "Controller"로 끝나는 이름을 가집니다.

# Action

Action은 Controller 클래스의 **메서드**로, <span class="font_highlight">HTTP 요청을 **처리**하고 적절한 응답을 **반환**하는 역할</span>을 합니다.

Action 메서드는 다양한 타입을 반환할 수 있지만, 주로 `ActionResult` 또는 그 파생 타입을 반환합니다. 예를 들어, `ViewResult`, `RedirectResult`, `JsonResult` 등이 있습니다.

각각의 Action 메서드는 특정 URL 패턴과 매핑됩니다.

# 라우팅

라우팅 시스템은 들어오는 URL 요청을 적절한 <span class="font_highlight">Controller와 Action으로 **매핑**</span>하는 역할을 합니다.

ASP.NET MVC의 기본 라우팅 규칙은 `Global.asax` 파일이나 `Startup.cs` 파일, `Program.cs` 파일을 사용하여 라우팅을 설정할 수 있습니다.

## program.cs

### WebApplication.CreateBuilder()

`WebApplication.CreateBuilder()`는 애플리케이션의 기본 빌더 객체를 생성합니다.

이 객체는 ASP.NET Core 애플리케이션의 기본 구성을 설정하는 데 사용되고, 이는 DI(Dependency Injection) 컨테이너, 로깅, 설정 파일 등을 포함합니다.

```csharp
var builder = WebApplication.CreateBuilder(args);
```

- **args**: 명령줄 인수를 받아 애플리케이션의 설정을 초기화하는 데 사용할 수 있습니다.
- **builder**: 웹 애플리케이션을 설정하는 데 필요한 서비스를 구성하는 데 사용되는 객체입니다.

### Build()

`Build()` 메서드는 Web Application Builder로부터 `WebApplication` 객체를 생성합니다.

이 객체는 애플리케이션의 실행 환경을 구성하고, 요청을 처리하는 파이프라인을 설정하는 데 사용됩니다.

```csharp
var app = builder.Build();
```

- **app**: ASP.NET Core 애플리케이션의 주 실행 객체로, 미들웨어 구성 및 요청 파이프라인 설정에 사용됩니다.

### MapGet()

`MapGet()` 메서드는 **특정 경로**에 대해 HTTP GET 요청을 처리하는 **엔드포인트**를 설정합니다.

주로 간단한 요청을 처리하거나 테스트 엔드포인트를 만들 때 사용됩니다.

```csharp
app.MapGet("/", () => "Hello World!");
```

- **"/"**: 요청 경로입니다.
- **() => "Hello World!"**: 요청을 처리하는 람다식으로, 문자열 응답을 반환합니다.

### Services.AddControllers()

`Services.AddControllers()`는 ASP.NET Core의 서비스 컨테이너에 MVC 컨트롤러 서비스를 추가합니다.

이는 컨트롤러를 사용한 API 및 MVC 애플리케이션을 구성하는 데 필요합니다.

```csharp
builder.Services.AddControllers();
```

- **builder.Services**: DI(Dependency Injection) 컨테이너를 통해 서비스 등록을 관리하는 객체입니다.
- **AddControllers()**: 컨트롤러를 위한 서비스(예: `IControllerActivator`, `IViewComponentActivator`)를 등록합니다.

### Services.AddControllersWithViews()

`Services.AddControllersWithViews()`는 ASP.NET Core의 서비스 컨테이너에 MVC 컨트롤러 및 뷰 관련 서비스를 추가합니다.

```csharp
builder.Services.AddControllersWithViews();
```

- **builder.Services**: DI(Dependency Injection) 컨테이너를 통해 서비스 등록을 관리하는 객체입니다.
- **AddControllersWithViews()**: 컨트롤러와 뷰를 위한 서비스(예: `IControllerActivator`, `IViewComponentActivator`, `IRazorViewEngine`)를 등록합니다.

### MapControllers()

`MapControllers()` 메서드는 **컨트롤러 기반의 엔드포인트**를 설정합니다.

이는 애플리케이션의 모든 컨트롤러를 라우팅 테이블에 등록합니다.

```csharp
app.MapControllers();
```

### MapControllerRoute()

`MapControllerRoute()` 메서드는 **MVC 패턴에서 사용되는 경로를 설정**합니다.

이를 통해 특정 패턴의 URL이 특정 컨트롤러와 액션으로 매핑되도록 설정할 수 있습니다.

```csharp
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

- **name**: 라우트의 이름입니다.
- **pattern**: URL 경로 패턴입니다. `{controller=Home}`은 기본 컨트롤러가 `Home`임을 의미하고, `{action=Index}`는 기본 액션이 `Index`임을 의미합니다. `{id?}`는 선택적인 `id` 매개변수를 나타냅니다.
    - 위와 같은 경우, 기본 경로가 “/Home/Index/{id?}”와 매핑됩니다.