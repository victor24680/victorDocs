## 主从复制形式
+ 一主一从
+ 主主复制
+ 一主多从---扩展系统读取的性能。
+ 多主一从

## 主从复制的用途
- 实时灾备，用于故障切换
- 读写分离,提供查询服务
- 备份，避免数据丢失，影响业务

## 主从部署的必要条件
+ 主库开启binlog日志（设置log-bin参数）
+ 主从server-id不同
+ 从库服务器能连通主库

## 主从复制原理
从库产生两个线程，一个I/O线程,一个SQL线程；
I/O线程去请求主库的binlog日志，并将得到的binlog日志写到relay log（中继日志）文件中；
主库会生成一个log dump线程，用来给从库I/O线程传binlog

SQL线程，会读取relay log文件中的日志,并解析成具体操作，来实现主从的操作一致，而最终得到数据一致。

详情介绍：[https://www.cnblogs.com/Aiapple/p/5792939.html](https://www.cnblogs.com/Aiapple/p/5792939.html)

备注：主从同步时，必须保证双方的数据是一样的，否则，可能从库无法同步（比如无法更新，插入数据等）；

## mysql 配置文件目录
`vim /etc/my.cnf`
## 主库配置
```

	在配置文件下添加如下配置
	server-id=1 #主服务等于1
	log-bin = /var/lib/mysql/binlog #日志目录
	binlog-do-db = testdba #同步的数据库

	datadir=/var/lib/mysql #数据目录
	socket=/var/lib/mysql/mysql.sock #链接目录

	#链接数据库 mysql -h ip_host -u user_name -p
	#查看主服务器当前二进制日志名和偏移量，这个操作的目录时为了在从库启动之后，
	从这个点开始进行数据的恢复；
	mysq>show master status;
	+------------------+----------+--------------+------------------+
	| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
	+------------------+----------+--------------+------------------+
	| mysql-bin.000016 |      107 | testdba      |                  |
	+------------------+----------+--------------+------------------+
	1 row in set (0.00 sec)
	
	主库已经配置好
```

## 从库配置 
```	

	vim /etc/my.cnf
	#从数据库库的配置
	server_id = 2
	master-host = 192.168.66.142
	master-user = root
	master-password= root
	master-port = 3306
	replicate-do-db = testdba
	#mysql5.5以前的版本配置如上配置,5.5以后，配置如下配置
	#连接mysql服务开始配置如下：
	mysql>
	   CHANGE MASTER TO MASTER_HOST='192.168.66.142',
	　　MASTER_PORT=3306,
	　　MASTER_USER='root',
	　　MASTER_PASSWORD='root',
	　　MASTER_LOG_FILE='mysql-bin.000016',#日志文件名
	　　MASTER_LOG_POS=107; #后面两个参数的值与主库保持一致

	mysql>slave start; #启动slave进程
	mysql>slave stop;  #停止slave进程

	mysql>show slave statu\G; #查看slave的状态,
	#如果下面两项目配置都为Yes，则表示配置正确
	Slave_IO_Running:  Yes
	Slave_SQL_Running: Yes
```

## 手动同步主库数据到从库【导出->远程传输->导入】

```
	
	mysql>flush tables with read lock;#停止主库的数据更新操作-即数据库锁定操作；
	mysql>lock tables ref_test read;#单独锁定某个数据表；
	#备注：上面的两个操作，在退出mysql终端的时候，会隐式的提交，执行unlock tables,如果想表锁定一直有效，则不能退出终端。

	#导出数据库，数据库备份；
	mysqldump -uroot -proot testdba > testdba.sql; #数据库备份脚本，及将数据库备份到当前目录
	#数据库导入，即导入数据到数据库testdba;
	mysqldump -uroot -proot testdba < testdba.sql;

	#1.将备份文件远程传输到指定目录
	#注意：用户账户对指定的目录必须有写入的权限；
	scp testdba.sql 用户名@192.168.66.143:/root/

	#2.将远程文件远程传输到本地指定目录
	scp 用户名@192.168.66.143:/root/testdba.sql /home/ 

	mysql>unlock tables;#解除锁定

	mysql> create database cmdb default charset utf8;#创建一个新的数据库

```


## 配置常见错误

	Slave_IO_Running: NO
	这是一个很常见的错误，总结起来就三个原因：

	1.主库的网络不通，或者主库的防火墙拒绝了外部连接3306端口，网络必须互通；
	2.在配置从库时，输错了ip地址和密码，或者主库在创建用户时写错了用户名和密码
	3.在配置从库时，输错了主服务器的二进制日志信息
	4.ID问题，在安装mysql数据库的时候，默认server-id=1,此处必须设置一样的才行；
	5.pos不正确，登录mysql服务终端，查看show master status，排除这方面的原因
	6.Last_Io_Errno错误代码详情：https://blog.csdn.net/u011698346/article/details/37934505




