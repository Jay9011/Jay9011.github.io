---
title: 중첩 함수 (Nested Function)와 클로저 (closure)
date: 2024-06-26 16:37:22 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 메모리
  - 스택
  - 함수
  - 힙
  - 가비지 컬렉터
math: false
type: javascript
keywords:
  - Nested Function
  - 중첩 함수
  - 클로저
  - closure
---

<span class="keyword">**중첩 함수(Nested Function)**</span>는 JavaScript에서 다른 <span class="font_highlight">함수 내부에 정의된 함수</span>를 말합니다.

중첩 함수는 그 함수가 정의된 범위에서만 접근할 수 있으며, 이를 통해 특정 기능을 캡슐화하거나 정보를 숨길 수 있습니다.

```javascript
function outerFunction(outerVariable) {  
  // 외부 함수의 변수 (매개 변수)  
  outerVariable = "I'm outside!";  
  
  // 내부 함수 정의  
  function innerFunction(innerVariable) {  
    console.log(outerVariable); // 외부 함수의 변수에 접근  
    console.log(innerVariable); // 매개 변수(내부 함수의 변수)에 접근  
  }  
  // 내부 함수 호출  
  innerFunction("I'm inside!");  
}  
  
// innerFunction(); // 내부 함수에 접근 불가 (ReferenceError: innerFunction is not defined)
outerFunction();
```

중첩 함수는 클로저(closure)와 밀접하게 관련이 있으며, 클로저를 사용하면 내부 함수가 외부 함수의 변수에 접근할 수 있습니다.

# 클로저 (closure)

클로저는 함수와 그 함수가 선언된 <span class="font_highlight">렉시컬 환경(Lexical Environment)을 기억하는 기능</span>을 말합니다.

클로저 덕분에 내부 함수는 자신이 선언된 시점의 외부 변수를 계속해서 참조할 수 있습니다. 이 개념을 통해 내부 함수가 외부 함수의 변수를 "기억"할 수 있습니다.

즉, 중첩 함수는 외부 함수의 변수를 기억하고 접근할 수 있는 클로저를 형성합니다. 이로인해 중첩 함수가 외부 함수의 변수에 접근하고 수정할 수 있습니다.

# 렉시컬 환경 (Lexical Environment)

렉시컬 환경은 JavaScript에서 변수와 함수가 정의되고 실행될 때, 그들의 **스코프(Scope)와 관련된 개념**입니다.

렉시컬 환경은 실행 컨텍스트(Execution Context)의 일부로, <span class="font_highlight">특정 시점에 변수와 함수의 접근 가능 범위를 결정</span>합니다.

렉시컬 환경은 환경 레코드(Environment Record)와 외부 렉시컬 환경 참조(Outer Lexical Environment Reference) 두 가지 구성 요소로 이루어집니다.

## 구성 요소

1. **환경 레코드(Environment Record)**
    - 현재 렉시컬 환경에서 정의된 모든 변수와 함수의 식별자 바인딩(Identifier Binding)을 기록합니다.
    - 변수와 함수의 이름과 값을 매핑합니다.
2. **외부 렉시컬 환경 참조(Outer Lexical Environment Reference)**
    - 현재 렉시컬 환경을 둘러싸고 있는 외부 렉시컬 환경을 참조합니다.
    - 이를 통해 중첩된 함수에서 외부 함수나 전역 스코프에 정의된 변수에 접근할 수 있습니다.

## 렉시컬 환경의 동작 방식

1. **함수 정의 시점**: 함수가 정의될 때, 그 함수의 렉시컬 환경이 결정됩니다. 이때의 렉시컬 환경은 함수가 정의된 위치의 스코프 체인을 기반으로 합니다.
2. **함수 호출 시점**: 함수가 호출될 때, 새로운 렉시컬 환경이 생성됩니다. 이 환경에는 호출 시의 인수 및 지역 변수가 포함됩니다.

예를 들어,

```javascript
function outerFunction() {
    let outerVariable = "I'm outside!";

    function innerFunction() {
        let innerVariable = "I'm inside!";
        console.log(outerVariable); // 외부 변수에 접근 가능
    }
    innerFunction();
}
outerFunction();
```

- `outerFunction`이 호출되면, `outerVariable`을 포함하는 새로운 렉시컬 환경이 생성됩니다.
- `innerFunction`이 호출되면, `innerVariable`을 포함하는 새로운 렉시컬 환경이 생성됩니다.
- `innerFunction`의 렉시컬 환경은 `outerFunction`의 렉시컬 환경을 참조합니다. 이를 통해 `innerFunction` 내부에서 `outerFunction`의 변수 `outerVariable`에 접근할 수 있습니다.

## 클로저와 렉시컬 환경

클로저는 함수가 정의될 때의 렉시컬 환경을 기억하는 기능을 말합니다. 이를 통해 함수가 실행될 때, 외부 함수의 변수에 접근할 수 있습니다.

```javascript
function makeCounter() {
    let count = 0;

    return function() {
        count++;
        return count;
    }
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

- `makeCounter` 함수가 호출되면, 새로운 실행 컨텍스트와 렉시컬 환경이 생성됩니다.
- 이 렉시컬 환경에는 `count` 변수가 포함되어 있으며, 이 변수는 `makeCounter` 함수의 환경 레코드에 저장됩니다.
- `makeCounter` 함수는 내부 함수(익명 함수)를 반환합니다. 이 내부 함수는 자신이 정의된 렉시컬 환경을 클로저로서 "기억"합니다. 즉, `count` 변수를 포함한 환경을 기억합니다.
- 이제 `counter`는 `makeCounter` 함수가 반환한 내부 함수입니다. 이 내부 함수는 `count` 변수를 참조할 수 있는 클로저를 가지고 있습니다.
- `counter()`를 호출할 때마다 내부 함수가 실행되며, 이 함수는 `count` 변수에 접근하여 값을 증가시키고, 증가된 값을 반환합니다.

## C++의 메모리 관리 관점에서 본 클로저

JavaScript와 C#은 C++과 달리 클로저와 가비지 컬렉션을 통해 변수의 수명을 다르게 관리합니다.
<br>
C++에서는 함수가 호출되면 스택에 지역 변수가 저장되고, 함수가 종료되면 이 변수들은 스택에서 제거됩니다. 따라서 지역 변수는 함수가 종료되면 더 이상 유효하지 않습니다.

JavaScript에서도 함수가 호출되면 실행 컨텍스트가 스택에 푸시되고, 함수가 반환되면 스택에서 팝됩니다. 하지만 **클로저**를 생성하면, 해당 함수가 참조하는 변수들은 **힙**에 저장됩니다.

클로저는 함수와 함수가 선언된 렉시컬 환경을 기억하기 때문에, 함수가 호출된 이후에도 외부 함수의 변수에 접근할 수 있게 됩니다.

이로인해 함수가 종료된 후에도, 클로저가 변수를 참조하고 있기 때문에 가비지 컬렉션의 대상이 되지 않고 계속 유지됩니다.

<br>
앞선 예제에서, `makeCounter` 함수는 내부 함수(클로저)를 반환하고, 이 클로저는 `count` 변수를 참조합니다. 따라서 `count` 변수는 `makeCounter` 함수가 종료된 후에도 힙에 남아 있습니다.
<br>
그렇기 때문에, `count`가 매번 초기화 되지 않고, `counter()`를 호출할 때마다 `count` 변수를 증가시키고 기억시킬 수 있습니다.

# 중첩 함수의 유용성

1. **캡슐화**: 중첩 함수를 사용하면 외부 함수의 변수를 숨길 수 있습니다. 이는 코드의 일부를 외부로부터 보호하는 데 유용합니다.
2. **모듈화**: 중첩 함수를 사용하면 특정 기능을 독립적으로 모듈화할 수 있습니다. 이는 코드의 유지보수성을 높입니다.
3. **클로저 활용**: 중첩 함수와 클로저를 사용하여 **상태를 유지**하거나, 외부 함수의 변수를 **계속해서 참조**할 수 있습니다.

---

# 참고

[JavaScript's Memory Management Explained \| Felix Gerschau](https://felixgerschau.com/javascript-memory-management/#heap-dynamic-memory-allocation)
