Centos 6.5下安装SFTP服务器；

##### 配置用户sftp
- 新增用户组 sftp
```shell
[root@TestWX ~]# groupadd sftp
```
- 新增用户sftp_www,属于sftp组，且禁止登陆远程终端；
```shell
[root@TestWX ~]# useradd -g sftp -s /sbin/nologin -M sftp_www
```
- 设置新增sftp_www用户的密码
```shell
[root@TestWX ~]# passwd sftp_www #设置密码
Changing password for user sftp_www
New password: 123123! #输入密码
Retype new password: 123123! #再次输入密码
passwd: all authentication tokens updated successfully

#如果浏览自己创建的用户信息
# cat /etc/passwd 
```
- 创建 sftp_www 存放文件的目录
```shell
[root@TestWX ~]# mkdir /home/sftp/sftp_www -p #-p：表示需要时创建上层目录
```
- 修改此目录拥有者
```shell
#多个用户访问此目录时，设置的目录权限及其所有的上级文件权限，属主和属组必须是root组
[root@TestWX ~]# chown -R sftp_www:sftp /home/sftp/sftp_www/ #改变目录拥有者
```
- 设置，用户登录后的默认目录
```
# usermod -d /tmp test (test为你的用户名) 这种改法是无效的,只能通过修改配置文件

[root@TestWX ~]# vim /etc/ssh/sshd_config
#Subsystem sftp /usr/libexec/openssh/sftp-server #此行需要注释掉，然后添加下面配置。
Subsystem sftp internal-sftp
X11Forwarding no
AllowTcpForwarding no
#ForceCommand internal-sftp #限制root的ssh登录，使用时请谨慎，以防止自己的root账户不能登录；
#Match User sftp_www #限制的用户（如果指定限制用户时，存在其他sftp用户，则其他用户的根目录为系统目录,这样不安全）。
Match Group sftp #（多个sftp用户时，请使用此项配置，即限制的用户组，而不是限制的用户）
ChrootDirectory /home/sftp/sftp_www     #用户的根目录（也可以说是sftp用户组的用户登录目录）

###sftp用户组的其他用户登录时，即登录的根目录为：/home/sftp/sftp_www

#目录权限遵循以下两点
#1. 设置的目录权限及其所有的上级文件夹权限，属主和属组必须是root；（如：/home/sftp/sftp_www/）
#2. 设置的目录权限及其所有的上级文件夹权限，只有属主能拥有写入权限，即最大设置为755

#UseDNS no #用户主机IP检测，防止客户端欺骗,可以加速SSH登录，有的不注释时，重启会报错。

```
- 重启SSH服务
```shell
[root@TestWX ~]# service sshd restart
```


备注：大致可以配置如下的目录及权限
```shell
[root@xdl home]# ll | grep sftp
drwxr-xr-x 3 root root 4096 Sep 18 14:49 sftp（755权限）

[root@xdl home]# cd sftp 

[root@xdl sftp]# ll
total 4
drwxr-xr-x 3 root root 4096 Sep 18 16:26 sftp_www（755权限）

[root@xdl sftp]# cd sftp_www/ && ll
total 4
drwxr-xr-x 2 sftp_www sftp 4096 Sep 18 15:16 history （755权限）

```