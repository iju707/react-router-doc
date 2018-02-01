# `<StaticRouter>`

경로가 절대 바뀌지 않는 [`<Router>`](/api/router.md) 입니다.

이것은 사용자의 클릭이 발생하지 않아 경로의 실제변경이 없는 서버측면의 렌더링 시나리오에 유용합니다. 따라서 이름이 Static 입니다. 또한 원하는 경로를 추가하여 렌더링 결과를 검증하는 간단한 테스트를 수행하는데 유용합니다.

아래는 [`<Redirect>`](/api/redirect.md)에 대해서는 302 상태코드를, 다른 요청은 일반적인 HTML을 전달하는 간단한 노드 서버입니다.

```jsx
import { createServer } from 'http'
import React from 'react'
import ReactDOMServer from 'react-dom/server'
import { StaticRouter } from 'react-router'

createServer((req, res) => {

  // This context object contains the results of the render
  const context = {}

  const html = ReactDOMServer.renderToString(
    <StaticRouter location={req.url} context={context}>
      <App/>
    </StaticRouter>
  )

  // context.url will contain the URL to redirect to if a <Redirect> was used
  if (context.url) {
    res.writeHead(302, {
      Location: context.url
    })
    res.end()
  } else {
    res.write(html)
    res.end()
  }
}).listen(3000)
```

## basename : string

모든 경로의 기본 URL 입니다. 이 속성은 슬래시(/)로 시작하며, 마지막은 제외합니다.

```jsx
<StaticRouter basename="/calendar">
  <Link to="/today"/> // renders <a href="/calendar/today">
</StaticRouter>
```

## location : string

서버가 전달받은 URL 입니다. 노드서버에서는 `req.url`이 될 것 입니다.

```jsx
<StaticRouter location={req.url}>
  <App/>
</StaticRouter>
```

## location : object

`{ pathname, search, hash, state }`구조를 가진 location 객체입니다.

```jsx
<StaticRouter location={{ pathname: '/bubblegum' }}>
  <App/>
</StaticRouter>
```

## context : object

Javascript 객체입니다. 렌더링할 때 컴포넌트가 필요한 정보를 저장하기 위한 객체에 속성을 추가할 수 있습니다.

```jsx
const context = {}
<StaticRouter context={context}>
  <App />
</StaticRouter>
```

`<Route>`가 매칭하면, `staticContext`라는 이름으로 컴포넌트의 렌더러에게 Context 객체를 전달합니다. 이것을 어떻게 사용하는지 자세한 내용은 [서버 렌더링](/guide/server_rendering.md)를 참고하시면 됩니다.

이 속성들은 렌러딩이 끝나고 서버의 응답을 구성할 때 사용됩니다.

```jsx
if(context.status === '404') {
  // ...
}
```

## children : node

렌더링할 [단일 노드](https://reactjs.org/docs/react-api.html#react.children.only) 입니다.