## RabbitMQ简介
- 基于AMQP协议的开源的消息代理和队列服务器
- Erlang语言编写开发的
- 官网：[http://www.rabbitmq.com/](http://www.rabbitmq.com/)

## 协议模型
- `Publisher application` 就是生产者应用服务
- `Consumer application` 就是消费者应用服务

## 下载地址
- rabbitmq下载：[http://www.rabbitmq.com/releases/rabbitmq-server/](http://www.rabbitmq.com/releases/rabbitmq-server/)
- erlang下载：[http://www.rabbitmq.com/releases/erlang/](http://www.rabbitmq.com/releases/erlang/)
- socat下载：[http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm](http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm)

## 安装脚本
rabbitmq依赖erlang,rabbitmq依赖socat
```shell
#下载
wget www.rabbitmq.com/releases/erlang/erlang-18.3-1.el7.centos.x86_64.rpm
wget http://repo.iotti.biz/CentOS/7/x86_64/socat-1.7.3.2-5.el7.lux.x86_64.rpm
wget www.rabbitmq.com/releases/rabbitmq-server/v3.6.5/rabbitmq-server-3.6.5-1.noarch.rpm
=========================================================================================
# 安装基础编译依赖
yum -y install build-essential openssl openssl-devel unixODBC unixODBC-devel make gcc gcc-c++ kernel-devel m4 ncurses-devel tk tc xz
# 请勿改变安装顺序
rpm -ivh erlang-18.3-1.el7.centos.x86_64.rpm
rpm -ivh socat-1.7.3.2-5.el7.lux.x86_64.rpm
rpm -ivh rabbitmq-server-3.6.5-1.noarch.rpm
#以上安装如果出错，出现：rpmdb: BDB0113 Thread/process 424227/139826856310848 failed
======================================================================================
#请重构rmp数据库，可使用以下解决方案
[root@cly ~]# cd /var/lib/rpm
[root@cly rpm]# ls
Basenames     __db.001  __db.003  Group       Name          Packages     Requirename  Sigmd5
Conflictname  __db.002  Dirnames  Installtid  Obsoletename  Providename  Sha1header   Triggername
[root@cly rpm]# rm -rf __db*
[root@cly rpm]# rpm --rebuilddb
```
- 重建rmp数据库参考：[http://www.rabbitmq.com/releases/erlang/](http://www.rabbitmq.com/releases/erlang/)

## 修改配置
- `vim /usr/lib/rabbitmq/lib/rabbitmq_server-3.6.5/ebin/rabbit.app`
- rabbitmq的核心配置，默认端口号5672，查找`loopback_users`配置项，去除`guest`的尖括号和双引号，保存退出即可。

## 基本命令使用
- 服务启动：`rabbitmq-server start &` （守护进程启动）
- 服务启动：`rabbitmq-server start` （非守护进程启动）
- 服务停止：`rabbitmqctl stop`
- 查看进程：`losf -i:5672`
- 管理插件：`rabbitmq-plugins enable rabbitmq_management`(启动插件)
- 插件访问：`127.0.0.1:15672`（账户：guest,密码：guest）
- 常见问题：插件访问失败【可能问题一：防护墙问题】。

## 其他命令
- 参考：[https://www.jianshu.com/p/8e77c6a958a7](https://www.jianshu.com/p/8e77c6a958a7)

