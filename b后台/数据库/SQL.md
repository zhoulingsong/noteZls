### where 语句执行顺序

1. mysql 从左至右；筛选大量数据的条件放第一个；
2. oracle 从右至左；筛选大量数据的条件放最后一个；





### create table

~~~
1. 表不存在
	复制表结构和数据：
		create table new_table select * from old_table;
		create table new_table as select * from old_table;
	只复制表结构：
	create table new_table like old_table;(包含主键等其他属性)
	create table new_table select * from old_table where 1=2;
	create table new_table as select * from old_table where 1=2;
	
2. 表存在复制数据
	insert into new_table select * from old_table;(表结构需要一模一样)
	insert into new_table(field1, ...) select field1, ... from old_table;(对应字段数据类型需保持兼容)
	select * into new_table from old_table;(复制结构和数据,未测试通过)
	select * into new_table from old_table where 1=2;(只复制表结构,未测试通过)
~~~

