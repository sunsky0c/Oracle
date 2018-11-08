# 实验三

## 第一步：以管理员身份分配表空间与权限给我自己的用户（new_user_sunsky）  

37
38

## 第二步：新建orders表并以年份为范围分区到users，users02，users03  

21
##  第三步：为order表设置主键 order_id
22

##  第四步：创建order_details为orders的从表并根据orders的分区进行分区  
23

## 第五步插入数据到orders表中  

39
40
41

###插入后order表部分展示

42

## 第六步联合查询：  


##  不分区对比：  


##  对比：    
- 同样多的数据分区后的查询：  
cpu消耗275 ，consistent gets=137
- 不分区：  
cpu消耗35 ，consistent gets=89  
虽然分区消耗cpu多，访问数据块次数多。但是在用时上小于不分区的。分区只需要搜索特定分区，而非整张表，提高了查询速度。
