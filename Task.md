## 실습 과제

1. 콘솔에 찍힐 b 값을 예상해보고, 어디에서 선언된 “b”가 몇번째 라인에서 호출한 console.log에 찍혔는지, 왜 그런지 설명해보세요.<br>
주석을 풀어보고 오류가 난다면 왜 오류가 나는 지 설명하고 오류를 수정해보세요.

```js
let b = 1;

function hi () {

const a = 1;

let b = 100;

b++;

console.log(a,b); // a = 1, b = 101

}

//console.log(a); // a = undifend

console.log(b); // b = 1

hi();

console.log(b); // b = 1
```

▽ 풀이

1) console.log(b)
 : 1 (전역 변수로 let = 1로 선언)
 
2) console.log(b)
 : 1, 101 (함수 내에서 a와 b를 선언하였고, 그 값을 출력함)
 
3) console.log(b)
 : 1 (전역 변수로 let b = 1로 선언했던 값이 출력)
 
* 주석 오류 원인
 : a의 선언을 function hi() 내부에서 했기 때문에 값을 불러올 수 없고,<br>
 전역 변수로 선언을 해주면 오류가 발생하지 않는다.
 
 
