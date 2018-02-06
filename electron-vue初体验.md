# electron-vue 初体验

## 注意事项
* 首先确保node和npm是最新版本

避免使用镜像（我淘宝镜像安装有报错现象）
* 避免window的一些坑 

若上一项检查完成，我们可以继续设置所需的构建工具。使用 [windows-build-tools](https://github.com/felixrieseberg/windows-build-tools) 来为我们完成大部分烦人的工作。全局安装此工具将依次设置 Visual C++ 软件包、Python 等等。（安装时间可能会很久）
```
npm install --global --production windows-build-tools
``` 
到现在为止，所有工具都应该成功安装了，如果没有，那么你就会需要安装一个干净的 Visual Studio。请注意，这些并不是 electron-vue 自身的问题 (Windows 有时候可能会很难用 ¯\_(ツ)_/¯)。

> 该样板代码被构建为 vue-cli 的一个模板，并且包含多个选项，可以自定义你最终的脚手架程序。本项目需要使用 node@^7 或更高版本。electron-vue 官方推荐 yarn 作为软件包管理器，因为它可以更好地处理依赖关系，并可以使用 yarn clean 帮助减少最后构建文件的大小。

```
# 安装 vue-cli 和 脚手架样板代码
npm install -g vue-cli
vue init simulatedgreg/electron-vue my-project

# 安装依赖并运行你的程序
cd my-project
yarn # 或者 npm install
yarn run dev # 或者 npm run dev
```
到此为止项目已经可以跑起来了（真正自己动手的时候可能远没有你看起来的那么容易，各种环境问题都会是绊脚石）


