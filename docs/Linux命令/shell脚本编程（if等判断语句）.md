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
    # 如果不想换行，可以使用pritf 代替echo
```

##### case脚本
```angular2html
    #!/bin/sh
    echo "请输入您的信息："
    read ANS
    case $ANS in
    1) echo "您输入的是 1"
        ;;
        2) echo "您输入的是 2"
        ;;
        *) echo "找不到您输入的数字: $ANS "
        ;;
    esac
```

##### for脚本 循环输出列出的文件清单
```angular2html
    #!/bin/sh
    for loop in `ls /home`
    do
        echo $loop
    done
```
##### until脚本输出
```angular2html
    #非目录时，循环删除。
    until [ ! -d $SELF_PATH ]
    do
        rm -rf $SELF_PATH
    done
    
   # 场景：监控磁盘
   
```
###### while脚本循环
```angular2html
    #!/bin/sh
    while [ $count -lt 5]
    do
        echo "小于5"
    done
    
    #循环计数
    #!/bin/sh
    count_number=10
    while [ $count_number -gt 0 ]
    do
        echo $count_number
        count_number=`expr $count_number - 1`
        #赋值运算的变量不能有空格,
        #即count_number与=之间不能有空格,=与赋值表达式之间也不能有空格
    done
    
    #!/bin/sh
    SEVER_IFS=$IFS
    IFS=:#以冒号分割行数，从三个变量,NAME,DEPT,ID代替访问的数据
    while read NAME DEPT ID
    do
        echo -e "$NAME\t $DEPT\t $ID"
    done < /home/testsh/name.txt #从文件name.txt读入信息
    IFS=$SEVER_IFS #恢复变量IFS设置
    #name.txt文件内容如下：
    victor_1:金融部:1
    victor_2:网络部:2
    victor_3:技术部:3
    victor_4:人力部:4
    
    #常用于从文件中读取数据
```

###### shell 函数处理
```angular2html
    #基本定义一
    function hello()
    {
        函数体
    }
    #基本定义二
    hello()
    {
        函数体
    }
    hello(){
        函数体
    }
    #删除shell函数
    $ unset function_name
```

##### shell脚本参数处理
```angular2html
    #!/bin/sh
    # test.sh
    echo "函数名为：$0 " #输出 test
    echo "脚本的参数列表：$@" #输出 aa bb cc
    while [ $# -ne 0 ]
    do
        echo $1 #依次输出：aa bb cc
        shift #提供偏移帮助，使得上面的'echo $1'无限制，即$1到$9无参数限制
    done

    $ sh test.sh aa bb cc
    # eval 命令：可以获取输入脚本的最后一个参数
```
