##### 运行级别

- 确定当前运行级别：`who -r #run-level 4 Apr 22 13:26 ,数字为运行级别，时间问最后一次重启的时间`
- 运行级别的目录：/etc/rdN.d或/etc/rd.d/rcN.d目录下。

        运行级别0 启动和停止整个系统
        运行级别1 单用户或管理模式
        运行级别2 多用户模式；部分网络服务被启动。有些系统将其作为正常运行模式，而不是级别 3
        运行级别3 正常操作运行模式，启动所有的网络服务
        运行级别4 用户定义的模式，可以使用该级别来定制所需要运行的服务
        运行级别5 有些 UNIX 操作系统变体将其作为缺省 X-windows模式，还有些系统把它作为系统维护模式
        运行级别6 重启动
        
- 在启动系统的过程中，将会启动一个名为init的进程，此进程为所有进程的祖先。看看启动哪些服务，应当缺省地进入哪一个运行级别。通过查看一个名为inittab的配置文件来获得上述信息，该配置文件位于/etc/目录下。

- 安装运行级别的脚本，放置在 /etc/init.d或/usr/sbin/init.d或/etc/rc.d中，都可以运行。

- inittable 文件所包含的域具有严格的格式
```angular2html
    i:rstart:action:processs
    #id域，相应的唯一标识。
    #rstart域，包含的数字表示运行该进程的级别
    #action域，告诉init进程如何对待process所对应的进程。常用的两个动作：wait和respawn.
    wait当进程启动后等待它结束，respawn则意味着如果进程不存在，则启动相应的进程，如果它存在，只要它一掉下来，就立即重新启动它。
```