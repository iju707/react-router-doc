# `<HashRouter>`

URL과 UI간의 동기화 유지를 위하여 URL의 해시값(예, `window.location.hash`)을 사용하는[`<Router>`](/api/router.md)입니다.

**중요** : Hash history는 `location.key`와 `location.state`를 제공하지 않습니다. 이전 버전에는 해당 기능을 구현하기 위해 시도를 했지만 해결하지 못했습니다. 어떠한 코드나 플러그인을 사용해도 해당기능을 재현하지 못했습니다. `HashRouter`는 이전버전의 브라우저를 지원하기위한 용도로만 사용을 하시고, `<BrowserRouter>`를 사용해서 서버를 구성하도록 권장드립니다.

```jsx
import { HashRouter } from 'react-router-dom'

<HashRouter>
  <App/>
</HashRouter>
```

## basename: string

모든 location 정보에 대한 기본 URL 입니다. 이 속성은 슬래시(/)로 시작하며, 마지막은 제외합니다.

```jsx
<HashRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="#/calendar/today">
```

## getUserConfirmation: function

경로탐색 확인 사용을 위한 함수입니다. 기본적으로는 [window.confirm](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)를 사용합니다.

```jsx
// this is the default behavior
const getConfirmation = (message, callback) => {
  const allowTransition = window.confirm(message)
  callback(allowTransition)
}

<HashRouter getUserConfirmation={getConfirmation}/>
```

## hashType: string

`window.location.hash`에서 사용될 인코딩타입정보입니다. 가능한 것은 다음과 같습니다.

* `slash` - 해시를 다음과 같이 생성합니다. `#/` 또는 `#/sunshine/lollipops`
* `noslash` - 해시를 다음과 같이 생성합니다. `#` 또는 `#sunshine/lollipops`
* `hashbang` - 해시를 [ajax crawlable](https://developers.google.com/webmasters/ajax-crawling/docs/learn-more)(Google에서 더이상 지원안함)방식으로 생성합니다. `#!/` 또는 `#!/sunshine/lollipops`

기본값은 `slash` 입니다.

## children : node

렌더링할 [단일 노드](https://reactjs.org/docs/react-api.html#react.children.only) 입니다.