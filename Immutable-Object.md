## 불변 객체

한 번 객체가 생성되면, 그 상태를 바꿀 수 없는 객체

자바스크립트에서 불변 객체를 만들 수 있는 방법은 기본적으로 2가지 인데 const와 Object.freeze()를 사용하는 것이다.

▷ const

자바스크립트 키워드 중 하나인 const이다. ES6문법부터 let과 const를 지원한다.

const 키워드는 변수를 상수로 선언할 수 있다, 일반적으로 상수로 선언된 변수는 값을 바꾸지 못하는 것으로 알려져 있다.

그렇다면 상수로 선언한 객체는 불변 객체일까? 

```js
const test = {};
test.name = "mingyo";

console.log(test);  // {"mingyo"}
```

ES6에서의 const는 할당된 값이 상수가 되는 것이 아닌 바인딩된 값이 상수가 되는,<br>
즉 test변수가 상수가 되기 때문에 const 키워드로 선언된 test변수에는 객체 재할당은 불가능하지만 객체의 속성은 변경 가능하다.

재할당이 불가능 한 이유는 변수와 값(객체) 사이의 바인딩 자체가 변경이 되기 때문에 상수인 test변수는 재할당이 불가능한 것이고,
객체의 속성이 변경가능 한 이유는 실제 객체가 변경은 되지만 ( {} -> name : "mingyo" ) 객체와 변수(test)사이의 바인딩은 변경이 되지 않기 때문이다.

때문에 비록 재할당은 불가능하지만 객체의 속성을 변경함으로 인해 변수에 바인딩된 객체의 내용까지 변경이 되기 때문에 불변객체라고 하기는 힘들다.<br>
따라서 Object.freeze()라는 JS내장메소드도 살펴보도록 하겠다.

▷ Object.freeze()

자바스크립트에서 기본적으로 제공하는 메소드인 Object.freeze() 메소드이다. 공식 문서에서는 "객체를 동결하기 위한 메소드" 라고 적혀있다.

```js
let test = {
    name : 'kim'
}

Object.freeze(test);
```

사용법은 간단하다. test 변수에 key value를 가진 객체를 바인딩 후 Object.freeze(test)를 사용해 바인딩된 변수를 동결 객체로 만들었다.<br>
때문에 test 객체는 객체의 속성을 변경하는 시도는 불가능하다.

```js
test.name = 'Jung';
console.log(test) // {name: 'kim'}
```

위와 같이 객체의 속성을 변경하는 시도는 무시된다.
그러나, Object.freeze()는 동결된 객체를 반환하지만 객체의 재할당은 가능하다. 

```js
test = {
    age : 15
};
console.log(test); // {age: 15}
```

위와 같이 객체의 재할당은 가능하다. 때문에 Object.freeze()도 불변 객체라고 할 수는 없을 것 같다.

최종적으로 불변 객체는 const와 Object.freeze()를 조합하여 만들 수 있다.<br>
(const의 재할당불가 + Object.freeze()의 객체속성 변경불가)

```js
const test = {
    'name' : 'jung'
};

Object.freeze(test);
```

먼저 const키워드로 바인딩 된 변수를 상수화 시킨 다음,<br>
Object.freeze()로 해당 변수를 동결 객체를 만들면 객체의 재할당과 객체의 속성 둘 다 변경불가능한 불변 객체가 된다.



