# `<Prompt>`

페이지가 다른곳으로 이동되기전에 사용자에게 확인을 표시할 때 사용됩니다. 어플리케이션에서 다른 곳으로 이동을 방지(예: 입력항목의 일부를 입력하고 나갈 경우)하고자 하는 상태에 들어갈 때, `<Prompt>`를 렌더링 합니다.

```jsx
import { Prompt } from 'react-router'

<Prompt
  when={formIsHalfFilledOut}
  message="Are you sure you want to leave?"
/>
```

## message : string

다른 곳으로 탐색될 때 사용자에게 표시할 메시지입니다.

```jsx
<Prompt message="Are you sure you want to leave?"/>
```

## message : function

사용자에게 경로이동을 확인하고자 할 때 다음의 `location`과 `action`을 가지고 호출됩니다. 사용자에게 표시할 문자열을 반환하거나, `true`가 반환되면 전환을 허용합니다.

```jsx
<Prompt message={location => (
  `Are you sure you want to go to ${location.pathname}?`
)}/>
```

## when : boolean

탐색이 발생될 때 `<Prompt>`를 렌더링하는 조건대신, 경로이동에 대한 방지/허용을 `when={true}` 또는 `when={false}`에 따라 렌더링할 수 있습니다.

```jsx
<Prompt when={formIsHalfFilledOut} message="Are you sure?"/>
```