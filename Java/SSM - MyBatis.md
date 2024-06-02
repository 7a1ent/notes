# SSM - Mybatis

## 1、概述

### 1.1 什么是 Mybatis？

- ***MyBatis*** ，即一款持久层 ORM 框架，支持自定义 SQL、存储过程以及高级映射。避免了几乎所有的 JDBC 代码和手动设置参数以及获取[结果集](https://baike.baidu.com/item/结果集/11040011?fromModule=lemma_inlink)。

  ***MyBatis*** 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJO类 映射成数据库中的记录。

  ***POJO（Plain Old Java Object）***是指普通的Java对象，它是一个简单的、基本的Java类，没有任何特殊要求或限制。[POJO类](https://so.csdn.net/so/search?q=POJO类&spm=1001.2101.3001.7020)通常只包含**私有字段、公共访问方法（*getter和setter*）以及一些自定义的方法**。



## 2、入门

### 2.1 利用 Mybatis 操作数据库

- 其底层操作逻辑依然是使用 sql语句操作数据库，只不过是在 idea 中利用 java 代码操作

#### 2.1.1 相关操作步骤![image-20240529160736401](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240529160736401.png)

- **代码示例**

```java
#1. 数据库连接信息
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接url
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
#数据库账号及密码
spring.datasource.username=root
spring.datasource.password=123456
   
#2. pojo类-对应数据库实体类 user类package com.example.pojo;

public class User {
    private Integer id;
    private String name;
    private short age;
    private short gender;
    private String phone;

    public User() {
    }

    public User(Integer id, String name, short age, short gender, String phone) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.gender = gender;
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", gender=" + gender +
                ", phone='" + phone + '\'' +
                '}';
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public short getAge() {
        return age;
    }

    public void setAge(short age) {
        this.age = age;
    }

    public short getGender() {
        return gender;
    }

    public void setGender(short gender) {
        this.gender = gender;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }
}

#3. 编写SQL 注解
@Mapper
public interface UserMapper {
    @Select("select * from user")
    public List<User> list();
}
    
#4. 单元测试 （依赖注入注入接口）
@SpringBootTest
class SpringbootMybatisIntroApplicationTests {

	@Autowired
	private UserMapper userMapper;


	@Test
	public void testListUser(){
		List<User> userList = userMapper.list();
		userList.stream().forEach(user -> {
			System.out.println(user);
		});
	}
}


```

#### 2.1.2 配置SQL提示![image-20240601153546930](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601153546930.png)



## 3、JDBC 和 Mybatis

### 3.1 概述

- [Java](https://baike.baidu.com/item/Java/85979?fromModule=lemma_inlink)数据库连接，（Java Database Connectivity，简称JDBC）是Java语言中用来规范客户端程序如何来访问数据库的[应用程序接口](https://baike.baidu.com/item/应用程序接口/10418844?fromModule=lemma_inlink)，提供了诸如查询和更新数据库中数据的方法。

  **是一套操作关系型数据库的API（规范）**

- ![image-20240601160306471](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601160306471.png)

### 3.2 JDBC操作数据库 

#### 3.2.1 操作代码![image-20240601160929051](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601160929051.png)

#### 3.2.2 JDBC 直接连接数据库的弊端

- 数据库连接部分代码经常需要改动，比如切换数据库时需要改动 ***URL值***， 而java代码由于打包等一系列操作，因此**改动java代码时会比较麻烦**
- 当实体类的属性变多时，解析结果的时候的结果集代码会非常**臃肿和繁琐**
- 由于每次使用时都要**获取连接对象和释放连接对象**，因此多次使用会造成**资源浪费和性能降低**

#### 3.2.3 Mybatic 和 JDBC 对比

![image-20240601161504582](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601161504582.png)

- 针对上述第一个问题，Mybatis中将**数据库连接代码放到了配置文件**中，因此每次改动时无需改动java代码，**只要改动配置文件**即可
- 针对第二个问题，Mybatis中的代码会自动将结果封装到对象中，而所有实体类对象直接存储到list中，简化了代码和操作
- 最后一个问题，配置连接时，Mybatis利用 ***前缀 spring.datasource 数据源*** 配置数据库连接信息，而 ***springboot*** 底层会自动调用**数据库连接池技术**来分配和管理连接，即 ***connection对象***，**每次操作数据库时，会从连接池获取一个连接进行操作，操作后再返回给连接池**，因此连接得到了复用，解决了资源浪费和性能降低的问题

### 3.3 数据库连接池

#### 3.3.1 概述

- 数据库连接池是一个**容器，负责分配、管理数据库连接（Connection）**
- 它允许应用程序**重复使用一个现有的数据库连接**，而不是重新建立一个

- **释放空闲时间超过最大空闲时间**的连接，来避免因为没有释放连接而引起的数据库连接遗漏

#### 3.3.2 标准接口 DataSource

- 官方提供的**数据库连接池接口**，**由第三方组织实现此接口**
- 功能是获取连接`Connection getConnection() throws SQLEXception;`
- 如今常用第三方连接池有 ***Druid, Hikari(SpringBoot 默认使用)***

#### 3.3.3 切换连接池

1. 引入相关连接池起步依赖
2. 配置数据库连接信息 （在 ***application.properties*** 中配置）![image-20240601204149840](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601204149840.png)



## 4、Lombok

### 4.1 概述

- Lombok项目**是一个java库**，它可以**自动插入到[编辑器](https://baike.baidu.com/item/编辑器/9067697?fromModule=lemma_inlink)和构建工具**中，增强java的性能。**不需要再写getter、setter或[equals](https://baike.baidu.com/item/equals/4481402?fromModule=lemma_inlink)方法**，只要有一个**注解**，就有一个功能齐全的**构建器、自动记录变量**等等。

### 4.2 Lombok 注解

![image-20240601215948969](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240601215948969.png)

- 红色标注的注解使用频次较多
