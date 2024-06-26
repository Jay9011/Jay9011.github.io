---
title: 템플릿 리터럴 (Template Literals)
date: 2024-06-26 15:31:42 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
math: false
type: javascript
---

템플릿 리터럴(Template Literals)은 ES6(ECMAScript 2015)에서 도입된 문법으로, 문자열을 작성할 때 기존의 따옴표 대신 <span class="font_highlight">백틱(\`)을 사용하여 보다 가독성이 좋고 **유연하게 문자열**을 다룰 수 있는 기능</span>입니다.
<br>
템플릿 리터럴은 변수와 표현식을 포함할 수 있으며, 여러 줄 문자열을 쉽게 작성할 수 있습니다.

# 기본 문법

1. **백틱(\`) 사용**: 문자열을 백틱(\` \`)으로 감쌉니다.
2. **변수 삽입**: `${}` 구문을 사용하여 변수나 표현식을 문자열 내부에 삽입할 수 있습니다.
3. **여러 줄 문자열**: 여러 줄에 걸쳐 문자열을 작성할 수 있습니다.

# 예제

#### 변수 삽입

```javascript
let name = 'John';
let age = 30;

let greeting = `Hello, my name is ${name} and I am ${age} years old.`;
console.log(greeting); // 출력: Hello, my name is John and I am 30 years old.
```

#### 표현식 삽입

```javascript
let a = 10;
let b = 20;

let result = `The sum of a and b is ${a + b}.`;
console.log(result); // 출력: The sum of a and b is 30.
```

#### 여러 줄 문자열

```javascript
let multiline = `This is a string
that spans across
multiple lines.`;
console.log(multiline);
// 출력:
// This is a string
// that spans across
// multiple lines.
```

#### 함수 호출 및 객체 접근

```javascript
function getGreeting(name) {
    return `Hello, ${name}!`;
}

let user = {
    name: 'Alice',
    age: 25
};

let message = `${getGreeting(user.name)} You are ${user.age} years old.`;
console.log(message); // 출력: Hello, Alice! You are 25 years old.
```

# 장점

1. **가독성**: 여러 줄 문자열과 변수 삽입이 쉬워 코드의 가독성이 향상됩니다.
2. **유연성**: 문자열 내에서 함수 호출이나 객체 접근 등 다양한 표현식을 사용할 수 있습니다.
3. **간결함**: 다양한 문자열 작성 시 백슬래시(\\)나 작은 따옴표(''), 큰 따옴표("")를 여러번 사용할 필요가 없습니다.