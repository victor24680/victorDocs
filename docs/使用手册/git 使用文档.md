#### git初始化配置

```
git config --global user.name "victorxxxx
git config --global user.email 490319xxx@qq.com
```
### 初始化一个仓库

```
#1.克隆一个仓库(里面已经包含远程仓库地址)
git clone https://git.hub.cn/victorxxx.git
#2.本地初始化（不包含远程仓库地址）
git init

```
### git 工作流程

#### 添加新的文件

```
#添加缓存区
git add 1.txt
#查看状态
git status
#进入缓存文件差异界面
git diff --cached //【查看缓存区哪些文件被修改】可以直接按q 退出

```
#### 提交内容

```
git commit -a -m "初始化文件"
#-a 不会添加新的文件
#-m 提交你的注释
```
#### 添加远程仓库

```
git remote add origin https://git.hub.cn/victorxxxx.git 
```
#### 推送新的内容到远程分支

```
git push origin master //(master-代表当前主分支)
```
### 分支管理
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
### 日志管理
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

# Cento服务器上搭建git服务【GIT服务-延伸】
+ 安装 `yum -y install git`
+ 创建git服务-账号密码

```

		useradd git #创建用户名
		passwd git #为git服务创建密码
```

+ 创建git仓库

```
	
	git init --bare /home/GIT/test.git #创建一个远程仓库
	chown -R git:git test.git/ #为服务远程服务仓库赋予权限
	
```

+ 客户端克隆一个远程仓库【和前面介绍的一样】

```

	git clone git@192.168.66.142:/home/GIT/test.git
	
```











