# 1、指令

~~~
truncate table 清空表
~~~

mysql有两种方式可以清空表。分别为：delete from 表名和truncate table 表名。

1. delete from 表名，删除表数据，全部删除则是可以清空表，相当于一条条删除，需要注意的是，如果有字段是自增的（一般为id)，这样删除后，id 值还是存在的。举例来说，就是加入你在删除之前最大的id为100，你用这种方式清空表后 ，新插入一条数据其id为101，而不是1。
2. 2.truncate table 表名，直接清空表，相当于重建表，保持了原表的结构，id也会清空。相当于保留mysql表的结构，重新创建了这个表，所有的状态都相当于新表。效率上truncate比delete快，但truncate删除后不记录mysql日志，不可以恢复数据。

~~~
describe table 显示表的结构（select空表看不出结构）
~~~



~~~
mysqldump -uroot -pdzy123 mydata record> /dzydata/b.sql 备份mydata库里的record表
~~~

