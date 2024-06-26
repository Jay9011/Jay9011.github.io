---
title: 함수 표현식 (Function Expression)
date: 2024-06-26 17:26:05 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 함수
math: false
type: javascript
keywords:
  - 함수 표현식
  - Function Expression
---

자바스크립트(JavaScript)에서 함수 표현식(Function Expression)은 함수를 정의하고 할당할 수 있는 하나의 방법입니다.

함수 표현식은 일반적으로 <span class="font_highlight">함수를 변수에 할당하여 사용하는 방법</span>을 말합니다.

함수 선언(Function Declaration)과는 다르게, 함수 표현식은 정의되기 전에 호출할 수 없습니다. 이는 자바스크립트의 [호이스팅(hoisting)](/posts/%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-(hoisting)/){: target="_blank"}과 관련이 있습니다.

# 일급 객체

바스크립트에서 함수는 <span class="important">**일급 객체(first-class object)**</span>로 취급되므로 변수에 담을 수 있습니다. 이를 통해 함수를 다른 함수의 인자로 전달하거나, 함수에서 함수를 반환하는 등 다양한 방식으로 활용할 수 있습니다.

#### 일급객체의 속성

1. **변수에 할당**될 수 있습니다.
2. **함수의 인자**로 전달될 수 있습니다.
3. **함수의 반환값**으로 사용될 수 있습니다.

```javascript
function VarFunction() {  
  console.log('Function');  
}  
let myFunction = VarFunction;
myFunction(); // Function
```

# 함수 표현식의 기본 형태

함수 표현식은 이 함수를 변수에 정의하고 할당하여 사용하는 방법을 말합니다.

```text
키워드 변수명 = 함수;

변수명(); // 변수명으로 호출 가능
```

키워드는 변수를 할당하는 `var`, `let`, `const`가 될 수 있으며, 함수를 변수에 할당할 수 있습니다.

이때, 함수명은 스코프 규칙에 따라 **표현식 내부에서만 접근**할 수 있는 함수명이므로, 함수 외부에서는 함수명으로 접근할 수 없습니다.

```javascript
let myFunction = function VarFunction() {
    console.log('Function');
}

VarFunction();  // ReferenceError: VarFunction is not defined
```

즉, 위와 같은 코드에서 함수 표현식이 변수 `myFunction`에 할당되었기 때문에, 외부에서는 `myFunction`을 통해서만 해당 함수를 호출할 수 있습니다.

이에 따라 <span class="font_highlight">**함수명을 생략** 할 수 있습니다</span>.

```javascript
const myFunction = function () {
  console.log('Hello, World!');
};
```

위의 예제에서 `myFunction` 변수에 **익명 함수**(이름이 없는 함수)가 할당되었습니다. 이를 호출하려면 다음과 같이 할 수 있습니다.

```javascript
myFunction();  // 출력: Hello, World!
```

만약, 함수명이 필요하다면 아래와 같이 함수명과 함께 써줄 수도 있습니다.

```javascript
let myFunction = function VarFunction(n) {
    if (n <= 0) {
        return;
    }
    console.log(n);
    VarFunction(n - 1);  // 재귀 호출
}

myFunction(5);  // 출력: 5, 4, 3, 2, 1
```

# 함수 표현식의 특징

#### 익명 함수와 이름 있는 함수 표현식

함수 표현식은 익명 함수일 수도 있고, 이름을 가진 함수일 수도 있습니다.

```javascript
// 익명 함수 표현식
const anonymousFunc = function () {
 console.log('This is an anonymous function');
};

// 이름 있는 함수 표현식
const namedFunc = function myNamedFunction() {
 console.log('This is a named function');
};
```

#### 호이스팅(hoisting)

함수 표현식은 선언되기 전에 호출할 수 없습니다. 이는 변수에 할당되기 전에 참조할 수 없기 때문입니다.

```javascript
myFunc();  // 오류: Uncaught ReferenceError: Cannot access 'myFunc' before initialization
const myFunc = function () {
 console.log('This is a function expression');
};
```

#### 즉시 실행 함수 표현식(IIFE)

즉시 실행 함수 표현식은 정의와 동시에 실행되는 함수입니다. 보통 소괄호로 감싸서 사용합니다.

```javascript
(function () {
 console.log('This is an IIFE');
})();
```

# 사용 사례

#### 콜백 함수

함수 표현식은 콜백 함수로 자주 사용됩니다. 콜백 함수는 다른 함수의 인수로 전달되어 특정 작업이 완료된 후 실행됩니다.

```javascript
setTimeout(function () {
console.log('This is a callback function');
}, 1000);
```

#### 클로저(closure)

함수 표현식은 [클로저](/posts/%EC%A4%91%EC%B2%A9-%ED%95%A8%EC%88%98(nested-function)%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-(closure)/){: target="_blank"}를 생성하는 데 유용합니다. 클로저는 함수와 그 함수가 선언된 어휘적 환경의 조합입니다.

```javascript
function makeCounter() {
let count = 0;
return function () {
  count++;
  return count;
};
}

const counter = makeCounter();
console.log(counter());  // 출력: 1
console.log(counter());  // 출력: 2
```

# 요약

- **함수 표현식**은 변수를 통해 함수를 정의하고 할당하는 방법입니다.
- **익명 함수**와 **이름 있는 함수 표현식**이 있습니다.
- 함수 표현식은 호이스팅되지 않기 때문에 선언 후에만 호출할 수 있습니다.
- 콜백 함수, 클로저, 즉시 실행 함수 표현식(IIFE) 등의 다양한 용도로 사용됩니다.

---

# 참고

[호이스팅 (Hoisting)](/posts/%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85-(hoisting)/){: }

[중첩 함수(Nested Function)와 클로저 (closure)](/posts/%EC%A4%91%EC%B2%A9-%ED%95%A8%EC%88%98(nested-function)%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-(closure)/){: }