---
title: javascript의 async와 await
date: 2024-06-30 18:29:51 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 비동기
math: false
type: javascript
keywords:
  - async
  - await
  - Promise
---

`async`와 `await`의 사용은 자바스크립트의 비동기 프로그래밍 패턴을 단순화하고, 코드의 가독성을 크게 향상시킬 수 있는 키워드입니다.

# async

`async` 키워드는 **함수 앞**에 붙여서 <span class="font_highlight">해당 함수를 **비동기 함수**로 변환</span>합니다.

이렇게 선언된 비동기 함수는 항상 [Promise](/posts/javascript%EC%9D%98-promise-%EA%B0%9D%EC%B2%B4%EC%99%80-promise-%EC%B2%B4%EC%9D%B4%EB%8B%9D/){: target="_blank"}를 반환합니다.

이 함수 내에서 <span class="font_highlight">**명시적으로 값을 반환**할 경우 Promise.resolve(반환값)을 사용한 것처럼 그 반환 값이 자동으로 Promise로 감싸져 반환됩니다</span>. 만약 <span class="font_highlight">**예외**가 발생하면, 이 예외는 내부적으로 Promise.reject()를 통해 처리</span>됩니다.

```javascript
async function asyncFunction(test) {  
    if (test) {  
        return '성공'; // Promise.resolve('성공')과 동일  
    } else {  
        throw new Error('실패'); // Promise.reject(new Error('실패'))와 동일  
    }  
}
  
console.log(asyncFunction(true)); // Promise {[[PromiseState]]: "fulfilled", [[PromiseResult]]: "성공" }  
console.log(asyncFunction(false)); // Promise {[[PromiseState]]: "rejected", [[PromiseResult]]: Error: 실패... }
```

위 예제는 `asyncFunction(test)`에서 `test` 매개변수로 명시적으로 값을 반환하는 경우와 `throw`를 통해 예외가 발생하는 경우 두 가지를 테스트 할 수 있도록 만들었습니다.
<br>
명시적으로 값을 반환하는 경우, `Promise.resolve('성공')`처럼 Promise를 "fulfilled" 상태로 만들고, 반환 값을 결과 값으로 Promise를 만들어 반환합니다.
<br>
만약, 예외가 발생하는 경우, `Promise.reject(new Error('실패'))`처럼 Promise를 "rejected" 상태로 만들고, 예외 사항을 결과 값으로 하여 Promise를 만들어 반환합니다.

# await

`await` 키워드는 <span class="font_highlight">async 함수 내부</span>에서 사용되며, <span class="font_highlight">Promise가 완료될 때까지 함수의 실행을 일시 중지</span>합니다.

`Promise`가 해결(fulfilled)되면 `await`는 해당 Promise의 **결과 값**을 반환합니다.

만약 `Promise`가 거부(rejected)되면 `await`는 해당 **예외**를 던집니다.

```javascript
function resolveAfter2Seconds() {  
    return new Promise(resolve => {  
        setTimeout(() => {  
            resolve('성공');  
        }, 2000);  
    });  
}  
function rejectAfter2Seconds() {  
    return new Promise((resolve, reject) => {  
        setTimeout(() => {  
            reject(new Error('실패'));  
        }, 2000);  
    });  
}  
  
async function asyncCall() {  
    console.log('시작');  
    try {  
        let result = await resolveAfter2Seconds(); // 2초 후에 '성공'을 반환  
        console.log(result);  
        result = await rejectAfter2Seconds(); // 총 4초 후에 예외 발생  
        console.log(result);  
    } catch (error) {  
        console.error(error.message); // '실패' 출력  
    }  
  
    return '완료';  
} 
  
asyncCall();
```

위 예제에서 `resolveAfter2Seconds()`와 `rejectAfter2Seconds()`는 각각 2초의 대기시간을 가지는 `setTimeout`으로 만들어졌습니다.
<br>
`asyncCall()`내의 `await resolveAfter2Seconds()`는 해당 함수가 완료될 때까지 기다립니다. 2초 후, `Promise`가 해결되면 반환된 값 `'성공'`이 `result`에 저장됩니다.
<br>
이후, `await rejectAfter2Seconds()`도 2초 후에 거부되는 `Promise`를 반환받고 해당 함수가 완료될 때까지 기다렸다가 Promise가 거부되면 `await`는 이 거부된 이유로 예외가 발생시키고, `catch` 블록에서 처리됩니다.

## 예외 처리

따라서, 비동기 함수 내에서 `await`를 사용하면, 예외가 발생할 수 있습니다.

이러한 예외를 처리하기 위해 `try...catch` 블록을 사용할 수 있습니다.

```javascript
async function asyncCall() {  
    console.log('시작');  
    try {  
        let result = await resolveAfter2Seconds();
        console.log(result);  
        result = await rejectAfter2Seconds(); // 예외 발생 부분
        console.log(result);  
    } catch (error) {  // 예외 발생시 catch
        console.error(error.message);
    }  
  
    return '완료';  
}
```

# Promise 체이닝과의 비교

[Promise 체이닝](/posts/javascript%EC%9D%98-promise-%EA%B0%9D%EC%B2%B4%EC%99%80-promise-%EC%B2%B4%EC%9D%B4%EB%8B%9D/#promise-%EC%B2%B4%EC%9D%B4%EB%8B%9D){: target="_blank"}은 여러 개의 비동기 작업을 순차적으로 실행하고자 할 때 사용되는 패턴입니다.
<br>
각 비동기 작업은 Promise를 반환하고, `.then()` 메서드를 사용하여 이전 작업의 결과를 다음 작업으로 전달할 수 있었습니다.
<br>
이를 통해 기존의 콜백 지옥(Callback Hell)이라고 불리는 복잡한 중첩 구조를 어느정도 해결할 수 있었으나, 사용하다보면 여전히 `.then()`과 `.catch()`를 연속해서 사용하면서 코드가 복잡해질 수 있습니다.

`async`와 `await`를 사용하면 이 복잡한 구조를 좀 더 단순화하고 가독성을 높일 수 있습니다.

## 1. 선형적 코드 스타일

`async`와 `await`를 사용하면 비동기 코드를 거의 동기 코드처럼 작성할 수 있습니다. 이는 코드의 흐름을 직관적으로 보기 편하게 만들어줍니다.

```javascript
async function handleTasks() {
    const result1 = await asyncFunction();
    const result2 = await asyncFunction2(result1);
    const result3 = await asyncFunction3(result3);
    console.log('완료');
}
```

## 2. 통합된 예외 처리

`try...catch` 블록을 사용하여 여러 비동기 작업을 포괄하는 단일 오류 처리 로직을 적용할 수 있습니다.

이는 체이닝에서 각 단계별로 별도로 처리해야만 했던 오류들을 한 곳에서 처리할 수 있도록 해줍니다.

```javascript
async function handleTasks() {  
    try {  
        const result1 = await asyncFunction();
	    const result2 = await asyncFunction2(result1);
	    const result3 = await asyncFunction3(result3); 
    } catch (error) {  // 예외 발생시 catch
        console.error(error.message);
    }  
    return '완료';  
}
```

---

# 참고

[javascript의 Promise 객체와 Promise 체이닝](/posts/javascript%EC%9D%98-promise-%EA%B0%9D%EC%B2%B4%EC%99%80-promise-%EC%B2%B4%EC%9D%B4%EB%8B%9D/){: }