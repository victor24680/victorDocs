#### linux下备份MySQL
- 思路

	1. 查找以前的本分的db备份文件，清除以前的（可删除，可转移）
	2. 生成备份的文件名
	3. 打包压缩
	

```
	
	#!/bin/sh
	find /home/bkdb_testdb/ -name "wxjxqc*" -mtime +15 -exec rm -f {} \;
	filepath=/home/bkdb_testdb/
	filename=testdb_`date +%Y%m%d_%H.%M.gz`
	mysqldump -u'root' -p'653421' -R  wxjxqc --skip-lock-tables|gzip>$filepath$filename
	chmod -rx,o-rx /home/bkdb_testdb/* #去除文件的读和执行的权限

	
	#从备份文件中导出所需的单个表的数据，利用
	#mkdir bkdb_testdb
	#cd bkdb_testdb
	#most=tb_***table
	#gzip -d  dbtest_***.gz
	#cat dbtest_2018***  | grep "INSERT INTO \`tb_***\`" > tb_***table.sql #刷选相应的数据，并执行输入到指定文件中。
	#再进mysql -uroot -p
	#mysql> source /**/***/tb_***table.sql
	

```

- find详解：[https://www.cnblogs.com/Ido-911/p/9638612.html](https://www.cnblogs.com/Ido-911/p/9638612.html)