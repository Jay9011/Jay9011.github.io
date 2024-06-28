---
title: JavaScript 배열 순회 및 조작 메서드(forEach, map, filter, join)
date: 2024-06-28 17:15:23 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 함수
math: false
type: javascript
keywords:
  - forEach
  - map
  - filter
  - join
  - JavaScript
  - 배열
  - 순회
  - 조작
---

| 메서드       | 사용 범위 | 설명                                   |
| --------- | ----- | ------------------------------------ |
| `forEach` | 배열    | 배열의 **각 요소에 대해 함수를 실행**하는 메서드        |
| `map`     | 배열    | 배열의 **각 요소를 변환하여 새로운 배열을 생성**하는 메서드  |
| `filter`  | 배열    | **조건을 만족하는 요소들만으로 새로운 배열**을 생성하는 메서드 |
| `join`    | 배열    | 배열의 모든 요소를 **문자열**로 결합하는 메서드         |

# forEach 메서드

`forEach` 메서드는 배열의 <span class="font_highlight">**각 요소**에 대해 제공된 **콜백 함수**를 한 번씩 실행하는 반복문 메서드</span>입니다.

다른 배열 메서드와 달리, `forEach`는 항상 `undefined`를 반환하며, 배열을 변경하지 않습니다. 단, 콜백 함수 내에서 배열의 요소를 수정하는 경우에는 수정될 수 있습니다.

```javascript
array.forEach(callback(element, index, array), thisArg)
```

- `callback`: 배열의 각 요소에 대해 실행할 함수입니다. 아래 인수를 가집니다.
    - `element`: 처리할 현재 요소
    - `index`: 배열의 현재 요소의 인덱스
    - `array`: `findIndex`를 호출한 현재 배열 자체
- `thisArg` (선택사항): `callback` 함수 내부에서 `this`로 사용할 값입니다.

## 추가 설명

- `forEach`는 반환 값을 사용하지 않으므로 `return` 키워드를 사용해도 아무런 효과가 없습니다.
- `forEach`는 배열을 변경하지 않지만, 콜백 함수 내에서 배열의 요소를 수정할 수 있습니다.
- `forEach`는 동기적으로 동작하며, 콜백 함수가 배열의 각 요소에 대해 실행되는 순서는 배열의 인덱스 순서와 같습니다.

## 사용 예제

#### 단순 순회 예제

```javascript
const fruits = ['apple', 'banana', 'mango'];

fruits.forEach((fruit, index, array) => {
  console.log(`Index: ${index}, Element: ${fruit}, Array: ${array}`);
});
// 출력:
// Index: 0, Element: apple, Array: apple,banana,mango
// Index: 1, Element: banana, Array: apple,banana,mango
// Index: 2, Element: mango, Array: apple,banana,mango
```

이 예제에서는 `forEach` 메서드의 콜백 함수에서 요소(`fruit`), 인덱스(`index`), 배열(`array`)을 모두 사용하여 각 요소의 정보와 배열 전체를 출력합니다.

#### 배열의 요소를 수정하는 예제

```javascript
const numbers = [1, 2, 3, 4, 5];

numbers.forEach((number, index, array) => {
  array[index] = number * 2;
});

console.log(numbers); // [2, 4, 6, 8, 10]
```

이 예제에서는 `forEach` 메서드를 사용하여 배열의 각 요소를 두 배로 수정합니다. 콜백 함수에서 `array[index]`를 수정하여 배열 자체를 변경합니다.

## 주의사항

- `forEach`는 반복을 중단하지 않습니다. 중간에 반복을 종료해야 하는 경우에는 `every`, `some`, `find`, `findIndex` 등의 다른 메서드를 사용하는 것이 좋습니다.
- `forEach`는 원본 배열을 변경하는 메서드는 아니지만, 콜백 함수 내에서 원본 배열을 변경할 수 있습니다.
- `forEach`는 비동기 작업에서는 사용하기 적합하지 않을 수 있습니다. 비동기 작업이 필요할 경우 `map`과 `Promise.all`을 함께 사용하는 것이 좋습니다.

---

# map 메서드

`map` 메서드는 배열 내의 <span class="font_highlight">**각 요소**에 대해 주어진 콜백 함수를 호출하고, **그 결과를 모아 새로운 배열**을 반환</span>합니다. (원본 배열은 변경되지 않습니다.)

따라서 `map` 메서드는 배열의 각 요소를 변환하여 새로운 배열을 만들 때 유용합니다.

이때, `map` 메서드는 조건에 따라 배열을 생성하는 것이 아니기 때문에, 항상 원본 배열과 길이가 같은 새로운 배열을 반환하게 됩니다.

```javascript
array.map(callback(element, index, array), thisArg)
```

- `callback`: 배열의 각 요소에 대해 실행할 함수입니다. 아래 인수를 가집니다.
    - `element`: 처리할 현재 요소
    - `index`: 배열의 현재 요소의 인덱스
    - `array`: `findIndex`를 호출한 현재 배열 자체
- `thisArg` (선택사항): `callback` 함수 내부에서 `this`로 사용할 값입니다.

## 사용 예제

#### 배열의 각 요소를 제곱하여 새로운 배열 만들기

```javascript
const numbers = [5, 12, 8, 130, 44];
const squaredNumbers = numbers.map(number => number * number);  
  
console.log(numbers); // [5, 12, 8, 130, 44]  
console.log(squaredNumbers); // [25, 144, 64, 16900, 1936]
```

이 예제에서는 `map` 메서드를 사용하여 배열의 각 요소를 제곱한 새로운 배열을 생성합니다. 원본 배열 `numbers`는 변경되지 않고, `squaredNumbers`라는 새로운 배열이 생성됩니다.

#### 객체 배열에서 특정 속성 값 추출하기

```javascript
const users = [
  { name: 'John', age: 25 },
  { name: 'Jane', age: 28 },
  { name: 'Peter', age: 30 }
];

const names = users.map(user => user.name);

console.log(names); // ['John', 'Jane', 'Peter']
```

이 예제에서는 `map` 메서드를 사용하여 객체 배열에서 각 객체의 `name` 속성 값을 추출한 새로운 배열을 생성합니다.

## 주의사항

- `map` 메서드도 반복을 중단하지 않고 배열의 각 요소에 대해 콜백 함수가 실행됩니다.
- `map` 메서드는 원본 배열을 변경하지 않습니다. 배열의 각 요소를 변환하여 새로운 배열을 생성합니다.
- `map` 메서드는 배열의 길이와 동일한 길이의 새로운 배열을 반환합니다. 콜백 함수가 `undefined`를 반환하더라도 해당 인덱스의 요소는 `undefined`로 채워진 새로운 배열이 반환됩니다.

---

# filter 메서드

`filter` 메서드는 배열 내의 모든 요소를 주어진 **판별 함수**로 테스트하여, 그 <span class="font_highlight">**테스트**를 통과하는 모든 요소를 모아 **새로운 배열**로 반환</span>합니다. (원본 배열은 변경되지 않습니다.)

즉, `filter` 메서드는 콜백 함수가 `true`를 반환하는 요소들만을 가지고 새로운 배열을 반환하는 메서드입니다.

```javascript
array.filter(callback(element, index, array), thisArg)
```

- `callback`: 배열의 각 요소에 대해 실행할 함수입니다. 아래 인수를 가집니다.
    - `element`: 처리할 현재 요소
    - `index`: 배열의 현재 요소의 인덱스
    - `array`: `findIndex`를 호출한 현재 배열 자체
- `thisArg` (선택사항): `callback` 함수 내부에서 `this`로 사용할 값입니다.

## 사용 예제

#### 숫자 배열에서 짝수 요소만 추출하기

```javascript
const numbers = [5, 12, 8, 130, 44];
const evenNumbers = numbers.filter(number => number % 2 === 0);  
  
console.log(evenNumbers); // [12, 8, 130, 44]  
console.log(numbers); // [5, 12, 8, 130, 44]
```

이 예제에서는 `filter` 메서드를 사용하여 배열의 각 요소를 검사하고, 짝수인 요소들만 새로운 배열로 반환합니다. 원본 배열 `numbers`는 변경되지 않고, `evenNumbers`라는 새로운 배열이 생성됩니다.

#### 객체 배열에서 특정 속성 값이 있는 객체 추출하기

```javascript
const users = [
  { name: 'John', age: 25 },
  { name: 'Jane', age: 28 },
  { name: 'Peter', age: 30 },
  { name: 'Mary', age: 28 }
];

const usersInTwenties = users.filter(user => user.age >= 20 && user.age < 30);

console.log(usersInTwenties);
// [
//   { name: 'John', age: 25 },
//   { name: 'Jane', age: 28 },
//   { name: 'Mary', age: 28 }
// ]
```

이 예제에서는 `filter` 메서드를 사용하여 객체 배열에서 `age` 속성이 20대인 객체들만 추출한 새로운 배열을 생성합니다.

## 주의사항

- `filter` 메서드도 반복을 중단하지 않고 배열의 각 요소에 대해 콜백 함수가 실행됩니다.
- `filter` 메서드는 원본 배열을 변경하지 않습니다. 배열의 각 요소를 테스트하여 새로운 배열을 생성합니다.
- `filter` 메서드는 조건을 만족하지 않는 요소가 없을 경우 빈 배열을 반환합니다.

---

# join 메서드

`join` 메서드는 배열의 모든 요소를 <span class="font_highlight">**문자열로 변환, 결합**하여 하나의 문자열을 생성</span>합니다.
각 요소 사이에 지정된 구분자(separator)를 추가하여 문자열을 만듭니다. (구분자를 지정하지 않으면 기본적으로 쉼표(`,`)가 사용됩니다.)
이때, 배열의 요소가 `undefined`, `null`, 또는 빈 문자열일 경우, 해당 요소는 빈 문자열로 처리됩니다.

```javascript
array.join(separator)
```

- `separator` (선택사항): 배열의 각 요소를 구분할 문자열입니다. 기본값은 쉼표(`,`)입니다.

## 사용 예제

#### 기본 구분자 사용

```javascript
const elements = ['Fire', 'Air', 'Water'];
const result = elements.join();

console.log(result); // "Fire,Air,Water"
```

이 예제에서는 `join` 메서드가 구분자를 지정하지 않았기 때문에 기본값인 쉼표(`,`)를 사용하여 배열의 모든 요소를 하나의 문자열로 결합합니다.

#### 사용자 지정 구분자 사용

```javascript
const urlParts = ['https:', '', 'www.example.com', 'page'];

const url = urlParts.join('/');

console.log(url); // "https://www.example.com/page"
```

이 예제에서는 `join` 메서드에 구분자로 슬래시(`/`)을 지정하여 URL의 여러 부분을 결합하여 완전한 URL을 생성합니다.

## 주의사항

- 배열에 요소가 없으면 `join` 메서드는 빈 문자열을 반환합니다.
- 배열의 요소가 문자열이 아닌 경우, `join` 메서드는 요소를 문자열로 변환한 후 결합합니다.

---

# 참고


[JavaScript 배열 탐색 및 검사 메서드(includes,indexOf,findIndex,find)](/posts/javascript-%EB%B0%B0%EC%97%B4-%ED%83%90%EC%83%89-%EB%B0%8F-%EA%B2%80%EC%82%AC-%EB%A9%94%EC%84%9C%EB%93%9C(includes,-indexof,-findindex,-find)/){: }

[JavaScript 배열 정렬 메서드(sort, toSorted)](/posts/javascript-%EB%B0%B0%EC%97%B4-%EC%A0%95%EB%A0%AC-%EB%A9%94%EC%84%9C%EB%93%9C(sort,-tosorted)/){: }