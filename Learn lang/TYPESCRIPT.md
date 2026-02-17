# 타입 시스템의 기초
- 목적: JS의 불안정성을 TS가 어떻게 해결하는지 이해합니다. 
- 이론: 정적 타이핑의 원리, 기본 프리미티브 타입, 배열과 튜플. 
- 실습: 변수 및 함수에 타입 입히기.

**1. 왜 필요한가요?** 
브라우저는 타입스크립트를 읽지 못합니다. 
우리가 쓴 멋진 TS 코드를 JS로 번역(Compile)해주는 과정이 필수적입니다. 
이 과정에서 에러를 미리 잡아내는 것이 TS의 핵심 가치입니다.

**2. 어떻게 동작하나요?** 
`tsc`(TypeScript Compiler) 명령어가 우리의 코드를 검사합니다. 
문법이 틀리면 실행 파일조차 만들지 않죠. 
이것이 바로 "실행 전 에러 방지"의 원리입니다.

```
# 1. 타입스크립트 설치 (이미 하셨다면 패스)
npm install -g typescript

# 2. 설정 파일 생성 (이게 있어야 프로젝트의 규칙이 정해집니다)
tsc --init

# 3. index.ts 파일 생성 후 아래 코드 작성
const message: string = "Hello, Senior Engineer!";
console.log(message);

# 4. 컴파일 실행
tsc index.ts
```

위 과정을 실행했을 때, `index.js` 파일이 생성되었나요? 생성되었다면 그 파일 안의 코드는 어떻게 변해 있나요?

`string` 같은 타입 구문을 이해하지 못하기 때문에, **TS 컴파일러가 이를 제거하고 가장 안전한 형태인 자바스크립트(`var` 또는 설정에 따라 `let/const`)로 번역**해준 것입니다.

이것이 바로 **"타입 소거(Type Erasure)"** 라는 핵심 개념입니다. 
타입은 오직 개발 단계에서 우리를 돕고, 실제 실행될 때는 사라지는 '설계도'일 뿐이죠.

## 변수와 함수의 정적 타이핑
**1. 왜 필요한가요?** 
자바스크립트는 "유연함"이 장점이지만, 규모가 커지면 독이 됩니다. 숫자가 들어가야 할 곳에 문자열이 들어가서 계산이 꼬이는 경험, 해보셨죠? 
TS는 변수의 **'신분증'** 을 미리 검사해서 이런 사고를 원천 봉쇄합니다.

**2. 어떻게 동작하나요?** 
변수명 뒤에 `: type`을 붙입니다. 
함수에서는 **'입력값(Parameter)'**과 **'결과값(Return)'** 모두에 타입을 지정할 수 있습니다.


**3. 실습 (현업 스타일 예제)** 
간단한 장바구니 가격 계산 함수를 작성해 봅시다.

```
// 1. 기본 타입 지정
let itemName: string = "Apple";
let quantity: number = 5;
let isStocked: boolean = true;

// 2. 함수 타입 지정 (매개변수와 리턴값)
// 이 함수는 숫자를 받아서 숫자를 돌려줘야 합니다.
function calculateTotal(price: number, qty: number): number {
    return price * qty;
}

const total = calculateTotal(1500, quantity);
console.log(`${itemName}의 총 가격은 ${total}원입니다.`);

// [실습 미션] 아래 코드의 주석을 해제하고 에러가 나는지 확인해보세요.
// itemName = 100; 
// calculateTotal("1500", 5);
```

**4. 실무 예제: API 데이터 가공** 
나중에 Node.js 서버를 만들 때, 사용자 ID를 받아서 이름을 돌려주는 함수는 보통 이렇게 작성합니다.

TypeScript

```
function getUserGreeting(userId: number, userName: string): string {
    return `Welcome back, ${userName}! (ID: ${userId})`;
}
```
# 객체 지향과 데이터 구조화
- 목적: 복잡한 데이터를 체계적으로 관리하고 협업 효율을 높입니다. 
- 이론: Interface와 Type Alias의 차이 및 활용, Optional/Readonly 속성. 
- 실습: 사용자 프로필 및 API 요청 데이터 규격 정의.

## 인터페이스로 객체설계
**1. 왜 필요한가요?** 
현업에서 다루는 데이터는 단순히 숫자나 문자가 아닙니다. 
**사용자 정보, 상품 리스트, 게시글 데이터**처럼 복잡한 '객체(Object)' 형태죠. 
`Interface`는 이런 복잡한 데이터의 **'설계도'** 입니다. 
팀원들과 "우리 유저 데이터는 꼭 이 모양이어야 해!"라고 규약을 정하는 핵심 도구입니다.

**2. 어떻게 동작하나요?** 
`interface` 키워드를 사용해 객체가 가져야 할 속성과 그 속성의 타입을 미리 정의합니다.

**3. 실습 (현업 스타일: 유저 프로필 관리)**

TypeScript

```
// 1. 설계도(Interface) 만들기
interface User {
    id: number;
    userName: string;
    email: string;
    isActive: boolean;
    nickname?: string; // '?'는 선택 사항(Optional)이라는 뜻입니다! 있어도 되고 없어도 돼요.
}

// 2. 설계도대로 객체 생성하기
const newUser: User = {
    id: 1,
    userName: "Gemini",
    email: "gemini@google.com",
    isActive: true
    // nickname은 없어도 에러가 나지 않습니다.
};

// 3. 실무 예제: 함수에 인터페이스 적용
function printUserInfo(user: User): void {
    console.log(`User: ${user.userName} (${user.email})`);
    if (user.nickname) {
        console.log(`Nickname: ${user.nickname}`);
    }
}

printUserInfo(newUser);
```

**4. 배경 지식: Optional(`?`)과 Readonly**

- **Optional (`?`)**: 데이터가 항상 존재하지 않을 때 사용합니다. (예: 주소, 별명 등)
- **Readonly**: `readonly id: number;`처럼 선언하면, 한 번 정해진 값은 절대 수정할 수 없습니다. (ID 값 변경 방지)
	- 데이터의 무결성을 지키는 아주 중요한 습관이죠.

**5. 실전 예제 및 주관식 문제**

**상황:** 당신은 쇼핑몰의 상품 데이터를 관리하는 엔지니어입니다. 다음 조건에 맞는 `Product` 인터페이스를 작성하고, 객체를 하나 만들어보세요.

- `id`: 숫자 (수정 불가능해야 함)
- `name`: 문자열
- `price`: 숫자
- `description`: 문자열 (있을 수도 있고 없을 수도 있음)

## Type Alias와 Union/Literal

**1. 왜 필요한가요?** 현실의 데이터는 "이것 아니면 저것"인 경우가 많습니다.
- 결제 상태: '성공', '대기', '실패' 중 하나
- 유저 권한: '관리자', '일반유저' 중 하나 단순히 `string`이라고만 하면 아무 글자나 다 들어오겠죠? **Union(`|`)**과 **Literal**을 쓰면 값의 범위를 아주 좁고 안전하게 제한할 수 있습니다. 

**2. Type Alias vs Interface**

- `Interface`: 주로 객체의 구조(Shape)를 정의할 때 씁니다. (확장이 용이함)
    
- `Type Alias`: `type Name = string;` 처럼 타입에 별명을 붙이거나, Union 타입을 만들 때 주로 씁니다.
    

**3. 실습 (현업 스타일: 주문 상태 관리)**

TypeScript

```
// 1. Literal Type + Union (|)
type OrderStatus = "pending" | "shipped" | "delivered";

// 2. Type Alias로 복잡한 타입 정의
type UserID = string | number; // ID가 숫자일 수도, 문자열일 수도 있을 때

interface Order {
    orderId: UserID;
    status: OrderStatus;
}

// 실습: 올바른 사용
const myOrder: Order = {
    orderId: "ABC-123",
    status: "shipped" 
};

// [에러 발생 예시]
// myOrder.status = "cancelled"; // Error: OrderStatus에 "cancelled"는 없어요!
```

**4. 실무 예제: 응답 결과 처리** API 요청이 성공했는지 실패했는지에 따라 다른 처리를 할 때 유용합니다.

TypeScript

```
type Result = "success" | "error";

function handleResponse(result: Result) {
    if (result === "success") {
        console.log("데이터 로드 성공!");
    } else {
        console.log("문제가 발생했습니다.");
    }
}
```

**5. 핵심 이해 문제 (주관식)**

**상황:** 당신은 SNS 앱을 만들고 있습니다. 게시글의 '공개 범위'를 설정하는 타입을 만드려 합니다. 다음 조건에 맞게 코드를 작성해 보세요.

1. `Visibility`라는 이름의 **Type Alias**를 만드세요.
    
2. 값은 `"public"`, `"private"`, `"followers"` 세 가지만 허용합니다.
    
3. `Post` 인터페이스를 만들고, `title(string)`과 `visibility(Visibility)` 속성을 포함시키세요.

 현업에서는 단순히 `string`으로 타입을 정하지 않습니다. "이 변수에는 딱 이 3가지 단어만 들어올 수 있어!"라고 못박는 것이 버그를 줄이는 일류 엔지니어의 비결이죠.

# 유연하고 강력한 타입설계
- 목적: 재사용 가능하고 변화에 강한 코드를 작성합니다. 
- 이론: Union, Intersection, Literal Types, 그리고 핵심 중의 핵심인 Generic. 
- 실습: 공통 응답 처리(Response Wrapper) 및 유틸리티 함수 제작

## Generic
**1. 왜 필요한가요?** 우리는 재사용 가능한 코드를 사랑합니다. 그런데 타입이 고정되어 버리면 재사용이 어렵죠.
- 숫자를 담는 상자도 필요하고, 문자를 담는 상자도 필요하다면?
- 각각 따로 함수를 만들 건가요? 아니면 `any`를 써서 타입을 포기할 건가요? **Generic**은 타입을 **'매개변수'**처럼 넘겨받아, 코드를 작성할 때가 아니라 **사용할 때 타입을 결정**하게 해줍니다.

**2. 어떻게 동작하나요?** 관습적으로 `<T>` (Type의 약자)를 사용합니다. 꺽쇠 안에 타입을 담아서 전달하는 방식입니다.

**3. 실습 (현업 스타일: API 응답 래퍼)** 서버에서 데이터를 받아올 때, 데이터의 내용은 매번 다르지만 `데이터`, `상태코드`, `메시지`라는 틀은 같을 때 Generic이 빛을 발합니다.

```typescript
// 1. 제네릭 인터페이스 정의 (T는 나중에 결정됨)
interface ApiResponse<T> {
    data: T;
    status: number;
    message: string;
}

// 2. 사용할 때 구체적인 타입을 넣어줌
interface User {
    name: string;
    age: number;
}

// 유저 데이터를 담은 응답
const userResponse: ApiResponse<User> = {
    data: { name: "John", age: 30 },
    status: 200,
    message: "Success"
};

// 문자열 배열을 담은 응답
const listResponse: ApiResponse<string[]> = {
    data: ["Apple", "Banana"],
    status: 200,
    message: "List loaded"
};
```

**4. 실무 팁: `any`를 쓰지 마세요!** `any`를 쓰면 TS의 모든 장점이 사라집니다. 
Generic은 **"어떤 타입이 들어올지 모르지만, 들어온 그 타입을 끝까지 지키겠다"**는 약속입니다.


**5. 핵심 이해 문제 (주관식)**

**상황:** 당신은 배열의 첫 번째 요소를 반환하는 함수 `getFirstElement`를 만드려 합니다. 이 함수는 숫자 배열을 넣으면 숫자를 반환하고, 문자 배열을 넣으면 문자를 반환해야 합니다.

**문제:** 아래의 빈칸을 Generic을 사용하여 완성해 보세요.

```
function getFirstElement<T>(arr: T[]): T {
    return arr[0];
}

const num = getFirstElement([1, 2, 3]); // num의 타입은 number가 되어야 함
const str = getFirstElement(["A", "B", "C"]); // str의 타입은 string이 되어야 함
```


6. generic 추가 학습 Case별 Generic 사용법 (집중 공략)

Case 1: 함수에서 사용하기 (입력과 출력의 연결)
`any`를 쓰면 입력이 무엇이든 상관없지만, **출력이 무엇인지 잊어버립니다.** 
하지만 제네릭은 입력된 타입을 **기억**했다가 그대로 돌려줍니다.

```
// <T>는 "타입을 변수처럼 받겠다"는 선언입니다.

// (items : T[]) - parameter는 그 타입의 배열이여야한다. 
// 만약 앞에서 `T`가 `number`로 결정되었다면, 이 자리는 자동으로 `number[]`가 됩니다.

//`: T[]` : "나가는 결과물도 똑같은 타입의 '배열'이야" return도 T타입의 배열

function reverseArray<T>(items: T[]): T[] {
    return items.reverse();
}

const numArr = reverseArray([1, 2, 3]); // T는 number가 됨. 결과도 number[]
const strArr = reverseArray(["A", "B"]); // T는 string이 됨. 결과도 string[]

// numArr[0].toFixed(); // 가능 (숫자인 걸 아니까!)
```

 Case 2: 인터페이스/타입에서 사용하기 (데이터 규격화)

API 응답처럼 **겉모양은 같은데 알맹이(data)만 바뀔 때** 사용합니다. 
```
interface Box<T> {
    content: T;
}

const stringBox: Box<string> = { content: "Hello" };
const numberBox: Box<number> = { content: 123 };
```


 Case 3: 제약 조건 걸기 (`extends`)

"아무 타입이나 다 되는데, 최소한 `length` 속성은 있어야 해!"라고 제한할 때 씁니다.

```
function logLength<T extends { length: number }>(item: T): void {
    console.log(item.length);
}

logLength("abc"); // 가능 (문자열은 length가 있음)
logLength([1, 2]); // 가능 (배열은 length가 있음)
// logLength(123); // 에러! (숫자는 length가 없음)
```

# 실전 프레임워크 준비
- 목적: React/Node.js 라이브러리 연동 시의 타입 처리를 익힙니다. 
- 이론: 외부 라이브러리 타입 정의(@types), Type Assertion vs Guard. 
- 실습: Todo 앱 로직을 TS로 완벽하게 구현하기.


최종 실무 코드 작성 및 테스트
**1. 시나리오** 
당신은 **게시판 관리 시스템**을 만들고 있습니다. 
이 시스템은 서버에서 데이터를 받아오는데, 데이터의 종류가 `Post(게시글)`일 수도 있고 `Comment(댓글)`일 수도 있습니다. 
어떤 데이터가 오든 공통된 형식(상태 코드 등)에 담아 처리하는 유연한 코드를 작성해야 합니다.

```
// 1. 데이터 타입 정의
interface Post {
    id: number;
    title: string;
}

interface Comment {
    id: number;
    text: string;
}

// 2. Generic을 사용한 공통 응답 구조 (Phase 5 복습)
interface ApiResponse<T> {
    data: T;
    success: boolean;
}

// 3. 실제 사용 예시
const postResponse: ApiResponse<Post> = {
    data: { id: 1, title: "TS 배우기 꿀팁" },
    success: true
};
```


# 🎯 최종 졸업 문제 (주관식 코드 작성)

아래의 요구사항을 읽고, 하나의 코드 블록에 완성된 코드를 작성해 주세요. 이 문제는 지금까지 배운 모든 핵심을 관통합니다.

**[요구사항]**

1. **Type Alias 정의**: 유저의 권한을 나타내는 `Role` 타입을 만드세요. 값은 `"admin"` 또는 `"user"`만 가능합니다.
2. **Interface 정의**: `UserProfile` 인터페이스를 만드세요.
    
    - `id`: 숫자 (읽기 전용)
        
    - `name`: 문자열
        
    - `role`: 위에서 만든 `Role` 타입
        
    - `email`: 문자열 (선택 사항 - `?`)
3. **Generic 함수 정의**: `wrapInData`라는 함수를 만드세요.
    
    - 이 함수는 제네릭 `<T>`를 사용합니다.
        
    - 매개변수 하나(`content: T`)를 받습니다.
        
    - 결과값으로 `{ content: T, timestamp: number }` 형태의 객체를 반환해야 합니다.

```
type Role = "admin" | "user";

interface UserProfile {
	readonly id: number;
	name: string;
	role: Role;
	email?: string;
}

function wrapInData<T>(content: T){
	return {
		content: content,
		timestamp: Date.now()
	}
}

// --- 확인 작업 --- 
const myProfile: UserProfile = { id: 1, name: "코딩초보", role: "admin" }; 

const result = wrapInData(myProfile); 
// result의 content 안에는 UserProfile 타입이 쏙 들어갑니다!
```


# 타입스크립트 학습

## 기본타입
```typescript
// 원시 타입
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;

// 배열
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["John", "Jane"];

// 튜플
let person: [string, number] = ["John", 30];

// enum
enum Role {
  Admin = "ADMIN",
  User = "USER",
  Guest = "GUEST"
}
let userRole: Role = Role.Admin;

// any (가능한 피하기)
let anything: any = "hello";
anything = 123;

// unknown (any보다 안전)
let uncertain: unknown = "hello";
if (typeof uncertain === "string") {
  console.log(uncertain.toUpperCase());
}

// void (반환값 없음)
function log(message: string): void {
  console.log(message);
}

// never (절대 반환하지 않음)
function throwError(message: string): never {
  throw new Error(message);
}
```

## 함수타입


```typescript
// 기본 함수
function add(a: number, b: number): number {
  return a + b;
}

// 화살표 함수
const multiply = (a: number, b: number): number => a * b;

// 선택적 매개변수
function greet(name: string, greeting?: string): string {
  return `${greeting || "Hello"}, ${name}!`;
}

// 기본값
function createUser(name: string, age: number = 18) {
  return { name, age };
}

// Rest 매개변수
function sum(...numbers: number[]): number {
  return numbers.reduce((acc, n) => acc + n, 0);
}

// 함수 타입 정의
type MathOperation = (a: number, b: number) => number;
const divide: MathOperation = (a, b) => a / b;
```

### 인터페이스와 타입

typescript

```typescript
// 인터페이스
interface User {
  id: string;
  name: string;
  email: string;
  age?: number; // 선택적 프로퍼티
  readonly createdAt: Date; // 읽기 전용
}

// 인터페이스 확장
interface Admin extends User {
  role: "admin";
  permissions: string[];
}

// 타입 별칭
type Product = {
  id: string;
  name: string;
  price: number;
};

// 유니온 타입
type Status = "pending" | "approved" | "rejected";
type ID = string | number;

// 인터섹션 타입
type Employee = User & {
  department: string;
  salary: number;
};

// 타입 vs 인터페이스 차이
// 타입: 유니온, 인터섹션, 원시값 등 더 유연
type StringOrNumber = string | number;

// 인터페이스: 선언 병합 가능, 클래스 구현에 적합
interface Window {
  customProperty: string;
}
```

### 제네릭

typescript

```typescript
// 기본 제네릭
function identity<T>(value: T): T {
  return value;
}

const num = identity<number>(42);
const str = identity("hello");

// 제네릭 인터페이스
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

const userResponse: ApiResponse<User> = {
  data: { id: "1", name: "John", email: "john@email.com", createdAt: new Date() },
  status: 200,
  message: "Success"
};

// 제네릭 클래스
class Container<T> {
  private value: T;
  
  constructor(value: T) {
    this.value = value;
  }
  
  getValue(): T {
    return this.value;
  }
  
  setValue(value: T): void {
    this.value = value;
  }
}

// 제네릭 제약
interface HasLength {
  length: number;
}

function logLength<T extends HasLength>(item: T): void {
  console.log(item.length);
}

logLength("hello"); // OK
logLength([1, 2, 3]); // OK
// logLength(123); // Error

// 여러 제네릭
function merge<T, U>(obj1: T, obj2: U): T & U {
  return { ...obj1, ...obj2 };
}

const merged = merge({ name: "John" }, { age: 30 });
```

### 유틸리티 타입

typescript

```typescript
interface User {
  id: string;
  name: string;
  email: string;
  age: number;
}

// Partial - 모든 속성을 선택적으로
type PartialUser = Partial<User>;
const updateUser: PartialUser = { name: "Jane" };

// Required - 모든 속성을 필수로
type RequiredUser = Required<User>;

// Readonly - 모든 속성을 읽기 전용으로
type ReadonlyUser = Readonly<User>;

// Pick - 특정 속성만 선택
type UserPreview = Pick<User, "id" | "name">;

// Omit - 특정 속성 제외
type UserWithoutEmail = Omit<User, "email">;

// Record - 키-값 타입 정의
type UserRoles = Record<string, string[]>;
const roles: UserRoles = {
  admin: ["read", "write", "delete"],
  user: ["read"]
};

// ReturnType - 함수 반환 타입 추출
function getUser() {
  return { id: "1", name: "John" };
}
type UserReturnType = ReturnType<typeof getUser>;

// Parameters - 함수 매개변수 타입 추출
type GetUserParams = Parameters<typeof getUser>;
```


### 고급 타입

typescript

```typescript
// 조건부 타입
type IsString<T> = T extends string ? true : false;
type A = IsString<string>; // true
type B = IsString<number>; // false

// Mapped Types
type Nullable<T> = {
  [P in keyof T]: T[P] | null;
};

type NullableUser = Nullable<User>;

// Template Literal Types
type EventName = "click" | "focus" | "blur";
type EventHandler = `on${Capitalize<EventName>}`;
// "onClick" | "onFocus" | "onBlur"

// infer 키워드
type UnpackArray<T> = T extends (infer U)[] ? U : T;
type StringArray = UnpackArray<string[]>; // string
type JustNumber = UnpackArray<number>; // number

// 타입 가드
function isString(value: unknown): value is string {
  return typeof value === "string";
}

function processValue(value: string | number) {
  if (isString(value)) {
    console.log(value.toUpperCase()); // string으로 타입 좁혀짐
  } else {
    console.log(value.toFixed(2)); // number로 타입 좁혀짐
  }
}

// Discriminated Unions
type SuccessResponse = {
  status: "success";
  data: User;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

type ApiResponse = SuccessResponse | ErrorResponse;

function handleResponse(response: ApiResponse) {
  if (response.status === "success") {
    console.log(response.data); // User 타입
  } else {
    console.log(response.message); // string 타입
  }
}
```

### 데코레이터

typescript

```typescript
// tsconfig.json에서 "experimentalDecorators": true 설정 필요

// 클래스 데코레이터
function sealed(constructor: Function) {
  Object.seal(constructor);
  Object.seal(constructor.prototype);
}

@sealed
class BugReport {
  type = "report";
  title: string;
  
  constructor(title: string) {
    this.title = title;
  }
}

// 메서드 데코레이터
function log(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;
  
  descriptor.value = function (...args: any[]) {
    console.log(`Calling ${propertyKey} with:`, args);
    const result = originalMethod.apply(this, args);
    console.log(`Result:`, result);
    return result;
  };
  
  return descriptor;
}

class Calculator {
  @log
  add(a: number, b: number) {
    return a + b;
  }
}

// 프로퍼티 데코레이터
function validate(min: number, max: number) {
  return function (target: any, propertyKey: string) {
    let value: number;
    
    const getter = () => value;
    const setter = (newVal: number) => {
      if (newVal < min || newVal > max) {
        throw new Error(`Value must be between ${min} and ${max}`);
      }
      value = newVal;
    };
    
    Object.defineProperty(target, propertyKey, {
      get: getter,
      set: setter
    });
  };
}

class Person {
  @validate(0, 120)
  age: number;
}
```

### 네임스페이스와 모듈

typescript

```typescript
// 네임스페이스 (레거시, 권장하지 않음)
namespace Validation {
  export interface StringValidator {
    isValid(s: string): boolean;
  }
  
  export class EmailValidator implements StringValidator {
    isValid(s: string) {
      return /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/.test(s);
    }
  }
}

// 모듈 (권장)
// user.ts
export interface User {
  id: string;
  name: string;
}

export function createUser(name: string): User {
  return { id: Math.random().toString(), name };
}

export default class UserService {
  // ...
}

// main.ts
import UserService, { User, createUser } from './user';
import * as UserModule from './user';

```


### 타입 안전한 API 클라이언트

typescript

```typescript
interface ApiClient {
  get<T>(url: string): Promise<T>;
  post<T, D>(url: string, data: D): Promise<T>;
}

class HttpClient implements ApiClient {
  constructor(private baseURL: string) {}
  
  async get<T>(url: string): Promise<T> {
    const response = await fetch(`${this.baseURL}${url}`);
    return response.json();
  }
  
  async post<T, D>(url: string, data: D): Promise<T> {
    const response = await fetch(`${this.baseURL}${url}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data)
    });
    return response.json();
  }
}

// 사용
const api = new HttpClient('https://api.example.com');

interface Post {
  id: number;
  title: string;
  content: string;
}

const posts = await api.get<Post[]>('/posts');
const newPost = await api.post<Post, Omit<Post, 'id'>>('/posts', {
  title: 'New Post',
  content: 'Content here'
});
```


## 추천 학습 자료

1. **공식 문서**: [https://www.typescriptlang.org/docs/](https://www.typescriptlang.org/docs/)
2. **TypeScript Playground**: 실시간 테스트
3. **실전 프로젝트**: Express + TypeScript, React + TypeScript
4. **오픈소스 코드 읽기**: 유명 라이브러리의 타입 정의 파일(.d.ts) 분석

## 학습 팁

typescript

```typescript
// 1. strict 모드 사용 (tsconfig.json)
{
  "compilerOptions": {
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  }
}

// 2. ESLint + TypeScript 설정
// 3. 실제 프로젝트에 적용하며 학습
// 4. 타입 에러를 두려워하지 말고 이해하기
```

어떤 부분부터 더 자세히 알고 싶으신가요?
# 디자인 패턴
## 1. Repository Pattern (가장 많이 사용)

데이터 접근 로직을 추상화하여 비즈니스 로직과 분리합니다.


```typescript
interface IUserRepository {
  findById(id: string): Promise<User | null>;
  findAll(): Promise<User[]>;
  create(user: User): Promise<User>;
  update(id: string, user: Partial<User>): Promise<User>;
  delete(id: string): Promise<void>;
}

class UserRepository implements IUserRepository {
  constructor(private db: Database) {}
  
  async findById(id: string): Promise<User | null> {
    return this.db.users.findOne({ id });
  }
  
  async findAll(): Promise<User[]> {
    return this.db.users.find();
  }
  
  async create(user: User): Promise<User> {
    return this.db.users.insert(user);
  }
  
  async update(id: string, user: Partial<User>): Promise<User> {
    return this.db.users.updateOne({ id }, user);
  }
  
  async delete(id: string): Promise<void> {
    await this.db.users.deleteOne({ id });
  }
}

// 사용
class UserService {
  constructor(private userRepo: IUserRepository) {}
  
  async getUser(id: string) {
    return this.userRepo.findById(id);
  }
}
```

## 2. Dependency Injection (DI)

의존성을 외부에서 주입하여 테스트와 유지보수를 쉽게 만듭니다.

typescript

```typescript
// 컨테이너 없이 간단한 DI
class EmailService {
  send(to: string, message: string) {
    console.log(`Sending email to ${to}: ${message}`);
  }
}

class UserService {
  constructor(
    private userRepo: IUserRepository,
    private emailService: EmailService
  ) {}
  
  async registerUser(userData: CreateUserDto) {
    const user = await this.userRepo.create(userData);
    this.emailService.send(user.email, 'Welcome!');
    return user;
  }
}

// 사용
const emailService = new EmailService();
const userRepo = new UserRepository(database);
const userService = new UserService(userRepo, emailService);
```

## 3. Factory Pattern (실무 버전)

복잡한 객체 생성 로직을 캡슐화합니다.

typescript

```typescript
// API Response Factory
type ApiResponse<T> = {
  success: boolean;
  data?: T;
  error?: string;
  timestamp: Date;
};

class ResponseFactory {
  static success<T>(data: T): ApiResponse<T> {
    return {
      success: true,
      data,
      timestamp: new Date()
    };
  }
  
  static error(message: string): ApiResponse<never> {
    return {
      success: false,
      error: message,
      timestamp: new Date()
    };
  }
}

// 사용
app.get('/users/:id', async (req, res) => {
  try {
    const user = await userService.getUser(req.params.id);
    res.json(ResponseFactory.success(user));
  } catch (error) {
    res.json(ResponseFactory.error(error.message));
  }
});
```

## 4. Builder Pattern (DTO/Query 생성)

복잡한 쿼리나 객체를 단계적으로 생성합니다.

typescript

```typescript
class QueryBuilder {
  private query: any = {};
  private sortOptions: any = {};
  private limitValue?: number;
  
  where(field: string, value: any): this {
    this.query[field] = value;
    return this;
  }
  
  sort(field: string, order: 'asc' | 'desc'): this {
    this.sortOptions[field] = order === 'asc' ? 1 : -1;
    return this;
  }
  
  limit(value: number): this {
    this.limitValue = value;
    return this;
  }
  
  build() {
    return {
      query: this.query,
      sort: this.sortOptions,
      limit: this.limitValue
    };
  }
}

// 사용
const query = new QueryBuilder()
  .where('status', 'active')
  .where('age', { $gte: 18 })
  .sort('createdAt', 'desc')
  .limit(10)
  .build();
```

## 5. Strategy Pattern (결제/인증)

알고리즘을 런타임에 선택할 수 있게 합니다.

typescript

```typescript
interface PaymentStrategy {
  processPayment(amount: number): Promise<PaymentResult>;
}

class StripePayment implements PaymentStrategy {
  async processPayment(amount: number): Promise<PaymentResult> {
    // Stripe API 호출
    return { success: true, transactionId: 'stripe_123' };
  }
}

class TossPayment implements PaymentStrategy {
  async processPayment(amount: number): Promise<PaymentResult> {
    // Toss API 호출
    return { success: true, transactionId: 'toss_456' };
  }
}

class PaymentService {
  private strategies: Map<string, PaymentStrategy> = new Map();
  
  constructor() {
    this.strategies.set('stripe', new StripePayment());
    this.strategies.set('toss', new TossPayment());
  }
  
  async pay(method: string, amount: number) {
    const strategy = this.strategies.get(method);
    if (!strategy) throw new Error('Invalid payment method');
    return strategy.processPayment(amount);
  }
}
```

## 6. Middleware Pattern (Express/NestJS)

요청 처리 파이프라인을 구성합니다.

typescript

```typescript
type Middleware = (
  req: Request,
  res: Response,
  next: NextFunction
) => void | Promise<void>;

class AuthMiddleware {
  static authenticate: Middleware = async (req, res, next) => {
    const token = req.headers.authorization;
    
    if (!token) {
      return res.status(401).json({ error: 'No token provided' });
    }
    
    try {
      const user = await verifyToken(token);
      req.user = user;
      next();
    } catch (error) {
      res.status(401).json({ error: 'Invalid token' });
    }
  };
  
  static authorize(roles: string[]): Middleware {
    return (req, res, next) => {
      if (!roles.includes(req.user.role)) {
        return res.status(403).json({ error: 'Forbidden' });
      }
      next();
    };
  }
}

// 사용
app.get('/admin', 
  AuthMiddleware.authenticate,
  AuthMiddleware.authorize(['admin']),
  adminController.dashboard
);
```

## 7. Decorator Pattern (로깅/캐싱)

기존 기능에 새로운 기능을 추가합니다.

typescript

```typescript
// 메서드 데코레이터
function Log() {
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    const originalMethod = descriptor.value;
    
    descriptor.value = async function (...args: any[]) {
      console.log(`[${propertyKey}] called with:`, args);
      const result = await originalMethod.apply(this, args);
      console.log(`[${propertyKey}] returned:`, result);
      return result;
    };
    
    return descriptor;
  };
}

function Cache(ttl: number) {
  const cache = new Map();
  
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    const originalMethod = descriptor.value;
    
    descriptor.value = async function (...args: any[]) {
      const key = JSON.stringify(args);
      
      if (cache.has(key)) {
        const cached = cache.get(key);
        if (Date.now() - cached.timestamp < ttl) {
          return cached.value;
        }
      }
      
      const result = await originalMethod.apply(this, args);
      cache.set(key, { value: result, timestamp: Date.now() });
      return result;
    };
  };
}

// 사용
class UserService {
  @Log()
  @Cache(60000) // 1분 캐싱
  async getUser(id: string) {
    return this.userRepo.findById(id);
  }
}
```

## 8. Observer Pattern (이벤트 시스템)

느슨한 결합으로 이벤트를 처리합니다.

typescript

```typescript
type EventHandler<T = any> = (data: T) => void | Promise<void>;

class EventEmitter {
  private events: Map<string, EventHandler[]> = new Map();
  
  on(event: string, handler: EventHandler) {
    if (!this.events.has(event)) {
      this.events.set(event, []);
    }
    this.events.get(event)!.push(handler);
  }
  
  async emit(event: string, data?: any) {
    const handlers = this.events.get(event) || [];
    await Promise.all(handlers.map(handler => handler(data)));
  }
}

// 사용
const eventBus = new EventEmitter();

eventBus.on('user.created', async (user) => {
  await emailService.sendWelcomeEmail(user);
});

eventBus.on('user.created', async (user) => {
  await analyticsService.trackSignup(user);
});

// 서비스에서
class UserService {
  async createUser(data: CreateUserDto) {
    const user = await this.userRepo.create(data);
    await eventBus.emit('user.created', user);
    return user;
  }
}
```

## 9. DTO (Data Transfer Object) Pattern

데이터 전송과 유효성 검사를 담당합니다.

typescript

```typescript
import { IsEmail, IsString, MinLength } from 'class-validator';

class CreateUserDto {
  @IsString()
  @MinLength(2)
  name: string;
  
  @IsEmail()
  email: string;
  
  @IsString()
  @MinLength(8)
  password: string;
}

class UpdateUserDto {
  @IsString()
  @MinLength(2)
  name?: string;
  
  @IsEmail()
  email?: string;
}

// 사용
async function createUser(dto: CreateUserDto) {
  // dto는 이미 검증됨
  return userService.create(dto);
}
```

## 10. Result Pattern (에러 핸들링)

예외 대신 Result 객체로 성공/실패를 처리합니다.

typescript

```typescript
class Result<T> {
  private constructor(
    public readonly isSuccess: boolean,
    public readonly value?: T,
    public readonly error?: string
  ) {}
  
  static ok<T>(value: T): Result<T> {
    return new Result(true, value);
  }
  
  static fail<T>(error: string): Result<T> {
    return new Result(false, undefined, error);
  }
}

// 사용
class UserService {
  async createUser(data: CreateUserDto): Promise<Result<User>> {
    const existing = await this.userRepo.findByEmail(data.email);
    
    if (existing) {
      return Result.fail('Email already exists');
    }
    
    try {
      const user = await this.userRepo.create(data);
      return Result.ok(user);
    } catch (error) {
      return Result.fail(error.message);
    }
  }
}

// Controller
const result = await userService.createUser(dto);

if (result.isSuccess) {
  res.json({ user: result.value });
} else {
  res.status(400).json({ error: result.error });
}
```