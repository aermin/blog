---
title: 记录下node项目部署上线的过程及坑
type: tags
date: 2017-09-15 10:11:32
tags: 
     - 运维
     - 开发总结
     - NODE
---

### 前言
这个月利用空余时间写的xmxz在修了n多bug之后，在填了不少坑之后终于把他部署到云服务器上线了。对我这个技术菜简直就是挖坑，填坑，挖坑，填坑。。。。。。现在趁还记得一些，记录一下，免得下次忘了
### nodejs写爬虫，论坛系统
说到nodejs，肯定离不开异步，我在项目中用的是
promise+async/await这一套异步方案

async/await是写异步代码的新方式，以前的方法有回调函数和Promise。
async/await是基于Promise实现的，它不能用于普通的回调函数。
async/await与Promise一样，是非阻塞的。
async/await使得异步代码看起来像同步代码，这正是它的魔力所在。


[了解回调函数是什么](https://www.zhihu.com/collection/119015788)

[了解异步与同步，阻塞与非阻塞](https://www.zhihu.com/question/19732473/answer/20851256)

[Async/Await替代Promise的6个理由](https://blog.fundebug.com/2017/04/04/nodejs-async-await/)

[Async/Await详解](https://cnodejs.org/topic/5640b80d3a6aa72c5e0030b6)

show code：

<!-- more -->

操作mysql

```
let query = function( sql, values ) {

  return new Promise(( resolve, reject ) => {
    pool.getConnection(function(err, connection) {
      if (err) {
        resolve( err )
      } else {
        connection.query(sql, values, ( err, rows) => {

          if ( err ) {
            reject( err )
          } else {
            resolve( rows )
          }
          connection.release()
        })
      }
    })
  })

}

let findDataByUser = function (  name ) {
  let _sql = `
    SELECT * from posts
      where name="${name}"
      `
  return query( _sql)
}
```
控制器：

```
Post:async(ctx,next)=>{
    if (ctx.request.querystring)
	 {				

		await userModel.findDataByUser(decodeURIComponent(ctx.request.querystring.split('=')[1]))
			.then(result=>{	
					 var string=JSON.stringify(result); 
					   res=JSON.parse(string).reverse();
			})
		await ctx.render('post',{
				session:ctx.session,
				posts:res	
			})
	}
	}
```
### 购买，部署云服务器
1.服务器购买
我买的是京东云的学生机，选的是centos7.2（国内用centos多一点）

2.服务器登陆
通过ssh方式登陆服务器
$ ssh root@192.168.1.112     //格式:ssh用户名@公网IP

3.[部署nodejs](https://help.aliyun.com/document_detail/50775.html)  / [部署nodejs](http://www.jb51.net/article/118493.htm)
ps:部署node环境我使用NVM安装多版本

### 上传项目文件
我用的是FileZilla 这个ftp可视化客户端
直接去官网下载安装
然后输入主机名（你买的云服务器的公网ip） ，用户名（默认是root），密码（你设的云服务器密码）
还有端口22 。然后连接。
想上传啥直接拖拽就行了，记得先把项目里的node包删掉，不然文件数量
分分钟上万。。。。上传到猴年马月。

正确姿势->
删除node包，在云服务器中 npm i  
  
### 部署mysql
如果想简单快速搞定mysql部署的可以用centos6.5
centos7以上的版本部署mysql有点麻烦
但是呢，对新鲜技术充满鸡血的我还是入坑centos7.2😂

1.确认你的系统环境

```
# cat /etc/redhat-release 
```
2.安装mysql

```
#yum install mysql
#yum install mysql-devel
#yum install mysql-server
```
如果你是centos7以上版本，你会发现安装mysql-server会失败

```
[root@yl-web yl]# yum install mysql-server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.sina.cn
 * extras: mirrors.sina.cn
 * updates: mirrors.sina.cn
No package mysql-server available.
Error: Nothing to do
```
原因是CentOS 7 版本将MySQL数据库软件从默认的程序列表中移除，用mariadb代替了。

> 解决办法

```
# wget http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
# rpm -ivh mysql-community-release-el7-5.noarch.rpm
# yum install mysql-community-server
```
安装成功后重启mysql服务。

```
# service mysqld restart
```
初次安装mysql，root账户没有密码。

```
[root@yl-web yl]# mysql -u root 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.6.26 MySQL Community Server (GPL)

Copyright (c) 2000, 2015, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
4 rows in set (0.01 sec)

mysql>
```

设置密码方案1

```
mysql> set password for 'root'@'localhost' =password('password');
Query OK, 0 rows affected (0.00 sec)

mysql> 
```
不需要重启数据库即可生效。


设置密码方案2（此方案将提高mysql安全性）

```
sudo mysql_secure_installation
```
这将提示您输入默认的根密码。一旦您输入，您将需要更改它。
接下去选择yes or no参考这个[连接](http://www.jb51.net/article/35426.htm)

登录mysql

```
# mysql -u root -p
```
输入密码

如果是这样的

```
mysql>
```
则说明没问题了

最后mariadb自动替换了，将不再生效。

```
# rpm -qa |grep mariadb
```

参考：
[文章1](https://www.howtoing.com/how-to-install-mysql-on-centos-7/) 
[文章2](http://www.cnblogs.com/starof/p/4680083.html)

然后要修改mysql可以查这些语法
[链接](http://www.cnblogs.com/719907411hl/p/6558987.html)

附带一个定心丸，如果mysql安装失败了要先彻底删除mysql

用这方法-> [centos下彻底删除MYSQL 和重新安装MYSQL](http://blog.duicode.com/1529.html)

另外一个看起来还可以的[教程](http://www.jb51.net/article/107075.htm)

这里我还用了pm2这个进程管理器，证进程永远都活着（刚好外加一些模块可以让我的爬虫程序每天定时爬取）
安装

```
npm install -g pm2
```
启动

```
pm2 start app.js
```

其他pm2[指令教程](http://www.nodeclass.com/articles/89283)
