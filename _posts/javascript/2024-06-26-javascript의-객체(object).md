---
title: javascript의 객체(Object)
date: 2024-06-26 18:47:24 +0900
categories:
  - 풀스택 개발
  - javascript
tags:
  - javascript
  - 객체 지향
math: false
type: javascript
keywords:
  - Object
  - 객체
  - 객체 지향 프로그래밍
  - OOP
---

JavaScript의 객체(Object)는 <span class="font_highlight">다양한 데이터를 하나의 단위로 묶어 관리하는 복합 데이터 타입</span>입니다.

객체는 <span class="font_highlight">**속성(properties)** 과 **메서드(methods)** 를 포함</span>할 수 있으며, 이 두 요소를 통해 데이터를 표현하고 조작합니다.

JavaScript는 [객체 지향 프로그래밍(OOP)](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: target="_blank"} 언어의 특성을 지니고 있으며, 객체를 중심으로 프로그램을 구성합니다.

# 객체(Object)

객체는 상태와 행동을 캡슐화하는 기본 단위입니다.

JavaScript의 객체는 속성(properties)과 메서드(methods)를 포함하며, 이러한 속성들과 메서드들은 객체의 상태와 행동을 정의합니다.

## 캡슐화(Encapsulation)

캡슐화는 관련된 데이터를 묶고, 데이터를 숨기고 외부에서 접근할 수 없도록 보호하는 개념입니다.
<br>
JavaScript는 객체를 통해 속성과 메서드를 묶어서 관리할 수 있으며, `클로저(closure)`를 이용하여 정보를 숨길 수 있습니다.

```javascript
function createPerson(name, age) {
    let _name = name;
    let _age = age;
    
    return {
        getName: function() {
            return _name;
        },
        getAge: function() {
            return _age;
        },
        setName: function(name) {
            _name = name;
        },
        setAge: function(age) {
            _age = age;
        }
    };
}

let person = createPerson('John', 30);
console.log(person.getName()); // John
person.setName('Jane');
console.log(person.getName()); // Jane
```

## 상속(Inheritance)

상속은 객체가 다른 객체의 속성과 메서드를 재사용할 수 있도록 하는 개념입니다.
<br>
JavaScript는 **프로토타입(prototype)** 을 통해 상속을 구현할 수 있습니다.

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(this.name + ' makes a noise.');
};

function Dog(name) {
    Animal.call(this, name); // super(name)
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
    console.log(this.name + ' barks.');
};

let dog = new Dog('Rex');
dog.speak(); // Rex barks.
```

## 다형성(Polymorphism)

다형성은 동일한 메서드가 다른 객체에서 다르게 동작할 수 있도록 하는 개념입니다.
<br>
JavaScript에서는 메서드 오버라이딩을 통해 다형성을 구현할 수 있습니다.

```javascript
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    console.log(this.name + ' makes a noise.');
};

function Dog(name) {
    Animal.call(this, name);
}

Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

Dog.prototype.speak = function() {
    console.log(this.name + ' barks.');
};

function Cat(name) {
    Animal.call(this, name);
}

Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;

Cat.prototype.speak = function() {
    console.log(this.name + ' meows.');
};

let animals = [new Dog('Rex'), new Cat('Whiskers')];
animals.forEach(animal => animal.speak()); 
// Rex barks.
// Whiskers meows.
```

## 추상화(Abstraction)

추상화는 복잡한 시스템을 단순화하고, 필요하지 않은 세부 사항을 감추는 개념입니다.
<br>
JavaScript에서 추상화는 객체 리터럴, 생성자 함수, 프로토타입 등을 이용해 구현할 수 있습니다.

```javascript
const authModule = (function() {  
  // 비공개 데이터  
  const users = [  
    { username: 'john', password: '1234' },  
    { username: 'jane', password: 'abcd' }  
  ];  
  
  // 공개 메서드  
  return {  
    login(username, password) {  
      const user = users.find(user => user.username === username && user.password === password);  
      if (user) {  
        console.log(`${username} is successfully logged in.`);  
        return true;  
      } else {  
        console.log('Invalid username or password.');  
        return false;  
      }  
    },  
    register(username, password) {  
      const userExists = users.some(user => user.username === username);  
      if (userExists) {  
        console.log('Username already exists.');  
        return false;  
      } else {  
        users.push({ username, password });  
        console.log(`${username} is successfully registered.`);  
        return true;  
      }  
    }  
  };  
})();
  
authModule.register('john', '1234'); // Username already exists.  
authModule.login('john', '1234'); // john is successfully logged in.
console.log(authModule.users); // undefined (비공개 데이터에 직접 접근 불가)
```

# 객체의 특징

## 속성(properties)

객체의 상태를 나타내며, 키(key)와 값(value)으로 구성됩니다. 키는 문자열이나 심볼일 수 있으며, 값은 **어떤 데이터 타입도 가능**합니다.

```javascript
let person = { 
	name: 'John',
	age: 30 
};
```

## 메서드(methods)

객체의 행동을 나타내는 함수입니다. 객체의 속성으로 함수를 정의할 수 있으며, 이를 메서드라고 부릅니다.

```javascript
let person = {
	greet: function() { console.log('Hello!'); }
};
```

# 객체 생성

JavaScript에서 객체는 다양한 방법으로 생성할 수 있으며, 대표적으로 객체 리터럴을 사용하거나 생성자 함수를 사용합니다.

```javascript
let obj1 = new Object(); // 객체 생성자 방식
let obj2 = {}; // 객체 리터럴 방식
```

# 속성(properties)

속성(properties)은 <span class="font_highlight">객체의 상태를 나타내는 값</span>입니다. 객체 리터럴이나 생성자 함수에서 정의할 수 있습니다.

## 속성 접근

이때, 속성을 접근하는 방법은 두가지가 있습니다.

### 점 표기법

점 표기법을 사용하면 객체의 속성에 쉽게 접근할 수 있습니다.

```javascript
console.log(person.name); // 점 표기법
```

### 대괄호 표기법

대괄호 표기법은 속성에 문자열이나 심볼로 접근할 수 있습니다. 이는 <span class="font_highlight">속성 이름이 동적으로 결정될 때 유용</span>하게 사용할 수 있습니다.

```javascript
function print(obj, key) {
  console.log(obj[key]);
}
print(person, 'age'); // 대괄호 표기법
```

## 속성 추가

점 표기법이나 대괄호 표기법을 사용하여 객체에 새로운 속성를 추가할 수 있습니다.

```javascript
person.hasJob = true;
console.log(person.hasJob); // true
```

## 속성 수정

기존 속성의 값을 속성 접근법으로 변경할 수 있습니다.

```javascript
person.hasJob = false; // 기존에 hasJob 속성이 true로 존재함.
console.log(person.hasJob); // false
```

## 속성 삭제 (`delete`)

`delete` 연산자를 사용하여 객체의 속성를 삭제할 수 있습니다.

```javascript
delete person.hasJob;
console.log(person.hasJob); // undefined
```

## 속성 유무 확인 (`in`)

`in` 연산자를 사용하여 객체에 특정 속성가 있는지 확인할 수 있습니다.

```javascript
console.log('name' in person); // true
console.log('age' in person); // true
console.log('hasJob' in person); // false
```

# 메서드 선언

메서드는 객체의 <span class="font_highlight">속성로서 정의된 함수</span>입니다.

물론, javascript의 함수는 **일급 객체**이기 때문에, 함수를 변수에 저장할 수 있고, 이름이 있는 함수 혹은 익명 함수(혹은 [화살표 함수](/posts/%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98-(arrow-function),-%EB%9E%8C%EB%8B%A4(lambda)/){: target="_blank"})로 선언 할 수 있습니다.

```javascript
let person = {
  say: function SayHello() {
    console.log('Hello');
  },
  say2: function () {
    console.log('Hello');
  },
  say3: () => {
    console.log('Hello');
  }
};
```

하지만, 위처럼 함수 표현식을 사용하지 않고, ES6부터 도입된 **메서드 축약 표현**을 사용해서 object내에서 바로 함수를 선언할 수 있습니다. 이를 **메서드 선언**이라고 표현하기도 합니다.

```javascript
let person = {
  sayHello() {  // 메서드 선언
    console.log('Hello');
  }
};
```

---

# 참고

[[OOP] 객체지향 프로그래밍(Object-Oriented Programming, OOP) 정리](/posts/oop-%EA%B0%9D%EC%B2%B4%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(object-oriented-programming,-oop)-%EC%A0%95%EB%A6%AC/){: }

[중첩 함수 (Nested Function)와 클로저 (closure)](/posts/%EC%A4%91%EC%B2%A9-%ED%95%A8%EC%88%98(nested-function)%EC%99%80-%ED%81%B4%EB%A1%9C%EC%A0%80-(closure)/){: }

[함수 표현식 (Function Expression)](/posts/%ED%95%A8%EC%88%98-%ED%91%9C%ED%98%84%EC%8B%9D-(function-expression)/){: }

[화살표 함수 (Arrow Function), 람다 (Lambda)](/posts/%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98-(arrow-function),-%EB%9E%8C%EB%8B%A4(lambda)/){: }