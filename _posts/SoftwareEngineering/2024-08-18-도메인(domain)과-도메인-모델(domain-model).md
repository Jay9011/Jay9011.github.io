---
title: 도메인(Domain)과 도메인 모델(Domain Model)?
date: 2024-08-18 10:03:13 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - Model
  - 객체 지향
  - 소프트웨어 공학
  - 추상화
math: false
keywords:
  - Domain
  - 도메인
  - 도메인 모델
  - Domain Model
---

# 도메인(Domain)이란?

Domain이라는 영단어를 네이버 영어사전에서 찾아보면 다음과 같은 뜻을 볼 수 있습니다.

> 
> (지식·활동의) 영역(분야); (책임의) 범위
> ['domain': 네이버 영어사전](https://en.dict.naver.com/#/entry/enko/1fd5632fb4c340439274adca38f240bd){: target="_blank"}
> 
{: .prompt-info }

도메인(Domain)은 소프트웨어 개발에서 <span class="font_highlight">특정 애플리케이션이 다루는 **문제 영역**이나 **비즈니스 영역**을 의미</span>합니다.

쉽게 말해, <span class="font_highlight">소프트웨어가 해결하고자 하는 실제 세계의 **주제**나 **분야**</span>를 말합니다.

문제는, 이 과정에서 주제나 분야는 크고 복잡한 문제에서 시작해, 더 작고 단순한 문제로 나눌 수 있다는 점입니다.

이때 생겨난 큰 문제와 작은 문제 모두 **큰 도메인**과 **하위 도메인**으로 나뉠 수 있습니다.

즉, 큰 문제도 도메인이며, 작은 문제 또한 도메인으로 볼 수 있습니다.

예를 들어, 온라인 쇼핑몰 도메인을 나누는 과정은 다음과 같이 해볼 수 있습니다.

**온라인 쇼핑몰 도메인**

- **문제 영역:** 사람들이 인터넷을 통해 상품을 사고파는 과정. (온라인 쇼핑몰)
- **주요 개념:** 고객, 상품, 주문, 결제, 배송 등…
- **비즈니스 규칙:** 고객이 상품을 장바구니에 담을 수 있어야 하고, 결제 후 주문이 생성되어야 하고, 주문된 상품은 배송되어야 한다.

- **큰 도메인(Main Domain):** 온라인 쇼핑몰
- **하위 도메인(Subdomains):**
    - **고객 관리(User Management):** 사용자 등록, 로그인, 프로필 관리 등…
        - 사용자, 등…
    - **상품 관리(Product Management):** 상품 등록, 수정, 삭제, 카테고리 관리 등…
    - **주문 관리(Order Management):** 주문 생성, 주문 상태 추적, 결제 처리 등…
    - **배송 관리(Shipping Management):** 배송 추적, 배송 상태 업데이트, 배송 주소 관리 등…

![쇼핑몰 도메인](https://i.postimg.cc/grbjX6mg/Domain.png){: w="970" h="516"}

# 도메인 모델(Domain Model)이란?

도메인 모델(Domain Model)이란, 소프트웨어 개발에서 <span class="font_highlight">**특정 도메인**(문제 영역)의 주요 개념, 규칙, 프로세스, 그리고 이들 간의 관계를 **추상화**하여 **표현**한 **개념**적인 모델</span>입니다.

쉽게 말해, 실제 세계의 개념, 프로세스, 규칙, 데이터 등을 <span class="font_highlight">**객체 지향적인 방식**으로 표현한 것</span>입니다.

도메인 모델 구성 요소의 예로는 다음과 같습니다.

1. **엔티티(Entity):** 고유 식별자를 가지고 있으며, 도메인 내에서 독립적으로 존재할 수 있는 객체입니다. 예를 들어, '사용자(User)'나 '제품(Product)' 등이 엔티티에 해당합니다.
2. **값 객체(Value Object):** 고유 식별자가 없고, 주로 엔티티의 속성을 표현하는 데 사용됩니다. 예를 들어, '주소(Address)'나 '날짜(Date)' 등이 값 객체에 해당합니다.
3. **집합체(Aggregate):** 여러 엔티티와 값 객체를 하나의 단위로 묶어 관리하는 집합입니다. 집합체의 루트 엔티티를 통해서만 접근이 가능합니다.
4. **리포지토리(Repository):** 도메인 객체의 영속성을 관리하는 인터페이스입니다. 데이터베이스와 같은 영속 저장소와 도메인 객체 간의 중개 역할을 합니다.
5. **서비스(Service):** 특정 도메인 로직을 캡슐화하여 제공하는 객체입니다. 도메인 서비스는 엔티티나 값 객체의 메서드로 표현하기 어려운 도메인 로직을 포함합니다.

```csharp
// 엔티티
public class User
{
    public int Id { get; set; }
    public string Name { get; set; }
    public Address Address { get; set; }
}

// 값 객체
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string PostalCode { get; set; }

    public Address(string street, string city, string postalCode)
    {
        Street = street;
        City = city;
        PostalCode = postalCode;
    }
}

// 리포지토리 인터페이스
public interface IUserRepository
{
    User GetUserById(int id);
    void SaveUser(User user);
}

// 도메인 서비스
public class UserService
{
    private readonly IUserRepository _userRepository;

    public UserService(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public void ChangeUserAddress(int userId, Address newAddress)
    {
        var user = _userRepository.GetUserById(userId);
        if (user != null)
        {
            user.Address = newAddress;
            _userRepository.SaveUser(user);
        }
    }
}
```

이 예시에서는 `User`가 엔티티, `Address`가 값 객체, `IUserRepository`가 리포지토리, `UserService`가 도메인 서비스 역할을 합니다.

도메인 모델을 통해 비즈니스 로직을 명확하게 표현하고, 이를 중심으로 애플리케이션을 설계할 수 있습니다.

결국, 도메인(Domain)은 우리가 해결하고자 하는 문제의 범위와 내용을 정의하고, 도메인 모델(Domain Model)은 이를 소프트웨어 내에서 명확하고 체계적으로 표현하여 구현하는 방법을 말합니다.