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
##### 1.安装rsync服务

 `yum install -y rsync`

##### 2.在B服务器上配置rsync服务

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


##### 3.在A服务器上配置rsync密码证书

```

	vim /etc/rsyncd.secrets #创建密码文件，文件内容如下：

	root #密码
	root:root #用户：密码
	
	chmod 0600 /etc/rsyncd.secrets #设置密码文件的权限
```

##### 4.执行同步文件
```

rsync -vzrtopg --delete --progress --password-file=/etc/rsyncd.secrets /home/test/ root@192.168.66.143::bakphp

```
##### 参考
详情参考配置：[http://linux008.blog.51cto.com/2837805/570936](http://linux008.blog.51cto.com/2837805/570936)

##### 5.实时同步
- 实时同步主要是通过 **<font color="red">inotify</font>** 工具进行同步

（1）安装,此次我安装的目录为:/usr/local/src/

```

	#cd /usr/local/src #切换到安装目录
	//下载文件包
	#wget http://github.com/downloads/rvoicilas/inotify-tools/inotify-tools-3.14.tar.gz 
	#tar xzvf inotify-tools-3.14.tar.gz #解压文件目录
	#cd inotify-tools-3.14 #切换到解压的包目录
	#./configure #指定安装目录，为下一步的编译做准备，比如指定目录:#./configure --prefix=/usr
	详情查看：https://www.cnblogs.com/tinywan/p/7230039.html
	#make #编译
	#make install #安装
```

（2）编写同步脚本

```

 	#ps -ef | grep rsync 查看进程命令
	#mkdir /root/bin 创建指定的目录
 	#cd /root/bin 切换到指定的目录
	#vim rsync.sh #编写脚本，脚本内容如下

	/********rsync.sh脚本内容*************/
	#!/bin/bash
	src=/home/test/  #定义同步目录
	des=bakphp #定义模块名称
	host="192.168.0.6 192.168.0.7 192.168.0.8" #定义同步的主机
	#实时监控同步的目录中的文件变化
	/usr/local/bin/inotifywait -mrq --timefmt '%d/%m/%y %H:%M' --format '%T %w%f' -e modify,delete,create,attrib $src | while read files 
	do
	for hostip in $host
	do
	rsync -vzrtopg --delete --progress --password-file=/etc/rsyncd.secrets $src root@$hostip::$des #用来执行同步的命令 
	done
	echo "${files} was rsynced" >>/tmp/rsync.log 2>&1
	done
	
```

（3）启动脚本

```
	
	#nohup /bin/bash /root/bin/rsync.sh &
	nohup 表示一直执行下去
	& 表示后台执行
```

（4）开机自动启动

```
	#echo "nohup /bin/bash /root/bin/rsync.sh &" >> /etc/rc.local
```

### 备注

1. 实时同步监控详细说明-[https://blog.csdn.net/halazi100/article/details/46807259](：https://blog.csdn.net/halazi100/article/details/46807259)
2. 执行实时同步监控时如果出现如下错误

错误提示：源安装包启动失败,error while loading shared libraries: libinotifytools.so.0: cannot open shared object file

解决方法：

```

	#echo "/usr/lib64/mysol" >> /etc/ld.so.conf #可能没用
	#ldconfig #必须执行
	#注释
	#ld.so.conf-动态链接库装载器，操作系统会根据这里面写的路径去找库
	#ldconfig-是告诉操作系统要引入新的变量了，需要重新识别一下。
	
```
	
错误详情和解决方法：[https://blog.csdn.net/wangxin6722513/article/details/44218893](https://blog.csdn.net/wangxin6722513/article/details/44218893)

rsync常见问题及解决办法：[https://blog.whsir.com/post-392.html](https://blog.whsir.com/post-392.html)

本地同步： rsync -av 源目录 目标目录
[https://blog.csdn.net/zsx0728/article/details/78455472](https://blog.csdn.net/zsx0728/article/details/78455472)


