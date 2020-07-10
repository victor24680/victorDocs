## 临时表使用规则
- 官网：[https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html](https://dev.mysql.com/doc/refman/8.0/en/internal-temporary-tables.html)<br />

#### 说明
1. 临时表一般存储在内存中和磁盘中；
1. 在处理请求的某些应用场景中，服务器会创建内部临时表，即表引擎[MEMORY]引擎在内存中处理；
2. MyISAM引擎在存储在磁盘上处理临时表；
3. 如果表过大，服务器可能会把内存中的临时表转存在磁盘上；
4. 以上都是被动的产生，**用户**是不能主动直接控制服务器内部用内存还是磁盘存储临时表。

#### 产生临时表的案例
- 如果`group by`的列没有索引，必产生内部临时表
```mysql
#通过explain查看，而且是全表扫描
type: ALL #使用全表扫描
Extra: Using temporary; Using filesort
# Using filesort：表示在索引之外，需要额外进行外部的排序动作
```
- 如果`order by`与`group by` 为不同列时
```mysql
#不同列的情况
type: Index #
Extra: Using temporary 
```
- 如果`distinct`与`order by` 一起使用可能产临时表（不同字段）
#### 临时表配置分析
- 如果使用强制性操作：`SQL_BUFFER_RESULT` 会使用内存临时表，除非必须要把临时表建立在磁盘上；
- 内存临时表过大，会自动转化为磁盘临时表，最大值设置为配置文件中的tmp_table_size,最小值max_heap_size
- crate table时指定的内存表不同于上面的内存临时表设置，这个只受max_heap_table_size系统参数影响；
- 当服务器创建内部临时表(无论是否内存或磁盘)，create_tmp_tables变量都会增加；
- 如果创建了在磁盘上内部临时表（无论是初始创建还是由in-memory转化）,create_tmp_disk_tables变量都会增加；
```mysql
show status like '%tmp%'
```
#### 优化技巧
- 建表，表结构的拆分，核心字段都用int，char，enum等定长结构，非核心字段或用到的text，超长的varchar，拆出来单放一张表；
- 建索引:合理的索引可以减少内部临时表
- 写语句：不合理的语句将导致大量数据传输以及内部临时表的使用；

## 表的优化与列的选择
#### 表的优化
- 定长与变长分离
1. id int整型，占4个字节，char(4) 占四个字符长度，time每一个单元值占的字节都是固定的（变长的可以放一张表）
2. 常用字段和不常用字段分离；
3. 合理添加冗余字段；
4. 空间换时间

#### 列选取原则
- 字段优先级：整型 > date,time > enum,char > varchar > blob
分析：
1. 整型：定长,没有字符集差异，没有时区概念；
2. time：定长，运算速度快，节省空间，考虑时区，写sql语句不便；
3. enum：能起约束的作用，内部实际用整型存储，内部要经历数字与串的转化,也结束了IO；
4. char：定长，考虑字符集合（排序）校对集；
5. varchar：不定长 要考虑字符集的转化与排序时的校对集，速度慢
6. text/Blob 因为无法使用内存临时表，查询速度最慢；
- 够用就行，不要多余
1. 大的字段浪费内存，影响速度；(比如年龄一般用tinyint，如果用int浪费3个字节)
- 尽量避免使用null
1.null不利于索引，要用特殊的字节来标注；
2.在磁盘上占据的空间更大。
- key_len表示索引所使用的字节数
1. 一个字符占三个字节;
2. 参考:[https://www.cnblogs.com/lukexwang/articles/7060950.html](https://www.cnblogs.com/lukexwang/articles/7060950.html);
3. 参考:[http://huatu.98youxi.com/lct/#Rdd1d362ab13458cb7bb012ad53e95d13](http://huatu.98youxi.com/lct/#Rdd1d362ab13458cb7bb012ad53e95d13)