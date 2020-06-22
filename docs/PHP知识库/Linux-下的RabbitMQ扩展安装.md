## 简介
RabbitMQ的协议AMQP(因为扩展为C语言编写，AMQP依赖librabbitmq库)与RbbitMQ通信时，需要安装依赖库rabbitmq-c库
## 官方安装方法
- [https://www.rabbitmq.com/tutorials/tutorial-one-php.html](https://www.rabbitmq.com/tutorials/tutorial-one-php.html)

## 模式讲解
[https://www.cnblogs.com/Jeely/p/10784013.html](https://www.cnblogs.com/Jeely/p/10784013.html)

## 安装`rabbitmq-c`
- 下载地址：[https://github.com/alanxz/rabbitmq-c](https://github.com/alanxz/rabbitmq-c)
- 安装命令
```shell
[root@testcentos rabbitmq]# git clone https://github.com/alanxz/rabbitmq-c.git  // 下载
//在rabbitmq-c目录中创建一个build目录
[root@testcentos rabbitmq]# cd rabbitmq-c && mkdir build && cd build
//让cmake根据CMakeList.txt 创建Makefile文件，Makefile文件会被创建在build目录中。注意：请勿漏洞 '..'
// /usr/local/rabbitmq-c 指定rabbitmq-c 安装目录
[root@testcentos build]# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/rabbitmq-c ..
//build安装rabbitmq-c库。注意：请勿漏掉 '.'
[root@testcentos build]# cmake --build .  --target install 
```

- 备注
因为目录/usr/local/rabbitmq-c下只有include和lib64.后面编译安装amqp扩展时，<br />
系统会在/usr/local/rabbitmq-c/lib目录下搜索依赖库，导致出错。无需要添加如下命令:</br>
`[root@testcentos build]# cd /usr/local/rabbitmq-c && ln -s lib64 lib`
目的：让lib64目录和lib目录建立链接。（类似Window中的快捷键）

## 安装`php`的`AMQP`扩展
- 官方下载地址：[http://pecl.php.net/package/amqp](http://pecl.php.net/package/amqp)
```shell
[root@testcentos rabbitmq]# wget http://pecl.php.net/get/amqp-1.9.3.tgz
[root@testcentos rabbitmq]# tar -zxvf amqp-1.9.3.tgz && cd amqp-1.9.3
# 根据当前php版本动态床架扩展configure可执行文件
[root@testcentos amqp-1.9.3]# phpize 
#请配置好对应的目录：php-config 目录 和 以上安装的 rabbitmq-c 目录
[root@testcentos amqp-1.9.3]# ./configure --with-php-config=/usr/local/php/bin/php-config --with-amqp --with-librabbitmq-dir=/usr/local/rabbitmq-c
[root@testcentos amqp-1.9.3]# make && make intall
```
## 配置php.ini文件及开启amqp.so模块
```shell
vim /usr/local/php/etc/php.ini
#添加 extension=amqp.so
#重启服务.
```
- 参考：[https://www.jianshu.com/p/31680a703c2f](https://www.jianshu.com/p/31680a703c2f)

## 测试
- 查看：可以通过phpinfo函数查看。
- 测试代码
```php
#config.php
$config=[
   'host'     => '127.0.0.1',
   'login'    => 'guest',
   'password' => 'guest',
   'port'     => 5672,
   'vhost'    => '/',
];
#/home/wwwroot/default/receive.php #消费端
$e_name  = 'e_change_1'; //交换机名
$q_name  = 'q_queue_1'; //队列名
$k_route = 'key_1'; //路由key
try{
   $connect = new \AMQPConnection($config);
   if(!$connect->connect()){
        exit('连接异常');
   }
   $channel = new \AMQPChannel($connect);

   //创建交换机
   $ex = new \AMQPExchange($channel);
   $ex->setName($e_name);
   $ex->setType(AMQP_EX_TYPE_DIRECT);
   $ex->setFlags(AMQP_DURABLE);
   echo "Exchange Status:".$ex->declare()."\n";

   //创建队列
   $queue = new \AMQPQueue($channel);
   $queue->setName($q_name);
   $queue->setFlags(AMQP_DURABLE);
   echo "Queue Bind:".$queue->declare()."\n";

   //绑定交换机与队列，并指定路由
   echo 'Queue Bind: ' . $queue->bind($e_name, $k_route) . "\n";

   //阻塞模式接收消息
   echo "Message: \n";
   while (true) { //可用可不用
      $queue->consume('processRabbitReceive');
   }
   $connect->disconnect();//关闭连接
}catch(Exception $e ){
  echo $e->getMessage();
}

//消息接收函数
function processRabbitReceive($envelop, $queue)
{
   $msg = $envelop->getBody();
   echo $msg . "\n";
   $queue->ack($envelop->getDeliveryTag());//应答模式
}

==============================
#生产端
$e_name  = 'e_change_1'; //交换机名
$q_name  = 'q_queue_1'; //队列名
$k_route = 'key_1'; //路由key

//创建连接和channel 
$conn = new \AMQPConnection($config);
if (!$conn->connect()) {
    exit("连接异常!\n");
}
$channel = new \AMQPChannel($conn);

//创建交换机对象    
$ex = new \AMQPExchange($channel);
$ex->setName($e_name);

//发送消息 
//$channel->startTransaction(); //开始事务  
for($i=0; $i<5; ++$i){
    sleep(1);//休眠1秒
    $message = "TEST MESSAGE!".date('Y-m-d H:i:s').'|isso:'.$i;
    echo "Send Message:".$ex->publish($message, $k_route)."\n";
}
//$channel->commitTransaction(); //提交事务 

$conn->disconnect();
```
## 守护进程监控`supervisor`
- supervisor
- 参考地址：[https://blog.csdn.net/qq_37837134/article/details/89407123](https://blog.csdn.net/qq_37837134/article/details/89407123)


