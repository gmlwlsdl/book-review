# 1. 브라우저 렌더링 과정에 대해

웹 브라우저가 HTML, CSS, 자바스크립트 코드를 화면에 시각적으로 표현하는 일련의 과정을 렌더링이라고 한다.

단계는 다음과 같다.

1.  **리소스 로딩**
    - 브라우저는 서버에 HTML 파일을 요청하고 응답을 받는다.
    - **HTML을 파싱**하며 **`<link> 태그`의 CSS 파일**, **`<script> 태그`의 JS 파일** 등 추가 리소스를 요청하고 응답을 기다린다.
2.  **파싱 (Parsing)**

    - HTML 파싱 및 DOM(Document Object Model) 생성
      - HTML 코드를 읽어서
      - 토큰으로 분해하고
      - 이 토큰을 노드로 변환하여
      - 트리 형태의 자료구조인 DOM 트리로 만든다. 🎄
    - CSS 파싱 및 CSSOM(CSS Object Model) 생성

      - CSS는 렌더링 차단 리소스 (render-blocking resource)이므로, CSSOM이 완료되어야 렌더링이 시작된다.

      > > 🤓 **렌더링 차단 리소스가 뭐냠**
      > >
      > > 브라우저가 화면을 그리기 전에 반드시 로딩하고 파싱해야만 하는 리소스로,
      > >
      > > 페이지가 사용자에게 보이기까지 시간 (FCP, LCP 등)이 지연될 수 있다.
      > >
      > > CSS, 동기 JS (`<link>`, `<script>`) 등이 있다.
      > >
      > > > **그럼 해결할 수 있는 방법은?**
      > > >
      > > > 1. **지연 로딩**
      > > >
      > > > ```html
      > > > <!-- 👿 문제 코드 -->
      > > > <!-- HTML 파싱 중단하고 CSS 먼저 로드 -->
      > > > <link rel="stylesheet" href="style.css" />
      > > > ```
      > > >
      > > > ```html
      > > > <!-- 👍🏻 해결 코드 -->
      > > > <!-- CSS를 미리 다운로드해두고, onload 시점에만 적용 -->
      > > > <link
      > > >   rel="preload"
      > > >   href="style.css"
      > > >   as="style"
      > > >   onload="this.onload=null;this.rel='stylesheet'"
      > > > />
      > > > <noscript><link rel="stylesheet" href="style.css" /></noscript>
      > > > ```
      > > >
      > > > `preload`를 이요해서 CSS를 미리 받아두되, 렌더링 차단은 하지 않는다.
      > > >
      > > > `onload`에서 `rel='stylesheet'`로 바꾸면 **그때부터 적용한다는 뜻이고**
      > > >
      > > > `<noscript>`는 JS 미지원 브라우저에 대응하는 태그이다.
      > > >
      > > > 2. **필요한 CSS만 인라인 로딩**
      > > >
      > > > 화면에 보이는 영역만 직접 HTML에 `<style>`을 삽입해서 렌더링을 빠르게 한다.
      > > >
      > > > ```html
      > > > <head>
      > > >   <style>
      > > >     /* 위 fold 영역에만 필요한 CSS */
      > > >     body {
      > > >       margin: 0;
      > > >       font-family: sans-serif;
      > > >     }
      > > >     .hero {
      > > >       background: skyblue;
      > > >       height: 100vh;
      > > >     }
      > > >   </style>
      > > >   <link rel="stylesheet" href="style.css" />
      > > > </head>
      > > > ```
      > > >
      > > > 3. **defer, async 사용**
      > > >
      > > > ```html
      > > > <!-- 👿 문제 코드 -->
      > > > <!-- HTML 파싱 중단하고 JS 먼저 다운로드 & 실행 -->
      > > > <script src="main.js"></script>
      > > > ```
      > > >
      > > > ```html
      > > > <!-- 👍🏻 해결 코드 -->
      > > > <!-- HTML 파싱 끝난 후 실행 (권장) -->
      > > > <script src="main.js" defer></script>
      > > >
      > > > <!-- 다운로드는 병렬, 실행은 준비되는 즉시 (주의 필요) -->
      > > > <script src="main.js" async></script>
      > > > ```
      > > >
      > > > 그래서 직접 실험을 해봤다!
      > > >
      > > > [🔎 간단하게 렌더링 최적화 실험해보기](https://github.com/gmlwlsdl/study-js/blob/main/38-%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%20%EB%A0%8C%EB%8D%94%EB%A7%81%20%EA%B3%BC%EC%A0%95/Additional.md)

3.  **렌더 트리 생성 (Render Tree Construction)**

    DOM 트리와 CSSOM 트리가 결합되어 렌더 트리를 형성한다.

    렌더 트리는 실제로 화면에 렌더링될 요소들만 포함한다.

    > 🤓 `display: none;` vs `visibility: hidden;`
    >
    > > `display: none;` 속성이 적용된 요소는 렌더 트리에 포함되지 않지만,
    > >
    > > `visibility: hidden;`은 포함된다!

4.  **레이아웃 (Layout / Reflow)**
    - 렌더 트리가 생성되면, 각 객체(노드)의 정확한 위치와 크기를 계산하는 레이아웃(리플로우) 과정이 시작된다.
      뷰포트 내에서 요소들이 차지할 공간을 결정하고,
      박스 모델 (width, height, padding, margin, border 등)을 기반으로 정확한 좌표를 계산한다.
    - 리플로우는 DOM 구조나 스타일 속성 (`width`, `height`, `margin`, `padding`, `border`, `display`, `position` 등) 이 변경될 때 다시 발생할 수 있다. 근데 비용 많이 듬 💸
5.  **페인팅 (Painting / Repaint)**

    - 레이아웃 단계에서 계산된 위치와 크기를 바탕으로, 렌더 트리의 각 노드를 실제 픽셀로 변환하여 화면에 그린다.
    - 페인팅은 레이아웃 변경 없이 스타일 (`color`, `background-color`, `box-shadow`, `visibility`)만 변경될 때 발생할 수 있다. 레이아웃보다 비용은 적게 들지만, 잦은 페인팅도 성능 저하를 야기할 수 있다.

      > 🤓 **왜 페인팅은 리소스가 더 적게 들지?**
      >
      > > 위치와 레이아웃은 이미 계산이 끝나있고, 거기에 어떻게 그릴건지만 계산하면 되니까!

      > 🤓 **레이아웃 변경 + 스타일 변경시에는 어떻게 동작하ㅈㅣ?**
      >
      > > 레이아웃 변경이 발생하면 무조건 Reflow → Paint까지 일어난다.

6.  **컴포지팅 (Compositing)**
    1. 그려진 여러 레이어를 합성하여 최종적으로 화면에 표시한다.
    2. `transform`이나 `opacity` 속성 변경은 레이아웃이나 페인팅을 유발하지 않고, 컴포지팅 단계에서만 처리될 수 있어서 성능에 유리하다.
