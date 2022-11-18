## 스코프 체인, 변수 은닉화

▷ 스코프 체인(Scope-chain)

scope를 안에서부터 바깥으로 차례로 검색해나가는 것

여러 scope에서 동일한 식별자를 선언한 경우에는 무조건 scope chain 상에서 가장 먼저 발견된 식별자에만 접근 가능하게 된다.

```js
var a = 1;
var outer = function () {
    var inner = function () {
        console.log(a);
        var a = 3;
    }
    inner();
    console.log(a);
}
outer();
console.log(a);
```

위 코드가 실행되면 가장 먼저 전역 컨텍스트가 활성화됩니다.<br> 
전역 컨텍스트의 environmentRecord에는 { a, outer } 식별자가 저장됩니다.<br>
그 다음 outer 함수가 실행되어 전역 컨텍스트는 중단되고 outer 컨텍스트가 실행됩니다.<br>
outer environmentRecord에 { inner } 식별자가 저장이 되고, outer 함수는 전역 공간에서 선언됐으므로 전역 컨텍스트의 LexicalEnvironment를 참조복사합니다.<br>
이를 표기하면 [ GLOBAL, { a, outer } ]입니다. 이 과정이 반복됩니다.

여기서 a 출력의 순서와 값은 inner 함수 안의 a(undefined), outer 함수 안의 a(1), 전역의 a(1) 이 됩니다.여기서 a 출력의 순서와 값은 inner 함수 안의 a(undefined), outer 함수 안의 a(1), 전역의 a(1) 이 됩니다.<br>
이 결과는 스코프 체인 상에서 가장 가까운 a가 먼저 발견되었기 때문입니다.(inner 실행 컨텍스트의 environmentRecord에 a가 저장되었기 때문) 위와 같은 케이스를 변수 은닉화라고 합니다.

스코프 체인 중 현재 실행 컨텍스트를 제외한 상위 스코프 정보들을 개발자 도구를 통해 확인 가능합니다.<br>
함수 내부에서 console.dir()을 통해 함수를 출력하는 것입니다. 

함수는 전역에서 정의할 수도 있고 함수 몸체 내부에서도 할 수 있다.<br> 
함수 몸체 내부에서 함수가 정의된 것을 '함수의 중첩', 중첩 함수를 포함하는 함수를 '외부 함수'라고 한다.

함수는 중첩될 수 있으므로 함수의 지역 스코프도 중첩될 수 있는데, 이는 스코프가 함수의 중첩에 의해 계층적 구조를 갖는다는 것을 의미한다.<br>
이때 외부 함수의 지역 스코프를 중첩 함수의 상위 스코프라고 지칭한다.

```js
var x = "global x";
var y = "global y";

function outer (){
  var z = "outer's local z";
  console.log(x); //global x
  console.log(y); //global y
  console.log(z); //outer's local z
  
  function inner (){
    var x = "inner's local x";
    console.log(x); //inner's local x
    console.log(y); //global y
    console.log(z); //outer's local z
  }
  inner();
}
outer();

console.log(x); //global x
console.log(z); //ReferenceError: z is not defined
```
위의 예제 코드는 outer 함수가 만든 지역 스코프 내에 inner 함수가 만든 지역 스코프가 있으므로 outer 스코프가 inner 스코프의 상위 스코프이다.<br> 
그리고 outer 함수의 지역 스코프의 상위 스코프는 전역 스코프이다.

이처럼 모든 스코프는 하나의 계층적 구조로 연결되며, 모든 지역 스코프의 최상위 스코프는 전역 스코프이다.

* 스코프 체인에 의한 변수 검색
변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 참조할 변수가 존재하지 않는다면 상위 스코프의 방향으로 이동하며 선언된 변수를 검색한다.<br>
이를 통해 상위 스코프에서 선언한 변수를 하위 스코프에서 참조할 수 있다.<br>
반대로 하위 스코프에서 유효한 변수는 상위 스코프에서 참조할 수 없다.

▷ 캡슐화와 정보 은닉

캡슐화는 객체의 상태를 나타내는 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.<br> 
캡슐화는 객체의 특징 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

정보 은닉은 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호한다.

대부분의 객체지향 프로그래밍 언어는 클래스를 정의하고 그 클래스를 구성하는 멤버에 public, private, protected 같은 접근 제한자를 선언하여 공개 범위를 한정할 수 있다.<br>
하지만 자바스크립트는 접근 제한자를 제공하지 않는다.
