# `<Route>`

Route는 React Router를 이해하고 잘 사용하는 법을 배우기 위한 가장 중요한 컴포넌트 입니다. [`location`](/api/location.md)이 Route의 `path`와 일치할 때 UI를 렌더링하는 것이 기본 동작입니다.

다음 코드를 보겠습니다.

```jsx
import { BrowserRouter as Router, Route } from 'react-router-dom'

<Router>
  <div>
    <Route exact path="/" component={Home}/>
    <Route path="/news" component={NewsFeed}/>
  </div>
</Router>
```

만약 경로가 _/_이면 UI는 다음과 같이 표출될 것 입니다.

```jsx
<div>
  <Home/>
  <!-- react-empty: 2 -->
</div>
```

경로가 _/news_로 변경되면 다음과 같이 표출될 것 입니다.

```jsx
<div>
  <!-- react-empty: 1 -->
  <NewsFeed/>
</div>
```

_react-empty_주석부분은 React의 _null_ 렌더링이라고 보시면 됩니다. Route는 기술적으로는 _null_이더라도 항상 렌더링을 합니다. 어플리케이션의 경로가 Route의 _path_와 일치하면 그때는 컴포넌트를 렌더링하게 됩니다.

## Route의 렌더링 방법 {#route-render-method}

`<Route>`에서는 3가지의 방식으로 렌더링을 제공합니다.

* [`<Route component>`](#component)
* [`<Route render>`](#render)
* [`<Route children>`](#children)

이것은 각각의 상황에 따라 유용하게 사용할 수 있습니다. `<Route>`에는 동시에 1가지만 사용을 해야합니다. 왜 3가지의 선택사항이 있는지는 아래에 자세히 설명하겠습니다. 아마 React Router에서 가장 많이 사용하는 컴포넌트가 될 것 입니다.

## Route 속성 {#route-props}

3가지의 [렌더링 방법](#route-render-method)에는 3가지 동일한 속성이 전달됩니다.

* [match](/api/match.md)
* [location](/api/location.md)
* [history](/api/history.md)

## component {#component}

경로가 일치할 때 렌더링할 React 컴포넌트 입니다. [Route 속성](#route-props)을 포함하여 렌더링됩니다.

```jsx
<Route path="/user/:username" component={User}/>

const User = ({ match }) => {
  return <h1>Hello {match.params.username}!</h1>
}
```

(`render`나 `children`이 아닌)`component`를 사용할 때 주어진 컴포넌트를 가지고 [React element](https://facebook.github.io/react/docs/rendering-elements.html)를 만들기 위해 라우터는 [`React.createElement`](https://facebook.github.io/react/docs/react-api.html#createelement)를 사용합니다. 이 의미는 `component`속성으로 인라인 함수를 전달하면 렌더링할 때 마다 새로운 컴포넌트를 생성하게 됩니다. 이것은 기존의 컴포넌트를 갱신하지 않고 언마운트를 한 뒤 새로운 컴포넌트를 마운트하게 합니다. 만약 렌더링을 위해 인라인함수를 사용하는 경우에는 `render`나 `children` 속성을 활용하시면 됩니다.

## render : function {#render}

위에서 설명한 원치않는 반복되는 마운트 없이 인라인 렌더링이나 래핑을 위한 것 입니다.

[component](#component) 속성에서 새로운 [React element](https://facebook.github.io/react/docs/rendering-elements.html)를 만드는것과 다르게 경로가 매핑이 되었을 때 호출될 함수를 전달할 수 있습니다. `component` 속성과 동일하게 `render` 속성도 [Route 속성](#route-props)을 가지고 호출됩니다.

```jsx
// convenient inline rendering
<Route path="/home" render={() => <div>Home</div>}/>

// wrapping/composing
const FadingRoute = ({ component: Component, ...rest }) => (
  <Route {...rest} render={props => (
    <FadeIn>
      <Component {...props}/>
    </FadeIn>
  )}/>
)

<FadingRoute path="/cool" component={Something}/>
```

> **경고** : `<Route component>`가 `<Route render>` 상위로 수행되기 때문에, 동일한 `<Route>`에서 같이 사용하지 마시기 바랍니다.

## children : function {#children}

## path : string {#path}

[path-to-regexp](https://www.npmjs.com/package/path-to-regexp) 모듈이 인식할 URL 경로입니다.

```jsx
<Route path="/users/:id" component={User}/>
```

`path`가 없는 `Route`는 어느 경로이든 매칭됩니다.

## exact : boolean {#exact}

`true`이면, `location.pathname` 항목과 정확하게 일치할 때만 매칭됩니다.

```jsx
<Route exact path="/one" component={About}/>
```

| path | location.pathname | exact | matches? |
| :--- | :--- | :--- | :--- |
| /one | /one/two | ture | no |
| /one | /one/two | false | yes |

## strict : boolean {#strict}

## location : object {#location}



