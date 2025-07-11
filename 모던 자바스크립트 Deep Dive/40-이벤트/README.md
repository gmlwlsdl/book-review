# 1. 이벤트 드리븐 프로그래밍

> "이벤트는 사용자의 행동이나 시스템에서 발생하는 모든 사건에 대한 신호다."
>
> **사용자의 모든 움직임(클릭, 스크롤 등) + 브라우저의 상태 변화(로딩, 리사이즈 등) = "이벤트"**

- **이벤트(Event)** <br />
  웹 페이지에서 일어나는 모든 사건 (클릭, 키 입력, 로딩 등)

- **이벤트 리스너(Event Listener)** <br />
  "이벤트 일어나나?" 귀 기울여 듣고 있는 것

- **이벤트 핸들러(Event Handler)** <br />
  "이벤트 발생!" 소식을 듣고 실제로 동작하는 코드(함수)

> 🤓 **결국 이벤트 기반 프로그래밍은?**
>
> 이벤트가 발생하면 → 그걸 듣고 → 정해진 코드를 실행! 하는 방식으로 프로그램이 흘러가는 것!

# 2. 이벤트 타입

이벤트는 종류가 엄청 많다!

- **마우스 이벤트**: `click`, `mouseover` 등
- **키보드 이벤트**: `keydown`, `keyup` 등
- **포커스 이벤트**: `focus`, `blur`
- **폼 이벤트**: `submit`, `reset`
- **값 변경 이벤트**: `change`, `input`
- **뷰 이벤트**: `resize`, `scroll`
- **리소스 이벤트**: `load`, `error`, `DOMContentLoaded`

## 3. 이벤트 핸들러 등록

이벤트가 발생했을 때 호출될 함수(핸들러)를 등록하는 방법을 알아보자

## 3-1. 이벤트 핸들러 어트리뷰트 방식

HTML 태그에 `on<이벤트타입>` 형태의 어트리뷰트로 핸들러를 직접 등록하는 옛날 방식이다.

```html
<button onclick="console.log('버튼 클릭!')">눌러봐!</button>
```

> **😱 비추천하는 이유**
>
> > HTML과 JavaScript 코드가 뒤섞여서 유지보수가 매우 어렵다.
> >
> > 이벤트 핸들러의 `this`는 전역 객체(`window`)를 가리키는 등 동작 방식이 직관적이지 않다.
> >
> > 하나의 이벤트에 하나의 핸들러만 등록할 수 있다.

## 3-2. 이벤트 핸들러 프로퍼티 방식

DOM 요소 객체의 프로퍼티(`onclick` 등)에 핸들러 함수를 할당하는 방식이다.

```javascript
const btn = document.querySelector('button')

btn.onclick = function () {
  console.log('버튼 클릭!')
}

// 새로운 핸들러를 할당하면?
btn.onclick = function () {
  console.log('이전 핸들러는 덮어쓰여서 사라짐 ㅠ')
}
```

> **🤔 한계점**
>
> > 어트리뷰트 방식보다는 낫지만, 여전히 **하나의 이벤트에 하나의 핸들러만** 등록할 수 있다는 단점이 있다.

## 3-3. addEventListener 메서드 방식

`EventTarget.prototype.addEventListener` 메서드를 사용하는 것이 가장 권장되는 현대적인 방식이다.

```javascript
const btn = document.querySelector('button')

function handler1() {
  console.log('첫 번째 핸들러! 실행!')
}

function handler2() {
  console.log('두 번째 핸들러도 실행!')
}

btn.addEventListener('click', handler1)
btn.addEventListener('click', handler2)
```

> **🤓 추천하는 이유**
>
> > **여러 개의 핸들러**를 등록할 수 있다. (위 예제처럼 `handler1`, `handler2` 둘 다 실행됨)
> >
> > `removeEventListener`로 등록한 핸들러를 깔끔하게 제거할 수 있다.
> >
> > 이벤트 전파 단계(캡처링/버블링)를 선택하는 등 더 많은 제어 옵션을 제공한다.
> >
> > 코드를 깔끔하게 분리하여 유지보수하기 좋다.

# 4. 이벤트 핸들러 제거

`removeEventListener`로 등록한 이벤트를 제거한다.

등록할 때와 동일한 함수 참조가 필요하다.

# 5. 이벤트 객체

이벤트 정보를 담은 객체로 핸들러의 첫 인자로 전달된다.

## 5-1. 이벤트 객체의 상속 구조

`Event` ← `UIEvent` ← `MouseEvent` / `KeyboardEvent` 처럼 상속받아, 부모의 프로퍼티를 모두 사용 가능하다.

## 5-2. 이벤트 객체의 공통 프로퍼티

`target`, `currentTarget`, `type`, `preventDefault()`, `stopPropagation()` 등

# 6. 이벤트 전파

이벤트는 DOM 트리를 따라 흐른다. (캡처링: 내려감 → 버블링: 올라감)

# 7. 이벤트 위임 (Event Delegation)

부모 요소에 이벤트 리스너를 하나만 달아서 여러 자식 요소의 이벤트를 한 번에 처리하는 똑똑한 패턴임

# 8. DOM 요소의 기본 동작 조작

## 8-1. 기본 동작 중단

- `event.preventDefault()`: `<a>` 태그 이동, `submit` 전송 등 기본 동작을 막는다.

## 8-2. 이벤트 전파 방지

- `event.stopPropagation()`: 이벤트가 부모 요소로 전파(버블링)되는 것을 막는다.

# 9. 이벤트 핸들러 내부의 this

- **일반 함수 (`function`)** 핸들러: `this` === `event.currentTarget` (이벤트 붙은 요소)
- **화살표 함수 (`=>`)** 핸들러: `this` === 상위 스코프의 `this`

# 10. 이벤트 핸들러에 인수 전달

`addEventListener`에 등록하는 핸들러는 기본적으로 `event` 객체만 받는다.

이때 추가 인수를 전달하려면?

**✨ 해결책: 클로저를 활용한 함수를 사용!**

```javascript
function sayHi(name) {
  // 이 함수가 진짜 핸들러 역할을 한다.
  return function (event) {
    console.log(`Hi, ${name}!`)
    console.log('이벤트 타입:', event.type)
  }
}

// addEventListener에는 함수를 실행하는 코드를 넣는다.
myButton.addEventListener('click', sayHi('Hazel'))
```

👉🏻 `sayHi('Hazel')`이 먼저 실행되면서 `name` 변수가 렉시컬 환경에 저장되고, `event`를 인자로 받는 내부 함수(클로저)가 반환되어 핸들러로 등록된다.

# 11. 커스텀 이벤트

우리가 직접 이벤트를 만들어서 사용할 수도 있다!

## 11-1. 커스텀 이벤트 생성

`new CustomEvent()` 생성자로 새로운 이벤트를 만든다.

```javascript
// 'my-custom-event' 라는 이름의 커스텀 이벤트를 생성
const myEvent = new CustomEvent('my-custom-event', {
  detail: { message: '이벤트 데이터 뿅!✨' }, // 전달하고 싶은 데이터
})
```

## 11-2. 커스텀 이벤트 디스패치(발생시키기)

`dispatchEvent` 메서드로 원하는 요소에서 이벤트를 강제로 발생시킬 수 있다.

```javascript
// 1. 커스텀 이벤트를 들을 리스너 등록
document.addEventListener('my-custom-event', function (event) {
  console.log('커스텀 이벤트 받음!')
  console.log(event.detail.message) // -> "이벤트 데이터 뿅!✨"
})

// 2. document에서 커스텀 이벤트를 발생시킴!
document.dispatchEvent(myEvent)
```

> 🤓 **언제 쓸까?**
>
> 프레임워크나 라이브러리 없이 컴포넌트 간에 데이터를 주고받거나, 특정 상태 변경을 다른 곳에 알리고 싶을 때 유용하게 사용할 수 있다.
