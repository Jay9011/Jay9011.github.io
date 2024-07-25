---
title: SQLParameter 클래스와 파라미터
date: 2024-07-25 20:35:12 +0900
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
  - SQLParameter
  - SQL
---

ASP.NET과 ADO.NET에서 SQL의 파라미터는 SQL 쿼리에서 값들을 안전하고 효율적으로 다루기 위한중요한 개념입니다.

일반적으로 문자열을 연결하여 SQL문을 작성하는 것은 SQL Injection (SQL문을 해킹) 등의 문제를 발생시킬 수 있으므로, 파라미터가 있는 경우 SqlParameter를 사용하는 것이 권장됩니다.

SqlParameter 클래스는 [SqlCommand](/posts/ado.net%EA%B3%BC-%EA%B5%AC%EC%84%B1-%EC%9A%94%EC%86%8C/#command-%EA%B0%9D%EC%B2%B4){: target="_blank" } 객체에 파라미터가 필요한 경우 사용되는 클래스입니다.

> SQL 인젝션은 악의적인 사용자가 SQL 쿼리에 임의의 SQL 코드를 삽입하여 데이터베이스를 조작하는 공격 방식입니다. 
> <br>
> SQL 파라미터를 사용하면 이러한 공격을 방지할 수 있습니다.
> <br>
> 파라미터화된 쿼리는 입력값을 별도로 처리하므로, 입력값이 쿼리의 일부로 실행되지 않습니다.
> 
{: .prompt-warning }

### SQL 인젝션에 취약한 코드

다음은 SQL 인젝션 공격에 취약한 코드의 예입니다.

```csharp
string userInput = "john'; DROP TABLE Users; --";
string query = "SELECT * FROM Users WHERE UserName = '" + userInput + "'";

using (SqlCommand command = new SqlCommand(query, connection))
{
    using (SqlDataReader reader = command.ExecuteReader())
    {
        while (reader.Read())
        {
            Console.WriteLine(reader["UserName"]);
        }
    }
}
```

이 코드는 `userInput` 값에 의해 SQL 인젝션 공격이 가능해집니다.

`userInput`이 "john'; DROP TABLE Users; --"와 같은 값을 가지면, 원래 쿼리와 함께 Users 테이블을 삭제하는 명령이 실행될 수 있습니다.

# SQL 파라미터란?

SQL 파라미터는 SQL 쿼리 내에서 <span class="font_highlight">직접 값을 포함하는 대신, **값을 대체할 수 있는 플레이스홀더**를 사용하는 방식</span>입니다.

플레이스홀더는 실제 값으로 대체되기 전에 명확히 정의되는 변수입니다. 이를 통해 SQL 쿼리를 보다 동적이고 유연하게 만들 수 있습니다.

## SqlParameter 객체

`SqlParameter` 객체는 ADO.NET에서 SQL 명령을 실행할 때 사용되는 파라미터를 나타내는 클래스입니다.

이 객체는 SQL 쿼리나 저장 프로시저에 값을 전달할 때 사용되며, SQL 인젝션 공격을 방지하는 데 중요한 역할을 합니다.

## SqlParameter 객체의 주요 구성 요소

- **ParameterName**: 파라미터의 이름을 지정합니다. SQL 쿼리 내에서 파라미터를 참조할 때 사용됩니다.
- **SqlDbType**: 파라미터의 데이터 타입을 지정합니다. SQL Server 데이터베이스에서 사용하는 데이터 타입을 나타냅니다.
- **Size**: 파라미터의 크기(길이)를 지정합니다. 주로 문자열 데이터 타입에서 사용됩니다.
- **Value**: 파라미터에 할당된 값을 나타냅니다.
- **Direction**: 파라미터의 방향을 지정합니다. 입력, 출력, 입력/출력, 반환 값 중 하나입니다.
- **IsNullable**: 파라미터가 NULL 값을 허용하는지 여부를 나타냅니다.
- **Precision**: 숫자 데이터 타입의 경우, 전체 자릿수를 지정합니다.
- **Scale**: 숫자 데이터 타입의 경우, 소수점 이하 자릿수를 지정합니다.

# SQL 파라미터 사용 방법

## @ 로 시작되는 파라미터

SqlParameter를 사용하기 위해서는 우선 **SQL문 안**에 <span class="font_highlight">변수로 값을 넣고 싶은 곳에 `@`로 시작되는 파라미터(예: @UserName)를 추가</span>해 줍니다.

그 다음 **SqlParamter 객체**를 생성하여 파라미터명 및 타입, 사이즈 그리고 값을 넣어 준다. 그리고 마지막으로 이 SqlParameter 객체를 SqlCommand객체의 Parameters 컬렉션 속성에 추가해 주면 된다.

## SqlParameter 객체에 데이터 추가

가장 기본적인 사용 방법은 `SqlParameter` 객체를 생성하고, 파라미터 이름, 데이터 타입, 값을 설정하는 것입니다.

(파라미터의 데이터 타입은 다양하게 존재하므로, `SqlDbType` Enum을 참조해 주세요.)

```csharp
string connectionString = "your_connection_string";
using (SqlConnection connection = new SqlConnection(connectionString))
{
    connection.Open();
    string query = "INSERT INTO Users (UserName, Age) VALUES (@UserName, @Age)";

    using (SqlCommand command = new SqlCommand(query, connection))
    {
        // 문자열 파라미터
        command.Parameters.Add(new SqlParameter("@UserName", SqlDbType.NVarChar, 50) { Value = "John Doe" });
        // 정수형 파라미터
        command.Parameters.Add(new SqlParameter("@Age", SqlDbType.Int) { Value = 30 });

        command.ExecuteNonQuery();
    }
}
```

# 다양한 파라미터 추가 방법

파라미터를 추가하는 방법은 `Add()`, `AddWithValue()`, `AddRange()`를 사용하는 방법이 있습니다.

### SqlParameterCollection.AddWithValue() 메서드 사용

`AddWithValue()` 메서드는 매우 간단한 방법으로, 데이터 타입을 명시적으로 지정하지 않습니다.

`AddWithValue()` 메서드는 내부적으로 SqlParameter 객체를 생성하여 `SqlCommand.Parameters` 컬렉션에 추가하는 역할을 하므로, SqlParameter를 명시적으로 생성하지 않아도 됩니다.

```csharp
command.Parameters.AddWithValue("@UserName", "John Doe");
command.Parameters.AddWithValue("@Age", 30);
```

### SqlParameterCollection.Add() 메서드 사용

`Add()` 메서드를 사용하여 파라미터를 추가할 수 있습니다. 이 방법은 `SqlParameter` 객체를 직접 전달합니다.

```csharp
command.Parameters.Add(new SqlParameter("@UserName", SqlDbType.NVarChar, 50) { Value = "John Doe" });
command.Parameters.Add(new SqlParameter("@Age", SqlDbType.Int) { Value = 30 });
```

### SqlParameterCollection.AddRange() 메서드 사용

여러 개의 파라미터를 한 번에 추가할 때 `AddRange()` 메서드를 사용할 수 있습니다.

```csharp
SqlParameter[] parameters = {
    new SqlParameter("@UserName", SqlDbType.NVarChar, 50) { Value = "John Doe" },
    new SqlParameter("@Age", SqlDbType.Int) { Value = 30 }
};

command.Parameters.AddRange(parameters);
```

# 파라미터 사용의 장점

- **보안성 향상**: SQL 인젝션 방지를 통해 데이터베이스의 보안성을 높입니다.
- **유지보수 용이성**: 파라미터화된 쿼리는 가독성이 좋고, 유지보수가 용이합니다.
- **유연성**: 동적인 SQL 쿼리를 보다 쉽게 작성할 수 있습니다.