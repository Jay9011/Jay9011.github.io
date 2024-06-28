---
title: JavaScript 배열 탐색 및 검사 메서드(includes,indexOf,findIndex,find)
date: 2024-06-28 15:08:07 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 함수
math: false
type: javascript
keywords:
  - includes
  - indexOf
  - findIndex
  - find
  - JavaScript
  - 배열
  - 탐색
  - 검사
---

| 메서드         | 사용 범위   | 설명                                        |
| ----------- | ------- | ----------------------------------------- |
| `includes`  | 배열과 문자열 | 배열과 문자열 내에 **특정 요소가 포함되어 있는지 확인**하는 메서드   |
| `indexOf`   | 배열과 문자열 | 배열과 문자열 내에서 **특정 요소의 첫 번째 인덱스를 반환**하는 메서드 |
| `findIndex` | 배열      | **조건을 만족하는 첫 번째 요소의 인덱스를 반환**하는 메서드       |
| `find`      | 배열      | **조건을 만족하는 첫 번째 요소를 반환**하는 메서드            |

# includes 메서드

`includes` 메서드는 ES7 (ECMAScript 2016)부터 도입된 **배열**과 **문자열** 모두에서 사용될 수 있는 메서드로, <span class="font_highlight">**특정 요소**나 **문자열**이 포함되어 있는지 확인</span>합니다.

`includes` 메서드는 <span class="important">**대소문자를 구분**</span>합니다.

포함되어 있으면 `true`를, 그렇지 않으면 `false`를 반환합니다.

## 배열에서 사용

배열의 `includes` 메서드는 배열 내에 **특정 요소**가 포함되어 있는지 확인하고, 포함되어 있으면 `true`를, 그렇지 않으면 `false`를 반환합니다.

```javascript
array.includes(searchElement, fromIndex)
```

- `searchElement`: 배열 내에서 찾고자 하는 **요소**입니다.
- `fromIndex` (선택사항): **검색을 시작할 인덱스**입니다. 기본값은 0이며, 음수를 사용하면 배열의 끝에서부터 오프셋을 계산하여 시작합니다.

```javascript
const fruits = ['apple', 'banana', 'mango', 'orange'];

console.log(fruits.includes('banana'));  // true
console.log(fruits.includes('grape'));   // false
console.log(fruits.includes('mango', 2)); // true
console.log(fruits.includes('mango', 3)); // false
```

## 문자열에서 사용

문자열의 `includes` 메서드는 문자열 내에 특정 **서브스트링**이 포함되어 있는지 확인하고, 포함되어 있으면 `true`를, 그렇지 않으면 `false`를 반환합니다.

```javascript
string.includes(searchString, position)
```

- `searchString`: 문자열 내에서 찾고자 하는 **서브스트링**입니다.
- `position` (선택사항): **검색을 시작할 위치**입니다. 기본값은 0입니다.

```javascript
const sentence = 'The quick brown fox jumps over the lazy dog';

console.log(sentence.includes('quick')); // true
console.log(sentence.includes('Quick')); // false
console.log(sentence.includes('fox', 10)); // true
console.log(sentence.includes('fox', 20)); // false
```

## 주요 특징

- **대소문자 구분**: `includes` 메서드는 대소문자를 구분합니다. 예를 들어, `'quick'`과 `'Quick'`은 다른 문자열로 인식됩니다.
- **부분 문자열 검색**: 문자열의 특정 위치부터 검색을 시작할 수 있습니다. 이는 `fromIndex` 매개변수를 사용하여 설정할 수 있습니다.

---

# indexOf 메서드

`indexOf` 메서드는 **배열**과 **문자열** 모두에서 사용될 수 있는 메서드로, <span class="font_highlight">**특정 요소**나 **문자열**이 처음으로 나타나는 인덱스를 반환</span>합니다.

인덱스를 반환하기 때문에, 요소가 없으면 `-1`을 반환합니다.

## 배열에서 사용

배열의 `indexOf` 메서드는 배열 내에서 **특정 요소의 첫 번째 인덱스**를 반환하며, 요소가 존재하지 않으면 -1을 반환합니다.

```javascript
array.indexOf(searchElement, fromIndex)
```

- `searchElement`: 배열 내에서 찾고자 하는 **요소**입니다.
- `fromIndex` (선택사항): **검색을 시작할 인덱스**입니다. 기본값은 0이며, 음수를 사용하면 배열의 끝에서부터 오프셋을 계산하여 시작합니다.

```javascript
const fruits = ['apple', 'banana', 'mango', 'orange', 'banana'];

console.log(fruits.indexOf('banana'));  // 1
console.log(fruits.indexOf('grape'));   // -1
console.log(fruits.indexOf('banana', 2)); // 4
```

## 문자열에서 사용

문자열의 `indexOf` 메서드는 문자열 내에서 특정 **서브스트링**의 **첫 번째 인덱스**를 반환하며, 서브스트링이 존재하지 않으면 -1을 반환합니다.

```javascript
string.indexOf(searchValue, fromIndex)
```

- `searchValue`: 문자열 내에서 찾고자 하는 **서브스트링**입니다.
- `fromIndex` (선택사항): **검색을 시작할 위치**입니다. 기본값은 0입니다.

```javascript
const sentence = 'The quick brown fox jumps over the lazy dog';

console.log(sentence.indexOf('quick')); // 4
console.log(sentence.indexOf('Quick')); // -1
console.log(sentence.indexOf('fox', 10)); // 16
console.log(sentence.indexOf('fox', 20)); // -1
```

## 주요 특징

- **대소문자 구분**: `indexOf` 메서드는 **대소문자를 구분**합니다. `'quick'`과 `'Quick'`은 다른 문자열로 인식됩니다.
- **부분 문자열 검색**: 문자열의 특정 위치부터 검색을 시작할 수 있습니다. 이는 `fromIndex` 매개변수를 사용하여 설정할 수 있습니다.
- **반환값**: 요소가 존재하면 **인덱스**를, 존재하지 않으면 **-1**을 반환합니다.

---

# findIndex 메서드

`findIndex` 메서드는 ES6(ECMAScript 2015)부터 도입된, **배열**에서만 사용될 수 있는 메서드로, <span class="font_highlight">**조건**을 만족하는 첫 번째 요소의 **인덱스**를 반환</span>합니다. 조건을 만족하는 요소가 없으면 `-1`을 반환합니다.

`findIndex` 메서드는 콜백 함수를 사용하여 배열의 각 요소를 테스트하게 되는데, 이때, 배열의 각 요소에 대해 한 번씩 콜백 함수를 실행합니다.

이후, 조건을 만족하는 요소가 발견되면 배열의 순회를 중단하고, 현재 인덱스를 반환합니다.

```javascript
array.findIndex(callback(element, index, array), thisArg)
```

- `callback`: 배열의 각 요소에 대해 실행할 함수입니다. 아래 인수를 가집니다.
    - `element`: 처리할 현재 요소
    - `index`: 배열의 현재 요소의 인덱스
    - `array`: `findIndex`를 호출한 현재 배열 자체
- `thisArg` (선택사항): `callback` 함수 내부에서 `this`로 사용할 값입니다.

## 사용 예

```javascript
const numbers = [5, 12, 8, 130, 44];  
const callback_findIndex = (element) => element > 15;  
  
console.log(numbers.findIndex(callback_findIndex)); // 3 : 15보다 큰 요소들 중 첫번째 요소인 130의 인덱스를 반환
```

## 주요 특징

- **조건 함수**: `findIndex` 메서드는 배열의 각 요소에 대해 제공된 콜백 함수를 호출하여 조건을 평가합니다.
- **첫 번째 매칭**: 조건을 만족하는 요소가 발견되면, 순회를 멈추고 현재 인덱스(조건을 만족하는 첫 번째 요소의 인덱스)를 반환합니다.
- **반환값**: 조건을 만족하는 요소가 존재하면 인덱스를, 존재하지 않으면 `-1`을 반환합니다.

---

# find 메서드

`find` 메서드도 ES6(ECMAScript 2015)부터 도입된, **배열**에서만 사용될 수 있는 메서드로, <span class="font_highlight">**조건**을 만족하는 첫 번째 **요소**를 반환</span>합니다.

조건을 만족하는 요소가 없으면 `undefined`를 반환합니다.

`find` 메서드도 콜백 함수를 사용하여 배열의 각 요소를 테스트하며, 배열의 각 요소에 대해 한 번씩 콜백 함수를 실행합니다.

이후, 조건을 만족하는 요소가 발견되면 배열의 순회를 중단하고, 현재 요소를 반환합니다.

```javascript
array.find(callback(element, index, array), thisArg)
```

- `callback`: 배열의 각 요소에 대해 실행할 함수입니다. 아래 인수를 가집니다.
    - `element`: 처리할 현재 요소
    - `index`: 배열의 현재 요소의 인덱스
    - `array`: `findIndex`를 호출한 현재 배열 자체
- `thisArg` (선택사항): `callback` 함수 내부에서 `this`로 사용할 값입니다.

## 사용 예

```javascript
const inventory = [  
    {name: 'health potion', quantity: 20},  
    {name: 'mana potion', quantity: 15},  
    {name: 'sword', quantity: 1}  
];  
const result = inventory.find(item => item.name === 'mana potion');  
  
console.log(result); // {name: 'mana potion', quantity: 15}
```

## 주요 특징

- **조건 함수**: `find` 메서드는 배열의 각 요소에 대해 제공된 콜백 함수를 호출하여 조건을 평가합니다.
- **첫 번째 매칭**: 조건을 만족하는 요소가 발견되면, 순회를 멈추고 현재 요소(조건을 만족하는 첫 번째 요소)를 반환합니다.
- **반환값**: 조건을 만족하는 요소가 존재하면 그 요소를, 존재하지 않으면 `undefined`를 반환합니다.


---

# 참고

[JavaScript 배열 순회 및 조작 메서드(forEach, map, filter, join)](/posts/javascript-%EB%B0%B0%EC%97%B4-%EC%88%9C%ED%9A%8C-%EB%B0%8F-%EC%A1%B0%EC%9E%91-%EB%A9%94%EC%84%9C%EB%93%9C(foreach,-map,-filter,-join)/){: }

[JavaScript 배열 정렬 메서드(sort, toSorted)](/posts/javascript-%EB%B0%B0%EC%97%B4-%EC%A0%95%EB%A0%AC-%EB%A9%94%EC%84%9C%EB%93%9C(sort,-tosorted)/){: }