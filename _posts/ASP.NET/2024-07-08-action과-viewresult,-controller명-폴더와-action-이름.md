---
title: Action과 ViewResult, Controller명 폴더와 Action 이름
date: 2024-07-08 23:20:34 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - Controller
  - View
math: false
type: aspnet
keywords:
  - Action
  - ViewResult
  - Controller
---

# ViewResult

`ViewResult` 는 `IActionResult` 를 상속 받은, <span class="font_highlight">특정 **뷰를 렌더링**하고 이를 클라이언트에 반환</span>하는 역할을 하는 `ActionResult`의 한 유형입니다.

일반적으로 컨트롤러의 액션 메소드에서 `return View()`를 호출하면 `ViewResult` 객체가 반환됩니다.

```csharp
public ViewResult MyAction()
{
    return View(); // ViewResult를 반환
}
```

# IActionResult

`IActionResult`는 ASP.NET Core MVC에서 <span class="font_highlight">액션 메소드가 반환할 수 있는 **다양한 결과** 유형</span>을 나타내는 <span class="important">**인터페이스**</span>입니다.

`ViewResult`, `JsonResult`, `RedirectResult`, `ContentResult` 등 여러 유형의 결과를 포함할 수 있습니다.

```csharp
public IActionResult MyAction()
{
    if (someCondition)
    {
        return View(); // ViewResult 반환
    }
    else
    {
        return Json(new { Message = "Hello, world!" }); // JsonResult 반환
    }
}
```

# 특정 빈 View()와 특정 주소를 반환하는 View

`return View()`는 기본적으로 호출된 액션 메소드와 동일한 이름을 가진 뷰를 찾습니다.

예를 들어, `HomeController`의 `Index` 액션 메소드에서 `return View()`를 호출하면 `Views/Home/Index.cshtml` 파일이 반환됩니다.

이때, `View()` 의 매개변수에 `~`로 시작하는 경로를 입력할 수 있는데, `~`는 가상 경로를 나타내는 ASP.NET의 특수 문자입니다.

즉, 애플리케이션의 루트 디렉토리를 기준으로 경로를 지정할 수 있게 해주는 역할을 합니다.

예를 들어, `~/Views/Home/Index.cshtml`는 애플리케이션 루트에서 시작해서 "Views 폴더 / Home 폴더 / Index.cshtml"을 찾아 반환해줍니다.

```csharp
public IActionResult Index()
{
    return View("~/Views/Home/Index.cshtml");
}
```

# Controller와 자동 매핑 되는 Action명 파일

ASP.NET MVC에서는 컨벤션 기반의 폴더 구조를 따릅니다.

이는 개발자가 명시적으로 경로를 지정하지 않아도 특정 규칙을 따라 파일을 배치하면 프레임워크가 자동으로 이를 찾을 수 있도록 합니다.

따라서, 아래 구조를 통해 ASP.NET MVC가 적절한 뷰 파일을 자동으로 찾아 렌더링 할 수 있습니다.

- **Views 폴더**: 이 폴더는 모든 뷰 파일을 포함합니다.
- **Controller명 폴더**: 각 컨트롤러에 대한 뷰 파일은 `Views` 폴더 내에 해당 컨트롤러 이름의 하위 폴더에 배치됩니다. 예를 들어, `HomeController`의 뷰 파일은 `Views/Home` 폴더에 위치합니다.
- **Action명 파일**: 각 액션 메소드에 대한 뷰 파일은 해당 컨트롤러 폴더 내에서 액션 메소드 이름과 동일한 이름을 가진 파일로 배치됩니다. 예를 들어, `Index` 액션 메소드는 `Views/Home/Index.cshtml` 파일을 가리킵니다.