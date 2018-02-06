# 开发

## 新建anthroute
index.js
```
import React from 'react';
import ReactDOM from 'react-dom';
/* applyMiddleware 应用中间件的函数  compose组合函数*/
import {createStore, applyMiddleware, compose} from 'redux'
import {Provider} from 'react-redux'
import {BrowserRouter, Route} from 'react-router-dom'
import thunk from 'redux-thunk'
import reducers from './reducer'
import './index.css';
// import App from './App';
import Login from './views/login/login.js'
import Signup from './views/signup/signup.js'
import  AuthRoute from './components/authroute/authroute'
import registerServiceWorker from './registerServiceWorker';

const store = createStore(reducers, compose(
  applyMiddleware(thunk),
  window.devToolsExtension ? window.devToolsExtension() : f => f
))
console.log('123', reducers)

// BrowserRouter 包裹整个应用
ReactDOM.render(
  (
    <Provider store={store}>
      <BrowserRouter>
        <div>
          <AuthRoute></AuthRoute>
          <Route path='/login' component={Login}></Route>
          <Route path='/signup' component={Signup}></Route>
        </div>
      </BrowserRouter>
    </Provider>
  ),
  document.getElementById('root')
);
registerServiceWorker();

```
authroute.js
```
import React from 'react'
import axios form 'axios'

class AuthRoute extends React.Component {
  compentDidMount() {
    axios({
      url:'/api/info'
    })
    // 获取用户信息
    // 用户是否登陆
    // 用户当前的地址是否是登录
    // 用户的信息是否完善
    // 用户是boos还是牛人
  }
}
export default AuthRoute
```