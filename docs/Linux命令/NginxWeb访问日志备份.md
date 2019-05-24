#### Nginx Web访问日志备份

```

	#!/bin/bash
	log_path="/home/wwwlogs" #原始日志路径
	baklogs_path="/home/wwwlogs/bak" #备份日志路径
	#备份前的日志文件
	log_name="test.tt.com"
	pid_path="/usr/local/nginx/logs/nginx.pid"
	YESTERDAY=$(date -d "yesterday" +%Y%m%d)
	mv ${log_path}\/${log_name}.log ${baklogs_path}\/${log_name}.bak_${YESTERDAY} #将当天的日志移动备份到备份目录下
	kill -USR1 `cat ${pid_path}`
	
```

###### 注意事项
1. 如果你的原始日志删除之后，重新创建之后，需要重启nginx服务器 `/etc/init.d/nginx restart`
2. kill -USR1 进程号 #通常被用来告知应用程序重载配置文件（平滑启动配置文件）
3. 参考一：[https://blog.csdn.net/fuming0210sc/article/details/50906372](https://blog.csdn.net/fuming0210sc/article/details/50906372)
4. 参考二：[http://www.cnblogs.com/Star-Haitian/p/8981535.html](http://www.cnblogs.com/Star-Haitian/p/8981535.html)