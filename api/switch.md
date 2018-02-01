# `<Switch>`

경로에 일치하는 첫번째 [`<Route>`](/api/route.md) 또는 [`<Redirect>`](/api/redirect.md)를 렌더링합니다.

**`<Route>`를 사용할 때와 차이점이 무엇입니까?**

`<Switch>`는 유일하게 라우트를 독점적으로 렌더링할 수 있습니다. 그와 반대로, `<Route>`는 경로가 일치하면 모든 라우트를 렌더링하게 됩니다. 코드를 살펴보겠습니다.

```jsx
<Route path="/about" component={About}/>
<Route path="/:user" component={User}/>
<Route component={NoMatch}/>
```

만약 경로가 _/about_이 오면, 경로의 일치여부에 따라 `<About>`, `<User>`, `<NoMatch>` 모두 렌더링 될 것 입니다. 이것은 다양한 방법으로 어플리케이션에 `<Route>`의 조합을 가능하게 하여 사이드바, breadcrumb, bootstrap tab 등을 디자인할 수 있게 합니다.

하지만 때때로 렌더링할 `<Route>`를 딱 한개만 선택하고자 할 경우도 있습니다. 만약 _/about_ 경로에 접근할 때 _/:user_에 매칭되지 않도록(또는 "404" 페이지를 표출하지 않도록) 하고싶다면 `Switch`를 사용하시면 됩니다.

```jsx
import { Switch, Route } from 'react-router'

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```

_/about_ 경로에 오면, `<Switch>`는 매칭되는 `<Route>`를 찾을 것 입니다. `<Route path="/about" />`이 처음으로 매칭되면 `<Switch>`는 더이상 매칭되는 것을 찾지 않으며 `<About>`을 렌더링할 것 입니다. 이와 비슷하게 _/michael_ 경로에 들어가면 `<User>`가 렌더링 될 것 입니다.

이것은 직전에 매칭된 것과 동일한 위치에 매칭된 `<Route>`를 렌더링하여 애니메이션 전환을 할 때 유용합니다.

```jsx
<Fade>
  <Switch>
    {/* there will only ever be one child here */}
    <Route/>
    <Route/>
  </Switch>
</Fade>

<Fade>
  <Route/>
  <Route/>
  {/* there will always be two children here,
      one might render null though, making transitions
      a bit more cumbersome to work out */}
</Fade>
```

## Switch 속성

### location : object

현재 history location(보통 브라우저의 URL)이 아닌 [`location`](/api/location.md) 객체로 자식요소를 매칭할 때 사용합니다.

### children : node

`<Switch>`의 모든 자식노드는 `<Route>`나 `<Redirect>`로 구성됩니다. 그 중 현재 경로와 첫번째로 매칭되는 자식노드가 렌더링 될 것 입니다.

`<Route>`는 `path` 속성을, `<Redirect>`는 `from` 속성을 활용하여 매칭을 처리합니다. 만약 `<Route>`가 `path` 속성이 없거나 `<Redirect>`가 `from` 속성이 없는 경우에는 항상 현재 경로와 일치한다고 판단합니다.

`<Switch>`에 `<Redirect>`를 사용할 때, `<Route>`의 경로 매칭 속성(`path`, `exact`, `strict`)를 사용할 수 있습니다. `from`은 `path` 속성의 별칭입니다.

`<Switch>`에 `location` 속성이 주어지면, 자식노드의 매칭처리할 때 `location` 속성으로 사용됩니다.

```jsx
<Switch>
  <Route exact path="/" component={Home}/>

  <Route path="/users" component={Users}/>
  <Redirect from="/accounts" to="/users"/>

  <Route component={NoMatch}/>
</Switch>
```