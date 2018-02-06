## 介绍
pm2 是一个带有负载均衡功能的Node应用的进程管理器.。它使您可以永久保持应用程序的活动状态，无需停机即可重新加载应用程序，并且可以方便常见的系统管理任务
## 特性
- 行为配置
- 源地图支持
- 容器集成
- 观看和重新加载
- 日志管理
- 监控
- 模块系统
- 最大内存重新加载
- 集群模式
- 热重新加载
- 开发工作流程
- 启动脚本
- 部署工作流程
- PaaS兼容
- Keymetrics监测
- API
## 资料
[官方文档](http://pm2.keymetrics.io/docs/usage/quick-start/)
## 安装
安装最新最稳定的版本
```
npm install pm2@latest -g
```
> linux 此时执行pm2可能会提示找不到pm2命令（如果是nvm安装的node）,
通过创建软链接的方法，使得在任意目录下都可以直接使用pm2命令：前面的地址是pm2安装的地方
，不知道自己装到哪里去了可以使用命令 whereis pm2 
```
ln -s /root/node-v6.9.5-linux-x64/bin/pm2  /usr/local/bin/pm2
```

## 使用
```
pm2 start ./bin/www
```
给启动的应用加个名称便于管理
```
pm2 start ./bin/www --name myapp
```

## 安装启动脚本
```
pm2 startup
```
> 注意：更新nodejs时，pm2二进制路径可能会更改（如果您使用nvm，它将一定会更改）。因此，我们建议您startup在更新后运行该命令

## 保存当前进程列表
一旦启动了要管理的所有应用程序，就可以通过输入以下命令将该列表保存在预期的/意外的服务器重新启动之中：
```
pm2 save
```
它会将具有相应环境的进程列表保存到转储文件中$PM2_HOME/.pm2/dump.pm2
## 更新启动脚本

要更新启动脚本（例如，您通过NVM更改了Node.js版本），请运行以下命令：
```
pm2 unstartup
pm2 startup
```
## 查看启动的所有的程序
![](./git-img/pm2list.png)
## 重启服务看看是否生效（阿里云为例）
![](./git-img/reset.png)
重启成功后可以看到应用的id等信息改变了，程序可以继续访问
![](./git-img/resetpm2.png)

快速使用pm2到此结束，如果不想深入研究，其实学到这里就已经可以了。
