# Maven

***Maven 是 用于构建和管理 Java 项目的工具*** 

## Maven作用

- **依赖管理**

  可以方便快捷的管理项目依赖的资源（**jar包**），即可以不用把包下载下来，再在代码中引入 **jar包** ，且可以避免版本冲突问题

- **统一项目结构**

  可以提供标准、统一的项目结构，各个软件（如，***idea、eclipse、myeclipse***）的项目无法直接导入到其他软件中，而 **maven** 可以解决这一问题

- **项目构建**

  标准跨平台（**Linux、Windows、Macos**）的自动化项目构建方式



## Maven概述

基于项目对象模型（**POM**）的概念，通过一小段描述来管理项目的构建![image-20240325142435358](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240325142435358.png)



## Maven坐标

- **坐标定义**

  坐标指的是资源的唯一标识，通过该坐标可以唯一定位资源位置

  使用坐标来定义项目或引入项目中需要的依赖

- **坐标主要组成**

  ***groupId***: 定义当前项目隶属组织名称，通常为域名反写

  ***artifactId***：定义当前项目名称，通常是模块名称

  ***version***：定义当前项目版本号



## 依赖管理

- 依赖：指项目运行所需的 ***jar ***包

- 配置：![image-20240328155014405](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328155014405.png)![image-20240328155026007](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328155026007.png)

​	***如果不知道依赖坐标信息可以去https://mvnrepository.com/中搜索***

### 	依赖传递

- **依赖传递** 

  依赖具有传递性

  如果当前项目依赖了其他项目，那么即使没有直接添加依赖其他项目的jar包，其他项目的jar包也会自动添加进去

  - 直接依赖：在当前项目中直接配置建立的依赖关系
  - 间接依赖：被依赖的资源中如果依赖其他资源，当前项目间接依赖其他资源 

- **排除依赖**

  排除依赖指主动断开不需要的依赖

  即通过***exclusion***标签，里面填写依赖的 ***groupId, aritactId***，然后刷新即可

- **依赖范围**![image-20240328161058220](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328161058220.png)

- **生命周期**

  为了对所有的项目构建过程进行抽象和统一![image-20240328161723231](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328161723231.png)![image-20240328162145228](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328162145228.png)

  ***常见Maven声明周期阶段***![image-20240328161927845](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240328161927845.png)

**在 *同一套* 生命周期中，当运行后面的阶段时，前面的阶段都会运行，但不是同一套生命周期中则不会运行**

执行生命周期的两种方式

- 在 ***idea*** 中，右侧的 ***maven*** 工具栏，选中	对应的生命周期双击运行即可
- 在命令行中，通过命令执行
