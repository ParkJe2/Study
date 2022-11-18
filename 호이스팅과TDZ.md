# JavaScript 과제

## ★ 호이스팅과 TDZ는 무엇일까?

### 스코프, 호이스팅, TDZ

▷ 스코프<br>
변수, 함수, 클래스가 접근할 수 있는 유효 범위

* 선언된 위치에 따라 유효 범위가 달라진다.<br>
전역에 선언된 변수는 전역 스코프를, 지역에 선언된 변수는 지역 스코프를 갖는다.
  

▷ 호이스팅<br>
인터프리터가 변수와 함수의 메모리 공간을 선언 전에 미리 할당하는 것을 의미

* 호이스팅의 대상

자바스크립트는 ES6에서 도입된 let, const를 포함하여 모든 선언(var, let, const, function, function*, class)을 호이스팅합니다.<br> 
호이스팅(Hoisting)이란, var 선언문이나 function 선언문 등을 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성을 말합니다.<br>
let/const 변수 선언과 함수 표현식 에서는 호이스팅이 발생하지 않습니다.

```js
foo();
  foo2();

  function foo() { // 함수선언문
          console.log("hello");
  }
  var foo2 = function() { // 함수표현식
          console.log("hello2");
  }
```

▷ TDZ(Temporal Dead Zone)<br>
스코프의 시작 지점부터 초기화 시작 지점까지의 구간

```js
let age = 30;
function showAge() {
    console.log(age) //여기서 위에 age에 잘 접근할수 있습니다.
}
showAge();
```

위 코드에서 let age = 20; 추가

```js
let age = 30;
function showAge() {
    console.log(age) //2) TDZ 영역이 되어서 ReferenceError 에러를 볼수 있습니다.
    let age = 20; //1) 이것을 추가하면 
}
showAge();
```

* TDZ의 영향을 받는 구문

<const 변수><br>
const 변수는 선언 및 초기화 전 줄까지 TDZ에 있습니다.

```js
// Dose not work!
pi; // throws `ReferenceError`
const pi = 3.14;
```

const 변수는 선언 한 후에 사용해야 합니다.

```js
const pi = 3.14;

// works!
pi; // => 3.14
```

<let 변수><br>
let도 선언 전 줄 까지 TDZ의 영향을 받습니다.

```js
// Does net work!
const; // throws `ReferenceError`
let count;

count = 10;
```

<class 구문><br>
선언 전에는 class를 사용할 수 없습니다.

```js
// Does net work!
const myNissan = new Car('red'); // throws `ReferenceError`

class Car {
  constructor(color) {
     this.color = color;
   }
}
```

위의 예제가 동작하려면,<br>
클래스클 선언한 후에 사용하도록 수정해야 합니다.

```js
class Car {
  constructor(color) {
    this.color = color;
  }
}

// Works!
const myNissan = new Car('red');
myNissan.color; // => 'red'
```

### 함수 선언문과 함수 표현식에서 호이스팅 방식의 차이

▷ 함수 선언문 (function declartion)<br>
함수 선언문은 코드를 구현한 위치와 관계없이 자바스크립트의 특징인 호이스팅에 따라 브라우저가 자바스크립트를 해석 할 때 맨위로 끌어 올려집니다.

```js
/* 정상 출력 */
function printName(firstname) { // 함수선언문 
    var result = inner(); // "선언 및 할당"
    console.log(typeof inner); // > "function"
    console.log("name is " + result); // > "name is inner value"

    function inner() { // 함수선언문 
        return "inner value";
    }
}

printName(); // 함수 호출 
```

▷ 함수 표현문 (function Expression)<br>
정의한 function을 별도의 변수에 할당하는 것

함수 표현식은 함수 선언문과 달리 선언과 호출 순서에 따라서 정상적으로 함수가 실행되지 않을 수 있습니다.<br>
함수 표현식에서는 선언과 할당의 분리가 생깁니다.

```js
/* 오류 */
 function printName(firstname) { // 함수선언문
     console.log(inner); // > "undefined": 선언은 되어 있지만 값이 할당되어있지 않은 경우
     var result = inner(); // ERROR!!
     console.log("name is " + result);

     var inner = function() { // 함수표현식 
         return "inner value";
     }
 }
printName(); // > TypeError: inner is not a function
```

함수 선언식으로 작성한 함수는 함수 전체가 호이스팅 된다고 하였는데 전역적으로 선언하게 되면<br>
중복적으로 동명의 함수를 쓰게 되었을때 원치 않는 결과를 초래할 수 있습니다.<br>
이를 방지하려면 함수 표현식으로 작성하면 됩니다.

* 함수 표현식의 선언이 호출보다 아래에 있는 경우

```js
/* 오류 */
 function printName(firstname) { // 함수선언문
     console.log(inner); // ERROR!!
     let result = inner();  
     console.log("name is " + result);

     let inner = function() { // 함수표현식 
         return "inner value";
     }
 }

printName(); // > ReferenceError: inner is not defined
```

console.log(inner);에서 inner에 대한 선언이 되어있지 않기 때문에 inner is not defined 오류가 발생합니다.

### var, let, const에 대해 알아보기

▷ var <br>
 function 단위의 scope를 가진다.<br>
 이는 함수안에서만 선언 될 경우에 scope를 가지는 것을 의미한다.<br>
 if나 for문안에서 var를 선언할 경우 해당 변수는 scope를 if나 for가 아닌 상위의 함수(없으면 전역)를 scope로 가지게 된다.
 
 ```js
 function func() {
	var a = 1; // func scope
 }
 
 console.log(a) // error
 
 for(var i =0; i<10;i++) {
 	var b = 2; // 전역 scope
 }
 
 console.log(b) // 2
 ```
 
 b가 scope가 전역으로 설정이 되면서 다음과 같이 이상한 코드가 발생하게 된다.<br>
function scope와 호이스팅 개념이 만나 다음과 같은 원리로 동작된다고 생각하면 된다 (실제로 이렇게 동작하진 않는다)

```js
var b;
function func() {
	var a;
	a = 1;
 }
 
 console.log(a) // error
 
 for(var i =0; i<10;i++) {
 	b = 2; 
 }
 
 console.log(b) // 2
 ```
 
 ▷ let, const <br>
 let과 const는 es6 이후에 선언 되었다.<br>
 위에 처럼 이상한 코드가 실제로 코드를 작성하는 입장에서는 헷갈릴 만한 요소가 많아 block scope로 설정 되었다.<br>
 blcok scope는 중괄호 단위로 스코프가 설정되는 것으로 이해하면 된다.<br>
 그로인해 for문과 if문안에서 선언할 경우에는 if문과 for문안에서만 사용가능한 변수로 설정이 된다.
 
 ```js
 const b = 1;

for(var i =0; i<10; i++) {
 	coonsole.log(b) // error
    const b = 2; 
}
```

→ let, const는호이스팅이 일어나지 않나요?<br>
이렇게 보면 const, let에선 호이스팅이 일어나지 않는 것처럼 보인다.<br>
하지만 호이스팅이란 개념은 결국 실행컨텍스트로 인해 발생하는 것임으로 const, let, var 모두 호이스팅이 발생한다.

호이스팅이 일어나지 않는 것 처럼 보이는 이유는 const와 let의 값 할당 시점이 var와 다르기 떄문이다.

일반적으로 변수는 다음과 같이 실행된다.<br>
변수 선언 > 변수 초기화 > 변수 값 할당

var의 경우는 변수의 선언과 초기화가 동시에 일어난다.<br> 
이말인 즉 변수가 초기화가 되는데 할당이 되기 이전의 상태로 undefiend상태로 존재하게 된다.<br>
그러나 let과 const는 변수의 선언과 초기화가 동시에 일어나지 않는다.<br>
변수의 선언만 인정되어 실행컨택스트에는 담기게 되나(호이스팅이 일어난다.)<br>
초기화가 되지 않았기 때문에 초기화 되기 이전에 console로 실행시키게 되면 error가 발생하게 된다.
 
```
* var
- function scope
- 호이스팅이 일어난다
- 변수의 선언과 초기화가 동시에 진행된다
```

```
* let, const
- pblock scope
- 호이스팅이 일어난다
- 변수의 선언만 일어난다
- 변수의 선언과 초기화가 동시에 일어나지 않는다
```

### 실행 컨텍스트와 콜 스택

▷ Execution context(실행 컨텍스트)

자바스크립트 코드가 실행되는 환경을 의미한다.<br>
자바스크립트에서 대표적으로 두 가지 타입의 Execution context가 있다.

실행할 코드에 제공할 환경 정보들을 모아놓은 객체들로<br>
자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념이다.

* Global Execution context
자바스크립트 엔진이 처음 코드를 실행할 때 Global Execution Context가 생성된다.<br>
생성 과정에서 전역 객체인 Window Object (Node는 Global) 를 생성하고 this가 Window 객체를 가리키도록 한다.

* Function Execution context
자바스크립트 엔진은 함수가 호출 될 때마다 호출 된 함수를 위한 Execution Context를 생성한다.<br>
모든 함수는 호출되는 시점에 자신만의 Execution Context를 가진다.

※ 자바스크립트는 실행 컨텍스트가 활성화되는 시점에 다음과 같은 현상이 발생한다.

- 호이스팅이 발생한다(선언된 변수를 위로 끌어올린다)
- 외부 환경 정보를 구성한다
- this 값을 설정한다.

▷ call stack

코드가 실행되면서 생성되는 Execution Context를 저장하는 자료구조<br>
엔진이 처음 script를 실행할 때, Global Execution Context를 생성하고 이를 Call Stack에 push한다.

그 후 엔진이 함수를 호출할 때 마다 함수를 위한 Execution Context를 생성하고 이를 Call Stack에 push 한다.

자바스크립트 엔진은 Call Stack의 Top에 위치한 함수를 실행하며 함수가 종료되면 stack에서 제거(pop)하고 제어를 다음 Top에 위치한 함수로 이동한다.

※ 프로그램이 함수 호출을 추적할때 사용한다.

### 스코프 체인, 변수 은닉화

▷ 스코프 체인 (Scope Chain)

스코프는 함수의 중첩에 의해 계층적 구조를 가진다

변수를 참조할 때, 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프로 이동하면서 선언된 변수를 검색한다.

스코프 체인은 outerEnvironmentReference와 밀접한 관계를 가진다.

▷ 변수 은닉화(variable shadowing)

여러 스코프에서 동일한 식별자를 선언한 경우, 무조건 스코프 체인 상에서 가장 먼저 검색된 식별자에만 접근이 가능하다.

```js
(function s(){
let a = 'hi'
})() //a is not defined
```

즉, 직접적으로 변경되면 안되는 변수에 대한 접근을 막는것이다.

```js
function hello(name) {
  let _name = name;
  return function () {
    console.log('Hello, ' + _name);
  };
}

let a = new hello('영서');
let b = new hello('아름');

a() //Hello, 영서
b() //Hello, 아름
```

이렇게 a와 b라는 클로저를 생성하면 함수 내부적으로 접근이 가능하다.
