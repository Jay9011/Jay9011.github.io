---
title: form과 Controller
date: 2024-07-16 21:24:42 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - Controller
math: false
type: aspnet
keywords:
  - form
  - Controller
---

HTML `<form>` 요소는 사용자가 웹 애플리케이션과 상호작용할 수 있도록 하는 입력 요소를 포함하는 컨테이너입니다.

주로 사용자로부터 데이터를 수집하여 서버로 전송하기 위해 사용됩니다.

### action

폼 데이터를 **제출할 URL**을 지정합니다.

ASP.NET 컨트롤러는 이 URL에 맞는 **액션 메서드**에서 폼 데이터를 수신하고 처리하게 됩니다.

### method

폼 데이터를 제출하는 **HTTP 메서드**를 지정합니다. 일반적으로 `GET` 또는 `POST`를 사용합니다.

ASP.NET 컨트롤러는 HTTP 메서드에 따라 다른 액션 메서드를 정의할 수 있는데, `HttpGet` 속성과 `HttpPost` 속성을 사용하여 메서드를 구분시키고, `GET` 과 `POST` 에 따라 동작을 다르게 정의할 수 있습니다.

# ASP.NET 액션과 form method

`HttpGet` 어트리뷰트와 `HttpPost` 어트리뷰트는 특정 HTTP method에 대해 컨트롤러 액션을 지정하는 데 사용됩니다.

## HttpGet 어트리뷰트

`HttpGet` 어트리뷰트는 GET 요청에 응답하는 액션 메서드를 지정합니다.

GET 요청은 브라우저의 주소창에 직접 입력하거나 링크를 클릭해서 발생하며, 주로 조회 작업에 사용됩니다.

GET 요청의 데이터는 URL에 쿼리 스트링 형태로 포함되므로, URL의 길이에 제한이 있을 수 있고, URL에 명시되기 때문에 사용자가 직접 볼 수 있습니다. 이는 보안에 취약할 수 있습니다.

## HttpPost 어트리뷰트

`HttpPost` 어트리뷰트는 POST 요청에 응답하는 액션 메서드를 지정합니다.

POST 요청은 주로 서버에 데이터를 제출하거나 서버의 상태를 변경할 때 사용되는데, 폼 데이터를 제출하거나 데이터베이스에 새로운 데이터를 추가하는 등의 작업에 사용됩니다.

POST 요청은 GET 요청과 달리 길이 제한이 없기 때문에 큰 데이터를 전송할 수 있습니다.

POST 요청의 데이터는 요청 본문(body)에 포함되므로, URL에 나타나지는 않습니다. 그러나 이는 데이터가 암호화된다는 의미는 아니므로 여러 방법으로 데이터를 보호하는 것이 중요합니다.

```csharp
public class CustomController : Controller
{
    // GET 요청: 폼 데이터를 쿼리 스트링에서 수신하여 처리
    [HttpGet]
    public ActionResult GetAction(string data)
    {
        // 데이터 처리
        return View("Result");
    }

    // POST 요청: 폼 데이터를 본문에서 수신하여 처리
    [HttpPost]
    public ActionResult PostAction(string data)
    {
        // 데이터 처리
        return View("Result");
    }
}
```