### 常用类

- _包装类(__WrapperType__)_ - _根据八种基本数据类型而对应的引用类型_

  ​	  *下面两个类的父类是 __Object__ 类，且实现了__Comparable__ 接口 和 __Serializable__ 接口*

  - ___boolean -> Boolean___

  - ___char -> Character___

    _以下类的父类都是_ ___Number____，且都实现了 __Comparable__接口_

    *而 __Number__ 的父类是 __Object__, 且实现了__Serializable__ 接口*

  -  ___byte -> Byte___

  - ___short -> Short___

  - ___int -> Integer___

  - ___long -> Long___

  - ___float -> Float___

  - ___double -> Double___

    

  - _应用_

    - 包装类和基本数据类型的相互转换，以 ___int___ <-> ___Integer___ 的__装箱(int -> Integer)和拆箱(Integer -> int)__为例, __其余同理__

    ```java
    public class Use {
        public static void main(String[] args) {
            int n = 10;
            // jdk5以前只能手动装箱和手动拆箱
            //装箱（两种写法）
            Integer I1 = new Integer(n);
            Integer I2 = Integer.valueOf(n);
            //拆箱
            Integer j1 = new Integer(100);
            int j2 = j1.intValue();
    
            // jdk5及以后可以使用自动装箱和自动拆箱
            //装箱
            Integer i1 = n;//底层用到了 Integer.valueOf(n)
            //valueOf(int) 中， 若该整数范围为 -128~127之间，则会从事先设置的Integercache数组中取出相同值的 Integer, 反之，则会重新 new 一个对象
    
            //拆箱
            int a = i1;//底层用到了 j1.intValue()
    
        }
    }
    ```

    - ___Integer___ 和 ___Character___ 类的*常用方法* 

    ```Java
    /*
    System.out.println(Integer.MAX_VALUE);
    System.out.println(Integer.MIN_VALUE);
    返回 Integer 范围的最大值和最小值，为2147483647    -2147483648
    
    System.out.println(Character.isDigit('a'));//判断是否为数字
    System.out.println(Character.isLetter('a'));//判断是否为字母
    System.out.println(Character.isUpperCase('a'));//判断是否为大写
    System.out.println(Character.isLowerCase('a'));//判断是否为小写
    System.out.println(Character.isWhitespace('a'));//判断是否为空格
    System.out.println(Character.toUpperCase('a'));//转化成大写字母
    System.out.println(Character.toLowerCase('a'));//转化成小写字母
    
    
    */
    ```
    
    - 包装类和 ___String___ 类型的相互转换， 以 ___Integer___ 和 ___String___ 为例
    
      ```java
      public class Use {
          public static void main(String[] args) {
              Integer n = 100;//自动装箱   
      	//Integer -> String
          //法1
          String s = n + "";
          //法2 调用类的 toString 方法
          String s1 = n.toString();
          //法3 调用 String 类的 valueOf 方法
          String s3 = String.valueOf(n);
      
          String st = "2568";
          //String -> Integer
          //法1 利用 parseInt 方法
          Integer i = Integer.parseInt(st);
          //法2  利用 Integer 构造器
          Integer i1 = new Integer(st);
      	}
      }
      ```
    
      

- ___String(面试涉及较多)___

  - _简介及对象的创建_

    ___String___ 用于保存字符串(用双引号括起的字符序列，如“1222”, ___"1222"才是字符串常量___)，使用 ___Unicode___ 字符编码，一个字符**(不管是汉字还是字母)**都占两个字节。

    

    _**String** 的创建_

    ```java
    public class Use {
        public static void main(String[] args) {
            //直接赋值
            /*创建步骤
            1. 先从常量池查看是否有"123321"数据空间 
            2. 有，则直接指向；没有则重新创建然后指向 
            3. s 最终指向的是常量池的空间地址
            */
            String s = "123321";
            
            
            //String 常用构造器创建
            String s0 = new String();//直接 new 对象
            String s1 = new String(s0);//构造器里传另一个 String 对象
            /*以下一行代码的创建步骤
            1. 先在堆中创建空间，里面维护了 value 属性，指向常量池的 "123" 空间
            2. 若常量池没有该空间，则重新创建；若有，则直接通过 value 指向。
            3. 即 s2 先指向堆中的 value 数组， value 数组再指向常量池的空间地址，这里要注意和之前的作出区分
            */
            String s2 = new String("123");//直接传字符串常量
    
            char []c = new char[10];
            String s3 = new String(c);//构造器里传一个 char 数组
            
    		//传一个 char 数组， 后两个参数分别为 数组需要开始读取的位置和读取的总长度
            String s4 = new String(c, 0, 10);
        }
    }
    ```

    

  - _细节_

    1. *__String 实现了 Serializable接口(若一个类实现了该接口，则表示该类可以串行化，即可在网络上传输) 、Comparable接口)表示可以该类对象可以相互比较，即字符串之间的比较) CharSequence接口(字符序列)	它的父类是  Object__*

    2. *__String__* 是 *__final__* 类，不能被其他类继承

    3. *__String__* 有属性 *__private final char value[], value__* 数组用于存放字符串内容

    4. *__value 数组是一个 final 类型，不可以修改（ final 指的是 value 数组的地址不可修改，而不是指数组存储内容不可修改）__*

    5. *__String__* 是一个 *__final__* 类，代表不可变的字符序列，字符串不可变，即一个字符串一旦被分配，其内容是不可变的。

    ```java
  String s = "hello";
    s = "haha";
  //第一行，此处常量池没有 "hello" 字符串，则会在常量池中创建该字符串数据空间，并使得 s 指向该空间
    //第二行 先会查找常量池中是否有 "haha" 字符串常量，若没有，则将创建该字符串数据空间后，使得 s 指向该空间
    //因此这段代码实际上是创建了两个字符串常量空间, 而不是将一个字符串常量改成另一个，因为这是不可变的。
    ```

    

  - *__StringBuffer 和 StringBuilder 的引出__*

    *__String 是用来保存字符串常量的，每次更新都需要开辟空间，效率低下。因此提供两者来增强 String 功能并提高效率__*

  - *__String__* 常用方法 -> *__腾讯文档__*

    

- ___StringBuffer___

  - _简介_

    - 代表可变的字符序列，可以对字符串内容进行删减
    - 很多方法和 *__String__* 相同，但 *__StringBuffer__* 是**可变长度**的
    - *__StringBuffer__* 是一个容器，它是一个 ___final___ 类，不能被继承
    - 直接父类是 *__AbstractStringBuilder__* , 实现了 *__Serializable__* 接口__(即 *StringBuffer* 的对象可以串行化，对象可以网络传输，也能保存到文件)__
    - 在直接父类中有属性 ___char [] value___ 字符串数组，该数组用于**存放字符串内容，不是 *final* 类，因此是存放在堆里的** 

  - _**StringBuffer 和 String** 类型的对比_

    - 前者保存的是字符串变量，将字符串存在 ___Char___ 类型的 ___value___ 数组中，不用每次更新地址__(当*value*数组空间不够时会根据其扩容机制而更新地址)__，因此可以更改其中内容，效率较高。后者保存的是字符串变量，每次更改 ___String___ 中的值时，实际上是改变它指向的地址，效率较低

  - ___StringBuffer___ 的几种常用构造器

    ```java
    //        1. 无参构造器，不带初始字符，其value数组初始容量为 16
            StringBuffer sb = new StringBuffer();
    //        2.带一个 int 参数，即不带初始字符，但可以指定value数组的容量
            StringBuffer sb0 = new StringBuffer(100);
    //        3.带一个 String，即带有初始字符，此时value数组的容量为 String 的大小加上 16
            StringBuffer sb1 = new StringBuffer("星瞳");
    ```

  - _**StringBuffer 和 String** 的互相转换_

    ```Java
            String str = "jdcl";
    //        String -> StringBuffer
    //        1. StringBuffer 构造器
    //        注意：返回的才是StringBuffer对象，对原字符串 str 没有影响
            StringBuffer sb0 = new StringBuffer(str);
    //        2.利用 append 方法
            StringBuffer sb1 = new StringBuffer();
            sb1 = sb1.append(str);
    
    //        StringBuffer -> String
    //        1. String 构造器
            String s0 = new String(sb1);
    //        2.tringBuffer 的 toString 方法
            String s1 = sb0.toString();
    ```
  
  - *__StringBuffer__ 的常见方法*
  
    ```java
    //        StringBuffer 的常见类方法
    //        1. append 增(追加)
    //        2. delete(start, end) 删 删除start ~ end的字符
    //        3. replace(start, end, string) 改 将[start, end)的字符替换成string
    //        4. indexOf 查(查找字符串第一次出现的位置，若找不到则返回-1)
    //        5. insert(start, string) 插, 在 start 位置开始插入字符串 string
    //        6. length 获取长度
            StringBuffer sb = new StringBuffer();
            sb.append("sb").append("?").append("Java").append("date&time").append("2023 8.8 8:13");
            System.out.println(sb);
            sb.delete(0, 3);
            System.out.println(sb);
            sb.replace(0, 4, "C++");
            System.out.println(sb);
            System.out.println(sb.indexOf("++"));
            sb.insert(0, "I'm learning");
            System.out.println(sb);
            System.out.println(sb.length());
    ```

- *__StringBuilder__* 

  - _基本介绍_
    - 一个可变的字符序列，此类提供一个与 *__StringBuffer__* 兼容的 ___API___ ，但不同步。该类是用作对于 ___StringBuffer___ 的简单替换。__在字符缓冲区被单个线程使用的时候,尽可能优先使用该类，因为此时在大多数实现中，它比 _StringBuffer_ 要快。__
    - 在 ___StringBuffer___ 上的操作是 ___append, insert___ 方法，可重载这些方法以接受任意类型的数据
    - 直接 父类是 *__AbstractStringBuilder__* , 实现了 *__Serializable__* 接口__(即 *StringBuffer* 的对象可以串行化，对象可以网络传输，也能保存到文件)__
    - *__StringBuilder__* 是 *__final__* 类，不能被继承
    - *__StringBuilder__* 仍将__字符串内容存放在父类的*char [] value*字符串数组中，该数组不是 *final* 类因此仍然存放在堆中__
    - *__StringBuilder__* 的方法没有做互斥的处理，即没有 *__synchronized__* 关键字，因此多线程时可能会出现问题，因此推荐在单线程时使用
  - *__String__* 、*__StringBuffer__* 、*__StringBuilder__* *三者的比较*
    - *__StringBuffer__* 和 *__StringBuilder__* 非常类似，均代表可变的字符串序列，而且方法也一样
    - *__String__* -> 不可变字符序列，效率低，但复用率高(字符串常量存在常量池中，可以复用)
    - *__StringBuffer__* -> 可变字符序列，效率较高(增删)，线程安全，即添加了*__synchronized__* 关键字
    - *__StringBuilder__* -> 可变字符序列，效率最高，线程不安全   
    - 如果对于字符串要进行**大量修改**，则不要使用 *__String__*，会使得大量字符串常量存在内存中，降低效率

  - *__String__* 、*__StringBuffer__* 、*__StringBuilder__* *三者的使用原则*
    - 如果对于字符串要进行**大量修改操作**，多线程情况用 *__StringBuffer__*，单线程情况用*__StringBuilder__*
    - 如果对于字符串很少修改，且被多个对象引用，则用 *__String__* ，比如配置信息等



- *__Math__* 

  - _介绍_

    - 包含了各种用于执行基本数学运算的**静态方法**，例如初等函数、对数、平方根、三角函数等

  - *常用方法* -> __腾讯文档__

    

- *__Arrays(数组类)__*
  
  - _简介_
  
    包含了对于数组管理和操作的一系列静态方法
  
  - _常用方法_
  
    - ___toString___	
  
      ```java
      	Integer [] i1 = {1, -510, 29, 92, 2568};
      	System.out.println(Arrays.toString(i1));
      //      输出结果：[1, -510, 29, 92, 2568]
      ```
  
    - ___sort___
  
      ```java
              Integer [] i1 = {1, -510, 29, 92, 2568};
              System.out.println(Arrays.toString(i1));
      //      输出结果：[1, -510, 29, 92, 2568]
              Arrays.sort(i1);
              System.out.println(Arrays.toString(i1));
      //      输出结果：[1, -510, 29, 92, 2568]
      
      //        这里是 sort 的定制排序方法，即实现了 Comparator 接口的匿名内部类的 compare 方法
      //        该方法底层是 binarysort 一个二分排序，而 compare 函数正是其中 if 中的 check 条件
      //		  体现了接口编程、动态绑定机制和匿名内部类的综合ying'y
              Arrays.sort(i1, new Comparator<Integer>() {
                  @Override
                  public int compare(Integer o1, Integer o2) {
                      return o2 - o1;
                  }
              });
              System.out.println(Arrays.toString(i1));
      //      输出结果：[2568, 92, 29, 1, -510]
      ===========自定义 sort 函数=============
      （自编接口）
      public class Use {
          public static void main(String[] args) {
              Integer [] a = {1, -510, 29, 92, 2568};
              number n = new number();
              n.method(a);
              System.out.println(Arrays.toString(a));
      //        输出：[-510, 1, 29, 92, 2568]
          }
      }
      
      interface numsort{
          public void nsort();
      }
      
      class number
      {
          public void method(Integer [] a){
              numsort nm = new numsort(){
                  @Override
                  public void nsort() {
                      for (int i = 0; i < a.length; i++)
                          for (int j = i + 1; j < a.length; j++)
                              if (a[i] > a[j]){
                                  Integer x = a[i];
                                  a[i] = a[j];
                                  a[j] = x;
                              }
      
                  }
      
              };
              nm.nsort();
          }
      }
      -----另一种写法-----（自编接口）
          
      public class Use {
          public static void main(String[] args) {
              Integer[] a = {1, -510, 29, 92, 2568};
              number n = new number();
              n.method(a, new numsort() {
                  @Override
                  public void nsort() {
                      for (int i = 0; i < a.length; i++)
                          for (int j = i + 1; j < a.length; j++)
                              if (a[i] > a[j]) {
                                  Integer x = a[i];
                                  a[i] = a[j];
                                  a[j] = x;
                              }
                  }
      
              });
              System.out.println(Arrays.toString(a));
      //        输出：[-510, 1, 29, 92, 2568]
          }
      }
      interface numsort{
          public void nsort();
      }
      
      class number {
          public static void method(Integer [] a, numsort nm){
              nm.nsort();
          }
      }    
      -----另一种写法-----（利用 compare 接口）
      public class Use {
          public static void main(String[] args) {
              Integer[] a = {1, -510, 29, 92, 2568};
      
              numsort(a, new Comparator() {
                  @Override
                  public int compare(Object o1, Object o2) {
                      int x = (Integer)o1;
                      int y = (Integer)o2;
                      return y - x;
                  }
              });
              System.out.println(Arrays.toString(a));
      //        输出：[2568, 92, 29, 1, -510]
          }
      
          public static void numsort(Integer a [], Comparator c){
      
              for (int i = 0; i < a.length; i++)
                  for (int j = i + 1; j < a.length; j++)
                      if (c.compare(a[i], a[j]) > 0) {
                          Integer x = a[i];
                          a[i] = a[j];
                          a[j] = x;
                      }
          }
      }
          
      ```
    
    - ___binarySearch___
    
      利用__二分法__找出数组中的数的位置，因为二分法因此要求数组__必须有序__。如果__数组元素不存在，返回 - (这个数应该所在的位置 + 1)__
      
      ```java
              Integer[] a = {1, -510, 29, 92, 2568};
      //利用二分法找出数组中的数的位置，因为二分法因此要求数组必须有序。如果数组元素不存在，返回 -(这个数应该所在的位置 + 1)
              System.out.println(Arrays.binarySearch(a, 29));
              System.out.println(Arrays.binarySearch(a, 30));//30 应该在数组的第 3 位，因此返回 -（3 + 1) 
      //        输出：2
      //              -4
      ```
    
    - ___copyOf___
    
      ```java
      Integer[] a = {1, -510, 29, 92, 2568};
      //将 a 数组中从下标 0 开始的 int 个数复制到 a 中
      //如果 int 大于原数组的元素个数，则后面超出的数会以 null代替
      //如果拷贝长度 int < 0，则会抛出异常 NegativeArraySizeException
      //该方法底层使用的是 System.arraycopy();
      Integer[] b = Arrays.copyOf(a, a.length);
      Arrays.toString(b);
      //输出：[1, -510, 29, 92, 2568]
      ```
    
    - ___fill___
    
      ```java
      Integer[] a = {1, -510, 29, 92, 2568};
      //使用 2568 去填充原有的数组，即替换数组原有的元素
      Arrays.fill(a, 2568);
      System.out.println(Arrays.toString(a));
      //输出：[2568, 2568, 2568, 2568, 2568]
      ```
    
    - ___equals___
    
      判断两个数组元素是否完全一样，返回结果为 ___true / false___
    
    - ___asList___
    
      ```JAVA
      Integer[] a = {1, -510, 29, 92, 2568};
      //将一个数组转换成 List (集合)
      //返回的集合 al 的编译类型是 List （接口），运行类型是 ArrayList（内部类）
      List<Integer> al = Arrays.asList(a);
      System.out.println(al);
      //输出：[1, -510, 29, 92, 2568]
      ```
    
    - _应用_ 
    
      ```java
      利用 自定义定制排序 将书按照价格排序
      public class Use {
          public static void main(String[] args) {
              book [] books = new book[4];
              books[0] = new book("红楼梦", 100);
              books[1] = new book("新金瓶梅", 90);
              books[2] = new book("青年文摘20年", 5);
              books[3] = new book("java从入门到放弃", 300);
              books[0].bookSort(books, new Comparator() {
                  @Override
                  public int compare(Object o1, Object o2) {
                      book b1 = (book) o1;
                      book b2 = (book) o2;
                      int p1 = (int) b1.getPrice();
                      int p2 = (int) b2.getPrice();
                      if (p1 > p2) return 0;//判断从大到小或从小到大的顺序
                      return 1;
                  }
              });
              for (int i = 0; i < books.length; i++)
                  System.out.println(books[i].toString());
          }
      }
      class book
      {
          private String name;
          private int price;
          public book(String name, int price){
              this.price = price;
              this.name = name;
          }
      
          public void bookSort(book [] b, Comparator c){
              for (int i = 0; i < b.length; i++)
                  for (int j = i + 1; j < b.length; j++)
                      if (c.compare(b[i], b[j]) > 0){
                          book temp = b[i];
                          b[i] = b[j];
                          b[j] = temp;
                      }
          }
      
          public String getName() {
              return name;
          }
      
          @Override
          public String toString() {
              return "book{" +
                      "name='" + name + '\'' +
                      ", price=" + price +
                      '}';
          }
      
          public void setName(String name) {
              this.name = name;
          }
      
          public int getPrice() {
              return price;
          }
      
          public void setPrice(int price) {
              this.price = price;
          }
      }
      ```

- ___System(系统类)___

  - _常用方法_ -> 腾讯文档

- ___BigInteger 和 BigDecimal___ 

  - _介绍_

    ___BigInteger 和 BigDecimal___ ，前者用于保存较大的整形数据，后者用于保存精度较高的浮点型数据

  - _加减乘除等运算_

    需要用类提供的 ___add, substract, multip, divide___ 等方法进行整数的加减乘除

    在 ___BigDecimal___ 的除法运算中，有可能会发生出现无限循环小数导致抛出异常，因此我们需要指定保留精度来解决

- __日期类（了解，会使用即可）__

  - ___Date(第一代日期类)___

    - _简介_

      精确到毫秒，代表特定的瞬间

    - _三种用法 -> 腾讯文档_ 


  - ___Calendar（第二代日期类）___

    -  _简介_

      它是一个抽象类构造器是 ___private___，因此是用 ___get___ 方法来获得实例的，为特定瞬间与一组诸如 ___YEAR、MONTH、DAY_OF_MONTH、HOUR___ 等日历字段之间的转换提供了一些方法，并为操作日历字段提供了一些方法

    - _用法 -> 腾讯文档_

  - ___LocalDate， LocalTime，LocalDateTime（第三代日期类）___

    - _和前两代的关系_

      在 ___JDK1.1___ 引入后，第一代日期类被第二代所代替。

      但第二代仍存在许多问题，比如：

      1. 可变性：像日期时间这样的类应该是不可变的
      2. 偏移性：___Date___ 中的年份是从1900开始的，而月份都从 0 开始
      3. 格式化：格式化只对 ___Date___ 有用， ___Calendar___ 则不行
      4. 此外，他们不是线程安全的，也不能处理闰秒等问题

      因而在 ___JDK8___ 中加入第三代日期类

      ___LocalDate， LocalTime，LocalDateTime___

      - _方法 -> 腾讯文档_



### 集合

- *__集合和数组的区别__*
  
  - _数组的特点_
    1. __长度开始时必须指定__，而且一旦指定则__不能更改__，不够灵活
    2. 保存的必须为__同一类型__的数据
    3. 使用数组进行增加元素的代码很麻烦（创建新的更大的数组，将原数组数据拷贝到i新数组中）
  - _集合_
    1.  可以__动态保存任意多个元素__，更灵活方便
    2. 提供了一系列__操作对象的方法__，增删改查等等
    3. 使用集合添加、删除新元素代码更加简洁
  
- ___集合的框架体系图（需要背） -> 腾讯文档___ 

- __*Collection* 接口及其常用方法__

  - _该接口实现类特点_
    1.  ___Collection___ 可以存放多个元素，每个元素可以是 ___Object___
    2.  有些 ___Collection___ 的实现类可以存放重复元素，有些不行
    3.  有些 ___Collection___ 的实现类是有序的（___List___），有些不是（___Set___）
    4.  ___Collection___ 接口没有直接的实现子类，是通过它的子接口 ___List___ 和 ___Set___ 来实现的

  - _利用 __ArrayList__ 示例常用方法 -> 腾讯文档_

  - *__Collection__ 遍历元素方式*
    
    - __方法一__  使用 ___Iterator___ 迭代器（来源于 ___Collection___ 的父接口 ___Iterable___）
      1. ___Iterator___ 对象称为迭代器 ，主要遍历 ___Collection___  中的元素
      2. 所有实现了 ___Collection___ 接口的集合类都有一个 ___Iterator___ 方法，用以返回一个迭代器对象
      3. ___Iterator___ 结构及运行原理 -> 腾讯文档
      4. ___Iterator___ 仅用于遍历集合，本身并不存放对象
      
    - __方法二__  *__for__* 循环增强 
    
      1. 本质和 迭代器 一样，只是其简化版
    
      2. 只能用于遍历集合或者数组
    
      3. __快捷方式 大写 i ，再回车即可__
    
      4.  基本语法代码演示 -> 腾讯文档
    
         ```JAVA
                 ArrayList list = new ArrayList();
                 list.add("cl");
                 list.add("xt");
                 list.add(2568);
         
         
         //      法一：利用迭代器 Iterator 遍历
                 Iterator ite = list.iterator();
                 while (ite.hasNext()){
                     System.out.println(ite.next());
                 }
         
         //      法二： 利用增强 for 循环遍历（代码底层还是迭代器）
         //		即简化版迭代器
                 for (Object ar : list){
                     System.out.println(ar);
         /*      输出：
                 cl
                 xt
                 2568
          */
         ```

- __*List* 接口和常用方法__

  - _注意_

    前面的 *__Collection__* 接口和这里的 *__List__* 接口尽管都是以 *__ArrayList__* 为代码演示，但是有区别。*__Collection__* 接口的方法 *__Set__* 也能用，但 *__List__* 的不行。（**看前面的接口结构图**）

  - *基本介绍*

    1. *__List__* 集合的元素是**有序的，即添加和取出的元素顺序一致**，且元素可以重复
    2. *__List__* 集合的每个元素都有对应的顺序索引，即支持索引取数（即和数组类似，索引从 0 开始）
    3.  *__Jdk__* 中实现了 *__List__* 接口的类有很多，常见的有 *__ArrayList__*、*__LinkedList__* 和 *__Vector__*

  - *__List__ 的常用方法举例*

    -> 腾讯文档

  - *__List__* 的遍历方法除了 *__Collection__* 的迭代器遍历和增强 *__for__* 循环遍历，还可以使用类似于数组遍历的普通 *__for__* 循环遍历

  - *注意事项*

    1. *__ArrayList__* 底层逻辑是利用数组实现数据存储的
    2. 可以加入 *__null__*
    3. *__ArrayList__* 和 *__Vector__* 基本相同，但 *__ArrayList__* 线程不安全（底层代码），因此在多线程情况下不建议用 *__ArrayList__*

  - *__ArrayList 底层结构和源码分析（重难点）__* 
  
    - *__底层结构（源码）__*
  
      1. 底层是利用 *__Object 类型的 数组 elementData__* // transient Object[] elementData;
  
         ( *__transient__* 表示瞬间、短暂，表示该属性不会被序列化)
  
      2. 若创建对象时用的是 **无参构造器** 则 *__elementData__* 数组大小为0，第一次添加数据则扩容该数组大小为10，**若需要再次扩容，则扩容大小为原来的 1.5 倍（15，15*1.5 ...）**
  
         _**详细底层源码步骤 -> 腾讯文档**_
      
      3. 若创建对象时用的**指定大小的构造器**，则初始的  *__elementData__* 大小为指定的大小，**后续再次扩容的大小也为原来的 1.5 倍**
      
         _**此处使用指定大小的构造器的创建源码步骤和上述的无参构造器类似，唯一的区别是刚开始的时候创建了一个容量为指定参数大小的数组，而非空的数组**_
         
         

- __*Vector* 的底层结构与源码分析__

  - *基本介绍*

    - 底层也是一个对象数组， *__protected Object[] elementData;__*

    - *__Vector__* 是线程同步的，即线程安全。因为其操作方法带有*__synchronized__*，因此开发中要考虑线程安全时，会考虑使用它

    - 定义说明

      ```java
      public class Vector<E>
          extends AbstractList<E>
      /*继承了 AbstractList,实现了以下接口
          implements List<E>, RandomAccess, Cloneable, java.io.Serializable*/
      ```
  
  - *__Vector 和 ArrayList 的比较 - > 腾讯文档__* 
  
  - *__扩容机制和 ArrayList 相似__*



- _**LinkedList 底层结构**_
  - __*LinkedList* 说明__
    1. 实现了双向列表和双端队列
    2. 可以添加任意元素，元素可以重复，包括*__NULL__*
    3. 线程不安全，没有实现同步
    4. **底层维护了一个双向链表，双向链表对于数组来说效率较高**
    
  - **_LinkedList_ 增删改查的底层操作逻辑 （以增删为例）**
  
    __由于底层维护的是双向链表，因此此时增删改查是基于双向链表的操作来实现的。__
  
    ```JAVA
    void linkLast(E e) {// E e 代表要添加的数据
        final Node<E> l = last; // 首先使得 节点l 指向 双向链表的last 节点（初始状态 last 节点指向 null）
        final Node<E> newNode = new Node<>(l, e, null);
        //创建新节点， （l, e, null) 中的数据分别表示一个节点的 pre、数值、和 next 的指向
        //此处则为 把 该新节点的 pre 值指向之前的 l 节点， 即添加前的 last 节点， 再把 next 指向 null，中间则为要添加的数据
        last = newNode;
        // 再把 last 节点指向 上一步 创建的新节点
        if (l == null)// 如果 l 指向为 null， 则表示为第一次添加数据
            first = newNode;//只有第一次添加数据时 才会使 first 指向新节点
        else//else 则表示不是第一次添加数据
            l.next = newNode;// 把之前的节点的 next 指向新节点
        size++;
        modCount++;
        //队列大小和操作次数加一
    }
    ```
  
    ```java
    private E unlinkFirst(Node<E> f) {//此处删除为固定删除第一个数据
        // assert f == first && f != null;
        final E element = f.item;
        final Node<E> next = f.next;//先保存 头节点的 next 指向，即下一个节点
        f.item = null;
        f.next = null; //将头节点的 next 置空， 即为链表的删除操作
        first = next;// 将头节点直接指向之前的第二个节点，即更换头节点
        if (next == null)//判断是否存在第二个节点
            last = null;
        else//如果存在，则将头节点pre值置空，成功更换头节点
            next.prev = null;
        size--;
        modCount++;
        //队列大小减一，操作次数加一
        return element;
    }
    ```
  
  - *___ArrayList 和 LinkedList 的比较 -> 腾讯文档__*



