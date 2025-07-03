# 1. 이터러블이란?

순회 가능한 객체를 의미한다.

ES6에서는 **이터레이션 프로토콜** 이라는 약속을 정의해서, 모든 이터러블이 공통된 방식으로 순회할 수 있도록 만들었다.

`for…of`, 스프레드 문법, 배열 디스트럭처링, `Promise.all`, `Map, Set` 등에서 활용된다.

> 🤓 **이터레이션 프로토콜?**
>
> > JS 엔진과 객체 간 순회 규칙을 통일하기 위한 표준 인터페이스이다.
> >
> > 크게 두 가지 규약이 있다.
> >
> > 1.  **이터러블 프로토콜 (Iterable Protocol)**
> >
> >     이 객체는 순회할 수 있어요! 라고 알려주는 규칙이다.
> >
> >     객체에 `Symbol.iterator` 라는 특수한 메서드가 있다면, 이 객체는 이터러블이다.
> >
> >     따라서 `obj[Symbol.iterator]()` 를 호출하면 이터레이터 객체를 반환해야 한다.
> >
> >     ```jsx
> >     const arr = [1, 2, 3]
> >     const iterator = arr[Symbol.iterator]() // arr는 이터러블이다
> >     ```
> >
> > 2.  **이터레이터 프로토콜 (Iterator Protocol)**
> >
> >     순회하면서 다음 값을 알려줄게요! 라고 작동하는 규칙이다.
> >
> >     `next()` 메서드를 가지고 있어야 하며
> >
> >     `next()` 를 호출하면 `{ value: 값, done: boolean }` 형식의 객체를 반환한다.
> >
> > 👉🏻 어떤 자료구조이든, 공통된 방식으로 순회할 수 있다!

> 🤓 부록 정보 📑
>
> > 1.  일반 객체 {}는 기본적으로 이터러블이 아니다.
> > 2.  Array.from(이터러블) 처럼 이터러블을 배열로 변환할 수 있다.

# 2. JS의 빌트인 이터러블

빌트인 이터러블이란, JS 엔진이 기본으로 제공하면서 이터레이션 프로토콜을 따르는 객체를 의미한다!

| **객체 종류**            | **이터레이터 메서드**                                                            |
| ------------------------ | -------------------------------------------------------------------------------- |
| Array                    | `Array.prototype[Symbol.iterator]`                                               |
| String                   | `String.prototype[Symbol.iterator]`                                              |
| Map                      | `Map.prototype[Symbol.iterator]`                                                 |
| Set                      | `Set.prototype[Symbol.iterator]`                                                 |
| TypedArray               | `TypedArray.prototype[Symbol.iterator]`                                          |
| arguments                | `arguments[Symbol.iterator]`                                                     |
| NodeList, HTMLCollection | `NodeList.prototype[Symbol.iterator], HTMLCollection.prototype[Symbol.iterator]` |

# 3. for…of 문

`for…of` 문은 내부적으로 `iterable[Symbol.iterator]()`를 호출해서 이터레이터를 생성하고

반복문마다 `.next()`를 호출해서 `{value, done} 객체`를 반환한다.

`done`이 `true`가 되면 반복을 종료한다.

```jsx
for (const item of iterable) {
  console.log(item)
}
```

> 🤓 `for…in` 과의 차이는?
>
> > - `for…in`은 객체의 키(열거 가능한 프로퍼티)를 순회하는 거고
> > - `for…of`는 이터러블 값을 순회한다.

# 4. 유사 배열 객체 vs 이터러블

| **항목**        | **유사 배열 객체** | **이터러블** |
| --------------- | ------------------ | ------------ |
| 인덱스 접근     | O                  | O            |
| length 있음     | O                  | O 또는 X     |
| for 루프 사용   | O                  | O            |
| for...of 사용   | X                  | O            |
| Symbol.iterator | 없음               | 있음         |

# 5. 이터레이션 프로토콜의 필요성

😱 ES6 이전에는 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등 순회 가능한 데이터를 각자 다른 방식으로 순회해야 했다.

> 🤓 ES6에선 이터레티션 프로토콜이 도입되면서, 다양한 데이터를 같은 방식으로 순회할 수 있게 되었다!

따라서 이터레이션 프로토콜은 데이터 소비자와 데이터 공급자를 연결하는 인터페이스 역할을 한다.

![image.png](34%E1%84%8C%E1%85%A1%E1%86%BC%20-%20%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%A5%E1%84%87%E1%85%B3%E1%86%AF%2021c10e12f8ca800c9915fc53b8af7726/image.png)

# 6. 사용자 정의 이터러블

아래의 기본 구조만 맞추면 어떤 객체든 이터러블로 만들 수 있다.

```jsx
const myIterable = {
  [Symbol.iterator]() {
    return {
      next() {
        return { value: ??, done: ?? };
      }
    };
  }
};
```

피보나치 수열 이터러블을 맹글어보면,,

```jsx
const fibonacci = {
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1]
    const max = 10

    return {
      next() {
        ;[pre, cur] = [cur, pre + cur]
        return {
          value: cur,
          done: cur >= max,
        }
      },
    }
  },
}

for (const num of fibonacci) {
  console.log(num) // 1 2 3 5 8
}
```

`Symbol.iterator`는 이터레이터를 반환하고

`next()`를 호출할 때마다 피보나치 수를 계산해서 반환한다.

`done`에서 `true`가 되면 순회를 종료하게 된다.

이터러블이면서 이터레이터인 객체도 있다.

`for...of`도 되고 `next()`도 직접 호출 가능하다.

```jsx
const fibonacciFunc = (max) => {
  let [pre, cur] = [0, 1]

  return {
    [Symbol.iterator]() {
      return this // this가 곧 이터레이터
    },
    next() {
      ;[pre, cur] = [cur, pre + cur]
      return {
        value: cur,
        done: cur >= max,
      }
    },
  }
}

const iter = fibonacciFunc(10)
console.log(iter.next()) // { value: 1, done: false }
console.log(iter.next()) // { value: 2, done: false }

for (const n of iter) {
  console.log(n) // 3 5 8
}
```

> 🤓 **이터러블 vs 이터레이터**
>
> > | **구분**                  | **설명**                | **핵심 메서드**     | **예**                                           |
> > | ------------------------- | ----------------------- | ------------------- | ------------------------------------------------ |
> > | **이터러블 (Iterable)**   | 반복 가능한 객체        | `Symbol.iterator()` | Array, Set, 사용자 정의 이터러블 등              |
> > | **이터레이터 (Iterator)** | 실제 값을 꺼내는 반복기 | `next()`            | iterator.next() 호출 시마다 { value, done } 반환 |

무한 이터러블에 지연 평가도 맹글 수 있다.

done을 생략하면 for...of는 끝없이 순회하므로 break로 조건을 줘야 한다.

```jsx
const infiniteFibonacci = () => {
  let [pre, cur] = [0, 1]

  return {
    [Symbol.iterator]() {
      return this
    },
    next() {
      ;[pre, cur] = [cur, pre + cur]
      return { value: cur } // done 생략
    },
  }
}

for (const n of infiniteFibonacci()) {
  if (n > 10000) break
  console.log(n)
}
```

> 🤓 **지연 평가란?**
>
> > 데이터를 필요할 때 생성하는 방식이다.
> >
> > 배열처럼 미리 만들어두는 게 아니라, next() 호출 시점에 생성하게 된다.
> >
> > 요 방식으로 하면 메모리 효율이 좋고 무한 이터러블 구현이 가능하게 된다.

> 🤓 **무한 이터러블이 뭔디**
>
> > done: true 없이 끝없이 값이 나오는 이터러블이다.
> >
> > 무한 수열, 스트림 처리 등에서 유용하다.
