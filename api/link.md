# `<Link>`

어플리케이션 내에 선언적, 접속가능한 네비게이션을 제공합니다.

```jsx
import { Link } from 'react-router-dom'

<Link to="/about">About</Link>
```

## to : string

연결하고자 하는 경로이름 또는 위치 입니다.

```jsx
<Link to="/courses"/>
```

## to : object

연결하고자 하는 `location` 객체입니다.

```jsx
<Link to={{
  pathname: '/courses',
  search: '?sort=name',
  hash: '#the-hash',
  state: { fromDashboard: true }
}}/>
```

## replace : bool

만약 `true`일 경우, 이 링크를 클릭하면 `history` 스택에 새로운 경로를 추가하지 않고 현재 위치를 교체합니다.

```jsx
<Link to="/courses" replace />
```