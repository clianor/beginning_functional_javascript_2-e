# 1장) 간단하게 살펴보는 함수형 프로그래밍
```text
함수의 첫 번째 규칙은 작야아한다는 것이다.
함수의 두 번째 규칙은 그보다 작아야한다는 것이다.
- 로버트 마틴 -
```

## 1. 함수형 프로그래밍이란?
- 함수형 프로그래밍은 수학에서의 함수와 그 아이디어에서 왔다.
  - 수학에서의 함수 정의
    1. 함수는 인자를 가져야한다.
    2. 함수는 값을 반환해야 한다.
    3. 함수는 외부가 아닌 자체 인자를 받아서만 동작한다.
    4. 주어진 X 하나에 Y는 오직 하나다.
- 모든 함수가 동일한 입력에 대해 동일한 값을 반환하도록 하는것을 `참조 투명성`이라 한다. 
  1. 함수를 결과값으로 치환 가능하다.
  2. 참조 투명성에 의해 병렬 코드와 캐싱이 가능하다.
  3. 참조 투명성에 해당하는 함수는 스레드로 실행될 경우 **락 매커니즘**에서 벗어날 수 있다.

### 함수형 프로그래밍 예시
```js
/*
 * 세금 계산을 위한 calculateTax 함수
 * */

# bad
var percentValue = 5;
var calculateTax = (value) => { return value / 100 * ( 100 + percentValue ) }

# good
var calculateTax = (value, percentValue) => { return value / 100 * ( 100 + percentValue ) }
```

위의 코드는 함수 외부의 percentValue의 영향을 받아 동작하기 때문에 사이드 이펙트가 발생할 수 있다.  
수학에서의 함수의 핵심 정의는 함수 논리가 외부에 의존하지 않는다는 것이므로 percentValue 변수를 함수의 인자로 받아 테스트를 하는데 용이하고 사이드 이펙트를 제거할 수 있다.

### 명령형, 선언형, 추상화
- 함수형 프로그래밍은 **선언**가능하고 **추상화된** 코드 작성에 관한 것이다.
- 명령형 프로그래밍: 컴파일러에게 특정 작업을 어떻게 해야 하는지 알려주는 것.
  - ```js
    # 명령형 형태의 배열 반복
    var array = [1, 2, 3];
    for(i = 0; i < array.length; i++) {
      console.log(array[i]);
    }
    ```
- 선언형 프로그래밍: 컴파일러가 어떻게 작업해야하는지보다 어떤것이 필요한지가 중요한 것.
  - ```js
    # 선언형 형태의 배열 반복
    var array = [1, 2, 3];
    array.forEach((element) => console.log(element));
    ```
- 추상화: 코드의 다른 부분을 재사용하는 추상적인 방법으로 함수를 생성하는것.

## 2. 함수형 프로그래밍의 장점

### 순수함수
- 순수함수: 주어진 입력에 대해 동일한 출력을 반환하는 함수.
  - 순수 함수는 테스트하기 편한 코드다.
    - 항상 동일한 입력에 동일한 출력을 반환하기 때문. 
  - 이상적인 코드
    - 읽기 코드상의 부수효과가 없기 때문.
  - 병렬 코드
    - 순수 함수는 해당 환경을 전혀 변동시키지 않으므로 동기화를 걱정할 필요가 없다.
  - 캐싱
    - 항상 주어진 입력에 대해 동일한 출력을 반환하므로 함수의 결과값을 캐싱할 수 있다.
  - 파이프라인과 컴포저블
    - 순수 함수는 한 번에 하나의 문제만 해결하지만 파이프 라인과 함수 합성으로 이 문제를 해결 할 수 있다.

### 순수 함수는 수학적인 함수다
```text
# 위키피디아
수학에서 함수는 입력 세트와 각 입력이 정확히 해당 출력과 관련이 있는 속성을 가진 출력 세트와의 관계다.
함수에 대한 입력을 인자라고 하며, 출력을 값이라고 부른다.
주어진 함수에 허용되는 모든 입력값을 함수의 도메인이라고 하며, 이때 출력되는 허용 집합을 코도메인이라고 한다.
```