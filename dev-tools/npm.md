在执行 npm install时，出现如下错误

npm ERR! phantomjs-prebuilt@2.1.14 install: `node install.js`
 
npm ERR! Exit status 1
 
npm ERR!
 
npm ERR! Failed at the phantomjs-prebuilt@2.1.14 install script 'node install.js
应该在命令后加参数 --ignore-scripts

npm install --ignore-scripts



如果觉得安装速度慢，安装源和原来 npm 是一样的，可以通用，修改方法如下：

yarn config get registry
# -> https://registry.yarnpkg.com
可以改成 taobao 的源：

yarn config set registry https://registry.npm.taobao.org
# -> yarn config v0.15.0
# -> success Set "registry" to "https://registry.npm.taobao.org".
# -> Done in 0.04s.


***一定注意源地址不能带引号


# npm 报发布注意事项

1.如果使用了淘宝源 要切回到默认的npm源 建议使用mrn切换

2.如果发布包的时候提示权限不足（windows） 请使用管理员权限打开cmd 登录npm
```
npm login
```