#MySQL 学习笔记#

###11.5 子查询###
- 连接查询实现多表查询性能差，推荐使用子查询；子查询可以出现在任意位置，常出现在 `where`、`from` 中；
- 关键字： **IN、NOT IN、ANY、ALL、EXISTS** (布尔类型true/false）
- 运算符： **<、>、=、!=** 等；
####《where 子查询》####
- 单行单列： where id > (select ...)
- 多行单列： where id IN (select ...)

		关键字：ANY
		where id =ANY (select ...); 相当于 IN
        where id >ANY（>=ANY);	大于任意一个（大于最小的）
        where id <ANY (<=ANY); 	小于任意一个（小于最大的） 

		关键字：ALL
   	 where id >ALL(>=ALL); 大于所有；
   	 where id <ALL(<=ALL); 小于所有；
	
		关键字： EXISTS(查询时采用遍历方式逐条查询，当条件成立时这返回当前记录）
		where EXISTS (select ...)
		where NOT EXISTS (select ...)
- 单行多列： where (id, name) = (select id, name from...)

####《from 子查询》####
- 多行多列：（*临时表*）子查询当做一张表然后再做关联查询

		select * from table1 a inner join (select * from table2 group by...) b
		on a.id = b.id 

##12 章 运算符##

- 算术运算：加(+)、减(-)、乘（*）、除（/、DIV）、求模(%、MOD);
  当除数、模数为 0 时返回 null
- 比较运算： 表达式、数值、字符串（ascii 码比较）
 - 等于(=、<=>) eg: a = b 不能有null; a <=> b 可以有null; 相等返回 1， 不相等返回 0；
 - 不等于(!=、<>) eg: a != b; a <> b;
 - between and : 范围内；
 - is null	: 空
 - in ： 
 - like： 通配符匹配；
 - regexp： 正则匹配；
- 逻辑运算：
- 位运算：





---
以下是样式：
-



a
---
b
=
c

-
---
无序列表： 有三种方式

* a
+ a
- b

> 区块引用说明：（可以无限级吧）
> > 两级：
> >
> > > 三级：

> ## 嵌套
> * 列表
>     
>     > 二级

### 华丽的分隔线：（星号，中划线，下划线。至少三个相同符号，可有空格）
***
---
___

### 连接：
[超链接](http://www.baidu.com)点我

* [超链接](http://www.baidu.com)点我


 [x] 勾选
 [] 不勾选


https://www.cnblogs.com/liugang-vip/p/6337580.html
https://www.jianshu.com/p/67daca4d4502








