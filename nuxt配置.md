Nuxt 是一个基于 Vue 生态的更高层的框架，为开发服务端渲染的 Vue 应用提供了极其便利的开发体验。更酷的是，你甚至可以用它来做为静态站生成器,推荐尝试。目前Nuxt.js官方文档目前已经覆盖了大部分常用需求 , 这篇文章主要讲nuxt工程中一些需要注意的知识点，以及一些比较常用的功能介绍。



安装和部署
```
npm install -g vue-cli   //安装vue-cli架子

vue init nuxt-community/starter-template <project-name> //安装nuxt

npm run dev //开发运行， http://localhost:3000
```

服务器部署：(需要安装node环境和pm2工具)
```
npm install pm2 -g //强大的node进程管理器

npm run build

npm start //需要先配置package.json,配置如下：

"scripts": {
  "start": "pm2 start ./node_modules/nuxt/bin/nuxt-start -i max --attach",//-i max使用最大cpu核数，不需要可不取消
 },
//如果需要生产静态文件使用命令：npm run generate

```

nuxt.config.js的一些配置
```
  head: {
    script: [
      { src: '/' }/*外部js的引入，或者static中的js文件引入（/**.js）*/
    ]
  },
  env: {
    url: 'http://***.com' /*全局asyncData({env})的配置，比如请求头URL常量*/
  },
  /*代理IP的使用*/
  proxy: [ ['/api', {target: 'http://**.com'}] ], 
  build: {
    vendor: ['axios', 'qs'],/*多个地方引用，防止多次打包*/
    }
  }
//其他配置请看官网文档

```
nuxt添加静态文件
当我们在使用nuxt的时候，网站可以会遇到一部分是动态生成，而另一部分直接就是静态文件，在nuxt的文件配置下static目录，直接把文件放入static目录下，就可以通过域名或者IP（/对应的文件名）直接访问。



过滤器的使用配置 plugins/filter.js
```
import Vue from 'vue'
export function trim (str) {
  return str.replace(/(^\s*)(\s*$)/g, '')
}
const filters = {
  trim
}
export default filters
Object.keys(filters).forEach(key => {
  Vue.filter(key, filters[key])
})
```
使用{{string | trim}},需要在nuxt.config.js 配置 plugins: [ '~plugins/filters.js' ]



公用方法的配置 plugins/globle.js
```
import Vue from 'vue'

Vue.mixin({
  methods: {
    /* 设置标题描述 */
    $setSeo (title, content) {
      return { title: title, meta: [ { hid: 'description', name: 'description', content: content } ] }
    },
}
```
需要在nuxt.config.js 配置 plugins: [ '~plugins/globle.js' ],页面的使用方法：
```
head(){
   return this.$setSeo('title','des')
}
```

中间件的使用
比如说用户未登录状态下，通过路由闯入了需要鉴权的页面，我们可以自定义一些错误：  
```
// auth.js
export default function ({ store, error }) {
// 可通过组件的props接收error信息
  if (!store.state.token) {
    error({
      message: 'cookie失效或未登录，请登录后操作',
      statusCode: 403
    })
  }
}
```
在组件中使用该中间件： 
```
export default {
  middleware: 'auth',
  // 还可以把用户重定位到登录页
  fetch ({redirect, store}) {
    if (!store.state.token) {
      redirect('/login')
    }
  },
}

```
第三方库的引用plugins/element-ui.js
官方不屏蔽你正常的import，但有提供插件模式且推荐使用插件模式 
```
import Vue from 'vue'
import Element from 'element-ui'
Vue.use(Element)
/*nuxt.config.js配置*/
plugins: [{src: '~plugins/element-ui', ssr: true}]
css: ['element-ui/lib/theme-default/index.css']
vendor: ['element-ui']
/*按需加载 在build下配置babel，安装插件babel-plugin-component*/
babel: {
  plugins: [['component', [
    {
      'libraryName': 'element-ui',
      'styleLibraryName': 'theme-default'
    },
    'transform-async-to-generator',
    'transform-runtime'
  ]]],
  comments: false
}
```

Window 或 Document 对象未定义？
这是因为一些只兼容客户端的脚本被打包进了服务端的执行脚本中去。必须使用该变量process.browser，在引入的地方：
```
if (process.BROWSER_BUILD) {
  let lib=require('external_library')
}
nuxt.config.js 文件中配置 vendor 配置项 ：

build: {
    vendor: ['external_library']
}
```