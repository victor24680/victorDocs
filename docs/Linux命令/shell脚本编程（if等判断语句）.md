##### 运行方法
```angular2html
#!/bin/sh
#注意判断时的空格
if [ -d 1.txt ];then
    echo "目录"
else
    echo "文件"
fi
    
```
###### 读取屏幕输入内容
`read list #从屏蔽中读取变量list的内容，并保存在list中`

```angular2html
    #!/bin/sh
    echo -n "请输入您的名字"
    read list
    if echo $list | grep "tangqing" > /home/error.log 2>&1
    then
            echo "输入的名字正确"
    else
            echo "请输入正确的名字"
    fi
```
- /dev/null 表示空设备文件
- 0 表示stdin标准输入
- 1 表示stdout标准输出
- 2 表示stderro标准错误
- 详情介绍：[https://www.linuxidc.com/Linux/2016-11/136993.htm](https://www.linuxidc.com/Linux/2016-11/136993.htm)
- $#、$1 等解释:[https://www.cnblogs.com/quxiangxiangtiange/p/10290715.html](https://www.cnblogs.com/quxiangxiangtiange/p/10290715.html)
##### 判断目录是否为指定的目录
```angular2html
    #!/bin/sh
    DIRECTORY=`pwd`
    if [ $DIRECTORY != "/home/sh" ]
    then
            echo "目录不正确"
    else
            echo "目录正确，开始执行中..."
    fi
```

##### 判断运行脚本的用户是否为指定的用户
```angular2html
    #!/bin/sh
    if [ $LOGNAME = "root" ]
    then
            echo "rorr用户"
    else
            echo "非root用户"
    fi
```
##### 询问式的脚本
```angular2html
    #!/bin/sh
    echo "请输入您的名字："
    read YOURNAME
    echo "请问您的名字是否正确："
    read ANY
    if [ $ANY = "y" ] || [ $ANY = “Y” ];then
            echo "请输入的名字是：$YOURNAME"
    else
            echo "您输入的名字错误"
    fi
    # else 接if的写法：elif 条件
```