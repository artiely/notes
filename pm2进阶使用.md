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

未完待续