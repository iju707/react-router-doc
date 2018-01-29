# 빠른 시작

React Web 프로젝트를 가장 빠르고 쉽게 하는 방법은 [Create React App](https://github.com/facebookincubator/create-react-app)을 사용하는 것 입니다.

맨 처음 create-react-app이 없다면 인스톨을 한 뒤 새로운 프로젝트를 만드시면 됩니다.

```bash
npm install -g create-react-app
create-react-app demo-app
cd demo-app
```

## 설치

React Router DOM은 [NPM에 배포](https://npm.im/react-router-dom)되어있기 때문에 npm이나 [yarn](https://yarnpkg.com/)을 활용해서 설치하시면 됩니다. Create React App이 yarn을 사용하기 때문에 여기서 또한 사용하겠습니다.

```bash
yarn add react-router-dom
# or, if you're not using yarn
npm install react-router-dom
```

그 다음, __src/App.js__에 아래의 예제를 복사/붙이기 해보겠습니다.

```jsx
import React from 'react'
import {
  BrowserRouter as Router,
  Route,
  Link
} from 'react-router-dom'

const Home = () => (
  <div>
    <h2>Home</h2>
  </div>
)

const About = () => (
  <div>
    <h2>About</h2>
  </div>
)

const Topic = ({ match }) => (
  <div>
    <h3>{match.params.topicId}</h3>
  </div>
)

const Topics = ({ match }) => (
  <div>
    <h2>Topics</h2>
    <ul>
      <li>
        <Link to={`${match.url}/rendering`}>
          Rendering with React
        </Link>
      </li>
      <li>
        <Link to={`${match.url}/components`}>
          Components
        </Link>
      </li>
      <li>
        <Link to={`${match.url}/props-v-state`}>
          Props v. State
        </Link>
      </li>
    </ul>

    <Route path={`${match.url}/:topicId`} component={Topic}/>
    <Route exact path={match.url} render={() => (
      <h3>Please select a topic.</h3>
    )}/>
  </div>
)

const BasicExample = () => (
  <Router>
    <div>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
        <li><Link to="/topics">Topics</Link></li>
      </ul>

      <hr/>

      <Route exact path="/" component={Home}/>
      <Route path="/about" component={About}/>
      <Route path="/topics" component={Topics}/>
    </div>
  </Router>
)
export default BasicExample
```

이제 알맞게 바꾸시면 됩니다. 즐겨보세요!