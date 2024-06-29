---
title: javascript의 Promise 객체와 Promise 체이닝
date: 2024-06-29 19:02:01 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 비동기
math: false
type: javascript
keywords:
  - Promise
  - Promise 체이닝
---

`Promise` 객체는 <span class="font_highlight">**어떤 함수**를 처리하며 **처리 상태**와 **처리 결과**를 가지고 있는 객체</span>입니다.

이 함수를 **executor(실행자, 실행 함수)** 라고 하며, 주로 비동기 함수의 처리 상태와 결과를 다루어서 비동기 코드를 더 쉽게 작성하기 위해 사용합니다.

또한, 이 객체를 통해 콜백 지옥(Callback Hell)이라고 불리는 복잡한 중첩 구조도 피할 수 있습니다.

```javascript
new Promise(executor_함수);
```

```javascript
const promise = new Promise((resolve, reject) => {
  // 비동기 작업을 수행
  if (/* 조건 */) {
    resolve('작업이 성공적으로 완료되었습니다.'); // 성공 시 호출
  } else {
    reject('작업이 실패했습니다.'); // 실패 시 호출
  }
});
```

# Executor 함수

`Executor` 함수는 `Promise` 객체를 생성할 때 전달되는 함수입니다.

이 함수는 두 개의 인자를 받는데, 이 두 인자의 정체는 콜백 함수로 `Promise`의 **상태**를 변경하고 **결과**를 처리하는데 사용됩니다.

- `resolve`: 비동기 작업이 성공적으로 완료되었을 때 호출됩니다.
- `reject`: 비동기 작업이 실패했을 때 호출됩니다.

```javascript
resolve(결과_매개변수);
reject(결과_매개변수);
```

```javascript
if (typeof num === 'number') {  
    resolve(num + 10);  // 작업이 성공하면 num + 10을 결과로 보냄
} else {  
    reject(new Error('숫자가 아닙니다.')); // 작업이 실패하면 Error 객체를 생성함
}
```

# Promise의 상태

Promise 객체는 다음과 같은 세 가지 상태를 가질 수 있습니다.

- **pending(대기)**: 초기 상태, 작업이 완료되지 않은 상태입니다.
- **fulfilled(성공)**: 작업이 성공적으로 완료된 상태입니다.
- **rejected(거부)**: 작업이 실패한 상태입니다.

# Promise의 메서드

## 1. then()

`then` 메서드는 Promise가 성공하던 실패하던, <span class="font_highlight">**완료되었을 때** 호출</span>됩니다.

단, `then` 메서드는 두 개의 콜백 함수를 인수로 받을 수 있는데, 첫 번째는 <span class="font_highlight">**작업이 성공**했을 때</span>, 두 번째는 <span class="font_highlight">**작업이 실패**했을 때</span> 호출됩니다.

#### 작업 성공시 호출

```javascript
testPromise(10)  
    .then((result) => {  
        console.log(result); // 20  
    });
```

#### 작업 실패시 두 번째 인자로 넘겨준 콜백 함수가 실행됨

`then` 메서드의 두 번째 인자에서 rejected를 처리하는 경우, 두 번째 콜백 함수가 실행됩니다.

```javascript
testPromise('Hello')  
    .then((result) => {  
        console.log(result);  
    }, (error) => {  
        console.error(error); // Error: 숫자가 아닙니다.  
    });
```

#### then에서 에러를 처리하는 콜백 함수를 넘겨주면, catch() 메서드는 해당 then에서 호출되지 않음

`then` 메서드의 두 번째 인자에서 rejected를 처리하면, 만약 `catch` 메서드를 정의했더라도 `catch` 메서드에서 rejected를 처리하지 않고 rejected가 발생한 `then`의 rejected를 처리합니다.

```javascript
testPromise('Hello')  
    .then((result) => {  
        console.log(result);  
    }, (error) => {  
        console.error('then()에서 에러 처리' + error); // then()에서 에러 처리Error: 숫자가 아닙니다.  
    })  
    .catch((error) => {  
        console.error('catch()에서 에러 처리' + error);  
    });
```

조금 더 살펴보면, 만약, Promise 체이닝을 사용했을 때, 두 번째 인자를 정의하지 않은 `then`에서 rejected가 발생했다면, 바로 `catch` 메서드로 빠져나가게 됩니다.

```javascript
testPromise(10)  
    .then((result) => {  
        console.log(result);    // 1. 20  
        return testPromise('Hello');  
    }, (error) => {  
        console.error('then()에서 에러 처리' + error);  
    })  
    .then((result) => {  
        console.log(result);  
        return testPromise(undefined);  
    })  
    .catch((error) => {  
        console.error('catch()에서 에러 처리' + error);   // 2. catch()에서 에러 처리Error: 숫자가 아닙니다.  
    });
```

위 예제에서는 처음 실행시 Promise는 resolve(fulfilled)가 되어 `20`을 출력하고, 이후 다시 Promise 객체를 리턴해 주면서 다음 `then`을 실행합니다.

그런데 이때 `then`에서는 따로 rejected를 처리하는 콜백 함수를 넘겨주지 않아 `catch` 메서드가 실행되는 것을 볼 수 있습니다.

즉, Promise 객체가 reject될 경우, `then` 메서드의 두 번째 인수로 전달된 콜백 함수가 우선적으로 실행되고, 이 콜백 함수를 전달하지 않았다면 `catch`로 넘어가게 됩니다.

## 2. catch()

`catch` 메서드는 Promise가 <span class="font_highlight">**실패했을 때** 호출</span>됩니다.

`then` 메서드의 두 번째 인수로 콜백 함수를 전달하는 대신 `catch` 메서드를 사용할 수 있으며, Promise 체이닝에서는 중간에 reject가 되는 경우 `catch` 메서드가 실행됩니다.

```javascript
testPromise('Hello')  
    .then((result) => {  
        console.log(result);  
    })  
    .catch((error) => {  
        console.error(error); // Error: 숫자가 아닙니다. 
    });
```

## 3. finally()

`finally` 메서드는 Promise가 <span class="font_highlight">완료되면 (성공 또는 실패 여부와 상관없이) **항상 호출**</span>됩니다.

```javascript
testPromise(10)  
    .then((result) => {  
        console.log(result);    // 1. 20  
    })  
    .catch((error) => {  
        console.error(error);  
    })  
    .finally(() => {  
        console.log('작업 완료');   // 2. 작업 완료  
    });
```

# Promise 체이닝

Promise는 하나의 <span class="font_highlight">`then` 메서드에서 반환된 값을 다음 `then` 메서드로 전달해서 사용</span>할 수 있습니다.

이를 통해 비동기 작업 이후 순차적인 처리를 하거나, 비동기 작업을 순차적으로 수행할 수 있습니다.

```javascript
// 비동기 처리 이후 순차적인 
testPromise(10)  
    .then((result) => {  
        console.log(result);    // 1. 20  
        return result * 2;  
    })  
    .then((result) => {  
        console.log(result);    // 2. 40  
        return undefined;  
    })  
    .then((result) => {  
        console.log(result);    // 3. undefined  
    })  
    .catch((error) => {  
        console.error(error);  
    })  
    .finally(() => {  
        console.log('작업 완료'); // 4. 작업 완료  
    });
```

`then`의 콜백 함수 내에서는 현재 `Promise` 객체의 상태를 직접 바꿀 수 없지만, <span class="font_highlight">`then` 내부에서 새로운 Promise를 반환</span>하는 것으로 다음 체인의 상태를 변경할 수 있습니다.

```javascript
testPromise(10)  
    .then((result) => {  
        console.log(result);    // 1. 20  
        return testPromise(result);  
    })  
    .then((result) => {  
        console.log(result);    // 2. 30  
        return testPromise(undefined);  
    })  
    .then((result) => {  
        console.log(result);  
    })  
    .catch((error) => {  
        console.error(error);   // 3. Error: 숫자가 아닙니다.  
    })  
    .finally(() => {  
        console.log('작업 완료'); // 4. 작업 완료  
    });
```

위 예제는 `then` 메서드 내에서 Promise 객체를 반환하여 새로운 Promise에 대한 `then`을 연속적으로 실행하는 예제입니다.

처음에는 성공적인 결과를 가진 Promise 객체를 새로 만들어 반환하고, 두 번째에서 `undefined`를 매개변수로 하는 Promise를 만들어 문제가 발생하도록 만들었습니다.

이렇게 만들면, 문제가 발생한 Promise로 인해 `catch` 메서드가 실행되고, 이후 `finally` 메서드가 실행됩니다.

# Promise.all()

모든 Promise가 완료될 때까지 <span class="font_highlight">기다린 후, **결과**를 배열</span>로 반환합니다.

하나의 Promise라도 실패하면 `Promise.all`은 실패합니다.

```javascript
const promise1 = Promise.resolve('성공1');  
const promise2 = Promise.resolve('성공2');  
const promise3 = Promise.resolve('성공3');  
const promise4 = Promise.reject('실패1');  
  
Promise.all([promise1, promise2, promise3])  
    .then((result) => {  
        console.log(result); // [ '성공1', '성공2', '성공3' ]  
    });  
  
Promise.all([promise1, promise2, promise3, promise4])  
    .then((result) => {  
        console.log(result);  
    })  
    .catch((error) => {  
        console.error(error);   // 실패1  
    });
```

# Promise.race()

<span class="font_highlight">**가장 빨리 완료**된</span> Promise의 결과가 <span class="font_highlight">성공이든 실패든 상관없이</span> 해당 결과를 반환합니다.

```javascript
const promise1 = new Promise((resolve, reject) => {  
    setTimeout(() => resolve('성공1'), 1000);  
});  
const promise2 = new Promise((resolve, reject) => {  
    setTimeout(() => resolve('성공2'), 2000);  
});  
const promise3 = new Promise((resolve, reject) => {  
    setTimeout(() => resolve('성공3'), 500);  
});  
const promise4 = new Promise((resolve, reject) => {  
    setTimeout(() => reject('실패1'), 100);  
});  
  
Promise.race([promise1, promise2, promise3])  
    .then((result) => {  
        console.log(result); // 성공3  
    });  
  
Promise.race([promise1, promise2, promise3, promise4])  
    .then((result) => {  
        console.log(result);  
    })  
    .catch((error) => {  
        console.error(error);   // 실패1  
    });
```

# Promise.allSettled()

모든 Promise가 완료될 때까지 <span class="font_highlight">기다린 후, 각 **Promise의 상태와 결과**를 배열로 반환</span>합니다.

각 결과는 `{status, value}` 또는 `{status, reason}` 형태입니다.

```javascript
const promise1 = Promise.resolve('성공1');  
const promise2 = Promise.resolve('성공2');  
const promise3 = Promise.reject('실패1');  
  
Promise.allSettled([promise1, promise2, promise3])  
    .then((result) => {  
        console.log(result);  
    })  
    .catch((error) => {  
        console.error(error);  
    });  
// [  
//   { status: 'fulfilled', value: '성공1' },  
//   { status: 'fulfilled', value: '성공2' },  
//   { status: 'rejected', reason: '실패1' }  
// ]
```

# Promise.any()

<span class="font_highlight">가장 먼저 **성공한** Promise의 결과를 반환</span>합니다.

모든 Promise가 실패하게 되면 `AggregateError`를 반환합니다.

```javascript
const promise1 = new Promise((resolve, reject) => {
    setTimeout(() => resolve('성공1'), 1000);
});
const promise2 = new Promise((resolve, reject) => {
    setTimeout(() => resolve('성공2'), 2000);
});
const promise3 = new Promise((resolve, reject) => {
    setTimeout(() => reject('실패1'), 500);
});
const promise4 = new Promise((resolve, reject) => {
    setTimeout(() => reject('실패2'), 100);
});

Promise.any([promise1, promise2, promise3])
    .then((result) => {
        console.log(result); // 성공1
    });

Promise.any([promise1, promise2, promise3, promise4])
    .then((result) => {
        console.log(result); // 성공1
    })
    .catch((error) => {
        console.error(error);
});

// Promise.any() 메서드는 모든 Promise 객체가 실패할 때만 에러를 반환한다.
Promise.any([promise3, promise4])
    .then((result) => {
        console.log(result);
    })
    .catch((error) => {
        console.error(error);   // [AggregateError: All promises were rejected] { [errors]: [ '실패1', '실패2' ] }
    });
```