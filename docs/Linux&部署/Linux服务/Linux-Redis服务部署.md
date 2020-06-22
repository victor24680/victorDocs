linux下安装redis及php-redis
<br>以centos6.5为例
#### 安装Redis
<br>选择稳定合适的redis版本
这里选择
- `Redis3.2.8版本`
###### 下载（二选一）
- 命令一：·`wget http://download.redis.io/releases/redis-3.2.8.tar.gz`
- 命令二：`wget https://github.com/victor24680/gitresource/blob/master/redis-3.2.8.tar.gz` 

###### 解压
命令：`tar xzf redis-3.2.8.tar.gz`

###### 编译
`make`
<br>如果提示：缺少gcc编译器，安装gcc即可
- `yum -y install gcc automake autoconf libtool make`
###### 安装
切换至src目录,开始安装 `make install`

###### 启动服务端
```
#需要重新启动配置文件
./redis-server ../redis.conf
```

###### 关闭服务端
```
./redis-cli shutdown
#如果绑定的非本机IP：127.0.0.1-则为：
./redis-cli -h x.x.x.x shutdown

```

###### 启动客户端
```
./redis-cli

127.0.0.1:6379>set "name" "vanlue"
OK
127.0.0.1:6379>get "name"
"value"
#出现如上界面-则表示成功
```
安装Redis-PHP
###### 下载
这里需要根据PHP版本选择适合的php-redis版本
- `PHP5.*`选择2.2.4即可 
- 命令：`wget https://github.com/victor24680/gitresource/blob/master/2.2.4.tar.gz`

###### 解压

```
tar zxvf redis-2.2.5.tgz
```

###### 进入目录执行phpize  (phpize 可通过which phpize查看)

```
cd redis-2.2.5
/usr/local/php/bin/phpize
```

###### 配置(php-config可通过which php-config查看)

```
./configure --with-php-config=/usr/local/php/bin/php-config
```


###### 编译安装

```
make && make install
```

配置php.ini即可
详情，可以参考[http://www.runoob.com/redis/redis-php.html](http://www.runoob.com/redis/redis-php.html)







