# 철학

이 가이드의 목적은 React Router를 사용할 때 가져야할 사상을 설명하기 위함입니다. 이것을 "Dynamic Routing"이라 부르며, 기존에 익숙한 "Static Routing"과는 매우 다릅니다.

## Static Routing

Rails, Express, Ember, Angular 등과 같은 것을 사용했다면, Static Routing을 사용했을 것 입니다. 이러한 Framework에서는 화면에 렌더링하기 전에 어플리케이션을 초기화하는 부분에서 Route에 대한 정보를 정의합니다. React Router prev-v4에서도 Static 방식이었습니다. Express에서 어떻게 Route를 설정하는지 살펴보겠습니다.

```jsx
app.get('/', handleIndex)
app.get('/invoices', handleInvoices)
app.get('/invoices/:id', handleInvoice)
app.get('/invoices/:id/edit', handleInvoiceEdit)

app.listen()
```

어플리케이션이 시작되서 요청을 수신하기 전에 어떻게 Route할 것인지 정의를 합니다. Client쪽 Router도 이와 비슷합니다. Angular에서는 상단에 Route할 정보를 정의하고 렌더링하기 전에 최상위 AppModule에 해당 정보를 import합니다.

```jsx
const appRoutes: Routes = [
  { path: 'crisis-center',
    component: CrisisListComponent
  },
  { path: 'hero/:id',
    component: HeroDetailComponent
  },
  { path: 'heroes',
    component: HeroListComponent,
    data: { title: 'Heroes List' }
  },
  { path: '',
    redirectTo: '/heroes',
    pathMatch: 'full'
  },
  { path: '**',
    component: PageNotFoundComponent
  }
];

@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
  ]
})

export class AppModule { }
```

Ember는 빌드할 때 읽고 어플리케이션에 가져오는 routes.js 파일을 가지고 있습니다. 다른것과 동일하게 어플리케이션이 렌더링 하기 전에 처리합니다.

```jsx
Router.map(function() {
  this.route('about');
  this.route('contact');
  this.route('rentals', function() {
    this.route('show', { path: '/:rental_id' });
  });
});

export default Router
```

API가 다르지만, 결국은 "Static Routes"라는 방식 모델을 사용하고 있습니다. React Router 또한 v4 버전 이전까지는 이러한 방식을 사용했었습니다.

하지만 지금부터는 React Router를 적용하기 위해 위 내용을 모두 잊도록 하겠습니다! :0

## 배경

솔직히, React Router의 v2를 만들 때 꽤 좌절감을 느꼈습니다. 우리(Michael과 Ryan)는 API의 한계에 도달했고, React의 일부분(라이프사이클 등)을 재 구현하는 것밖에 안되며 UI를 구성하는 React의 사상과도 맞지 않음을 알게 되었습니다.

우리는 작업장에 가기전 호텔 복도를 거닐면서 무엇을 해야할 지 논의를 했습니다. 그때 서로에게 "우리의 작업장에서 가르치는 패턴을 사용해서 라우터를 만들어보면 어떨까?"라고 이야기 했습니다.

우리는 라우팅을 위한 어떤 모습이 필요한지 알고 있었고 그 개념 또한 가지고 있어서 개발하는데 필요한 시간만 있으면 되었습니다. 우리는 API를 완성하게 되었고 그것은 구성된 React와 API에 동떨어지지 않았고, React에 자연스럽게 조화를 이뤘습니다. 많은 사랑 부탁드립니다.

## Dynamic Routing

Dynamic Routing에서는 라우팅은 동작하고 있는 어플리케이션 밖에 설정이나 항목에 있는 것이 아닌 어플리케이션이 렌더링될 때 발생해야한다고 생각합니다. 이것은 React Router는 Component로 구성되어있습니다. 여기 간단한 예제로 어떻게 동작하는지 보겠습니다.

먼저, 대상으로 하는 환경을 _Router_ 컴포넌트로 감싸고 어플리케이션의 가장 최상위에 __render__를 호출합니다.

```jsx
// react-native
import { NativeRouter } from 'react-router-native'

// react-dom (what we'll use here)
import { BrowserRouter } from 'react-router-dom'

ReactDOM.render((
  <BrowserRouter>
    <App/>
  </BrowserRouter>
), el)
```

다음, 새로운 위치로 연결하기 위해 __Link__를 추가합니다.

```jsx
const App = () => (
  <div>
    <nav>
      <Link to="/dashboard">Dashboard</Link>
    </nav>
  </div>
)
```

마지막으로 사용자가 __/dashboard__에 접근했을 때 표출할 UI를 __Route__를 사용하여 렌더링합니다.

```jsx
const App = () => (
  <div>
    <nav>
      <Link to="/dashboard">Dashboard</Link>
    </nav>
    <div>
      <Route path="/dashboard" component={Dashboard}/>
    </div>
  </div>
)
```

__Route__는 __<Dashboard {...props} />__를 렌더링할 것 이며, __props__는 라우팅에 필요한 __{ match, location, history }__로 구성됩니다. 만약 사용자가 __/dashboard__가 아닌 곳을 접근한다면 __Route__는 __null__을 렌더링할 것 입니다. 이것이 전부입니다.

### Nested Route (Route 속의 Route)

많은 Router들이 "nested routes" 컨셉을 가지고 있습니다. 만약 React Router v4 이전을 사용했다면 알고 있을 것 입니다. Static Route 방식에서 Dynamic(rendered routes)으로 바꿀 때, 어떻게 "nest routes"를 할까요? Division 안에 넣으면 어떨까요?

```jsx
const App = () => (
  <BrowserRouter>
    {/* here's a div */}
    <div>
      {/* here's a Route */}
      <Route path="/tacos" component={Tacos}/>
    </div>
  </BrowserRouter>
)

// when the url matches `/tacos` this component renders
const Tacos  = ({ match }) => (
  // here's a nested div
  <div>
    {/* here's a nested Route,
        match.url helps us make a relative path */}
    <Route
      path={match.url + '/carnitas'}
      component={Carnitas}
    />
  </div>
)
```

보는것과 같이 별도의 __Router__안에 "nesting" API가 없습니다. __Route__은 __div__와 같은 컴포넌트이기 때문에 원하는대로 __Route__나 __div__ 등에 추가하면 됩니다.

좀더 복잡한 것을 살펴보겠습니다.

### 반응형 Route

사용자가 __/invoices__를 접근한다고 가정합니다. 어플리케이션은 다른 크기의 화면을 지원하며, 작은 화면에서는 invoice의 목록을 보여주고 invoice 대시보드에 대한 링크를 가지고 있습니다. 여기서 더 상세정보로 이동할 수 있습니다.

```
Small Screen
url: /invoices

+----------------------+
|                      |
|      Dashboard       |
|                      |
+----------------------+
|                      |
|      Invoice 01      |
|                      |
+----------------------+
|                      |
|      Invoice 02      |
|                      |
+----------------------+
|                      |
|      Invoice 03      |
|                      |
+----------------------+
|                      |
|      Invoice 04      |
|                      |
+----------------------+
```

큰 화면에서는 마스터-디테일 구조의 화면으로 왼쪽에는 네비게이션이 오른쪽에는 대시보드 또는 특정 invoice 상세화면을 볼 수 있습니다.

```
Large Screen
url: /invoices/dashboard

+----------------------+---------------------------+
|                      |                           |
|      Dashboard       |                           |
|                      |   Unpaid:             5   |
+----------------------+                           |
|                      |   Balance:   $53,543.00   |
|      Invoice 01      |                           |
|                      |   Past Due:           2   |
+----------------------+                           |
|                      |                           |
|      Invoice 02      |                           |
|                      |   +-------------------+   |
+----------------------+   |                   |   |
|                      |   |  +    +     +     |   |
|      Invoice 03      |   |  | +  |     |     |   |
|                      |   |  | |  |  +  |  +  |   |
+----------------------+   |  | |  |  |  |  |  |   |
|                      |   +--+-+--+--+--+--+--+   |
|      Invoice 04      |                           |
|                      |                           |
+----------------------+---------------------------+
```

잠시 진행을 멈추고 생각을 해보겠습니다. 큰화면에서 사용자가 __/invoices__로 접근하면 어떻게 될까요? 큰 화면에도 유효한 라우팅이 될까요? 오른쪽에는 무엇이 렌더링될까요?

```
Large Screen
url: /invoices
+----------------------+---------------------------+
|                      |                           |
|      Dashboard       |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 01      |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 02      |             ???           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 03      |                           |
|                      |                           |
+----------------------+                           |
|                      |                           |
|      Invoice 04      |                           |
|                      |                           |
+----------------------+---------------------------+
```

큰 화면에서는 __/invoices__는 잘못된 라우팅입니다. 그렇지만 작은화면에선 가능합니다! 이 재미있는 것을 만들기 위해, 누군가 큰 핸드폰을 가지고 있다고 가정해보겠습니다. 처음에는 세로모드로 __/invoices__를 접근한 다음 화면을 가로모드로 회전하겠습니다. 그러면 마스터-디테일 UI를 보여줄 수 있는 충분한 화면을 가지게 되었기 때문에 우리는 오른쪽을 리다이렉트 해야합니다.

React Router의 이전버전에 사용된 Static Routing 방식에서는 위와 같은 경우에 대한 해결책을 제시할 수 없었습니다. 그러나 동적 라우팅에서는 이러한 기능을 선언하여 작성할 수 있습니다. 정적인 설정이 아닌 방식으로 UI를 라우팅하려고 생각해보면 아마 다음과 같은 소스코드로 작성이 될 것 입니다.

```jsx
const App = () => (
  <AppLayout>
    <Route path="/invoices" component={Invoices}/>
  </AppLayout>
)

const Invoices = () => (
  <Layout>

    {/* always show the nav */}
    <InvoicesNav/>

    <Media query={PRETTY_SMALL}>
      {screenIsSmall => screenIsSmall
        // small screen has no redirect
        ? <Switch>
            <Route exact path="/invoices/dashboard" component={Dashboard}/>
            <Route path="/invoices/:id" component={Invoice}/>
          </Switch>
        // large screen does!
        : <Switch>
            <Route exact path="/invoices/dashboard" component={Dashboard}/>
            <Route path="/invoices/:id" component={Invoice}/>
            <Redirect from="/invoices" to="/invoices/dashboard"/>
          </Switch>
      }
    </Media>
  </Layout>
)
```

사용자가 핸드폰을 세로모드에서 가로모드로 회전하면 코드에서는 자동으로 대시보드로 리다이렉트할 것 입니다. 사용자에게 있는 모바일 디바이스에 맞춰서 유효한 라우팅정보로 변경됩니다.

이것은 단순한 예제이며, 더 많은 사례가 있지만 이쯤으로 하겠습니다. React Router를 사용하실 때 정적 라우팅이 아닌 컴포넌트로 생각하시면 됩니다. React Router에 대한 질문은 거의 React 질문과 유사하므로, React의 선언적 조합으로 문제를 어떻게 해결하는지 생각해보시면 됩니다.
