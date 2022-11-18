
## 느슨한 타입(loosely typed)의 동적(dynamic) 언어

JavaScript는 느슨한 타입(loosely typed)의 동적(dynamic) 언어이다.<br>
JavaScript의 변수는 어떤 특정 타입과 연결되지 않으며, 모든 타입의 값으로 할당 및 재할당 가능하다.


```js
let foo = 42 // foo : 숫자
foo = 'bar' // foo : 문자열
foo = ture //  foo : 불리언
```

## 느슨한 타입(loosely typed)의 동적(dynamic) 언어의 문제점과 보완할 수 있는 방법

* 실행 도중에 변수에 예상치 못한 타입이 들어와 타입에러가 발생할 수 있음
* 동적타입 언어는 런타임 시 확인할 수 밖에 없기 때문에, 코드가 길고 복잡해질 경우 타입 에러를 찾기가 어려워짐
* 이러한 불편함을 해소하기 위해 TypeScipt나 Flow 등을 사용할 수 있음
