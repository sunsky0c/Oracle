# 实验一 #

* 查询语句1
```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
-- 运行结果如下：

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test1/1.png)

* 查询语句2
```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
-- 运行结果如下：

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test1/2.png)

>相同之处：以部门名称分组查询所有部门及以下员工的总数与平均工资。
>
>不同之处：“Where”是一个约束声明，在查询数据库的结果返回之前对数据库中的查询条件进行约束，即在结果返回之前起作用；
“Having”是一个过滤声明，所谓过滤是在查询数据库的结果返回之后进行过滤，即在结果返回之后起作用。
***
* 自定义查询语句
```sql
SELECT d.department_id,d.department_name 
FROM hr.departments d 
ORDER BY d.department_id 
OFFSET 5 ROWS FETCH NEXT 5 ROW ONLY;
```
-- 运行结果如下：

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test1/tu.png)

>查询部门编号以及名字，并以一次一个数据块分页查询，以五条数据为基础。
>并且在使用OFFSET....FETCH子句时必须同时使用ORDER BY，
>需要注意的是可以单独使用OFFSET,但是不可以单独使用FETCH，这种分页查询形式方便在庞大的数据下，可以将数据分割显示。
>
>在查询过程中能够起到分页显示且不需要将数据全部显示（数据量大时），能够快速的显示出数据内容。

