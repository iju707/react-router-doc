# withRouter

`witRouter`를 통해서 [history](/api/history.md) 객체의 속성과 컴포넌트 우선순위중 가장 근접한 [`<Route>`](/api/route.md)의 [match](/api/match.md)를 접근할 수 있습니다. `withRouter`는 route가 변경될 때 마다 [`<Route>`](/api/route.md)와 동일한 속성 `{ match, location, history }`을 가지고 컴포넌트를 다시 렌더링합니다.

```js
import React from 'react'
import PropTypes from 'prop-types'
import { withRouter } from 'react-router'

// A simple component that shows the pathname of the current location
class ShowTheLocation extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  }

  render() {
    const { match, location, history } = this.props

    return (
      <div>You are now at {location.pathname}</div>
    )
  }
}

// Create a new component that is "connected" (to borrow redux
// terminology) to the router.
const ShowTheLocationWithRouter = withRouter(ShowTheLocation)
```

#### 주의사항

`shouldComponentUpdate`에 의해 업데이트가 차단되지 않도록 `withRouter`를 사용하는 경우에는 `shouldComponentUpdate`를 구현한 컴포넌트를 가지고 `withRouter`를 사용하는 것이 중요합니다. 예로들어, Redux를 사용하는 경우

```js
// This gets around shouldComponentUpdate
withRouter(connect(...)(MyComponent))

// This does not
connect(...)(withRouter(MyComponent))
```

자세한 내용은 [여기](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/guides/blocked-updates.md)를 참고하시기 바랍니다.

#### Static Method와 속성

감싼 컴포넌트의 React가 아닌 모든 정적 Method나 속성들은 자동으로 `withRouter`가 처리된 컴포넌트에 복사되어집니다.

## Component.WrappedComponent

감싸진 컴포넌트는 정적 속성인 `WrappedComponent`를 통하여 접근할 수 있습니다. 보통 독립적으로 컴포넌트를 테스트하거나 다른 용도로 사용할 때 이용됩니다.

```js
// MyComponent.js
export default withRouter(MyComponent)

// MyComponent.test.js
import MyComponent from './MyComponent'
render(<MyComponent.WrappedComponent location={{...}} ... />)
wrappedComponentRef: func
```

## wrappedComponentRef : func

감싸진 컴포넌트의 `ref` 속성을 전달하는 함수입니다.

```js
class Container extends React.Component {
  componentDidMount() {
    this.component.doSomething()  
  }

  render() {
    return (
      <MyComponent wrappedComponentRef={c => this.component = c}/>
    )
  }
}
```