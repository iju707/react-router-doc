# `<BrowserRouter>`

HTML5의 history API(__pushState__, __replaceState__, __popState__)를 활용하여 URL과 화면을 동기화하는 [<Router>](/api/router.md) 종류 중 하나 입니다.

```jsx
import { BrowserRouter } from 'react-router-dom'

<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App/>
</BrowserRouter>
```

## basename : string

모든 location 정보에 대한 기본 URL 입니다. 만약 어플리케이션이 하위경로에서 실행되는 구조라면, 이 정보를 하위경로에 맞게 설정하시면 됩니다. 이 속성은 슬래시(/)로 시작하며, 마지막은 제외합니다.

```jsx
<BrowserRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="/calendar/today">
```

## getUserConfirmation : func

경로탐색 확인 사용을 위한 함수입니다. 기본적으로는 [window.confirm](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)를 사용합니다.

```jsx
// this is the default behavior
const getConfirmation = (message, callback) => {
  const allowTransition = window.confirm(message)
  callback(allowTransition)
}

<BrowserRouter getUserConfirmation={getConfirmation}/>
```

## forceRefresh : bool

만약 값이 `true`이면 라우터는 해당 경로에 대하여 모든 페이지를 갱신할 것 입니다. 이 기능은 아마 [브라우저가 HTML5 history API를 지원하지 않을 경우](https://caniuse.com/#feat=history) 사용할 것 입니다.

```jsx
const supportsHistory = 'pushState' in window.history
<BrowserRouter forceRefresh={!supportsHistory}/>
```

## keyLength : number

`location.key`의 길이입니다. 기본값은 6 입니다.

```jsx
<BrowserRouter keyLength={12}/>
```

## children : node

렌더링할 [단일 노드](https://reactjs.org/docs/react-api.html#react.children.only) 입니다.