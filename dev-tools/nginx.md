环境：ubuntu 16.04
nginx 最新版本

提示错误：
Starting nginx (via systemctl): nginx.serviceJob for nginx.service failed because the control process exited with error code. See "systemctl status nginx.service" and "journalctl -xe" for details.

解决方案1:
systemctl status nginx.service可以看到具体的错误是什么

得到错误：
Failed to start A high performance web server and a reverse proxy server

I am running nginx on Raspbian Jessie operating system.

I just created new virtual host and reloaded nginx service:
```
/etc/init.d/nginx restart
```
Now I got:
```
[....] Reloading nginx configuration (via systemctl): nginx.serviceJob for nginx.service failed. See 'systemctl status nginx.service' and 'journalctl -xn' for details.
 failed!
 ```

Turns out I can test if the configuration file is OK with:
```
nginx -t -c /etc/nginx/nginx.conf
```
This time it says:
```
nginx: [emerg] could not build the server_names_hash, you should increase server_names_hash_bucket_size: 32
nginx: configuration file /etc/nginx/nginx.conf test failed
```
Lets check what this setting is in config:
```
grep server_names_hash_bucket_size /etc/nginx/nginx.conf
```

And it says that it is not configured:
```
# server_names_hash_bucket_size 64;
```
OK lets increase this as it says:
```
sed -i "s/^.*server_names_hash_bucket_size..*;
$/server_names_hash_bucket_size 64;/" /etc/nginx/nginx.conf
```

Check again:
```
grep server_names_hash_bucket_size /etc/nginx/nginx.conf
```

Its good now:
```
server_names_hash_bucket_size 64;
```

Lets test config file:
```
nginx -t -c /etc/nginx/nginx.conf
```

This now says:
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is 
successful
```

Got to go:
```
/etc/init.d/nginx restart
```