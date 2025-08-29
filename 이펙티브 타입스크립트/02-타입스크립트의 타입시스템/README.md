> 타입 시스템은 무엇인가? 어떻게 사용해야 하는가, 무엇을 결정해야 하는가, 사용하지 말아야 하는 기능은 무엇일까?

# Item 06. 편집기를 사용하여 타입 시스템 탐색하기

타임스크립트를 설치하면 다음의 두 가지를 실행할 수 있다.

1. 타입스크립트 컴파일러 (tsc)
2. 단독으로 실행할 수 있는 타입스크립트 서버 (tsserver)

이 외에도 타입스크립트는 언어 서비스를 제공하는데, 이 서비스는 코드 자동 완성, 명세(spection) 검사, 검색, 리팩터링 등을 포함한다.

이 서비스를 사용하면 읽기 쉬운 코드를 작성할 수 있다.

예를 들어, 타입스크립트는 다음과 같이 함수의 **타입을 추론**할 수 있다. _이는 추후 나오는 타입 넓히기(Item 21), 좁히기(Item 22)와 관련이 있다._
![편집기를 통한 함수 타입 추론](image.png)

---

타입 추론은 디버깅에도 용이하다. <br />
예를 들어, 다음과 같이 메서드 체인이 있다.

> 메서드 체인이란?
> 여러 메서드가 연속적으로 호출되는 구조

```ts
function restOfPath(path: string) {
  return path.split('/').slice(1).join('/')
}
```

![타입 추론과 메서드 체인 설명](image-1.png)

타입스크립트의 타입 추론을 통해 `path.split('/')`은 string[]을 반환하고, slice는 배열의 일부를 잘라내는 새로운 배열을 반활하는 것을 알 수 있다. 현재는 간단한 메서드 체인이지만, 메서드 체인이 길어지고 복잡해질수록 중간 단계에서 타입이 어떻게 변화하는지 확인할 수 있다.

---

언어 서비스는 라이브러리와 라이브러리의 타입 선언을 탐색할 때도 도움이 된다. 만약 특정 함수에 대해 더 자세히 알고싶다면 편집기의 `Go to Definition` 옵션을 이용하면 된다. (cmd + 마우스 좌클릭)

![Go to Definition](image-2.png)

1. `lib.dom.d.ts`에 `fetch`는 다음과 같이 선언되어 있다.
   ![fetch 함수 선언](image-3.png)

   fetch 함수의 이름, 매개변수들과 반환하는 타입이 명시되어 있다.

2. `fetch`의 첫 번째 매개변수인 `RequestInfo`는 다음과 같이 선언되어 있다.
   ![RequestInfo 타입 선언](image-4.png)

   `RequestInfo`는 `Request` 또는 `string` 타입 중 하나가 될 수 있는 유니온 타입이다.

3. Request의 타입 선언은 다음과 같이 선언되어 있다.
   ![Request 타입 선언](image-5.png)

   여기서 `Request` 타입과 값은 분리되어 있다.

   **line 19822**의 `var Request`는 **`Request`가 값으로 존재**한다는 것을 선언한다. <br />
   **line 19823**의 `prototype: Request`는 **`Request` 생성자가 만들어내는 인스턴스들의 프로톼입**을 가리킨다. <br />
   **line 19824**의 `new(input: RequestInfo, init?: RequestInit): Request;`는 `Request` 생성자가 **`RequestInfo`와 R`equestInit`을 받아 `Request` 타입의 객체를 반환**한다는 것을 명시한다.

> **`type`, `interface`, `declare의` 차이에 대하여**
>
> > 1.  `interface`
> >
> >     객체의 모양(구조)를 정의하는 데 사용된다. <br />
> >     특정 객체가 어떤 속성과 메서드를 가져야 하는지 명시하는 '설계도'이며, 병합이 가능하다. <br />
> >     동일한 이름의 interface를 여러 번 선언하면, 타입스크립트가 이들을 하나로 합쳐준다.
> >
> >     ```ts
> >     interface Person {
> >       name: string
> >     }
> >
> >     interface Person {
> >       age: number
> >     }
> >     // 두 interface가 하나로 병합되어 { name: string; age: number; }가 됨
> >     const user: Person = { name: 'Hazel', age: 90 }
> >     ```
> >
> > 2.  `type`
> >
> >     새로운 타입의 이름을 만드는 데 사용된다. (interface보다 복잡한 타입을 정의할 때 사용한다.) <br />
> >     객체 모양뿐만 아니라 원시 타입, 유니온(`|`), 인터섹션(`&`), 튜플 등 다양한 타입을 정의할 수 있다. <br />
> >     별칭을 만드는 데 유용하고, 동일한 이름으로 여러 번 선언하면 오류가 발생한다.
> >
> >     ```ts
> >     type ID = string | number
> >     type User = {
> >       name: string
> >       id: ID
> >     }
> >     // type은 병합되지 않음
> >     ```
> >
> > 3.  `declare`
> >
> >     타입스크립트가 아닌 위부에서 온 코드의 타입 정보를 선언할 때 사용된다. <br />
> >     해당 변수가 프로젝트의 어딘가에 존재하므로, 타입스크립트가 검사할 때 오류를 발생하지 않도록 알려주는 역할을 한다.
> >
> >     [🔎 참고 - [Typescript] declare](https://html-jc.tistory.com/604)
