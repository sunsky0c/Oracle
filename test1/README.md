查询语句1
```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
查询语句2
```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
***
自定义查询语句
```sql
SELECT d.department_id,d.department_name 
FROM hr.departments d 
ORDER BY d.department_id 
OFFSET 5 ROWS FETCH NEXT 5 ROW ONLY;
```
--运行结果如下：

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/tu.png)

查询部门编号以及名字，并以一次一个数据块分页查询，以五条数据为基础。
并且在使用OFFSET....FETCH子句时必须同时使用ORDER BY，
需要注意的是可以单独使用OFFSET,但是不可以单独使用FETCH，这种分页查询形式方便在庞大的数据下，可以将数据分割显示。

在查询过程中能够起到分页显示且不需要将数据全部显示（数据量大时），能够快速的显示出数据内容。
