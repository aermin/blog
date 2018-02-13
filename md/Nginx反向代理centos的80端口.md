---
title: Nginx反向代理centos的80端口
type: tags
date: 2017-10-14 19:12:57
tags:
		- 开发总结
		- Nginx
---

最近换了个新的云主机，重新配置了下centos的环境。记录下Nginx反向代理centos的80端口的流程。

HTTP请求是80端口，但是在Linux上非root权限是无法使用1024以下端口的，并且因为安全原因，最好不要使用root权限登录服务器，所以无法直接用node.js程序监听80端口。因此我们需要使用Nginx给node.js做反向代理，将80端口指向应用程序监听的端口(如node.js默认的3000端口)。

1. 添加Nginx仓库

> yum install epel-release

2.下载Nginx

> yum install nginx

3.启用nginx服务

> service nginx start

 <!-- more -->

4.添加开机启动

> systemctl enable nginx

5.修改Nginx配置文件

> vi /etc/nginx/nginx.conf

6.进入配置文件后修改下

```
    server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name www.hxvin.com,hxvin.com;   /#修改这一行（写上你绑定的域名）
       root         /usr/share/nginx/html;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
proxy_pass http://127.0.0.1:4000;  # 添上这一行（端口号写你nodejs运行的端口号）
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

7.测试配置文件是否能够正确运行

> nginx -t

```
[root@jdu4e00u53f7 ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
出现这样，证明配置成功

8.重启nginx

>service nginx restart

现在直接在浏览器中输入我们配置的域名就可以访问我们的项目了。

ps:如果你用的云主机是国内的，那么你的域名必须先备案才能访问，不然只能域名加后端端口号访问了，如www.hxvin.com:4000