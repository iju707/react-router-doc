# location

`location`은 어플리케이션이 현재 어디에 위치해있는지, 어디로 갈지, 어디에서 왔는지 보여줍니다. 모습은 다음과 같습니다.

```jsx
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere',
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

아래와 같은 상황일 때 `location` 정보를 제공합니다.

* [Route의 component](/api/route.md#component)에 `this.props.location`
* [Route의 render](/api/route.md#render)에 `({ location }) => ()`
* [Route의 children](/api/route.md#children)에 `({ location }) => ()`
* [withRouter](/api/withrouter.md)에 `this.props.location`

`history.location`으로도 접근이 가능하지만, 변경될 수 있으므로 권장되지 않습니다. 자세한 내용은 history를 참고하시면 됩니다.

`location` 객체는 변경되지 않기 때문에 경로변경이 발생하였을 때 데이터를 활용할 수 있습니다. 특히 데이터를 조회하거나 애니메이션 처리에 유용합니다.

```jsx
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // navigated!
  }
}
```

탐색할 때 다양한 위치에 문자열말고 location 정보를 제공할 수 있습니다.

* Web의 [Link to](/api/link.md#to)
* Native의 Link to
* [Redirect to](/api/redirect.md#to)
* [history.push](/api/history.md#push)
* [history.replace](/api/history.md#replace)

경로문자열 이외에 특정 경로에서 반환받을 수 있는 별도의 location state를 추가할 때 `location` 객체를 활용하면 됩니다. Modal과 같이, 단순 경로 말고 `history`를 활용하여 UI를 탐색을 하는 어플리케이션일 때 유용합니다.

```jsx
// usually all you need
<Link to="somewhere"/>

// but you can use a location instead
const location = {
  pathname: 'somewhere'
  state: { fromDashboard: true }
}

<Link to={location}/>
<Redirect to={location}/>
history.push(location)
history.replace(location)
```

마지막으로 다음의 컴포넌트에 `location`을 전달할 수 있습니다.

* [Route](/api/route.md)
* [Switch](/api/switch.md)

이것은 라우터의 실제 location 정보를 사용하지 않고 진행할 수 있습니다. 실제 위치와 다른 위치에 있을 때 애니메이션하거나 네비게이션을 추가, 렌더링 속의 무언가 트릭이 필요할 때 유용합니다.