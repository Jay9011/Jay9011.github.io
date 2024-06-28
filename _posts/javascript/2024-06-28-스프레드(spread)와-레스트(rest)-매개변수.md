---
title: 스프레드(Spread)와 레스트(Rest) 매개변수
date: 2024-06-28 13:28:02 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
math: false
type: javascript
keywords:
  - Spread
  - 레스트
  - 스프레드
  - Rest
---

JavaScript의 Spread 연산자와 Rest 매개변수는 ES6(ECMAScript 2015)에서 도입된 문법으로, 둘 다 세 개의 점(`...`)으로 표기되지만, 용도와 동작 방식이 다릅니다.

# Spread 연산자

Spread 연산자(`...`)는 배열이나 문자열과 같이 <span class="font_highlight">반복 가능한(iterable) 객체를 각각의 요소로 분해하여 각 요소를 개별적으로 다루는데 사용</span>하는 연산자입니다.
<br>
반복 가능한(iterable) 객체에는 배열, 문자열, 맵(Map), 셋(Set) 등이 포함됩니다.

## 배열에서의 Spread 연산자

Spread 연산자는 배열을 복사하거나 확장하여 새로운 배열을 만들 수 있습니다. 예를 들어, 두 배열을 합치는 경우 다음과 같이 사용할 수 있습니다.

```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];

console.log(combined); // [1, 2, 3, 4, 5, 6]
```

함수 호출 시 배열을 개별 인수로 전달하려는 경우에도 사용할 수 있습니다.

```javascript
function add(a, b, c) {
    return a + b + c;
}

const numbers = [1, 2, 3];
console.log(add(...numbers)); // 6
```

## 객체에서의 Spread 연산자

### 객체 Spread 연산자와 iterable Spread 연산자

원래 일반 object 객체는 Iterable 객체가 아니기 때문에 Spread 연산자를 사용할 수 없습니다.

다만, ES2018부터는 객체 리터럴에서도 Spread 연산자를 사용할 수 있게 되었는데, 저는 이것을 객체 Spread 연산자라고 따로 부르기도 합니다.(공식 용어가 아닙니다.)

객체 Spread 연산자는 객체 리터럴 내에서 사용하는 Spread 연산자로, <span class="font_highlight">객체 리터럴 내에서 사용할 때 객체의 속성을 복사</span>할 수 있습니다

이 경우, 다른 객체의 key-value 쌍을 현재 객체 리터럴에 복사하는데, 예를 들어, `{ ...job }`은 `job` 객체의 모든 속성을 새 객체에 복사합니다.

```javascript
let job = {  
    jobNum: 1,  
    jobName: '학생'  
};    
let person = {  
    uid: 1,  
    name: '홍길동',  
    age: 16,  
    address: '서울',  
    ...job  // job 속성 복사
};

console.log(person); // {uid: 1, name: "홍길동", age: 16, address: "서울", jobNum: 1, jobName: "학생"}
```

Spread 연산자를 객체에 직접 적용하려고 할 때 발생하는 문제를 보여주는 예제는 다음과 같습니다.

```javascript
let person2 = {  
    uid: 1,  
    name: '홍길동',  
    age: 16,  
    address: '서울'  
};  
// 일반 object 객체는 Iterable 객체가 아니기 때문에 Spread 연산자를 사용할 수 없다.  
console.log(...person2); // TypeError: person2 is not iterable
```

위 예제에서 발생한 `TypeError`는 `person2` 객체가 반복 가능하지 않기 때문에 Spread 연산자를 사용할 수 없음을 보여줍니다.

### 얕은 복사와 깊은 복사

그러나 객체 Spread 연산자는 **얕은 복사(shallow copy)** 를 수행하기 때문에 중첩된 객체가 있을 경우 주의가 필요합니다.

만약, 아래와 같이 person1의 속성을 복사하기 위해 객체 Spread 연산자를 사용했다면, 다음과 같은 의도치 않은 변경이 발생할 수 있습니다.

```javascript
let address = { city: '서울', dong: '역삼동' };  
let person1 = { name: '홍길동', age: 16, address: address };  
let person2 = { ...person1 };  
  
person1.name = '김철수';  
person1.age = 20;  
person1.address.city = '인천';  
person1.address.dong = '마전동';  
  
// person1에 대한 변경만 시도했으나 person2도 같이 변경되어 의도치 않은 변경이 발생하게 된다.  
console.log(person1); // { name: '김철수', age: 20, address: { city: '인천', dong: '마전동' } }  
console.log(person2); // { name: '홍길동', age: 16, address: { city: '인천', dong: '마전동' } }
```

자세한 내용은 [원시 타입과 객체 타입 글](/posts/%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85(primitive)%EA%B3%BC-%EA%B0%9D%EC%B2%B4-%ED%83%80%EC%9E%85(object)/){: target="_blank"}의 [깊은 복사와 얕은 복사](/posts/%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85(primitive)%EA%B3%BC-%EA%B0%9D%EC%B2%B4-%ED%83%80%EC%9E%85(object)/#%EA%B9%8A%EC%9D%80-%EB%B3%B5%EC%82%AC%EC%99%80-%EC%96%95%EC%9D%80-%EB%B3%B5%EC%82%AC){: target="_blank"} 파트에서 다루었기 때문에 자세히 다루지는 않겠습니다.

# Rest 매개변수

Rest 매개변수(`...`)는 함수 매개변수 목록에서 <span class="font_highlight">**나머지 인수**들을 배열로 수집</span>하거나, <span class="font_highlight">구조 분해 할당에서 **나머지 요소**들을 배열이나 객체로 수집</span>하는 데 사용됩니다.

## 함수에서 나머지 인수를 수집

Rest 매개변수(`...`)는 함수의 매개변수 목록에서 **나머지 인수**들을 배열로 수집하는 데 사용됩니다. 이는 가변 인수를 처리하는 함수에서 유용하게 사용할 수 있습니다.

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15
```

이때, Rest 매개변수는 나머지 인수를 수집하는 매개변수이기 때문에, <span class="font_highlight">함수 매개변수의 마지막 위치에 존재</span>해야 합니다.

```javascript
let arr2 = [1, 2, 3, 4, 5];
let print = (a, b, ...rest) => {    // ...rest 는 나머지 인수들을 전달 받는 역할이기 때문에 마지막 매개변수로 사용해야 한다.  
    console.log(a); // 1  
    console.log(b); // 2  
    console.log(rest); // [3, 4, 5]  
}  
  
print(...arr2);
```

## 구조 분해 할당에서의 Rest 매개변수

Rest 매개변수를 배열이나 객체의 [구조 분해 할당](/posts/%EA%B5%AC%EC%A1%B0-%EB%B6%84%ED%95%B4-%ED%95%A0%EB%8B%B9-(destructuring-assignment)/){: target="_blank"}에서도 사용할 수 있습니다.

이때, 배열 또는 객체에서 일부 요소를 분해 할당으로 추출한 후, 나머지를 다시 배열이나 객체로 그룹화할 때 사용됩니다.

```javascript
// 배열 분해 할당에서의 Rest 매개변수 사용
let arr3 = [1, 2, 3, 4, 5];
let [a, b, ...rest] = arr3;

console.log(a); // 1
console.log(b); // 2
console.log(rest); // [3, 4, 5]

// 객체 분해 할당에서의 Rest 매개변수 사용
let person3 = {
    uid: 1,
    name: '홍길동',
    age: 16,
    address: '서울'
};
let { uid, name, ...rest2 } = person3;

console.log(uid); // 1
console.log(name); // 홍길동
console.log(rest2); // { age: 16, address: '서울' }
```

# Spread 연산자와 Rest 매개변수 정리

1. **역할**
	- `Spread 연산자`는 배열이나 객체의 요소를 각각의 요소로 분해하여 각 요소를 개별적으로 다루는데 사용합니다.
  - `Rest 매개변수`는 함수 매개변수 목록에서 나머지 인수들을 배열로 수집하거나 분해 할당 된 요소의 나머지 요소들을 배열이나 객체로 수집하는데 사용합니다.
2. **사용 위치**
	- `Spread 연산자`는 배열 리터럴, 객체 리터럴, 함수 호출 등에서 사용됩니다.
  - `Rest 매개변수`는 함수 매개변수 목록과 배열/객체 분해 할당에서 사용됩니다.

---

# 참고

[구조 분해 할당 (Destructuring Assignment)](/posts/%EA%B5%AC%EC%A1%B0-%EB%B6%84%ED%95%B4-%ED%95%A0%EB%8B%B9-(destructuring-assignment)/){: }

[원시 타입(Primitive)과 객체 타입(Object)](/posts/%EC%9B%90%EC%8B%9C-%ED%83%80%EC%9E%85(primitive)%EA%B3%BC-%EA%B0%9D%EC%B2%B4-%ED%83%80%EC%9E%85(object)/){: }