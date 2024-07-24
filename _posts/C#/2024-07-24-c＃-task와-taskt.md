---
title: C＃ Task와 Task＜T＞
date: 2024-07-24 22:18:32 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 비동기
  - 성능
math: false
type: C#
keywords:
  - Task
---

C#의 `Task`는 비동기 프로그래밍의 기본 단위로, 작업 단위를 표현하고 관리하는데 사용됩니다.

# `Task`와 `Task<T>`

- `Task`: **반환 값이 없는** 비동기 작업을 나타냅니다.
- `Task<T>`: 반환 값이 있는 비동기 작업을 나타냅니다. 여기서 `T`는 **반환 값의 타입**을 나타냅니다.

# Task의 생성과 사용

## 1. Task 생성 및 시작

`Task`를 생성하고 시작하는 여러 방법이 있습니다.

- **Task.Run**: 비동기 작업을 시작하는 가장 간단한 방법입니다.
    
    ```csharp
    Task task = Task.Run(() => {
        // 작업 내용
    });
    ```
    
- **Task.Factory.StartNew**: `Task.Run`과 유사하지만 더 많은 옵션을 제공합니다.
    
    ```csharp
    Task task = Task.Factory.StartNew(() => {
        // 작업 내용
    });
    ```
    

## 2. `Task<T>` 사용

`Task<T>`는 작업이 완료된 후 값을 반환할 수 있습니다.

```csharp
Task<int> task = Task.Run(() => {
    // 작업 내용
    return 42;
});

int result = await task;
Console.WriteLine(result);  // 출력: 42
```

# 주요 Task 메서드

## 1. Task.Delay

지정된 시간 동안 비동기적으로 대기하는 작업을 생성합니다.

```csharp
await Task.Delay(1000);  // 1초 대기
```

## 2. Task.WhenAll

여러 `Task`를 동시에 기다립니다. 모든 작업이 완료될 때까지 비동기적으로 대기합니다.

```csharp
Task task1 = Task.Run(() => { /* 작업 1 */ });
Task task2 = Task.Run(() => { /* 작업 2 */ });

await Task.WhenAll(task1, task2);
```

## 3. Task.WhenAny

하나 이상의 `Task` 중 하나라도 완료되기를 기다립니다.

```csharp
Task task1 = Task.Run(() => { /* 작업 1 */ });
Task task2 = Task.Run(() => { /* 작업 2 */ });

Task completedTask = await Task.WhenAny(task1, task2);
```

# Task의 상태 관리

`Task`는 여러 상태를 가질 수 있습니다. 주요 상태는 다음과 같습니다:

- **WaitingForActivation**: 작업이 아직 시작되지 않았습니다.
- **WaitingToRun**: 작업이 실행될 준비가 되었습니다.
- **Running**: 작업이 실행 중입니다.
- **RanToCompletion**: 작업이 정상적으로 완료되었습니다.
- **Canceled**: 작업이 취소되었습니다.
- **Faulted**: 작업이 예외를 던졌습니다.

```csharp
Task task = Task.Run(() => {
    // 작업 내용
});
Console.WriteLine(task.Status);  // 현재 Task 상태 출력
await task;
Console.WriteLine(task.Status);  // 작업 완료 후 Task 상태 출력
```

# Task의 예외 처리

비동기 작업 중 발생한 예외는 `await` 키워드를 통해 처리할 수 있습니다. `await`는 `Task`의 예외를 자동으로 던집니다.

```csharp
try
{
    Task task = Task.Run(() => {
        throw new InvalidOperationException("예외 발생!");
    });
    await task;
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);  // 출력: 예외 발생!
}
```

# 예제: Task를 사용한 비동기 작업

다음은 `Task`를 사용하여 여러 비동기 작업을 동시에 실행하고 결과를 처리하는 예제입니다.

```csharp
public async Task<int> PerformTaskAsync(int value)
{
    await Task.Delay(1000);  // 1초 대기
    return value * value;    // 제곱 값 반환
}

public async Task ExecuteTasksAsync()
{
    Task<int> task1 = PerformTaskAsync(2);
    Task<int> task2 = PerformTaskAsync(3);
    Task<int> task3 = PerformTaskAsync(4);

    int[] results = await Task.WhenAll(task1, task2, task3);

    Console.WriteLine(string.Join(", ", results));  // 출력: 4, 9, 16
}
```

위 예제에서 `PerformTaskAsync` 메서드는 입력 값의 제곱을 계산하는 비동기 작업을 나타냅니다. <br>
`ExecuteTasksAsync` 메서드는 세 개의 비동기 작업을 동시에 시작하고, `Task.WhenAll`을 사용하여 모든 작업이 완료될 때까지 기다립니다.  <br>
완료된 작업의 결과는 배열로 반환됩니다.

이와 같이 `Task`를 사용하면 비동기 작업을 효율적으로 관리하고, 동시성 문제를 해결하며, 프로그램의 응답성을 높일 수 있습니다.