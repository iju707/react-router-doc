# match

__match__ 객체는 __<Route path>__에서 어떻게 URL과 매칭했는지에 대한 정보입니다. __match__ 객체는 다음을 가지고 있습니다.

* __params__ - (object) 키/값 쌍으로 경로에서 추출된 동적요소입니다.
* __isExact__ - (boolean) 전체적인 URL이 모두 일치하는 경우 __true__ (문자 테일링 없음)
* __path__ - (string) 일치하는 경로 패턴. 중첩된 <Route>에 유용
* __url__ - (string) URL에서 일치하는 부분. 중첩된 <Link>에 유용

__match__ 객체를 다양한 곳에서 접근할 수 있습니다.

* Route component에서 this.props.match
* Route render에서 ({match}) => ()
* Route children에서 ({ match }) => ()
* withRouter에서 this.props.match
* matchPath에서 반환값

만약 __Route__가 __path__가 없고 항상 일치한다면, 가장 가까운 상위 __match__값을 가지게 됩니다. 이것은 __withRouter__ 사용하는 것과 동일합니다.