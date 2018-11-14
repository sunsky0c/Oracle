# 实验4：对象管理

## 实验步骤：
### 1.录入数据：
要求至少有1万个订单，每个订单至少有4个详单。至少有两个部门，每个部门至少有1个员工，其中只有一个人没有领导，一个领导至少有一个下属，并且它的下属是另一个人的领导（比如A领导B，B领导C）。

### 2.查询语句：

- #### 查询某个员工的信息: 
```sql
select * from employees where employee_id=1;
```  
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/12.PNG)  
 
- #### 递归查询某个员工及其所有下属，子下属员工  
```sql
select a.*,b.Name AS "下属"
from employees a,employees b
where a.EMPLOYEE_ID=b.MANAGER_ID
```  
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/13.PNG)  


- #### 查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。    

```sql
select a.*,(select sum(b.product_num*b.product_price)
from order_details b
where b.order_id=c.order_id
group by b.order_id)-a.discount as "应收货款"
from orders a,order_details c
where a.order_id=c.order_id;
```
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/14.PNG)  

- #### 查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。    
```sql
select a.customer_name,a.customer_tel,c.product_type as "产品名称"
from orders a,order_details b,products c
where a.order_id=b.order_id and b.product_name=c.product_name;
```  
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/15.PNG)

- #### **查询出所有空订单，即没有订单详单的订单。  
```sql
select orders.*
from orders a left join order_details b
on a.order_id=b.order_id
where b.order_id is null
```
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/16.PNG)
- #### 查询部门表，同时显示部门的负责人姓名  
```sql
select departments.*,employees.name as "负责人"
from departments,employees
where departments.department_id=employees.department_id
```  
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/17.PNG)

- #### 查询部门表，统计每个部门的销售总金额。  
```sql
select a.department_name,(select sum(c.Trade_Receivable)from orders c  where c.employee_id=d.employee_id group by c.employee_id)as "销售总额"
from departments a,employees b,orders d
where a.department_id=b.department_id
and d.employee_id=b.employee_id
```  
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test4/18.PNG)
