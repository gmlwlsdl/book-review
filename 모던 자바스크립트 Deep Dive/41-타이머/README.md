# 1. 호출 스케줄링

함수를 명시적으로 호출하면 즉시 실행되지만, **호출 스케줄링**을 사용하면 일정 시간이 경과된 후에 함수를 호출하도록 예약할 수 있다.

이를 위해 자바스크립트는 타이머 함수인 `setTimeout`과 `setInterval`을 제공한다.

이 함수들은 ECMAScript 사양에 정의된 빌트인 함수는 아니지만, 브라우저와 Node.js 환경에서 모두 전역 객체의 메서드(호스트 객체)로서 제공된다.

# 2. 타이머 함수

`setTimeout`과 `setInterval`은 비동기 처리 방식으로 동작한다.

## 2-1. `setTimeout` / `clearTimeout`

- **`setTimeout(callback, delay, [param1, param2, ...])`**

  `delay` 시간(ms) 후에 `callback` 함수를 **단 한 번** 실행하는 타이머를 생성한다.

  `delay`를 생략하면 기본값 0이 지정된다. (단, 4ms 이하의 값은 4ms로 지정될 수 있다.)

  생성된 타이머를 식별할 수 있는 고유한 **타이머 ID**를 반환한다.

  콜백 함수에 인수를 전달하려면 `delay` 뒤에 인수를 추가한다.

- **`clearTimeout(timerId)`**

  `setTimeout`으로 생성된 타이머를 취소한다.

  `timerId`는 `setTimeout`이 반환한 타이머 ID이다.

```javascript
// 1초 후에 'Hi!'를 출력하는 타이머 생성
const timeoutId = setTimeout(() => console.log('Hi!'), 1000)

// 타이머 취소
clearTimeout(timeoutId)

// 1초 후에 'Lee'에게 인사하는 타이머 생성 (인수 전달)
setTimeout((name) => console.log(`Hi! ${name}.`), 1000, 'Lee')
```

## 2-2. `setInterval` / `clearInterval`

- **`setInterval(callback, delay, [param1, param2, ...])`**

  `delay` 시간이 경과할 때마다 `callback` 함수가 **반복적으로** 실행되는 타이머를 생성한다.

  `clearInterval`을 사용하여 중지하기 전까지 계속 반복된다.

  고유한 **타이머 ID**를 반환한다.

- **`clearInterval(timerId)`**

  `setInterval`로 생성된 반복 실행 타이머를 취소한다.

```javascript
let count = 1

// 1초마다 숫자를 출력하는 타이머 생성
const intervalId = setInterval(() => {
  console.log(count)
  if (count++ === 5) {
    // count가 5가 되면 타이머를 취소한다.
    clearInterval(intervalId)
  }
}, 1000)
```

# 3. 디바운스(Debounce)와 스로틀(Throttle)

`scroll`, `resize`, `input`, `mousemove`처럼 짧은 시간 간격으로 연속해서 발생하는 이벤트를 처리할 때, 과도한 이벤트 핸들러 호출을 방지하기 위한 프로그래밍 기법이다.

## 3-1. 디바운스 (Debounce)

짧은 시간 간격으로 이벤트가 연속해서 발생하면, 이전 타이머를 취소하고 새로운 타이머를 설정한다.

결과적으로, 특정 시간이 경과한 후에 **마지막 이벤트에 대한 핸들러만 한 번 호출**되도록 한다.

- **용도**: `input` 요소의 자동완성 UI, 버튼 중복 클릭 방지 등

```javascript
const debounce = (callback, delay) => {
  let timerId
  return (event) => {
    // 이전 타이머가 있다면 취소하고 새로운 타이머를 설정한다.
    if (timerId) clearTimeout(timerId)
    timerId = setTimeout(callback, delay, event)
  }
}
```

## 3-2. 스로틀 (Throttle)

짧은 시간 간격으로 이벤트가 연속해서 발생하더라도, 일정 시간 간격으로 **이벤트 핸들러가 최대 한 번만 호출**되도록 조절한다.

- **용도**: `scroll` 이벤트 처리, 무한 스크롤 UI 구현 등

```javascript
const throttle = (callback, delay) => {
  let timerId
  return (event) => {
    // 타이머가 설정되어 있다면 아무것도 하지 않는다.
    if (timerId) return
    timerId = setTimeout(
      () => {
        callback(event)
        timerId = null // 타이머가 만료되면 null로 초기화
      },
      delay,
      event
    )
  }
}
```
