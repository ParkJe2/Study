## 실행 컨텍스트(Excution Context)와 콜 스택(Call Stack)

▷ 실행 컨텍스트 (Excution Context)

코드의 실제 진행 상황을 추적하는데 필요한 정보를 모아둔 구조

자바스크립트가 로드되고 엔진이 이를 처리하면서 실행 컨텍스트를 만듭니다.<br>
실행 컨텍스트는 어떤 코드를 실행하고 있고 이 컨텍스트에는 어떤 변수가 있는지에 대한 정보를 가지고 있습니다.

```js
function appedString(str1, str2) { //(1)
	let result = `${str1}${str2}`;
	return result;
}

let appended = appendString("홍길동","붙임"); //(2)
```
 
먼저 전역에 선언된 함수나 변수를 전역 실행 컨텍스트의 메모리에 등록합니다.<br>
1번 라인의 appendString함수를 전역메모리에 등록해 줍니다.<br>
그 후, appendString의 결과값이 담길 appended 변수를 만들어 메모리에 등록합니다.

```js
function appedString(str1, str2) {
	let result = `${str1}${str2}`; //(2)
	return result;
}

let appended = appendString("홍길동","붙임"); //(1)
```

함수명 뒤에 ()를 붙여 함수를 호출해 줍니다.<br>
위의 예시에서는 appendString("홍길동","붙임")을 호출해 줍니다.<br>
그 후 str1 매개변수에 "홍길동" str2 매개변수에 "붙임"을 할당한 후 result 변수를 지역메모리에 생성합니다.

```js
function appedString(str1, str2) {
	let result = `${str1}${str2}`; //(1)(2)
	return result; //(3)
}

let appended = appendString("류호진","붙임");
```

str1,str2 문자열 합치기 연산을 실행하여 result에 할당하고 해당 result를 반환하여 전역메모리의 appended에 할당합니다.

```js
function appedString(str1, str2) {
	let result = `${str1}${str2}`;
	return result;
}

let appended = appendString("류호진","붙임"); // (1)
```

이렇게 appendString 함수가 종료되고 나면 함수의 실행컨텍스트와 내부 정보들은 모두 사라지게 됩니다.

정리 하자면, 함수 뒤에 ()를 붙여 서브루틴이 호출되면 실행 컨텍스트가 만들어지고 해당 함수가 끝나면 실행컨텍스트가 사라지게 됩니다.<br>
실행 컨텍스트엔 함수 실행에 필요한 정보가 모여 있다고 볼 수 있습니다.

[실행 컨텍스트 구성요소]

: 변수 객체(arguments, variable)

: scope chain

: this

[실행 컨텍스트의 종류]

전역 컨텍스트 : 특정 함수가 실행되지 않는 한 전역 컨텍스트에서 실행됩니다.

함수 컨텍스트 : 함수가 실행될 때 마다 실행 컨텍스트가 생성되고 함수가 동작을 다하면 콜스택에서 삭제됩니다.

Eval함수 실행 컨텍스트 : eval 함수도 자신만의 실행 컨텍스트를 가집니다.

eval 함수 : 문자열을 코드로 인식하게 하는 함수 eval('2+2') => 4, eval is evil 이라는 말도 있으니 지양하는 것이 좋음

쓸 필요성을 크게 못느끼면 쓰지 않는게 좋음

```js
var work = "프로그래밍"
function doing(value){
	console.log(`${value} ${work} 중 입니다`);
}
function whatareyoudoing(){
	var work = "문서작업";
	doing("나는");
}

whatareyoudoing();
```

이 소스코드의 결과 값은 모두 다 아시겠지만 나는 프로그래밍 입니다.<br>
이러한 결과가 어떻게 나오는지 좀 더 명확하게 보기 위해서 해당 코드의 실행 컨텍스트들을 보겠습니다.

▷ 전역 컨텍스트

```js
{ //전역 컨텍스트
	"변수 객체" : {
		arguments : null,
		variable : ["work","doing","whatareyoudoing"]
	},
	scopeChain : ["전역 변수 객체"],
	this : window
}
```

* whatareyoudoing 함수 컨텍스트

```js
{ //whatareyoudoing 함수 컨텍스트
	"변수 객체" : {
		arguments : null,
		variable : ["work"]
	},
	scopeChain : ["whatareyoudoing 변수 객체","전역 변수 객체"],
	this : window
}
```

* doing 함수 컨텍스트

```js
{ //doing 함수 컨텍스트
	"변수 객체" : {
		arguments : [{value : "나는"}],
		variable : null
	},
	scopeChain : ["doing 변수 객체","전역 변수 객체"],
	this : window
}
```

▷ 콜 스택 (Call Stack)

자바스크립트는 단일 스레드 프로그래밍 언어이므로, 하나의 콜 스택만 존재합니다.<br> 
즉, 하나의 일만 처리할 수 있다는 뜻입니다.<br>
콜 스택은 여러 함수들을 호출하는 스크립트에서 해당 위치를 추적하는 엔진을 위한 매커니즘이며 현재 어떤 함수가 동작하고 있는지,<br>
그 함수 내에서 어떤 함수가 동작하는지, 다음에 어떤 함수가 호출되어야 되는지를 제어합니다.

* 스크립트가 함수를 호출하면 엔진은 이를 콜 스택에 추가하고 함수를 수행합니다.
* 해당 함수에 의해 호출되는 모든 함수들도 호출 스택에 추가되고 호출이 도달하는 위치에서 실행됩니다.
* 메인함수가 끝나면 엔진은 스택을 제거하고 메인 코드 목록에서 중단된 실행을 다시 시작합니다.
* 스택에 할당된 공간보다 많은 공간을 차지하면 stack overflow 에러가 발생하게 됩니다.

```jc
function sayAtoB(a,b){ // (1)
	let message = `${a}님이 ${b}님을 호출하셨습니다.`;
	return message;
}

function employee(a,b,func){ // (2)
	let result func(a,b)
	return result;
}

let action = employee("홍동길","홍길동",sayAtoB); // (3)
```

위의 실행 컨텍스트에서와 마찬가지로 sayAtoB라는 이름의 함수를 전역 컨텍스트의 메모리에 등록하고<br>
그 후, employee라는 이름을 가진 함수를 전역 컨텍스트 메모리에 등록합니다.<br>
마지막으로 calc의 결과 값을 받을 action 변수를 만들어 줍니다.

```js
function sayAtoB(a,b){
	let message = `${a}님이 ${b}님을 호출하셨습니다.`;
	return message;
}

function employee(a,b,func){
	let result = func(a,b)
	return result;
}

let action = employee("홍동길","홍길동",sayAtoB); // (1)
```

employee 함수를 ()을 이용해 호출합니다.<br>
그럼 함수의 실행 컨텍스트가 새로 만들어 집니다. 이 때 employee 함수의 실행 컨텍스트가 callStack에 쌓이게 됩니다.<br>
자바스크립트 엔진은 현재 내가 실행하고 있는 실행 컨텍스트가 무엇인지 callStack의 top()을 통해서 인지하게 됩니다.<br>
그리고 a, b, func이라는 매개변수에 각 값을 연결해 줍니다.

```js
function sayAtoB(a,b){
	let message = `${a}님이 ${b}님을 호출하셨습니다.`;  // (2)
	return message;
}

function employee(a,b,func){
	let result = func(a,b) // (1)
	return result;
}

let action = employee("홍동길","홍길동",sayAtoB);
```

이 후 함수의 내부 코드를 실행하면 func의 결과 값으로 같는 result를 지역메모리에 등록합니다.<br> 
func도 ()를 만나서 호출되게 되고 그럼 또 func()실행 컨텍스트가 만들어질 것입니다.<br>
콜스택에도 func 실행 컨텍스트가 push됩니다.<br>
이 후 앞선 작업들과 똑같이 a와 b의 매개변수에 값이 연결되고 message라는 변수를 만들어주고,<br>
'${a}님이 ${b}님을 호출하셨습니다'의 연산결과가 message에 할당 될 것입니다.

```js
function sayAtoB(a,b){
	let message = `${a}님이 ${b}님을 호출하셨습니다.`;
	return message; // (1)
}

function employee(a,b,func){
	let result = func(a,b) // (1)
	return result;
}

let action = employee("홍동","홍길동",sayAtoB);
```

이 후 message가 리턴되어 result에 할당되고 func함수 실행 컨텍스트는 종료되고 콜 스택에서도 pop되어 사라지게 됩니다.

```js
function sayAtoB(a,b){
	let message = `${a}님이 ${b}님을 호출하셨습니다.`;
	return message;
}

function employee(a,b,func){
	let result = func(a,b) 
	return result; // (1)
}

let action = employee("홍동","홍길동",sayAtoB);
```

이 후 result의 값이 리턴되어서 action에 할당됩니다.<br> 
action값에 할당되면서 employee 실행 컨텍스트 또한 종료되고 콜스택에서 employee가 pop되어 사라지게 됩니다.

정리하자면, 콜스택의 바닥엔 전역 컨텍스트가 존재하고,<br>
함수가 호출될 때 해당 함수의 실행컨텍스트가 콜스택에 puish되고 함수가 종료되면 pop됩니다.
