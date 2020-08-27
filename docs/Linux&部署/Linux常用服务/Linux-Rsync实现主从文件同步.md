### 说明：本地虚拟机
- 测试A服务器：192.168.66.142
- B服务器：192.168.66.143
- 把A服务器修改的文件同步到B服务器
- 原理：B服务器相当于服务端，A服务器为客户端

##### 可能用到的Linux命令
- cat /etc/sysconfig/iptables #浏览防护墙命令`
- iptables -A INPUT -p tcp --dport 873 -j ACCEPT #添加端口【可能没效果-原因待查】
- /etc/init.d/iptables save #执行上面添加命令后，需要执行保存命令
- vim /etc/sysconfig/iptables #直接修改端口`
- service iptables start/restart/stop #防护墙启动/重启/停止
- iptables -L #浏览防护墙
- netstat -lnpt grep 873 查看监听的端口
- ps -ef | grep rsync #浏览进程
- kill -9 进程号 #剔除某个进程

### Rsync（非实时）同步配置
#### 1. 密码授权同步文件
- 安装服务（A客户端，B服务端）
 ```shell
 yum install -y rsync
 ```
 - 推送文件
 ```shell
 #需要输入密码ssh登录密码
 rsync -av /home/test/ 192.168.66.143:/home/test/
 ```
 - 拉取文件
 ```shell
 #需要输入密码ssh登录密码
 rsync -av 192.168.66.143:/home/test/ /home/test/ 
 ```
 - 指定ssh端口
 ```shell
 #需要输入密码ssh登录密码,当ssh端口号发生变更时,需要指定端口号
 rsync -av -e "ssh -h 6018" 192.168.66.143:/home/test/ /home/test/ 
 ```
#### 2. 免密同步文件
###### 服务端配置

- 安装服务

 ```shell
 yum install -y rsync
 ```

- 手动创建 `vim /etc/rsyncd.conf` 配置文件

```shell
# 服务端配置
#用户名或用户ID号（可以通过：cat /etc/passwd 查看）
uid = root
#用户组名或组ID号
gid = root 

#最大并发连接数，0表示无限制
max connections = 5
#进程号文件
pid file = /var/run/rsyncd.pid
#设置锁文件名称
lock file = /var/run/rsync.lock
#设置日志文件路径
log file = /var/log/rsyncd.log

#设置端口号：默认为873端口
prot = 873 
# 设置本服务器所监听网卡接口的IP地址（一般不设置）
#address = 192.168.0.230 

#默认为true，rsync在传输文件以前，首先chroot到path参数所指定的目录；
#实现额外的安全防护，但需要root权限；
#不能备份指向外部的符号连接所指向的目录，（不推荐）
use chroot = no

#描述模块名的名称	
[bakphp] 
#同步的目录
path=/home/test/ 
#定义注释
comment = update
#忽略一些IO错
ignore errors
#是否允许客户端上传数据，yes不允许
read only = no
#客户端请求显示模块列表，本模块是否显示，默认显示
list = no
#允许的主机IP
hosts allow = 192.168.66.142 
#A服务器的授权用户名，此账户可以是客户端系统中不存在的用户（不建议使用不存在的用户）
auth users = root
#A服务器的ID
uid = root 
#A服务器的用户组
gid = root
#授权的密码文件【和A服务器相对应】
secrets file = /etc/rsyncd.secrets 

###【特别备注：请勿在配置之后添加注释内容，否则会导致如下错误，即认证失败】
@ERROR: auth failed on module bakphp
rsync error: error starting client-server protocol (code 5) at main.c(1649) [sender=3.1.2]

```
- 配置服务器端密码文件
```shell
# 创建服务端密码文件（内容形式：用户名:密码）
# echo "root:123456" > /etc/rsyncd.secrets 
# chmod 600 /etc/rsyncd.secrets #设置密码文件的权限
```
- 启动服务
```shell
# rsync --daemon #启动服务
【如果如下错误提示，则执行：rm -rf /var/run/rsyncd.pid】
错误提示：failed to create pid file /var/run/rsyncd.pid: File exists
# 开机自动自动
# echo "rsync --daemon" >> /etc/rc.local 
```

###### 客户端配置
- 安装服务
```shell
yum install -y rsync
```
- 配置客户端密码文件

```shell
#
# 创建客户端密码文件（内容形式：密码）
# echo "123456" > /etc/rsyncd.secrets 
# 设置密码文件的权限
# chmod 600 /etc/rsyncd.secrets 
```
- 启动服务：`rsync --daemon`

###### 手动执行同步文件命令
```shell
# 从A客户端同步（推送）到B服务端
# rsync -vzrtopg --delete --progress --password-file=/etc/rsyncd.secrets /home/test/ root@192.168.66.143::bakphp

# 从B服务端同步（拉取）到A客户端
# rsync -vzrtopg --delete --progress --password-file=/etc/rsyncd.secrets  root@192.168.66.143::bakphp /home/test/
```

<p style="text-indent:30px;">
参考配置一：<a href="http://linux008.blog.51cto.com/2837805/570936">http://linux008.blog.51cto.com/2837805/570936</a>
</p>

###### Rsync 命令选项说明
```shell
-a: --archives 归档模式，表示以递归方式传输文件，保持文件属性
-v: --verbose 详细模式输出
-r: --recursive 对子目录以递归模式处理
-t: --times 保留文件的时间戳
-o: --owner 保留文件的属主信息
-p: --perms 保留文件权限信息
-g: --group 保留文件属组信息
-z：对文件压缩后传输
# -a,-r，必须使用其中一个选项
--progress 显示备份进度
--delete 删除那些多余文件
例如：A客户端中有 1.txt 文件，B服务端中没有 1.txt，拉取服务端中的文件时，就会删除多余的1.txt文件;
```
- 参考一：[https://blog.51cto.com/13706064/2152373](https://blog.51cto.com/13706064/2152373)【配置参数详解】
- 参考二：[https://blog.csdn.net/stpeace/article/details/71788437](https://blog.csdn.net/stpeace/article/details/71788437)【配置参数&命令选项详解】
- 参考三：[https://www.cnblogs.com/wangchengshi/p/10770538.html](https://www.cnblogs.com/wangchengshi/p/10770538.html)【命令选项详解】
- 参考四：[https://blog.csdn.net/u012865381/article/details/77506397](https://blog.csdn.net/u012865381/article/details/77506397) 【配置参数详解】




### 实时同步配置
说明：要求已配置好免密同步文件
- 实时同步主要是通过 **<font color="red">inotify</font>** 工具进行同步

（1）安装-此次安装的目录为: `/usr/local/src/`

```shell
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

```shell
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

```shell
#nohup /bin/bash /root/bin/rsync.sh &
nohup 表示一直执行下去(ssh终端断开时，程序可以一直执行下去)
& 表示后台执行
```

（4）开机自动启动

```shell
#echo "nohup /bin/bash /root/bin/rsync.sh &" >> /etc/rc.local
```

### 备注

1. 实时同步监控详细说明-[https://blog.csdn.net/halazi100/article/details/46807259](：https://blog.csdn.net/halazi100/article/details/46807259)
2. 执行实时同步监控时如果出现如下错误

错误提示：源安装包启动失败,error while loading shared libraries: libinotifytools.so.0: cannot open shared object file

解决方法：

```shell
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


