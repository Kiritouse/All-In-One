# 建表
create table table_name(
   属性1    属性类型1,
   属性2    属性类型2 not null,
   ...
   primary key(属性1),   --这下面都是对这个表的修改
   foreign key(属性2) references (另一个表) 这里默认是连接另一个表的同名属性
)
# 表的更新
## insert
insert into instructor values ('10211', 'Smith', 'Biology', 66000);
## delete

## drop

## alter

# 基础查询

## select
select distinct dept_name	from instructor

## 对列进行重命名
select '437' as FOO
或者select ‘437’  foo from dual;

## select子句进行数学运算
select ID, name, salary/12  as monthly_salary

## 自连接

## 字符串匹配
查询名字中含“dar”的所有老师名字.
		select name	from instructor	where name like '%dar%
 _ 只匹配一个任意字符
 % 任意匹配
## where
### 元组比较
```sql
select name, course_id
	from instructor, teache	where (instructor.ID, dept_name) =(teaches.ID, 'Biology');
```
这种写法是oracle不支持的,oracle只支持某一行数据和**单行**的子查询进行比较,注意一定要单行,多行会报错,可以使用**distinct**
```sql
select * from instructor
where (dept_name,salary) = 
(select dept_name,salary from instructor where id = '80759')
```

## 集合运算
- union  并
- intersetct  交
- minus 
自动采用去重

## 多值函数
> 操作在一个列的**多值集**上,获得一个单值的函数
> 所有的聚集函数都会忽略空值

avg平均值
min最小值
max最大值
sum求和
count计数

## 聚集(group by)
select中选中的非聚集的函数的列必须出现在group by子句

## 嵌套子查询
select-from-where
	select A1,A2,...,An
		from r1,r2,...,rm
			where P
	From 
		ri可以替换为任意合法的子查询
	where
	 
	Select
		Ai能被标量子查询(返回一行)来进行替代


## 空集测试
exists(sub)  只要sub返回的有数据就为真,可以看作  ,存在这样一种情况为真
not exists(sub) 只要sub返回的数据为null就为真,不存在这样一种情况为真

## not exists
1. 查询上过心理学院所有课程的学生的ID和姓名.
select S.ID, S.name
from student as S
```sql
select S.ID, S.name
from student as S
where not exists ( (select course_id
                                 from course
                                 where dept_name ='Psychology')
                               except
                                 (select T.course_id
                                   from takes as T
                                   where S.ID = T.ID));

```

Note that X – Y = Ø     X是 Y的子集
第一个子查询列举生物院的所有课程
第二个子查询列举学生上过的所有课程
2. 给计算机学院姓名以"J"开头的所有同学都上过课的老师的信息
> 这种有**所有**的,考虑两个not exits嵌套(查询 xx过xx所有xx的xx的信息)
> 反向思考:拆分
> 如果这个老师教的同学不存在对于任意一个以j开头的同学
> 其不存在没有上过这个老师上过的课
```sql
select * 
	from instructor a
		where not exits(  --任意一个以j开头
			select 1 
				from student b
					where dept_name='Comp.Sci.'
					and name like 'J%'
					and not exists(
						select 1   --上过这个老师的课
							from takes c join teaches d using(year,semester,course_id,sec_id)
							where a.id=d.id and b.id=c.id
					)	
		)
```

3. 查询上过心理学院所有课程的学生的ID和姓名.
```sql
SELECT a.id, a.name
  FROM student a
 WHERE NOT EXISTS
          (SELECT 1
             FROM course b
            WHERE     b.dept_name = 'Psychology'
                  AND NOT EXISTS
                         (SELECT 1
                            FROM takes c
                           WHERE a.id = c.id 
                             AND b.course_id = c.course_id))
```

## with
> 查询每年课程的授课人次超过本课程开设院在本年开设课程的修课总人数1/3的课程名，年份，修课人次，本院课程总修课人次，所占百分比

```sql
WITH ta   --这里能连续使用
     AS (  SELECT title,
                  year,
                  dept_name,
                  COUNT (*) cnt
             FROM takes NATURAL JOIN course
         GROUP BY title, year, dept_name),  //某课程在某院在某年开设课程人数
     tb
     AS (  SELECT dept_name, year, SUM (cnt) total
             FROM ta
         GROUP BY dept_name, year) //某院在某年开设的所有课程的修课程的总人数
SELECT ta.title,
       ta.year,
       ta.dept_name,
       cnt,
       total,
       ROUND (cnt / total * 100, 0) || '%' ratio
  FROM ta, tb
 WHERE     ta.dept_name = tb.dept_name
       AND ta.year = tb.year
       AND ta.cnt * 3 > total;

```

## 标量子查询
返回的必须是单个数值,不能是一行或者多个或者多行数据

## 删除
```sql
删除所有院系所在地在watson楼的院系的老师
delete from instructor
where dept name in (
		select dept name                                                         from department where building = ‘Watson’);

```

## 插入
![](Pasted%20image%2020240523145749.png)

> 根据条件来插入
> ![](Pasted%20image%2020240523150109.png)

## 更新
![](Pasted%20image%2020240523154057.png)

## CASE语句
![](Pasted%20image%2020240523155244.png)
### CASE用法示例
 ####  对select语句进行有效地选择

```sql
select name,
max(case course when 'C' then score else null end) C,
max(case course when 'DB' then score else null end) DB,
max(case course when 'English' then score else null end) English
from scores
group by name;
```

#### 代替count进行计数
```sql
SELECT  
    SUM(CASE WHEN order_amount > 1000 THEN 1 ELSE 0 END) AS high_order_count,  
    SUM(CASE WHEN order_amount > 500 AND order_amount <= 1000 THEN 1 ELSE 0 END) AS medium_order_count,  
    SUM(CASE WHEN order_amount <= 500 THEN 1 ELSE 0 END) AS low_order_count  
FROM orders;
```