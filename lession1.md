# 环境

## 安装架手架
``` 
npm i -g create-react-app
```
## 创建名为geek的项目
```
create-react-app geek
cd geek
npm start // 如果端口冲突 scripts -> start.js 修改port 
```
## 安装第三方库redux
```
npm i redux -S
```
## 自定义配置文件
```
npm run eject
```
## 目录结构
```
│  .gitignore
│  lesson.md
│  package.json
│  README.md
│  tree.text
│  tree.txt
│  yarn.lock
│  
├─config
│  │  env.js
│  │  paths.js
│  │  polyfills.js
│  │  webpack.config.dev.js
│  │  webpack.config.prod.js
│  │  webpackDevServer.config.js
│  │  
│  └─jest
│          cssTransform.js
│          fileTransform.js
│          
├─node_modules
│       ...
│              
├─public
│      favicon.ico
│      index.html
│      manifest.json
│      
├─scripts
│      build.js
│      start.js
│      test.js
│      
└─src
        App.css
        App.js
        App.test.js
        index.css
        index.js
        logo.svg
        registerServiceWorker.js
```

## 更新react到最新版本

```
npm install react@next react-dom@next
```

## 安装antd-mobile

```
npm install antd-mobile@next --save
```
忍无可忍的安装速度
下载nrm插件
```
npm install nrm -g
```
```
$ nrm ls

* npm -----  https://registry.npmjs.org/
  cnpm ----  http://r.cnpmjs.org/
  taobao --  https://registry.npm.taobao.org/
  nj ------  https://registry.nodejitsu.com/
  rednpm -- http://registry.mirror.cqupt.edu.cn
  skimdb -- https://skimdb.npmjs.com/registry
```
```
$ nrm test

* npm ---- 702ms
  cnpm --- 272ms
  taobao - 3124ms
  nj ----- Fetch Error
  rednpm - Fetch Error
  npmMirror  1359ms
  edunpm - Fetch Error
// 什么鬼
```
```
$ nrm use cnpm  //switch registry to cnpm

    Registry has been set to: http://r.cnpmjs.org/
```

## 按需加载
```
npm install babel-plugin-import -D
```
在package.json-> babel对象里加如下一项
```
 "plugins": [["import", { "libraryName": "antd-mobile", "style": "css" }]]
```
这样就可以直接
```
import { Button } from 'antd-mobile'
```
## 如果是vs code编辑器要支持emmet语法 可以点设置->"emmet.triggerExpansionOnTab": true

## 安装状态管理redux
```
npm install redux -S
```
## redux处理异步插件 redux-thunk
```
npm install redux-thunk -S
```
使用applyMiddleware开启redux-thunk插件
index.js
```
import {creatStore,applyMiddleware} form 'redux'
import thunk form 'redux-thunk'
const store = createStore(reducer,applyMiddleware(thunk))
```
## 加入redux调试工具chrome下载扩展应用 reduxDevtoolsExtension
index.js
```
/* applyMiddleware 应用中间件的函数  compose组合函数*/
import { createStore, applyMiddleware,compose } from 'redux'
import thunk from 'redux-thunk'
import reducer from 'store/reducer'
import './index.css';
import App from './App';
import registerServiceWorker from './registerServiceWorker';
const store =createStore(reducer,compose(
    applyMiddleware(thunk),
    window.devToolsExtension?window.devToolsExtension():()=>{} // 检查浏览器是否有插件，没有就传个空函数
))
```
## 安装 react-redux 优雅的链接react和redux
```
npm install react-redux -S
```
index.js 引入 Provider 组件
```
import {Provider} from 'react-redux'
//使用
ReactDOM.render(
  (<Provider store={ store }>
   <App />
  </Provider>),
   document.getElementById('root')
   );
registerServiceWorker();
```
## 安装npm install babel-plugin-transform-decorators-le
gacy -D
让redux支持装饰器
package.json按照插件名加入配置 
## 安装 react-router4
```
npm install react-router-dom
```
index.js
```
import {BrowerRouter,Route,Link} from 'react-router-dom'
// BrowerRouter 包裹整个应；可以用this.props访问到路由对象
ReactDOM.render(
  (<Provider store={ store }>
  <BrowerRouter>
    <App />
  </BrowerRouter>
  </Provider>),
   document.getElementById('root')
   );
```
