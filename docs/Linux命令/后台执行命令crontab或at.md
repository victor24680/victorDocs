 ####### at命令
 - 向linux提交命令或脚本
 ```
    $at 11:00
    at> find -name /home/1.txt -exec cat > /home/2.txt {} \;
    按ctr+d 退出即可，
    
    #列出所有的作业
    $at -l
    #清除作业
    atrm job 1
    
    # & 改命令用来把命令放到后台执行，不再占用终端。 
    
    
 ```