# 2장) 자바스크립트 함수의 기본

## 함수 생성과 실행

### 엄격한 방식
- 엄격한 방식은 자바스크립트의 제한된 변형.
  - 엄격한 방식으로 실행되는 자바스크립트 코드는 그렇지 않은 동일한 코드와 의미적으로 다를 수 있음.
  - 변수 선언시 var, let, const를 붙여주지 않으면 에러가 난다.

### ES5 함수는 ES6 이후에서도 동작한다.
- 새로운 버전에서는 기존 함수 문법을 바꾸진 않는다.

### 반복 문제에 대한 첫 번째 함수적 접근
```js
# bad
var array = [1, 2, 3];
for(i = 0; i < array.length; i++)
    console.log(array[i]);

# good
const forEach = (array, fn) => {
    let i;
    for(i = 0; i < array.length; i++)
        fn(array[i]);
}

var array = [1, 2, 3];
forEach(array, (data) => console.log(data));
```