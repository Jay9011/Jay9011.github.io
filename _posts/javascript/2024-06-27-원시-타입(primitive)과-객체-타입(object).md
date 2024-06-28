---
title: 원시 타입(Primitive)과 객체 타입(Object)
date: 2024-06-27 23:15:55 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 값 타입
  - 데이터
  - 메모리
  - 스택
  - 참조
  - 참조 타입
  - 힙
math: false
type: javascript
keywords:
  - Deep Equality
  - Object Types
  - Primitive Types
  - Reference Equality
  - Shallow Copy
  - 객체 타입
  - 깊은 복사
  - 깊은 비교
  - 얕은 복사
  - 원시 타입
  - 참조 비교
  - Deep Copy
---

자바스크립트는 데이터 타입을 크게 **원시 타입(Primitive Types)** 과 **객체 타입(Object Types)** 으로 나눌 수 있습니다.

원시 타입은 변경 불가능한 값으로 단순한 데이터를 표현하며, 객체 타입은 변경 가능한 복합 데이터 구조를 나타냅니다.

# 원시 타입(Primitive Types)

원시 타입은 기본 자료형 타입이며, 변경 불가능한 값입니다. 더 이상 나눌 수 없는 단일 값이라고도 합니다.

| 원시 타입     | 설명                                                     |
| --------- | ------------------------------------------------------ |
| Number    | 정수와 실수를 모두 포함하는 숫자 타입입니다. 예: `42`, `3.14`              |
| String    | 문자의 시퀀스(문자열)를 나타냅니다. 예: `"hello"`, `'world'`           |
| Boolean   | 참(`true`) 또는 거짓(`false`)을 나타내는 논리적 값입니다.               |
| null      | 값이 없음을 명시적으로 나타내는 값입니다.                                |
| BigInt    | 임의 정밀도의 정수를 표현하기 위한 타입입니다. 예: `12345678901234567890n`  |
| undefined | 값이 할당되지 않은 변수를 나타냅니다.                                  |
| Symbol    | 고유하고 변경 불가능한 원시 값입니다. 주로 객체의 고유한 프로퍼티 키를 만들기 위해 사용됩니다. |

Symbol은 다른 원시 타입과는 다르게 생성될 때마다 고유한 값을 가지기 때문에, 같은 문자열을 사용하여 생성된 두 개의 Symbol은 서로 다릅니다.

이는 Symbol이 객체의 고유한 프로퍼티 키를 만들기 위해 설계되었기 때문입니다. 따라서 Symbol은 유일무이한 식별자를 만드는 데 사용됩니다.

```javascript
let sym1 = Symbol('description');
let sym2 = Symbol('description');

console.log(sym1 === sym2); // false
```

# 객체 타입(Object Types)

객체 타입은 여러 값을 포함할 수 있는 복합 자료형으로, 다양한 프로퍼티와 메소드를 가질 수 있습니다.

| 객체 타입    | 설명                                                                   |
| -------- | -------------------------------------------------------------------- |
| Object   | 키-값 쌍의 집합입니다. 예: `{ name: 'John', age: 30 }`                         |
| Array    | 순서가 있는 값의 집합입니다. 예: `[1, 2, 3, 'four', true]`                        |
| Function | 실행 가능한 코드 블록이며, 객체의 일종입니다. 예: `function add(a, b) { return a + b; }` |
| Date     | 날짜와 시간을 표현하기 위한 내장 객체입니다. 예: `new Date()`                            |
| RegExp   | 패턴 매칭과 텍스트 검색 및 조작을 위한 정규 표현식 객체입니다. 예: `/\d+/`                      |

# 원시 타입과 객체 타입의 주요 차이점

1. **변경 가능성**
   - 원시 타입은 **불변(immutable)** 합니다. 값을 변경하려면 새로운 값을 할당해야 합니다.
   - 객체 타입은 **변경 가능(mutable)** 합니다. 객체의 프로퍼티를 변경할 수 있습니다.
2. **저장 방식**
   - 원시 타입의 값은 일반적으로 **스택(stack)** 에 저장되고, 이는 크기가 작고 관리가 용이하기 때문입니다.
   - 객체 타입의 값은 **힙(heap)** 에 저장되고, <span class="font_highlight">스택에는 **힙의 주소(reference)** 가 저장</span>됩니다.
3. **할당 및 전달 방식**
   - 원시 타입은 **값 자체가 복사**됩니다.
   - 객체 타입은 **참조(reference)가 복사**됩니다. 따라서 <span class="font_highlight">참조를 공유하는 변수들을 통해 객체의 상태를 변경</span>할 수 있습니다.

## 원시 타입의 값 할당과 값 변경 과정

원시 타입의 값은 일반적으로 **스택에 저장**됩니다.

불변(immutable) 타입이기 때문에, 값을 변경하려면 **새로운 값을 할당**해야 하며, 기존 값은 변경되지 않고 새로운 값이 새로운 메모리 공간에 저장됩니다.

또한, 원시 타입은 **값 자체가 복사**되기 때문에 새로운 변수에 할당하더라도 복사된 값이 저장되고, 따라서 서로 다른 값을 가지게 됩니다.

```javascript
let a = 10; // 스택에 10을 저장하고 a가 참조.  
let b = a; // a의 값인 10을 b에 복사한다.  
a = 20; // 새로운 값 20을 스택의 다른 위치에 저장되고, a는 이제 20을 참조.  
  
console.log(a); // 20  
console.log(b); // 10
```

![원시 타입 메모리 할당](https://i.postimg.cc/25h6734H/image.webp){: w="721" h="174"}

## 참조 타입의 값 할당과 값 변경 과정

객체 타입의 값은 **힙에 저장**되고, **스택에는 힙의 주소가 저장**됩니다.

객체 타입은 **참조(reference)가 복사**되기 때문에, 객체를 복사하면 참조 주소가 복사되고, 이로 인해 동일한 힙 주소를 참조하게 됩니다.

따라서, 동일한 객체를 가리키는 변수를 수정하게 되면, 같은 객체를 가리키는 다른 변수의 결과도 바뀌게 됩니다.

```javascript
let obj1 = {num : 10}; // 힙에 {num : 10}을 저장하고, 스택에서 obj1이 힙의 주소를 참조.  
let obj2 = obj1; // obj1의 주소를 obj2에 복사한다.  
obj1.num = 20; // 힙에 저장된 {num : 10}의 값을 20으로 변경한다.  
  
console.log(obj1.num); // 20  
console.log(obj2.num); // 20 : obj1을 통해 수정된 값이 obj2에도 반영된다.
```

![객체 타입 메모리 할당](https://i.postimg.cc/pLgrjKnh/image.webp){: w="550" h="174"}

## 예제: 함수 인자로 전달된 객체 타입

함수에 객체를 인자로 전달할 때도 참조가 전달됩니다.
<br>
함수 내에서 객체의 프로퍼티를 변경하면 원본 객체가 수정됩니다.

```javascript
let obj3 = {num : 10};  
function changeValue(param) {  
    param.num = 30; // param이 참조하는 주소의 객체 값을 변경한다.  
}  
changeValue(obj3);  

console.log(obj3.num); // 30
```

# 깊은 복사와 얕은 복사

객체 타입을 복사할 때 얕은 복사(Shallow Copy) 와 깊은 복사(Deep Copy) 두 가지 주요 방식이 있습니다.
<br>
이 두 방식은 복사된 객체의 프로퍼티가 원본 객체와 어떻게 연결되는지에 따라 다릅니다.

## 의도치 않은 변경 발생

만약, 같은 템플릿을 사용하고 싶어서 객체의 형태를 미리 만들어두고, 이를 복사하여 사용하는 경우.

같은 힙 메모리를 참조하게 되어 의도치 않은 변경이 발생할 수 있습니다.

```javascript
let address = { city: '서울', dong: '역삼동' };  
let person1 = { name: '홍길동', age: 16, address: address };  
let person2 = { name: '김철수', age: 21, address: address };  
  
// 각 사람의 실제 주소를 반영하려고 시도  
person1.address.city = '인천';  
person1.address.dong = '마전동';  
person2.address.city = '부산';  
person2.address.dong = '광안동';  
  
console.log(person1); // { name: '홍길동', age: 16, address: { city: '부산', dong: '광안동' } }  
console.log(person2); // { name: '김철수', age: 21, address: { city: '부산', dong: '광안동' } }
```

## 얕은 복사 (Shallow Copy)

위 예제와 같은 복사를 얕은 복사라고 합니다.

얕은 복사는 객체의 최상위 레벨 프로퍼티만 복사하며, 중첩된 객체(즉, 객체 내의 객체)들은 <span class="font_highlight">원본 객체와 동일한 참조</span>를 가집니다.

이렇게 하면 두 객체는 서로 다른 객체처럼 보이지만, 실제로 안에 있는 중첩된 객체는 서로 같은 객체를 가지게 됩니다.

## 깊은 복사 (Deep Copy)

깊은 복사는 <span class="font_highlight">객체의 모든 레벨을 복사하여 원본 객체와 완전히 독립적인 복사본을 생성</span>합니다.

중첩된 객체도 모두 새로 생성되기 때문에, 복사된 객체는 원본 객체와 참조를 공유하지 않습니다.

깊은 복사를 구현하는 명확한 표준 방법은 없으나, 다양한 방법으로 구현할 수 있습니다.

예를 들어, JSON.parse()나 JSON.stringify()를 이용하는 방법이나, 재귀적 복사 함수 작성하는 방법, 객체의 깊이를 알 수 있는 경우 [Spread 연산자](/posts/%EC%8A%A4%ED%94%84%EB%A0%88%EB%93%9C(spread)%EC%99%80-%EB%A0%88%EC%8A%A4%ED%8A%B8(rest)-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98/){: target="_blank"}를 사용하여 원시 타입 레벨로 복사하는 방법 등이 있습니다.

```javascript
// Spread 연산자를 사용하여 해결
let address = { city: '서울', dong: '역삼동' };
let person1 = { name: '홍길동', age: 16, address: { ...address } };
let person2 = { name: '김철수', age: 21, address: { ...address } };

// 각 사람의 실제 주소를 반영하려고 시도
person1.address.city = '인천';
person1.address.dong = '마전동';
person2.address.city = '부산';
person2.address.dong = '광안동';

console.log(person1); // { name: '홍길동', age: 16, address: { city: '인천', dong: '마전동' } }
console.log(person2); // { name: '김철수', age: 21, address: { city: '부산', dong: '광안동' } }
```

위 예제는 객체의 깊이를 알 수 있기 때문에, Spread 연산자를 통해 원시 타입 레벨로 복사해서 각각 서로 다른 객체를 만들어 주었습니다.

# 객체 타입 비교

자바스크립트에서 객체 타입의 비교는 원시 타입의 비교와 다릅니다. 원시 타입의 값은 실제 값 자체를 비교하지만, <span class="font_highlight">객체 타입은 참조(Reference)를 비교</span>합니다.
<br>
즉, 두 객체가 동일한 메모리 주소를 가리키는지를 확인합니다.

```javascript
let obj4 = { num: 10 };
let obj5 = { num: 10 };
let obj6 = obj4;

console.log(obj4 === obj5); // false : 서로 다른 주소를 참조하고 있기 때문에 false
console.log(obj4 === obj6); // true : obj6은 obj4와 같은 주소를 참조하고 있기 때문에 true
```

## 객체 비교의 두 가지 주요 방식

### 1. 참조 비교 (Reference Equality)

위 예제의 방식은 참조 비교라고 합니다.

참조 비교는 <span class="font_highlight">객체의 메모리 주소(참조)를 비교</span>합니다.

### 2. 깊은 비교 (Deep Equality)

만약, 의도한 비교가 두 수가 같은지 비교하려는 것이라면(혹은 구성 요소의 값이 같은지 비교하려는 것이라면) 깊은 비교를 해야 합니다.

깊은 비교는 객체의 <span class="font_highlight">구조와 내용을 재귀적으로 비교하여 두 객체가 동일한 데이터를 가지는지 확인</span>하는 것입니다.

자바스크립트는 기본적으로 깊은 비교를 제공하지 않으므로, 이를 비교하기 위해서는 직접 구현하거나 JSON.stringify() 등을 이용해서 구현해야 합니다.

```javascript
let obj4 = { num: 10 };
let obj5 = { num: 10 };
let obj6 = obj4;

console.log(JSON.stringify(obj4) === JSON.stringify(obj5)); // true : JSON.stringify()를 사용하여 객체를 문자열로 변환한 뒤 비교
```

---

# 참고

[스프레드(Spread)와 레스트(Rest) 매개변수](/posts/%EC%8A%A4%ED%94%84%EB%A0%88%EB%93%9C(spread)%EC%99%80-%EB%A0%88%EC%8A%A4%ED%8A%B8(rest)-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98/){: }