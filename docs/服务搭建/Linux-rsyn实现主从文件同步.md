### 说明：本地虚拟机
- 测试A服务器：192.168.66.142
- B服务器：192.168.66.143
- 把A服务器修改的文件同步到B服务器
- 原理：B服务器相当于服务端，A服务器为客户端

##### 用到的Linux命令
- cat /etc/sysconfig/iptables #浏览防护墙命令`
- iptables -A INPUT -p tcp --dport 873 -j ACCEPT #添加端口【可能没效果-原因待查】
- /etc/init.d/iptables save #执行上面添加命令后，需要执行保存命令
- vim /etc/sysconfig/iptables #直接修改端口`
- service iptables start/restart/stop #防护墙启动/重启/停止
- iptables -L #浏览防护墙
- netstat -lnpt grep 873 查看监听的端口
- ps -ef | grep rsync #浏览进程
- kill -9 进程号 #剔除某个进程

##### 操作流程
 1.安装rsync服务
 `yum install -y rsync`
 2.在B服务器上配置rsync服务

- 手动创建/etc/rsyncd.conf配置文件

```

	uid = root #用户ID

	gid = root #用户组ID

	use chroot = no

	max connections = 5

	pid file = /var/run/rsyncd.pid

	lock file = /var/run/rsync.lock

	log file = /var/log/rsyncd.log
	
	[bakphp] #描述模块名的名称

	path=/home/test/ #同步的目录

	comment = update

	ignore errors

	read only = no

	list = no

	hosts allow = 192.168.66.142 #允许的主机IP

	auth users = root #A服务器的授权用户名

	uid = root #A服务器的ID

	gid = root #A服务器的用户组

	secrets file = /etc/rsyncd.secrets #授权的密码文件【和A服务器相对应】
	
```

- 手动创建密码文件

```

	vim /etc/rsyncd.secrets #创建密码文件，文件内容如下：

	root #密码
	root:root #用户：密码
	
	chmod 0600 /etc/rsyncd.secrets #设置密码文件的权限
	
	rsync --daemon #启动服务
	【如果如下错误提示，则执行：rm -rf /var/run/rsyncd.pid】
	错误提示：failed to create pid file /var/run/rsyncd.pid: File exists

	echo "rsync --daemon" >> /etc/rc.local #开机自动自动

```

 3.在A服务器上配置rsync密码证书

```

	vim /etc/rsyncd.secrets #创建密码文件，文件内容如下：

	root #密码
	root:root #用户：密码
	
	chmod 0600 /etc/rsyncd.secrets #设置密码文件的权限
```

 4.执行同步文件
```

rsync -vzrtopg --delete --progress --password-file=/etc/rsyncd.secrets /home/test/ root@192.168.66.143::bakphp

```
##### 参考
详情参考配置：[http://linux008.blog.51cto.com/2837805/570936](http://linux008.blog.51cto.com/2837805/570936)