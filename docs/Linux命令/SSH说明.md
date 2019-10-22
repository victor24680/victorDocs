#### Linux抓包监听命令
```shell
   tcpdump -i eth0 -nnX port 21 #监听21端口
    -nn 用IP地址的方式监听
    -X 用16进制分析数据
    
```

SSH 加密原理
非对称加密

SSH配置文件说明
###### 位置：/etc/ssh/sshd_config
- Port 22 端口（默认是22端口）
- ListenAddress 0.0.0.0 监听的IP地址
- Protocol 2            SSH版本选择
- HostKey /etc/ssh/ssh_host_rsa_key 私钥保存位置
- ServerKeyBits 1024 私钥的位数
- SyslongFacility AUTH 日志记录SSH登录情况
- LogLevel INFO 日志等级
- GSSAPIAuthentication yes GSSAPI认证开启 （默认是开启的）
- PermitRootLogin yes 允许root登录

- PubkeyAuthentication yes 是否使用公钥验证
- AuthorizedKeyFile .ssh/authorized_keys 公钥的保存位置

- PasswordAuthentication yes 允许使用密码验证登录
- PermitEmptyPasswords no 不允许空密码登录

- 服务端主程序：/usr/sbin/sshd
- 客户端程序：/usr/bin/ssh
- SSH连接命令：ssh root@192.168.0.0，
- SFTP连接命令：sftp root@192.168.0.0.1
```angular2html
    #过默认端口部位默认端口，则需要附带端口,如下：
    sftp -P 21302 root@192.168.0.0
    #相关命令
    -ls 查看服务端数据
    -cd 切换服务端目录
    -lls 查看本地数据
    -lcd 切换本地目录
    -get 下载
    -put 上传
```
- 如果需要相对安全，可以使用私钥对验证