# 37장 - Set과 Map

주차: 7주차, ModernJS

# 1. Set이란?

**중복되지 않는 유일한 값들의 집합**을 저장할 수 있는 객체이다.

👉🏻 ”집합”과 동일한 개념이다.

> 🤓 **배열이랑 머가 다른가?**
>
> > | **특징**      | **배열(Array)** | **Set** |
> > | ------------- | --------------- | ------- |
> > | 중복 허용     | O               | X       |
> > | 순서 보장     | O               | X       |
> > | 인덱스로 접근 | O               | X       |

## 1-1. Set 주요 메서드

생성자 인자로 이터러블을 받을 수 있다.

```jsx
const set = new Set() // 빈 Set
const set2 = new Set([1, 2, 3, 3]) // 중복 제거됨 → Set(3) {1, 2, 3}
```

**배열 중복을 제거**할 땐 다음과 같이 활용할 수 있다.

```jsx
const arr = [1, 2, 2, 3, 4, 4]
const uniqueArr = [...new Set(arr)]
console.log(uniqueArr) // [1, 2, 3, 4]
```

**요소 개수를 확인하기**

```jsx
const set = new Set([1, 2, 3])
console.log(set.size) // 3
```

이때 size는 getter 전용 프로퍼티이므로, 값 할당은 무시해도 딘다!

**요소 추가하기**

```jsx
const set = new Set()
set.add(1).add(2) // method chaining 가능
```

중복 추가는 무시된다.

이 외의 주요 메서드는 `has`, `delete` 등이 있다.

> 🤓 **Set에서 +0과 -0은 동일한 값으로 간주된다!**

# 2. Map

Map은 키와 값의 쌍으로 이루어진 **컬렉션 객체**로

객체와 비슷하지만 더 다양한 **키 타입**과 **이터러블 지원** 등의 기능을 가진다.

> 🤓 **객체 vs Map**
>
> > | **구분**               | **객체**                  | **Map**                   |
> > | ---------------------- | ------------------------- | ------------------------- |
> > | 키로 사용할 수 있는 값 | 문자열, 심벌              | 모든 값 (객체 포함)       |
> > | 이터러블 여부          | X                         | O (for...of, spread 가능) |
> > | 요소 개수 확인         | `Object.keys(obj).length` | `map.size`                |

## 2-1. Map 주요 메서드

**Map 생성하기**

```jsx
const map = new Map() // 빈 Map 생성
const map2 = new Map([
  ['a', 1],
  ['b', 2],
])
```

> 🤓 중첩 배열의 첫 번째 요소는 key, 두 번째 요소는 value로 해석된다.
>
> 중복된 키는 마지막 값으로 덮어쓴다.

**요소 개수 확인하기**

```jsx
const map = new Map([
  ['a', 1],
  ['b', 2],
])
console.log(map.size) // 2
```

**요소 추가하기**

```jsx
const map = new Map()
map.set('a', 1).set('b', 2) // 체이닝 가능
```

**요소 접근하기**

```jsx
const map = new Map()
map.set('a', 1)
console.log(map.get('a')) // 1
console.log(map.get('b')) // undefined
```

이 외에도 `.has()`, `.delete()`, `.clear()` 등의 주요 메서드가 있다.

> 🤓 Map은 객체를 키로 쓸 수 있어서 캐시나 메타데이터 저장 등에 유용하다.
