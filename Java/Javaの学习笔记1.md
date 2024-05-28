# 						Javaの学习笔记



## 面向对象编程（中级）



### Object 类详解



####  _Object_ 中的 _toString_ 方法

- **作用**

  默认返回： __全类名 + @ + 哈希值的十六进制(即_hashCode_值转化为十六进制)__

   

- __细节注意__

  - 直接输出一个对象时，该方法会被__默认调用__

  - 子类往往会重写___toString___方法用于返回对象的属性信息__(代码示例中有具体说明)_

    

- __代码示例__

  ```java
  public class Use {
      public static void main(String[] args) {
          //默认方法:
          Person p1 = new Person();
          System.out.println(p1.toString());
          //输出Com.learning.project1.Person@4eec7777
          
          //子类中利用 alt + insert 选中 toString 方法后, 对象的 toString 方法会输出该对象的属性值
          //例如输出Person{name='星瞳', age=18}
      }
  }
  ```



####  _Object_中的 _finalize_ 方法

- __作用__

  即__垃圾回收器__，当一个堆空间中的对象没有被栈空间变量指向的时候，这个对象会等待被___Java___回收

  

- __细节注意__

  - 当对象被回收时，系统会自动调用该对象的___finalize___方法，所以我们可以在子类可以重写该方法以__释放资源而达到自己的逻辑__，而如果不重写则会调用默认的___finalize___方法。

  - 对象的__回收时机是当对象没有任何引用__时，系统会先调用___finalize___方法再销毁该对象

  - 回收机制的调用一般由系统决定，但也可以通过___System.gc()___主动触发，如果不主动触发，会根据系统__GC算法__（回收算法）来触发，因此有可能不会像___System.gc()___一样马上触发

  - __该方法很少会被用到，主要用于应付面试__

    

- __用法__

  ```Java
  public class Use {
      public static void main(String[] args) {
  
          Person p1 = new Person();
          p1 = null;//利用置空使其被回收
          System.gc();//主动触发回收机制
      }
  }
  //在 Person 类中重写了 finliaze 方法
  /*   @Override
      protected void finalize() throws Throwable {
          super.finalize();
          System.out.println("该对象即将被销毁");
      }
   */
  ```





#### 断点调试（debug）

- __作用__

  一步步看代码执行所得到的结果，从而帮助我们找出___Bug___,还能追踪底层源码，它是一个基本技能
  
- __细节注意__

  在断点调试过程中是运行状态，以对象的__运行类型__来执行

  

- __用法（快捷键）__

  ___F7___  跳入方法，追踪方法的源码		___F8___ 跳过		___F9___ resume, 执行到下个断点 		___shift + F8___ 跳出方法

  

- __步骤__

  - 1. 设置我们所需的断点（也能在___debug___过程中下断点）

  - 2. ___shift + F9___进入___debug___

  - 3. 利用上述快捷键观看各个变量情况进行___debug___		





## 面向对象编程（高级）



#### 类变量和类方法

##### 类变量

- __定义和作用__

  - 类变量即 ___static___修饰的变量**(静态变量)**

    

  -  该变量的最大特点是会被__一个类的所有对象共享__，即任意一个对象对该变量进行访问或者修改时都是对同一个变量进行操作

- __细节注意__

  - 类变量能用 __类名.类变量名__方式访问，例如 ___classNumber___ 是静态变量，表示一个班的同学的班级编号，而___Student___是表示学生类，所以可以通过 ___Student.classNumber___的方法直接访问，当然也能用对象名访问

    

  - 类变量在__类加载时__就会生成，因此即使没有创建对象,只要类加载了也可以访问类变量
  
    
  
  - 类变量的生命周期是随着类的加载开始，随着类的消亡而销毁



##### 类方法

- __定义和作用__

  - 类方法即___static___修饰的方法__(静态方法)__，可以用来访问静态变量

  - 当方法中__不涉及到任何和对象相关成员__，可以将方法设计成静态方法以提高开发效率

    

- __细节注意__

  - 类方法也可以用类似类变量的类名方式访问 (格式在上面类变量那)

    

  - 和类变量一样，没有创建对象但只要加载了类也可以__直接调用类方法__，__非常方便__，因此开发工具类时，可以将方法做成静态的，方便调用

    

  - 类方法中没有___this 和 super___，即类方法中不能调用___this 和 supers___
  
    
  
  - 类方法中__只能访问静态变量或者静态方法__，即__静态方法只能访问静态成员__，而普通方法都可以访问，__但访问时都需要遵循访问权限的限制__
  
    

#### main 方法

- __关于 _public static void main(String[] args)_ 的解释__
  - 由于是 ___Java_虚拟机__ 对 ___main___ 方法进行调用，因此访问权限必须为___public___ 
  
    
  
  - 而 ___Java_虚拟机__ 调用 ___main___ 方法时不需要创建对象，因此方法必须为类方法即用 ___static___ 关键词修饰
  
    
  
  - 该方法接收 ___String___ 类型的数组即 ___args[]___ 字符串数组， 在我们用 __文本编辑器 + _cmd_ 命令窗口 执行该程序时可以在后面跟上字符串来传入参数__
  
    
  
- __细节说明__

```java
public class Use {

    //静态类成员
    public static void xthi(){
        System.out.println("I'm 星瞳, 18 years old! I'm glad to meet you!");
    }
    //非静态类成员
    public void jrhi(){
        System.out.println("I'm 嘉然, 18 years old! I'm glad to meet you!");
    }
    public static void main(String[] args) {

        //有没有 static 关键字都可以直接访问静态类成员
        xthi();

       //由于 main 方法有 static 关键字，因此不能直接访问非静态类成员, 需要先创建该类的一个对象
        Use u1 = new Use();
        u1.jrhi();
        
    }
}
//在 idea 中向 main 方法传入 String 参数
//1. 点击右上角锤子右边的小框，拉下后点 edit Configurations..
//2. 然后 add 一个 Application, 在bulid and run 中选中要运行的 class 文件
//3. 最后在 program arguments 中输入要传入的字符串即可
//4. for (int i = 0; i < args.length; i++)
//          System.out.println(args[i]);
//利用此代码输出传入的字符串
```

​	

#### 代码块

- __定义和作用__

  - 代码块又称为初始化块，是类的一部分，类似于方法，将逻辑语句封装在方法体中，通过__{}__包围起来，可以看作为对于构造器的补充 

    

  - 和方法不同，__没有方法名，没有返回，只有参数，只有方法体__，而且不通过对象或类__显示调用__，而是__加载类时，或创建对象时隐式调用__

  	- 如果多个__构造器__中都有__重复的语句__，可以抽取到初始代码块中，提高代码的复用性

- __使用__

  - __格式__：

    ​	___[修饰符]{___

    ​    	___代码....___

    ​	___}___

- __注意__

  - 修饰符可选写于不写，写也只能写 ___static___ ，根据写和不写而分为普通代码块和静态代码块

  - 代码块中的 ___;___ 号可以写，也可以不写

  - 代码块会在构造器调用时自动调用，且在构造器中代码运行前运行。

    

- __细节__
  - ___static_ 静态代码块__随着__类的加载__而执行，并且 __只会执行一次__，如果是__普通代码块，每创建一个对象就会执行__
  
    
  
  - __类什么时候会被加载__
    - 1. 创建对象实例时___(new)___
    
    - 2. 创建子类对象实例时， 父类也会被加载，且__父类先被加载__
    
    - 3. 使用类的静态成员时
    
         
    
  - 使用类的静态成员时， __普通的代码块并不会执行，而静态代码块会执行__（原因如上所述）
  
    
  
  - 创建一个对象时，在一个类中的 __执行顺序__：
    - 1. 调用__静态代码块和静态属性初始化__，两者的优先级相同，具体顺序看代码的定义顺序
    
    - 2. 调用__普通代码块和普通属性初始化__，两者的优先级相同，具体顺序看代码的定义顺序
    
    - 3. 调用构造器
    
         
  
  - 为什么静态代码块和静态属性的初始化会先于构造器被调用？
  
    构造器隐含了调用__1. _super()_（之前继承时的知识点）和 2. 普通代码块的操作__,而静态代码块和静态属性的初始化__早在类加载时就执行完毕了__，因此优先于构造器
  
    
  
  - __一个类被创建时的调用顺序总结__
  
    - 1. 父类的静态代码块和静态属性初始化（因为父类先于子类被加载，而静态成员在加载时就会执行，且这里会一直追溯到最先的父类然后再一级级向下）
  
    - 2. 子类的静态代码块和静态属性初始化
  
    - 3. 父类的普通代码块和普通属性初始化
  
    - 4. 父类的构造器
  
    - 5. 子类的普通代码块和普通属性初始化
  
    - 6. 子类的构造器
  
         
  
  - 静态代码块只能调用静态成员（即静态属性和静态方法），普通代码块可以调用任意成员
  
  

#### 单例设计模式

- __单例设计模式的定义__
  - 是态方法和属性的经典应用
  - 是在大量实践后总结和理论化之后优选的代码结构、编程风格和解决问题的思考方式。不同的问题用不同的设计模式，可以省去自己进行思考



- __单例设计模式的详细解释__

  - 单例指单个的实例，单例设计模式就是指采取一定的方法使得整个软件系统中对某个类只能存在一个对象实例，且该类只提供一个取得该对象实例的方法。

  - 单例设计模式有__1. 饿汉式 2. 懒汉式__

    - __区别__：

      - 两者__创建对象实例的时机不同__，饿汉式在__类加载时__就创建了对象实例，而懒汉式是在__使用时__才会创建
      - 饿汉式不会出现线程安全问题，但懒汉式存在线程安全问题
      - 饿汉式有可能会出现资源浪费的问题，因为若程序员没有使用对象实例，就会浪费；而懒汉式则是使用时再创建，所以不存在该问题

      

    - __饿汉式__

      ```java
      class people{
      
          private String name;
          // 在类内部直接创建一个固定对象（是static修饰的）
          private static people p1 = new people("星瞳");
          
          // 将构造器私有化
          private people(String name){
              this.name = name;
          }
          
          // 在类内部创建一个公共static方法返回该对象
          public static people setPeople(){
              return p1;
          }
      
      }
      //关于饿汉式的解释：利用static创建对象，因此类还没创建时对象就已经创建，将这种比较急迫的操作称为饿汉式
      //构造单例模式[饿汉式]的步骤
      //1. 将构造器私有化
      //2. 在类内部直接创建一个固定对象（是static修饰的）
      //3. 在类内部创建一个公共static方法返回该对象
      ```

    - __懒汉式__

      

      ```Java
      class people{
      
          private String name;
          // 在类内部先定义一个固定对象（是static修饰的）
          private static people p1;
          // 将构造器私有化
          private people(String name){
              this.name = name;
          }
      
          // 在类内部创建一个公共static方法返回该对象
          public static people setPeople(){
              if (p1 == null)
                  p1 = new people("星瞳");
      
              return p1;
          }
      
      }
      //构造单例模式[懒汉式]的步骤
      //1. 将构造器私有化
      //2. 在类内部先定义一个对象（是static修饰的）
      //3. 在类内部创建一个公共static方法返回该对象（利用指向是否等于 Null 来创建）
      ```


  - ___Java.lang.Runtime___ 就是经典单例模式的应用



#### _final_关键字

- __基本介绍__
  - ___final___ 可以修饰__类、属性、方法和局部变量__
  - ___final___ 的功能用途
    - 1. 当不希望__类被继承__时，可以用 ___final___ 修饰
    - 2. 当不希望__父类的某个方法被子类重写/覆盖__时
    - 3. 当不希望__类中的某个属性被修改时__
    - 4. 当不希望__某个局部变量被修改时__



- __使用细节__

  -  ___final___ 修饰的属性也叫做__常量__，一般以 XX_XX_XX 的方式命名

  -  ___final___ 修饰的属性在定义后__必须赋初值且无法再修改__，赋值可以在以下位置之一：

    - 1. 定义的时候
    - 2. 代码块中
    - 3. 构造器中__（如果 _final_ 修饰的是静态属性，则只能在 _定义时或者静态代码块_ 赋值，不能在构造器中赋值，因为构造器、普通代码块和静态属性的优先级不同）__

  - 若类不是 ___final___ 类，但含 ___final___ 方法，__则虽该方法不能被重写/覆盖，但可以被继承__

  - 若一个类是 ___final___ 类，则里面的方法一般不用再加 ___final___ 修饰

  -  ___final___ 不能修饰构造方法

  -  ___final___ 和 ___static___ 搭配使用效率更高，不会导致类的加载，底层编译器做了优化处理

    ```java
    public class Use {
    
        public static void main(String[] args) {
            System.out.println(A.name);
        }
    }
    
    class A{
        public final static String name = "星瞳";
    
        static{
            System.out.println("23岁是学生");
        }
    }
    /*
    此处输出为：星瞳
    利用 final static 不会导致类加载，因而静态代码块不会被执行，从而就没有输出了
     */
    ```

  - 包装类__（例如_ Integer, Double, Float, Boolean, String_)__ 等都用了 ___final___ 修饰 



#### 抽象类

- __使用场景__

  - 父类的方法__需要声明但不确定如何实现__，可以用 ___abstract___ 关键字修饰，声明该类为抽象方法，__该方法没有实体__，而该类也需要声明为抽象类（即解决父类方法不确定性问题）

    
  
- __介绍__

  - __格式：__  ___访问修饰符 abstract 类名{___

    ___访问修饰符 abstract 方法名(参数列表);___

    ___}___

  - 抽象类价值更多在于设计，通常会让子类继承并实现抽象类（）

  - __容易被问到，在框架和设计模式中用的比较多__



- __细节__
  - 抽象类中可以没有抽象方法
  - ___abstract___ 只能修饰类和方法，不能修饰其他的
  - 抽象类不能被实例化（即不能new创建对象）
  - 抽象方法不能有主体
  - 抽象类中可以有类的各种成员（它的本质还是类）
  - __若一个类继承了抽象类，那么它必须实现抽象类的所有抽象方法（即有方法体），除非它自己也是抽象类__
  - __抽象方法不能用 private，final 和 static 修饰，因为这些关键字与方法的重写相违背，如果用这些关键字后子类无法再实现抽象方法__



- __抽象的实践 — 模板设计模式__

  - __需求__

    - 1. 需要多个类，完成各自不同的功能
    - 2. 要求统计得到各自完成的时间
         

  - __实现__

    - 设计一个父类模板，将重复的部分设计一个普通类，再将每个工作类都不同的部分写成抽象类放到普通类中，再在工作的子类中重写该抽象方法用来补充，最后子类调用父类的普通类即可完成各自的功能

    - 代码

      ```java
      public class Use {
      
          public static void main(String[] args) {
      
      //      调用模板类的普通类即可
              A a = new A();
              a.running();
      
          }
      }
      
      abstract class template{
      
          public abstract void work();
      
          public void running(){
              //.....重复的部分
              work();
          }
      
      }
      
      class A extends template{
          public void work(){
              //...子类需要做的功能
          }
      }
      ```



#### 接口（Interface)

- __基本介绍__

  - 给出一些没有实现的方法，封装到一个接口中，某个类需要使用时再具体实现___（implements）___这些方法

    

- __使用__
  - 1. 创建接口，在其中编写利用抽象类需要的__规范__
  
  - 2. 在类中实现接口，把接口中的抽象类重写，以具体实现
  
  - 3. 语法：
  
       ___interface 接口名{___
  
       ​		___属性___
  
       ​		___方法 (抽象方法，默认实现方法，静态方法)___
  
       ​		___(jdk8 后可以利用 default 修饰方法即可变成可实现的方法，接口中默认为抽象方法，可以不加 abstract 修饰)___
  
       ___}___
  
       
  
       ___class 类名 implements 接口名{___
  
       ​		___自己的属性和方法___
  
       ​		___实现接口的抽象方法___
  
       ___}___
  
       
  
- __细节__

  - 1. 接口不能被实例化

  - 2. 接口中所有方法默认为 ___public___ 方法，其中的抽象方法可以不用 ___abstract___ 修饰

  - 3. 若一个普通类实现接口，必须把接口中所有的方法都实现（快捷键 ___alt + enter___)

  - 4. 抽象类实现接口可以不用实现接口的所有方法

  - 5. 一个类可以同时实现多个接口，将多个接口的名字以逗号的方式陈列在 ___implements___ 后面

  - 6. 接口中的属性修饰符默认且只能为 ___public static final___，因此必须初始化

  - 7. 接口的修饰符只能是 ___public 和 默认___ , 和类的修饰符一样

  - 8. 接口的属性的访问语法：___接口名.属性名___

  - 9. 接口不能继承其他类，但可以继承多个别的接口

       
  
- __接口和继承(interface and extends)__

  - _不同_

    - 继承：主要解决代码的复用性和可维护性

    - 接口：设计各种规范，让其他类去实现

    - 接口比继承更加灵活，继承满足__is-a 的关系 __,接口满足 __like-a的关系__

    - 接口在一定程度上实现代码解耦（接口规范性 + 动态绑定）

      

  - _细节_

    - 子类继承父类则自动拥有其功能，而接口则需要再实现

    - __接口可以看作为单继承机制的补充__，即可以让子类使用接口来扩展功能

      例如 ___class man extends person implements students{}___

      

- __接口的多态特性__

  - _接口类型的变量可以指向实现了接口的对象实例_
  
    ​	
  
    ```java
    class using{
        public static void main(String[] args) {
            //接口类型的变量可以指向实现了接口的类的对象实例
            itf itf1 = new A();
            itf itf2 = new B();
        }
    }
    
    //接口
    interface itf{
        public void work();
    }
    //两个类实现接口
    class A implements itf{
        public void work(){
    
        }
    }
    class B implements itf{
        public void work(){
    
        }
    }
    ```
  
    ​	
  
  - _接口类型数组_
  
    ```Java
    class using{
        public static void main(String[] args) {
    
        	//接口多态数组    
            itf aitf [] = new itf[2];
            aitf[0] = new phone();
            aitf[1] = new keyboard();
    
            for (int i = 0; i < aitf.length; i++){
                aitf[i].work();
                //这里和继承多态数组类似，由于phone类没有input方法，所以需要先用instanceof判断是否是keyboard类
                if (aitf[i] instanceof keyboard)
                    ((keyboard)aitf[i]).input();
            }
        }
    }
    
    interface itf{
        public void work();
    }
    class phone implements itf{
        public void work(){
            System.out.println("手机正在连接");
        }
    }
    class keyboard implements itf{
        public void work(){
            System.out.println("键盘正在连接");
        }
    
        public void input(){
            System.out.println("键盘正在输入");
        }
    }
    /*
    输出：
    手机正在连接
    键盘正在连接
    键盘正在输入*/
    ```
  
    
  
  - _接口的多态传递现象_
  
    - 接口类型的变量可以指向实现了接口的对象实例，但此时另一个接口指向该实例则会报错，__而如果该接口和第一个接口为继承关系时，则该接口也可以指向实现了第一个接口的对象实例（_注意是第一个接口继承第二个接口_）__
    
    ```java
    class using{
        public static void main(String[] args) {
            //接口类型的变量可以指向实现了接口的类的对象实例
            itf itf1 = new A();
            intf itf2 = new A();
        }
    }
    
    //接口
    interface itf{
        public void work();
    }
    interface intf extends itf{
        
    }
    
    //A类实现接口 itf
    class A implements itf{
        public void work(){
    
        }
    }
    ```

​					

#### 内部类

- _作用_
  - 1. 当某个类只为一个类提供服务时，可以将这个类定义成内部类以免去重新创建一个类的操作
  - 2. 可以解决接口或者抽象类不能实例化的问题



- _定义_

  一个类的内部又完整的嵌套了另一个类结构，被嵌套的类称为内部类，嵌套其他类的类称为外部类。_内部类最大的特点是可以直接访问私有属性，并且可以体现类与类之间的包含关系，是类的第五大成员___(属性，方法，构造器，代码块，内部类)___

  

- _基本语法_

  ```Java
  //外部其他类
  class A{
      
  }
  
  //外部类
  class Outer {
      public int a = 1999;
      public void ot(){
  
      }
      {
          System.out.println("xt");
      }
      //内部类
      class inner{
  
      }
  }
  ```

  

- _分类_

  - 定义在外部类的局部位置上（比如方法内）
    - 局部内部类（有类名）
    - 匿名内部类（没有类名，__重点__）
  - 定义在外部类的成员位置上
    - 成员内部类（无 ___static___）
    - 静态内部类（有 ___static___）

  

- _说明_

  - __局部内部类（局部内部类是定义在外部类的局部位置上，通常为方法内）__

    - 1. 可以直接访问外部类的所有成员，且__包括私有__的
    - 2. __它的地位相当于局部变量，不能添加访问修饰符，但能添加_final___，可以被继承
    - 3. __作用域：在定义它的方法或者代码块中__
    - 4. 外部类在方法中可以通过创建内部类对象来调用局部内部类
    - 5. __本质仍为一个类__		
    - 6. 因为局部内部类是__局部变量__，所以外部其他类无法访问局部内部类
    - 7. 若外部类和局部内部类的成员__重名__，遵循就近原则，若想访问外部类的成员，使用	__外部类名._this_.成员__ 的格式去调用，__此时谁调用了外部类，这里的外部类名._this_就是哪个对象__

    - _代码示例_

      ```Java
      //外部其他类
      public class using{
          public static void main(String[] args) {
              outer O = new outer();
              O.method();
          }
      }
      //亟待被实现的接口
      interface Interface01 {
          public void sayHello();
      }
      //外部类
      class outer{
      //外部类的属性
          public int age = 18;
          public String name = "陈琳";
      //将 内部类 定义在 外部类的方法中
          public void method(){
              //内部类
             class inner{
                 //内部类属性和方法
                 public String name = "星瞳";
                 public void say(){
                     //就近原则，因此第一个是内部类中的name
                     System.out.println(name);
                     //利用外部类.this.属性 的格式可以指定访问外部类属性
                     System.out.println(outer.this.name);
                 }
             }
             inner innner01 = new inner();
             innner01.say();
                 //由于就近原则，所以内部类中输出的是内部类中的name，而下面一行输出的是外部类中的name
              System.out.println(name);
          }
      }
      //输出：
      //星瞳
      //陈琳
      //陈琳
      ```

    

  - __匿名内部类__（这里很关键！__定义位置和局部内部类相同，区别是没有类名__）

    - 1. 本质是一个类，且为内部类，__同时是一个对象__,但没有类名__（系统自动分配的，但隐藏了无法看到）__

    - 2. _基本语法_

         ```java
          new 类或接口 (参数列表){
             类体
         };
         ```

    - 3. _满足何种需求？_  当程序需要使用一个类但只会用到一次，此时如果单独写一个类来实现会有些浪费，因为只用到一次，所以使用匿名内部类。

    - 4. _使用例子_

         ```Java
         public class using{
             public static void main(String[] args) {
         //在外部其他类中创建外部类对象并调用外部类方法来调用匿名内部类        
                 outer O = new outer();
                 O.method();
             }
         }
         //接口（需要在匿名内部类中实现）
         interface Interface01 {
             public void sayHello();
         }
         //外部类
         class outer{
         //外部类成员在匿名内部类中可以访问
             public int age = 18;
             public String name = "陈琳";
         //通过一个方法 将 匿名内部类定义在 里面
             public void method(){
                 //定义匿名内部类
                 //该处隐藏了 创建了一个新的类继承 Interface01 类的代码，因此此处实际上是一个新的类，所以可以重写原来类的方法
                 Interface01 op = new Interface01(){
                     @Override
                   public void sayHello(){
                       System.out.println(name + "今年" + age);
                   }
                 };
         //调用匿名内部类的方法
                 op.sayHello();//动态绑定机制
             }
         }
         ```
    
    - 5. _底层隐藏命名规律_：__类名 + $ + 序号（表示第几个匿名内部类，从1开始）__
    
    - 6. _编译类型和运行类型_
    
      ```java
      //此处是匿名内部类代码
      public void method(){
          //不难看出编译类型仍为 Interface01
          //然而此处的运行类型是该匿名内部类，并不是 Interface01
          Interface01 op = new Interface01() {
              @Override
              public void sayHello() {
                  System.out.println(name + "今年" + age);
              }
          };
          op.sayHello();
      }
      ```
    
    - 7. _不同的调用用法_
    
         ```Java
         public void method(){
             // new Interface01 (){} 本质上还是一个对象，因此可以调用方法
             new Interface01() {
                 @Override
                 public void sayHello() {
                     System.out.println(name + "今年" + age);
                 }
             }.sayHello();
             
         }
         ```
    
    - 8. 可以直接访问外部类的所有成员，且__包括私有__的
    
    - 9. 它的地位相当于局部变量，不能添加访问修饰符
    
    - 10. __作用域：在定义它的方法或者代码块中__
    
    - 11. 若外部类和匿名内部类的成员__重名__，遵循就近原则，若想访问外部类的成员，使用	__外部类名._this_.成员__ 的格式去调用，__此时谁调用了外部类，这里的外部类名._this_就是哪个对象__
    
    - 12. __实际应用__
    
          - _将匿名内部类当作实参直接传递_
    
            ```java
            public class using{
                public static void main(String[] args) {
                    outer o = new outer();
                    //匿名内部类本身是一个对象，因此可以作为参数
                    o.method(new Interface01() {
                        @Override
                        public void sayHello() {
                            System.out.println(name + "今年" + age);
                        }
                    });//注意括号位置
                }
            }
            
            interface Interface01 {
                public int age = 18;
                public String name = "陈琳";
                public void sayHello();
            }
            
            class outer{
                public static void method(Interface01 op){
                    op.sayHello();
                }
            }
            ```
    
            
  
  - __成员内部类__
  
    - 1. 在外部类的__成员位置__定义，且没有___static___修饰
  
    - 2. 可以直接访问外部类的所有成员，包括私有
  
    - 3. 可以__任意添加访问修饰符__（public protected 默认 private）因为本身是一个成员
  
    - 4. 作用域是整个外部类
  
    - 5. 外部类访问成员内部类需要先 ___new___ 新对象再调用
  
    - 6. 其他外部类访问成员内部类
  
      ```Java
      //其他外部类
      public class hustle {
          public static void main(String args[]){
              //1：通过先创建外部类再创建成员内部类来访问
              cellPhone cp = new cellPhone();
              cellPhone.huawei cph = cp.new huawei();
              //2：本质和第一种一样，但采用简便写法,即将两句结合在一句中
              cellPhone.huawei cph1 = new cellPhone().new huawei();
              //3：利用外部类中写的方法来访问
              cellPhone.huawei cph2 = new cellPhone().gethuawei();
          }
      }
      
      //外部类
      class cellPhone{
          //成员内部类，在外部类成员位置定义
          class huawei{
              public void say(){
                  System.out.println("中华有为！");
              }
          }
          
          //3：通过写个方法来访问成员内部类
          public huawei gethuawei (){
              huawei hw = new huawei();
              return hw;
          }
      }
      ```
  
    - 7. 若外部类和成员内部类的成员__重名__，遵循就近原则，若想访问外部类的成员，使用	__外部类名._this_.成员__ 的格式去调用，__此时谁调用了外部类，这里的外部类名._this_就是哪个对象__
  
         
  
  - __静态内部类__
  
    - 1. 在外部类的__成员位置__定义，有___static___修饰	
  
    - 2. 可以直接访问外部类的所有成员，包括私有
  
    - 3. 可以__任意添加访问修饰符__（public protected 默认 private）因为本身是一个成员
  
    - 4. 作用域是整个外部类
  
    - 5. 外部类访问静态内部类需要先 ___new___ 新对象再调用
  
    - 6. 其他外部类访问静态内部类
  
      ```java
      //其他外部类
      public class hustle {
          public static void main(String args[]){
              //1：通过先创建外部类再创建成员内部类来访问
              cellPhone cp = new cellPhone();
              cellPhone.huawei cph = new cellPhone.huawei();
              //2：本质和第一种一样，但采用简便写法,即将两句结合在一句中
              cellPhone.huawei cph1 = new cellPhone.huawei();
              //3：利用外部类中写的方法来访问,这里的方法可以是 static 也可以是普通方法
              cellPhone.huawei cph2 = new cellPhone().gethuawei();
          }
      
      }
      
      //外部类
      class cellPhone{
          //外部类的静态成员
          public static String name = "星瞳";
          //静态内部类，在外部类成员位置定义
          static class huawei{
              //静态内部类的静态成员和外部类的同名时
              public static String name = "cl";
              public void say(){
                  System.out.println("中华有为！");
                  //就近原则直接访问静态内部类中的 name 属性
                  System.out.println(name);
                  //利用 类名.属性 访问外部类中的 name 属性
                  System.out.println(cellPhone.name);
              }
          }
          //3：通过写个方法来访问成员内部类
          public huawei gethuawei (){
              huawei hw = new huawei();
              return hw;
          }
      }
      ```
  
    - 7. 若外部类和静态内部类的成员__重名__，遵循就近原则，若想访问外部类的成员，使用	__外部类名.成员__ 的格式去调用（因为是静态内部类，不用 this）
    
      

#### 枚举(enumeration)

- _定义_

  ​	即把每个对象具体的__枚举__出来

  

- _应用场景_

  ​	枚举的对象应该包含__有限__的几个__固定值__（__只读取不修改__，例如一年四季只有春夏秋冬是有限且固定的）

  

- _实现方式_

  - 自定义

    ```JAVA
    public class hustle {
        public static void main(String args[]){
            System.out.println(season.SPRING);
        }
    
    }
    
    class season{
        //属性设置私有
        private String name;
        private String weather;
    
        //final + static 即不用创建类也可以使用，且享有系统底层の优化
        public final static season SPRING = new season("春","暖");
        public final static season SUMMER = new season("夏","热");
        public final static season AUTUMN = new season("秋","凉");
        public final static season WINTER = new season("冬","寒");
        
        //利用构造器初始化多个属性，且私有化构造器，即不提供创建该类的其他对象的方法
        private season(String name, String weather) {
            this.name = name;
            this.weather = weather;
        }
        //只提供读取的方法不提供修改的方法
        public String getName() {
            return name;
        }
    
        public String getWeather() {
            return weather;
        }
    
        @Override
        public String toString() {
            return "season{" +
                    "name='" + name + '\'' +
                    ", weather='" + weather + '\'' +
                    '}';
        }
    }
    ```

  - ___enum___关键字

    ```JAVA
    public class hustle {
        public static void main(String args[]){
            System.out.println(season.SPRING);
        }
    
    }
    
    //利用 enum 关键字替代 class
    enum season{
        
        //使用 常量名(参数列表)的方法定义常量对象，对象之间用逗号隔开。
        SPRING("春","暖"), SUMMER("夏","热"), AUTUMN("秋","凉"), WINTER("冬","寒");
    
    
        //属性设置私有，且应该在常量对象的后面定义
        private String name;
        private String weather;
    
    
        //利用构造器初始化多个属性
        private season(String name, String weather) {
            this.name = name;
            this.weather = weather;
        }
        //只提供读取的方法不提供修改的方法
        public String getName() {
            return name;
        }
    
        public String getWeather() {
            return weather;
        }
    
        @Override
        public String toString() {
            return "season{" +
                    "name='" + name + '\'' +
                    ", weather='" + weather + '\'' +
                    '}';
        }
    }
    ```

    
  
- _细节_

  - 1. 利用 ___enum___ 关键字时实际上隐式继承了 ___enum ___类，且是 ___final___ 修饰的（利用 ___javap___反编译）
  - 2. 若用__无参构造器创建对象__，则小括号和参数列表可以省略。(如 SPRING; 和 SPRING();)
  - 3. __枚举对象必须放在枚举类的行首___
  - 4. 使用 ___enum___ 关键字后__不能再继承其他类__，因为隐式继承和 ___Java___单继承机制
  - 5. 虽不能继承其他类，但__可以实现接口__
  - 

- ___enum____常用方法_

   | 方法名      | 详细描述                                                     |
   | :---------- | ------------------------------------------------------------ |
   | _valueOf_   | 利用已定义的对象创建一个参数相同的枚举常量                   |
   | _toString_  | 没有重载时会调用 ___enum___ 类的默认该方法，即返回对象名称，重载后则为重载后的输出 |
   | _name_      | 返回当前对象名称                                             |
   | _ordinal_   | 返回当前枚举对象次序，从0开始编号，按照代码顺序              |
   | _values_    | 返回一个数组，含有所有定义的枚举对象，遍历该数组即依次利用 ___toString___ 方法输出对象 |
   | _compareTo_ | 将两个枚举对象的编号进行比较，返回前者减去后者的编号之差     |
   | _hashcode_  | 返回对象的哈希地址                                           |
   | _equals_    | 利用___"=="___实现判断两个枚举常量是否相等，和___"=="___一致 |
   | _clone_     | 枚举对象不可克隆，为防止子类实现，___enum___ 实现了抛出了一个异常的 ___clone()___ 方法 |

   - _代码演示_

     ```JAVA
     public class hustle {
         public static void main(String args[]){
     
             //name 返回对象名称
             System.out.println(season.SPRING.name());
     
             //toString 默认返回对象名称(父类的toString)，重载后输出对象信息
             System.out.println(season.SPRING);
     
             //ordinal 从0开始编号，返回当前对象次序
             System.out.println(season.AUTUMN.ordinal());
     
             //compareTo 返回前者对象和后者对象的编号差
             System.out.println(season.SPRING.compareTo(season.AUTUMN));
     
             //values 返回了一个存有所有对象信息的数组
             season [] seasons = season.values();
             for (season a : seasons){
                 //这里(类名 要接收返回对象的对象 : 对象数组)
                 // for循环依次从对象数组中取出对象赋值给前者接收的对象
                 System.out.println(a);
             }
     
             //hashCode 返回一个对象的哈希地址
             System.out.println(season.SPRING.hashCode());
     
     
             //valuerOf 创建与参数同名对象相同信息的枚举常量
             season s1 = season.valueOf("SPRING");
             season s2 = season.valueOf("AUTUMN");
     
             //equals 判断两个枚举常量是否相等，利用==实现
             System.out.println(s1.equals(s2));
         }
     
     }
     
     //利用 enum 关键字替代 class
     enum season{
         //使用 常量名(参数列表)的方法定义常量对象，对象之间用逗号隔开。
         SPRING("春","暖"), SUMMER("夏","热"), AUTUMN("秋","凉"), WINTER("冬","寒");
     
     
         //属性设置私有，且应该在常量对象的后面定义
         private String name;
         private String weather;
     
     
         //利用构造器初始化多个属性
         private season(String name, String weather) {
             this.name = name;
             this.weather = weather;
         }
         //只提供读取的方法不提供修改的方法
         public String getName() {
             return name;
         }
     
         public String getWeather() {
             return weather;
         }
     
     
         public String toString() {
             return "season{" +
                     "name='" + name + '\'' +
                     ", weather='" + weather + '\'' +
                     '}';
         }
     }
     /*输出
     SPRING
     season{name='春', weather='暖'}
     2
     -2
     season{name='春', weather='暖'}
     season{name='夏', weather='热'}
     season{name='秋', weather='凉'}
     season{name='冬', weather='寒'}
     381259350
     false
     */
     ```



#### 注解（Annotation）

- _介绍_

  注解也被称为元数据，用于修饰解释包、类、方法、属性、构造器、局部变量等数据信息。和注释一样，注解不影响程序逻辑，__但可以被编译运行__，相当于嵌入代码的补充信息。

  ___@interface___ 表示注解类，而非接口

  

- _功能_

​		在 ___JavaSE___ 中，用来标记过时的功能、忽略警告等；在 ___JavaEE___ 中更重要，例如配置程序的任何切面，代替旧版遗留的冗长代码和 ___XML___ 配置



- _三种常用基本注解_
  - 1. ___@Override___ : 限定某个方法是重写父类方法，只能用于方法,__注解源码为_@Target(ElementType.METHOD)_，（_@Target_ 是修饰注解的注解，称为元注解，括号内表示使用的范围）主要用于语法校验，若没有构成重写则编译时会报错__。不写该注解时也能构成重写
  - 2. ___@Deprecated___: 用于表示某个程序元素（类、方法等）已过时，即不再推荐使用，但能用。__可以做版本升级过渡。注解源码为_@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})___，括号内表示使用范围
  - 3. ___@SuppressWarnings___: 抑制编译器 ___warning___ 警告 ,使其不显示出来。通过括号内填写不同的字符串来抑制不同的警告类型，具体指令可以去查,__格式如 _@SuppressWarnings({"rawtypes", "unchecked", "unused"})_ , 注解源码_@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})___; 括号内表示使用范围，不过__通常放在方法或者类上__



- _元注解_
  - 介绍: 用于修饰其他注解的注解，本身作用不大，但可以了解源码
  - 种类（使用不多，仅作了解）
    - 1.  ___Retention_（保留）__， 指定注解的作用范围和保留时长， 含有三种 ___RetentionPolicy _类型参数SOURCE（表示编译器使用后直接丢弃,不会写入class文件，也不会在runtime生效）, CLASS（编译器将注解记录在class文件中，当运行时，JVM不会保留注解，这是默认值）, RUNTIME（编译器将注解记录在class文件中，当运行时，JVM会保留注解，会在使用时生效，程序可以通过反射获取该注释）__
    - 2. ___Target___, 指定注解可以修饰哪些程序元素，即标注其他注解的__适用元素范围__，在上面有提到
    - 3. ___Documented___, 指定该注解是否会被 ___javadoc__ 工具提取成文档，即在生成文档时，可以看到该注释，定义该注解必须设置 ___Retention___ 的值为 ___RUNTIME___ 
    - 4. ___Inherited___，表示子类会继承父类，若一个类被该注解修饰，则它的子类会自动带有该注解



#### 异常（Exception）

- _概念_

  程序中发生的不正常情况称为 __异常__。

  可分两类，

  - 一类为：___Error___ (错误) 表示___Java___虚拟机无法解决的严重问题   无法用异常处理机制解决，例如栈溢出，内存空间溢出
  - 另一类：___Exception___ 其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码处理，如空指针访问，试图读取不存在的文件，网络连接中断等。 ___Exception___ 也分两类：__运行时异常__ 和 __编译时异常__ ，运行时异常通常可以不处理，但编译时异常我们必须处理

  

- _异常处理机制的作用_

​		程序出现错误后，抛出异常 ___(Exception)___ 导致程序退出，后续代码__不再执行__。这会出现一个不致命的问题就导致整个系统崩溃的情况，由此系统提供了 __异常处理机制__ 从而保证程序的__容错__，使得程序出现异常后得以继续执行



- _使用_

  选中代码块 -> 快捷键 ___ctrl + alt + t___ -> 选中 ___try - catch___



- ___异常体系图___

  ​	很重要，表示了各种异常的继承关系，可在网上搜索



- _常见运行时异常_

  - 1. ___NullPointerException___ 空指针异常： 当程序在需要使用对象的地方 使用了 ___null___ 时出现该异常

       ```Java
       String name = null;
       System.out.println(name.length());
       ```

  - 2. ___ArithmeticException___ 数学运算异常： 出现异常的运算条件时，抛出该异常，例如除以0时

  - 3. ___ArrayIndexOutOfBoundsException___ 数组下标越界异常：恰如其名，当数组下标大于等于数组大小或为负数时会抛出该异常

  - 4. ___ClassCastException___ 类型转换异常：当试图将一个对象的引用强制转换为不相关类型的类的实例时，就会抛出此异常

       ```java
       AA a = new BB();
       BB b = (BB) a;//此处正确，向下转型
       CC c = (CC) a;//此处错误，BB 和 CC没有直接关联
       
       class AA{
       
       }
       class BB extends AA{
       
       }
       class CC extends AA{
       
       }
       ```

  - 5. ___NumberFormatException___ 数字格式不正确异常：  表示在将字符串转换为数字时出现了问题。这通常发生在使用 Java 的内置函数如 ___Integer.parseInt() 或 Double.parseDouble()___ 时，当传递给它们的字符串不是合法的数字格式时。

​					

- _常见编译异常_
  - 1. ___SQLException___ : SQL数据库异常
  - 2. ___IOException___ ：操作文件时异常
  - 3. ___FileNotFoundException___ :操作一个不存在的文件时发生的异常
  - 4. ___ParseException___ : 格式不正确
  - 5. ___IllegaArguementException___ :参数异常
  - 6. ___EOFException___: 操作文件，到文件末尾，发生异常



- _异常处理_

  - 异常处理方式__（两种异常处理方式二选一,如果没有处理异常，默认会使用 _throws_ 方法）__

    - ___try-catch-finally___ ：程序员在代码中捕获发生的异常，自行处理 

      - 语法格式

      ```Java
      try{
      //    代码
      //    此处代码可能会有异常
          
      }catch(Exception e){
          
      //    1. 如果当异常发生时
      //    2. 系统将异常封装成 Exception 对象 e, 传递给 catch
      //    3. 得到异常对象后,程序员自行处理
      //    4. 如果代码没有异常发生, 则 catch 不会执行
      
      }finally{//如果没有 finally 语法上也能通过 
          
      //  1. 无论 try 中是否有异常发生，finally 始终要执行
      //  2. 通常将释放资源的代码放在 finally
          
      }
      ```

      - __注意__

        1.  如果异常发生，则异常后面的代码__不会执行__，而是直接跳入 ___catch___ 块中
        2.  如果__没有异常发生__，则会执行 ___try___ 的代码块，不会进入 ___catch___
        3.  如果希望一段代码__不受异常是否发生影响而执行__，应放到 ___finally___ 中
        4.  可以有多个 ___catch___ 语句，用于捕获不同的异常，从而进行不同的业务处理。__要求子类异常在前，父类异常在后，例如 _NullPointerException_ 在前， _Exception_ 在后， 若发生异常，只会匹配一个对应的 _catch_ 块	__
        5.  只有 ___try-finally___ 也能使用，相当于__没有捕获异常__，但程序会在执行 ___finally___ 后再崩溃；应用于无论有无异常都必须执行某个业务逻辑的场景

      - 简单实践

        ```JAVA
        //  利用 try-catch 完成：若输入的不是整数则让他重复输入直到为整数为止
        public class Use {
            public static void main(String[] args) {
                Scanner mysc = new Scanner(System.in);
        
                while (true) {
                    String num = mysc.next();
                    try {
                        //Integer.parseInt() 是 Java 中用于将字符串转换为 int 类型的方法。它接受一个字符串参数，并返回一个 int 类型的值。如果输入的字符串不是数字，会抛出 NumberFormatException 异常
                        int n = Integer.parseInt(num);
                        
                        break;//如果上述没有异常则表示输入的是整数，因此会执行 break 语句，而不会跳过 break
                    } catch (Exception e) {
                        System.out.println("输入的不是整数");
                    }
        
                }
                System.out.println("整数输入完成");
            }
        }
        ```

        

    - ___throws___ ：将发生的异常抛出，交给调用者（方法）处理，最顶级的处理者是___JVM___。在方法声明中可以声明抛出异常的列表，___throws___ 后面的异常类型可以是方法中产生的异常类型，也可以是父类

      程序在某一个方法出现异常后，并不确定如何处理该异常，可以选择 ___throws___ 即扔给方法的调用者处理，方法的调用者同样可以选择 ___throws___ ，也可以选择 ___try-catch-finally___ 处理，如果一直选择扔异常，最终会到 ___JVM___ 中处理，___JVM___ 会直接输出异常信息并中断程序
    
      - 注意事项和细节
        1. 对于__编译异常__ 程序必须处理，而对于__运行异常__，如果没有处理，则默认处理方式为 ___throws___, 自动抛给 ___JVM___ ，因此会直接中断程序运行
        2. 子类重写父类时，对于异常的规定：子类重写方法的抛出异常类型要么__和父类一样__，要么__是父类抛出异常类型的子类__
        3. 在 ___throws___ 过程中，如果有方法 ___try - catch___ ，则相当于处理异常，就可以不用继续 ___throws___ 
        4.  ___throw 和 throws___
    
    
    
  - _自定义异常_
  
    - 概念 ：程序中出现了某种错误，但该错误信息并没有在 ___Throwable___ 子类中描述处理，此时可以自己设计异常类以作描述
  
    - 设置自定义异常步骤
  
      定义异常类名，继承 ___Exception_（表示编译异常）__ 或 ___RuntimeException_（表示运行异常）__ 
  
    - 实例
  
      ```java
      public class Use {
          public static void main(String[] args) {
      
              int age = 999;
      		//限定年龄范围
              if (age < 0 || age > 120){
                  throw new AgeException("超出年龄范围");
              }
      
              System.out.println("符合正常年龄范围");
      
          }
      }
      
      //一般情况下将自定义异常定义为 RuntimeException 即运行异常，利用默认处理机制，更加方便。
      class AgeException extends RuntimeException {
          public AgeException(String mes) {
              super(mes);
          }
      }
      ```



