---
title: 구조 분해 할당 (Destructuring Assignment)
date: 2024-06-27 19:35:41 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 변수
  - 편의성
math: false
type: javascript
keywords:
  - Destructuring Assignment
  - 구조 분해 할당
  - 배열
  - 객체
---

구조 분해 할당(Destructuring Assignment)은 <span class="font_highlight">**배열**이나 **객체**의 **속성을 분해**하여 개별 변수에 할당</span>할 수 있게 하는 기능입니다.
<br>
이 기능은 한 곳에서 변수들을 선언하고 할당 받을 수 있어서 코드의 가독성을 높이는 데 유용합니다.

# 배열 구조 분해 할당

배열 구조 분해 할당을 사용하면 배열의 **요소를 개별 변수에 할당**할 수 있습니다.

```javascript
let array = [1, 2, 3, 4];

// 배열 구조 분해 할당
let [first, second, third] = array;

console.log(first);  // 1
console.log(second); // 2
console.log(third);  // 3
```

#### 요소 건너뛰기

배열 구조 분해 할당을 사용할 때, **필요하지 않은 요소**는 건너뛸 수 있습니다.

```javascript
let array = [1, 2, 3, 4];

// 첫 번째와 네 번째 요소만 할당
let [first, , , fourth] = array;

console.log(first);  // 1
console.log(fourth); // 4
```

#### 기본값 지정

또한 해당 위치에 요소가 없는 경우, **기본값을 지정**할 수도 있습니다.

```javascript
let array = [1, 2];

// 기본값 할당
let [first, second, third = 3] = array;

console.log(first);  // 1
console.log(second); // 2
console.log(third);  // 3
```

# 객체 구조 분해 할당

객체 구조 분해 할당을 사용하면 객체의 **속성을 개별 변수에 할당**할 수 있습니다.

```javascript
let person = {  
    name: '홍길동',  
    age: 16,  
    address: '서울',  
    job: '학생'  
};

// 객체 구조 분해 할당
let { name, age, address, job = '백수' } = person;

console.log(name);  // 홍길동
console.log(age);   // 16
console.log(address);  // 서울
console.log(job);   // 학생
```

#### 다른 이름으로 저장

변수 **이름**을 객체의 속성과 **다르게 지정**할 수도 있습니다.

```javascript
let person = {  
    name: '홍길동',  
    age: 16,  
    address: '서울',  
    job: '학생'  
};

// 다른 변수 이름으로 할당
let { name: personName, age: personAge } = person;

console.log(personName);  // 홍길동
console.log(personAge);   // 16
```

#### 기본값 지정

**기본값**을 지정할 수도 있습니다.

```javascript
let person = {  
    name: '홍길동',  
    age: 16,  
    address: '서울'
};

// 기본값 할당
let { name, age, address, job = '백수' } = person;

console.log(job);  // 백수
```

#### 중첩된 객체

**중첩된 객체**의 구조 분해 할당도 가능합니다.

```javascript
let person = {  
    name: '홍길동',  
    age: 16,  
    address: {  
    city: '서울',  
    dong: '역삼동'  
	}
};

// 중첩된 객체 구조 분해 할당
let {name, age, address: {city, dong}} = person; // 객체의 프로퍼티를 변수로 사용

console.log(name); // 홍길동
console.log(city); // 서울  
console.log(dong); // 역삼동
```

#### 매개변수에서 사용

또한 구조 분해 할당은 함수의 **매개변수에서도 사용**할 수 있습니다.

```javascript
let person = {  
    name: '홍길동',  
    age: 16,  
    address: '서울'
};

// 함수 매개변수 구조 분해 할당
function print({name, age, address, job = '백수'}) {
    console.log(`${name}은 ${address}에 사는 ${age}살 ${job}입니다.`);  
}

print(person); // 홍길동은 서울에 사는 16살 학생입니다.
```