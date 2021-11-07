# 3장) 고차함수

## 고차함수
- 인자로 다른 함수를 전달받는 함수를 `고차함수`라 한다.
  - 3장부터는 고차함수를 HOC라 통칭.

## 데이터의 이해

### 자바스크립트 데이터형 이해
- 숫자(Number)
- 문자열(String)
- 불리언(Boolean)
- 객체(Object)
- 널(Null)
- 정의되지 않은(Undefined)

### 함수 저장
- 자바스크립트에서 함수는 변수 안에 저장할 수 있다.

### 함수 전달
- 자바스크립트는 인자로 다른 함수를 전달할 수 있기 때문에 그렇게 인자로 받은 함수를 함수내에서 실행할 수 있다.

### 함수 반환
- 자바스크립트는 함수를 인자로 전달할 수 있는것뿐만 아니라 반환값으로 함수를 넘겨줄수도 있다.

## 추상화와 고차 함수
- 일반적으로 고차 함수는 일반적인 문제를 추출하고자 작성된다.
  - 고차 함수는 추상화를 정의하는 것이라고 할 수 있다.

### 추상화의 정의
```text
# 위키피디아
소프트웨어 공학과 컴퓨터 과학에서 추상화는 복잡한 컴퓨터 시스템을 다루기 위한 기술이다.
현재 단계보다 더 복잡한 내용을 제외하면서 인간과 시스템이 상호작용하는 계층을 구성한다.
프로그래머는 이상적인 인터페이스를 사용하며, 그렇지 않으면 다루기에 너무 복잡한 추가적인 함수 게층을 넣을 수 있다.
```

## 현실에서의 고차 함수

### every 함수
- 이 함수는 배열의 모든 요소가 전달된 함수에 대해 참인지 확인한다.
```js
const every = (arr, fn) => {
    let result = true;
    for(let i = 0; i < arr.length; i++)
        result = result && fn(arr[i]);
    return result;
}

every([NaN, NaN, NaN], isNaN);
=> true

every([NaN, NaN, 4], isNaN);
=> false
```
위의 every는 기존의 for 루프를 추상화한 for ... of를 이용해 다음과 같이 추상화 할 수 있다.
```js
const every = (arr, fn) => {
    let result = true;
    for (const value of arr)
        result = result && fn(value);
    return result;
}
```

### some 함수
- some 함수는 every 함수와는 반대로 전달된 함수에 대해 배열 요소가 참을 반환하면 참을 반환하는 함수이다.
```js
const some = (arr, fn) => {
    let result = false;
    for (const value of arr)
        result = result || fn(value);
    return result;
}

some([NaN, NaN, 4], isNaN);
=> true

some([3, 4, 4], isNaN);
=> false
```

### sort 함수
- sort 함수는 자바스크립트의 Array 프로토타입에서 사용 가능한 내장 함수이다.
```js
var fruit = ['cherries', 'apples', 'bananas'];
fruit.sort();
=> ['apples', 'bananas', 'cherries'];
```
- sort 함수는 함수를 인자로 취하는 고차 함수로 compareFunction을 인자로 넘겨 정렬에 관한 논리를 정할 수 있게 한다.
```js
const sortBy = (property) => {
    return (a, b) => {
        var result = (a[property] < b[property]) ? -1 :
            (a[property] > b[property]) ? 1 : 0;
        return result;
    }
};

var people = [
    { firstname: "aaFirstName", lastname: "cclastName" },
    { firstname: "ccFirstName", lastname: "aalastName" },
    { firstname: "bbFirstName", lastname: "bblastName" },
];

people.sort(sortBy("firstname"));
=>  [
        { firstname: "aaFirstName", lastname: "cclastName" },
        { firstname: "bbFirstName", lastname: "bblastName" },
        { firstname: "ccFirstName", lastname: "aalastName" }
    ]
```
