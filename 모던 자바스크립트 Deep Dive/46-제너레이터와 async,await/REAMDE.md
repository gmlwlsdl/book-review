# 1. Generator란

코드 블록의 실행을 일시 중단했다가 필요한 시점에 재개할 수 있는 특수한 함수이다.

> 🤓 **제너레이터 vs 일반 함수**
>
> > | **구분**               | **일반 함수**          | **제너레이터 함수**                    |
> > | ---------------------- | ---------------------- | -------------------------------------- |
> > | 실행 방식              | 한 번에 끝까지 실행됨  | 멈췄다, 다시 실행 가능                 |
> > | 실행 제어              | 함수가 제어권을 가짐   | 호출자가 제어권을 갖고 조절            |
> > | 외부와 데이터 주고받기 | 매개변수 → 결과만 반환 | 실행 중에도 값을 주고받을 수 있음      |
> > | 반환값                 | 일반 값                | Generator 객체 (이터러블 + 이터레이터) |

# 2. Generator 함수 정의

`function*` 키워드로 선언하고,

하나 이상의 `yield` 표현식을 포함한다.

> `yield`: “양도” 라는 뜻으로, 함수 실행을 일시 중지하고 외부에서 재개할 수 있는 특별한 동작을 수행한다.

```jsx
// 제너레이터 함수 선언문
function* generatorFunc() {
  yield 'A'
  yield 'B'
}

// 제너레이터 함수 표현식
const gen = function* () {
  yield 1
  yield 2
}

// 제너레이터 메서드
const obj = {
  *iterator() {
    yield 'x'
    yield 'y'
  },
}

// 클래스의 제너레이터 메서드
class MyCollection {
  *[Symbol.iterator]() {
    // 반복 가능한 이터러블 객체
    yield 1
    yield 2
  }

  *customGen() {
    yield '🍎'
    yield '🍌'
  }
}
```

> 🤓 [🔗 제너레이터 함수는 화살표 함수로 정의할 수 없으며,](https://www.notion.so/46-async-await-23210e12f8ca80bfbc66e0bcc8179989?pvs=21)
>
> new 연산자와 함께 생성자 함수로 호출할 수 없다.

> 🤓 **왜 제너레이터는 화살표 함수로 정의할 수 없지?**
>
> > 1.  화살표 함수는 자신만의 실행 컨텍스트를 만들지 않고 외부 스코프의 this를 그대로 캡쳐해서 쓰는데, 제너레이터는 자신만의 실행 컨텍스트, 자신만의 상태를 가져야 하기 때문이다.
> > 2.  화살표 함수에서는 `yield` 키워드가 동작하지 않는다.

# 3. Generator 객체

제너레이터 객체는 **이터러블(iterable)**이면서 동시에 **이터레이터(iterator)**이다.

그래서 제너레이터 객체를 `next` 메서드로 호출하면 `yield` 표현식까지 코드 블럭을 실행하고,

`yield` 값을 `value` 프로퍼티 값으로 빼고

함수가 끝까지 실행되었는지 나타내는 `done` 프로퍼티 값까지 함께 반환한다. **(이터레이터 리절트 객체)**

# 4. Generator 일시 중지와 재개

위에서 적은 것처럼 `yield`까지만 코드 블럭이 실행되고 **일시 중지(suspend)**되는데

이때 함수의 제어권이 호출자로 **양도(yield)**되는 것이다.

다시 `next`로 제너레이터를 실행할 때 `next` 메서드에 인수를 전달할 수 있다.

이때 `next` 메서드에 전달한 인수는 `yield` 표현식을 할당받는 변수에 할당된다.

```jsx
function* gen() {
  const x = yield '🥳' // '마지막 스터디'는 x에 할당되는 것!!
  console.log('x:', x)
}

const g = gen()
console.log(g.next()) // { value: '🥳', done: false }
console.log(g.next('마지막 스터디')) // x: 마지막 스터디
```

> 🤓 **“yield 표현식을 할당받는 변수에 yield 표현식의 평가 결과가 할당되지 않는다.”** 는 무슨 뜻이나면
>
> > ```jsx
> > const x = yield '🥳';
> > ```
> >
> > 여기서 `x`는 '🥳'가 아니라, **다음번에 호출한 `next(value)`의 `value`**를 받는다는 뜻이다!

> 🤓 제너레이터와 비동기 처리?? 비동기 처리를 동기 처리처럼??

# 5. Generator 활용

## 5-1. 이터러블 구현

이터러블 프로토콜을 준수하며 이터러블을 생성하는 방식보다 간단하게 이터러블을 구현할 수 있다!

## 5-2. 비동기 처리

`next` 메서드와 `yield` 표현식을 통해 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.

> 🤓 **비동기 처리를 동기 흐름처럼 만드는 이유는?**
>
> > 코드의 가독성과 유지보수성을 향상시키기 위해서

```jsx
// 실행 컨트롤러

function run(generatorFunc) {
  const gen = generatorFunc()

  function handle(result) {
    if (result.done) return

    const promise = result.value
    promise.then((res) => {
      handle(gen.next(res)) // 다음 next 호출하며 값을 넘김
    })
  }

  handle(gen.next()) // 최초 시작
}
```

```jsx
// 제너레이터

function* fetchData() {
  // 1. fetch 끝날 때까지 멈춤
  const response = yield fetch('https://jsonplaceholder.typicode.com/posts/1')

  // 2. json 파싱 끝날 때까지 멈춤
  const data = yield response.json()

  // 3. 모든 데이터 완료 후 실행
  console.log('최종 결과:', data)
}

run(fetchData)
```

# 6. `async` / `await`

프로미스를 기반으로 동작하며,

후속 처리 메서드 없이 마치 동기 처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

```jsx
async function fetchData() {
  try {
    // 1. fetch 끝날 때까지 대기
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1')

    // 2. json 파싱 끝날 때까지 대기
    const data = await response.json()

    // 3. 모든 데이터 완료 후 실행
    console.log('최종 결과:', data)
  } catch (error) {
    console.error('에러 발생:', error)
  }
}

fetchData()
```

`async`는 암묵적으로 반환 값을 `resolve` 하는 프로미스를 반환한다.

`await`은 프로미스가 `settled` 상태가 될 때까지 대기하다가 `settled` 상태가 되면 프로미스가 `resolve` 한 처리 결과를 반환한다.

## 6-1. 에러 처리

에러는 호출자 방향으로 전파되어서

이전에는 비동기 함수가 `try…catch` 문에서 잡히지 못했는데 (콜백 함수가 실행될 때는 이미 호출자가 콜 스택에서 제외되었기 때문에)

`async/await`를 쓰면서 잡을 수 있게 되었다.
