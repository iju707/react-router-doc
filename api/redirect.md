# `<Redirect>`

`<Redirect>`를 렌더링할 경우 새로운 경로로 이동하게 됩니다. 새로운 경로는 history 스택의 현재 경로를 바꾸게 되며, 서버측면의 리다이렉트(HTTP 3xx)가 하는것과 유사합니다.

```jsx
import { Route, Redirect } from 'react-router'

<Route exact path="/" render={() => (
  loggedIn ? (
    <Redirect to="/dashboard"/>
  ) : (
    <PublicHomePage/>
  )
)}/>
```

## to : string

리다이렉트하고자 하는 URL

```jsx
<Redirect to="/somewhere/else"/>
```

## to : object

리다이렉트하고자 하는 location 객체

```jsx
<Redirect to={{
  pathname: '/login',
  search: '?utm=your+face',
  state: { referrer: currentLocation }
}}/>
```

## push : boolean

만약 `true` 값이면 리다이렉트가 발생될 때 현재 위치를 바꾸지 않고 새롭게 추가합니다.

```jsx
<Redirect push to="/somewhere/else"/>
```

## from : string

리다이렉션이 수행되는 경로이름입니다. `<Switch>`안에서 `<Redirect>`를 렌더링할 때 경로 매칭에 사용되는 항목입니다. 자세한 내용은 [`<Switch children>`](/api/switch.md#children-node)를 참고하시기 바랍니다.

```jsx
<Switch>
  <Redirect from='/old-path' to='/new-path'/>
  <Route path='/new-path' component={Place}/>
</Switch>
```