# Web后端开发



## SpringBoot

***简化了Spring开发步骤的一个子项目***

#### 入门程序解析

- 起步依赖

  ***spring-boot-starter-web:***	包含了web开发所需要的常见依赖

  ***spring-boot-starter-test:***	包含了单元测试所需要的常见依赖

- 内嵌TomCat

  SpringBoot 内嵌了Tomcat，因此可以直接使用main方法启动项目

#### 快速入门

1. 新建***class***文件，然后写关于页面操作的代码，**要在类上加注解**
2. 在项目的 ***class*** 文件中运行 ***main*** 方法
3. 在浏览器输入***ip地址***，例如 ***localhost8080/hello***, 此处的/hello是之前请求处理类中自己定义的地址，然后即可看到返回值

```java
//请求处理类
@RestController
public class helloController {
    @RequestMapping("/hello")
    //括号中指的是，浏览器若将来处理"/hello"地址时，会调用下面这个方法
    public String hello0() {

        System.out.println("hello world");
        return "hello world";
    }
}
```



#### 分层解耦

- **三层架构**

  单一职责原则：使得每个类尽量只完成自己对应的逻辑功能，而不是很多功能全部写在一个类里面

  使用三层架构复用性强，便于维护，利于拓展

  - **具体内容![image-20240506133251577](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240506133251577.png)**

- **分层解耦**

  右侧两部分代码中，当其中一个方法的名称发生改变，则另一个代码中也需要作出调整，这种情况即耦合。

  写代码时要尽量避免耦合情况的发生。

  ![image-20240506134158609](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240506134158609.png)

- **解决办法**

  将所需要的对象创建并存放到统一的容器中，其他方法调用时在容器里寻找所需对象即可

  - **控制反转和依赖注入**

    **控制反转**

    ***Inversion Of Control***， 简称 ***IOC***。**对象的创建控制权由程序自身转移到外部（容器）**，这种思想称之为控制反转

    **依赖注入**

    ***Dependency Injection***， 简称 ***DI***。容器为应用程序提供运行时所依赖的资源，称之为依赖注入

    **Bean对象**

    ***IOC*** 容器中创建、管理的对象都称之为***Bean***

    


- ***IOC&DI (控制反转&依赖注入) 入门***

  - 第一步![image-20240510174604624](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240510174604624.png)**即在类上方加 *@Component* 注解即可**

  - 第二步![image-20240510174656616](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240510174656616.png)

    **即在创建成员变量上加上注解 *@Autowired* 即可**

    
  
- ***IOC(控制反转) 详解***


  - 即不同的业务层的 ***@Componet*** 注解可以使用如下注解之一代替，当然要与当前业务层对应

    声明 ***bean*** 的时候可以通过 ***value*** 属性指定其名字，若没有指定，则默认首字母小写

    **使用以上四个注解都可以声明 *bean* 但在 *springbooot集成web开发中，只能用@Controller***![image-20240522152746738](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240522152746738.png)

  - ***Bean*** 组件扫描

    先前的四大注解若想生效，还需被组件扫描注解 ***@ComponentScan*** 扫描

    而 ***@ComponentScan*** 该注解虽没有显式配置，但实际上已经包含在了启动类声明注解 ***@SpringBootApplication***,默认范围是启动类所在包及其子包，因此推荐直接把项目放在启动类所在包及其子包下即可




- ***DI (依赖注入) 详解***

***@Resource 与 @Autowired区别***

前者是jdk提供的注解，而后者是spring框架提供的注解

前者默认按照名称进行注入，而后者默认是按照类型进行注入

![image-20240522161155443](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240522161155443.png)



****

#### 注意

![image-20240506132449999](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240506132449999.png)



### HTTP 协议 

- 概念 

  ***Hyper Text Transfer Protocol*** ，即超文本传输协议，规定了浏览器和服务器之间数据传输的规则

  

- 特点

  1. 基于 ***TCP*** 协议，面向连接，安全
  2. 基于请求-响应模型，一次请求对应一次响应
  3. 是无状态协议，对于事物处理没有记忆，每次请求-响应都是独立的
  4. 缺点-多次请求不能共享数据   优点-速度快 

  

#### 请求响应

- 请求数据格式![image-20240410145323396](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240410145323396.png)![image-20240410144905890](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240410144905890.png)

  

- ***DispatcherServlet 前端控制器***

  负责接收前端浏览器 和 各个 ***Controller类*** 请求，并分别处理并传递给***Controller类*** 和 前端浏览器

  

- 请求对象 ***HttpServletRequest*** 和响应对象 ***HttpServletResponse***

​		分别用于**获取浏览器发出的请求数据**和**设置返回给浏览器的响应数据**



- ***B/S*** 架构模式 和 ***C/S*** 架构

  ***B/S*** 架构 

  即***Browser/Server*** 浏览器/服务器架构。

  客户端只要浏览器，应用程序的逻辑和数据都存储在服务端

  只要用浏览器可以访问的网站，都是***B/S*** 架构

​		***C/S*** 架构 

​		即 ***Cilent/Server*** 客户端/服务器架构

​		即需要安装单独的客户端，且根据不同操作系统有不同版本的客户端。

​		因而开发和维护麻烦，但体验会更好



- 请求响应格式![image-20240410165324780](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240410165324780.png)
  - 响应状态码![image-20240410164706104](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240410164706104.png)
  - 常见状态码![image-20240410165211754](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240410165211754.png)

##### 请求

- **简单参数**接收![image-20240420233126238](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240420233126238.png)

  - 代码示例

    ```java
        @RequestMapping("/simplePm")
        public String simplePm(String name, Integer age){
    
            System.out.println((name + " " + age));
            return "finish";
        }
    
    //上面代码是简化版本，只要在形参列表中直接填入需要接收的参数类型和名称即可注意两者名称须保持一致
    //注意两者名称须保持一致，因为会自动进行类型转换因此类型不用一致
    //如若名称没有保持一致，则运行后就不能将参数成功传递，但不会报错
    //此时可以利用注解@RequestParam来成功传递。但同时此注解的一个叫做required属性默认值为true，代表此参数必须传递，若没有传递则会报错，也可以将其改为false，就不用必须传递了
    
        @RequestMapping("/simplePm")
        public String simplePm(HttpServletRequest request){
            String name = request.getParameter("name");
            String ageStr = request.getParameter("age");
    
            int age = Integer.parseInt(ageStr);
            System.out.println((name + " " + age));
            return "finish";
        }
    
    ```



- **实体参数接收**

  当需要传递的参数量较为庞大时，简单参数接收的方法显得过于繁琐，此时将其封装到一个实体类中，传递实体类，从而解决该问题。

  - **简单实体参数封装**代码示例

    ```java
    //定义一个新的实体类用来接收参数，注意参数列表名称必须相同
    public class User {	
    
        private String name;
        private int age;
    
        public String getName() {
            return name;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public int getAge() {
            return age;
        }
    
        public void setAge(int age) {
            this.age = age;
        }
    
        @Override
        public String toString() {
            return "User{" +
                    "name='" + name + '\'' +
                    ", age=" + age +
                    '}';
        }
    }
    //控制器代码中的方法
        @RequestMapping("/simplePojo")
        public String simplePojo(User user){
            System.out.println(user);
            return "ok";
    
        }
    
    ```



- **数组集合参数**接收

  - 代码示例

    ```java
    //数组参数接收
    @RequestMapping("/simpleParam")
    public String simpleParam(String[] datas){
        System.out.println(Arrays.toString(datas));
        return "OK";
    }
    //数组参数接收，以集合形式
    @RequestMapping("/simpleList")
    public String simpleList(@RequestParam List<String> datas){
        System.out.println(datas);
        return "Ok";
    }
    ```

  - **注意**

![image-20240422163839203](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240422163839203.png)



- **日期时间参数**接收

  ![image-20240422165931605](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240422165931605.png)

  - 代码示例

    ```java
    @RequestMapping("/dateParam")
    public String dateParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")LocalDateTime updateTime){
        System.out.println(updateTime);
        return "ok";
    }
    ```
    
  - **注意**

  **需要在方法形参列表中指定日期参数格式的形式，注意格式中的大小写，且传递参数时的时间参数必须用0补齐位数**

  

- **Json参数**接收

  - 代码示例

  - ```java
    @RequestMapping("/jsonParam")
    public String jsonParam(@RequestBody User user){
        System.out.println(user);
        return "ok";
    
    }
    ```

  - **注意**

​			**参数列表的各个参数名必须和类中名称一一对应，在POSTMAN中须调整为POST操作，然后选中BODY，然后选中ROW，选中JSON**



- **路径参数**接收

  - 代码示例

    ```java
    @RequestMapping("/path/{id}/{name}")
    public String pathParam(@PathVariable Integer id, @PathVariable String name){
        System.out.println(id);
        System.out.println(name);
        return "ok";
    }
    ```

  - **注意**

    **即直接通过传递参数的URL来直接传递参数，注意形参列表要与注解内大括号内的参数一一对应，要传递几个参数就加几个大括号和形参**

##### 响应

- ***@ResponseBody*** 注解![image-20240506125048003](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240506125048003.png)

  - **统一响应结果**

    将每个请求方法称之为接口，而 ***@RequestMapping*** 后的地址则为访问地址。每个方法的响应结果不同，在数据较大情况下难以管理，因此需要统一响应结果。返回一个统一格式的对象，如下：![image-20240506130223764](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240506130223764.png)



### Web 服务器

Web 服务器是一个软件程序，是对 HTTP 协议操作进行封装，让Web开发更为便捷。主要提供网上信息浏览功能

#### Tomcat

一个轻量级web服务器，支持servlet，jsp等少量javaEE规范

- 项目部署

  将项目直接复制到其***webapps***目录下即部署完成

- 

