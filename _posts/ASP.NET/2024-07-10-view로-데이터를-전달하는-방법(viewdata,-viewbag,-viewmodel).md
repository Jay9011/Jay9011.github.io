---
title: View로 데이터를 전달하는 방법(ViewData, ViewBag, ViewModel)
date: 2024-07-10 21:09:25 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - View
  - Controller
math: false
type: aspnet
keywords:
  - View
  - ViewBag
  - ViewData
  - ViewModel
---

ASP.NET에서 Controller에서 View로 데이터를 전달하는 방법은 여러 가지가 있습니다.

대표적으로 `ViewData`, `ViewBag`, `ViewModel`을 사용하여 데이터를 전달할 수 있습니다.

# ViewData

**ViewData**는 Controller와 View 간에 **키-값 쌍**으로 데이터를 저장하고 전달하는 데 사용됩니다.

`ViewData`는 `ViewDataDictionary` 타입의 객체입니다.

## 특징

- 타입 캐스팅이 필요합니다.
- 유연성이 높습니다.
- 문자열 키를 사용하여 데이터를 저장하고 접근합니다.

## 사용법

**Controller**에서 ViewData 사용

```csharp
public ActionResult Index()
{
    ViewData["Message"] = "Hello, World!";
    return View();
}
```

**View**에서 ViewData 사용

```csharp
@{
    string message = ViewData["Message"] as string;
}

<p>@message</p>
```

# ViewBag

**ViewBag**은 **동적 속성(dynamic property)**으로 데이터를 저장하고 전달하는 방법입니다.

`ViewBag`은 내부적으로 `ViewData`를 사용하여 데이터를 저장합니다. 따라서 `ViewBag`은 `ViewData`의 간편한 버전이라고 할 수 있습니다.

## 특징

- 타입 캐스팅이 필요 없습니다.
- `dynamic` 타입을 사용하여 속성처럼 데이터를 저장하고 접근합니다.
- 코드가 더 간결해집니다.

## 사용법

**Controller**에서 ViewBag 사용

```csharp
public ActionResult Index()
{
    ViewBag.Message = "Hello, World!";
    return View();
}
```

**View**에서 ViewBag 사용

```csharp
<p>@ViewBag.Message</p>
```

# ViewModel

**ViewModel**은 View에 필요한 데이터를 담는 **클래스**입니다.

`ViewModel`을 사용하면 View에 전달할 데이터 구조를 명확하게 정의할 수 있습니다. `ViewModel`은 강력한 형식의 데이터를 제공하며, 복잡한 데이터 구조를 전달할 때 유용합니다.

## 특징

- 강력한 타입을 사용합니다.
- 모델의 프로퍼티를 통해 데이터를 전달합니다.
- 뷰에 필요한 모든 데이터를 하나의 객체로 묶어서 전달할 수 있습니다.

## 사용법

먼저 ViewModel 클래스를 정의합니다.

```csharp
public class MyViewModel
{
    public string Message { get; set; }
    public int Number { get; set; }
}
```

**Controller**에서 ViewModel 사용.

```csharp
public ActionResult Index()
{
    var model = new MyViewModel
    {
        Message = "Hello, World!",
        Number = 42
    };
    return View(model);
}
```

**View**에서 ViewModel 사용

```csharp
@model MyViewModel

<p>@Model.Message</p>
<p>@Model.Number</p>
```