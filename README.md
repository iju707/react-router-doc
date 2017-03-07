# Installation

React Router runs in multiple environments: browsers, servers, native, and even VR \(works in the dev preview!\) While many components are shared \(like Route\) others are specific to environment \(like NativeRouter\). Rather than requiring you install two packages, you only have to install the package for the target environment. Any shared components between the environments are re-exported from the environment specific package.



## Web

```bash
npm install react-router-dom@next
# or
yarn add react-router-dom@next
```

All of the package modules can be imported from the top:

```js
import {
  BrowserRouter as Router,
  StaticRouter, // for server rendering
  Route,
  Link
  // etc.
} from 'react-router-dom'
```

If you’re going for a really minimal bundle sizes on the web you can import modules directly. Theoretically a tree-shaking bundler like Webpack makes this unnecessary but we haven’t tested it yet. We welcome you to!

```js
import Router from 'react-router-dom/BrowserRouter'
import Route from 'react-router-dom/Route'
// etc.
```



