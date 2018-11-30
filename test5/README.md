# 实验5：PL/SQL编程
## 实验场景：
假设有一个生产某个产品的单位，单位接受网上订单进行产品的销售。通过实验模拟这个单位的部分信息：员工表，部门表，订单表，订单详单表。
本实验以实验四为基础。
## 实验步骤：
1.创建一个包(Package)，包名是MyPack。
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test5/50.png)<br>
2.声明函数与过程并实现。
- #### 声明：
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test5/53.png)<br>
- #### 实现：
```sql
create or replace PACKAGE BODY MyPack IS
  --测试一
  FUNCTION Get_SaleAmount(V_DEPARTMENT_ID NUMBER) RETURN NUMBER
  AS
    N NUMBER(20,2); --注意，订单ORDERS.TRADE_RECEIVABLE的类型是NUMBER(8,2),汇总之后，数据要大得多。
    BEGIN
      SELECT SUM(O.TRADE_RECEIVABLE) into N  FROM ORDERS O,EMPLOYEES E
      WHERE O.EMPLOYEE_ID=E.EMPLOYEE_ID AND E.DEPARTMENT_ID =V_DEPARTMENT_ID;
      RETURN N;
    END;
  /*测试二
  FUNCTION Get_SaleAmount(V_EMPLOYEE_ID NUMBER) RETURN NUMBER
  AS
    N NUMBER(20,2); --注意，订单ORDERS.TRADE_RECEIVABLE的类型是NUMBER(8,2),汇总之后，数据要大得多。
    BEGIN
      SELECT SUM(O.TRADE_RECEIVABLE) into N  FROM ORDERS O,EMPLOYEES E
      WHERE O.EMPLOYEE_ID=E.EMPLOYEE_ID AND E.EMPLOYEE_ID =V_EMPLOYEE_ID;
      RETURN N;
    END;*/

  PROCEDURE GET_EMPLOYEES(V_EMPLOYEE_ID NUMBER)
  AS
    LEFTSPACE VARCHAR(2000);
    begin
      --通过LEVEL判断递归的级别
      LEFTSPACE:=' ';
      --使用游标
      for v in
      (SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees
      START WITH EMPLOYEE_ID = V_EMPLOYEE_ID
      CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID)
      LOOP
        /*测试二
        N:=MYPACK.Get_SaleAmount(V.EMPLOYEE_ID);*/
        DBMS_OUTPUT.PUT_LINE(LPAD(LEFTSPACE,(V.LEVEL-1)*4,' ')||
                             V.EMPLOYEE_ID||' '||v.NAME);
      END LOOP;
    END;
END MyPack;    
```
- #### 测试一结果图（要求输入的参数是部门号，输出部门的销售金额）：<br>
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test5/51.png)<br>
- #### 测试二结果图（要求输入参数是员工号，输出员工的ID,姓名，销售总金额）：<br>
![自定义运行结果](https://github.com/sunsky0c/Oracle/raw/master/test5/52.png)<br>

3.由于订单只是按日期分区的，上述统计是全表搜索，因此统计速度会比较慢，如何提高统计的速度呢？<br>
用索引提高效率;分区技术。
