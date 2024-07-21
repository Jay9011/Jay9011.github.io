---
title: Connection 객체의 Open()과 OpenAsync()
date: 2024-07-21 16:57:18 +0900
categories:
  - 풀스택 개발
  - ASPNET
tags:
  - aspnet
  - Controller
  - DAL/DAO
math: false
type: aspnet
keywords:
  - Connection
  - Open
  - OpenAsync
---

`SqlConnection` 클래스는 데이터베이스 연결을 여는 메서드로 `Open()`과 `OpenAsync()`를 제공합니다.

이 두 메서드는 데이터베이스 연결을 여는 기능을 수행하지만, 동기와 비동기 방식으로 동작하는 차이가 있습니다.

# Open()

```csharp
public override void Open();
```

- **동기적 메서드**: `Open()` 메서드는 동기적으로 실행됩니다. 즉, 이 메서드를 호출하면 **연결이 열릴 때까지 현재 스레드는 블록**됩니다. 연결이 성공적으로 열리면 다음 코드가 실행됩니다.
- **사용 사례**: 간단한 애플리케이션 또는 동기식 작업을 수행할 때 사용합니다.
예를 들어, 작은 데이터베이스 작업이나 데이터베이스 연결이 짧은 시간 내에 이루어질 때 적합합니다.

# OpenAsync()

```csharp
public Task OpenAsync();
public Task OpenAsync(CancellationToken cancellationToken);
```

- **비동기적 메서드**: `OpenAsync()` 메서드는 비동기적으로 실행됩니다. 이 메서드를 호출하면 <span class="font_highlight">데이터베이스 연결이 비동기적으로 열리며, 현재 스레드는 블록되지 않고 다른 작업을 계속 수행</span>할 수 있습니다.
- **사용 사례**: 비동기 프로그래밍 모델을 사용하는 애플리케이션에서 유용합니다.
예를 들어, 서버 애플리케이션에서 여러 클라이언트 요청을 동시에 처리하거나 UI 애플리케이션에서 UI 스레드를 블록하지 않고 데이터베이스 작업을 수행할 때 적합합니다.

## async와 await

`OpenAsync` 메서드를 사용할 때는 C#의 `async`와 `await` 키워드를 사용하는 것이 일반적입니다.

- **async**: 메서드가 비동기적으로 실행될 것임을 나타냅니다. `async` 키워드는 메서드 선언에 사용되며, 메서드 내부에서 `await` 키워드를 사용할 수 있게 해줍니다.
- **await**: 비동기 작업이 완료될 때까지 기다립니다. `await` 키워드는 `Task` 또는 `Task<T>`를 반환하는 메서드 앞에 사용되며, 해당 작업이 완료될 때까지 비동기적으로 기다립니다.

# Task<T>

`Task<T>`는 .NET에서 비동기 작업을 나타내는 클래스입니다.

`Task`는 **반환 값이 없는** 비동기 작업을 나타내고, `Task<T>`는 **반환 값이 있는** 비동기 작업을 나타냅니다. 이때, `T`는 작업이 완료되었을 때 반환되는 값의 형식을 나타냅니다.

`Task<T>`는 **작업의 상태를 추적**할 수 있습니다. 작업이 완료되었는지, 실패했는지, 취소되었는지 등을 확인할 수 있습니다.

또한, `Task<T>`는 작업 중 발생한 **예외를 캡처**하고 나중에 처리할 수 있도록 하거나, 중간에 **작업을 취소**할 수 있는 메커니즘을 제공합니다. 주로 `CancellationToken`을 사용합니다.

## Task<T>의 구성 요소

### Awaiter

`await` 키워드를 사용하여 `Task<T>`의 작업이 완료될 때까지 기다릴 수 있습니다.

```csharp
int result = await PerformCalculationAsync(5, 10);
```

### Status 속성

작업의 상태를 나타냅니다. 예를 들어, `Running`, `Completed`, `Faulted`, `Canceled` 등의 상태를 확인할 수 있습니다.

```csharp
Task<int> task = PerformCalculationAsync(5, 10);
Console.WriteLine($"Task status: {task.Status}");
```

### Result 속성

작업이 완료되면 `Result` 속성을 통해 결과 값을 얻을 수 있습니다. 작업이 완료되지 않은 상태에서 `Result`를 접근하면 현재 스레드가 블로킹됩니다.

```csharp
Task<int> task = PerformCalculationAsync(5, 10);
int result = task.Result; // 비동기 작업이 완료될 때까지 기다린 후 결과를 반환
```

### ContinueWith

작업이 완료된 후에 실행될 **콜백 함수를 등록**하는 기능을 제공합니다. 이 콜백 함수는 작업이 완료된 시점에 실행되며, 작업의 결과나 상태를 기반으로 추가 작업을 수행할 수 있습니다.

```csharp
PerformCalculationAsync(5, 10).ContinueWith(task =>
{
    int result = task.Result;
    Console.WriteLine($"Calculation result: {result}");
});
```

### Exception 속성

작업 중 발생한 **예외를 캡처**합니다.

```csharp
try
{
    int result = await PerformCalculationAsync(5, 10);
}
catch (Exception ex)
{
    Console.WriteLine($"An error occurred: {ex.Message}");
}
```

# Open()과 OpenAsync()의 비교

| 특성 | Open() | OpenAsync() |
| --- | --- | --- |
| 실행 방식 | 동기적 | 비동기적 |
| 스레드 블록 여부 | 블록 | 블록되지 않음 |
| 사용 사례 | 간단한 동기식 작업 | 비동기 작업, 다중 작업 처리 |
| 반환 값 | void | Task 또는 Task<void> |
| 취소 지원 | 지원하지 않음 | CancellationToken 지원 |

# 비동기 프로그래밍의 장점

- **응답성 향상**: UI 애플리케이션의 경우, 비동기 메서드를 사용하면 UI 스레드를 블록하지 않아 애플리케이션이 더 응답성이 높아집니다.
- **자원 효율성**: 서버 애플리케이션의 경우, 비동기 작업을 통해 더 적은 스레드를 사용하여 더 많은 클라이언트 요청을 처리할 수 있습니다.
- **성능 향상**: 비동기 I/O 작업을 통해 네트워크 또는 디스크 I/O와 같은 느린 작업에서 스레드를 유휴 상태로 두지 않고 다른 작업을 수행할 수 있습니다.