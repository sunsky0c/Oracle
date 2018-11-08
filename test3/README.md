# 实验三

## 第一步：以管理员身份分配表空间与权限给我自己的用户（new_user_sunsky）  

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/37.png)
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/38.png)

## 第二步：新建orders表并以年份为范围分区到users，users02，users03  

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/21.PNG)

##  第三步：为order表设置主键 order_id

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/22.PNG)

##  第四步：创建order_details为orders的从表并根据orders的分区进行分区  

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/23.PNG)

## 第五步插入数据到orders表中  

```
//users表
begin
for i in 1..4000
loop   
insert into orders(ORDER_ID ,customer_name, customer_tel, order_date, employee_id, trade_receivable, discount) VALUES(i,'liujun', '158...', to_date ( '2017-10-21 10:31:32' , 'YYYY-MM-DD HH24:MI:SS' ), 007, 16, 7);
end loop;
    commit;
end;
/
//users02表
begin
for i in 4001..8000
loop   
insert into orders(ORDER_ID ,customer_name, customer_tel, order_date, employee_id, trade_receivable, discount) VALUES(i,'liujun', '158...', to_date ( '2017-10-21 10:31:32' , 'YYYY-MM-DD HH24:MI:SS' ), 007, 16, 7);
end loop;
    commit;
end;
/
//users03表
begin
for i in 8001..10000
loop   
insert into orders(ORDER_ID ,customer_name, customer_tel, order_date, employee_id, trade_receivable, discount) VALUES(i,'liujun', '158...', to_date ( '2017-10-21 10:31:32' , 'YYYY-MM-DD HH24:MI:SS' ), 007, 16, 7);
end loop;
    commit;
end;
/
```

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/39.png)
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/40.png)
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/41.png)

###插入后order表部分展示

![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/42.png)

## 第六步联合查询：  

```
//查询PARTITION_BEFORE_2016分区中两张表的数据（部分列）
SELECT
    orders.order_id,
    orders.order_date,
    order_details.order_id,
    TRADE_RECEIVABLE,
    PRODUCT_ID,
    PRODUCT_PRICE
FROM orders partition (PARTITION_BEFORE_2016) LEFT JOIN order_details partition (PARTITION_BEFORE_2016)
ON (orders.order_id = order_details.order_id);
```
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test3/51.png)


