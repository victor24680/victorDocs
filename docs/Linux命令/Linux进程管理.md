### screen 命令管理
- 可以把正在进行的进程任务放置后台管理
```angular2html
    screen -S test #test-窗口名称
    echo "112" #正在工作的任务；
    ctr+A+D #把工作的任务放置后台，然后显示:[detached]
    screen -ls #显示正在工作的进程
    screen -r 进程号 #恢复上面正在工作的任务，即回到之前的工作窗口
```
### 服务（功能）
- 系统服务

- 网络服务

### 服务（方法）
- 独立系统服务

- 临时服务

#### 网络进程服务：DNS客户机配置文件
`/etc/resolve.conf `
#### chkconfig 改变服务
