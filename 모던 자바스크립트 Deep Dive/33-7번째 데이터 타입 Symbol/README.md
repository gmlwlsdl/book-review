# 1. Symbol이란? (ES6 도입)

심벌이란 JS의 7번째 데이터 타입이다.

immutable ~~(이쯤하면 외웠겠지 “변경 불가능”이라는 뜻임)~~ 한 원시 값이다.

유일무이한 값을 생성할 수 있고,

주로 충돌 위험이 없는 개체의 프로퍼티 키로 사용한다.

```jsx
const mySymbol = Symbol()
console.log(typeof mySymbol) // 'symbol'
```

# 2. Symbol 함수

`new Symbol()`은 생성자가 아니기 때문에 에러를 발생시킨다.

선택적 인수로 설명을 전달할 수도 있다. (디버깅 용도)

```jsx
const s1 = Symbol('desc')
const s2 = Symbol('desc')
console.log(s1 === s2) // false → 항상 유일함
```

# 3. 심벌의 특징

`toString()`이나 `description` 접근은 가능하다.

```jsx
const s = Symbol('mySymbol')
console.log(s.toString()) // Symbol(mySymbol)
console.log(s.description) // 'mySymbol'
```

그러나 `+ 연산자`나 숫자 변환은 불가능하다.

```jsx
console.log(s + '') // TypeError
console.log(+s) // TypeError
```

불리언 변환은 가능하다.

```jsx
if (s) console.log('값 있음') // 가능
```

# 4. Symbol.for / Symbol.keyFor

전역 심벌 레지스트리(Global Symbol Registry)를 사용한다.

문자열 키로 등록 후 공유가 가능하다.

```jsx
const s1 = Symbol.for('shared')
const s2 = Symbol.for('shared')
console.log(s1 === s2) // true

console.log(Symbol.keyFor(s1)) // 'shared'
```

# 5. 상수(enum) 용도

기존 숫자 기반 상수는 중복 위험이 존재하기 때문에 심벌을 쓰면 유일성을 보장할 수 있다.

```jsx
const Direction = Object.freeze({
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
})
```

# 7. 프로퍼티 은닉

`for...in`, `Object.keys`, `Object.getOwnPropertyNames`로는 **보이지 않는다.**
`Object.getOwnPropertySymbols()` 로 접근이 가능하기 때문에 프로퍼티 은닉이 가능하다!

```jsx
const secret = Symbol('secret')
const obj = { [secret]: 'hidden' }

console.log(Object.getOwnPropertySymbols(obj)) // [Symbol(secret)]
```

# 8. Well-known Symbol

JS가 내부적으로 사용하는 빌트인 심벌들은 아래와 같다.

| **심벌**             | **용도 예시**                          |
| -------------------- | -------------------------------------- |
| `Symbol.iterator`    | for…of 순회 가능하게                   |
| `Symbol.toPrimitive` | 객체 → 원시값 변환                     |
| `Symbol.toStringTag` | Object.prototype.toString 결과 지정    |
| `Symbol.species`     | 파생 클래스 인스턴스 생성 방식 제어    |
| `Symbol.match`       | 정규표현식 관련 메서드 커스터마이징 등 |
