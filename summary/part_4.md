# 3장) 클로저와 고차 함수

## 클로저의 이해

### 클로저 
- 클로저는 다른 함수 내에 있는 내부 함수이다.
```js
function outer() {
    function inner() {}
}
```
- 위와코드에서 inner 함수를 클로저 함수라고 부른다.
- 클로저 함수는 스코프 체인에 접근할 수 있어 유용하다.
- 클로저는 세 개의 스코프를 가지고 있다.
  1. 자체 선언 내에서 선언된 변수
  2. 전역 변수에 접근
  3. 외부 함수의 변수에 접근

## 실제 세계에서의 고차 함수

### tap 함수
- tap 함수는 value 스코프에 접근할 수 있는 클로저를 갖는 함수를 반환하며 실행된다.
```js
const tap = (value) => {
    return (fn) => {
        return typeof(fn) === 'function' && fn(value), console.log(value);
    }
}

tap('fun')((it) => console.log('value is', it))
=> value is fun
=> fun
```

### unary 함수
- unary 함수는 n개 인자를 가진 함수를 받고 단일 인자로 변환하는 함수.
```js
const unary = (fn) => {
    return fn.length === 1 ? fn : (arg) => fn(arg);
}

['1', '2', '3'].map(parseInt);
=> [1, NaN, NaN]

['1', '2', '3'].map(unary(parseInt));
=> [1, 2, 3]
```

### once 함수
- 주어진 함수를 한 번만 실행하게 하는 함수.
```js
const once = (fn) => {
    let done = false;
    return function() {
        return done ? undefined : ((done = true), fn.apply(this, arguments));
    }
}

var doPayment = once(() => {
    console.log("Payment is done");
});

doPayment();
=> Payment is done
doPayment();
=> undefined
```
- 위의 doPayment 함수는 여러번 호출되어도 한 번만 실행된다.

### memorize 함수
- 순수 함수는 주어진 입력에 대해 동일한 출력을 반환하기 때문에 캐싱을 할 수가 있다.
- memorize 함수는 캐싱을 위한 함수이다.
```js
const memorized = (fn) => {
    const lookupTable = {};
    return (arg) => lookupTable[arg] || (lookupTable[arg] = fn(arg));
}

const fastFactorial =   ((n) => {
    if (n === 0) {
        return 1;
    }
    return n * fastFactorial(n - 1);
})

fastFactorial(5);
=> 120
fastFactorial(3);
=> 6 // lookupTable 테이블에서 반환됨
```

### assign 함수
- 객체를 병합하는 함수 ES6의 Object.assign 함수와 동일.
```js
function objectAssign(target, source) {
    var to = {};
    for (var i = 0; i < arguments.length; i += 1) {
        var from = arguments[i];
        var keys = Object.keys(from);
        for (var j = 0; j < keys.length; j += 1) {
            to[keys[j]] = from[keys[j]];
        }
    }
    return to;
}

var a = { name: "srikanth" };
var b = { age: 30 };
var c = { sex: "M" };
var customObjectAssign = objectAssign(a, b, c);
console.log(customObjectAssign);
=> {name: "srikanth", age: 30, sex: "M"}
var nativeObjectAssign = Object.assign(a, b, c);;
console.log(nativeObjectAssign);
=> {name: "srikanth", age: 30, sex: "M"}
```
- 단 Object.assign을 사용하면 객체 a가 합쳐진 결과값으로 갱신되기 때문에 아래와 같이 사용해야 한다.
```js
var nativeObjectAssign = Object.assign({}, a, b, c);;
console.log(nativeObjectAssign);
```

### entries 함수
```js
var book = {
  id: 111,
  title: "C# 6.0",
  author: "ANDREW TROELSEN",
  rating: [4.7],
  reviews: [{ good: 4, excellent: 12 }]
}

function objectEntries(target) {
  var to = [];
  for (var key in target) {
    to.push([key, target[key]])
  }
  return to;
}

objectEntries(book)[1];
=> ['title', 'C# 6.0']
Object.entries(book)[1];
=> ['title', 'C# 6.0']
```