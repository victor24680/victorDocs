根据自己的工作心得，记录一下mysql常用到的权限手册

+  连接数据库

```

	#允许用户myuser连接数据库，且有操作表的所有权限【all代表所有权限】
	mysql>grant all privileges on *.* to 'myuser'@'%'identified by 'mypasswod' with grant option;
	#with grant option 表示该用户可以将自己拥有的权限授权给别人；
```

+ 查看用户自己的权限

```

	mysql>show grants;
```
+ 创建一个只用查询权限的用户

```

	mysql>grant usage,select on *.* to 'myuser'@'%' identified by 'mypasswod' with grant option;

 	usage 表示无权限
 	*.* 第一个"*"表示数据库名，第二个表示-表名
	with grant option 表示允许用户将自己的权限授权给其他用户

```

+ 回收用户的删除权限【删除用户的权限】

```

	mysql>revoke delete on *.* from 'myuser'@'%';
```
+ 查看指定用户拥有的权限

```

	show grants for 'myuser'@'%';
```
+ 删除用户信息

```

	mysql>drop user 'webuser'@'%';
```

+ 为某个用户重命名

```

	mysql>rename user 'test1'@'%' to 'test2'@'%';
```

+ 用set password命令，为某个用户设置密码

```

	mysql>set password for 'root'@'localhost'=password('root')
```

+ 刷新权限

```

	#在执行权限操作之后，需要刷新权限，使之新的操作生效
	mysql>flush privileges；
```

+ 重启mysql服务

```

	service mysqld restart #重启命令一
	/etc/init.d/mysql restart #重启命令二
```

+ 修改默认的3306端口

```

	mysql>show global variables like 'port';#显示如下：
	+---------------+-------+
	| Variable_name | Value |
	+---------------+-------+
	| port          | 3306 |
	+---------------+-------+
	1 row in set (0.00 sec)
	
	vim /etc/my.cnf
	直接修改 port=51250 参数即可
	备注：记得重启数据库；如果连接不上，可能是linux防护墙的端口问题；
	
```


###### 备注
连接数据服务常见问题

1. IP授权问题，查看上免的授权命令：-'user'@'%'
2. 防护墙端口授权问题：vim /etc/sysconfig/iptables;

详情：[https://blog.csdn.net/anzhen0429/article/details/78296814](https://blog.csdn.net/anzhen0429/article/details/78296814)

