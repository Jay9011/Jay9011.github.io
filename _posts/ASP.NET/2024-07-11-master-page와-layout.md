---
title: Master Page와 Layout
date: 2024-07-11 23:07:17 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - View
math: false
type: aspnet
keywords:
  - Master Page
  - Layout
---

ASP.NET의 Master Page와 Layout은 웹 애플리케이션 개발에서 <span class="font_highlight">**일관된 레이아웃**과 **디자인**을 유지하는 데 사용되는 기능</span>입니다.

두 개념 모두 템플릿 기반의 페이지 디자인을 가능하게 하여, 여러 페이지에 걸쳐 공통된 레이아웃을 쉽게 적용할 수 있습니다.

# Shared 폴더

Shared 폴더는 여러 뷰(View)와 레이아웃(Layout) 파일을 공유하기 위해 사용됩니다.

Shared 폴더 내에는 유지보수성을 높이는 데 유용한, 재사용 가능한 뷰로 구성됩니다.

## 공통 뷰 (Views)

Shared 폴더에는 **여러 컨트롤러에서 공통으로 사용될 수 있는 뷰** 파일이 포함됩니다.

예를 들어, 오류 페이지, 로그인 페이지, 부분 뷰(Partial View) 등이 이 폴더에 위치할 수 있습니다.

## 레이아웃 (Layouts)

레이아웃 파일은 전체 애플리케이션에서 **공통으로 사용되는 기본 레이아웃**을 정의합니다. 이러한 레이아웃 파일은 모든 뷰에서 참조할 수 있습니다.

## 부분 뷰 (Partial Views)

부분 뷰는 웹 페이지의 일부분을 구성하는 재사용 가능한 뷰입니다. 이러한 뷰는 종종 Shared 폴더에 위치하여 여러 뷰에서 참조됩니다.

예를 들어, 헤더 부분이나 푸터 부분 등이 이 폴더에 위치할 수 있습니다.

# 레이아웃(Layout)

마스터 페이지란, **여러 페이지에 걸쳐 일관된 레이아웃을 제공하기 위해 사용되는 페이지**를 말합니다.

ASP.NET MVC 또는 Razor Pages에서는 **레이아웃**이라고 하며, 여러 뷰(View)에서 공통으로 사용되어 일관된 외관을 위해 사용됩니다.

## RenderBody

기존 View 페이지와는 달리, 레이아웃 파일에서는 `RenderBody` 메서드를 사용하여 뷰의 콘텐츠를 삽입할 수 있습니다.

### _Layout.cshtml

```html
<!DOCTYPE html>
<html>
<head>
    <title>@ViewData["Title"] - My ASP.NET Application</title>
    <link rel="stylesheet" type="text/css" href="~/css/site.css" />
</head>
<body>
    <div>
        <main>
            @RenderBody()
        </main>
    </div>
</body>
</html>
```

### Index.cshtml

```html
@{
    ViewData["Title"] = "Home Page";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Welcome to my website!</h2>
<p>This is the home page.</p>
```

# 뷰 시작(View Start)

`ViewStart` 파일은 모든 뷰에 공통으로 적용되는 설정을 정의할 수 있는 특별한 파일입니다.

이 파일을 사용하면 각 뷰마다 중복된 설정을 반복할 필요 없이, 공통 설정을 중앙에서 관리할 수 있습니다.

`ViewStart` 파일은 뷰가 렌더링되기 전에 실행되며, 일반적으로 레이아웃 설정과 같은 공통 설정을 정의하는 데 사용됩니다.

## ViewStart 파일의 위치

- **글로벌 설정**: `Views` 폴더의 루트에 `_ViewStart.cshtml` 파일을 두면 애플리케이션의 모든 뷰에 공통 설정이 적용됩니다.
- **폴더별 설정**: 특정 폴더(예: `Views/Home` 폴더)에 `_ViewStart.cshtml` 파일을 두면, 해당 폴더 내의 뷰에만 설정이 적용됩니다.

# 레이아웃 지정의 우선순위

`View` 파일과 `ViewStart` 파일들 내에서 `Layout` 속성이 중복으로 지정되었을 때, 다음과 같은 우선순위가 적용됩니다.

1. **개별 뷰의 Layout 설정**: 특정 뷰 파일 내에서 `Layout` 속성을 지정하면, 이 설정이 우선적으로 적용됩니다.
2. **폴더별 ViewStart 파일**: 폴더 내의 `_ViewStart.cshtml` 파일에 지정된 설정이 다음으로 적용됩니다.
3. **글로벌 ViewStart 파일**: `Views` 폴더 내의 `_ViewStart.cshtml` 파일에 지정된 설정이 가장 마지막으로 적용됩니다.