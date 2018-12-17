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
##### 撤销一个合并

```
git reset --hard HEAD^
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










