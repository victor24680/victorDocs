####  window下安装redis及php-redis
##### redis安装

- 下载redis 下载：[https://pan.baidu.com/s/1JjK1gcNaSOgcC7RYqjVwkQ](https://pan.baidu.com/s/1JjK1gcNaSOgcC7RYqjVwkQ)（个人打包的redis-3.2.10版本）
- 直接解压-免安装，如：D:\Redis
- 打开控制台，切换至Redis
- 开启服务:`redis-server.exe redis.window.conf`
- 客户端运行,直接运行redis-cli.exe，出现如下命令台
```
127.0.0.1:6379>set "name" "vanlue"
OK
127.0.0.1:6379>get "name"
"value"
#出现如上界面-则表示成功
```
##### redis扩展（php-redis）安装
先查看自己的PHP版本,根据参数“Architecture”（phpinfo()函数可以查看） 
- 下载合适的php-redis版本
- 下载：[https://pan.baidu.com/s/1PSZ1NxXIVi7p4dFCkhK0GA](https://pan.baidu.com/s/1PSZ1NxXIVi7p4dFCkhK0GA) (个人打包的x86版本)
- 原始地址：[下载](https://windows.php.net/downloads/pecl/releases/redis/2.2.7/)
 <br>备注：根据自己的PHP版本位数下载,否则不能使用


打开php.ini配置文件，添加如下扩展
- extension=php_igbinary.dll
- extension=php_redis.dll
<br>备注：php_igbinary.dll 一定在php_redis.dll前面

重启Apache获取Nginx服务即可

#####  redis测试

```
$redis = new \Redis();
$redis->connect('127.0.0.1','6379');
#$redis->auth('foobared');//如果开启了授权这需要此操作，否则忽略
echo "Connection to server sucessfully<br>";
//设置 redis 字符串数据
$redis->set("my_name", "This is a redis connect!");
// 获取存储的数据并输出
echo "Stored string in redis：" . $redis->get("my_name");

输出结果
Connection to server sucessfully
Stored string in redis：This is a redis connect!
```



