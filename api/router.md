# `<Router>`

모든 라우터 컴포넌트의 기반이 되는 공통 인터페이스 입니다. 일반적으로 어플리케이션에서는 이것을 직접사용하진 않고 상위 구현된 라우터를 사용합니다.

* [`<BrowserRouter>`](/api/browserrouter.md)
* [`<HashRouter>`](/api/hashrouter.md)
* [`<MemoryRouter>`](/api/memoryrouter.md)
* [`<NativeRouter>`](https://reacttraining.com/native/api/NativeRouter)
* [`<StaticRouter>`](/api/staticrouter.md)

`<Router>`를 직접 사용하는 일반적인 경우는 Redux나 Mobx와 같은 라이브러리를 사용해서 상태관리를 통한 history 동기화를 할 때 입니다. React Router를 사용함에 있어서 상태관리 라이브러리를 꼭 이용해야될 필요는 없습니다. 좀더 깊게 통합하고자 할 때 필요합니다.

```jsx
import { Router } from 'react-router'
import createBrowserHistory from 'history/createBrowserHistory'

const history = createBrowserHistory()

<Router history={history}>
  <App/>
</Router>
```

## history : object

탐색에 사용될 [history](https://github.com/ReactTraining/history) 객체입니다.

```jsx
import createBrowserHistory from 'history/createBrowserHistory'

const customHistory = createBrowserHistory()
<Router history={customHistory}/>
```

## children : node

렌더링할 [단일 노드](https://reactjs.org/docs/react-api.html#react.children.only) 입니다.

```jsx
<Router>
  <App/>
</Router>
```