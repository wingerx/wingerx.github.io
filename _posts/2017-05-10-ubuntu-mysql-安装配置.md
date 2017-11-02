---
layout: post
title: Ubuntu 安装 MySQL
subtitle: Ubuntu 16.04下安装 MySQL
date: 2017-05-10
author:     winger
header-img: img/post-bg-25.jpeg
catalog: true
tags:
    - Linux
    - MySQL
---


## 安装 MySQL

`sudo apt-get install mysql-server mysql-client`

## 测试是否安装成功

`sudo netstat -tap | grep mysql`

## 相关操作

* 登录 `mysql -uroot -p`
* 检查MySQL 服务器占用端口 `netstat -nlt|grep 3306`
* 检查MySQL 服务器系统进程 `ps -aux|grep mysql`
* 查看数据库的字符集编码 `show variables like '%char%'`

## 让MySQL服务器被远程访问
* 打开mysql配置文件
```
sudo vim /etc/mysql/my.cnf
#将bind-address = 127.0.0.1 注释掉
#bind-address = 127.0.0.1
```

* 修改后，重启MySQL服务器
`sudo /etc/init.d/mysql restart`

* 重新登录 `mysql -uroot -p`
```
grant all privileges on *.* to 'root'@'%' identified by 'xxxxxx';
flush privileges;
```

* 检查MySQL服务器占用端口
```
netstat -nlt|grep 3306
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN
```

我们看到从之间的网络监听从 127.0.0.1:3306 变成 0 0.0.0.0:3306，表示MySQL已经允许远程登陆访问。

## 将字符编码设置为UTF-8

默认情况下，MySQL的字符集是latin1，因此在存储中文的时候，会出现乱码的情况，所以我们需要把字符集统一改成UTF-8。
打开mysql配置文件
`sudo vim /etc/mysql/my.cnf`

```
a） 打开mysql配置文件：

                vim/etc/mysql/my.cnf
b） 在[client]下追加：

                default-character-set=utf8
c） 在[mysqld]下追加：

                character-set-server=utf8
d） 在[mysql]下追加：

                default-character-set=utf8
```
修改后，重启MySQL服务器,并登录
`mysql -uroot -p`

再次查看字符串编码
```
mysql> show variables like '%char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
```
