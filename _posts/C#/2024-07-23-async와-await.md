---
title: async와 await
date: 2024-07-23 21:47:16 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 비동기
  - 성능
math: false
type: C#
keywords:
  - async
  - await
  - 비동기
---

`async`와 `await` 키워드는 <span class="font_highlight">비동기 프로그래밍을 쉽게 할 수 있게 도와주는 도구</span>입니다.

이 두 키워드는 .NET Framework 4.5에서 도입되었으며, 주로 I/O 바운드 작업 (예: 파일 읽기/쓰기, 네트워크 요청)이나 긴 시간이 걸리는 작업 등을 수행할 때 유용합니다.

# 동기화(Synchronization)의 의미

동기화는 멀티스레딩 환경에서 여러 스레드가 동시에 공유 자원에 접근할 때, <span class="font_highlight">**자원의 일관성**을 유지하기 위한 방법</span>을 의미합니다.

네이버 영어사전에서 **synchronize**의 뜻은 "**동시에 발생하다(움직이다), 동시에 발생하게(움직이게) 하다**"라는 뜻인데, 이는 어떤 여러 일들의 타이밍을 맞추어 움직이도록 조정한다는 뜻을 담고 있고, 예시로 나온 이미지에서는 수중발레의 타이밍을 여러 사람들이 맞추고 있는 모습을 담고 있습니다.
>
> [Synchronize \| 네이버 영어사전](https://en.dict.naver.com/#/entry/enko/6674c7abf0874612a86836b457d1fb96){: target="_blank"}


또한, 용어 사전에서는 "**시간 차를 두고 일어나는 일들의 타이밍을 맞추어 전체적인 사건의 진행 속도를 조절하는 것**"이라고도 해석합니다.
>
> [Synchronization \| 인터넷 용어 영어·약자 사전](https://naver.me/5vcWN6p6){: target="_blank"}

결국, 동기화를 컴퓨터의 입장에서 보면 공유 자원을 동기화의 중점으로 잡고, 여러 스레드가 동시에 공유 자원에 접근할 때, 스레드 간의 협력이 이뤄진다고 볼 수 있습니다.

1. **공유 자원 접근**:
   <br> 여러 스레드가 동시에 같은 변수나 데이터 구조에 접근할 때, 데이터의 일관성을 보장하기 위해 동기화가 필요합니다.
2. **스레드 간의 협력**:
   <br> 한 스레드가 다른 스레드의 작업 결과를 기다리는 상황에서, 동기화를 통해 올바른 순서와 타이밍을 보장합니다.

즉, 동기화 기법은 가장 흔하고 일반적인 방법으로 작업들의 순서를 일치시켜 순서대로 동작시키게 함으로써, 비동기 작업을 수행하지 않고, 순서대로 작업을 진행하는 방법과,
<br>
세마포어(Semaphore)나 뮤텍스(Mutex), 락(Lock)이나 모니터(Monitor)와 같은 기법으로 공유 자원에 대한 접근을 스레드 간의 협력을 통해 일관되게 유지하는 방법 등이 있습니다.

정리하자면, 현실에서의 동기화란, 여러 사람들이 타이밍을 맞추어 동시에 동작들을 일치시키는 것을 의미하고, 
<br> 컴퓨터에서의 동기화란, 여러 스레드가 올바른 순서와 타이밍을 갖추어 공유 자원에 접근할 때, 데이터의 일관성과 무결성을 유지하기 위해 필요한 조정을 하는 것을 의미합니다.
<br> 이를 통해 스레드들이 충돌 없이 자원을 공유하며 프로그램의 정상적인 실행을 보장합니다.

# async 키워드

`async` 키워드는 메서드, 람다 표현식, 또는 익명 메서드 앞에 사용됩니다.

`async` 키워드를 사용하면 해당 메서드가 비동기 메서드임을 컴파일러에 알려줍니다.

`async` 메서드는 반드시 `Task`, `Task<T>` 또는 `void`를 반환해야 합니다.

여기서 `void`는 주로 이벤트 핸들러에 사용됩니다.

```csharp
public async Task MyAsyncMethod()
{
    // 비동기 작업 호출
    await SomeAsyncOperation();
}
```

# await 키워드

`await` 키워드는 비동기 작업의 완료를 비동기적으로 기다립니다.

`await`를 사용하면 비동기 작업이 완료될 때까지 현재 메서드의 실행이 일시 중지되었다가, 작업이 완료되면 중지된 위치에서 실행이 재개됩니다.

`await` 키워드는 반드시 `async` 키워드가 붙은 메서드 내에서 사용해야 합니다.

```csharp
public async Task MyAsyncMethod()
{
    // 비동기 작업 호출 및 대기
    var result = await SomeAsyncOperation();
    Console.WriteLine(result);
}
```

# 동작 원리

1. `async` 키워드를 사용한 메서드는 <span class="font_highlight">기본적으로 동기적으로 실행</span>됩니다.
2. `await` 키워드를 만나면, 비동기 작업이 시작되고, 해당 비동기 작업이 완료될 때까지 메서드의 실행이 일시 중지됩니다.
3. 비동기 작업이 완료되면 일시 중지된 메서드의 실행이 재개됩니다.
4. `await` 키워드는 비동기 작업이 완료될 때까지 UI 스레드나 주 스레드를 차단하지 않습니다.

예를 들어, 아래와 같은 코드가 실행되면, `await` 키워드를 만나기 전까지의 코드는 동기적으로 실행되어 "2. Before await"까지 순서대로 출력되다가, `await`를 만나면 비동기 작업이 시작되고 메서드는 즉시 반환됩니다.

이후, “3, 4, 5” 순서대로 실행되면서 `ExampleMethodAsync()`가 끝나 "6. After await”의 출력을 기다리지만, 1000ms 전에 프로그램이 종료되면서 출력되지 않은 모습입니다.

```csharp
internal class Program
{
    public static async Task ExampleMethodAsync()
    {
        // 이 부분은 동기적으로 실행됩니다.
        Console.WriteLine("2. Before await");

        // 비동기 작업을 기다립니다.
        await Task.Delay(1000);

        // 이 부분은 비동기 작업이 완료된 후에 실행됩니다.
        Console.WriteLine("6. After await");
    }

    public static void CallExampleMethod()
    {
        // ExampleMethodAsync()를 호출하면 즉시 Task를 반환합니다.
        Task task = ExampleMethodAsync();
        
        Console.WriteLine("3. ExampleMethodAsync 호출 됨");
    }

    static void Main(string[] args)
    {
        Console.WriteLine("1. Main 실행 됨.");

        CallExampleMethod();

        Console.WriteLine("4. CallExampleMethod 호출 됨.");

        Console.WriteLine("5. Main 종료 됨.");
    }
}
```

![비동기 프로그램 결과 1](https://i.postimg.cc/Dwj4CMyK/2024-07-22-130837.png)

위 코드를 아래와 같이 고치면, `await`로 진행된 비동기 작업이 종료될 때까지 기다렸다가, 다음 코드를 진행하는 모습을 보실 수 있습니다.

```csharp
internal class Program
{
    public static async Task ExampleMethodAsync()
    {
        // 이 부분은 동기적으로 실행됩니다.
        Console.WriteLine("2. Before await");

        // 비동기 작업을 기다립니다.
        await Task.Delay(1000);

        // 이 부분은 비동기 작업이 완료된 후에 실행됩니다.
        Console.WriteLine("6. After await");
    }

    public static async Task CallExampleMethod()
    {
        // ExampleMethodAsync()를 호출하면 즉시 Task를 반환합니다.
        Task task = ExampleMethodAsync();

        Console.WriteLine("3. ExampleMethodAsync 호출 됨");

        await task;  // 비동기 작업이 완료될 때까지 기다립니다.
    }

    static async Task Main(string[] args)
    {
        Console.WriteLine("1. Main 실행 됨.");

        await CallExampleMethod();

        Console.WriteLine("4. CallExampleMethod 호출 됨.");

        Console.WriteLine("5. Main 종료 됨.");
    }
}
```

![비동기 프로그램 결과 2](https://i.postimg.cc/QCYV6BhJ/2024-07-22-131455.png)

# Task - 비동기 메서드의 반환 형식

비동기 메서드는 세 가지 반환 형식을 가질 수 있습니다.

- `Task`: 반환값이 없는 비동기 작업을 나타냅니다.
- `Task<T>`: 반환값이 있는 비동기 작업을 나타냅니다.
- `void`: 주로 이벤트 핸들러에서 사용되며, 호출자는 작업이 완료될 때까지 대기할 수 없습니다.

### 예제

```csharp
public async Task<string> GetDataAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        HttpResponseMessage response = await client.GetAsync(url);
        response.EnsureSuccessStatusCode();
        string responseBody = await response.Content.ReadAsStringAsync();
        return responseBody;
    }
}

public async Task ProcessDataAsync()
{
    string data = await GetDataAsync("https://example.com");
    Console.WriteLine(data);
}
```

## Task.WhenAll

여러 비동기 작업을 병렬로 실행할 때는 `Task.WhenAll`을 사용할 수 있습니다.

```csharp
Task task1 = Task.Run(() => { /* 작업 1 */ });
Task task2 = Task.Run(() => { /* 작업 2 */ });

await Task.WhenAll(task1, task2);
```

## Task.WhenAny

하나 이상의 `Task` 중 하나라도 완료되기를 기다릴 때에는 `Task.WhenAny`를 사용할 수 있습닏나.

```csharp
Task task1 = Task.Run(() => { /* 작업 1 */ });
Task task2 = Task.Run(() => { /* 작업 2 */ });

Task completedTask = await Task.WhenAny(task1, task2);
```

## Task의 상태

`Task`는 여러 상태를 가질 수 있으며, 주요 상태는 다음과 같습니다.

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

# 예외 처리

비동기 메서드에서 발생한 예외는 일반적으로 `await` 호출 시 포착할 수 있습니다. `try-catch` 블록을 사용하여 예외를 처리할 수 있습니다.

```csharp
public async Task ProcessDataAsync()
{
    try
    {
        string data = await GetDataAsync("https://example.com");
        Console.WriteLine(data);
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"Request error: {e.Message}");
    }
}
```

# ConfigureAwait

`await`는 기본적으로 호출자 스레드로 다시 돌아가지만, `ConfigureAwait(false)`를 사용하여 이를 비활성화할 수 있습니다. 이는 특히 UI 스레드가 아닌 스레드 풀에서 코드를 실행할 때 유용합니다.

```csharp
public async Task<string> GetDataAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        HttpResponseMessage response = await client.GetAsync(url).ConfigureAwait(false);
        response.EnsureSuccessStatusCode();
        string responseBody = await response.Content.ReadAsStringAsync().ConfigureAwait(false);
        return responseBody;
    }
}
```

---

# 참고

[C＃ Task와 Task\<T\>](/posts/c-task%EC%99%80-taskt/){: }