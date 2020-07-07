基于Linux版本-Centos7.x
# 安装
```shell
yum -y update

yum install docker -y
```
# 启动
- 方式一：`systemctl start docker.service`
- 方式二：`service docker start`
- 如果看到 `Active: active(running)`则启动成功
# 停止
`systemctl stop docker.service`
# 查看
`systemctl status docker.service`
# 重启
`systemctl restart docker.service`
# 配置镜像加速
```shell
vim /etc/docker/daemon.json
#写入如下内容
{
  "registry-mirrors": ["https://hvmf8r55.mirror.aliyuncs.com"]
}
#保存退出
```
# 拉取镜像
`docker pull ubuntu:13.10`
- 如果本机上不存在此镜像，docker会自动下载此镜像
- 镜像搜索地址:[https://hub.docker.com/](https://hub.docker.com/)
# 搜索镜像
`docker search ubuntu`
# 查看本地所有镜像
```shell
[root@instance-0de00bia docker]# docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
how2j/tmall             now                 b5665202616f        2 weeks ago         1.36 GB
docker.io/centos        centos7             b5b4d78bc90c        4 weeks ago         203 MB
docker.io/hello-world   latest              bf756fb1ae65        5 months ago        13.3 kB
centos_ssl              v1                  bbd839ae58fd        5 months ago        406 MB
centos                  v4                  d633bd4f9d5c        8 months ago        394 MB
docker.io/how2j/tmall   latest              18d90c918965        15 months ago       1.23 GB
```
# 删除镜像
`docker rmi docker.io/hello-world`
- 删除镜像时，如果有对应的容器在运行时，必须先删除容器，才能删除镜像
# 镜像标记
`docker tag  609d7c88aac5(原始镜像ID号) docker.io/myhello-world` 
# 删除所有镜像
`docker rmi $(docker images -q) #请谨慎使用`

# 容器创建
所谓容器创建，即运行一个镜像，即为容器创建。
`docker run -dit --privileged -p21:21 -p80:80 -p8080:8080 -p30000-30010:30000-30010 --name how2jtmall how2j/tmall:latest /usr/sbin/init`<br/>
参数解析
- -dit是-d -i -t的缩写.-d，表示detach，即在后台运行。-i表示提供交互接口，这样才可以通过docker和跑起来的操作系统交互。-t 表示提供一个tty(伪终端),与-i配合就可以通过ssh工具连接到这个容器里面去。
- --privileged 表示启动容器的时候，把权限带进去，这样可以在容器中进行完整的操作
- -p21:21 第一个21，表示Linux系统上开放21端口.第二个21表示在容器里开放21端口,当访问Linux系统21端口的时候，会间接访问到容器里了。
- -name how2jtmall 表示给起个名字，方便管理
- how2j/tmall:latest 镜像名:版本号（最新版本）（请放置在最后） 否则会导致容器启动失败：
- /usr/sbin/init 表示启动后台运行的程序，即通过这个命令初始化
# 进入创建的容器
`docker exec -it how2jtmall /bin/bash`
- v 目录挂载：`-v /home/nginx/www:/usr/share/nginx/html` 表示 把宿主机中的www目录挂载到容器中的html目录中

# 查看容器
- `docker ps` 查看在运行的容器
- `docker ps -a` 查看所有容器
# 容器生命周期
- 暂停`docker pause how2j/tmall`
- 恢复`docker unpause how2j/tmall`
- 停止`docker stop how2j/tmall`
- 开始`docker start how2j/tmall`
# 删除容器
- `docker rm how2j/tmall`
- docker rm `docker ps -a -q` -f 删除所有容器
# 制作新的镜像
```shell
docker commit -m="描述信息" -a="作者" how2jtmall how2j/tmall:now
how2jtmall 原始容器名（可以是容器ID号）
how2j/tmall:now 目的容器名
```

# 导入导出*迁移备份
## save 
- 备份命令：从镜像中备份（可以保留镜像历史信息）
- `docker save -o/> xxx.tar xxx:latest`
- -o和>表示输出到文件xxx.tar(目标文件)，xxx:latest是源镜像名(name:tag)

## load
- 加载备份文件：加载程镜像文件
- `docker load -i/< xxx.tar`
- -i和<表示从文件输入.会导入镜像及相关元数据，包括tag信息

## export
- 导出命令：从运行的容器中备份到目标文件(无法保留镜像所有历史信息)
- `docker export -o xxx-test.tar xxx-test`
- -o表示输出到文件,xxx-test.tar 为目标文件 xxx-test为源容器名（name）

## import
- 导入命令
- `docker import xxx-test.tar xxx:imp`
## 建议
- 备份镜像：用save、load即可
- 备份容器：用export、import（可以增量备份命令）
## 查看镜像历史
`docker history xxx:latest`

- 参考：[https://www.runoob.com/docker/docker-container-connection.html](https://www.runoob.com/docker/docker-container-connection.html)
- 参考挂载信息：[https://blog.csdn.net/weixin_37773766/article/details/80702926](https://blog.csdn.net/weixin_37773766/article/details/80702926)
- PHP-NGIX容器中整合：[https://www.runoob.com/docker/docker-install-php.html](https://www.runoob.com/docker/docker-install-php.html)
- Docker命令集合：[https://www.runoob.com/docker/docker-command-manual.html](https://www.runoob.com/docker/docker-command-manual.html)