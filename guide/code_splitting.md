# 코드 분리

## 렌더링이 완료된 뒤 로딩하기

__Bundle__ 컴포넌트는 새로운 화면에 도달했을 때 로딩하기 좋습니다. 또한 백그라운드에서 어플리케이션의 나머지를 미리 로딩하는데도 좋습니다.

```js
import loadAbout from 'bundle-loader?lazy!./loadAbout'
import loadDashboard from 'bundle-loader?lazy!./loadDashboard'

// components load their module for initial visit
const About = (props) => (
  <Bundle load={loadAbout}>
    {(About) => <About {...props}/>}
  </Bundle>
)

const Dashboard = (props) => (
  <Bundle load={loadDashboard}>
    {(Dashboard) => <Dashboard {...props}/>}
  </Bundle>
)

class App extends React.Component {
  componentDidMount() {
    // preloads the rest
    loadAbout(() => {})
    loadDashboard(() => {})
  }

  render() {
    return (
      <div>
        <h1>Welcome!</h1>
        <Route path="/about" component={About}/>
        <Route path="/dashboard" component={Dashboard}/>
      </div>
    )
  }
}
```

### 코드 분리 + 서버 렌더링

몇번의 실패과정을 겪은 뒤 다음을 배웠습니다.

1. 서버에서 동기식 모듈 처리가 필요할 경우, 초기 render에 모든 번들을 가져와야합니다.
2. 렌더링 전, 서버 렌더링에 관련된 모든 번들을 클라이언트에 로딩할 경우 클라이언트의 render가 서버와 동일해야합니다. (가장 까다로우며, 가능할 듯 하지만 포기했습니다.)
3. 클라이언트 생명의 나머지를 위해 비동기 처리가 필요합니다.

우리는 구글이 서버렌더링 없이 필요한 만큼 잘 인덱싱 했다고 생각하며, 코드 분리와 서버 처리 캐싱에 대한 것을 제외하기로 했습니다. 서버 렌더링과 코드 분리를 시도하는 사람에게 성공을 기원합니다.