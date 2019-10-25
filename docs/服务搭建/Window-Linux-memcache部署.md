####  window下安装memcache及php-memcache
##### memcache安装
- 下载：[http://static.runoob.com/download/memcached-win64-1.4.4-14.zip](http://static.runoob.com/download/memcached-win64-1.4.4-14.zip)
- 安装参考方法：[https://www.runoob.com/memcached/window-install-memcached.html](https://www.runoob.com/memcached/window-install-memcached.html)
```php
    telnet 127.0.0.1 11211
    按ctr+]，然后按回车键，
    set name 0 0 3
    tang
    STORED
    #出现如上界面-则表示成功
```
##### memcache扩展（php-memcache）安装
- 下载合适的php-memcache版本
- 下载：[ http://pecl.php.net/package/memcache/3.0.8/windows]( http://pecl.php.net/package/memcache/3.0.8/windows) 
- 参考地址：[https://blog.csdn.net/okokchina/article/details/46679305](https://blog.csdn.net/okokchina/article/details/46679305)
 <br>备注：根据自己的PHP版本,编译位数等下载,否则不能使用
- 打开phpinfo()函数检查如下三个参数:
1. php版本
2. php的编译器版本  比如现在最新的msvc11
3. php是不是nts，或者ts版本 

- 把php_memcache.pdb，php_memcache.dll，放置ext目录下：
- 打开php.ini配置文件，添加如下扩展
- extension=php_memcache.dll

重启Apache或者Nginx服务即可

#####  memcahe测试
```php
<?php
    $memcache = new Memcache;             //创建一个memcache对象
    $memcache->connect('localhost', 11211) or die ("Could not connect"); //连接Memcached服务器
    $memcache->set('key', 'test');        //设置一个变量到内存中，名称是key 值是test
    $get_value = $memcache->get('key');   //从内存中取出key的值
    echo $get_value;
 ?>
```
#### Linux下安装
安装方法：[https://www.runoob.com/memcached/memcached-install.html](https://www.runoob.com/memcached/memcached-install.html)

#### Linux下php-memcache扩展安装
手动编译PHP的各种扩展安装，大致流程如下（以memcache为例）
- 下载官方编译源码(stable-稳定包)：[http://pecl.php.net/package/memcache](http://pecl.php.net/package/memcache)
```shell
    #下载.解压
    wget http://pecl.php.net/get/memcache-2.2.7.tgz               
    tar -zxvf memcache-2.2.7.tgz
    cd memcache-2.2.7
```
- 进入解压的目录开始如下草
```shell
    #几乎所有的PHP扩展都会通过1.2方法。
    #1.根据当前的PHP版本动态的创建扩展的configure文件(phpize：动态执行的工具)
    /usr/local/php/bin/phpize  --with-php-config=/usr/local/php/bin/php-config
    #2.检查配置文件
    ./configure --with-php-config=/usr/local/php/bin/php-config
    #编译安装
    make && make install
    #修改php配置文件（php.ini，如果不知道路径，直接通过phpinfo函数查找即可）
    #/usr/local/php/lib/php/extensions/no-debug-non-zts-20090626/，此路径也可以根据phpinfo查找。
    extension = /usr/local/php/lib/php/extensions/no-debug-non-zts-20090626/memcache.so
    #重启php
    /etc/init.d/php restart
    #检查安装:通过phpinfo查看即可
```

