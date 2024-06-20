---
title: C＃의 String과 StringBuilder
date: 2024-06-18 19:35:46 +0900
categories:
  - 풀스택 개발
  - CSharp
tags:
  - 문자
math: false
type: C#
keywords:
  - C#
  - String
  - StringBuilder
  - 문자열
---

C#에서 `string`과 `StringBuilder`는 문자열을 처리하기 위한 두 가지 주요 클래스입니다.

`string` 클래스는 불변(immutable) 객체이고, `StringBuilder` 클래스는 가변(mutable) 객체입니다.

# string 클래스

`string` 객체는 한 번 만들면 변경할 수 없습니다. 이것을 <span class="keyword">**불변(immutable) 객체**</span>라고 합니다.

따라서, 문자열 연산(예: 연결, 삽입, 삭제 등)을 수행할 때마다 <span class="font_highlight">새로운 `string` 객체를 만들어 변수명에 할당</span>해 주는 것으로 동작합니다.

문자열이 빈번하게 변경되는 경우에는 객체가 많이 만들어지고, 그만큼 **실행 속도**와 **메모리 사용**이 많아집니다.


이런 특성으로 인해 `string`은 다음과 같은 상황에서 유용합니다.

- **단순 문자열 조작**: 짧은 문자열을 자주 변경하지 않고 사용하는 경우.
- **안전성**: 불변성이 보장되므로 스레드 안정성(thread safety)이 필요할 때.

#### 사용 예제

```csharp
string str1 = "Hello";
string str2 = str1 + " World";  // 새로운 문자열 객체 생성
Console.WriteLine(str2);        // "Hello World" 출력
```

# StringBuilder 클래스

`StringBuilder` 클래스는 <span class="keyword">**가변(mutable) 객체**</span>입니다. 이는 <span class="font_highlight">한 번 생성된 후에도 그 내용이 변경될 수 있음</span>을 의미합니다.

`StringBuilder`는 내부적으로 동적 배열을 사용하여 문자열을 저장하고, 필요에 따라 크기를 조절합니다.

따라서, 문자열을 <span class="important">반복적으로 수정해야 하는 경우</span> `StringBuilder`를 사용하는 것이 더 효율적입니다.

#### 주요 특징

- **성능**: 문자열 조작(특히 반복적인 연결, 삽입, 삭제 등) 시 성능이 뛰어납니다.
- **메모리 효율성**: 문자열 변경 시 새로운 객체를 생성하지 않으므로 메모리 사용이 효율적입니다.

#### 사용 예제

```csharp
StringBuilder sb = new StringBuilder("Hello");
sb.Append(" World");
Console.WriteLine(sb.ToString());  // "Hello World" 출력
```

# 주로 사용하는 메서드 표

다음은 `string` 클래스와 `StringBuilder` 클래스의 자주 사용하는 메서드를 포함한 표입니다. 메서드가 많아 표를 나누어 정리하였습니다.

## string 메서드

| 메서드                                                                            | 설명                                          |
| ------------------------------------------------------------------------------ | ------------------------------------------- |
| `Contains(string value)`                                                       | 지정한 문자열이 포함되어 있는지 여부를 확인합니다.                |
| `StartsWith(string value)`                                                     | 지정한 문자열로 시작하는지 여부를 확인합니다.                   |
| `EndsWith(string value)`                                                       | 지정한 문자열로 끝나는지 여부를 확인합니다.                    |
| `IndexOf(string value)`                                                        | 지정한 문자열이 처음으로 나타나는 인덱스를 반환합니다.              |
| `LastIndexOf(string value)`                                                    | 지정한 문자열이 마지막으로 나타나는 인덱스를 반환합니다.             |
| `Substring(int startIndex)`                                                    | 지정한 인덱스에서 시작하는 부분 문자열을 반환합니다.               |
| `Replace(string oldValue, string newValue)`                                    | 문자열 내의 지정한 부분 문자열을 다른 문자열로 대체합니다.           |
| `ToLower()`                                                                    | 문자열을 소문자로 변환합니다.                            |
| `ToUpper()`                                                                    | 문자열을 대문자로 변환합니다.                            |
| `Trim()`                                                                       | 문자열의 시작과 끝에서 모든 공백 문자를 제거합니다.               |
| `TrimStart()`                                                                  | 문자열의 시작에서 모든 공백 문자를 제거합니다.                  |
| `TrimEnd()`                                                                    | 문자열의 끝에서 모든 공백 문자를 제거합니다.                   |
| `Split(char[] separator)`                                                      | 지정한 구분 문자를 사용하여 문자열을 부분 문자열 배열로 나눕니다.       |
| `Join(string separator, string[] value)`                                       | 지정한 구분 문자를 사용하여 문자열 배열의 요소를 하나의 문자열로 결합합니다. |
| `Equals(object value)`                                                         | 두 문자열이 동일한지 비교합니다.                          |
| `PadLeft(int totalWidth)`                                                      | 지정된 총 너비를 가지도록 문자열의 왼쪽을 패딩합니다.              |
| `PadRight(int totalWidth)`                                                     | 지정된 총 너비를 가지도록 문자열의 오른쪽을 패딩합니다.             |
| `Compare(string strA, string strB)`                                            | 두 문자열을 비교하여 정수 값을 반환합니다.                    |
| `Concat(params string[] values)`                                               | 여러 문자열을 하나의 문자열로 결합합니다.                     |
| `Copy(string str)`                                                             | 문자열의 새로운 인스턴스를 생성합니다.                       |
| `CopyTo(int sourceIndex, char[] destination, int destinationIndex, int count)` | 문자열의 지정된 범위를 다른 문자 배열로 복사합니다.               |
| `Insert(int startIndex, string value)`                                         | 지정한 인덱스 위치에 문자열을 삽입합니다.                     |
| `Format(string format, params object[] args)`                                  | 지정한 형식에 맞춰 문자열을 구성합니다.                      |

## StringBuilder 메서드

| 메서드                                          | 설명                                                                           |
|-----------------------------------------------|-------------------------------------------------------------------------------|
| `Append(string value)`                        | 문자열을 현재 `StringBuilder` 객체의 끝에 추가합니다.                                   |
| `AppendLine()`                                | 현재 `StringBuilder` 객체의 끝에 새 줄을 추가합니다.                                         |
| `AppendFormat(string format, object arg0)`    | 지정한 형식에 맞춰 문자열을 추가합니다.                                           |
| `Insert(int index, string value)`             | 지정한 인덱스 위치에 문자열을 삽입합니다.                                             |
| `Remove(int startIndex, int length)`          | 지정한 범위 내의 문자를 제거합니다.                                                 |
| `Replace(string oldValue, string newValue)`   | 문자열의 지정한 부분 문자열을 다른 문자열로 대체합니다.                             |
| `Clear()`                                     | 현재 `StringBuilder` 객체의 모든 내용을 제거합니다.                                         |
| `ToString()`                                  | `StringBuilder` 객체의 내용을 문자열로 변환합니다.                                        |
| `Length`                                      | `StringBuilder` 객체의 길이를 가져오거나 설정합니다.                                         |
| `Capacity`                                    | 내부적으로 할당된 메모리의 크기를 가져오거나 설정합니다.                                     |
| `EnsureCapacity(int capacity)`                | `StringBuilder`가 최소한 지정된 용량을 가지도록 합니다.                                   |

# 차이점 요약

| 특성          | `string`       | `StringBuilder` |
| ----------- | -------------- | --------------- |
| **가변성**     | 불변 (Immutable) | 가변 (Mutable)    |
| **성능**      | 문자열 변경 시 비용이 큼 | 반복적인 변경 시 성능 우수 |
| **사용 용도**   | 변경이 적고 짧은 문자열  | 반복적인 변경이 많은 경우  |
| **스레드 안정성** | 스레드 안전         | 스레드 안전하지 않음     |
| **메모리 사용**  | 자주 변경 시 비효율적   | 메모리 사용 효율적      |
