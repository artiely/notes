# mongodb数据库简单迁移 windows -> linux

cd 到本机mongodb的安装目录 如：
`C:\Program Files\MongoDB\Server\3.4\bin` 可以发现里面除了可以启动mongodb的`mongod.exe`还有很多启动程序

其中`mongodump.exe`和`mongorestore.exe`就分别是用来数据备份迁移的

## mongodump备份数据库

1. 常用命令格
```
mongodump -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 -o 文件存在路径
```
如果没有用户，可以去掉-u和-p。   
如果导出本机的数据库，可以去掉-h。    
如果是默认端口，可以去掉--port。   
如果想导出所有数据库，可以去掉-d。    
如果不指定-o，文件备份在当前目录下    

2. 导出所有数据库

```
 mongodump 

```
 
3. 导出指定数据库

```
mongodump -h 192.168.1.108 -d movie
```

导出后会在当前的bin目录下生成一个dump的文件夹，里面就是备份的数据打包上传到服务器等待恢复


## mongorestore还原数据库

> 注意事项 mongorestore 并不是在mongo shell里执行

可以执行查看命令在哪
```
root@:~# whereis mongorestore
mongorestore: /usr/bin/mongorestore /usr/share/man/man1/mongorestore.1.gz
```
然后
```
cd /usr/bin
```
1. 常用命令格式
```
mongorestore -h IP --port 端口 -u 用户名 -p 密码 -d 数据库 --drop 文件存在路径
```
 

--drop的意思是，先删除所有的记录，然后恢复。

2. 恢复所有数据库到mongodb中
```
root@bin:# mongorestore /root/dump/myblog/  #这里的路径是所有库的备份路径
```
 

3. 还原指定的数据库
```
root@bin:# mongorestore -d movie /root/dump/myblog/movie/  #movie这个数据库的备份路径
  
root@bin:# mongorestore -d movie_new /root/dump/myblog/movie/  #将movie还有movie_new数据库中
```

这二个命令，可以实现数据库的备份与还原，文件格式是json和bson的。无法指写到表备份或者还原。`mongoexport` 和`mongoimport`实现表的导入导出。