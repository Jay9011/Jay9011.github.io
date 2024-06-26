---
title: javascript 연산자
date: 2024-06-26 15:56:39 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
math: false
type: javascript
keywords:
  - 연산자
---

# JavaScript 연산자 표

| 연산자        | 설명            | 예제 (간단)                                         |
| ---------- | ------------- | ----------------------------------------------- |
| +          | 덧셈            | 5 + 2 (결과: 7)                                   |
| -          | 뺄셈            | 5 - 2 (결과: 3)                                   |
| *          | 곱셈            | 5 * 2 (결과: 10)                                  |
| /          | 나눗셈           | 5 / 2 (결과: 2.5)                                 |
| %          | 나머지           | 5 % 2 (결과: 1)                                   |
| ++         | 증가            | let a = 1; a++ (결과: 2)                          |
| --         | 감소            | let a = 1; a-- (결과: 0)                          |
| =          | 할당            | let a = 5                                       |
| +=         | 더한 후 할당       | let a = 5; a += 2 (결과: 7)                       |
| -=         | 뺀 후 할당        | let a = 5; a -= 2 (결과: 3)                       |
| *=         | 곱한 후 할당       | let a = 5; a *= 2 (결과: 10)                      |
| /=         | 나눈 후 할당       | let a = 5; a /= 2 (결과: 2.5)                     |
| %=         | 나머지를 구한 후 할당  | let a = 5; a %= 2 (결과: 1)                       |
| ==         | 동등 비교         | 5 == "5" (결과: true)                             |
| ===        | 엄격 동등 비교      | 5 === "5" (결과: false)                           |
| !=         | 부등 비교         | 5 != "5" (결과: false)                            |
| !==        | 엄격 부등 비교      | 5 !== "5" (결과: true)                            |
| >          | 큰 비교          | 5 > 2 (결과: true)                                |
| <          | 작은 비교         | 5 < 2 (결과: false)                               |
| >=         | 크거나 같은 비교     | 5 >= 5 (결과: true)                               |
| <=         | 작거나 같은 비교     | 5 <= 2 (결과: false)                              |
| &&         | 논리 AND        | true && false (결과: false)                       |
| \|\|       | 논리 OR         | true \|\| false (결과: true)                      |
| !          | 논리 NOT        | !true (결과: false)                               |
| &          | 비트 AND        | 5 & 1 (결과: 1)                                   |
| \|         | 비트 OR         | 5 \| 1 (결과: 5)                                  |
| ^          | 비트 XOR        | 5 ^ 1 (결과: 4)                                   |
| ~          | 비트 NOT        | ~5 (결과: -6)                                     |
| <<         | 왼쪽 시프트        | 5 << 1 (결과: 10)                                 |
| >>         | 오른쪽 시프트       | 5 >> 1 (결과: 2)                                  |
| >>>        | 부호 없는 오른쪽 시프트 | 5 >>> 1 (결과: 2)                                 |
| ? :        | 조건 (삼항) 연산자   | let result = (5 > 2) ? "yes" : "no" (결과: "yes") |
| ??         | null 병합 연산자   | null ?? "default" (결과: "default")               |
| ,          | 쉼표 연산자        | let a = (1, 2, 3) (결과: 3)                       |
| typeof     | 데이터 타입 반환     | typeof 123 (결과: "number")                       |
| instanceof | 객체 타입 확인      | date instanceof Date (결과: true)                 |

# 연산자 부연 설명

## null 병합 연산자 (`??`)

null 병합 연산자는 좌항이 `null` 또는 `undefined`일 때 우항을 반환하고, 그렇지 않으면 좌항을 반환합니다.
<br>
주로 **기본값을 설정**할 때 사용됩니다.

##### 예제

```javascript
let name = null;
let defaultName = name ?? "Guest";
console.log(defaultName); // "Guest"
```

- `name`이 `null`이므로 `??` 연산자 오른쪽의 `"Guest"`가 반환됩니다.
- `name`이 `null` 또는 `undefined`가 아니라면 `name` 값이 반환됩니다.

## 쉼표 연산자 (`,`)

쉼표 연산자는 두 개 이상의 **표현식을 평가**하고, **마지막 표현식의 결과를 반환**합니다.
<br>
주로 여러 표현식을 하나의 문장에서 평가할 때 사용됩니다.

##### 예제

```javascript
let x = 1;
x = (x++, x);
console.log(x); // 2

x = (1, 2, 3);
console.log(x); // 3
```

- `x++, x`에서 `x++`를 평가한 후 `x`의 값을 갱신하고 최종적으로 `x`를 반환합니다.
- `1`, `2`, `3` 순서대로 평가되고, 마지막 값인 `3`이 `a`에 할당됩니다.
- 여러 표현식을 하나의 문장에서 실행하고자 할 때 유용합니다.

## 데이터 타입 반환 연산자 (`typeof`)

`typeof` 연산자는 피연산자의 **데이터 타입을 문자열** 형태로 반환합니다.
<br>
변수 또는 값의 타입을 확인할 때 사용됩니다.

#####  예제

```javascript
console.log(typeof 123);       // "number"
console.log(typeof "Hello");   // "string"
console.log(typeof true);      // "boolean"
console.log(typeof {});        // "object"
console.log(typeof undefined); // "undefined"
```

- `typeof 123`는 "number"를 반환합니다.
- `typeof "Hello"`는 "string"을 반환합니다.
- 각 타입에 대해 그에 맞는 문자열을 반환하여 타입을 쉽게 확인할 수 있습니다.

## 객체 타입 확인 연산자 (`instanceof`)

`instanceof` 연산자는 객체가 **특정 클래스(또는 생성자 함수)의 인스턴스인지 여부를 확인**할 때 사용됩니다.
<br>
`true` 또는 `false`를 반환합니다.

##### 예제

```javascript
function Person(name) {
  this.name = name;
}

let john = new Person("John");
console.log(john instanceof Person); // true
console.log(john instanceof Object); // true
```

- `john instanceof Person`는 `john`이 `Person`의 인스턴스이므로 `true`를 반환합니다.
- `john instanceof Object`는 모든 객체가 `Object`의 인스턴스이므로 `true`를 반환합니다.