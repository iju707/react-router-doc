# matchPath

`<Route>`가 하는것과 유사하게 일반적인 렌더링과정 밖에서 경로에 대한 매칭을 처리할 때 사용합니다. 예로 렌더링하기전에 서버에서 연관된 데이터를 수집하는 경우입니다.

```jsx
import { matchPath } from 'react-router'

const match = matchPath('/users/123', {
  path: '/users/:id',
  exact: true,
  strict: false
})
```

## pathname

첫번째 파라미터는 일치하는지 확인하고 싶은 경로정보입니다. 만약 Node.js 서버에서 사용한다면 `req.url`이 될 것 입니다.

## props

두번째 파라미터는 pathname과 매칭하고 싶은 정보입니다. `Route`의 속성과 동일한 구조입니다.

```jsx
{
  path, // like /users/:id
  strict, // optional, defaults to false
  exact // optional, defaults to false
}
```