## 启用集群模式
只需要在启动应用时带上i参数
```
pm2 start app.js -i max
```
max：意味着PM2将自动检测可用的CPU数量和运行多个进程可以在负载均衡模式(但是不推荐使用)

或者使用json文件启动的
```
{
  "apps" : [{
    "script"    : "api.js",
    "instances" : "max",
    "exec_mode" : "cluster"
  }]
}
```
当然还支持js和ylm文件,js示例如下
[相关资料](http://pm2.keymetrics.io/docs/usage/application-declaration/)
```
module.exports = {
  apps : [{
    name        : "worker",//应用名称
    script      : "./worker.js", //脚本路径相对于pm2开始
    watch       : true, //开启监察，文件改变自动重启
    env: {
      "PORT": 3000,
      "NODE_ENV": "development",
    },
    env_production : {
        "PORT": 80
       "NODE_ENV": "production"
    }
  },{
    name       : "api-app",
    script     : "./api.js",
    cwd         : "/home/www/project_root/current",// 
    "error_file": "./logs/app.err.log",
    "out_file": "./logs/app.out.log", 
    "log_date_format" : "YYYY-MM-DD HH:mm Z"
    instances  : 4, // 实例（多核）
    exec_mode  : "cluster" // 集群模式
  }]
}
```

然后再启动进程
```
pm2 start processes.json
```
重载应用
```
pm2 reload <app_name>
```
或者
```
pm2 reload process.json
pm2 reload process.json --only api
```

## 一键发布
yml的书写方式(process.yml)
```
apps:
  - script   : server.js
    name : 'pm2 test'
    watch  : true
    env    :
      NODE_ENV: development
    env_production:
      NODE_ENV: production
deploy :
  production :
    user : root
    key : C:/Windows/SSH-ubuntu.pem  #服务器sshkey(阿里云再服务器镜像创建的时候会生成 然后保存到本地)
    host : 
      - 120.78.174.212                #服务器ip
    port : 22
    ref : origin/master
    repo : git@gitee.com:artiely/pm2test.git  #仓库地址
    path : /www/pm2test/production               #发布地址
    ssh_options : StrictHostKeyChecking=no       #ssh权限
    pre-deploy : git fetch --all                 #发布前的操作
    post-deploy : 'npm install && npm run build && pm2 startOrRestart process.yml --env production'
    env : 
      NODE_ENV : production
```
第一次发布本地执行
```
pm2 deploy process.yml production setup
```
然后每次发布只需本地执行如下命令，服务器就会自动拉取创库代码并发布
```
pm2 deploy process.yml production
```

## 注意事项
github上必须有服务器的公钥和本地的公钥

https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/

这里有各个系统生成id_rsa的方法


1.服务器node版本尽量使用最高稳定版
2.devDependencies里的代码在发布时不会被下载安装，如有需要请移动到dependencies
3.如果出现Host key verification failed.
解决方案 
1. 删除~/.ssh/known_hosts 文件中包含"gitlab.xxx.com"这一行的记录

2. 删除~/.ssh/known_hosts整个文件

3. 修改open ssh配置文件，安全级别调低（不推荐，仅限内网等安全级别较高的环境，公网不要使用）

SSH对主机的public_key的检查等级是根据StrictHostKeyChecking变量来配置的。可以通过降低安全级别的方式，来减少这一类提示。 

修改方法： 
编辑~/.ssh/config（代表个人配置，或/etc/ssh/ssh_config，代表全局配置） 
添加以下行 
Shell代码  收藏代码
StrictHostKeyChecking no  
UserKnownHostsFile /dev/null  # 为了更简化，把known_hosts也省略掉了  

下面附上StrictHostKeyChecking配置项的说明。 
StrictHostKeyChecking=no 
最不安全的级别，当然也没有那么多烦人的提示了，相对安全的内网测试时建议使用。如果连接server的key在本地不存在，那么就自动添加到文件中（默认是known_hosts），并且给出一个警告。 
StrictHostKeyChecking=ask 
默认的级别，就是出现刚才的提示了。如果连接和key不匹配，给出提示，并拒绝登录。 
StrictHostKeyChecking=yes 
最安全的级别，如果连接与key不匹配，就拒绝连接，不会提示详细信息。 