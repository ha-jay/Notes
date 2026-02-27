깃허브 주소
- https://github.com/ha-jay/LearnLang_javascript

[[자바스크립트 엔진의 코드실행과정]]

[[주석]]
- 여러줄주석 - header(모듈설명, 기능설명), todolist
- 여러줄주석 - 함수설명, 파라미터설명
- 한줄주석 - 코드로직 설명 
> 당연하게 아는내용은 생략하고, 왜이렇게 코드를 짰는지 의도를 설명하는것이 중요하다. 이렇게 주석으로 남겨두면 ai에게 훨씬 정확한 답변을 얻을 수 있다. 


[[strictmode]]
- 선언없이 변수사용 금지, 읽기전용속성에 값쓰기금지, 중복매개변수금지, with문 사용금지, 예약어 및 키워드를 변수명 사용금지

[[identifier-naming convention]]
- const 배열, 객체 / let - primitive / hoisting 방지 scope의 최상단에 선언
- 함수 이름
	- 불리언 체크 be나 have 
	- get,make,apply,find,create,set 같은의미동사 통일
	- 명사는 상세히 표현
	- 추가정보는 부사구 사용
	
[[메모리와 데이터 타입]]
[[변수]]
[[표현식 과 연산자]]
[[구조분해할당]]
[[논리연산자 활용 Optional Logic 및 옵셔널체이닝]]

[[조건문]]
[[반복문]]
[[함수]]

```


### 함수

javascript

```javascript
// 함수 선언식
function add(a, b) {
  return a + b;
}

// 함수 표현식
const multiply = function(a, b) {
  return a * b;
};

// 화살표 함수
const divide = (a, b) => a / b;

const greet = name => `Hello, ${name}!`;

const calculate = (a, b) => {
  const sum = a + b;
  const product = a * b;
  return { sum, product };
};

// 기본 매개변수
function createUser(name, age = 18) {
  return { name, age };
}

// Rest 매개변수
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}

console.log(sum(1, 2, 3, 4));  // 10

// 콜백 함수
function processArray(arr, callback) {
  const result = [];
  for (const item of arr) {
    result.push(callback(item));
  }
  return result;
}

const doubled = processArray([1, 2, 3], num => num * 2);
```





## 2단계: 중급 개념 (2-3주)

### 배열 메서드

javascript

```javascript
const numbers = [1, 2, 3, 4, 5];

// map - 변환
const doubled = numbers.map(num => num * 2);
// [2, 4, 6, 8, 10]

// filter - 필터링
const evens = numbers.filter(num => num % 2 === 0);
// [2, 4]

// reduce - 축약
const sum = numbers.reduce((acc, num) => acc + num, 0);
// 15

// find - 첫 번째 일치 항목
const found = numbers.find(num => num > 3);
// 4

// findIndex - 인덱스 찾기
const index = numbers.findIndex(num => num > 3);
// 3

// some - 하나라도 조건 만족
const hasEven = numbers.some(num => num % 2 === 0);
// true

// every - 모두 조건 만족
const allPositive = numbers.every(num => num > 0);
// true

// forEach - 순회
numbers.forEach((num, index) => {
  console.log(`Index ${index}: ${num}`);
});

// sort - 정렬
const sorted = [3, 1, 4, 1, 5].sort((a, b) => a - b);
// [1, 1, 3, 4, 5]

// includes - 포함 여부
const hasThree = numbers.includes(3);
// true

// slice - 부분 추출 (원본 유지)
const sliced = numbers.slice(1, 4);
// [2, 3, 4]

// splice - 부분 제거/추가 (원본 변경)
const arr = [1, 2, 3, 4, 5];
arr.splice(2, 1, 99);  // 인덱스 2부터 1개 제거하고 99 추가
// [1, 2, 99, 4, 5]

// concat - 배열 합치기
const combined = [1, 2].concat([3, 4]);
// [1, 2, 3, 4]

// join - 문자열로 변환
const joined = numbers.join(", ");
// "1, 2, 3, 4, 5"
```

### 객체 다루기

javascript

```javascript
const user = {
  name: "John",
  age: 30,
  email: "john@email.com"
};

// 속성 접근
console.log(user.name);        // "John"
console.log(user["age"]);      // 30

// 속성 추가/수정
user.city = "Seoul";
user.age = 31;

// 속성 삭제
delete user.email;

// 속성 존재 확인
console.log("name" in user);   // true
console.log(user.hasOwnProperty("age"));  // true

// Object 메서드
const keys = Object.keys(user);
// ["name", "age", "city"]

const values = Object.values(user);
// ["John", 31, "Seoul"]

const entries = Object.entries(user);
// [["name", "John"], ["age", 31], ["city", "Seoul"]]

// 객체 복사 (얕은 복사)
const copy = Object.assign({}, user);
const copy2 = { ...user };

// 객체 병합
const merged = { ...user, country: "Korea", age: 32 };

// Object.freeze - 수정 불가
const frozen = Object.freeze({ name: "Jane" });
frozen.name = "John";  // 무시됨 (strict mode에서는 에러)

// Object.seal - 속성 추가/삭제 불가, 수정은 가능
const sealed = Object.seal({ name: "Jane" });
sealed.name = "John";  // OK
sealed.age = 30;       // 무시됨

// 구조 분해 할당
const { name, age } = user;
console.log(name, age);

const { name: userName, age: userAge = 18 } = user;

// 나머지 패턴
const { name: n, ...rest } = user;
console.log(rest);  // { age: 31, city: "Seoul" }
```

### 문자열 메서드

javascript

```javascript
const str = "Hello, World!";

// 길이
console.log(str.length);  // 13

// 검색
console.log(str.indexOf("World"));     // 7
console.log(str.lastIndexOf("o"));     // 8
console.log(str.includes("Hello"));    // true
console.log(str.startsWith("Hello"));  // true
console.log(str.endsWith("!"));        // true

// 추출
console.log(str.slice(0, 5));          // "Hello"
console.log(str.substring(7, 12));     // "World"
console.log(str.substr(7, 5));         // "World" (deprecated)

// 변환
console.log(str.toLowerCase());        // "hello, world!"
console.log(str.toUpperCase());        // "HELLO, WORLD!"
console.log(str.trim());               // 양쪽 공백 제거
console.log(str.trimStart());          // 시작 공백 제거
console.log(str.trimEnd());            // 끝 공백 제거

// 대체
console.log(str.replace("World", "JavaScript"));
console.log(str.replaceAll("o", "0"));

// 분할
console.log(str.split(", "));          // ["Hello", "World!"]

// 반복
console.log("abc".repeat(3));          // "abcabcabc"

// 패딩
console.log("5".padStart(3, "0"));     // "005"
console.log("5".padEnd(3, "0"));       // "500"

// 템플릿 리터럴
const name = "John";
const age = 30;
const message = `My name is ${name} and I'm ${age} years old.`;

// 여러 줄 문자열
const multiline = `
  This is
  a multiline
  string
`;
```

### 비구조화와 스프레드

javascript

```javascript
// 배열 구조 분해
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);   // 1
console.log(second);  // 2
console.log(rest);    // [3, 4, 5]

// 기본값 설정
const [a = 10, b = 20] = [1];
console.log(a, b);    // 1, 20

// 값 교환
let x = 1, y = 2;
[x, y] = [y, x];

// 객체 구조 분해
const user = { name: "John", age: 30, city: "Seoul" };
const { name, age, country = "Korea" } = user;

// 함수 매개변수에서 구조 분해
function greet({ name, age }) {
  console.log(`Hello, ${name}! You are ${age} years old.`);
}
greet(user);

// 스프레드 연산자
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];  // [1, 2, 3, 4, 5, 6]

const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const mergedObj = { ...obj1, ...obj2 };  // { a: 1, b: 2, c: 3, d: 4 }

// 함수 인자로 스프레드
function sum(a, b, c) {
  return a + b + c;
}
console.log(sum(...[1, 2, 3]));  // 6

// 배열/객체 복사
const arrCopy = [...arr1];
const objCopy = { ...obj1 };
```

## 3단계: 고급 개념 (3-4주)

### 클로저와 스코프

javascript

```javascript
// 스코프
let globalVar = "global";

function outer() {
  let outerVar = "outer";
  
  function inner() {
    let innerVar = "inner";
    console.log(globalVar);  // 접근 가능
    console.log(outerVar);   // 접근 가능
    console.log(innerVar);   // 접근 가능
  }
  
  inner();
  // console.log(innerVar);  // 에러! 접근 불가
}

// 클로저
function createCounter() {
  let count = 0;  // private 변수
  
  return {
    increment() {
      count++;
      return count;
    },
    decrement() {
      count--;
      return count;
    },
    getCount() {
      return count;
    }
  };
}

const counter = createCounter();
console.log(counter.increment());  // 1
console.log(counter.increment());  // 2
console.log(counter.getCount());   // 2
// console.log(counter.count);     // undefined (접근 불가)

// 클로저 활용 예시 - 부분 적용
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const multiplyBy2 = multiply(2);
console.log(multiplyBy2(5));  // 10
console.log(multiplyBy2(10)); // 20

// 즉시 실행 함수 (IIFE)
(function() {
  const privateVar = "private";
  console.log("Executed immediately");
})();
```

### this와 바인딩

javascript

```javascript
// 일반 함수에서 this
function normalFunction() {
  console.log(this);  // 전역 객체 (브라우저: window, Node.js: global)
}

// 객체 메서드에서 this
const person = {
  name: "John",
  greet: function() {
    console.log(`Hello, ${this.name}`);
  },
  greetArrow: () => {
    console.log(`Hello, ${this.name}`);  // undefined (화살표 함수는 this를 바인딩하지 않음)
  }
};

person.greet();       // "Hello, John"
person.greetArrow();  // "Hello, undefined"

// this 바인딩 문제
const greetFunc = person.greet;
greetFunc();  // "Hello, undefined" (this가 전역 객체를 가리킴)

// call, apply, bind
greetFunc.call(person);   // "Hello, John"
greetFunc.apply(person);  // "Hello, John"

const boundGreet = greetFunc.bind(person);
boundGreet();  // "Hello, John"

// call vs apply
function introduce(greeting, punctuation) {
  console.log(`${greeting}, I'm ${this.name}${punctuation}`);
}

introduce.call(person, "Hi", "!");      // "Hi, I'm John!"
introduce.apply(person, ["Hi", "!"]);   // "Hi, I'm John!"

// 화살표 함수의 this
const obj = {
  name: "Object",
  regularFunc: function() {
    setTimeout(function() {
      console.log(this.name);  // undefined
    }, 100);
  },
  arrowFunc: function() {
    setTimeout(() => {
      console.log(this.name);  // "Object" (상위 스코프의 this 사용)
    }, 100);
  }
};
```

### 프로토타입과 클래스

javascript

```javascript
// 생성자 함수
function Person(name, age) {
  this.name = name;
  this.age = age;
}

// 프로토타입에 메서드 추가
Person.prototype.greet = function() {
  console.log(`Hello, I'm ${this.name}`);
};

const john = new Person("John", 30);
john.greet();  // "Hello, I'm John"

// 프로토타입 체인
console.log(john.__proto__ === Person.prototype);  // true
console.log(Person.prototype.__proto__ === Object.prototype);  // true

// ES6 클래스
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
  
  // 정적 메서드
  static compare(user1, user2) {
    return user1.age - user2.age;
  }
  
  // getter
  get info() {
    return `${this.name} (${this.age})`;
  }
  
  // setter
  set username(value) {
    this.name = value;
  }
}

const user = new User("Jane", 25);
user.greet();
console.log(user.info);
User.compare(user, john);

// 상속
class Admin extends User {
  constructor(name, age, role) {
    super(name, age);  // 부모 생성자 호출
    this.role = role;
  }
  
  greet() {
    super.greet();  // 부모 메서드 호출
    console.log(`I'm an ${this.role}`);
  }
}

const admin = new Admin("Alice", 28, "admin");
admin.greet();
```

### 비동기 프로그래밍

javascript

```javascript
// 콜백
function fetchData(callback) {
  setTimeout(() => {
    callback("Data loaded");
  }, 1000);
}

fetchData((data) => {
  console.log(data);
});

// 콜백 지옥
fetchUser(userId, (user) => {
  fetchPosts(user.id, (posts) => {
    fetchComments(posts[0].id, (comments) => {
      console.log(comments);
    });
  });
});

// Promise
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve("Success!");
    } else {
      reject("Error!");
    }
  }, 1000);
});

promise
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log("Done"));

// Promise 체이닝
fetch('https://api.example.com/user/1')
  .then(response => response.json())
  .then(user => fetch(`https://api.example.com/posts?userId=${user.id}`))
  .then(response => response.json())
  .then(posts => console.log(posts))
  .catch(error => console.error(error));

// async/await
async function fetchUserData() {
  try {
    const userResponse = await fetch('https://api.example.com/user/1');
    const user = await userResponse.json();
    
    const postsResponse = await fetch(`https://api.example.com/posts?userId=${user.id}`);
    const posts = await postsResponse.json();
    
    return posts;
  } catch (error) {
    console.error("Error:", error);
  }
}

// Promise.all - 병렬 처리
async function fetchMultipleUsers() {
  const promises = [
    fetch('https://api.example.com/user/1'),
    fetch('https://api.example.com/user/2'),
    fetch('https://api.example.com/user/3')
  ];
  
  const responses = await Promise.all(promises);
  const users = await Promise.all(responses.map(r => r.json()));
  return users;
}

// Promise.race - 가장 빨리 완료되는 것
const fastestResponse = await Promise.race([
  fetch('https://api1.example.com/data'),
  fetch('https://api2.example.com/data')
]);

// Promise.allSettled - 모든 결과 (성공/실패 모두)
const results = await Promise.allSettled([
  Promise.resolve(1),
  Promise.reject('error'),
  Promise.resolve(3)
]);
```

### 모듈

javascript

```javascript
// export (user.js)
export const name = "John";
export function greet() {
  console.log("Hello!");
}

export default class User {
  constructor(name) {
    this.name = name;
  }
}

// import (main.js)
import User, { name, greet } from './user.js';
import * as UserModule from './user.js';

// Dynamic import
async function loadModule() {
  const module = await import('./user.js');
  const user = new module.default("Jane");
}
```

## 4단계: 실전 개념 (계속)

### 에러 처리

javascript

```javascript
// try-catch
try {
  const data = JSON.parse('invalid json');
} catch (error) {
  console.error('Parsing error:', error.message);
} finally {
  console.log('Cleanup');
}

// 커스텀 에러
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}

function validateAge(age) {
  if (age < 0 || age > 120) {
    throw new ValidationError('Invalid age');
  }
  return true;
}

try {
  validateAge(150);
} catch (error) {
  if (error instanceof ValidationError) {
    console.log('Validation failed:', error.message);
  } else {
    throw error;  // 다른 에러는 다시 던지기
  }
}

// async/await 에러 처리
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
    return null;
  }
}
```

### 정규표현식

javascript

```javascript
// 기본 패턴
const regex = /hello/;
const regex2 = new RegExp('hello');

// 플래그
// g: 전역 검색, i: 대소문자 무시, m: 여러 줄
const globalRegex = /hello/gi;

// 메서드
const str = "Hello World, hello";

console.log(regex.test(str));        // true
console.log(str.match(/hello/gi));   // ["Hello", "hello"]
console.log(str.search(/world/i));   // 6
console.log(str.replace(/hello/gi, 'hi'));  // "hi World, hi"

// 자주 사용하는 패턴
const emailRegex = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/;
const phoneRegex = /^\d{3}-\d{4}-\d{4}$/;
const urlRegex = /^https?:\/\/.+/;

// 그룹 캡처
const dateRegex = /(\d{4})-(\d{2})-(\d{2})/;
const match = "2024-01-15".match(dateRegex);
console.log(match[1]);  // "2024"
console.log(match[2]);  // "01"
console.log(match[3]);  // "15"
```

### Map과 Set

javascript

```javascript
// Map
const map = new Map();

map.set('name', 'John');
map.set('age', 30);
map.set(1, 'number key');

console.log(map.get('name'));   // "John"
console.log(map.has('age'));    // true
console.log(map.size);          // 3

map.delete('age');
map.clear();

// Map 순회
const userMap = new Map([
  ['name', 'John'],
  ['age', 30]
]);

for (const [key, value] of userMap) {
  console.log(key, value);
}

userMap.forEach((value, key) => {
  console.log(key, value);
});

// Set
const set = new Set();

set.add(1);
set.add(2);
set.add(2);  // 중복 무시

console.log(set.has(1));    // true
console.log(set.size);      // 2

set.delete(1);
set.clear();

// 배열 중복 제거
const numbers = [1, 2, 2, 3, 3, 4];
const unique = [...new Set(numbers)];  // [1, 2, 3, 4]

// WeakMap과 WeakSet (메모리 관리)
const weakMap = new WeakMap();
let obj = { name: 'John' };
weakMap.set(obj, 'metadata');
// obj = null; -> weakMap의 항목도 가비지 컬렉션 대상이 됨
```

## 학습 순서 추천

1. **기초 다지기** (1-2주): 변수, 데이터 타입, 연산자, 조건문, 반복문, 함수
2. **배열/객체 마스터** (1주): 배열/객체 메서드, 구조 분해, 스프레드
3. **고급 함수** (1주): 클로저, this, 콜백, 고차 함수
4. **비동기** (1-2주): Promise, async/await
5. **클래스와 프로토타입** (1주)
6. **실전 프로젝트**: ToDo 앱, API 연동 프로젝트

## 실습 프로젝트 예시

javascript

```javascript
// Todo List 만들기
class TodoList {
  constructor() {
    this.todos = [];
  }
  
  add(text) {
    const todo = {
      id: Date.now(),
      text,
      completed: false,
      createdAt: new Date()
    };
    this.todos.push(todo);
    return todo;
  }
  
  remove(id) {
    this.todos = this.todos.filter(todo => todo.id !== id);
  }
  
  toggle(id) {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
    }
  }
  
  getActive() {
    return this.todos.filter(todo => !todo.completed);
  }
  
  getCompleted() {
    return this.todos.filter(todo => todo.completed);
  }
}

const todoList = new TodoList();
todoList.add("Learn JavaScript");
todoList.add("Build a project");
```

어떤 주제부터 더 깊이 알고 싶으신가요?




---
THIS IS CLaUAD


## 📖 Phase 1: 핵심 기본 (빠른 복습)

### 1.1 변수와 데이터 타입



### 1.2 함수

- 함수 선언 vs 표현식 vs 화살표 함수
- 호이스팅(Hoisting)의 동작 원리
- this 바인딩 완전 정복
- 클로저(Closure)와 실무 활용
- **실무 케이스**: 이벤트 핸들러, 콜백 지옥 해결

### 1.3 객체와 배열

- 객체 생성 패턴
- 배열 메서드 완전 정복 (map, filter, reduce 등)
- 구조 분해 할당(Destructuring)
- 스프레드/레스트 연산자
- **실무 케이스**: API 응답 데이터 가공, 테이블 데이터 처리

---

## 📖 Phase 2: ES6+ 최신 문법

### 2.1 템플릿 리터럴 & 심볼

- 문자열 처리의 진화
- Symbol의 실무 활용

### 2.2 Promise와 비동기 처리

- 콜백 → Promise → async/await 진화 과정
- Promise 체이닝과 에러 핸들링
- Promise.all, Promise.race 실무 활용
- **실무 케이스**: API 병렬 호출, 순차 처리, 타임아웃 처리

### 2.3 모듈 시스템

- CommonJS vs ES6 Modules
- import/export 패턴
- 동적 import
- **실무 케이스**: 코드 스플리팅, 의존성 관리

### 2.4 클래스와 프로토타입

- 프로토타입 체인의 이해
- ES6 Class 문법
- 상속과 다형성
- **실무 케이스**: 도메인 모델 설계, OOP 패턴

### 2.5 제너레이터와 이터레이터

- 이터러블 프로토콜
- 제너레이터 함수
- **실무 케이스**: 대용량 데이터 스트리밍, 페이지네이션

---

## 📖 Phase 3: 고급 패턴 & 함수형 프로그래밍

### 3.1 함수형 프로그래밍 기초

- 순수 함수와 사이드 이펙트
- 불변성(Immutability)
- 고차 함수(Higher Order Function)
- 커링(Currying)과 부분 적용
- **실무 케이스**: Redux 리듀서, 유틸리티 함수 설계

### 3.2 고급 배열/객체 처리

- reduce의 고급 활용
- 함수 합성(Composition)
- 파이프라인 패턴
- **실무 케이스**: 복잡한 데이터 변환, 집계 로직

### 3.3 에러 처리 전략

- try-catch의 올바른 사용
- 커스텀 에러 클래스
- 에러 바운더리 패턴
- **실무 케이스**: 전역 에러 핸들링, 로깅 시스템

---

## 📖 Phase 4: 실무 디자인 패턴

### 4.1 생성 패턴

- 싱글톤(Singleton)
- 팩토리(Factory)
- 빌더(Builder)
- **실무 케이스**: DB 커넥션, API 클라이언트, 설정 관리

### 4.2 구조 패턴

- 모듈(Module)
- 프록시(Proxy)
- 데코레이터(Decorator)
- **실무 케이스**: 미들웨어, 로깅, 캐싱

### 4.3 행동 패턴

- 옵저버(Observer)
- 전략(Strategy)
- 책임 연쇄(Chain of Responsibility)
- **실무 케이스**: 이벤트 시스템, 알림 시스템, 권한 체크

### 4.4 MVC/MVVM 패턴

- 아키텍처 패턴 이해
- 관심사의 분리
- **실무 케이스**: ERP 모듈 구조 설계

---

## 📖 Phase 5: DOM & 브라우저 API

### 5.1 DOM 조작 완전 정복

- querySelector vs getElementById
- 이벤트 위임(Event Delegation)
- 성능 최적화 (DocumentFragment, Reflow 최소화)
- **실무 케이스**: 동적 테이블, 폼 검증, 대시보드

### 5.2 브라우저 API

- LocalStorage/SessionStorage
- Fetch API와 HTTP 통신
- File API (파일 업로드/다운로드)
- **실무 케이스**: 엑셀 업로드, PDF 다운로드, 오프라인 저장

### 5.3 성능 최적화

- Debounce & Throttle
- 메모이제이션
- Lazy Loading
- **실무 케이스**: 검색 자동완성, 무한 스크롤