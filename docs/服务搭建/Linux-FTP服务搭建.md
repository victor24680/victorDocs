#### Linux FTP服务搭建 【基于Centos v6.5】
##### 1.安装ftp服务
`yum install -y  vsftpd`

##### 2.启动服务,重启服务,监听，禁用匿名登录
```
service vsftpd start #启动服务
service vsftp restart #重启服务
netstat -nltp | grep 21 #查看监听端口

#【禁止匿名登录】打开配置文件 /etc/vsftpd/vsftpd.conf
# 禁止匿名登录
anonymous_enable=NO #默认YES，改为NO
#禁止切换目录
chroot_local_user=YES #默认是不开启的。
```
##### 3.创建ftp用户，设置密码
```
useradd ftpuser #创建ftp用户
echo "root" | passwd ftpuser --stdin #创建密码
```

#####  4.限制用户仅能通过FTP访问

- 限制用户ftpuser只能通过FTP访问服务器，不能直接登录服务器

`usermod -s /sbin/nologin ftpuser`

##### 5.设置用户上传目录
```
#设置访问权限
chmod a-w /home/wwwroot/ftpimg.mytp.com && chmod 777 -R /home/wwwroot/ftpimg.mytp.com/Uploads
#设置主目录
usermod -d /home/wwwroot/ftpimg.mytp.com ftpuser
```
##### 6.访问FTP设置
`setenforce 0 #关闭SELinux服务`

`iptables -F #关闭访问墙【建议开启防护墙，开放FTP默认21端口】`

- Windows 可以尝试测试下：ftp://ip地址
- Linux 可用用lftp命令连接

###### 参考地址
 
- 基于Centos7搭建的FTP服务
1. [https://www.cnblogs.com/jefflee168/p/6575014.html](https://www.cnblogs.com/jefflee168/p/6575014.html) 
2. [https://www.linuxidc.com/Linux/2017-11/148518.htm](https://www.linuxidc.com/Linux/2017-11/148518.htm)

