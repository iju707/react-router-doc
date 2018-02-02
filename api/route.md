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

## Route 속성

3가지의 [렌더링 방법](#route-render-method)에는 3가지 동일한 속성이 전달됩니다.

* [match](/api/match.md)
* [location](/api/location.md)
* [history](/api/history.md)

## component

