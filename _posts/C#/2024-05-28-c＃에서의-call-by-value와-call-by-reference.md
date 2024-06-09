---
title: C＃에서의 Call By Value와 Call By Reference
date: 2024-05-28 17:49:59 +0900
categories:
    - 풀스택 개발
    - CSharp
tags:
    - 값 타입
    - 참조
    - 참조 타입
    - 함수
math: false
type: C#
keywords:
    - Call By Reference
    - Call By Value
    - 복사
    - 참조
---

C# 프로그래밍 언어에서는 메서드 호출 시 인자를 전달하는 두 가지 주요 방식인 Call By Value와 Call By Reference가 있습니다.

이 두 가지 방식은 메서드를 호출할 때, 값을 복사해서 전달하느냐, 원본 데이터를 넘기느냐로 나뉠 수 있습니다.

# Call By Value

Call By Value 방식은 <span class="font_highlight">인자가 메서드에 **값으로 전달**되는 것</span>을 의미합니다.

다시 말해, 메서드를 호출할 때 실제 인자의 값만 넘겨서 <span class="important">**복사**본이 생성</span>되어 메서드에 전달됩니다. 이로 인해 메서드 내에서 인자의 값을 변경하더라도 원래 변수에는 영향을 미치지 않습니다.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        int number = 10;
        Console.WriteLine($"Before: {number}"); // Before: 10
        
        CallByValue(number);
        Console.WriteLine($"Value After: {number}");  // Value After: 10
    }

    public static void CallByValue(int value)
    {
        value = 20;
    }
}
```
위 예제에서 `CallByValue` 메서드는 `value`라는 매개변수를 받아 그 값을 20으로 변경하지만, 실제로 Main 메서드 내의 `number` 변수의 값에는 아무런 영향을 미치지 않습니다.
<br>
이는 CallByValue 메서드 호출 시 number 변수의 **복사본**이 전달되었기 때문입니다.

# Call By Reference

Call By Reference 방식은 <span class="font_highlight">인자가 메서드에 **참조로 전달**되는 것</span>을 의미합니다.

이렇게 참조로 전달 된 변수는 <span class="font_highlight">메서드 내에서 직접 접근 및 수정이 가능</span>합니다. 이를 통해 메서드 내에서 변수의 값을 변경하면 **원래 변수에도 영향**을 미치게 됩니다.

C#에서는 `ref`와 `out` 키워드를 사용하여 참조로 인자를 전달할 수 있습니다.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        int number = 10;
        Console.WriteLine($"Before: {number}"); // Before: 10
        
        CallByReference(ref number);
        Console.WriteLine($"Ref After: {number}");  // Ref After: 20
    }

    public static void CallByReference(ref int value)
    {
        value = 20;
    }
}
```
위 예제에서 `CallByReference` 메서드는 `ref` 키워드를 사용하여 `value` 매개변수를 참조로 받습니다. 이로 인해 value의 변경은 Main 메서드 내의 number 변수에 직접 영향을 미칩니다.

# 참조 타입에서의 Call By Value와 Call By Reference

C#에서의 재미있는 점은 바로 참조 타입(예: 클래스 인스턴스)의 Call By Value와 Call By Reference의 동작입니다.

## 참조 타입과 Call By Value

참조 타입을 Call By Value의 매개변수로 전달할 때, 해당 참조 타입 변수의 참조 값(즉, 객체를 가리키는 포인터)이 복사되어 메서드에 전달됩니다.

위에서 Call By Value는 복사본이 생성 된다고 했는데, 참조 타입의 동작은 객체 자체의 복사본이 아니라 <span class="important">**객체를 가리키는 참조 값의 복사본**</span>이라는 점에서 일반 객체의 복사와 차이가 있습니다.

즉, C++을 비유로 들자면, 객체를 가리키는 주소를 복사해서 전달했고.
<br>
따라서, 메서드 내에서 이 참조 값을 통해 객체의 멤버를 변경하면, 원래 객체에도 영향을 미치지만, 참조 값 자체를 변경하면 원래 변수에는 영향을 미치지 않게 됩니다.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        MyClass myClass = new MyClass();
        myClass.Value = 10;
        Console.WriteLine($"Before: {myClass.Value}"); // Before: 10

        ModifyObject(myClass);
        Console.WriteLine($"ModifyObject: {myClass.Value}");    // ModifyObject: 20

        ReplaceObject(myClass);
        Console.WriteLine($"ReplaceObject: {myClass.Value}");   // ReplaceObject: 20
    }

    public static void ModifyObject(MyClass myClass)
    {
        myClass.Value = 20;
    }
    public static void ReplaceObject(MyClass myClass)
    {
        myClass = new MyClass();
        myClass.Value = 30;
    }
}
```
위의 예에서는 처음 Call By Value로 멤버의 값을 변경했고, 그 결과가 원본 데이터에 반영 된 것을 보여줍니다.

하지만, `ReplaceObject`라는 메서드를 호출해서 새로운 객체로 변경하고 그 멤버 값을 변경해 줬지만, 그 결과가 원본 데이터에 영향을 주지 않았다는 것을 알게 됩니다.

즉, `myClass`라는 메서드 내의 매개변수에는 일종의 '주소 값'이 들어 있었고, 그 주소 값을 다른 객체의 주소 값으로 변경한다고 한들, 메서드 내에서 사용되는 `myClass` 변수에서만 주소가 변경 되었을 뿐, 원본 데이터를 바꾼게 아니라는게 됩니다.

## 참조 타입과 Call By Reference

이제, Call By Reference를 봅시다.

Call By Reference 방식은 인자가 메서드에 **참조로 전달**되어 메서드 내에서 직접 접근 및 수정이 가능하다고 했습니다.

즉, 주소가 복사되는 방식이 아니라, 원본 그 자체가 넘어간다고 보셔도 좋을 것 같습니다.

따라서, Call By Reference로 원본 데이터를 가리키던 주소를 다른 주소로 바꾸면 이게 적용되어 객체가 바뀌게 됩니다.

```csharp
public class Program
{
    static void Main(string[] args)
    {
        MyClass myClass = new MyClass();
        myClass.Value = 10;
        Console.WriteLine($"Before: {myClass.Value}"); // Before: 10

        ModifyObject(myClass);
        Console.WriteLine($"ModifyObject: {myClass.Value}");    // ModifyObject: 20

        ReplaceObject(ref myClass);
        Console.WriteLine($"ReplaceObject With ref: {myClass.Value}");  // ReplaceObject With ref: 30
    }

    public static void ModifyObject(MyClass myClass)
    {
        myClass.Value = 20;
    }
    public static void ReplaceObject(ref MyClass myClass)
    {
        myClass = new MyClass();
        myClass.Value = 30;
    }
}
```
위 예제는 Call By Reference로 참조 타입을 전달한 모습입니다.

신기하게도 메서드 내부에서 객체를 만들고, 이를 `myClass`라는 변수에 대입시키기만 했는데, 원본 변수가 바뀐 것을 볼 수 있습니다.

정리하자면, C#에서 참조 타입을 매개변수로 전달할 때, 기본적으로는 Call By Value로 전달되며 참조 값(주소 값)의 **복사**본이 전달됩니다.

또한, `ref` 또는 `out`으로 참조 타입을 전달하면, Call By Reference 방식으로 전달되며, 이때에는 참조 타입 변수의 참조 값 변경이 원래 변수에 영향을 미칠 수 있습니다.


# Call By Value와 Call By Reference의 차이점

- 메모리 관리
  - Call By Value는 인자의 복사본을 생성하여 전달하므로 메모리 사용량이 증가할 수 있습니다. 따라서, 크기가 작은 데이터를 전달하는데 유용합니다.
  - Call By Reference는 원본의 주소를 전달하므로 메모리 사용량이 적습니다. 따라서, 크기가 큰 데이터를 전달하는데 유용합니다.
- 변경 가능성
  - Call By Value는 메서드 내에서 인자의 변경이 원본 변수에 영향을 미치지 않습니다.
  - Call By Reference는 메서드 내에서 인자의 변경이 원본 변수에 직접 영향을 미칩니다.
- 용도
  - Call By Value는 주로 값 형식(primitive type) 데이터를 전달할 때 사용됩니다.
  - Call By Reference는 주로 객체 또는 대용량 데이터를 전달할 때 사용됩니다.

---

# 참고

[C＃의 값 타입과 참조 타입](/posts/c-%EC%9D%98-%EA%B0%92-%ED%83%80%EC%9E%85%EA%B3%BC-%EC%B0%B8%EC%A1%B0-%ED%83%80%EC%9E%85/){: }