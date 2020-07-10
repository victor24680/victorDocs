## 如果定位到有问题的语句？
1. 开启服务器慢查询
2. 了解临时表的使用规则
3. 维护经验

## 如下题
create table A{
id varchar(64) primary key,
ver int,...
)
id,ver建立联合索引
```mysql
select id from A order by id; #特别慢？
select id from A order by id,ver; #非常快
```
表中有很长的字段
<p>大致原因</p>

1. 是innodb引擎；
2. 有多个比较长的列；
3. 是聚族索引，导致ID列排序时，要跨越很多小文件块；
4. 有比较长的列，导入块比较多，导致大量块移动；
5. 如果是MyISAM引擎，不存此种情况；
6. 如果是order by id ,优先走聚族索引，跨块太多（因为有很长的列）;
7. 如果是order by id,ver 走联合索引（二级索引），底下没有数据行,直接执行ID列，无需回行。

