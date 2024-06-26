---
title: 화살표 함수 (Arrow Function), 람다 (Lambda)
date: 2024-06-26 17:55:34 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 함수
math: false
type: javascript
keywords:
  - Arrow Function
  - 람다
  - 화살표 함수
  - Lambda
---

**화살표 함수(Arrow Function)** 란, ES6에서 도입된, <span class="font_highlight">익명 함수를 간결한 문법으로 정의할 수 있는 기능</span>을 말합니다.

저는 이를 람다 표현식이라고 부르기도 합니다.

# 문법

```text
(매개_변수) => { 함수_몸체 }
```

함수 몸체는 한 줄로 작성할 수 있는 경우, 중괄호와 `return` 키워드를 생략할 수 있습니다.

```javascript
(param1, param2) => expression
```

매개 변수는 괄호 안에 작성되며, 매개변수가 없을 경우, 빈 괄호를 사용합니다.

또한, 매개변수가 하나일 경우, 괄호도 생략할 수 있습니다.

```javascript
param => expression
```

#### 예제

```javascript
const add = (a, b) => a + b; // 화살표 함수를 함수 표현식으로 사용
console.log(add(1, 2));

function count(arr, callback) {
  let count = 0;
  for (let i = 0; i < arr.length; i++) {
    if (callback(arr[i])) {
          count++;
        }
    }

  console.log(count);
}

count([1, 2, 3, 4, 5], items => items % 2 === 0); // 매개 변수가 하나이고 표현식이 한 줄인 화살표 함수
```

위의 예제에서는 익명 함수(화살표 함수)를 함수 표현식으로 사용하는 방법과 콜백 함수로 넘겨주는 방법을 보여주고 있습니다.

# 화살표 함수와 일반 함수의 차이점

#### 1. `this` 바인딩

화살표 함수는 자신의 `this` 값을 가지지 않습니다.

대신, 화살표 함수는 자신이 정의된 위치에서의 `this` 값을 상속받습니다. 이 점은 특히 콜백 함수 내에서 유용할 수 있습니다.

```javascript
function Person() {
    this.age = 0;

    setInterval(() => {
        this.age++;  // 여기서의 'this'는 Person 객체를 가리킵니다.
        console.log(this.age);
    }, 1000);
}

const p = new Person();
```

#### 2. `arguments` 객체

화살표 함수는 `arguments` 객체를 가지고 있지 않습니다. 대신, 필요한 경우 나머지 매개변수 문법(Rest Parameters, `...args`)을 사용할 수 있습니다.

```javascript
const sum = (...args) => args.reduce((a, b) => a + b, 0);
console.log(sum(1, 2, 3, 4));  // 출력: 10
```

---

# 참고

[함수 표현식 (Function Expression)](http://127.0.0.1:4000/posts/%ED%95%A8%EC%88%98-%ED%91%9C%ED%98%84%EC%8B%9D-(function-expression)/){: }

