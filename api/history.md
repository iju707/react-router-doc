# history

이 문서에서 "history"와 "history 객체"에 관련된 내용은 [history package](https://github.com/ReactTraining/history)를 참고하시면 됩니다. React Router가 React를 제외하고 참조하는 2개의 의존성 중 하나입니다. 다양한 JavaScript 환경에서 세션 history 관리를 위한 여러가지 구현체를 제공합니다.

다음의 용어 또한 사용됩니다.

* **browser history** - DOM에 특화된 HTML5 history API를 지원하는 웹 브라우저에 유용한 구현체
* **hash history** - DOM에 특화된 기존 웹브라우저를 위한 구현체
* **memory history** - 메모리 방식으로 테스팅 또는 DOM 기반이 아닌 환경(React Native 등)에 유용한 구현체

**history** 객체는 일반적으로 다음의 속성과 함수를 가지고 있습니다.

* `length` - (숫자) history 스택에 쌓인 엔트리 숫자
* `action` - (문자열) 현재 액션 (PUSH, REPLACE, POP)
* `location` - (객체) 현재 위치로, 다음의 속성을 가짐
  * `pathname` - (문자열) URL 경로
  * `search` - (문자열) URL 쿼리 문자열
  * `hash` - (문자열) URL 해시값
  * `state` - (문자열) 위치 특화 상태값(예, 위치가 스택에 쌓인 경우 PUSH), broswer 또는 memory histroy에서만 가능
* `push(path, [state])` - (함수) history 스택에 새로운 엔트리 추가
* `replace(path, [state])` - (함수) history 스택의 현재 엔트리 교체
* `go(n)` - (함수) history 스택에서 현재 위치를 `n` 위치로 이동
* `goBack()` - (함수) `go(-1)`과 동일
* `goForward()` - (함수) `go(1)`과 동일
* `block(prompt)` - (함수) 위치탐색 방지 ([history 문서 참고](https://github.com/ReactTraining/history#blocking-transitions))

## history는 mutable(변경가능)입니다.

history 객체는 mutable입니다. 그렇기 때문에 [location](/api/location.md)를 접근할 때는 `history.location`이 아닌 [`<Route>`](/api/route.md)의 렌더링 속성에서 접근해야 합니다. 이렇게 해야 React의 처리 주기에 맞게 정상동작 할 수 있습니다. 예로들면,

```jsx
class Comp extends React.Component {
  componentWillReceiveProps(nextProps) {
    // true가 될 것 입니다.
    const locationChanged = nextProps.location !== this.props.location

    // INCORRECT, history 객체는 변하기 때문에 *항상* false가 될 것 입니다.
    const locationChanged = nextProps.history.location !== this.props.history.location
  }
}

<Route component={Comp}/>
```

추가적인 속성은 사용할 구현방식에 따라 표현하시면 됩니다. 자세한 내용은 [history 문서](https://github.com/ReactTraining/history#properties)를 참고하시기 바랍니다.
