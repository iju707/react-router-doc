# 서버 렌더링

서버에서 렌더링하는 것은 상태가 없기 때문에 조금 다릅니다. 기본적으로 어플리케이션을 __<BrowserRouter>__가 아닌 상태가 없는 __<StaticRouter>__로 감쌉니다. 그리고 서버에 요청된 url과 다음에 이야기할 context 속성을 추가합니다.

```jsx
// client
<BrowserRouter>
  <App/>
</BrowserRouter>

// server (not the complete story)
<StaticRouter
  location={req.url}
  context={context}
>
  <App/>
</StaticRouter>
```

클라이언트에서 __<Redirect>__를 렌더링할 때 브라우저의 히스토리를 변경하고 새로운 화면을 가져옵니다. 정적 서버 환경에서는 어플리케이션의 상태를 바꿀 수 없습니다. 대신 __context__ 속성을 활용해서 렌더링한 결과가 무엇이었는지 확인을 합니다. __context.url__에서 어플리케이션이 어디로 리다이렉트할지 확인합니다. 이것을 통해 서버에서 적정한 리다이렉션처리를 할 수 있습니다.

```jsx
const context = {}
const markup = ReactDOMServer.renderToString(
  <StaticRouter
    location={req.url}
    context={context}
  >
    <App/>
  </StaticRouter>
)

if (context.url) {
  // Somewhere a `<Redirect>` was rendered
  redirect(301, context.url)
} else {
  // we're good, send the response
}
```

## 어플리케이션별 context 정보추가

라우터는 __context.url__만 추가할 수 있습니다. 그렇지만 아마 301 또는 301 상태에 대해서도 리다이렉트가 필요할 것 입니다. 또는 특정 부분을 렌더링할 때 404 오류를, 권한이 없다면 401 오류를 발생시키고자 할 것 입니다. __context__ 속성은 제어가 가능하므로 변형시켜서 사용하시면 됩니다. 아래 301과 302에 대한 리다이렉트를 처리하는 예제를 보겠습니다.

```jsx
const RedirectWithStatus = ({ from, to, status }) => (
  <Route render={({ staticContext }) => {
    // there is no `staticContext` on the client, so
    // we need to guard against that here
    if (staticContext)
      staticContext.status = status
    return <Redirect from={from} to={to}/>
  }}/>
)

// somewhere in your app
const App = () => (
  <Switch>
    {/* some other routes */}
    <RedirectWithStatus
      status={301}
      from="/users"
      to="/profiles"
    />
    <RedirectWithStatus
      status={302}
      from="/courses"
      to="/dashboard"
    />
  </Switch>
)

// on the server
const context = {}

const markup = ReactDOMServer.renderToString(
  <StaticRouter context={context}>
    <App/>
  </StaticRouter>
)

if (context.url) {
  // can use the `context.status` that
  // we added in RedirectWithStatus
  redirect(context.status, context.url)
}
```

## 404, 401 또는 다른 상태

이전의 예제와 비슷하게 처리하시면 됩니다. context를 추가한 컴포넌트를 만든 뒤 다른 상태처리를 위한 곳에 사용하시면 됩니다.

```jsx
const Status = ({ code, children }) => (
  <Route render={({ staticContext }) => {
    if (staticContext)
      staticContext.status = code
    return children
  }}/>
)
```

__staticContext__에 추가할 코드를 사용하여 어플리케이션 어디든 __Status__ 컴포넌트를 사용하여 렌더링 하시면 됩니다.

```jsx
const NotFound = () => (
  <Status code={404}>
    <div>
      <h1>Sorry, can’t find that.</h1>
    </div>
  </Status>
)

// somewhere else
<Switch>
  <Route path="/about" component={About}/>
  <Route path="/dashboard" component={Dashboard}/>
  <Route component={NotFound}/>
</Switch>
```