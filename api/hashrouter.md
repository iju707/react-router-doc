# `<HashRouter>`

URL과 UI간의 동기화 유지를 위하여 URL의 해시값(예, `window.location.hash`)을 사용하는[`<Router>`](/api/router.md)입니다.

**중요** : Hash history는 `location.key`와 `location.state`를 제공하지 않습니다. 이전 버전에는 해당 기능을 구현하기 위해 시도를 했지만 해결하지 못했습니다. 어떠한 코드나 플러그인을 사용해도 해당기능을 재현하지 못했습니다. `HashRouter`는 이전버전의 브라우저를 지원하기위한 용도로만 사용을 하시고, `<BrowserRouter>`를 사용해서 서버를 구성하도록 권장드립니다.

```jsx
import { HashRouter } from 'react-router-dom'

<HashRouter>
  <App/>
</HashRouter>
```

