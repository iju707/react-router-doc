# `<NavLink>`

[`<Link>`](/api/link.md)의 특수한 버전으로 현재 URL과 매칭될 경우 해당 요소를 렌더링하는데 스타일을 추가할 수 있습니다.

```jsx
import { NavLink } from 'react-router-dom'

<NavLink to="/about">About</NavLink>
```

## activeClassName : string

만약 활성화되면 추가되는 `className` 입니다. 기본적으로 `active`라는 클래스가 추가됩니다. 이것은 `className` 속성에 추가됩니다.

```jsx
<NavLink
  to="/faq"
  activeClassName="selected"
>FAQs</NavLink>
```

## activeStyle : object

활성화되면 추가될 스타일입니다.

```jsx
<NavLink
  to="/faq"
  activeStyle={{
    fontWeight: 'bold',
    color: 'red'
   }}
>FAQs</NavLink>
```

## exact : boolean

만약 `true`일 경우, 경로가 정확히 일치해야 활성화되며, 클래스와 스타일이 적용됩니다.

```jsx
<NavLink
  exact
  to="/profile"
>Profile</NavLink>
```

## strict : boolean

만약 `true`일 경우, 현재 URL이 경로와 일치한지 고려할 때 `location.pathname`의 맨뒤 슬래시까지 비교합니다. 자세한 내용은 [`<Route strict>`](/api/route.md#strict)를 참고하시면 됩니다.

```jsx
<NavLink
  strict
  to="/events/"
>Events</NavLink>
```

## isActive : function {#isActive}

해당 링크가 활성화되는지 확인하는 로직을 추가하는 함수입니다. 기본적으로 URL의 경로가 링크의 경로와 일치하는 것 이외에 더 많은 것을 하고자 할 경우 유용합니다.

```jsx
// only consider an event active if its event id is an odd number
const oddEvent = (match, location) => {
  if (!match) {
    return false
  }
  const eventID = parseInt(match.params.eventID)
  return !isNaN(eventID) && eventID % 2 === 1
}

<NavLink
  to="/events/123"
  isActive={oddEvent}
>Event 123</NavLink>
```

## location : object

[isActive](#isActive)는 현재 history location 정보를 가지고 비교합니다.(일반적으로 브라우저의 URL) 만약 다른 location과 비교가 필요할 경우에 [location](/api/location.md)를 추가하면 해당 정보가 전달됩니다.