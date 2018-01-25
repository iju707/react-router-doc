# match

`match` 객체는 `<Route path>`에서 어떻게 URL과 매칭했는지에 대한 정보입니다. `match` 객체는 다음을 가지고 있습니다.

* `params` - (object) 키/값 쌍으로 경로에서 추출된 동적요소입니다.
* `isExact` - (boolean) 전체적인 URL이 모두 일치하는 경우 __true__ (문자 테일링 없음)
* `path` - (string) 일치하는 경로 패턴. 중첩된 <Route>에 유용
* `url` - (string) URL에서 일치하는 부분. 중첩된 <Link>에 유용

`match` 객체를 다양한 곳에서 접근할 수 있습니다.

* Route component에서 `this.props.match`
* Route render에서 `({match}) => ()`
* Route children에서 `({ match }) => ()`
* withRouter에서 `this.props.match`
* matchPath에서 반환값

만약 `Route`가 `path`가 없고 항상 일치한다면, 가장 가까운 상위 `match`값을 가지게 됩니다. 이것은 `withRouter` 사용하는 것과 동일합니다.