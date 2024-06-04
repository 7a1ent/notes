# JavaWeb





## Web前端标准 

- 即网页标准，由一系列标准组成，大部分由W3C万维网联盟制定。
- 三个组成部分：
  - **HTML：负责网页的结构，即页面元素和内容**
  - **CSS：负责网页的表现，即页面外观，位置等样式**
  - **JavaScript：负责网页的行为，即交互效果**



## *HTML & CSS*

#### 说明

- ***HTML（HyperTextMarkup Language）***，即超文本标记语言
  - **超文本**，代表了比普通文本功能更强大，可以定义图片、音频等内容
  - **标记语言**，由标签构成的语言
    - 其标签都是预定义好的，例如使用 ***<a>*** 展示超链接，使用 ***<img>*** 展示图片，***<vedio>*** 展示视频
    - 其代码直接在浏览器中运行，由浏览器负责解析 

- ***CSS (Cascading Style Sheet)*** ,即层叠样式表，用于控制页面样式



#### ***HTML***

##### 特点

- 不区分大小写

- 标签属性值单引号双引号都行

- 语法松散

  

##### 标签

###### **图片img标签**

- ***src：*** 图片资源路径 	***width：***宽度 	***height：*** 高度 	**宽度和高度有两种写法，一种是像素，一种是相对于父类的百分比**
- 路径书写方式
  - **绝对**磁盘路径：图片在电脑磁盘中保存的路径
  - **绝对**网络路径：图片从网络中复制的网络链接路径 （得联网且在网络上存在）
  - **相对**路径：相对于当前 ***HTML文件***位置的路径，***./*** 表示当前目录，***../*** 表示上一级目录，**./** 可省略
- 修改图片宽度和高度属性时，一般只修改一个，其余会自动按比例缩放

###### 标题标签

- ***<h1>...</h1>*** 从1到6重要程度依次降低

###### 水平标签

- ***<hr>*** 在页面中表示一段水平线

###### 无语义标签

- ***span /span*** 表示无语义标签

###### 超链接标签

- 基本格式 ：***<a href = "www. ..." target = "_self"></a>*** 

- 属性：

  - ***href*** 表示指定资源访问的 ***url***

  - ***target*** 表示指定在何处打开资源链接

  - ***_self*** 默认值，表示在当前页面打开

  - ***_blank*** 表示在空白页面打开

###### 视频标签

- ***<vedio>***
- 属性：
  - ***src*** 规定视频的 ***URL***
  - ***controls*** 显示播放空间
  - ***width*** 播放器宽度
  - ***height*** 播放器高度

###### 音频标签

- ***<audio>***
- 属性：
  - ***src*** 规定音频的 ***URL***
    - ***controls*** 显示播放音频的控件

###### 段落标签

- ***<p>***

###### 文本加粗标签

- ***<b> 或 <strong>***

###### 换行标签

- ***<br>***

###### 表格标签

- ***<TABLE>*** 定义表格整体，可以包裹多个 ***<tr>*** ，拥有三个属性
  - ***border：规定表格边框宽度***
  - ***width：规定表格宽度***
  - ***cellspacing：规定单元之间的空间***

- ***<tr>*** 表格的行，可以包裹多个 ***<td>***
- ***<td>*** 表格的普通单元格，可以包裹内容，如果是表头单元格，可以替换为 ***<th>*** ，**拥有居中jia'cu**

###### 空格占位符

- 无论在代码中输入多少空格，都只会显示一个
- 如果要显示更多个空格，须使用空格占位符 ***&nbsp***

​    



#### *CSS*

##### *CSS* 引入方式

- 行内样式：写在标签的 ***style*** 属性中，**不推荐**

  `<h1 style="color: red;">`

- 内嵌样式：写在 ***style*** 标签中，可以写在页面的任何位置，但通常写在 ***head*** 标签中

  例如，对于一个标题 ***h1***，`<style> h1{color: red;} </style>` 	

- 外联样式：即将代码写在一个单独的 ***.css*** 文件中，需要通过 ***Link*** 标签在网页中引入

  

##### ***CSS*** 样式

###### 字体

- ***font-size*** 表示字体大小，记得加 ***px***，如 ***13px***

###### 颜色

- 关键字，即英文单词，如 ***red,green...***
- ***rgb***表示法，红绿蓝三原色，每项取值范围为 **0~255**
- 十六进制表示法，以 **#** 开头，将数字转换成十六进制表示，如 ***#000000...***

###### text-decoration

- 规定添加文本的修饰，***none*** 表示定义标准的文本

###### line-height

- 设置文字之间行高

###### text-indent

- 定义第一个行内容的缩进

###### text-align

- 规定元素中的文本的水平对齐方式



  

##### 选择器

###### 说明

**即用来选取需要设置样式的元素（标签）**

- 元素选择器 `元素名称 { color : red;} ` 如：`h1 {color : red;} <h1> hello </h1>`

- id选择器 `#id属性值 { color : red;}` 如：`#hid {color : red;} <h1 id = "hid"> hello </h1>`

- 类选择器 `.class属性值 { color : red;}` 如：`cls {color : red;} <h1 class = "cls"> hello </h1>`

###### 注意

**三种选择器同时存在时，其优先级为 id选择器 > 类选择器 > 元素选择器**



##### 页面布局

###### 盒子布局

- 介绍：页面中所有元素都可以看做是一个盒子，将所有元素包含在一个矩形区域内，通过盒子的视角更方便布局

- 组成：***内容区域（content）、内边距区域（padding）、边框区域（border）、外边距区域（margin）margin并不包含在盒子之内，一般只有border里的称之为盒子，内边距指padding和content，外边距指border和margin，四个边距取值次序为 上、右、下、左***![image-20240202135011049](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240202135011049.png)

- **布局标签**：实际开发中会大量使用 ***div 和 span*** 这两个没有语义的**布局标签**
  - ***div***
    - 独占一行，一行显示一个
    - 宽度默认是父元素的宽度，高度默认由内容撑开
    - 可以设置 ***width、height***
    
  - ***span***
    - 一行显示多个
    
    - 宽度和高度默认由内容撑开
    
    - 不可以设置 ***width、height***
    
      

## XML

### 概述

XML 指可扩展标记语言（e**X**tensible **M**arkup **L**anguage）。

XML 被设计用来**传输和存储数据**，不用于表现和展示数据，HTML 则用来表现数据。

### 功能及优点

##### 1. 把数据从 HTML 分离

如果您需要在 HTML 文档中显示动态数据，那么每当数据改变时将花费大量的时间来编辑 HTML。通过 XML，数据能够存储在独立的 XML 文件中。这样您就可以专注于使用 HTML/CSS 进行显示和布局，并确保修改底层数据不再需要对 HTML 进行任何的改变。

##### 2. 简化操作

- 在真实的世界中，计算机系统和数据使用**不兼容的格式来存储数据**。

  XML 数据以**纯文本格式进行存储**，因此提供了一种独立于软件和硬件的数据存储方法。

  这让创建不同应用程序可以**共享的数据变得更加容易**。

- 对开发人员来说，其中一项最费时的挑战一直是在互联网上的**不兼容系统之间交换数据**。

  由于可以通过各种不兼容的应用程序来读取数据，以 XML 交换数据**降低了这种复杂性**。

- 升级到新的系统（硬件或软件平台），总是非常费时的。必须转换大量的数据，不兼容的数据经常会丢失。

  XML 数据以文本格式存储。这使得 XML 在**不损失数据的情况下，更容易扩展或升级**到新的操作系统、新的应用程序或新的浏览器。

##### 3. 其他

- 可以用于创建新的互联网语言
- 由于很多不同程序都能访问 XML 文件，因此可以供各种阅读设备使用

### 基础知识

```xml
<?xml version="1.0" encoding="UTF-8"?>
<site>
  <name>RUNOOB</name>
  <url>https://www.runoob.com</url>
  <logo>runoob-logo.png</logo>
  <desc>编程学习网站</desc>
</site>
```

##### 元素

- XML 元素指的是**从（且包括）开始标签直到（且包括）结束标签**的部分

- XML 元素必须遵循以下命名规则：

  - 名称**可以**包含**字母、数字以及其他的字符**
  - 名称**不能**以**数字或者标点符号开始**
  - 名称**不能**以**字母 xml（或者 XML、Xml 等等）开始**
  - 名称**不能包含空格**

  可使用任何名称，没有保留的字词。

- **最佳命名习惯**

  使名称具有描述性。使用下划线的名称也很不错：<first_name>、<last_name>。

  名称应简短和简单，比如：<book_title>，而不是：<the_title_of_the_book>。

  避免 "-" 字符。如果您按照这样的方式进行命名："first-name"，一些软件会认为您想要从 first 里边减去 name。

  避免 "." 字符。如果您按照这样的方式进行命名："first.name"，一些软件会认为 "name" 是对象 "first" 的属性。

  避免 ":" 字符。冒号会被转换为命名空间来使用（稍后介绍）。

  XML 文档经常有一个对应的数据库，其中的字段会对应 XML 文档中的元素。有一个实用的经验，即使用数据库的命名规则来命名 XML 文档中的元素。

  在 XML 中，éòá 等非英语字母是完全合法的，不过需要留意，您的软件供应商不支持这些字符时可能出现的问题。

- XML **元素可扩展** 详细说明：https://www.runoob.com/xml/xml-elements.html

##### 语法

- XML **必须要有根元素**
- XML **对于大小写敏感，即区分大小写**
- XML 中，**多个空格会被保留**
- XML 中，注释语法和HTML的很相似 `<!-- This is a comment -->`
- XML **属性值必须加引号**

​	`<note date=12/11/2007>` 这句代码是**错误**的，因为 date 后面的值没有引号引用

​	`<note date="12/11/2007">` 这句代码才是正确的，属性值必须加引号



- XML 文档由元素构成，每个元素包括**开始标签，结束标签和元素内容**

- 元素可以包含**属性**，属性提供有关**元素的附加信息**，**属性位于开始标签中**，如：`<person age="30" gender="male">John Doe</person>`

  

- **XML单标签** 

  所有的 XML 元素一般都有一个关闭标签，但也允许单标签的使用的。

  单标签是指在一个标签中同时包含了开始和结束标签，形式类似于HTML中的空元素标签。在XML中，你可以使用以下两种方式表示单标签：

  - **空元素标签**

    `<exampleTag />`

  - **使用开始和结束标签，但其元素内容为空**

    

- **XML 以 LF 存储换行**

  在 Windows 应用程序中，换行通常以一对字符来存储：回车符（CR）和换行符（LF）。

  在 Unix 和 Mac OSX 中，使用 LF 来存储新行。

  在旧的 Mac 系统中，使用 CR 来存储新行。

  **XML 以 LF 存储换行。**

  

- **实体引用**

  在 XML 中，一些字符拥有特殊的意义。

  如果您把字符 "<" 放在 XML 元素中，会发生错误，这是因为解析器会把它当作新元素的开始。

  `<message>if salary < 1000 then</message>`

  使用 **实体引用** 来避免这个错误

  `<message>if salary &lt; 1000 then</message>`![image-20240604171855045](D:\GitHub\notes\Java\imgs\image-20240604171855045.png)

  

  **注释：**在 XML 中，只有字符 "<" 和 "&" 确实是非法的。**大于号是合法的**，但是用**实体引用来代替它是一个好习惯**。





##### 树结构

- XML 文档必须包含**根元素**。该元素是**所有其他元素的父元素**。

  XML 文档中的元素形成了一棵文档树。这棵树从根部开始，并扩展到树的最底端。

  **所有的元素都可以有子元素**

- 父、子以及同胞等术语用于描述元素之间的关系。父元素拥有子元素。相同层级上的子元素成为同胞（兄弟或姐妹）。

  **所有的元素都可以有文本内容和属性（类似 HTML 中）**

- 结合实例来看，其中 ***site*** 元素即为根元素，其子元素可以依次以此类推。且多个子元素都有自己的文本内容和属性。





## JavaScript

#### 介绍

- 跨平台，面向对象的脚本语言，使得网页可交互
- 与 ***Java*** 是完全不同的语言，但基础语法类似



#### JS引入方式

##### 内部脚本

将JS代码定义在HTML页面中

- 代码必须位于<script> </script> 标签之间
- 一般将脚本放在 <body> 元素底部，但理论上可以将任意个数的***JS***代码放在任意位置

##### 外部脚本

​	将JS代码定义在外部JS中，然后引入到HTML里

- 外部 ***JS*** 文件只包含代码，不包含<script> </script> 标签
- ***script*** 标签不能自闭合，如 <script src="js/demo.js"/>， 即需要在该代码后加</script>才是正确的
- 具体步骤，将代码写在另一个 ***JS*** 文件中，然后通过 上述第二点的代码通过文件地址将代码引入



#### 语法

- 结尾分号可有可无，但建议写
- 单行注释：//           多行注释： /* */

##### 输出语句

![image-20240302143144527](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240302143144527.png)



#### 变量

- 利用 ***var*** 关键字来声明变量，如 var A = 20;
- ***JS*** 是一门弱类型语言 **变量可以存放不同类型的值**， **例如 第一点中的 A 也可以在声明为数字后存放 “字符串”**

- ***var*** 关键字声明的变量作用域较大，是全局变量，**且可以重复声明**

- ***ECMAScript 6*** 新增了 ***let*** 关键字来定义变量，但其作用域只在该代码块中有效，**且不可以重复声明**

- ***ECMAScript 6*** 新增了 ***const*** 关键字来定义常量，即一旦定义后值不可改变

  

#### 数据类型

- ***JS*** 中分为原始类型 和 引用类型

- 原始类型：***number（整数，小数，NaN 即 Not a Number）, string, boolean, null, undefined(声明变量未初始化时默认类型为该类型)***

- 引用类型：

- 输出 ***typeof 变量名*** 可以获取数据类型 

  ##### 类型转换

  - 字符串类型转化为数字：字符串字面值转为数字，若字面值不是数字，**则转为 NaN（当字母和数字混在一个字符串中，若字母在前则直接转为 NaN，若数字在前则会先读取数字，转化到字母后则停止然后输出前面读取的数字）**

  - 其他类型转为 ***boolean***：

    - ***Number*** ：0 和 ***NaN*** 为 ***false***，其余反之

    - ***String***：空字符串为 ***false***，其余反之

    - ***Null*** 和 ***undefin***：均转为 ***false***

      

#### 运算符

- 大多和 ***JAVA*** 一样，除了 ***===***

- ***===*** 表示全等运算符，== 会进行类型转换，但 === 不会 

  ```javascript
  var A = 10;
  alert(A == "10") //此处 == 会进行类型转换，因此返回 True
  alert(A === "10")//此处 === 不会进行类型转换，因此返回False
  ```

  

#### 函数

- 介绍：函数是被设计为执行特定任务的代码块
- 定义：通过 ***function*** 关键字定义

```javascript
function functionName(参数1，参数2...){
//代码主体
}
/*注意
因为JS是弱类型语言
1. 不需要指定形参类型。
2. 返回值也不需要定义类型，可以在函数体内直接return
3. 调用函数时可以传递多个参数，但函数体只会接收其设定时的参数个数的参数
*/
```



#### 对象

##### 基础对象

- ***Array*** ：数组对象，用于定义数组

  ```javascript
  //定义：
  //1：
  var arrayName = new Array(1,2,3);
  //2：
  var arrayName = [1,2,3];
  /*
  特点：
  1. 长度可变，即超过定义数组大小也可以
  2. 类型可变，即存储数据类型不同也可以
  
  属性：
  length 获取数组元素个数
  
  方法：
  1. forEach方法 ：用于遍历数组中有值的元素，即若有undefined元素则不会被遍历，这点和for循环遍历不同
  
  2. push方法 ：添加元素到数组尾部
  
  3. splice方法 ：删除元素，第一个参数表示开始删除的位置，第二个表示要删除的个数
  */
  
  ```

- ***String***

  ```javascript
  //定义：
  //1.
  var stringName = new String("...");
  //2. 
  var stringName = "...";
  
  /*
  属性：
  length 获取字符串长度
  
  方法：
  1. charAt: 获取指定位置的字符
  
  2. indexOf ：检索字符串的位置，即在原字符串中找出参数里的字符串出现的位置
  
  3. trim: 将字符串的左右两侧的空格去除并返回新字符串
  
  4.substring: 截取字符串，第一个参数开始位置，第二个参数结束位置，含头不含尾
  
  */
  ```

  

- ***JSON（JavaScriptObjectNotation 对象标记法）***

  ```javascript
  //JS自定义对象
  //格式
  /*
  var 对象名 = {
  	属性名1 ： 值1;
  	属性名2 ： 值2;
  	属性名3 ： 值3;
  	函数名称： function（形参列表）{}
  };
  */
  
  //介绍：JSON 是通过JS对象标记法书写的文本，利用该格式为数据载体传输数据
  //定义： var 变量名 = '{"key1" : value1, "key2" : value2 , "key3":["bj", "sh"]}';
  
  //方法：
  /*
  1.JSON字符串转为JS对象
  var jsObject = JSON.parse(userStr);
  
  2.JS对象转为JSON字符串
  var jsonStr = JSON.stringify(jsObject);
  
  
  */
  ```

- ***BOM(BrowserObjectModel 浏览器对象模型)***

  - 组成：

    - ***Window***：浏览器窗口对象

      ```javascript
      //获取：直接使用window，window.可以省略	 
      window.alert("hello");
      alert("hello")
      
      /*
      属性：
      1. history ： 只读引用
      2. location ： 用于窗口或框架的 Location对象
      3. navigator ： 只读引用
      
      方法
      1. alert() :显示带有一段消息和一个确认按钮的警告框
      2. confirm() ：显示带有一段消息以及确认和取消按钮的警告框，点击确认返回值为true，点击取消返回值为false
      3. setInterval() : 周期性的调用函数或计算表达式
      4. setTimeout() : 在指定毫秒数后调用函数或者表达式
      */
      ```

      

    - *Navigator* ：浏览器对象

    - *Screen* ：屏幕对象

    - *History*  ：历史记录对象

    - ***Location*** ：地址栏对象

      ```javascript
      //介绍：地址栏对象
      //获取：使用 window.location 获取，其中window. 可省略
      //window.location.属性; location.属性;
      
      //属性：href 设置或返回完整的URL
      // alert(location.href);    location.href = "www.baidu.com";
      ```

      

- ***DOM（DocunmentObjectModel 文档对象模型）***

  - 将**标记语言**的各个组成部分封装为对应的 ***JS*** 对象

    - *Document*：整个文档对象

    - *Element*：元素对象（即，如文档中的标签）

    - *Attribute*：属性对象（即，标签中的属性）

    - *Text*：文本对象（即，标签当中定义的文本）

    - *Comment*：注释对象（即，标签当中的注释）

      

  - ***JS 通过 DOM， 就能够对 HTML 进行操作，即控制网页行为的途径之一***
  
  - ***DOM结构 - DOM树***
  
    - 例子
  
      ![image-20240307165925804](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240307165925804.png)
  
  - ***DOM对HTML操作的基本步骤***
    
    - 首先获取要操作的对象
    - 获取对象中要操作的属性，随后通过方法进行操作
  - ***DOM*** 获取元素对象的函数

​			![image-20240307170802482](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240307170802482.png)



#### 事件监听

- 事件：指发生在***HTML***元素上的事情

- 监听：指可以通过**JS**代码在元素上事件发生时执行代码 

##### 事件绑定

![image-20240307172240019](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240307172240019.png)

##### 常见事件

![image-20240307172336866](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240307172336866.png)

##### 实际操作

**通过上面的DOM和事件监听来实现对于HTML的操作**



### Vue - 前端框架

#### 介绍

- 框架：是一个半成品软件，是可重复利用的基础代码模型，提高开发效率

- 作用：免除 ***JS*** 中的 ***DOM*** 操作，简化书写。

- 基于 ***MVVM（model - view - view - model）*** 思想，实现数据的双向绑定，将编程关注点放在**数据**上

  

#### Vue快速入门 和 插值表达式![image-20240309190538571](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240309190538571.png)

- 编写视图中的 ***v-model*** 表示的是一个指令

- 效果呈现：其中，框中的文本数据改变时，后面的也会接着改变，体现了数据绑定效果

  ![image-20240309190718383](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240309190718383.png)

#### Vue常用指令

- 指令含义：***HTML*** 中标签上带有 ***v -*** 前缀的特殊属性

  ​	

- **常用指令**![image-20240309190926081](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240309190926081.png)
  
  - ***v-bind / v-model***![image-20240309191340810](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240309191340810.png)
    - 当两者配合时，利用 ***bind*** 动态绑定（如绑定 ***href*** 属性值，设置 ***URL***），再利用 ***model*** 设置文本框从而达成在页面中修改所绑定的内容（例如 ***URL***，效果如下图）![image-20240311204837286](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311204837286.png)
    - 如果通过这两者绑定了一个对象，则**该对象必须要在数据模型中进行**声明，即 ***在 Vue 中的 data大括号中需要进行声明***
  - ***v - on***![image-20240311205217425](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311205217425.png)
    - 和 ***JS*** 中原生的事件绑定机制类似
  
  - **条件指令**![image-20240311231955634](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311231955634.png)
  
    - ***v-if 和 v-show*** 的区别是，前者只会渲染并展示条件成立的语句，而后者则是渲染所有语句，但展示条件成立的语句 
  
  - ***v-for***![image-20240311232807734](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311232807734.png)
  
    - ```javascript
      <div v-for="(item,index) in items"></div>
      //括号中，前一个参数表示当前遍历容器的元素，后一个表示遍历容器元素的序号，in items 的 items 则是所遍历容器名称
      <div v-for="item in items"></div>
      //由于遍历容器元素序号默认为0，因此将其删除也可以正常从0开始遍历，若需从1开始，则可以像上图一样在大括号内对序号加一，因为插值表达式中可以进行计算
      ```





#### Vue 生命周期

- 概念：指一个对象从创建到销毁的整个过程
- 生命周期的八个阶段

![image-20240311235102610](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311235102610.png)![image-20240311235200196](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311235200196.png)	![image-20240311235355419](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240311235355419.png)

通常在 ***mounted*** 钩子方法中向**服务器发送请求，获取数据**，写法如上图。其他七个生命周期也有相应的方法



#### Vue 脚手架

- **功能**：更加规范和高效的进行前端开发	

- **项目创建**

  在 ***cmd*** 中有两种形式

  - 直接输入 ***vue create vue-project01***
  - 调用图形化界面进行创建 ***vue ui***

- **目录结构**![image-20240313150500415](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240313150500415.png)![image-20240314084054326](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314084054326.png)



#### Vue-Element

- ***Element*** ：提供了一套***Vue2.0***的桌面端组件库
- ![image-20240314085424357](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314085424357.png)
- **常见组件**

  使用时，访问官网复制需要组件的代码，随后进行按需调整即可

  - 表格![image-20240314085609477](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314085609477.png)
  - 分页![image-20240314085957736](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314085957736.png)
  - 对话框![image-20240314090755416](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314090755416.png)
  - 表单![image-20240314091447781](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240314091447781.png)
  
  - **具体步骤**
  
    - 写好 ***template, script, style*** 三个模板
    - 将需要的组件代码复制进 ***template*** 并调整，如果需要展示数据，需要在 ***script的 export default{data()}*** 中展示
    - 如果遇到需要展示细致的数据，则可以利用行内插槽获取一行的详细数据并根据需要效果呈现
    - 使用 ***axios*** 完成异步加载时，需要先在当前 ***vue*** 终端中下载导入
  
    #####  

##### Vue路由

当一个网站需要呈现很多不同页面时，需要 ***Vue路由功能***，即**前端路由（*URL*中的哈希与组件之间的对应关系）** ***类似于超链接？***![image-20240318144833690](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20240318144833690.png)

在创建 ***Vue*** 项目时选中 ***router*** ，并在 编译器目录中找到 ***router*** 下的 ***index.js***，进行操作



##### Vue打包部署

- 步骤
  - 首先点击编译器 ***NPM脚本*** 中第二个 ***build*** 打包
  - 随后在 ***nginx*** 服务器部署

