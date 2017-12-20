# 安装
github https://github.com/parcel-bundler/parcel?utm_source=gold_browser_extension


首先通过 Yarn 或者 npm 安装 Parcel :
```
npm install -g parcel-bundler
```

在你正在使用的项目目录下创建一个 package.json 文件:

```
npm init -y
```
我用的原来项目的package.json
加入或修改原来的script
```
"dev2": "parcel index.html",
"build2": "parcel build index.html --public-url /"
```

# 安装依赖
```
npm i parcel-bundler parcel-plugin-vue babel-preset-env --dev
```
其中parcel-bundler是主要的工具，对于vue结尾的单文件，需要单独处理文件类型，
parcel-plugin-vue这个插件会通过vueify来生成对应的代码，parcel会自动加载parcel-plugin开头的依赖。

# 配置.babelrc
```
{
  "presets": [
    ["env"]
  ]
}

```
# 配置postcss.config.js
```
// postcss.config.js
module.exports = {
  plugins: [
    require('autoprefixer')({
      browsers: [
        'last 20 versions',
        'IE 9',
        'iOS >= 8'
      ]
    })
  ]
}

```
# index.html引入 入口js
```
<body>
  <div id="app"></div>
  <!-- built files will be auto injected -->
  <script src="./src/main.js"></script>
</body>
```
# 运行
```
npm run dev2
```