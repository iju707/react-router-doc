# `<MemoryRouter>`

URL의 이력을 메모리에 보관(주소창을 읽거나 쓰지 않음)하는 [`<Router>`](/api/router.md)입니다. 테스트할때나 [React Native](https://facebook.github.io/react-native/)와 같은 브라우저가 아닌 환경에서 유용합니다.

```jsx
import { MemoryRouter } from 'react-router'

<MemoryRouter>
  <App/>
</MemoryRouter>
```

## initialEntries : array

history 스택에 있는 `location`의 초기 배열입니다. `{ pathname, search, hash, state }`와 같은 `location` 객체 또는 단순 URL 문자열이 들어갑니다.

```jsx
<MemoryRouter
  initialEntries={[ '/one', '/two', { pathname: '/three' } ]}
  initialIndex={1}
>
  <App/>
</MemoryRouter>
```

## initialIndex : number

`initialEntries` 배열의 초기 인덱스 번호입니다.

## getUserConfirmation : function

탐색의 확인을 할 때 사용되는 함수입니다. `<MemoryRouter>`와 `<Prompt>`를 함께 사용하고자 하는 경우에는 이 옵션을 사용하시면 됩니다.

## keyLength : number

`location.key`의 길이입니다. 기본값은 6 입니다.

```jsx
<MemoryRouter keyLength={12}/>
```

## children : node

렌더링할 [단일 노드](https://reactjs.org/docs/react-api.html#react.children.only) 입니다.