## 변수 및 객체 관리의 엄격화
가장 흔한 실수를 방지하기 위해 변수 선언과 삭제에 제약을 둡니다.

- **선언 없는 변수 사용 금지:** `let`, `const`, `var` 없이 변수를 사용할 수 없습니다.
- **변수/함수 삭제 금지:** `delete` 연산자로 변수나 함수를 지울 수 없습니다.
- **읽기 전용 속성 보호:** `writable: false`나 `getter` 전용 속성에 값을 쓰려고 하면 에러가 발생합니다.

```js
x = 3.14;                // ❌ Error: 선언되지 않은 변수
let y = 3.14;
delete y;                // ❌ Error: 변수 삭제 불가

//Deleting an undeletable property is not allowed: 삭제할 수 없는 속성은 삭제할 수 없습니다.
delete Object.prototype; // This will cause an error

//Writing to a read-only property is not allowed:  읽기 전용 속성에 쓰는 것은 허용되지 않습니다.
const obj = {};
Object.defineProperty(obj, "x", {value:0, writable:false});
obj.x = 3.14;            // This will cause an error

//Writing to a get-only property is not allowed:  get-only 속성에 대한 쓰기는 허용되지 않습니다.
const obj = {get x() {return 0} };
obj.x = 3.14;            // This will cause an error


```

## 함수 및 매개변수 관련 규칙

함수 호출과 정의 단계에서의 모호함을 제거합니다.

- **중복 매개변수 금지:** 한 함수 내에서 같은 이름의 파라미터를 사용할 수 없습니다.
- **`this`의 변화:** 일반 모드에서 단독 호출 시 `window`를 가리키던 `this`가 엄격 모드에서는 **`undefined`**가 됩니다. (전역 오염 방지)
	- this키워드는 함수를 호출 한 객체를 참조합니다.
	- 객체가 지정되지 않은 경우, 엄격 모드에서 함수는 undefined를 반환하고, 일반 모드의 함수는 전역 객체(window)를 반환합니다.
```js
function sum(a, a, b) { ... } // ❌ Error: 중복된 매개변수 이름

function showThis() {
  console.log(this); 
}
showThis();                   // ❌ 결과: undefined (일반 모드는 window)
```

## 문법 및 보안 제한

보안에 취약하거나 엔진 최적화를 방해하는 문법을 차단합니다.

- **`with` 문 사용 금지:** 코드의 모호성을 높이는 `with`를 사용할 수 없습니다.
- **`eval()`의 독립 스코프:** `eval()` 내부에서 생성한 변수는 외부 스코프에 영향을 주지 않습니다.
- **8진수 리터럴 금지:** 숫자 앞에 `0`을 붙여 8진수를 표현하는 방식은 혼란을 줄 수 있어 금지됩니다.
```js
with (Math) { x = cos(2) };   // ❌ Error: 사용 불가

eval("let temp = 5;");
console.log(temp);            // ❌ Error: eval 내부 변수는 밖에서 참조 불가
//For security reasons, eval() is not allowed to create variables in the scope from which it was called:
// 보안상의 이유로 eval()은(는) 호출된 범위에서 변수를 생성할 수 없습니다.
eval ("let x = 2");
alert (x);             // This will cause an error


//Octal numeric literals are not allowed:  8진수 숫자 리터럴은 허용되지 않습니다.
let x = 010;             // This will cause an error
//Octal escape characters are not allowed: 8진 이스케이프 문자는 허용되지 않습니다.
let x = "\010";            // This will cause an error



```

## 예약어 및 키워드 제한

향후 자바스크립트 버전 업데이트에서 사용될 가능성이 있는 키워드를 변수명으로 사용하지 못하게 차단합니다.

|**분류**|**금지 키워드**|
|---|---|
|**현재 제한**|`eval`, `arguments`|
|**미래 예약어**|`implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`|
```js
//The word eval cannot be used as a variable:  단어 eval는 변수로 사용할 수 없습니다.
let eval = 3.14;         // This will cause an error


//The word arguments cannot be used as a variable: 단어 arguments는 변수로 사용할 수 없습니다.
let arguments = 3.14;    // This will cause an error
```