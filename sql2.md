#		MySQL查询语句

##		表设计

--------

**表关系**

- 一对一
- 一对多
- 多对多

场景：学生表与班级表

```sql
//创建班级表
create table class(
cid int unsigned  primary key auto_increment,//主键
cname char(20) unique not null,
sum tinyint unsigned not null,
dateandtime timestamp not null DEFAULT CURRENT_TIMESTAMP comment '发布时间'
);
//将一张表的主键 作为另一张表字段(逻辑外键关联)
create table stus(
sid int unsigned  primary key auto_increment,//主键
sname char(20) unique not null,
sex enum('男','女') not null default '男',
cid int unsigned not null,
score tinyint unsigned not null
);
//创建分数表
create table score(
id int unsigned  primary key auto_increment,
score tinyint unsigned not null,
subject char(20) not null,
sid int unsigned not null
);
```

*************

##		如何设计表关系

----------------

####		外键

> 1.物理外键：mysql数据库提供表关联方式。引擎必须是Innodb（保证数据完整统一）

>2.逻辑外键：设计者通过逻辑关系进行关联。

####		数据库存储引擎

> `myisam`  检索效率高 支持事务
>
> `innodb`  执行效率 支持外键 不支持事务
>
> `memory`  执行效率/检索效率高 数据放置在内存中，数据库发生异常，数据直接丢失，无法恢复。

---------

##		多表查询

--------------

语法：`select * from tb1,tb2;`（笛卡尔乘积，无意义）

解决办法：`select * from class,stus 条件；`

条件：只能存在有关联关系的表，相同字段相等；

> `select * from class,stus  where class.cid=stus.cid；`//查询所有学生

![](https://ws3.sinaimg.cn/large/005BYqpggy1g232x9bbpij30k3070aab.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

> `select * from class,stus where class.cid=stus.cid and  stus.cid=5；`//查询5班的人

![](https://ws3.sinaimg.cn/large/005BYqpgly1g23368t7l6j30ke04p753.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

> `select count(1) from stus group by sex;`//男女各有多少人

![](https://ws3.sinaimg.cn/large/005BYqpgly1g233d76sgrj30dx04l74o.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

> `select count(1) from stus group by cid;`//每个班多少人

![](https://ws3.sinaimg.cn/large/005BYqpgly1g233fjhpukj30fj051di2.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

> `select cname,count(1)sum from class,stus where class.cid=stus.cid group by stus.cid;`//带班级号

![](https://ws3.sinaimg.cn/large/005BYqpggy1g233vdirgwj30q4055q48.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

> `select * from stus where score=(select max(score)from stus);`//分数最高的学生

![](https://ws3.sinaimg.cn/large/005BYqpggy1g2343dst48j30kg05hjue.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

------------------------------

####		嵌套查询

```php
select * from stus where score=(select max(score)from stus);
//将一个查询语句的结果作为另一个条件语句的条件
//嵌套查询/子查询 效率不高 但是可以实现复杂查询
```

---------------

####	内连接查询

> 语法：`[inner] join 表 on 条件（相同字段相等）；`
>
> > 作用：实现多表查询 代替多表查询

![](https://ws3.sinaimg.cn/large/005BYqpgly1g234fb50trj30ca00oaa1.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

![](https://ws3.sinaimg.cn/large/005BYqpggy1g234j853rdj30js05nwem.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

----------

####		外连接查询

>语法：`left jion(左)` `right join(右)`
>
>> 作用：实现多表查询

```php
select * from class left join stus on class.cid=stus.cid ;
 先查class所有数据，再匹配stus有没有满足条件的数据
select * from class right join stus on class.cid=stus.cid ;
 先查stus所有数据，再匹配class有没有满足条件的数据
```

###		差异效果图

![](https://ws3.sinaimg.cn/large/005BYqpgly1g2353zs0wfj30kd0ejq4q.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

----------

##		模糊查询

> 语法：`like`
>
> > 作用：搜索功能

```php
符号：
 		%（任意多个字符）；
 		_(标准匹配)；
 		select * from stus where sname like '%张%';//所有名字包含张字的学生
		//张% 所有姓张的学生  
 		select * from stus where sname like '张_';
		//张_ 张X // 张__ 张XX
```

####	差异效果图


![](https://ws3.sinaimg.cn/large/005BYqpggy1g235p9o1cij30jw0beaab.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

-------------

###		一对一关系

```sql
丈夫
create table has(
hid int,
hname char(20) not null,
wid int unique
);
妻子
create table wife(
wid int,
wname char(20) not null,
hid int unique
);
```

> `select * from has join wife on has.hid=wife.wid;` 通过丈夫找到妻子

![](https://ws3.sinaimg.cn/large/005BYqpggy1g236blnpiqj30nk09jq2y.jpg#mirages-width=1334&mirages-height=750&mirages-cdn-type=1&shadow)

----------
###		扩展

> `select * from stus where score > (select avg(score) from stus);`求大于平均分的同学

![](https://cdn.xiaohuwei.cn/2019/04/543767241.png#mirages-width=754&mirages-height=232&mirages-cdn-type=1)

> `select cid,max(score) sum from stus group by cid;`求每个班的最高分

![https://cdn.xiaohuwei.cn/2019/04/2064710315.png](https://cdn.xiaohuwei.cn/2019/04/2064710315.png#mirages-width=847&mirages-height=209&mirages-cdn-type=1)
[hide]我爱你[/hide]