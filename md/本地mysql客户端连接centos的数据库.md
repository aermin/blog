---
title: 本地mysql客户端连接centos的数据库
type: tags
date: 2017-10-01 11:04:02
tags: 
		- 开发记录总结
---

最近觉得用终端修改centos数据库有点麻烦，所以用本地mysql客户端可视化

进入centos的数据库
> mysql -u root -h localhost -p

使用数据库
> use mysql;

赋予远程权限

> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;

让权限立即生效
> flush privileges;


--解释
其中root表示用户名，%表示所有的电脑都可以连接，也可以设置某个ip地址运行连接，password表示密码

然后在本地的mysql客户端连接