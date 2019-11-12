---
title: 阿里云mysql远程连接配置
date: 2019-04-28 16:22:47
tags: mysql
categories: mysql
---
# 阿里云远程连接mysql
轻量级和ECS云服务器都是相同配置方法

## 分配远程用户权限
```bash
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '你的密码' WITH GRANT OPTION; 
FLUSH PRIVILEGES;
```
打开``/etc/mysql/mysql.conf.d/mysqld.cnf``文件 
修改``bind-address = 127.0.0.1``为``bind-address = 0.0.0.0``

## 关闭服务器防火墙
```bash
iptables -X
iptables -F
iptables -Z
```
## 检查用户组权限分配和3306端口的开启
进入mysql
```bash
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select host,user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | root             |
| localhost | debian-sys-maint |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
5 rows in set (0.00 sec)
```
root账户host为%表示正确
检查3306端口
``tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN``
LISTEN监听3306表示正确

## 非本地服务器需要
我使用的是阿里云轻量级应用服务器，进入阿里云官网控制台
![](https://i.loli.net/2019/04/28/5cc567aa4c8c0.jpg)
然后添加MYSQL规则
![](https://i.loli.net/2019/04/28/5cc567e0e1d69.png)
最后再重启服务器就可以了

## 连接成功 
![](https://i.loli.net/2019/04/28/5cc5683c13fc6.png)
