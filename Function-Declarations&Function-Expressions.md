## 함수 선언문 (Function Declarations)과 함수 표현식 (Function Expressions) 에서 호이스팅((Hoisting)) 방식의 차이

```js
function getName() {
    console.log('name');
}

var name = function() {
   console.log('name');
};
```

javascript 에서 함수를 변수에 담을 수 있다.<br>
이렇게 사용하는 것을 함수 표현식 이라고 하며,<br>
function getName() 과 같이 함수를 선언하는 것을 함수 선언문이라고 한다.

함수 선언문과 함수 표현식의 차이는 호이스팅을 빼놓고 설명할 수 없다.

▷ 호이스팅

호이스팅(Hoisting)의 사전적 의미는 끌어 올리다 라는 뜻을 가지고 있다.<br>
여기서도 같은 의미로 쓰인다. 함수 안에 있는 변수나 함수 맨위로 끌어올린다는 것이다.<br>

실제로 코드가 끌어올려지는 것은 아니며, 자바스크립트 Parser가 내부적으로 끌어올려서 처리한다.

* 호이스팅 대상

var와 함수 선언문의 호이스팅 대상이다.

let 또는 const 그리고 함수 표현식은 해당되지 않는다.

* 호이스팅 규칙

부등호가 큰 쪽이 먼저 인식된다.

[변수 선언 > 함수 선언]
[할당 되어 있는 변수 > 할당되지 않은 변수]

* 함수 표현식의 호이스팅

```js
count();

var count = function() {
    console.log('count는 1이다.');
}
```

```js
var count = function() {
    console.log('count는 1이다.');
}

count();
```

```js
count();

let count = function() {
    console.log('count는 1이다.');
}
```

첫번째 결과는 TypeError 이다. error : count is not function

count() 호출 후, var count를 선언하며 함수를 담았다.<br>
var 는 호이스팅의 영향을 받으므로 위로 끌어올려진다.<br>
그러므로 아래와 같이 var count; 가 가장 먼저 실행된다. 변수에 아무 값도 담지 않았으므로 undefined 상태이다.<br>
그 후로 count()가 호출되면 위에 선언한 count가 호출되므로 변수를 호출하는 격이된다.<br>
그러므로 not function 이라는 에러 메시지가 나타난다.

```js
var count;    // undefined

count();      // count는 함수가 아닌데 왜 함수를 호출하니?

var count = function() {
    console.log('count는 1이다.');
}
```

두번째 결과는 정상적으로 console.log가 동작한다.

var count가 호이스팅으로 인해 위로 끌어올려지지만,<br>
count() 호출 전 count에 함수를 담으므로 count() 를 호출하였을 때 정상 작동한다.

```js
var count;    //undefined

var count = function() {
    console.log('count는 1이다.');
}

count();
```

세번째 결과는 ReferenceError 이다.

세번째 예시는 첫번째 예시에서 var 를 let으로 바꾼 것이다.<br>
세번째도 역시 에러를 발생하지만, 첫번째와 다른 Referecne Error가 발생한다.<br>
let 은 호이스팅의 영향을 받지 않기 때문에, 예시 작성한 코드 순서대로 실행된다.<br>
그러므로 count()라는 함수가 정의되지 않았는데 호출하였기 떄문이다.

```js
count();

let count = function() {
    console.log('count는 1이다.');
}
```

* 함수선언문의 호이스팅

```js
count();

function count() {
    console.log('count는 2이다.');
}
```

```js
function count() {
    console.log('count는 2이다.');
}

count();
```

첫번째, 두번째 모두 정상 작동한다.<br>
호출이 함수선언문의 위에 있든 아래쪽에 있든 함수 선언문은 호이스팅 영향으로 끌어올려지기 때문이다.

[Reference] https://ko.javascript.info/function-expressions
