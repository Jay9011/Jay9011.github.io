---
title: JavaScript 배열 정렬 메서드(sort, toSorted)
date: 2024-06-28 21:46:03 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 함수
math: false
type: javascript
keywords:
  - sort
  - toSorted
  - JavaScript
  - 배열
  - 정렬
---

| 메서드        | 사용 범위 | 설명                                  |
| ---------- | ----- | ----------------------------------- |
| `sort`     | 배열    | 배열의 요소를 정렬하는 메서드                    |
| `toSorted` | 배열    | 원본 배열을 변경하지 않고 새로운 정렬된 배열을 반환하는 메서드 |

# sort 메서드

`sort` 메서드는 <span class="font_highlight">배열의 요소를 **정렬**하여 해당 배열에 반영</span>합니다.

기본 정렬 순서는 문자열 유니코드 코드 포인트에 따라 **오름차순**입니다. 숫자 배열을 올바르게 정렬하거나 사용자 정의 정렬 순서를 지정하려면 **비교 함수**를 제공해야 합니다.

```javascript
array.sort([compareFunction])
```

- `compareFunction` (선택사항): 요소를 정렬하는 데 사용되는 함수입니다. 제공하지 않으면 요소들은 문자열로 변환된 후, 유니코드 코드 포인트 순서(사전순)에 따라 정렬됩니다.

## 기본 정렬 (문자열)

기본적으로 `sort` 메서드는 배열의 요소를 문자열로 변환하여 유니코드 코드 포인트 순서로 정렬합니다.

```javascript
const fruits = ['banana', 'apple', 'cherry'];

fruits.sort();

console.log(fruits); // ['apple', 'banana', 'cherry']
```

## 숫자 배열 정렬

숫자 배열을 올바르게 정렬하려면 **비교 함수를 제공**해야 합니다. 기본 문자열 정렬로는 숫자 배열을 제대로 정렬할 수 없습니다.

```javascript
const numbers = [4, 2, 5, 1, 3];

numbers.sort((a, b) => {
  if (a < b) return -1; // a가 b보다 작으면, a를 b 앞에 둔다
  if (a > b) return 1;  // a가 b보다 크면, a를 b 뒤에 둔다
  return 0;             // a와 b가 같으면, 순서를 바꾸지 않는다
});

console.log(numbers); // [1, 2, 3, 4, 5]
```

이때, 반환 값은 각각 다음과 같은 의미를 가지고 있습니다.

- `-1` (음수 값 ): 첫 번째 인수를 두 번째 인수보다 앞에 배치합니다.
- `0`: 비교 함수가 0을 반환하면, 두 인수의 순서를 유지합니다 (정렬 순서에 영향을 미치지 않습니다).
- `1` (양수 값): 첫 번째 인수를 두 번째 인수보다 뒤에 배치합니다.

## 사용자 정의 정렬

사용자 정의 비교 함수를 사용하여 사용자 정의 정렬 순서를 지정할 때, 비교 함수는 두 인수를 받아 다음과 같은 값을 반환해야 합니다.

- **음수**: `a`가 `b`보다 앞에 있어야 함.
- **0**: `a`와 `b`가 동등함.
- **양수**: `a`가 `b`보다 뒤에 있어야 함.

#### 예제

```javascript
const items = [
  { name: 'John', age: 25 },
  { name: 'Jane', age: 28 },
  { name: 'Peter', age: 21 }
];

items.sort((a, b) => a.age - b.age);

console.log(items);
// [
//   { name: 'Peter', age: 21 },
//   { name: 'John', age: 25 },
//   { name: 'Jane', age: 28 }
// ]
```

이 예제에서는 반환 값이 음수, 양수로 나뉘게 되므로 객체 배열을 `age` 속성에 따라 오름차순으로 정렬하게 됩니다.

## 주의사항

- `sort` 메서드는 원본 배열을 변경합니다. 원본 배열을 유지하려면 배열을 복사한 후 정렬해야 합니다.
- `sort` 메서드는 안정적이지 않을 수 있습니다. 즉, 동일한 값을 가진 요소들의 상대적 순서가 보장되지 않을 수 있습니다. (이는 JavaScript 엔진에 따라 다를 수 있습니다.)
- 배열 요소가 `undefined`인 경우, 기본 비교 함수는 이러한 요소들을 배열의 끝으로 이동시킵니다.

---

# toSorted 메서드

`toSorted` 메서드는 ECMAScript 2022 (ES13) 표준에 도입된 새로운 메서드로, 원본 배열을 변경하지 않고 <span class="font_highlight">배열의 요소를 **정렬**한 **새로운 배열**을 반환</span>합니다.

(최신 브라우저와 최신 Node.js 버전에서만 지원됩니다. 구형 브라우저나 구형 Node.js 환경에서는 지원되지 않을 수 있습니다.)

```javascript
array.toSorted(compareFunction)
```

- `compareFunction` (선택사항): 요소를 정렬하는 데 사용되는 함수입니다. 제공하지 않으면 요소들은 문자열로 변환된 후, 유니코드 코드 포인트 순서에 따라 정렬됩니다.

## 기본 사용법 (문자열)

`toSorted` 메서드는 원본 배열을 변경하지 않고, 정렬된 새로운 배열을 반환합니다. 이를 통해 원본 배열을 유지하면서 정렬된 배열을 얻을 수 있습니다.

```javascript
const fruits = ['banana', 'apple', 'cherry'];

const sortedFruits = fruits.toSorted();

console.log(fruits); // ['banana', 'apple', 'cherry'] - 원본 배열은 변경되지 않음
console.log(sortedFruits); // ['apple', 'banana', 'cherry'] - 정렬된 새로운 배열
```

이 예제에서는 `toSorted` 메서드를 사용하여 문자열 배열을 알파벳 순서로 정렬합니다. 원본 배열 `fruits`는 변경되지 않고, 정렬된 새로운 배열 `sortedFruits`가 반환됩니다.

## 숫자 배열 정렬

숫자 배열을 올바르게 정렬하려면 `sort`와 같이 **비교 함수**를 제공해야 합니다.

```javascript
const numbers = [4, 2, 5, 1, 3];

const sortedNumbers = numbers.toSorted((a, b) => a - b);

console.log(sortedNumbers); // [1, 2, 3, 4, 5]
console.log(numbers); // [4, 2, 5, 1, 3] - 원본 배열은 변경되지 않음
```

이 예제에서는 비교 함수 `(a, b) => a - b`를 제공하여 숫자 배열을 올바르게 오름차순으로 정렬합니다. 원본 배열 `numbers`는 변경되지 않고, 정렬된 새로운 배열 `sortedNumbers`가 반환됩니다.

## 사용자 정의 정렬

`toSorted`메서드도 사용자 정의 비교 함수를 사용하여 사용자 정의 정렬 순서를 지정할 수 있습니다.

- **음수**: `a`가 `b`보다 앞에 있어야 함.
- **0**: `a`와 `b`가 동등함.
- **양수**: `a`가 `b`보다 뒤에 있어야 함.

#### 예제

```javascript
const items = [
  { name: 'John', age: 25 },
  { name: 'Jane', age: 28 },
  { name: 'Peter', age: 21 }
];

const sortedItems = items.toSorted((a, b) => a.age - b.age);

console.log(sortedItems);
// [
//   { name: 'Peter', age: 21 },
//   { name: 'John', age: 25 },
//   { name: 'Jane', age: 28 }
// ]
console.log(items); // 원본 배열은 변경되지 않음
```

## 주의사항

- `toSorted` 메서드는 원본 배열을 변경하지 않습니다. 배열의 요소를 정렬한 새로운 배열을 반환합니다.
- `toSorted` 메서드는 배열을 복사한 후 정렬 작업을 수행하므로, 불변성을 유지하는 코드 작성에 유용합니다.
- 배열 요소가 `undefined`인 경우, 기본 비교 함수는 이러한 요소들을 배열의 끝으로 이동시킵니다.


---

# 참고

[JavaScript 배열 탐색 및 검사 메서드(includes,indexOf,findIndex,find)](/posts/javascript-%EB%B0%B0%EC%97%B4-%ED%83%90%EC%83%89-%EB%B0%8F-%EA%B2%80%EC%82%AC-%EB%A9%94%EC%84%9C%EB%93%9C(includes,-indexof,-findindex,-find)/){: }

[JavaScript 배열 순회 및 조작 메서드(forEach, map, filter, join)](/posts/javascript-%EB%B0%B0%EC%97%B4-%EC%88%9C%ED%9A%8C-%EB%B0%8F-%EC%A1%B0%EC%9E%91-%EB%A9%94%EC%84%9C%EB%93%9C(foreach,-map,-filter,-join)/){: }