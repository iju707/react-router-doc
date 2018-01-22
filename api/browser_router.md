# <BrowserRouter>

HTML5의 history API(__pushState__, __replaceState__, __popState__)를 활용하여 URL과 화면을 동기화하는 <Router> 종류 중 하나 입니다.

```js
import { BrowserRouter } from 'react-router-dom'

<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App/>
</BrowserRouter>
```

## basename: string

모든 location 정보에 대한 기본 URL 입니다. 만약 어플리케이션이