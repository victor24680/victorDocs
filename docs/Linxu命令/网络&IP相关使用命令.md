#### IP相关命令
```

	ip addr 查看IP相关信息
	ifconfig 查看IP相关信息
```

#### 网络文件传输命令-scp


```

	#scp secure copy的简写
	#1.将备份文件远程传输到指定目录
	#注意：用户账户对指定的目录必须有写入的权限
	scp testdba.sql 用户名@192.168.66.143:/root/   #testdba.sql-传输的文件名

	#2.将远程文件远程传输到本地指定目录
	scp 用户名@192.168.66.143:/root/testdba.sql /home/ 
	选项配置scp 【选项】
	-r 递归复制整个目录
	-p 保留源文件的修改时间，访问时间和访问权限
	-P 大写的P，传输端口号；
	-q 不显示进度条
	-v 详细方式显示输出
	-1 强制scp命令使用协议ssh1
	-2 强制scp命令使用协议ssh2
	-C 允许压缩（将-C标志传递给ssh1，从而打开压缩功能）
	-l 限定用户所使用的带款，以kbit/s为单位
	-B 使用批处理模式（传输过程中不询问传输口令或短语）
	-c cipher 以cipher将数据传输进行加密，这个选项将直接传给ssh
	-i identity_file  从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。 
	-S program  指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。
```

#### 防护墙相关命令 iptables

```
	
	vim /etc/sysconfig/iptables #查看防护墙命令
	service iptables restart #防护墙重启

	#白名单添加
	:whitelist - [0:0]   #先添加这行，表示加了一类为白名单
	-A whitelist -s *.*.*.*/32 -j ACCEPT #表示添加了白名单

```

