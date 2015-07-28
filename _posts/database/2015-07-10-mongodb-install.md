---
layout: post
title: linux 安装MongoDB
category: database
tags: [MongoDB , database, linux]
description: linux 安装MongoDB
---

## 源码安装MongoDB

### 准备工作

- 下载源文件

> wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.4.tgz

- 创建MongoDB执行账号

> groupadd mongodb
useradd mongodb -g mongodb

- 创建数据与日志保存目录

> mkdir -p /var/mongodb/data
mkdir -p /var/mongodb/logs
chown -R mongodb:mongodb /var/mongodb

## 解压安装

> tar -xzvf mongodb-linux-x86_64-3.0.4.tgz 
mv mongodb-linux-x86_64-3.0.4 mongodb3
chown -R mongodb:mongodb mongodb3/

## 启动 (dbpath,logpath前面是两个小横杠)

```
/opt/soft/mongodb3/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log --fork
```

> 使用--auth选项启动mongod进程即可启用认证模式

```
/opt/soft/mongodb3/bin/mongod --auth --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log --fork
```

**看到类似以下信息证明启动成功**

```
about to fork child process, waiting until server is ready for connections.
forked process: 5200
child process started successfully, parent exiting
```

## 测试

- 进入mongodb的shell模式

> /opt/soft/mongodb3/bin/mongo

- 查看数据库列表

> show dbs

- 查看当前版本

> db.version()

![测试图片][1]


## 其他配置事项

- 开机启动(将上面的启动脚本追加到文件中)

```
echo "/opt/soft/mongodb3/bin/mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log --fork" >> /etc/rc.d/rc.local
```

- 停止数据库实例 (admin)

> db.shutdownServer()

- 使用kill杀掉进程

> kill -2 PID 或 kill -15 PID
> **不要使用-9，可能会导致数据损坏**


## yum方式安装mongodb

- 在/etc/yum.repos.d/目录下新建文件mongodb-org-3.0.repo，并写入以下内容

> [mongodb-org-3.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.0/x86_64/
gpgcheck=0
enabled=1

- yum 安装

> sudo yum install -y mongodb-org

- 启动，关闭，重启

> sudo service mongod start
sudo service mongod stop
sudo service mongod restart

- 开机启动

> sudo chkconfig mongod on




  [1]: http://chuantu.biz/t2/10/1436501410x-954498974.jpg