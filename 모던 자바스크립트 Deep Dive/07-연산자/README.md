# JS의 연산자

연산자(operator)는 하나 이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입, 지수 연산 등을 수행하여 하나의 값을 만든다.

연산의 대상을 피연산자(operand) 라고 한다.

피연산자는 값으로 평가될 수 있는 표현식이어야 한다.

---

# 7-1. 산술 연산자

산술 연산자(arithmetic operator)는 피연산자를 대상으로 수학적 계산을 수행하여 새로운 숫자 값을 만든다.

산술 연산이 불가능한 경우, NaN을 반환한다.

## 7-1-1. 이항 산술 연산자

이항(binary) 산술 연산자는 2개의 피연산자를 산술 연산하여 숫자 값을 만든다. <br />
어떤 산술 연산을 해도 피연산자의 값이 바뀌는 경우가 없다.

항상 새로운 값을 만든다.

![이항 산술 연산자 표](https://github.com/user-attachments/assets/6304ae32-e410-417d-be5b-579be1f2abdd)

```js
console.log(5 + 2) // 7
console.log(5 - 2) // 3
console.log(5 * 2) // 10
console.log(5 / 2) // 2.5
console.log(5 % 2) // 1
```

![실행 결과](https://github.com/user-attachments/assets/11d178a6-9cee-4f0b-a9a2-7a41663bbff7)

## 7-1-2. 단항 산술 연산자

단항(unary) 산술 연산자는 1개의 피연산자를 산술 연산하여 숫자 값을 만든다.

![단항 산술 연산자 표](https://github.com/user-attachments/assets/d05c4c20-0e8a-419b-8e58-cf885e168e68)

```js
var x = 1

x++
console.log(x) // 2

x--
console.log(x) // 1
```

![실행 결과](https://github.com/user-attachments/assets/409d6e9e-4dfd-416f-aaf4-eb058273ce05)

> ⚠️ **주의**: 증가/감소 연산자가 피연산자 앞에 위치하면 먼저 값을 증가/감소하고, 뒤에 위치하면 다른 연산을 수행한 후 값을 증가/감소한다.

## 7-1-3. 문자열 연결 연산자

연산자는 피연산자 중 하나 이상이 문자열인 경우, 문자열 연결 연산자로 동작한다.

```js
console.log('Hello' + ' ' + 'World') // 'Hello World'
console.log(1 + '2') // '12'
```

![실행 결과](https://github.com/user-attachments/assets/ced779bd-11e4-4571-8e06-8eacab94ca6f)

개발자의 의도와 관계없이 JS 엔진이 암묵적 타입 변환을 통해 타입을 자동 변환할 수 있다.

⸻

# 7-2. 할당 연산자

할당 연산자(assignment operator)는 우항의 평가 결과를 좌항 변수에 할당한다.

![할당 연산자 표](https://github.com/user-attachments/assets/a9004ef5-7b55-49cd-83fa-60362e09ec96)

```js
let a, b, c
a = b = c = 0
console.log(a, b, c) // 0 0 0
```

할당문은 표현식인 문으로, 값으로 평가되어 여러 변수에 연쇄 할당할 수 있다.

⸻

# 7-3. 비교 연산자

비교 연산자(comparison operator)는 좌항과 우항의 피연산자를 비교하여 불리언 값을 반환한다.

주로 if문이나 for문과 같은 조건문에서 사용된다.

![비교 연산자 표](https://github.com/user-attachments/assets/ce908d1c-1d91-4310-9f94-1ca27dd2f898)

동등 비교 연산자는 좌항과 우항의 피연산자를 비교할 때, 먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교한다.

![실행 결과](https://github.com/user-attachments/assets/e7711ed6-55cf-47dd-bdac-36946924a34f)

![실행 결과](https://github.com/user-attachments/assets/1699d3d4-227d-4a58-8885-79942484a17a)

따라서 동등 비교 연산자는 편리하지만, 결과 예측이 어렵고 실수하기가 쉽다.

> **안티 패턴이란?** 가독성, 성능, 유지보수 등에 부정적인 영향을 줄 수 있어 지양하는 패턴

일치 비교 연산자는 좌항과 우항의 피연산자가 타입도 같고 값도 같은 경우에 한하여 true를 반환한다.

??? 그런데 일치 비교에서 주의해야 하는 것은 NaN이다.

![실행 결과](https://github.com/user-attachments/assets/662e7126-f822-4fa9-815b-482985bdc0e5)

???? 숫자 0도 양의 0과 음의 0이 있는데 이걸 비교하면 true를 반환한다.

![실행 결과](https://github.com/user-attachments/assets/683161de-38c5-4f92-a8d4-7b9ca52c2c51)

⸻

# 7-4. 삼항 조건 연산자

삼항 조건 연산자(ternary operator)는 조건식의 평가 결과에 따라 반환할 값을 결정한다.

![삼항 조건 연산자](https://github.com/user-attachments/assets/55ab08c4-4793-4ebf-acca-735c777961d2)

```js
const age = 18
const isAdult = age >= 18 ? '성인' : '미성년자'
console.log(isAdult) // '성인'
```

삼항 조건 연산자는 `if … else 문`을 사용해도 처리할 수 있는데, 다른 점은 `if … else문`은 **값처럼 사용할 수 없다** 는 점이다.

![image](https://github.com/user-attachments/assets/34d53591-4879-473b-aac1-9cc21b86cae5)

조건에 따라 어떤 값을 결정해야 한다면 삼항 조건 연산자를 사용하는 게 유리할 수도 있지만, 조건에 따라 수행해야 할 문이 여러 개라면 if … else의 가독성이 더 좋다

⸻

# 7-5. 논리 연산자

논리 연산자(logical operator)는 논리값을 조합하거나 반전하여 새로운 논리값을 만든다.

![논리 연산자 표](https://github.com/user-attachments/assets/9a85a3ef-f7ef-463e-9fe7-d33177ed7a9d)


```js
console.log(true && false) // false
console.log(true || false) // true
console.log(!true) // false
```

논리 부정 연산자는 언제나 불리언 값을 반환한다.
논리합 또는 논리곱 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다,
-> “단축 평가” 9.4절 참고

> 복잡한 표현식은 드 모르간의 법칙을 이용해서 가독성 좋은 표현식으로 변환할 수도 있다!

⸻

# 7-6. 쉼표 연산자

쉼표는 왼쪽 피연산자부터 차례대로 피연산자를 평가한다.

마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환한다.

⸻

# 7-7. 그룹 연산자

소괄호로 피연산자를 감싸면 해당 피연산자 표현식을 가장 먼저 평가하게 된다.

우선순위가 가장 높아진다.

⸻

# 7-8. typeof 연산자

피연산자의 데이터 타입을 문자열로 반환한다. <br />
null을 반환하는 경우는 없고, 함수의 경우에는 function을 반환한다.

typeof 연산자로 null 값을 연산해보면 object를 반환하는데 이건 JS의 첫 번째 버전의 버그이다.

![실행 결과](https://github.com/user-attachments/assets/7696d866-bfc3-4af5-8f63-6f7da4607992)

따라서 null 타입인지 확인할 때는 일치 연산자를 활용해야 한다.

또! 선언하지 않은 식별자를 typeof 연산자로 연산하면 ReferenceError가 아니라 undefined를 반환해버린다….

![실행 결과](https://github.com/user-attachments/assets/8ccb8d7f-24df-4385-917f-f8334452f455)

⸻

# 7-9. 지수 연산자
ES7에서 도입된 지수 연산자는 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭제곱하여 숫자를 반환한다. <br />
지수 연산자 도입 전에는 Math.pow 메서드를 사용했다.

지수 연산자의 결합 순서는 우항에서 좌항(우결합성)이므로 가독성이 더 좋다!

```js
2 ** (3 ** 2) // 512
Math.pow(2, Math.pow(3, 2)) // 512
```

음수를 밑으로 사용하려면 괄호로 묶으면 된다.

⸻

# 결론

JavaScript의 다양한 연산자를 활용하여 산술, 할당, 비교, 논리 연산 등을 수행할 수 있다.

연산자의 우선순위와 결합 순서를 고려하여 적절하게 사용해야 한다.
