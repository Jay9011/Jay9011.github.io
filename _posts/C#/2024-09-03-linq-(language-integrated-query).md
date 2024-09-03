---
title: LINQ (Language Integrated Query)
date: 2024-09-03 21:49:45 +0900
categories:
  - 풀스택 개발
  - CSharp
tags: []
math: false
fmContentType: C#
keywords:
  - Language Integrated Query
  - LINQ
  - Query
---

**LINQ(Language Integrated Query)**는 C#과 .NET에서 데이터 소스를 쿼리할 수 있게 해주는 통합된 쿼리 문법입니다.

SQL과 유사한 문법을 사용해 데이터베이스뿐만 아니라 컬렉션, XML, 데이터셋 등 다양한 데이터 소스를 일관된 방식으로 쿼리할 수 있습니다.

LINQ는 기본적으로 `IEnumerable<T>` 또는 `IQueryable<T>` 인터페이스를 구현하는 모든 데이터 소스에 대해 사용할 수 있습니다. 여기에는 배열, 리스트, 데이터베이스, XML, 데이터셋 등이 포함됩니다.

즉, `IEnumerable<T>` 또는 `IQueryable<T>` 인터페이스를 구현하는 모든 데이터에서 `from`, `where`, `select` 등의 키워드를 사용하여 간결하고 가독성 높은 코드를 작성할 수 있게 한 것이 LINQ입니다.

# LINQ 쿼리 구문과 메서드 구문

LINQ는 **쿼리 구문(Query Syntax)**과 **메서드 구문(Method Syntax,** 또는 **Fluent Syntax),** 두 가지 방식으로 쓸 수 있습니다.

쿼리 구문은 SQL과 비슷한 문법을 사용하며, 메서드 구문은 람다 식을 많이 사용합니다.

```csharp
int[] numbers = [1, 2, 3, 4, 5, 6];

// 쿼리 구문
var evenNumbersQuery = from num in numbers
                       where num % 2 == 0
                       select num;

// 메서드 구문 (람다 식 사용)
var evenNumbersMethod = numbers.Where(num => num % 2 == 0);
```

쿼리 구문은 SQL과 유사한 문법을 사용하고, 메서드 구문은 메서드 체이닝과 람다 식을 사용합니다.

쿼리 구문은 가독성이 좋고 SQL과 유사하여 친숙할 수 있지만, 메서드 구문은 람다 식을 활용하여 더 간결하고 표현력이 좋습니다.

두 구문 중 마음에 드는 구문을 사용하면 되는데, 쿼리 구문에서 지원되지 않는 기능이 메서드 구문에서는 가능할 수 있습니다.

예를 들어, SelectMany, Aggregate와 같은 고급 메서드는 주로 메서드 구문에서 사용됩니다.

# LINQ 쿼리 구문 (Query Syntax)

LINQ 쿼리 구문은 SQL과 유사한 문법을 사용하여 데이터 소스를 쿼리하는 방식입니다.

C#에서 `from`, `where`, `select` 등의 키워드를 사용하여 간결하고 가독성 높은 쿼리를 작성할 수 있습니다.

주로 컬렉션, 배열, 데이터베이스, XML 등 다양한 데이터 소스를 쿼리할 수 있습니다.

## 기본 구성 요소

1. **from**: 데이터 소스와 쿼리 변수를 지정합니다.
2. **where**: 조건을 지정하여 데이터를 필터링합니다.
3. **select**: 결과를 선택하여 반환합니다

```csharp
int[] numbers = [1, 2, 3, 4, 5, 6];
var evenNumbers = from num in numbers
                  where num % 2 == 0
                  select num;

foreach (var num in evenNumbers)
{
    Console.WriteLine(num); // 출력: 2, 4, 6
}
```

위 예제는 간단하게 짝수만 골라내는 예제입니다.

**`from num in numbers`**는 `numbers` 배열의 각 요소를 순회하여 `num`으로 저장합니다.

이후 **`where num % 2 == 0`**에서 `num`이 2로 나누어 떨어지는(즉, 짝수인) 요소만 필터링하여 **`select num`**으로 필터링된 짝수를 결과로 반환합니다.

## 주요 키워드

1. **from**: 데이터 소스와 변수 지정
2. **where**: 조건 필터링
3. **select**: 결과 선택
4. **orderby**: 정렬
5. **join**: 두 데이터 소스를 결합
6. **group**: 데이터를 그룹화

```csharp
var query = from student in students
            join grade in grades on student.Id equals grade.StudentId
            select new { student.Name, grade.Score };
```

위 예제는 join을 보여주는 예제입니다.

**`from student in students`**로 `students` 컬렉션의 각 요소를 순회하여 학생 객체를 `student`로 저장합니다.

이때, **`join grade in grades on student.Id equals grade.StudentId`**구문에서 `students`와 `grades` 컬렉션을 `student.Id`와 `grade.StudentId`가 같은 요소끼리 결합합니다.

이후, 결합된 데이터에서 학생의 이름과 점수를 선택하여 새로운 익명 객체로 만들어 반환합니다.

```csharp
var query = from student in students
            group student by student.Grade into studentGroup
            select studentGroup;
```

위 예제는 group을 보여주는 예제입니다.

**`from student in students`**로 `students` 컬렉션의 각 요소를 순회하여 학생 객체를 `student`로 저장합니다.

이후, **`group student by student.Grade into studentGroup`**로 ****학생들을 `Grade` 필드 값에 따라 그룹화하고, 그 결과를 `studentGroup`에 저장하여 반환합니다.

각 그룹은 같은 `Grade`를 가진 학생들의 모임이 됩니다.

# LINQ 메서드 구문 (Method Syntax)

LINQ 메서드 구문은 LINQ 쿼리를 메서드 체이닝 방식으로 작성하는 방식입니다.

이는 `Enumerable` 및 `Queryable` 클래스의 확장 메서드를 사용하여 데이터 소스를 쿼리하는 방식으로, 람다 식을 주로 사용합니다.

## 주요 메서드

1. **Where**: 조건에 맞는 요소를 필터링합니다.
2. **Select**: 각 요소를 변환합니다.
3. **OrderBy, OrderByDescending**: 요소를 정렬합니다.
4. **Join**: 두 컬렉션을 결합합니다.
5. **GroupBy**: 요소를 그룹화합니다.
6. **Sum, Average, Min, Max**: 집계 함수로 값을 계산합니다.
7. **Any, All**: 요소가 조건을 만족하는지 확인합니다.

```csharp
int[] numbers = { 1, 2, 3, 4, 5, 6 };
var evenNumbers = numbers.Where(num => num % 2 == 0);

foreach (var num in evenNumbers)
{
    Console.WriteLine(num); // 출력: 2, 4, 6
}
```

위 예제는 기본적으로 `numbers`에 쿼리 메서드 **`.Where()`**를 사용하고, 람다식 **`num => num % 2 == 0`**으로 각 요소를 `num`으로 넘겨 짝수를 출력하는 예제입니다.

```csharp
var query = students.Join(
    grades,
    student => student.Id,
    grade => grade.StudentId,
    (student, grade) => new { student.Name, grade.Score });
```

위 예제는 `students` 컬렉션에 대해 `Join` 메서드를 호출하여 `grades` 컬렉션과 결합합니다.

이때, 첫 번째 매개변수로 `students` 컬렉션의 `Id` 필드를 지정하여 결합 조건을 정의하고, 두 번째 매개변수로 `grades` 컬렉션의 `StudentId` 필드를 지정하여 결합 조건을 정의합니다.

이들은 각각 `student`, `grade` 매개변수로 람다식 **`(student, grade) => new { student.Name, grade.Score }`** 에 넘어가고, 새 익명 객체를 생성하여 학생의 이름(`Name`)과 점수(`Score`)를 반환합니다.

```csharp
var query = students.GroupBy(student => student.Grade);
```

위 예제는 `students` 컬렉션에 대해 `GroupBy` 메서드를 호출하여 `Grade` 필드를 기준으로 학생들을 그룹화하는 예제입니다.

`students` 컬렉션의 각 요소들을 `student`를 매개변수로 가지는 람다식 **`student => student.Grade`** 에 넘겨, 각 `Grade` 값에 해당하는 학생들의 모임으로 구성된 컬렉션을 반환하게 됩니다.

## 메서드 체이닝

LINQ 메서드 구문의 장점은 <span class="font_highlight">**메서드 체이닝**을 통해 **여러 LINQ 메서드를 조합**하여 복잡한 쿼리를 작성</span>할 수 있다는 점입니다.

```csharp
var complexQuery = numbers
    .Where(num => num > 2)
    .OrderByDescending(num => num)
    .Select(num => new { Original = num, Square = num * num });

foreach (var item in complexQuery)
{
    Console.WriteLine($"Original: {item.Original}, Square: {item.Square}");
}
```

위 예제처럼, `numbers` 배열에서 값이 2보다 큰 숫자들을 골라, 내림차순으로 정렬하고, 각 숫자를 “원래의 숫자”와 “제곱값”으로 이루어진 익명 객체를 만들어 반환 시킬 수 있습니다.

# 정리

LINQ는 다양한 데이터 소스에 대해 SQL과 유사한 쿼리 구문과 메서드 체이닝을 통한 구문을 제공하여 가독성 높은 코드를 작성할 수 있게 해줍니다.

간단하게 작성 가능하거나 인덱스 요소가 필요한 경우에는 `for`문이 더 가독성이 좋거나 편할 수 있습니다.

`for`문이나 `LINQ`는 각각 각기 다른 장점을 가지므로, 상황에 맞게, 기호게 맞게 사용하시면 좋을 것 같습니다.

좀 더 자세한 내용이나 더 많은 LINQ 연산자를 확인하시려면 공식 사이트([LINQ 쿼리 소개 - C# \| Microsoft Learn](https://learn.microsoft.com/ko-kr/dotnet/csharp/linq/get-started/introduction-to-linq-queries){: target="_blank"})를 참고해 주시면 좋을 것 같습니다.

---

# 참고

[LINQ 쿼리 소개 - C# \| Microsoft Learn](https://learn.microsoft.com/ko-kr/dotnet/csharp/linq/get-started/introduction-to-linq-queries){: target="_blank"}