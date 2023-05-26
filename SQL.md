# SQL

## 1 创建删除表格

```sql
CREATE DATABASE `sql_tutorial`; 
SHOW DATABASES;  -- 展示现有的资料库
USE `sql_tutorial`;

-- 创建一个表格,设置每种数据的属性
CREATE TABLE `student`(
    `student_id` INT PRIMARY KEY, -- 这个PRIMARY KEY不可以重复
    `name` VARCHAR(20), -- 20表示最大字符串长度
    `major` VARCHAR(20)  -- 最后一个不需要  ，
);

DESCRIBE `student`;  -- 显示student的性质
DROP TABLE `student`;  -- 删除表格，再次DESCRIBE会报错

ALTER TABLE `student` ADD gpa DECIMAL(3,2); -- 增加属性，一共三位，小数占2位
ALTER TABLE `student` DROP COLUMN gpa;	-- 删除刚刚添加的属性
```

## 2 存入资料

```sql
CREATE TABLE `student`(
    `student_id` INT PRIMARY KEY,
    `name` VARCHAR(20), -- 20表示最大字符串长度
    `major` VARCHAR(20)  -- 最后一个不需要  ，
);

SELECT * FROM `student`; -- 搜寻全部资料

INSERT INTO `student` VALUES(1,'小白'，'历史'); -- 需要加引号，单双都可以
INSERT INTO `student` VALUES(2,'小黑'，NULL); -- 空过第三个
INSERT INTO `student`('name','major','student_id') VALUES('小黑'，NULL,2); -- 自己决定填入顺序
INSERT INTO `student`('name','student_id') VALUES('小黑',2); -- 正确执行
```



## 3 限制 约束

```sql
CREATE TABLE `student`(
    `student_id` INT AUTO_INCREMENT, -- 自动新增1 2 3 ……
    `name` VARCHAR(20) NOT NULL, -- 约束条件
    `major` VARCHAR(20) DEFAULT `历史`， -- 没有设置属性，将预设值设置为 历史
    PRIMARY KEY(`student_id`)
);

INSERT INTO `student`('name','major') VALUES('小黑',`历史`); -- 自动编号
```

## 4 修改 删除

```sql
SET SQL_SAFE_UPDATES = 0;  -- 将预设的更新关闭

CREATE TABLE `student`(
    `student_id` INT PRIMARY KEY, -- 自动新增1 2 3 ……
    `name` VARCHAR(20) , -- 约束条件
    `major` VARCHAR(20) ， -- 没有设置属性，将预设值设置为 历史
    `score` INT
);

SELECT * FROM `student`; -- 显示原来的

UPDATE `student`  -- 列表名
SET `major` = `英语文学`
WHERE  `major` = `英语`;		-- 所有的都更改

UPDATE `student` 
SET `major` = `生物`
WHERE  `student_id` = 3;

UPDATE `student` 
SET `major` = `生化`
WHERE  `major` = `生物` OR `major` = `化学`;

UPDATE `student` 
SET `major` = `生化`, `name`=`小会`
WHERE  `student_id` = 1;

UPDATE `student` 
SET `major` = `生化`; -- 所有的都改成

DELETE FROM `student`
WHERE `student_id` = 4;  -- 条件符合的删除掉

DELETE FROM `student`; -- 全部删除

INSERT INTO `student`('name','major') VALUES('小黑',`历史`); -- 自动编号
```



## 5 取得资料

```sql
SELECT * FROM `student`; -- 全部
SELECT `name`, `major` FROM `student`;	-- 取得两列
SELECT * 
FROM `student` 
ORDER BY `score`; -- 从小到大排序（ASC)

SELECT * 
FROM `student` 
ORDER BY `score` DESC; -- 从大到小排序

SELECT * 
FROM `student` 
ORDER BY `score` , `student_id`;  -- 相同的按照第二个条件排序

SELECT * 
FROM `student` 
LIMIT 3;  -- 只会传前三笔（一开始的顺序）

SELECT * 
FROM `student` 
ORDER BY `score` DESC
LIMIT 3; 

SELECT * 
FROM `student` 
WHERE `major` = '英语' OR `score` <> 70;  --  <> 是不等于的意思

SELECT * 
FROM `student` 
WHERE `major` IN ('历史'，'英语');
```



```sql
CREATE TABLE `employee`(
	`emp_id` INT PRIMARY KEY,
    `name` VARCHAR(20),
    `birth_date` DATE,
    `sex` VARCHAR(1),
    `salary` INT,
    `branch_id` INT,
    `sup_id` INT
);

CREATE TABLE `branch`(
	`branch_id` INT PRIMARY KEY,
    `branch_name` VARCHAR(20),
    `manager_id` INT,
    FOREIGN KEY (`manager_id`) REFERENCES `employee`('emp_id') ON DELETE SET NULL
);		-- REFERENCES 对应哪个表格的什么属性 

ALTER TABLE `employee`
ADD FOREIGN KEY (`branch_id`) 
REFERENCES `branch`('branch_id') 
ON DELETE SET NULL;

ALTER TABLE `employee`
ADD FOREIGN KEY (`sup_id`) 
REFERENCES `employee`('emp_id') 
ON DELETE SET NULL;

CREATE TABLE `works_with`(
	`emp_id` INT ,
    `client_id` INT,
    `total_id` INT,
    PRIMARY KEY(`emp_id`,`client_id`),
    FOREIGN KEY (`emp_id`) REFERENCES `employee`('emp_id') ON DELETE CASCADE,
    FOREIGN KEY (`client_id`) REFERENCES `client`('client_id') ON DELETE CASCADE
);	

/* 填资料。。。
*/
```

![image-20230314161716033](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230314161716033.png)

```sql
-- 取得所有员工资料
SELECT * FROM `employee`;

-- 取得工资前三高的员工
SELECT * 
FROM `employee` 
ORDER BY `salary` DESC 
LIMIT 3;
```



## 6 聚合函数

```sql
-- 取得员工个数
SELECT COUNT(*) FROM `employee`; 
SELECT COUNT(`sup_id`) FROM `employee`; -- 取得带有这个属性的个数 有的是NULL
-- 所有出生于1970-01-01之后的女性员工人数
SELECT COUNT(*)
FROM `employee`
WHERE `birth_date` > '1970-01-01' AND `sex` = 'F';

-- 取得平均薪水
SELECT AVG(`salary`) FROM `employee`;
-- 总和
SELECT SUM(`salary`) FROM `employee`;
-- 薪水最高最低
SELECT MAX(`salary`) FROM `employee`;
SELECT MIN(`salary`) FROM `employee`;
```

## 7 万用字元

```sql
-- % 代表多个字元， _代表一个字元
-- 电话尾号335的客户
SELECT *
FROM `client`
WHERE `phone` LIKE `%335`;

-- 姓氏为“艾”
SELECT *
FROM `client`
WHERE `client_name` LIKE `艾%`;

-- 生日再12月
SELECT *
FROM `client`
WHERE `birth_date` LIKE `_____12%`; -- 五个_代表五个字
```

## 8 联集

```sql
-- 员工名字union客户名字  显示结果都合并到同一列上面了，做合并的两个资料的个数应该相等，比如1类和1类合并，并且属性应该相等
SELECT `name`
FROM `employee`
UNION
SELECT `clinet_name`
FROM `clinet`;

-- 员工ID +员工名字union客户ID+客户名字,回传的属性名称是第一个属性名称`emp_id`,`name`
SELECT `emp_id`,`name`
FROM `employee`
UNION
SELECT `client_id`,`clinet_name`
FROM `clinet`;

-- 这样回传的属性名称是`total_id``total_name`
SELECT `emp_id`AS `total_id`,`name` AS `total_name`
FROM `employee`
UNION
SELECT `client_id`,`clinet_name`
FROM `clinet`;

-- 员工薪水union销售金额,同一属性
SELECT `salary`
FROM `employee`
UNION
SELECT `total_sales`
FROM `works_with`;
```

## 9 join连接

```sql
-- 取得所有部门经理的名字
SELECT *
FROM `employee`
JOIN `branch`
ON `emp_id` = `manager_id`;

SELECT `employee`.`emp_id`, `employee`.`name`, `branch`.`branch_name`
FROM `employee`
JOIN `branch`
ON `employee`.`emp_id` = `branch`.`manager_id`;

SELECT `employee`.`emp_id`, `employee`.`name`, `branch`.`branch_name`
FROM `employee` LEFT JOIN `branch` -- 不管条件是否成立，都把左边的表格信息全部传入，不符合的是NULL,或RIGHT JOIN
ON `employee`.`emp_id` = `branch`.`manager_id`;
```

![image-20230315144249157](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230315144249157.png)

branch那个表格只有经理的ID没有名字，所以连接起来

符合条件的三笔加入：

![image-20230315144515306](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230315144515306.png)

清爽写法

![image-20230315144902727](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230315144902727.png)

LEFT JOIN

![image-20230315145118424](C:\Users\HP\AppData\Roaming\Typora\typora-user-images\image-20230315145118424.png)



## 10 子查询

```sql
-- 查出研发部门的经理名字,这里是一个结果 =

SELECT `name` 
FROM `employee`
WHERE `emp_id` = (
    SELECT `manager_id` 
	FROM `branch`
	WHERE `branch_name` = '研发'
);

-- 找出对单一位的客户销售金额超过5000的员工名字，可能很多结果， 用IN
SELECT `name` 
FROM `employee`
WHERE `emp_id` IN (
    SELECT `emp_id` 
	FROM `works_with`
	WHERE `total_sales` > 50000
);
```

## 11 ON DELETE

```sql

CREATE TABLE `branch`(
	`branch_id` INT PRIMARY KEY,
    `branch_name` VARCHAR(20),
    `manager_id` INT,
    FOREIGN KEY (`manager_id`) REFERENCES `employee`('emp_id') ON DELETE SET NULL
);		-- REFERENCES 对应哪个表格的什么属性 
-- ON DELETE SET NULL 如果这个emp_id被删除掉了 那么将manager_id设置为NULL

CREATE TABLE `works_with`(
	`emp_id` INT ,
    `client_id` INT,
    `total_id` INT,
    PRIMARY KEY(`emp_id`,`client_id`),
    FOREIGN KEY (`emp_id`) REFERENCES `employee`('emp_id') ON DELETE CASCADE,
    FOREIGN KEY (`client_id`) REFERENCES `client`('client_id') ON DELETE CASCADE
);	
-- ON DELETE CASCADE如果emp_id 207被删掉 那么works_with里面对应的也一起删掉
```



# 牛客题目学习

SQL判断的等于号为 = 而不是==

### 返回包含某个字符的

```sql
分析 关键词：like 用法：[字符] like '%_[]字符' 
%表示任何字符出现任意次数 _表示单个字符 []表示一个字符集 
代码 
select prod_name,prod_desc 
from Products 
where prod_desc like '%toy%'；
```

不包含的

```sql
not like
```

### 标题行重命名AS

```sql
SELECT vend_id,
vend_name AS vname,
vend_address AS vaddress,
vend_city AS vcity
FROM Vendors
ORDER BY vname
```

### 一整列进行相同计算

```sql
SELECT prod_id,prod_price ,
prod_price*0.9 AS sale_price
FROM Products;
```

### 函数以及拼接的使用

```sql
# 字符串的截取：substring(字符串，起始位置，截取字符数）
# 字符串的拼接：concat(字符串1，字符串2，字符串3,...)
# 字母大写：upper(字符串）
select cust_id,cust_name,
upper(concat(substring(cust_name,1,2),substring(cust_city,1,3))) as user_login
from Customers
```

### 返回总数并生成新的列

```sql
关键词：sum--汇总和函数 代码 
select sum(quantity) as items_ordered from OrderItems
#----------------------------
# 是什么的总数
SELECT SUM(quantity) AS items_ordered
FROM OrderItems
WHERE prod_id='BR01'
```

### 返回每个订单号各有多少行数

```sql
select order_num,count(order_num) as order_lines
from OrderItems 
group by order_num 
order by order_lines
```

### 回订单数量总和不小于100的所有订单的订单

```SQL
select order_num
from OrderItems
group by order_num
having sum(quantity)>=100  -- having--过滤分组，与group by连用
order by order_num
```

### 子查询——两个关联表

```sql
SELECT DISTINCT cust_id
FROM Orders
WHERE order_num IN (
    SELECT order_num
    FROM OrderItems
    WHERE item_price >= 10
)
```

### 两个表结合输出

```sql
WITH total_order_price AS (
    SELECT order_num, SUM(item_price*quantity) AS total_ordered
    FROM OrderItems
    GROUP BY order_num
) -- total_ordered这个是需要新产生的一项,total_order_price是为这一项新生成的一个表
SELECT cust_id, total_ordered
FROM Orders 
    LEFT JOIN 
    total_order_price
    USING(order_num)
ORDER BY total_ordered DESC
```

## 牛客非技术快速入门

### 查询去重

**distinct**

```sql
select distinct university from user_profile;
```

### LIMIT

select device_id from user_profile limit 0,2---运行效率更高

select device_id from user_profile limit 2 ---运行效率低

也可结合 limit offset： 一起使用时，limit表示要取的数量，offset表示跳过的数量

select device_id from user_profile limit 2 offset 0 // 跳过0条，从第一条数据开始取，取两条数据 ---运行效率中

### 多列排序级别

```sql
select
    device_id,
    gpa,
    age
from user_profile
order by gpa,age
# 先按照gpa升序排序，再按照年龄升序排序输出
```



### 查找区间区间

不能连续大小判断

```sql
SELECT device_id, gender, age
FROM user_profile
WHERE age>=20&&age<=23;

SELECT device_id, gender, age
FROM user_profile
WHERE age>=20 AND age<=23;

SELECT device_id, gender, age
FROM user_profile
WHERE age BETWEEN 20 AND 23;
```

### 过滤空值

```sql
select device_id,gender,age,university
from user_profile
where age is not NULL;

select device_id,gender,age,university
from user_profile
where age !='';
```

### 增加复用性in

```sql
select device_id,gender,age,university,gpa from user_profile
where university in('北京大学','复旦大学','山东大学');
```

### 模糊匹配

一般形式为：

列名 [NOT ] LIKE

**匹配串中可包含如下四种通配符：**
**_：匹配任意一个字符；**
**%：匹配0个或多个字符；**
**[ ]：匹配[ ]中的任意一个字符(若要比较的字符是连续的，则可以用连字符“-”表 达 )；**
**[ ^  ]：不匹配[ ]中的任意一个字符。**

例23．查询学生表中姓‘张’的学生的详细信息。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `‘张%’
```

例24．查询姓“张”且名字是3个字的学生姓名。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'张__’
```

如果把姓名列的类型改为nchar(20)，在SQL Server 2012中执行没有结果。原因是姓名列的类型是char(20)，当姓名少于20个汉字时，系统在存储这些数据时自动在后边补空格，空格作为一个字符，也参加LIKE的比较。可以用rtrim()去掉右空格。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `rtrim(姓名) ``LIKE` `'张__'
```

例25.查询学生表中姓‘张’、姓‘李’和姓‘刘’的学生的情况。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'[张李刘]%’
```

例26.查询学生表表中名字的第2个字为“小”或“大”的学生的姓名和学号。

```
SELECT` `姓名,学号 ``FROM` `学生表 ``WHERE` `姓名 ``LIKE` `'_[小大]%'
```

例27.查询学生表中所有不姓“刘”的学生。

```
SELECT` `姓名 ``FROM` `学生 ``WHERE` `姓名 ``NOT` `LIKE` `'刘%’
```

例28.从学生表表中查询学号的最后一位不是2、3、5的学生信息。

```
SELECT` `* ``FROM` `学生表 ``WHERE` `学号 ``LIKE` `'%[^235]'
```

### 寻找最大值

```sql
select gpa from user_profile
WHERE university='复旦大学'
order by gpa desc limit 1;

select max(gpa )
from user_profile
WHERE university='复旦大学'
```

### 计算数量和平均值

```sql
select 
  count(gender) as male_num,
  round(avg(gpa), 1) as avg_gpa
from user_profile where gender="male";
```

### 分组计算

```sql
select 
    gender, university,
    count(device_id) as user_num,
    avg(active_days_within_30) as avg_active_days,
    avg(question_cnt) as avg_question_cnt
from user_profile
group by gender, university
```

由表格：

| id   | device_id | gender | age  | university | gpa  | active_days_within_30 | question_cnt | answer_cnt |
| ---- | --------- | ------ | ---- | ---------- | ---- | --------------------- | ------------ | ---------- |
| 1    | 2138      | male   | 21   | 北京大学   | 3.4  | 7                     | 2            | 12         |
| 2    | 3214      | male   |      | 复旦大学   | 4.0  | 15                    | 5            | 25         |
| 3    | 6543      | female | 20   | 北京大学   | 3.2  | 12                    | 3            | 30         |
| 4    | 2315      | female | 23   | 浙江大学   | 3.6  | 5                     | 1            | 2          |
| 5    | 5432      | male   | 25   | 山东大学   | 3.8  | 20                    | 15           | 70         |
| 6    | 2131      | male   | 28   | 山东大学   | 3.3  | 15                    | 7            | 13         |
| 7    | 4321      | male   | 26   | 复旦大学   | 3.6  | 9                     | 6            | 52         |

生成：

| gender | university | user_num | avg_active_day | avg_question_cnt |
| ------ | ---------- | -------- | -------------- | ---------------- |
| male   | 北京大学   | 1        | 7.0            | 2.0              |
| male   | 复旦大学   | 2        | 12.0           | 5.5              |
| female | 北京大学   | 1        | 12.0           | 3.0              |
| female | 浙江大学   | 1        | 5.0            | 1.0              |
| male   | 山东大学   | 2        | 17.5           | 11.0             |

### 分组过滤

请取出平均发贴数低于5的学校或平均回帖数小于20的学校。

生成：

| university | avg_question_cnt | avg_answer_cnt |
| ---------- | ---------------- | -------------- |
| 北京大学   | 2.5000           | 21.000         |
| 浙江大学   | 1.000            | 2.000          |

```sql
select
    university,
    avg(question_cnt) as avg_question_cnt,
    avg(answer_cnt) as avg_answer_cnt
from user_profile
group by university
# 最后这里 having没写出
having avg_question_cnt<5 or avg_answer_cnt<20
```

### HAVING

HAVING 子句用于在执行聚合查询（如 GROUP BY）后对结果进行过滤。它允许您根据聚合函数的结果对结果集进行条件筛选。

以下是 HAVING 子句的基本用法：

1. 聚合查询：
   首先，使用 GROUP BY 子句对数据进行分组，并应用一个或多个聚合函数（如 COUNT、SUM、AVG）来计算每个组的结果。

   例如：

   ```SQL
   SELECT category, COUNT(*) as count
   FROM products
   GROUP BY category;
   ```

   在上述查询中，通过使用 GROUP BY 子句，将 "products" 表中的数据按照 "category" 列进行分组，并计算每个组的记录数量。

2. 过滤结果：
   在聚合查询的结果上，使用 HAVING 子句来指定筛选条件。HAVING 子句使用聚合函数的结果进行比较。

   例如：

   ```SQL
   SELECT category, COUNT(*) as count
   FROM products
   GROUP BY category
   HAVING COUNT(*) > 5;
   ```

   在上述查询中，通过在 HAVING 子句中指定条件 "COUNT(*) > 5"，只返回具有记录数量大于 5 的组。

请注意，**HAVING 子句在执行 GROUP BY 后应用筛选，而 WHERE 子句在执行 GROUP BY 之前进行筛选**。因此，HAVING 子句可以用于对聚合结果进行更细粒度的过滤。

另外，HAVING 子句可以使用其他运算符和函数来构建更复杂的条件筛选，类似于 WHERE 子句的用法。



### CASE

```SQL
CASE 测试表达式
WHEN 简单表达式1 THEN 结果表达式1
WHEN 简单表达式2 THEN 结果表达式2 …
WHEN 简单表达式n THEN 结果表达式n
[ ELSE 结果表达式n+1 ]
END
```

例子：现在运营想要将用户划分为25岁以下和25岁及以上两个年龄段，分别查看这两个年龄段用户数量 -本题注意：age为null 也记为 25岁以下

```sql
SELECT CASE WHEN age < 25 OR age IS NULL THEN '25岁以下' 
            WHEN age >= 25 THEN '25岁及以上'
            END AS age_cut,  COUNT(*)number
FROM user_profile
GROUP BY age_cut
```

