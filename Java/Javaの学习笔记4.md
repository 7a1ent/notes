### 多线程Thread（基础）

- **相关概念**

  - *程序*

     简单的讲就是写的代码，是一种完成某种任务，利用语言编写的集合

  - *进程*

    1. 进程是指运行中的程序，操作系统会为进程分配新的内存空间
    2. 进程是程序的一次执行过程，或者是正在运行的一个程序。是一个动态过程，有产生、存在、消亡的过程

  - *线程*

    1. 线程由进程创建的是进程的一个实体
    2. 一个进程可以拥有多个线程
    3. 单线程指同一时刻只能执行一个线程，多线程指同一时刻执行多个线程
    4. 并发，同一时刻多个任务交替执行，比如单核cpu实现多任务
    5. 并行，同一时刻多个任务同时执行，比如多核cpu实现多任务

- **使用**
  1. 当一个类继承了 **_Thread_** 类，该类就能够当作线程使用，也可以通过实现 **_Runnable_** 接口的 **_run_** 方法来创建线程
  2. 一般来说，类继承 **_Thread_** 类之后，都会重写 **_run_** 方法，实现自己的业务逻辑，*__Java__* 中真正实现多线程的是 *__start__* 中的 *__start0__* 方法，*__run__* 方法只是一个普通的方法 
  3. **_run Thread_** 类 实现了 **_Runnable_** 接口的 **_run_** 方法
  
- **多线程执行流程**

  1.  一个进程启动
  2. 开启主线程，即 *__main__* 线程
  3. 主线程会开启另一个子线程，即 *__Thread-0__* 线程（**当 主线程启动一个子线程后，主线程不会阻塞，会继续执行，即会和子线程交替执行，可以利用 _JConsole_ 来查看线程的情况以进行验证**）
  4. 在多线程中，主线程结束时，如果其他子线程仍在运行，则不一定意味着整个进程的结束，当所有线程结束时，这个进程即会结束

- **实例**

  ```Java
  //通过继承 Thread类来创建线程
  public class Use{
      public static void main(String[] args) throws InterruptedException {
          Cat cat = new Cat();
          cat.start();//启动 cat 线程
          //这里不能直接调用 run方法，因为run方法只是一个普通方法，调用它并不会启动线程，而 start方法才是真正实现多线程的方法，它的底层会创建新线程，run方法不会创建新线程，因此要调用 start方法
          //而 start方法中的 start0方法（真正实现多线程的底层方法，通过c/c++实现） 会调用run方法
          for (int i = 0; i < 30; i++){
              System.out.println("i = " + i);
              Thread.sleep(1000);
          }
  
      }
  }
  
  class Cat extends Thread{
      @Override
      public void run() {
          int num = 0;
          while(num < 100){
              System.out.println("cat" + " " + num++);
              try {
                  Thread.sleep(1000);
              } catch (InterruptedException e) {
                  throw new RuntimeException(e);
              }
  
          }
      }
  }
  /*
  输出：
  i = 0
  cat 0
  i = 1
  cat 1
  i = 2
  cat 2
  cat 3
  i = 3
  cat 4
  i = 4
  i = 5
  cat 5
  i = 6
  cat 6
  i = 7
  cat 7
  i = 8
  cat 8
  i = 9
  cat 9
  i = 10
  cat 10
  i = 11
  cat 11
  i = 12
  cat 12
  i = 13
  cat 13
  i = 14
  cat 14
  i = 15
  cat 15
  i = 16
  cat 16
  i = 17
  cat 17
  i = 18
  cat 18
  i = 19
  cat 19
  i = 20
  cat 20
  i = 21
  cat 21
  i = 22
  cat 22
  i = 23
  cat 23
  i = 24
  cat 24
  i = 25
  cat 25
  i = 26
  cat 26
  i = 27
  cat 27
  cat 28
  i = 28
  i = 29
  cat 29
  ....
  cat 99
   */
  ```

```Java
//通过 实现Runnable接口来创建线程
public class Use extends Thread{
    public static void main(String[] args) throws InterruptedException {
        //由于 Java是单继承的，因此当一个类继承了其他类时，则不能使用继承 Thread类的方法来创建线程了
        //因此使用 实现Runnable接口的方法来创建线程
        Thread0 t0 = new Thread0();
        Thread thread = new Thread(t0);
        thread.start();
        //由于 实现 Runnable 接口来创建线程后的类没有 start() 方法
        //因此需要通过创建 Thread对象，再把原来的实现了接口的对象放进去来调用 start() 方法
        //这里使用了设计模式[代理模式] ThreadProxy

    }
}

class Thread0 implements Runnable{
    @Override
    public void run() {
        int num = 0;
        while (num++ < 10)
            System.out.println("这是一个线程" + num);
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

- **继承 *Thread* 和 实现 *Runnable* 的区别**

1. 从 ***Java*** 的设计来看，两者本质上没有区别，***Thread*** 本身也实现了 ***Runnable*** 接口
2. 实现 ***Runnable*** 接口的方式更加适合多个线程共享一个资源的情况，也避免了单继承的限制

- **线程终止**
  
  - *基本说明*
    1. 当线程完成任务后会自动退出
    2. 还可以通过**使用变量** 来控制 **_run_** 方法退出的方法停止线程，即通知方式 （在类中设置一个变量，在 ***main*** 方法中利用 **_set_** 方法控制该变量终止线程）
  
- **线程常用方法 **

  ***-> 腾讯文档***

  - *注意事项*

    1. **_interrupt_** 是中断线程而不是真正的结束线程，一般用于中断正在休眠的线程
    2. **_sleep_** 线程的静态方法，使当前线程休眠
    3. ***run*** 方法只是一个普通方法，调用它并不会启动线程，而 ***start*** 方法才是真正实现多线程的方法，它的底层会创建新线程，***run*** 方法不会创建新线程
    4. 线程优先级的范围：***MAX_PRIORITY 10   MIN_PRIORITY 1  NORM(AL)_PRIORITY 5***（可从底层代码查看）

  - *用户线程和守护线程 (**Daemon**)*

    1. 用户线程：也叫工作线程，当线程任务执行完或用通知方式结束
    2. 守护线程：一般是为工作线程服务的，**当所有的用户线程结束，守护线程自动结束，即若想要子线程在主线程结束后自动结束，则将子线程设置成守护线程即可**
    3. 常用守护线程：垃圾回收机制

    - *使用举例*

    - ```java
      public class Use extends Thread{
          public static void main(String[] args) throws InterruptedException {
      
              Thread0 t0 = new Thread0();
              Thread myt = new Thread(t0);
              myt.setDaemon(true);
              myt.start();
              for (int i = 0; i < 9; i++){
                  System.out.println("Hello " + i);
                  Thread.sleep(500);
              }
      
          }
      }
      
      class Thread0 implements Runnable{
          @Override
          public void run() {
              while (true){
                  System.out.println("HI ");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      throw new RuntimeException(e);
                  }
              }
          }
      }
      ```

- **线程的生命周期**
  - *线程的六大状态 -> 腾讯文档*
    - 将 _**Runnable**_ （可运行）状态细化为两个状态时则为七大状态
    - 可以用 **_线程名.getState()_** 方法来查看线程当前状态
  
- **线程同步机制**

  - *介绍*

    1. 一些敏感数据不允许被多个线程同时访问，此时使用同步访问技术，保证数据在任何同一时刻，**最多只有一个线程访问**，以保证数据完整性

    2. 线程同步，即当有一个线程在对内存进行操作时，其他线程都不可以对这个**内存地址**进行操作，除非此线程完成了操作

  - *具体方法*

    - ***Synchronized***
      1. *同步代码块*(即在代码块中加入 ***synchronized*** 关键字)
      2. *同步方法*(即在方法中加入 ***synchronized*** 关键字)

  - *互斥锁*

    - **介绍**

      1. 关键字 ***synchronized*** 来与对象的互斥锁联系，当某个对象用 ***synchronized*** 修饰时，表明该对象在任意时刻只能由一个线程访问

      2. 同步的局限性： 会导致程序的执行效率降低

      3. 同步方法（非静态）的锁可以是 ***this*** ，也可以是其他对象（要求是同一个对象），是加在对象上的

      4. 同步方法（静态）的锁是加在当前类(***.class***)本身上的

         在静态方法中实现同步代码块，则 ***synchronized 后面的括号必须指定类名***

         `public statci void run(){`

         ​		`synchronized (类名){}`

         `}`

    - **举例**

      ```java
      //同步方法的写法
      public synchronized void go(){
          while (true){
              System.out.println("HI ");
              try {
                  Thread.sleep(1000);
              } catch (InterruptedException e) {
                  throw new RuntimeException(e);
              }
          }
      }
      //同步代码块写法
      public void go(){
          synchronized (this){
              while (true){
                  System.out.println("HI ");
                  try {
                      Thread.sleep(1000);
                  } catch (InterruptedException e) {
                      throw new RuntimeException(e);
                  }
              }
          }
      }
      ```

    -  **注意事项及细节**
      1. 同步方法若没有用 ***static*** 修饰，默认锁对象为 ***this***
      2. 如果方法使用 ***static*** 修饰，默认锁的对象为 **当前类**
      3. 要求多个线程的锁对象为同一个，因为锁加在对象上时，如果不是同一个对象，则会导致锁失效

  - *线程的死锁*

    多个线程都占用了对方的锁资源且互不想让，从而形成死锁。因此要避免死锁的发生

  - *释放锁*

    - **释放锁的几种情况**

      1. 线程的同步方法、同步代码块执行结束
      2. 当前线程在同步方法、同步代码块中遇到 ***break return***
      3. 当前线程在同步方法、同步代码块中出现了未处理的 ***Error 或者 Exception***, 导致异常结束
      4. 当前线程在同步方法、同步代码块中执行了线程对象的 ***wait()*** 方法，当前线程暂停，并释放锁

    - **下列操作不会释放锁**

      1. 当前线程在同步方法、同步代码块中执行时调用 ***Thread.sleep()  Thread.yield() 方法暂停当前线程的执行时，不会释放锁***

      2. 线程执行同步代码块时，其他线程调用了该线程的 ***suspend()*** 方法将该线程挂起，该线程不会释放锁

         **避免使用 *suspend() 和 resume()方法*，两者都不推荐使用**

  

### IO流

- **文件**

  - *文件流*

    文件在程序中是以流的形式来操作的

    ***Java* 程序 （内存）-> 文件 （磁盘）为输出流 ** 输出流，即程序（内存）到数据源（文件）的路径

  ​			 ***Java* 程序 （内存）<- 文件 （磁盘）为输入流 **输入流，即数据源（文件）到程序（内存）的路径

  - *常用的文件操作*

    ​	***File*实现了 *Serializable 和 Comparable 接口*，可以进行比较和串行化**

    - 构造器创建文件

      `new File(String pathname) 根据路径构建一个 File 对象`

      `new File(File parent, String childName) 根据父目录文件+子路径构建`

      `new File (String parent, String childName) 根据父目录 + 子路径构建`

      `createNewFile 创建新文件`

      ```Java
      //只有 file.createNewFile() 这句代码才是真正创建文件的代码
      //根据文件路径创建 file 对象
      String pathName = "d:/coding/news1.txt";
      File file = new File(pathName);
      
      try {
          file.createNewFile();
          System.out.println("创建成功");
      } catch (IOException e) {
          throw new RuntimeException(e);
      }
      
      //根据父目录文件 + 子路径创建
      File parentfile = new File("d:/coding");
      String childName = "news2.txt";
      File file1 = new File(parentfile, childName);
      try {
          file1.createNewFile();
          System.out.println("创建成功");
      } catch (IOException e) {
          throw new RuntimeException(e);
      }
      
      //根据父目录 + 子路径创建
      String parentPath = "d:/coding";
      String childName1 = "news3.txt";
      File file2 = new File(parentPath, childName1);
      try {
          file2.createNewFile();
          System.out.println("创建成功");
      } catch (IOException e) {
          throw new RuntimeException(e);
      }
      ```

    - 获取文件的相关信息

      **腾讯文档**

    - 目录的操作和文件的删除

      `mkdir方法 创建一级目录`

      `mkdirs方法 创建多级目录`

      `delete方法 删除空目录或文件`

  - *流的分类*

    按操作数据单位分：字节流（***8bit***） 字符流（按照字符）

    按数据流的流向分：输入流、输出流

    按流的角色的不同分：节点流、处理流/包装流

    字节流：***InputStream  OutputStream（输入流读取文件内容，输出流写入文件内容）***

    字符流：***Readrer  Writer ***

    以上四个类都是抽象类，需要实现它们的子类来使用它们

  - ***InputStream** 字节输入流*

    ​	***InputStream* 抽象类是所有类字节输入流的超类**

    - ***InputStream*** 常用子类
      - ***FileInputStream：***文件输入流
      - ***BufferedInputStream：*** 缓冲字节输入流
      
      - ***ObjectInputStream：*** 对象字节输入流
      
    - 案例演示
    
    ```Java
    //FileInputStream
    String pathName = "d:/coding/news1.txt";
    try {
        FileInputStream fileInputStream = new FileInputStream(pathName);
        //用于判断读取的情况
        int read = 0;
        while((read = fileInputStream.read()) != -1){
            System.out.print((char) read);
        }
        //以上的第一种方法是按照一个字节一个字节读取文件内容，读取完毕时会返回-1，当遇到中文内容时会出现乱码,因此读取文件时最好采用字符输入流
        //且效率较低，可以考虑用FileInputStream的read()方法优化，一次性读取一个数组大小内容的字节数量
        byte [] buf = new byte[8];
        //利用byte数组读取，每次一个数组长度的字节内容，返回的是每次实际读取的字节数
        // 即最后字节数不够时，会返回实际读取的字节数量，当读取完毕时会返回-1
    
        while ((read = fileInputStream.read(buf)) != -1){
            System.out.print(new String(buf, 0, read));
        }
    
    } catch (Exception e) {
        throw new RuntimeException(e);
    //输出：Apex is a fucking noob game.
    }
    ```
    
    
    
    - ***FileOutputSream*** 字节输出流
    
    ```java
    //根据文件路径写入
            String pathName = "d:/coding/news2.txt";
            try {
                //这种方法创建fileOutputStream文件写入内容时，会覆盖原有的内容
                //而若以 new FileOutputStream(pathName, true) 创建时，则会在原有内容上追加写入
                //因为其中有个 append 参数用来控制是否覆盖原有内容
                FileOutputStream fileOutputStream = new FileOutputStream(pathName);
    
                String str = "Apex is a piece of shit.";
    			//利用 write 方法和 字符串的 getBytes 方法将字符串转成字节写入文件
                fileOutputStream.write(str.getBytes());
    			//该 write 方法是将数组的 off 偏移量位置开始的 len 长度的字节写入文件中
                fileOutputStream.write(str.getBytes(),0, 4);
    
    
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
    
        }
    ```

- **文件拷贝**

  ```Java
  //将需要拷贝的文件读取，再同时将读取的内容输入到需要拷贝的文件中
  String pathName1 = "d:/coding/news2.txt";
  String pathName2 = "d:/coding/news3.txt";
  FileInputStream fileInputStream = null;
  FileOutputStream fileOutputStream = null;
  
  try {
      fileInputStream = new FileInputStream(pathName1);
      fileOutputStream = new FileOutputStream(pathName2);
      //创建一个byte数组用来读取数据，数组大小表示读取数据的个数
      byte [] words = new byte[1024];
      int num = 0;
      while ((num = fileInputStream.read(words)) != -1){
          fileOutputStream.write(words);//一边读取数据一边写入到另一个文件中
      }
  
  
  } catch (Exception e) {
      throw new RuntimeException(e);
  } finally {
      try {
          if (fileInputStream != null) {
              fileInputStream.close();
          }
          if (fileOutputStream !=null){
              fileOutputStream.close();
          }
      } catch (Exception e){
          throw new RuntimeException(e);
      }
  }
  ```



- ***FileReader* 和 *FileWriter***

  - *介绍*

    它们是字符流，即按照字符来操作io

  - *相关方法* -> 腾讯文档

  - *注意*

    ***FileWriter*** 使用后，必须要关闭(***close***)或者刷新(***flush***)，否则写入不到指定的文件, 关闭(***close***) 等于刷新+关闭的操作

  - *示例*

    代码步骤和前面的字节输出流类似，主要是要运用它们自己的方法，然后要注意关闭和刷新 ***FilerWriter***

- **节点流和处理流**

  - *介绍*

    1. 节点流可以从一个特定的数据源（可以是数组、文件等其他数据源）读写数据，如，***FileReader、FileWriter***
    2. 处理流（也称包装流）是连接再已存在的流（节点流或处理流）之上，微程序提供更为强大的读写功能，如，***BufferedReader、BufferedWriter***

  - *节点流和处理流的区别和联系*

    1. 节点流是底层流/低级流，直接和数据源相连
    2. 处理流包装节点流，既可以消除不同节点流的差异，也可以提供更方便的方法来完成输入输出
    3. 处理流对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连（处理流功能主要体现在**1.性能的提高**：主要以增加缓冲的方式来提高输入输出的效率 **2. 操作的便捷**： 处理流可能提供了一系列便捷的方法来一次性输入输出大批量的数据，使用更加灵活方便）

  - ***BufferedReader和BufferedWriter***

    - 它们俩属于字符流，是按照字符来读取数据的，关闭处理流时，只需要关闭外层流即可

    - *示例*

     ```java
      String pathName1 = "d:/coding/news2.txt";
      //表示以追加的方式写入
      BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(pathName1, true));
      
      bufferedWriter.write("Fxxk noob Apex");
      //只需要关闭外层即可，传入的 FileWriter 会在底层代码中关闭
      bufferedWriter.close();
     ```

     

      ```java
      String pathName1 = "d:/coding/news2.txt";
      BufferedReader bufferedReader = new BufferedReader(new FileReader(pathName1));
      String line;
      //按行读取数据，当读取完毕时会返回null
      
      while ((line = bufferedReader.readLine()) != null){
          System.out.println(line);
      }
      //关闭流，只需要关闭BufferedReader，因为底层会自动的去关闭节点流FileR
      bufferedReader.close();
      ```
    
    - *文件拷贝*
    
      和之前的字节流操作类似，不过不能用于二进制文件，用于二进制文件时可能会造成文件损坏
    
  - ***BufferedInputStream和BufferedOutputStream***
  
    字节处理流可以用于处理二进制文件
  
    - *代码示例*
  
      和之前的字节操作流几乎一模一样
  
    
  
  - 对象流 ***ObjectInputStream 和 ObjectOutputStream***
    - *需求*
      
      1. 将 ***int num = 100*** 这个数据保存到文件中，注意不是数字100，而是 ***int 100***,并且能够从文件中直接恢复
      2. 将 ***Dog dog = new Dog("huang", 3)*** 这个 ***dog对象*** 保存到文件中，并且能够从文件中恢复
      3. 上述要求，就是能够将基本数据类型或者对象进行序列化和反序列化操作
      
    - *序列化和反序列化*
      1. 序列化就是在保存数据时，保存数据的值和数据类型
      
      2. 反序列化就是在恢复数据时，恢复数据的值和数据类型
      
      3. 需要让某个对象支持序列化机制，则必须让其类时可序列化的，为了让某个类是可序列化的，***该类必须实现 Serializable（标记接口，里面没有方法，因此一般使用这个） 和 Externalizable 两个接口之一***
      
      4. ***ObjectInpuStream* 负责反序列化，*ObjectOutputStream* 负责序列化**
      
      5. 读取（反序列化）的顺序需要和你保存数据（序列化）的顺序一致
      
      6. 
      
    - *示例*
    
      ```java
      public class Use{
          public static void main(String[] args) throws Exception{
              //ObjectOutputStream 序列化，把数据保存到文件中
              String pathName = "d:/coding/OBJData.txt";
              ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(pathName));
              objectOutputStream.writeInt(100);
              objectOutputStream.writeUTF("UZI");
              objectOutputStream.writeBoolean(false);
              objectOutputStream.writeObject(new Person(18, "张三"));
      
      
              //ObjectInputStream 反序列化， 把文件中的数据恢复
              //反序列化时，读取的数据次序要与序列化保存时的数据次序一致
              ObjectInputStream objectInputStream = new ObjectInputStream(new FileInputStream(pathName));
              System.out.println(objectInputStream.readInt());
              System.out.println(objectInputStream.readUTF());
              System.out.println(objectInputStream.readBoolean());
              System.out.println(objectInputStream.readObject());
              objectOutputStream.close();
              objectInputStream.close();
              /*
              输出：
              100
              UZI
              false
              Person{age=18, name='张三'}
               */
              
          }
      }
      
      //为了序列化，需要实现 Serializable
      class Person implements Serializable{
      
          private int age;
          private String name;
      
          public Person(int age, String name) {
              this.age = age;
              this.name = name;
          }
      
          @Override
          public String toString() {
              return "Person{" +
                      "age=" + age +
                      ", name='" + name + '\'' +
                      '}';
          }
      }
      ```
    
    - *细节及注意事项*
      
      1. 读写顺序须一致
      2. 为了完成（反）序列化操作，要实现 ***Serializable***
      3. 序列化类中建议添加 ***SerialVersionUID***，为了提高版本兼容性
      4. 序列化对象时，默认将里面所有属性都序列化了，***除了Static *或 *transient* 修饰的成员**
      5. 序列化对象时，要求里面的属性类型也需要实现序列化接口
      6. 序列化具备可继承性，即，某类完成序列化后它的子类也实现了序列化
    
  - 标准输出输入流 ***System.in System.out***
  
    - ***System.in*** 
  
      其编译类型为 ***InputStream*** 其运行类型是 ***BufferedInputStream***
  
      表示标准输入 默认位置键盘
  
    - ***System.out***
  
      其编译类型和运行类型都是 ***PrintStream*** 
  
      表示标准输出 默认位置屏幕
  
  - 转换流 ***InputStreamReader OutputStreamWriter***
  
    - *功能*
  
      可以将字节流转换成字符流，且转换时可以指定编码方式，例如 ***utf-8, gbk, ansi等等***，从而解决乱码问题
  
    - *介绍*
  
      1. ***InputStreamReader*** ：***Reader*** 的子类，可以将 ***InputStream*** （字节流）包装（转换）成 ***Reader***（字符流）
      2. ***OutputStreamWriter***：***Writer*** 的子类，可以将 ***OutputStream*** 包装成 ***Writer***
      3. 当处理纯文本数据时，使用字符流效率更高并且可以有效解决中文问题。因此建议将字节流转换成i字符流
      4. 可以再使用时指定编码格式 
  
    - *示例*
  
      `InputStremaReader isr = new InputStreamReader(new FileInputStream(pathName), "gbk");`
  
      将 ***FileInputStream 转换成 InputStreamReader, 且指定编码为 gbk***
  
      `OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(pathName, "gbk"));`
  
      将 ***FileOutputStream 转成 OutputStream，且指定编码为 gbk***
  
      
  
  - 打印流 ***PrintStream PrintWriter***
  
    **打印流只有输出流，没有输入流**
  
    - ***PrintStream*** 字节打印流
  
      默认为标准输出位置，即显示器，但可以修改输出设备
  
      `System.setOut(new PrintStream(pathName));`
  
      即修改输出至 ***pathName*** 的地址
  
    - ***PrintWriter*** 字符打印流
  
      使用效果和上述 ***PrintStream*** 类似    
  
  
  
  - ***Properties* 类**
  
    - *需求*
  
      有个 ***mysql*** 配置文件，需要读取它的信息，例如 ip、user、pwd等
  
      传统方法需要循环读取所有数据并提取，比较麻烦
  
      而 ***Properties*** 则可以轻松实现
  
    - *介绍*
  
      1. 专门用于读取配置文件的集合类，但要求配置文件格式必须为：**键=值**
      2. 注意键值对不需要空格，值不需要用引号，默认类型为 ***String***
  
    - *常见方法*
  
      ***load*** ：加载配置文件的键值对到 ***Properities*** 对象
  
      ***list*** ：将数据显示到指定设备
  
      ***getProperty(key)***： 根据键获取值
  
      ***setProperty(Key,Value)***：设置键值对到 ***Properties*** 对象
  
      ***store***：将 ***Properties*** 中的键值对存储到配置文件。在 ***idea*** 中，保存信息到位置文件时，如果含有中文，会存储为 ***unicode*** 码
  
    - *示例*
  
      **-> 腾讯文档**

### 网络

- **网络的概念**

![{367A68CE-C518-4d5a-A225-12FC8C9E2659}](C:\Users\tyx\AppData\Local\Temp\{367A68CE-C518-4d5a-A225-12FC8C9E2659}.png)

  ​    

​      ![image-20231030140157904](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030140157904.png)

![image-20231030140223728](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030140223728.png)

***IPV6拥有128位（一个字节使用8位二进制数，即16个字节），是IPV4的十倍***

![image-20231030141622997](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030141622997.png)

![image-20231030141945575](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030141945575.png)

![image-20231030143609658](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030143609658.png)

**通过IP找到主机，再通过端口号访问具体的主机的各项服务**

![image-20231030144913477](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030144913477.png)

![image-20231030151437980](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030151437980.png)

![image-20231030152710155](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030152710155.png)

- ***InetAddress* 类**

  - *常用方法示例*

    ![image-20231030153811031](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231030153811031.png)

- ***Socket***

  - *介绍*

    1. 套接字（***Socket***）开发网络应用程序被广泛采用，以至于成为事实上的标准
    2. 通信的两端都要有 ***Socket*** ，是两台机器间通信的端点
    3. 网络通信实际上就是 ***Socket*** 间的通讯
    4. ***Socket*** 允许程序把网络连接当成一个流，数据在 ***Socket*** 间通过 ***IO*** 流传输
    5. 一般主动发起通信的应用程序是客户端，等待通信的为服务端
    6. 底层使用的是 ***TCP/IP*** 协议

  - *示意图*

    ![image-20231118155318014](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231118155318014.png)
  
  - *代码示例*
  
    ```Java
    //客户端代码
    package Com.learning.project2;
    
    import java.io.BufferedReader;
    import java.io.InputStream;
    import java.io.InputStreamReader;
    import java.io.OutputStream;
    import java.net.InetAddress;
    import java.net.Socket;
    import java.net.UnknownHostException;
    
    public class SocketCilent {
    
    public static void main(String [] args) throws Exception {
    
        //1. 根据主机IP 和 需要连接的端口号创建 Socket 对象
        Socket socket = new Socket(InetAddress.getLocalHost(), 9999);
        System.out.println("Socket 对象创建成功");
    
    
        //得到和 Socket 对象所关联的输出流
        //字节流写入数据
        OutputStream outputStream = socket.getOutputStream();
        outputStream.write("It's SocketCilent, u know?".getBytes());
        socket.shutdownOutput();;//该句代码表示输出流的结束，给其他端一个结束输出的信号，使程序得以继续
        //将字节流转换成字符流
        InputStream inputStream = socket.getInputStream();
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(inputStream));
        String s = bufferedReader.readLine();
        System.out.println(s);
    //字节流
    //    byte[] buf = new byte[1024];
    //    int readlen = 0;
    //    while ((readlen = inputStream.read(buf))!= -1){
    //        System.out.println(new String(buf, 0, readlen));
    //    }
    
        //关闭流对象 和 Socket
        outputStream.close();
        bufferedReader.close();
    //    inputStream.close();
        socket.close();
        System.out.println("客户端已退出");
    
    
    	}
    
    
    }
    
    
    //服务端代码
    package Com.learning.project2;
    
    import java.io.*;
    import java.net.ServerSocket;
    import java.net.Socket;
    
    public class SocketServer {
        public static void main(String[] args) throws IOException {
    
            //1. 对需要连接的端口号进行监听，准备连接，如果没有连接程序则会阻塞
            //此处需要本机没有其他服务在监听该端口
            ServerSocket serverSocket = new ServerSocket(9999);
            System.out.println("服务端在 9999 端口监听，等待连接");
    
            //2.从serverSocket 的 accept 方法 获取 Socket 对象
            //可以通过 accept 方法返回多个 Socket 对象（多个客户端连接服务器的并发）
            Socket socketaccept = serverSocket.accept();
    
            System.out.println("成功返回 Socket 对象");
    
            //3. 获取与 Socket 关联的输入流对象， 获取客户端的输出数据
            //字节流读取数据
            InputStream inputStream = socketaccept.getInputStream();
            byte[] buf = new byte[1024];
            int readlen = 0;
            while ((readlen = inputStream.read(buf))!= -1){
                System.out.println(new String(buf, 0, readlen));
            }
            //将字节流转换成字符流
            OutputStream outputStream = socketaccept.getOutputStream();
            BufferedWriter bufferedWriter = new BufferedWriter(new OutputStreamWriter(outputStream));
            bufferedWriter.write("I see, and It's SocketServer.");
            bufferedWriter.newLine();//插入换行符表示写入内容结束
            bufferedWriter.flush();//使用字符流时，需要手动刷新，否则数据不会写入数据通道
    //        outputStream.write("I see, and It's SocketServer.".getBytes());
    //        socketaccept.shutdownOutput();//该句代码表示输出流的结束，给其他端一个结束输出的信号，使程序得以继续
            //也可以使用writer.newLine(); 来结束，但需要对面端口使用 readLine(); 来读取该行代码
            //4.关闭输入流 和 socket
    //        inputStream.close();
            bufferedWriter.close();
            outputStream.close();
            socketaccept.close();
            serverSocket.close();
            System.out.println("服务端已关闭");
    
        }
    }
    
    ```

- ***netstat** 指令*
  1. ***netstat -an*** 可以查看当前主机网络情况，包括端口监听情况和网络连接情况
  2. ***netstat -an|more***  可以分页显示
  3. 要求在 ***dos*** 控制台下执行

- ***Tips***

  - 当客户端连接到服务端时，实际上客户端也是通过一个端口和服务端进行通讯的，这个端口是 ***TCP/IP*** 负责进行随机分配的。

- ***UDP** 网络通信编程【仅作了解】*

  ![image-20231121143336911](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231121143336911.png)![image-20231121143500090](C:\Users\tyx\AppData\Roaming\Typora\typora-user-images\image-20231121143500090.png)

​		
