# 1. DOM의 개념

DOM(Document Object Model)은 HTML, XML, SVG와 같은 문서를 위한 프로그래밍 인터페이스이다.

웹 페이지를 스크틸븥나 프로그래밍 언어에서 쉽게 접근하고 조작할 수 있도록 문서를 트리 형태의 모델로 표현한다.

DOM은 단순히 문서의 구조를 나타내는 것을 넘어,

문서의 요소, 스타일, 컨텐츠를 조작할 수 있는 API를 제공한다!

# 2. Node

DOM은 노드 객체들로 구성된 트리 자료구조이다.

각 노드 객체는 문서의 일부를 나타내며, 고유한 타입과 프로퍼티, 메서드를 가진다

## 2-1. Node type

1. **Document Node (문서 노드)**

   DOM 트리 최상단에 존재하며 `document` 객체를 가리킨다.

   `document.documentElement`는 `html` 요소를, `document.body`는 `body` 요소를, `document.head`는 `head` 요소를 가리킨다.

2. **Element Node (요소 노드)**

   HTML 요소를 나타낸다. (`<div>`, `<p>`, `<a>`)

   DOM 트리의 구조를 형성하며, 다른 요소 노드나 텍스트 노드의 부모가 될 수 있다.

   어트리뷰트를 조작하고, 자식 노드를 추가/삭제 하는 등 다양한 DOM 조작 메서드를 제공한다.

3. **Attribute Node (어트리뷰트 노드)**

   HTML 요소의 어트리뷰트를 나타낸다. (`id="foo"`, `class="bar"` 등)

   요소 노드에만 연결되며, DOM 트리의 일부가 아니므로 부모, 자식, 형제 노드를 가지지 않는다.

   요소 노드의 `attributes` 프로퍼티를 통해 접근할 수 있다.

4. **Text Node (텍스트 노드)**

   HTML 요소의 텍스트 컨텐츠를 나타낸다.

   DOM 트리의 최종단 노드이며, 자식 노드를 가질 수 없음!

5. **Comment Node (주석 노드)**

   HTML 주석을 나타낸다.

# 3. DOM 요소 조작

JS에서 DOM을 조작한다는 것은 주로 요소 노드를 다루는 것을 의미한다.

## 3-1. Element Selection (요소 취득)

DOM 조작을 위해서는 조작하고자 하는 요소 노드를 먼저 취득해야 한다.

- `document.getElementById(id)`
  `id` 어트리뷰트 값을 사용하여 요소를 취득하며 가장 빠르다.
- `document.getElementsByClassName(classNames)`
  `class` 어트리뷰트 값을 사용하여 요소들을 취득한다.
  `HTMLCollection`을 반환한다.
- `document.getElementsByTagName(tagName)`
  태그 이름을 사용하여 요소들을 취득한다.
  `HTMLCollection`을 반환한다.
- `document.querySelector(selector)`
  CSS 셀렉터를 사용하여 첫 번째 일치하는 요소를 취득한다.
- `document.querySelectorAll(selector)`
  CSS 셀렉터를 사용하여 모든 일치하는 요소들을 취득한다.
  `NodeList`를 반환한다.

`HTMLCollection`과 `NodeList`는 **유사 배열 객체(Array-like object)** 이며, `forEach` 메서드만 지원하고 `map`, `filter` 등의 배열 메서드는 직접 사용할 수 없다.

스프레드 문법이나 `Array.from`을 사용하여 배열로 변환 후 사용하는 것이 일반적!

## 3-2. Element Manipulation (요소 조작)

취득한 요소 노드를 기반으로 다양한 조작을 수행할 수 있다!

- **HTML 콘텐츠 조작**
  - `element.innerHTML`
    요소의 모든 자식 요소를 포함한 HTML 마크업을 문자열로 가져오거나 설정한다.
    보안 문제(XSS)와 Reflow/Repaint 발생 가능성이 있어 주의해야 한다.
  - `element.textContent`
    요소의 텍스트 콘텐츠만 가져오거나 설정한다.
    HTML 태그가 포함되어도 텍스트로 처리한다.
- **어트리뷰트 조작**
  - `element.getAttribute(attributeName)`
    특정 어트리뷰트의 값을 가져온다.
  - `element.setAttribute(attributeName, value)`
    특정 어트리뷰트의 값을 설정한다.
  - `element.removeAttribute(attributeName)`
    특정 어트리뷰트를 제거한다.
  - `element.hasAttribute(attributeName)`
    특정 어트리뷰트가 존재하는지 확인한다.
  - `element.className`, `element.id`
    `class`나 `id`와 같은 표준 어트리뷰트는 프로퍼티로 직접 접근할 수 있다.
  - `element.classList`
    `class` 어트리뷰트의 값을 `DOMTokenList` 객체로 반환하며, `add`, `remove`, `toggle`, `contains` 등의 메서드를 통해 클래스를 편리하게 조작할 수 있다.
- **스타일 조작**
  - `element.style.propertyName`
    인라인 스타일을 직접 설정하거나 가져온다.
    CSS 프로퍼티는 카멜 케이스로 작성한다! (ex) `backgroundColor`)
  - `window.getComputedStyle(element[, pseudoElt])`
    요소에 실제로 적용된 모든 스타일(인라인, 내부, 외부 스타일시트)을 포함하는 `CSSStyleDeclaration` 객체를 반환한다.
- **DOM 구조 조작 (노드 생성/추가/삭제)**
  - `document.createElement(tagName)`
    새로운 **요소 노드**를 생성한다.
  - `document.createTextNode(text)`
    새로운 **텍스트 노드**를 생성한다.
  - `parent.appendChild(child)`
    부모 노드의 자식 목록 중 마지막 자식으로 노드를 추가한다.
  - `parent.insertBefore(newNode, referenceNode)` `referenceNode` 앞에 `newNode`를 추가한다.
  - `parent.removeChild(child)`
    부모 노드의 특정 자식 노드를 제거한다.
  - `element.remove()`
    자기 자신을 DOM에서 제거한다.
  - `Node.prototype.cloneNode([deep])`
    노드를 복제한다, `deep`이 `true`이면 모든 자식 노드까지 깊게 복제한다.

## 3-3. 이벤트 핸들링

DOM은 요소에 이벤트 핸들러를 등록하여 사용자 상호작용에 응답할 수 있도록 한다.

- `element.addEventListener(type, listener[, options])`
  특정 이벤트(ex. `click`, `mouseover`, `keydown`)가 발생했을 때 실행될 함수(`listener`)를 등록한다.
- `element.removeEventListener(type, listener[, options])`
  등록된 이벤트 핸들러를 제거한다.

# 4. DOM과 성능

빈번한 DOM 조작은 브라우저의 Reflow와 Repaint를 유발하여 웹 애플리케이션의 성능 저하로 이어질 수 있다.

- **Reflow (레이아웃) 유발**
  요소의 크기, 위치, 기하학적 속성 (ex. `width`, `height`, `margin`, `padding`, `border`, `display`, `position`) 변경, DOM 노드 추가/삭제, 폰트 크기 변경, 윈도우 리사이즈 등이 Reflow를 유발한다.
- **Repaint (페인팅) 유발**
  Reflow 없이 색상, 배경색, `visibility` 등 시각적인 스타일만 변경될 때 Repaint가 발생한다.

그래서 이런 성능 저하를 예방하는 방법으로는

- **DOM 조작 최소화**
  `DocumentFragment`를 사용하여 여러 DOM 변경을 한 번에 적용하거나, 가상 DOM을 사용하는 라이브러리/프레임워크를 활용한다.
  아래는 `DocumentFragment` 예시 코드!

  ```jsx
  const ul = document.querySelector('ul')
  const fragment = document.createDocumentFragment()

  for (let i = 0; i < 100; i++) {
    const li = document.createElement('li')
    li.textContent = `Item ${i}`
    fragment.appendChild(li) // DOM에 추가 안 되고 fragment에 모임
  }

  ul.appendChild(fragment) // 한 번에 DOM 추가 = 성능 GOOD!
  ```

- **CSS `transform` 및 `opacity` 활용**
  애니메이션에 `transform`이나 `opacity`와 같이 컴포지팅 단계에서만 처리되는 속성을 사용하여 Reflow/Repaint를 방지하고 GPU 가속을 활용한다.

  ```jsx
  /* 안 좋은 예: left 변경은 레이아웃에 영향 */
  .box {
    position: absolute;
    left: 100px;
  }

  /* 좋은 예: transform은 GPU 처리 */
  .box {
    transform: translateX(100px);
  }
  ```

- **`will-change` 속성**
  브라우저에 요소의 변경 가능성을 미리 알려주어 최적화를 돕는다.
  ```jsx
  .card {
    will-change: transform;
  }
  ```
  브라우저한테 해당 요소가 곧 바뀔 거라고 미리 알려서, 미리 GPU 가속 레이어 생성해두는 것임
  근데 너무 남용하면 메모리 낭비가 크니까, 진짜 자주 바뀌는 요소에만 쓰는 게 좋다!
- **읽기/쓰기 분리**
  Reflow를 강제하는 읽기 작업을 쓰기 작업과 분리하여`requestAnimationFrame` 등으로 스케줄링한다.
  읽고 쓰는 작업이 섞이면, 브라우저가 강제로 계산을 다시 하게 된다.
  요고를 Layout Trashing이라고 하고!
  이렇게 하는 것보다
  ```jsx
  for (let i = 0; i < 100; i++) {
    const height = el.offsetHeight // 읽기
    el.style.height = height + 10 + 'px' // 쓰기
  }
  ```
  이렇게 하는 게 좋다.
  ```jsx
  const height = el.offsetHeight // 한 번만 읽기
  requestAnimationFrame(() => {
    el.style.height = height + 10 + 'px' // 쓰기 분리
  })
  ```
- **CSS Containment**
  `contain` 속성을 사용하여 특정 요소 내부의 변경이 전체 문서에 미치는 영향을 제한한다.
  ```jsx
  .sidebar {
    contain: layout style;
  }
  ```
  .sidebar 내부에서 무슨 일이 있어도, 전체 문서에 영향을 주지 않는다 굿굿
