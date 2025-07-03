# 1. 디스트럭처링 할당이란?

배열이나 객체에서 값을 꺼내 변수에 쉽게 할당할 수 있게 해주는 문버비다.

# 2. 배열 디스트럭처링 할당

**기본 문법**

```jsx
const [a, b] = [1, 2]
console.log(a, b) // 1 2
```

**스킵 및 기본값 설정**

```jsx
const [x, , y = 5] = [10]
console.log(x, y) // 10 5
```

**Rest 요소 사용**

```jsx
const [head, ...rest] = [1, 2, 3, 4]
console.log(head) // 1
console.log(rest) // [2, 3, 4]
```

# 3. 객체 디스트럭처링 할당

**기본 문법**

```jsx
const { name, age } = { name: 'Hazel', age: 22 }
```

**변수명 변경**

```jsx
const { name: userName } = { name: 'Hazel' }
console.log(userName) // 'Hazel'
```

**기본 값 설정**

```jsx
const { role = 'guest' } = {}
console.log(role) // 'guest'
```

**중첩 구조 추출**

```jsx
const user = {
  address: {
    city: 'Geonggi',
  },
}
const {
  address: { city },
} = user
console.log(city) // 'Geonggi'
```

**Rest 프로퍼티**

```jsx
const { x, ...others } = { x: 1, y: 2, z: 3 }
console.log(others) // { y: 2, z: 3 }
```

**함수 매개변수 활용**

```jsx
function greet({ name, age }) {
  console.log(`Hi ${name}, you are ${age}`)
}
greet({ name: 'Tom', age: 30 })
```

> 🤓 **함께 알아두면 좋은 정보들@!**
>
> > 1.  **이터러블(배열, 문자열, Set 등)은 배열 디스트럭처링 대상이다.**
> >
> >     👉🏻 배열 디스트럭처링은 순서를 기반으로 값을 꺼내기 때문에, 반복이 가능한 값만 대상으로 사용할 수 있다!
> >
> > 2.  **객체는 프로퍼티 키 기준으로 수행된다. 순서는 상관 없다.**
> >
> >     👉🏻 배열은 순서 기준이지만, 객체는 프로퍼티 키 이름을 기준으로 값을 꺼낸다는 뜻!
> >
> > 3.  **const로 선언 후 나중에 디스트럭처링 할당은 불가능하다.**
> >
> >     👉🏻 const는 한 번 초기화하면 값을 바꿀 수 없는 변수 선언 방식이기 때문에, 나중에 디스트럭처링 하려면 초기화도 같이 해야 함!
