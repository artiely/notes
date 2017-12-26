跨域的解决办法有很多
这里先来个简单暴力一劳永逸的办法
1.找到chrome的安装目录chrome.exe新建快捷方式
2.重命名快捷方式`跨域.exe`右键属性 目标 加入参数`  --disable-web-security` 记得前面空格
3.快捷方式发送到桌面

> 关闭所有chrome然后启动`跨域.exe` 会提示 您使用的是不受支持的命令行标记disable-web-security 安全和稳定性有所下降

这样你就有了一个支持跨域的chrome