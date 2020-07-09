<p>索引类型</p>
## B-Tree索引
+ BTree索引，大方面，都用的是平衡树，具体的实现，各引擎稍有不同;
+ Myisam，innodb中，默认都是用B-tree索引;
+ BTree，可以理解为"**排好序的快速查找结构**",即**利于范围**快速查找。
## Hash索引
<p>在memory表里，默认是Hash索引，hash的理论查询时间复杂度为O（I）</p>
<p>hash的查找如此高效，为什么不都用hash索引？</p>

1. hash函数计算后的结果是随机的，如果在磁盘上放置数据，如：id主键，id对应的行也是随机存放的（不利于查找）
2. 没法对查询范围优化
3. 无法利用前缀索引，如：在tree中，field列的值为“helloword”,并加上索引xx=helloword,可以利用到索引，xx=hello，也可以利用索引（左前缀索引）,
   但hash('helloword'),hash('hello'),两者的关系分为随机，即无法查找
4. 排序无法优化；
5. 必须回行，即：通过索引拿到数据位置，必须回到表中去数据；
## 索引常见误区
- 在where条件常用的列上都加上**独立索引**
<p style="text-indent:20px;">如：where uid = 1 and capital > 10;</p>
<p style="text-indent:20px;">误：uid,capital都加上索引</p>
<p style="text-indent:20px;">两个索引实际上只能用上一个，因为独立的索引，同时只能用上一个。</p>

- 在多列上建立索引后，查询哪个列，索引都将发挥作用
<p style="text-indent:20px;">多列索引上，索引发挥作用，需要满足前缀要求。</p>
<p style="text-indent:20px;">index(a,b,c)</p>
<p style="text-indent:20px;">以下查询的测试都是查询全部
    <code class="sql">select * from table1</code>，不是单个或多个<strong>含索引列的字段</strong>
（因为此种情况，会自动优化，自动使用索引,也叫索引覆盖）
</p>

|where条件|索引是否发挥作用|原因|
|---|---|---|
|where b=7 and c=3|b,c列无法使用索引||
|where a=3|是，只用了a列||
|where a=3 and b=5|是，使用了a,b列||
|where a=3 and b=5 and c=4|是，使用了abc列||
|where b=3 or c=4|否|没有使用a索引|
|where a=3 and c=4|a列能发挥索引,c不能|
|where a=3 and b>10 and c=7|a列能利用，b能利用，c不能利用|因为b是范围|
|where a=3 and b like '%xxx%' and c=7|a列能利用，b能利用，c不能利用|
|where a=3 and c=4 order by b asc|a能用,b用上了(在于排序上)，c不能利用|b是范围|
|where a=3 order by b asc,c asc|a能用,b,c能用(在于排序上)|比如 order by b desc又无法用上索引了|
|group by 和上面的order by 类似|

<p style="padding-left:10px;">总结</p>

1. 联合索引里，必须使用第一列，否则后面的索引都无法使用，即需要层层使用;
2. 如果中间使用了范围，则后面的索引，都无法使用.
3. **分组**：一般而言，分组统计要先按分组字段，有序排列（用临时表来排序）



# Explain参数[常用的]
### type
- system 系统级别，表示已经优化至系统级别了;
- range 使用索引的方式，表范围；
### key_len
<p>所使用索引的长度</p>

### Extra
1. Using where：表示优化器需要通过索引回表查询数据;
2. Using index: 表示直接访问索引就足够获取到所需要的数据，不需要通过索引回表
3. Using index condition：V5.6之后的特性，会先过滤索引，过滤完索引之后找到符合索引条件的数据行，随后用where子句中的其他条件取过滤这些数据行;

