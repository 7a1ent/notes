# 数据库



数据模型

关系型数据库：建立在关系模型基础上，由多张相互连接的**二维表**组成的数据库



## 连接到 Mysql 数据库的方法

利用命令行连接数据库

![image-20231125095222303](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231125095222303.png)



## 数据库三层结构

### 结构示意图

![image-20231125101559669](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231125101559669.png)



三层结构即为： **DBMS（数据库管理系统） - 数据库 - 表（还有其他的对象）**

数据库拥有端口且通过端口连接客户端，即命令终端(***Dos***) 比如：***SQLyog、java***



## 数据库语句

是一种操作关系型数据库的统一标准

### 数据库语句的分类

![image-20231125101845962](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231125101845962.png)



### 约束

![image-20240523093150056](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240523093150056.png)



### 创建数据库

![image-20231125103346037](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231125103346037.png)

![image-20231125104126967](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231125104126967.png)

**如果创建时没有写校对规则和字符集，则数据库会自动使用数据库默认的校对规则和字符集。**



## 通过 Java程序 来操作数据库

**大概步骤**

1. 通过代码连接数据库
2. 在程序中编写数据库语句
3. 将编写的数据库语句发送给数据库进行执行
4. 关闭相关连接