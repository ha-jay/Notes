#  identifier - 이름 명명 규칙
## 1. 변수 선언 원칙

- **Hoisting 방지:** 모든 변수는 해당 스코프(범위)의 **최상단**에서 선언합니다.
- **Strict Mode:** 파일 상단에 `'use strict';`를 선언하여 잠재적인 에러를 방지합니다.
- **할당 기준:** * **Primitive type**(문자, 숫자 등): `let` 사용.
- **Collective type**(배열, 객체) & **Function**: `const` 사용.


## 네이밍 컨벤션

### 변수 - 명사로 표시
- 대문자 표기 - 절대 변하지않는 값 상수 - `const PI = Math.PI;`
- carmelCase - 변하는 값 - primitive는 `let` , 함수나 container 타입은 `const` 에 담는다. 
변수는 변수의 값을 잘 표현할 수 있도록 세분화해서 명사로 작성한다. 
- `let firstName = "ha";`
- `let isSold = false;` - 불리언 타입을 담는 값은 be 동사를 붙여서 사용한다. 
- `const userNames = []` - collective type은 복수의 값을 포함함을 알리기 위해 s를 붙인다. 


### 함수 - 동사로표시
함수는 동작을 수행하는 동사와 + 명사로 함수이름을 짓는다. 
- 불리언을 리턴하는 체크함수들은 be동사나 has, have 등을 사용 
	- `function isChecked (){}` `function hasEncription(){}`
- 일반적으로 자주쓰는 동사들은 get, make, apply, find, create, set 이다. 같은 의미의 다른 동사를 남발하지말고 통일하라. 
	- `getQuestions, retrieveComment` 같은의미의 다른동사를 쓰고있다. 
	- `getComments` - get으로 통일하라. 
- 명사는 상세히 표현하라. 
	- getData 데이터라는것은 너무 장황하다 -> `getUserPosts`
- 추가정보를 제공하는게 유용할때는 좀더 정확히 표현위해 부사구를 사용하라. 
	- findUser -> `findUserByName` `setUserLoggedInTrue`
- 같은 의미를 전달하는 경우에는 짧을 수록 좋다. 
	- getUserFriendFromDB -> `getUserFriend`


### 내부에서 사용되는 Private함수
다른곳에 참조되면 안됨을 알리기 위해서 앞에 `_`를 붙인다. 
```js
'use strict';

class CoffeeMachine {
  constructor() {
    // _waterTemp: 내부에서만 조절하는 물 온도 (관례상 Private)
    this._waterTemp = 90; 
  }

  // [Public] 사용자가 누르는 버튼
  makeCoffee() {
    this._boilWater(); // 내부 기능을 호출
    console.log("☕ 커피가 완성되었습니다!");
  }

  // [Private 관례] 외부에서 호출하면 안 되는 내부 동작
  _boilWater() {
    this._waterTemp = 100;
    console.log(`물을 ${this._waterTemp}도까지 끓이는 중...`);
  }
}

const myMachine = new CoffeeMachine();

// ✅ 올바른 사용법
myMachine.makeCoffee(); 

// ❌ 나쁜 사용법 (개발자들 사이의 약속 위반)
// myMachine._boilWater(); // "기계 뚜껑 열고 내부 부품 직접 건드리는 격"
// myMachine._waterTemp = 0; // "찬물로 커피 타라고 강제로 고쳐버리는 격"
```


```js
'use strict';

class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    // 클래스 내부 메서드를 호출할 때는 반드시 'this'를 붙여야 합니다.
    this.name = this._getName(firstName, lastName);
  }

  // 관례적으로 외부에서 호출하지 말라는 의미의 '_' 접두어
  _getName(firstName, lastName) {
    return `${firstName} ${lastName}`;
  }
}

const user1 = new Person("Jay", "Ha");

console.log(user1.name); // Output: Jay Ha (Good)
// console.log(user1._getName(...)); // (Bad: 내부 로직이므로 외부 호출 지양)
```

### 최신자바스크립트 표준에서는 `#필드` 로 private 변수와 함수 표현
ESSCRIPT 2022에서는 `_` 대신 `#` 을 사용하여 언어차원에서 외부접근을 강제 차단할 수 있습니다. 
이 방식을 더 권장합니다. 
```JS
class Person {
  #firstName; // Private 필드 선언
  #lastName;

  constructor(firstName, lastName) {
    this.#firstName = firstName;
    this.#lastName = lastName;
    this.name = this.#combineName(); // 내부에서만 사용
  }

  // 외부에서 절대 호출할 수 없는 진짜 Private 메서드
  #combineName() {
    return `${this.#firstName} ${this.#lastName}`;
  }
}

const user2 = new Person("Gildong", "Hong");
console.log(user2.name); // 접근 가능
// console.log(user2.#combineName()); // 에러 발생: SyntaxError (보안 및 캡슐화 강화)
```

