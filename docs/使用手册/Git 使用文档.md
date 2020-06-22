# git初始化配置

```
git config --global user.name "victorxxxx
git config --global user.email 490319xxx@qq.com
```
# 初始化一个仓库

```
#1.克隆一个仓库(里面已经包含远程仓库地址)
git clone https://git.hub.cn/victorxxx.git
#2.本地初始化（不包含远程仓库地址）
git init

```
# git 工作流程

#### 添加新的文件

```
#添加缓存区
git add 1.txt
#查看状态
git status
#进入缓存文件差异界面
git diff --cached //【查看缓存区哪些文件被修改】可以直接按q 退出
```
##### 提交内容

```
git commit -a -m "初始化文件"
#-a 不会添加新的文件
#-m 提交你的注释
```
##### 远程仓库管理

```
#关联远程仓库（添加远程仓库）
git remote add origin https://git.hub.cn/victorxxxx.git 
#查看信息
git remote -v
#移除远程仓库
git remote rm origin 
```
##### 推送新的内容到远程分支

```
git push origin master //(master-代表当前主分支)
```
# 分支管理
##### 创建分支

```
#创建一个新的分支
git branch branch_name
#查看当前的工作在哪个分支上
git branch
```
##### 切换分支

```
git checkout branch_name
```

##### 合并分支

```
#切换到主分支
git checkout master
git merge -m "合并其他分支到主分支" branch_name
//合并分支时，可能会有冲突，手动解决一下冲突
```
##### 删除分支

```
git branch -d branch_name
```
##### 撤销/回滚历史操作

```
#撤销操作
git checkout -- <filename> 放弃文件的当前更改，回到最近一次的提交状态
git reset HEAD <filename>，取消暂存文件
git commit --amend ，覆盖上一次的提交，虽然不是撤销操作，但有类似的效果
git rebase -abort 撤销一次rebase

#回滚历史
git reset --hard HEAD^ 回滚到上个版本(^和~1等价，^^和~2等价，^-2和~3等价，依次类推)
git reset --hard <SHA-1> 回滚到指定版本号，如果是版本号的前几位，git会自动寻找匹配的版本号
git reset --hard~2,回到前两个版本
git reset --hard <tag> 回到指定标签的版本
git reset --hard <SHA-1><filename> 回到某个文件到指定版本号
#撤销历史
git revert HEAD 撤销最近一次的提交，并作为一次新的提交
git revert HEAD~1 撤销最近两次的提交，并作为一次新的提交
git revert <SHA-1> 撤销指定版本的提交，并作为一次新的提交
```
# 日志管理
##### 查看日志
```
#查看日志
git log
#查看帮助命令
git help log
git log v2.5.. Makefile。。
```
##### 日志统计

```
#会显示在每个提交中哪些文件被修改，即打印详细修改信息
git log --sat
```
##### 格式化日志

```
git log --pretty=oneline //每条日志输出一行
#--pretty 可以查看若干表现形式
git log --graph --pretty=oneline
#--graph 可以格式化日志记录（可视化）
#日志排序
```
# 忽略文件提交失效
- 当忽略文件还是提交时，需要手动清除缓存
```shell
--cached              仅仅移除索引
-f, --force           覆盖最新检查文件
-r                    允许清理递归文件
--ignore-unmatch      无法匹配时，零状态退出
#【指定目录或文件清理-示例】
git rm -r --cached  文件
```

# Cento服务器上搭建git服务器【GIT服务-延伸】
+ 安装
 1. Centos `yum -y install git` 
 2. Ubuton `sudo apt-get install git`
+ 创建git服务-账号密码

```
	useradd git #创建用户名
	passwd git #为git服务创建密码
```

+ Linux上创建git远程仓库

```
	cd /home/GIT
	git init --bare test.git #创建一个远程仓库
	chown -R git:git test.git/ #改变远程仓库的目录权限
```

+ 客户端克隆一个远程仓库【和前面介绍的一样】
```
    #克隆已有仓库
	git clone git@192.168.66.142:/home/GIT/test.git
	# 关联远程仓库
	git remote add origin git@192.168.66.142:/home/GIT/test.git
	#因为git是与ssh挂钩，如果ssh端口号已变更，则使用如下命令：
	git clone ssh://git@ip地址:61622/home/GitsCode/qjsweb.git
	
```
+ 禁止git用户远程登录
```shell
usermod -s /bin/false username #没有提示
usermod -s /usr/sbin/nologin git #有提示
 ```
 1. 参考网址:[https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)
 2. 参考:[https://git-reference.readthedocs.io/zh_CN/latest/](https://git-reference.readthedocs.io/zh_CN/latest/) 
 3. 参考二:[http://www.ruanyifeng.com/blog/2014/06/git_remote.html](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)
 4. 参考三:[https://www.cnblogs.com/gbyukg/archive/2011/12/02/2271372.html](https://www.cnblogs.com/gbyukg/archive/2011/12/02/2271372.html)
 5. 仓库关联：[https://www.iteye.com/blog/eksliang-2249472](https://www.iteye.com/blog/eksliang-2249472)





