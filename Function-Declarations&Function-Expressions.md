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

### 함수 선언식 (Function Declarations)

일반적인 프로그래밍 언어에서의 함수 선언과 비슷한 형식이다.

```js
function 함수명() {
  구현 로직
}
```

```js
// 예시
function funcDeclarations() {
  return 'A function declaration';
}
funcDeclarations(); // 'A function declaration'
```

### 함수 표현식 (Function Expressions)

유연한 자바스크립트 언어의 특징을 활용한 선언 방식

```js
var 함수명 = function () {
  구현 로직
};
```

```js
// 예시
var funcExpression = function () {
    return 'A function expression';
}
funcExpression(); // 'A function expression'
```

함수 선언문과 함수 표현식의 차이는 호이스팅을 빼놓고 설명할 수 없다.

### 호이스팅

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

▷ 함수 표현식의 장점

‘함수 표현식이 호이스팅에 영향을 받지 않는다’ 는 특징 이외에도 함수 선언식보다 유용하게 쓰이는 경우는 다음과 같다.

* 클로져로 사용
* 콜백으로 사용 (다른 함수의 인자로 넘길 수 있음)

[함수 표현식으로 클로져 생성하기]

클로져는 함수를 실행하기 전에 해당 함수에 변수를 넘기고 싶을 때 사용된다.<br>
더 쉽게 이해하기 위해 아래 예제를 살펴보자.

```js
function tabsHandler(index) {
    return function tabClickEvent(event) {
        // 바깥 함수인 tabsHandler() 의 index 인자를 여기서 접근할 수 있다.
        console.log(index); // 탭을 클릭할 때 마다 해당 탭의 index 값을 표시
    };
}

var tabs = document.querySelectorAll('.tab');
var i;

for (i = 0; i < tabs.length; i += 1) {
    tabs[i].onclick = tabsHandler(i);
}
```

위 예제는 모든 .tab 요소에 클릭 이벤트를 추가하는 예제다.<br>
주목할 점은 클로져를 이용해 tabClickEvent() 에서 바깥 함수 tabsHandler() 의 인자 값 index 를 접근했다는 점이다.

```js
function tabsHandler(index) {
    return function tabClickEvent(event) {
        console.log(index);
    };
}
```

for 반복문의 실행이 끝난 후, 사용자가 tab 을 클릭했을 때 tabClickEvent() 가 실행된다.<br>
만약 클로져를 쓰지 않았다면 모든 tab 의 index 값이 for 반복문의 마지막 값인 tabs.length 와 같다.

```js
for (i = 0; i < tabs.length; i += 1) {
    tabs[i].onclick = tabsHandler(i);
}
```

클로져를 쓰지 않은 예제를 보자.

```js
var tabs = document.querySelectorAll('.tab');
var i;

for (i = 0; i < tabs.length; i += 1) {
    tabs[i].onclick = function (event) {
      console.log(i); // 어느 탭을 클릭해도 항상 tabs.length (i 의 최종 값) 이 출력
    };
}
```
위 소스는 탭이 3개라고 했을 때, 어느 탭을 클릭해도 i 는 for 반복문의 최종 값인 3이 찍힌다.

문제점을 더 파악하기 쉽게 for 문 안의 function() 을 밖으로 꺼내서 선언해보면

```js
var tabs = document.querySelectorAll('.tab');
var i;
var logIndex = function (event) {
  console.log(i); // 3
};

for (i = 0; i < tabs.length; i += 1) {
    tabs[i].onclick = logIndex;
}
```

logIndex 가 실행되는 시점은 이미 for 문의 실행이 모두 끝난 시점이다. 따라서, 어느 탭을 눌러도 for 문의 최종 값인 3이 찍힌다.

이 문제점을 해결하기 위해 클로져를 적용하면

```js
function tabsHandler(index) {
    return function tabClickEvent(event) {
        // 바깥 함수인 tabsHandler 의 index 인자를 여기서 접근할 수 있다.
        console.log(index); // 탭을 클릭할 때 마다 해당 탭의 index 값을 표시
    };
}

var tabs = document.querySelectorAll('.tab');
var i;

for (i = 0; i < tabs.length; i += 1) {
    tabs[i].onclick = tabsHandler(i);
}
```

for 반복문이 수행될 때 각 i 값을 tabsHandler() 에 넘기고, 클로져인 tabClickEvent() 에서 tabsHandler() 의 인자 값 index 를 접근할 수 있게 된다.<br>
따라서, 우리가 원하는 각 탭의 index 를 접근할 수 있다.

[함수 표현식을 다른 함수의 인자 값으로 넘기기]

함수 표현식은 일반적으로 임시 변수에 저장하여 사용한다.

```js
// doSth 이라는 임시 변수를 사용
var doSth = function () {
  // ...
};
```

함수 표현식을 임시 변수에 넣지 않고도 아래와 같이 콜백함수로 사용할 수 있다.

```js
$(document).ready(function () {
  console.log('An anonymous function'); // 'An anonymous function'
});
```

jQuery 를 사용할 때 많이 보던 문법으로 위와 아래의 코드 결과는 같다.

```js
var logMessage = function () {
  console.log('An anonymous function');
};

$(document).ready(logMessage); // 'An anonymous function'
```

자바스크립트 내장 API 인 forEach() 를 사용할 때도 콜백함수를 사용할 수 있다.

```js
var arr = ["a", "b", "c"];
arr.forEach(function () {
  // ...
});
```

[Reference] https://ko.javascript.info/function-expressions
