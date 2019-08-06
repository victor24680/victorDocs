### Linux常用命令管理
#### find-命令使用
###### 语法规则
```
#语法 
find path -option [ -print]
#path 查找路径
#-option 选项
#-print 将匹配的文件匹配到标准输出
```
###### 选项规则

```
#按文件名查找
find -name 1.txt

#按执行权限查找 后面接权限模式【很少用】
find -perm 750

#按文件属性查找
find -user username

#按组查找
find -group groupname

#按文件更改时间来查找文件 ，-n指n天以内，+n指n天以前
find -mtime -n +n 
#按文件访问时间来查找,参数意义同上
find -atime -n  +n 
#按文件创建时间来查找,参数意义同上
find -ctime -n +n 
#查更改时间比f1新但比f2旧的文件
find -f1 !f2

#查无有效属组的文件，即文件的属组在/etc/groups中不存在
find -nogroup
#查无有效属主的文件，即文件的属主在/etc/passwd中不存在

#查找是块设备，目录，字符设备，管道，符号链接，普通文件
find -type b/d/c/p/l/f
#查找长度为n[字节,块,千,兆,吉]的文件
find -size n[c,b,k,M,G]
#介于大小之间：如10M-100M,find -size +10M -100M

#使查找在进入子目录前先行查找完本目录【待核查】,递归子目录查找。
find -depth  


#查找某一类型文件系统中的文件 这些文件系统类型通常可在/etc/fstab中找到【待核查】
find -fstype
#查找文件时，不跨越文件系统mount点【待核查】(即不进入其他文件系统查找)
find -mount
#如果遇到到链接符号文件，就跟踪所指的目标文件【待核查】
find -follow
#对匹配的文件使用cpio命令,将找到的文件备份到磁盘设备中【待核查】

#忽略某个文件
find -prune 忽略某个目录【待核查】

#查找，并执行某个文件后
find -name 1.sh -exec sh 1.sh {} \;

#后面的{} \;不能丢；

#查找，并以安全模式去执行(询问的方式去执行)
find -name 1.sh -ok sh 1.sh {} \;


locate -i #忽略大小写差异
locate -r #后面可接正规表示法的显示方法
locate passwd #找出系统中与passwd相关的文档。

whereis -b 只找binary格式的文件
whereis -m 只找在说明manual路径下的文件
whereis -s 只找source 来源文件
whereis -u 搜寻不在上述三个项目当中的其他特殊文件。

```

