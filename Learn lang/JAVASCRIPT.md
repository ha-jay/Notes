
# 변수와 데이터 타입
- var, let, const의 차이와 스코프
- 원시형 vs 참조형 데이터
- 타입 변환과 동등성 비교 (==, ===)
- **실무 케이스**: 불변성 관리, 상태 관리


```javascript
// 변수 선언
let name = "John";        // 재할당 가능
const age = 30;           // 재할당 불가 (권장)
var oldWay = "legacy";    // 사용 지양

// 데이터 타입
const string = "Hello";
const number = 42;
const boolean = true;
const nullValue = null;
const undefinedValue = undefined;
const symbol = Symbol("unique");
const bigint = 1234567890123456789012345678901234567890n;

// 객체
const person = {
  name: "John",
  age: 30,
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

// 배열
const numbers = [1, 2, 3, 4, 5];
const mixed = [1, "two", true, { key: "value" }];

// typeof 연산자
console.log(typeof "hello");  // "string"
console.log(typeof 42);       // "number"
console.log(typeof true);     // "boolean"
console.log(typeof {});       // "object"
console.log(typeof []);       // "object" (주의!)
console.log(typeof null);     // "object" (버그, 역사적 이유)
```

## var, let, const의 차이와 스코프

### 1️⃣ 무엇인가? (What)

`var`, `let`, `const`는 JavaScript에서 **변수를 선언하는 키워드**입니다. 같은 목적이지만 **스코프, 재할당, 호이스팅** 측면에서 중요한 차이가 있습니다.

### 2️⃣ 왜 필요한가? (Why)

과거 JavaScript는 `var`만 존재했는데, 여러 문제점이 있었습니다:

- 함수 스코프만 지원 (블록 스코프 미지원)
- 재선언이 가능해서 실수로 변수 덮어쓰기 발생
- 호이스팅으로 인한 예상치 못한 동작

ES6(2015)에서 `let`과 `const`가 추가되어 **더 안전하고 예측 가능한 코드 작성**이 가능해졌습니다.


### 3️⃣ 배경 지식 (Background)  스코프(Scope)란?

변수가 **유효한 범위**를 의미합니다.

- **전역 스코프**: 어디서든 접근 가능
- **함수 스코프**: 함수 내부에서만 접근 가능
- **블록 스코프**: `{}` 블록 내부에서만 접근 가능

### 4️⃣ 어떻게 동작하는가? (How it works) - var, let , const

var의 동작 방식
```javascript
// 1. 함수 스코프
function example() {
  var x = 10;
  if (true) {
    var x = 20; // 같은 변수를 재선언!
    console.log(x); // 20
  }
  console.log(x); // 20 (덮어씌워짐!)
}

// 2. 호이스팅
console.log(name); // undefined (에러 아님!)
var name = 'John';

// 위 코드는 실제로 이렇게 동작:
// var name; // 호이스팅됨
// console.log(name); // undefined
// name = 'John';
```

 let의 동작 방식
```javascript
// 1. 블록 스코프
function example() {
  let x = 10;
  if (true) {
    let x = 20; // 새로운 변수 (블록 스코프)
    console.log(x); // 20
  }
  console.log(x); // 10 (원래 값 유지)
}

// 2. TDZ (Temporal Dead Zone)
console.log(age); // ReferenceError! (호이스팅은 되지만 초기화 전까지 접근 불가)
let age = 25;

// 3. 재할당 가능, 재선언 불가
let count = 1;
count = 2; // OK
let count = 3; // SyntaxError!
```

const의 동작 방식

```javascript
// 1. 블록 스코프 (let과 동일)
if (true) {
  const PI = 3.14;
  console.log(PI); // 3.14
}
// console.log(PI); // ReferenceError

// 2. 재할당 불가
const API_KEY = 'abc123';
API_KEY = 'xyz789'; // TypeError!

// 3. 선언 시 초기화 필수
const value; // SyntaxError! (초기값 없음)
const value = 10; // OK

// 4. 객체/배열의 내부는 변경 가능!
const user = { name: 'Alice' };
user.name = 'Bob'; // OK! (재할당이 아닌 속성 변경)
user.age = 30; // OK!
user = {}; // TypeError! (재할당 시도)

const arr = [1, 2, 3];
arr.push(4); // OK! [1, 2, 3, 4]
arr = []; // TypeError!
```


### 6️⃣ 실무에서 사용하는 방법 (Best Practice)

```javascript
// 1. 기본적으로 const 사용
const MAX_RETRY = 3;
const API_URL = 'https://api.example.com';

// 2. 재할당이 필요할 때만 let 사용
let currentPage = 1;
currentPage = 2; // OK

// 3. var는 사용하지 않기 (레거시 코드에만 존재)
```


### 실무 케이스 1: 반복문 - let 사용


```javascript
// ❌ var 사용 시 문제
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// 출력: 3, 3, 3 (모두 같은 i를 참조)

// ✅ let 사용
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// 출력: 0, 1, 2 (각각 독립적인 i)
```

### 🔥 실무 케이스 2: 설정 객체 불변성 - 불변 Object.freeze()


```javascript
// API 설정 (변경되면 안 됨)
const config = {
  timeout: 3000,
  retries: 3,
  baseURL: 'https://api.example.com'
};

// 만약 완전 불변으로 만들고 싶다면
const frozenConfig = Object.freeze(config);
frozenConfig.timeout = 5000; // 무시됨 (strict mode에서는 에러)
```

### 🔥 실무 케이스 3:  상태 관리 - 상태를 명시하는 객체이용

```javascript
// ❌ 나쁜 예
var orderStatus = 'pending';
if (payment) {
  var orderStatus = 'paid'; // 실수로 덮어씀
}

// ✅ 좋은 예
let orderStatus = 'pending';
if (payment) {
  orderStatus = 'paid'; // 의도적 재할당
}

// ✅ 더 좋은 예 (상태 전환)
const ORDER_STATUS = {
  PENDING: 'pending',
  PAID: 'paid',
  SHIPPED: 'shipped',
  COMPLETED: 'completed'
};

let currentStatus = ORDER_STATUS.PENDING;
currentStatus = ORDER_STATUS.PAID;
```


### CASE 4: 클로저와 스코프
```js
function createCounter() {
  let count = 0; // 외부에서 접근 불가 (private)
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getValue: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getValue());  // 2
// console.log(count); // ReferenceError
```

### CASE 5: 조건부 변수 선언 - if문 활용, 조건만족시에만 변수선언
avascript

```javascript
const userRole = 'admin';

if (userRole === 'admin') {
  const permissions = ['read', 'write', 'delete'];
  console.log(permissions); // OK
}

// console.log(permissions); // ReferenceError (블록 밖에서 접근 불가)
```

### CASE 6: 배열/객체 const의 활용

```javascript
// ERP 주문 데이터
const order = {
  id: 1001,
  customer: 'ABC Corp',
  items: []
};

// 주문 항목 추가 (OK)
order.items.push({ product: 'Widget A', qty: 10 });
order.items.push({ product: 'Widget B', qty: 5 });

// 총액 계산 추가 (OK)
order.total = order.items.reduce((sum, item) => sum + item.qty, 0);

console.log(order);
// { id: 1001, customer: 'ABC Corp', items: [...], total: 15 }

// ❌ 전체 교체는 불가
// order = { id: 1002 }; // TypeError
```



## 원시형 vs 참조형 데이터 타입

### 1️⃣ 무엇인가? (What)

JavaScript의 데이터 타입은 크게 **2가지**로 나뉩니다:

### 원시형 (Primitive Type) - 7가지

1. `string` - 문자열
2. `number` - 숫자
3. `boolean` - 불리언 (true/false)
4. `undefined` - 정의되지 않음
5. `null` - 명시적 빈 값
6. `symbol` - 고유한 식별자 (ES6)
7. `bigint` - 큰 정수 (ES2020)

### 참조형 (Reference Type)

- `Object` (객체)
    - Array (배열)
    - Function (함수)
    - Date, RegExp, Map, Set 등

### 2️⃣ 왜 필요한가? (Why)

이 차이를 이해하지 못하면:

- **예상치 못한 버그 발생** (값이 의도치 않게 변경됨)
- **메모리 누수** (객체가 제대로 해제되지 않음)
- **성능 문제** (불필요한 복사)
- **상태 관리 실패** (React, Redux 등에서 치명적)

javascript

````javascript
// ❌ 예상치 못한 버그
const user1 = { name: 'Alice' };
const user2 = user1; // 참조 복사!
user2.name = 'Bob';
console.log(user1.name); // 'Bob' (원본도 변경됨!)
```

---

## 3️⃣ 배경 지식 (Background)

### 메모리 구조
```
Stack (스택)                Call Stack (콜 스택)
┌─────────────┐            ┌──────────────┐
│ 원시형 값   │            │ 함수 호출    │
│ 참조형 주소 │            │ 실행 컨텍스트│
└─────────────┘            └──────────────┘
                                    
Heap (힙)
┌─────────────────────────┐
│ 객체, 배열, 함수 등     │
│ (실제 데이터)           │
└─────────────────────────┘
````

- **원시형**: 스택에 **값 자체**를 저장
- **참조형**: 스택에 **힙의 주소**를 저장, 힙에 실제 데이터 저장


### 4️⃣ 어떻게 동작하는가? (How it works)
원시형의 동작

```javascript
// 1. 값 복사 (Value Copy)
let a = 10;
let b = a; // a의 값을 복사
b = 20;

console.log(a); // 10 (원본 유지)
console.log(b); // 20 (복사본만 변경)

// 메모리 상태:
// Stack: [a: 10] [b: 20] ← 독립적인 값
```


```javascript
// 2. 불변성 (Immutable)
let str = 'Hello';
str[0] = 'h'; // 동작하지 않음!
console.log(str); // 'Hello' (변경 안 됨)

// 새로운 값을 만들어야 함
str = 'hello'; // 새로운 문자열 생성
```

```javascript
// 3. 비교 연산
let x = 10;
let y = 10;
console.log(x === y); // true (값이 같으면 true)

let str1 = 'hello';
let str2 = 'hello';
console.log(str1 === str2); // true
```

---

 참조형의 동작


```javascript
// 1. 참조 복사 (Reference Copy)
let obj1 = { value: 10 };
let obj2 = obj1; // 주소를 복사! (같은 객체를 가리킴)
obj2.value = 20;

console.log(obj1.value); // 20 (원본도 변경됨!)
console.log(obj2.value); // 20

// 메모리 상태:
// Stack: [obj1: 0x001] [obj2: 0x001] ← 같은 주소
// Heap:  [0x001: { value: 20 }]
```


```javascript
// 2. 가변성 (Mutable)
const person = { name: 'Alice' };
person.name = 'Bob'; // OK! (객체 내부 변경)
person.age = 30;     // OK! (속성 추가)

// person = {}; // TypeError! (재할당은 불가)
```


```javascript
// 3. 비교 연산
let arr1 = [1, 2, 3];
let arr2 = [1, 2, 3];
console.log(arr1 === arr2); // false (다른 객체)

let arr3 = arr1;
console.log(arr1 === arr3); // true (같은 객체)
```

---

### 5️⃣ 비교 표

| 특성        | 원시형            | 참조형                  |
| --------- | -------------- | -------------------- |
| **저장 위치** | 스택 (값 자체)      | 스택 (주소) + 힙 (실제 데이터) |
| **복사**    | 값 복사 (독립)      | 주소 복사 (공유)           |
| **불변성**   | 불변 (Immutable) | 가변 (Mutable)         |
| **비교**    | 값 비교           | 주소 비교                |
| **크기**    | 고정 (작음)        | 동적 (클 수 있음)          |
| **예시**    | 10, 'hi', true | {}, [], function     |
### 6️⃣ 실무에서 사용하는 방법

### 🔥 실무 케이스 1: 객체 복사

javascript

```javascript
// ❌ 얕은 복사 문제
const original = {
  name: 'Alice',
  address: { city: 'Seoul' }
};

const copy = { ...original }; // Spread 연산자
copy.name = 'Bob'; // OK (1단계는 복사됨)
copy.address.city = 'Busan'; // ❌ 원본도 변경됨! (중첩 객체는 참조)

console.log(original.address.city); // 'Busan'
```

javascript

```javascript
// ✅ 깊은 복사 (Deep Copy)

// 방법 1: JSON 사용 (간단하지만 한계 있음)
const deepCopy1 = JSON.parse(JSON.stringify(original));
// 한계: Date, Function, undefined, Symbol 등은 복사 안 됨

// 방법 2: 재귀 함수
function deepCopy(obj) {
  if (obj === null || typeof obj !== 'object') return obj;
  
  if (Array.isArray(obj)) {
    return obj.map(item => deepCopy(item));
  }
  
  const copied = {};
  for (let key in obj) {
    copied[key] = deepCopy(obj[key]);
  }
  return copied;
}

// 방법 3: structuredClone (최신 - 2022)
const deepCopy3 = structuredClone(original);
```

---

### 🔥 실무 케이스 2: 배열 복사

javascript

```javascript
const orders = [
  { id: 1, status: 'pending' },
  { id: 2, status: 'paid' }
];

// ❌ 참조 복사
const ordersCopy1 = orders;
ordersCopy1[0].status = 'shipped';
console.log(orders[0].status); // 'shipped' (원본 변경됨!)

// ✅ 얕은 복사 (1단계만)
const ordersCopy2 = [...orders]; // or orders.slice()
ordersCopy2[0].status = 'completed'; // ❌ 여전히 원본 변경됨 (객체는 참조)

// ✅ 깊은 복사
const ordersCopy3 = orders.map(order => ({ ...order }));
ordersCopy3[0].status = 'cancelled'; // OK (원본 유지)
```

---

### 🔥 실무 케이스 3: 함수 파라미터

javascript

```javascript
// 원시형 파라미터
function increment(num) {
  num = num + 1; // 복사본만 변경
  return num;
}

let count = 5;
increment(count);
console.log(count); // 5 (원본 유지)

// 참조형 파라미터
function updateUser(user) {
  user.status = 'active'; // ❌ 원본 변경됨!
}

const currentUser = { name: 'Alice', status: 'pending' };
updateUser(currentUser);
console.log(currentUser.status); // 'active'

// ✅ 불변성 유지 (Immutability)
function updateUserImmutable(user) {
  return { ...user, status: 'active' }; // 새 객체 반환
}

const newUser = updateUserImmutable(currentUser);
console.log(currentUser.status); // 'pending' (원본 유지)
console.log(newUser.status);     // 'active'
```

---

### 🔥 실무 케이스 4: ERP 데이터 관리

javascript

```javascript
// ❌ 잘못된 상태 관리
let inventory = [
  { productId: 'A001', stock: 100 },
  { productId: 'A002', stock: 50 }
];

function reduceStock(productId, quantity) {
  const product = inventory.find(p => p.productId === productId);
  product.stock -= quantity; // ❌ 원본 직접 수정 (추적 어려움)
}

// ✅ 올바른 상태 관리 (불변성)
function reduceStockImmutable(inventory, productId, quantity) {
  return inventory.map(product => 
    product.productId === productId
      ? { ...product, stock: product.stock - quantity }
      : product
  );
}

inventory = reduceStockImmutable(inventory, 'A001', 10);
// 새로운 배열 반환, 원본은 유지 (히스토리 관리 가능)
```

---

### 7️⃣ 다양한 CASE 예제

### CASE 1: null vs undefined

javascript

```javascript
let a; // 선언만
console.log(a); // undefined (자동 할당)
console.log(typeof a); // 'undefined'

let b = null; // 명시적 빈 값
console.log(b); // null
console.log(typeof b); // 'object' (JavaScript 버그!)

// 실무에서 구분
function getUser(id) {
  if (!id) return undefined; // 잘못된 입력
  
  const user = database.find(id);
  if (!user) return null; // 찾을 수 없음 (의도적)
  
  return user;
}
```

---

### CASE 2: 배열은 객체다

javascript

```javascript
const arr = [1, 2, 3];
console.log(typeof arr); // 'object'
console.log(Array.isArray(arr)); // true

// 배열도 참조형
const arr2 = arr;
arr2.push(4);
console.log(arr); // [1, 2, 3, 4]
```

---

### CASE 3: 함수도 객체다

javascript

```javascript
function greet(name) {
  return `Hello, ${name}`;
}

// 함수에 속성 추가 가능!
greet.language = 'Korean';
greet.version = '1.0';

console.log(greet.language); // 'Korean'
console.log(typeof greet); // 'function'
```

---

### CASE 4: 실무 - React State 불변성

javascript

```javascript
// ❌ React에서 절대 하면 안 되는 것
const [user, setUser] = useState({ name: 'Alice', age: 25 });

function updateAge() {
  user.age = 26; // ❌ 직접 수정 (React가 감지 못함)
  setUser(user); // ❌ 같은 참조 (리렌더링 안 됨)
}

// ✅ 올바른 방법
function updateAge() {
  setUser({ ...user, age: 26 }); // 새 객체 생성
}

// ✅ 배열 업데이트
const [items, setItems] = useState([1, 2, 3]);

// ❌ items.push(4); setItems(items);
// ✅ setItems([...items, 4]);
```

---

### CASE 5: 비교 연산 함정

javascript

```javascript
// 원시형
console.log(5 === 5); // true
console.log('hello' === 'hello'); // true

// 참조형
console.log({} === {}); // false (다른 객체)
console.log([] === []); // false
console.log([1,2] === [1,2]); // false

// 객체 비교 방법
const obj1 = { a: 1 };
const obj2 = { a: 1 };

// 1. 얕은 비교
JSON.stringify(obj1) === JSON.stringify(obj2); // true

// 2. 라이브러리 사용 (lodash)
_.isEqual(obj1, obj2); // true

// 3. 직접 구현
function shallowEqual(obj1, obj2) {
  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);
  
  if (keys1.length !== keys2.length) return false;
  
  return keys1.every(key => obj1[key] === obj2[key]);
}
```

---

### 📝 학습 검증 예제 문제

javascript

```javascript
// 문제 1: 다음 코드의 출력은?
let x = 10;
let y = x;
y = 20;
console.log(x);

// 문제 2: 다음 코드의 출력은?
const arr1 = [1, 2, 3];
const arr2 = arr1;
arr2.push(4);
console.log(arr1.length);

// 문제 3: 다음 코드의 출력은?
const obj = { a: { b: 1 } };
const copy = { ...obj };
copy.a.b = 2;
console.log(obj.a.b);

// 문제 4: 다음 코드의 문제점을 찾고 수정하세요 (실무 시나리오)
function processOrders(orders) {
  orders.forEach(order => {
    order.total = order.items.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0);
  });
  return orders;
}

const myOrders = [
  { id: 1, items: [{ price: 100, quantity: 2 }] }
];

const processed = processOrders(myOrders);
console.log(myOrders[0].total); // 원본이 변경됨!

// 문제 5: 깊은 복사 함수 구현하기
// 다음 객체를 완벽하게 복사하는 함수를 작성하세요
const complex = {
  name: 'Product',
  price: 1000,
  tags: ['new', 'sale'],
  supplier: {
    name: 'ABC Corp',
    address: { city: 'Seoul' }
  }
};
```


### 📝 정답 및 해설

### 문제 1 ⭕

javascript

```javascript
let x = 10;
let y = x;
y = 20;
console.log(x);
```

**정답**: `10` ✅

**해설**: 원시형은 **값 복사**이므로 `y`를 변경해도 `x`는 영향받지 않습니다!

---

### 문제 2 ⭕

javascript

```javascript
const arr1 = [1, 2, 3];
const arr2 = arr1;
arr2.push(4);
console.log(arr1.length);
```

**정답**: `4` ✅

**해설**: 배열은 참조형이므로 `arr2`는 `arr1`과 같은 객체를 가리킵니다. `arr2.push(4)`하면 원본 `arr1`도 변경됩니다!

```
메모리 상태:
Stack: [arr1: 0x001] [arr2: 0x001] ← 같은 주소
Heap:  [0x001: [1, 2, 3, 4]]
```

---

### 문제 3 ⭕

javascript

```javascript
const obj = { a: { b: 1 } };
const copy = { ...obj };
copy.a.b = 2;
console.log(obj.a.b);
```

**정답**: `2` ✅

**해설**: **얕은 복사(Shallow Copy)의 함정!**

javascript

```javascript
// Spread 연산자는 1단계만 복사
const copy = { ...obj };

// 실제 메모리 상태:
// obj.a와 copy.a는 같은 객체를 가리킴!
obj.a === copy.a // true

// 따라서 copy.a.b를 변경하면 obj.a.b도 변경됨
```

**시각화**:

```
obj = {
  a: 0x100  ← 주소 참조
}

copy = {
  a: 0x100  ← 같은 주소!
}

Heap [0x100]: { b: 2 }  ← 공유됨
```

---

### 문제 4: 실무 시나리오 수정

**원본 코드의 문제점**:

javascript

```javascript
function processOrders(orders) {
  orders.forEach(order => {
    order.total = order.items.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0);
  });
  return orders; // ❌ 원본 배열을 직접 수정함!
}
```

**문제점**:

1. **원본 데이터를 직접 수정** (Side Effect 발생)
2. **불변성 위반** (데이터 추적 어려움)
3. **예측 불가능한 동작** (다른 곳에서 같은 데이터 사용 시 버그)

---

**✅ 수정된 코드 (불변성 유지)**:

javascript

```javascript
// 방법 1: map으로 새 배열 생성
function processOrders(orders) {
  return orders.map(order => ({
    ...order, // 기존 속성 복사
    total: order.items.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0)
  }));
}

const myOrders = [
  { id: 1, items: [{ price: 100, quantity: 2 }] }
];

const processed = processOrders(myOrders);
console.log(myOrders[0].total); // undefined (원본 유지!)
console.log(processed[0].total); // 200 (새 배열)
```

javascript

```javascript
// 방법 2: 깊은 복사 후 처리 (중첩 객체가 있는 경우)
function processOrdersDeep(orders) {
  const ordersCopy = structuredClone(orders); // 깊은 복사
  
  ordersCopy.forEach(order => {
    order.total = order.items.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0);
  });
  
  return ordersCopy;
}
```

javascript

```javascript
// 방법 3: 함수형 프로그래밍 스타일 (최고!)
function processOrdersPure(orders) {
  const calculateTotal = (items) => 
    items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  return orders.map(order => ({
    ...order,
    total: calculateTotal(order.items)
  }));
}
```

---

### 문제 5: 깊은 복사 함수 구현

javascript

```javascript
const complex = {
  name: 'Product',
  price: 1000,
  tags: ['new', 'sale'],
  supplier: {
    name: 'ABC Corp',
    address: { city: 'Seoul' }
  }
};
```

**✅ 정답**:

javascript

```javascript
// 방법 1: 재귀 함수로 구현 (완전한 깊은 복사)
function deepClone(obj) {
  // 1. null 또는 원시형이면 그대로 반환
  if (obj === null || typeof obj !== 'object') {
    return obj;
  }
  
  // 2. Date 객체
  if (obj instanceof Date) {
    return new Date(obj);
  }
  
  // 3. 배열
  if (Array.isArray(obj)) {
    return obj.map(item => deepClone(item));
  }
  
  // 4. 일반 객체
  const cloned = {};
  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      cloned[key] = deepClone(obj[key]);
    }
  }
  
  return cloned;
}

// 사용
const cloned = deepClone(complex);
cloned.supplier.address.city = 'Busan';

console.log(complex.supplier.address.city); // 'Seoul' (원본 유지!)
console.log(cloned.supplier.address.city);  // 'Busan'
```

javascript

```javascript
// 방법 2: structuredClone (최신 브라우저/Node.js 17+)
const cloned = structuredClone(complex);

// 장점: 간단, Date, Map, Set 등 지원
// 단점: 함수, Symbol, DOM 노드 등은 복사 안 됨
```

javascript

```javascript
// 방법 3: JSON 방식 (간단하지만 제한적)
const cloned = JSON.parse(JSON.stringify(complex));

// 장점: 간단
// 단점: 
// - undefined, Symbol, 함수는 사라짐
// - Date는 문자열로 변환됨
// - 순환 참조 에러
```


### 🎯 실무 비교표

| 방법                    | 장점          | 단점         | 사용 상황  |
| --------------------- | ----------- | ---------- | ------ |
| **Spread `{...obj}`** | 간단, 빠름      | 1단계만 복사    | 단순 객체  |
| **JSON**              | 간단          | 함수/Date 손실 | 순수 데이터 |
| **structuredClone**   | Date/Map 지원 | 함수 복사 안 됨  | 대부분 상황 |
| **재귀 함수**             | 완벽한 제어      | 복잡함        | 특수한 경우 |

### 💡 핵심 정리

javascript

```javascript
// ❌ 실무에서 자주 하는 실수
const original = { nested: { value: 1 } };
const copy = { ...original }; // 얕은 복사!
copy.nested.value = 2; // 원본도 변경됨!

// ✅ 올바른 방법
const copy = structuredClone(original); // 깊은 복사
// 또는
const copy = {
  ...original,
  nested: { ...original.nested } // 중첩 객체도 복사
};
```


## 타입 변환과 동등성 비교 (`==, ===`)

### 1️⃣ 무엇인가? (What)

JavaScript는 **동적 타입 언어**로, 타입이 자동으로 변환됩니다.

 타입 변환 (Type Conversion)

1. **명시적 변환 (Explicit)** - 개발자가 의도적으로 변환
2. **암묵적 변환 (Implicit)** - JavaScript가 자동으로 변환

 비교 연산자

1. **`==` (동등 연산자)** - 타입 변환 후 비교 (Loose Equality)
2. **`===` (일치 연산자)** - 타입까지 엄격하게 비교 (Strict Equality)

### 2️⃣ 왜 필요한가? (Why)

JavaScript의 타입 변환과 비교는 **매우 복잡하고 예측하기 어렵습니다**.

javascript

```javascript
// 😱 충격적인 JavaScript
console.log([] == ![]);     // true (???)
console.log(0 == '0');      // true
console.log(0 == []);       // true
console.log('0' == []);     // false (???)
```

이런 동작을 이해하지 못하면:

- **예상치 못한 버그** 발생
- **조건문 오작동**
- **데이터 검증 실패**
- **보안 취약점** (인증/권한 체크 실패)

**실무 예시**:

javascript

```javascript
// ❌ 위험한 코드
if (userInput == 0) {
  // '', false, null 등도 통과됨!
}

// ✅ 안전한 코드
if (userInput === 0) {
  // 정확히 숫자 0만 통과
}
```

### 3️⃣ 배경 지식 (Background)  Truthy와 Falsy

JavaScript에서는 모든 값이 **boolean 컨텍스트**에서 true 또는 false로 평가됩니다.

**Falsy 값 (8개만 기억하세요!)**:

javascript

```javascript
false
0
-0
0n (BigInt 0)
'' (빈 문자열)
null
undefined
NaN
```

**Truthy 값**: Falsy가 아닌 모든 값!

javascript

```javascript
true
1, -1, 3.14 (0이 아닌 모든 숫자)
'hello', '0', 'false' (빈 문자열이 아닌 모든 문자열)
[], {} (빈 배열/객체도!)
function() {}
```

### 4️⃣ 어떻게 동작하는가? (How it works) 타입 변환 규칙

#### 1) 문자열 변환

javascript    

```javascript
// 명시적 변환
String(123);        // '123'
String(true);       // 'true'
String(null);       // 'null'
String(undefined);  // 'undefined'
String([1, 2, 3]);  // '1,2,3'
String({ a: 1 });   // '[object Object]'

// 암묵적 변환 (+ 연산자)
123 + '';           // '123'
true + ' story';    // 'true story'
[1, 2] + [3, 4];    // '1,23,4'
```

#### 2) 숫자 변환

javascript

```javascript
// 명시적 변환
Number('123');      // 123
Number('12.5');     // 12.5
Number('');         // 0 (!)
Number('  ');       // 0 (!)
Number('hello');    // NaN
Number(true);       // 1
Number(false);      // 0
Number(null);       // 0 (!)
Number(undefined);  // NaN
Number([]);         // 0 (!)
Number([5]);        // 5 (!)
Number([1, 2]);     // NaN

// 암묵적 변환
+'123';             // 123
'5' * 2;            // 10
'10' / '2';         // 5
'5' - 3;            // 2
```

#### 3) 불리언 변환

javascript

```javascript
// 명시적 변환
Boolean(1);         // true
Boolean(0);         // false
Boolean('hello');   // true
Boolean('');        // false
Boolean([]);        // true (!)
Boolean({});        // true (!)

// 암묵적 변환
!!'hello';          // true (이중 부정)
if ('0') { }        // 실행됨 ('0'은 truthy)
```

---

### == vs === 비교

#### === (일치 연산자) - 권장!

javascript

```javascript
// 타입과 값 모두 같아야 true
5 === 5;            // true
5 === '5';          // false (타입 다름)
null === null;      // true
undefined === undefined; // true
null === undefined; // false (타입 다름)
NaN === NaN;        // false (특수 케이스!)

// 객체는 참조 비교
{} === {};          // false
[] === [];          // false
```

#### == (동등 연산자) - 피하세요!

javascript

```javascript
// 타입 변환 후 비교
5 == '5';           // true (문자열 → 숫자)
0 == false;         // true
0 == '';            // true
0 == '0';           // true
'' == '0';          // false (???)
false == 'false';   // false (???)
false == '0';       // true (???)

// null과 undefined는 특별 취급
null == undefined;  // true (!)
null == 0;          // false
undefined == 0;     // false
```

---

### 복잡한 변환 예제

javascript

```javascript
// 배열의 타입 변환
[] + [];            // '' (빈 문자열)
[] + {};            // '[object Object]'
{} + [];            // 0 (브라우저에 따라 다를 수 있음)

// 논리 연산자
true + true;        // 2 (1 + 1)
true + false;       // 1 (1 + 0)
'5' + 3;            // '53' (문자열 연결)
'5' - 3;            // 2 (숫자 변환)

// 비교 연산
'2' > '12';         // true (문자열 비교!)
'2' > 12;           // false (숫자 변환)
```

---

## 5️⃣ 실무에서 사용하는 방법

### 🔥 실무 케이스 1: 입력 값 검증

javascript

```javascript
// ❌ 잘못된 검증
function validateAge(age) {
  if (age == 0) return false; // '', false, null도 통과
  return true;
}

validateAge('');    // false (의도하지 않음)
validateAge(null);  // false (의도하지 않음)

// ✅ 올바른 검증
function validateAgeCorrect(age) {
  // 1. 타입 체크
  if (typeof age !== 'number') return false;
  
  // 2. 범위 체크
  if (age <= 0 || age > 150) return false;
  
  // 3. NaN 체크
  if (Number.isNaN(age)) return false;
  
  return true;
}
```

---

### 🔥 실무 케이스 2: null/undefined 체크

javascript

```javascript
// ❌ 잘못된 방법
if (user.name == null) {
  // null과 undefined 모두 true
}

// ✅ 권장 방법 1: 각각 체크
if (user.name === null || user.name === undefined) {
  console.log('값이 없습니다');
}

// ✅ 권장 방법 2: Nullish Coalescing (ES2020)
const name = user.name ?? 'Guest'; // null이나 undefined면 'Guest'

// ❌ 주의: OR 연산자는 falsy 전체를 체크
const name2 = user.name || 'Guest'; // '', 0도 'Guest'로 변환됨

// ✅ 권장 방법 3: Optional Chaining (ES2020)
const city = user?.address?.city ?? 'Unknown';
```

---

### 🔥 실무 케이스 3: 배열/객체 존재 체크

javascript

```javascript
// ❌ 잘못된 방법
if (arr == null) { } // null과 undefined만 체크

// ✅ 배열 체크
if (Array.isArray(arr) && arr.length > 0) {
  console.log('유효한 배열');
}

// ✅ 객체 체크
if (obj && typeof obj === 'object' && !Array.isArray(obj)) {
  console.log('유효한 객체');
}
```

---

### 🔥 실무 케이스 4: ERP 데이터 처리

javascript

```javascript
// 주문 금액 계산
function calculateTotal(items) {
  // ❌ 잘못된 코드
  let total = 0;
  items.forEach(item => {
    total = total + item.price; // item.price가 '100'이면?
    // total = 0 + '100' = '0100' (문자열!)
  });
  return total;
  
  // ✅ 올바른 코드
  let total = 0;
  items.forEach(item => {
    const price = Number(item.price);
    if (Number.isNaN(price)) {
      throw new Error(`Invalid price: ${item.price}`);
    }
    total += price;
  });
  return total;
}
```

javascript

```javascript
// 재고 확인
function checkStock(productId) {
  const product = inventory.find(p => p.id === productId);
  
  // ❌ 잘못된 체크
  if (!product.stock) {
    // stock이 0이면 true (재고 있는데 없다고 판단!)
  }
  
  // ✅ 올바른 체크
  if (product.stock === 0) {
    return '재고 없음';
  }
  
  if (product.stock == null) {
    return '재고 정보 없음';
  }
  
  return `재고: ${product.stock}개`;
}
```

---

### 🔥 실무 케이스 5: 조건부 렌더링

javascript

```javascript
// React 예시
function OrderList({ orders }) {
  // ❌ 잘못된 코드
  return (
    <div>
      {orders.length && <OrderTable data={orders} />}
    </div>
  );
  // orders.length가 0이면 화면에 "0"이 표시됨!
  
  // ✅ 올바른 코드
  return (
    <div>
      {orders.length > 0 && <OrderTable data={orders} />}
    </div>
  );
  
  // ✅ 또는 불리언 변환
  return (
    <div>
      {!!orders.length && <OrderTable data={orders} />}
    </div>
  );
}
```

---

## 6️⃣ 다양한 CASE 예제

### CASE 1: 타입 체크 헬퍼 함수

javascript

```javascript
// 실무에서 자주 사용하는 유틸리티 함수
const TypeCheck = {
  isString: (value) => typeof value === 'string',
  
  isNumber: (value) => typeof value === 'number' && !Number.isNaN(value),
  
  isBoolean: (value) => typeof value === 'boolean',
  
  isArray: (value) => Array.isArray(value),
  
  isObject: (value) => 
    value !== null && typeof value === 'object' && !Array.isArray(value),
  
  isFunction: (value) => typeof value === 'function',
  
  isNull: (value) => value === null,
  
  isUndefined: (value) => value === undefined,
  
  isNullish: (value) => value === null || value === undefined,
  
  isEmpty: (value) => {
    if (value === null || value === undefined) return true;
    if (typeof value === 'string') return value.trim() === '';
    if (Array.isArray(value)) return value.length === 0;
    if (typeof value === 'object') return Object.keys(value).length === 0;
    return false;
  }
};

// 사용 예시
TypeCheck.isNumber('123');  // false
TypeCheck.isNumber(123);    // true
TypeCheck.isEmpty([]);      // true
TypeCheck.isEmpty([1]);     // false
```

---

### CASE 2: 안전한 숫자 변환

javascript

```javascript
function toNumber(value, defaultValue = 0) {
  // null이나 undefined면 기본값
  if (value == null) return defaultValue;
  
  // 이미 숫자면 그대로
  if (typeof value === 'number') return value;
  
  // 문자열 변환 시도
  if (typeof value === 'string') {
    const trimmed = value.trim();
    if (trimmed === '') return defaultValue;
    
    const num = Number(trimmed);
    return Number.isNaN(num) ? defaultValue : num;
  }
  
  // 그 외는 기본값
  return defaultValue;
}

// 사용 예시
toNumber('123');      // 123
toNumber('  456  ');  // 456
toNumber('abc');      // 0
toNumber('abc', -1);  // -1
toNumber(null);       // 0
toNumber('');         // 0
```

---

### CASE 3: 비교 함수 (정렬 시 사용)

javascript

```javascript
// 주문을 날짜 순으로 정렬
const orders = [
  { date: '2024-01-15', amount: 1000 },
  { date: '2024-01-10', amount: 2000 },
  { date: '2024-01-20', amount: 1500 }
];

// ❌ 잘못된 비교 (문자열 비교)
orders.sort((a, b) => a.date > b.date ? 1 : -1);

// ✅ 올바른 비교 (날짜 객체로 변환)
orders.sort((a, b) => {
  const dateA = new Date(a.date);
  const dateB = new Date(b.date);
  return dateA - dateB; // 숫자로 변환됨
});

// ✅ 금액 순 정렬 (안전한 숫자 비교)
orders.sort((a, b) => {
  const amountA = Number(a.amount) || 0;
  const amountB = Number(b.amount) || 0;
  return amountA - amountB;
});
```

---

## 📝 학습 검증 예제 문제

javascript

```javascript
// 문제 1: 다음 코드의 출력은?
console.log(0 == '0');
console.log(0 === '0');
console.log(false == '0');
console.log(false === '0');

// 문제 2: 다음 코드의 출력은?
console.log([] == false);
console.log([] === false);
console.log(!![]);

// 문제 3: 다음 코드의 출력은?
console.log(null == undefined);
console.log(null === undefined);
console.log(null == 0);
console.log(undefined == 0);

// 문제 4: 다음 코드의 문제점을 찾고 수정하세요
function getDiscount(customer) {
  if (customer.vipLevel == 1) {
    return 10;
  }
  return 0;
}

// customer.vipLevel이 '1' (문자열)이면?
// customer.vipLevel이 true이면?

// 문제 5: 다음 조건문의 문제점을 찾고 수정하세요
const products = [];

if (products) {
  console.log('상품이 있습니다');
  // 빈 배열인데도 실행됨!
}

// 문제 6: 실무 시나리오 - 안전한 합계 계산 함수 작성
// 다음 데이터의 총 금액을 계산하는 함수를 작성하세요
const orderItems = [
  { name: 'A', price: 100 },
  { name: 'B', price: '200' },  // 문자열!
  { name: 'C', price: null },   // null!
  { name: 'D', price: undefined }, // undefined!
  { name: 'E', price: 'invalid' }  // 잘못된 값!
];

function calculateSafeTotal(items) {
  // 여기에 안전한 계산 로직 작성
}
```




### 연산자와 조건문

javascript

```javascript
// 산술 연산자
let sum = 10 + 5;
let difference = 10 - 5;
let product = 10 * 5;
let quotient = 10 / 5;
let remainder = 10 % 3;
let power = 2 ** 3;  // 8

// 비교 연산자
console.log(5 == "5");   // true (타입 변환)
console.log(5 === "5");  // false (엄격한 비교, 권장)
console.log(5 != "5");   // false
console.log(5 !== "5");  // true

// 논리 연산자
const and = true && false;  // false
const or = true || false;   // true
const not = !true;          // false

// 조건문
const score = 85;

if (score >= 90) {
  console.log("A");
} else if (score >= 80) {
  console.log("B");
} else {
  console.log("C");
}

// 삼항 연산자
const result = score >= 80 ? "Pass" : "Fail";

// switch문
const day = "Monday";
switch (day) {
  case "Monday":
    console.log("Start of week");
    break;
  case "Friday":
    console.log("Almost weekend");
    break;
  default:
    console.log("Regular day");
}
```

### 반복문

javascript

```javascript
// for문
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// while문
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

// do-while문
let num = 0;
do {
  console.log(num);
  num++;
} while (num < 5);

// for...of (배열 순회)
const fruits = ["apple", "banana", "orange"];
for (const fruit of fruits) {
  console.log(fruit);
}

// for...in (객체 속성 순회)
const user = { name: "John", age: 30 };
for (const key in user) {
  console.log(key, user[key]);
}

// break와 continue
for (let i = 0; i < 10; i++) {
  if (i === 3) continue;  // 3 건너뛰기
  if (i === 7) break;     // 7에서 중단
  console.log(i);
}
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