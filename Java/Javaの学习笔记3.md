

- __*Set* 接口和常用方法__

  - *基本介绍*
    1. 无序（添加和取出的顺序不一致），没有索引
    2. 不允许重复元素，所以最多包含一个 null
  
- *常用方法*
  
  和*__List__* 接口一样，*__Set__* 也是 *__Collection__* 的子接口，因此常用方法和 *__Collection__* 接口一样


  - *遍历方式*

    同*__Collection__* 的遍历方式一样

    1. 迭代器
    2. 增强 for
    3. __不能用索引的方式来获取__ 


  - *__以 Set 接口的实现类 HashSet 来讲解 Set 接口的方法__*

    ```JAVA
            Set s1 = new HashSet();
            s1.add("hashset1");
            s1.add(2);
            s1.add("three");
            s1.add(null);//Set 实现类对象中可以添加 null
            s1.add(2);//在输出中没有输出两个 2, 表示 Set 实现类对象中不能添加重复元素（和 C++ Stl 中的 Set一样）
    
            //遍历：
            //1. 增强 for 循环
            for (Object ar : s1){
                System.out.println(ar);
            }
            //2. 使用迭代器
            Iterator itr = s1.iterator();
            while (itr.hasNext()){
                System.out.println(itr.next());
            }
            /* 输出：
            null
            2
            three
            hashset1
            null
            2
            three
            hashset1
            证实了添加和取出数据的顺序是不一致的，但每次取出数据的顺序是固定的。
            */
    
    ```


- __*HashSet* __

  - *介绍*

    1. __*HashSet*__ 实际上是 __*HashMap*__
    2. 可以存放 __*null*__ 值，但只能有一个 __*null*__
    3. __*HashSet*__ 不保证元素是__有序__的（和哈希算法有关）
    4. 不能有重复元素（因为它实现了__*Set*__ 接口）

  - *__HashSet__ 底层机制说明*

    __*HashSet*__ 的底层即为 __*HashMap*__ 的底层结构 ，__*HashMap*__ 的底层是 数组 + 单向链表 + 红黑树

    模拟简单的数组 + 链表的结构来 理解底层机制

    - **步骤**

      1. 先获取需要添加元素的**哈希值**

      2. 对哈希值进行运算，得到它在数组中的位置序号

      3. 若没有发生哈希冲突，则直接放在处，若发生哈希冲突，则进行 *__equals 判断__*，若相等，则不再添加（说明元素重复了），若不相等，则以链表的方式挂在该处链表的前一个数据后。
      
      4. 在 *__Java8__* 中当一条链表的元素大小超过*__TREEIFY_THRESHOLD(默认是8)，并且 table 的大小 >= MIN_TREEIFY_CAPACITY(默认64)__*时，就会进行树化(__红黑树__)，红黑树的存储效率高于链表
      
    - **详细底层代码**
    
      以 *__add__* 操作为例
    
      ```Java
      public class Use {
          public static void main(String[] args) {
              HashSet hs1 = new HashSet();
              hs1.add("星瞳");
              hs1.add("aytdd");
          }
      }
      /*
      1.
      public boolean add(E e) { e : "星瞳"
              return map.put(e, PRESENT)==null;
              (static) PRESENT = new Object(); 	
          }
      2.执行 put()，该方法会执行 hash(key) 得到key对应的 hash 值 算法 (h = key.hashcode()) ^ (h >> 16);
      public V put(K key, V value) {//key : "星瞳"
              return putVal(hash(key), key, value, false, true);
          }
      3.
      final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                         boolean evict) {
              定义了辅助变量
              Node<K,V>[] tab; Node<K,V> p; int n, i;
              判断该节点是否为空（即当前还没有元素添加过）或者剩余大小为0
              resize() 函数为扩容函数
              if ((tab = table) == null || (n = tab.length) == 0)
                  n = (tab = resize()).length;  
              判断要加入的元素的哈希值的对应数组位置是否有元素添加过
              if ((p = tab[i = (n - 1) & hash]) == null)
                  tab[i] = newNode(hash, key, value, null);
              else {如果有元素添加过则产生了哈希冲突
                  Node<K,V> e; K k;
                  p 这里指向的是 链表位置的第一个元素
                  如果 p 的哈希值与要添加的元素的哈希值相同，且 p 所指向的元素 和 要添加的元素 是同一个对象，或者 它们的值相同，则不添加
                  if (p.hash == hash &&
                      ((k = p.key) == key || (key != null && key.equals(k))))
                      e = p;
                  另外如果 p 是 一颗红黑树，则会调用 树的算法来判断
                  else if (p instanceof TreeNode)
                      e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
                  如果不满足上述情况，则会将要添加的元素和链表的元素遍历比较，如果链表元素中有与之相等的，则就不添加，若比较完后都没有，则会添加进链表中，添加后会对链表大小立即进行判断是否已经到达 8 个 节点，以判断是否需要树化。
                  进行树化时，要先进行判断
                  //if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY(64))
                  resize();
                  即判断table表（链表数组）大小是否到达64
                  如果上述条件成立，则先进行 table 扩容
                  反之，才会转成红黑树
                  else {
                      for (int binCount = 0; ; ++binCount) {
                          if ((e = p.next) == null) {
                              p.next = newNode(hash, key, value, null);
                              if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                                  treeifyBin(tab, hash);
                              break;
                          }
                          if (e.hash == hash &&
                              ((k = e.key) == key || (key != null && key.equals(k))))
                              break;
                          p = e;
                      }
                  }
                  if (e != null) { // existing mapping for key
                      V oldValue = e.value;
                      if (!onlyIfAbsent || oldValue == null)
                          e.value = value;
                      afterNodeAccess(e);
                      return oldValue;
                  }
              }
              ++modCount;
       threshold 表示 table表 扩容的阈值，该值为（DEFAULT_LOAD_FACTOR*DEFAULT_INITIAL_CAPACITY）即 0.75 * 16 即为 12，扩容后，后者会扩容2倍，即从16变成32，以此类推，因此下次扩容阈值为 0.75 * 32，超过阈值就要扩容以防过多元素添加时剩余空间不够
              if (++size > threshold)
                  resize();
              该函数在 hashMap 中没有实际意义，主要是留给其他类实现
              afterNodeInsertion(evict);
              return null;
          }
      
      */
      
      ```
    



- *__LinkedHashSet__*

  - *说明*

    1. 是 *__HashSet__* 子类

    2. 底层是 *__LinkedHashMap__*，维护了一个数组 + 双向链表

    3. *__LinkedHashSet__* 根据元素的 *__HashCode__* 值来决定元素的存储位置，同时使用链表维护元素的次序，使得元素看起来是以插入顺序保存的（双向链表可以做到）

    4. 不允许添加重复元素
  
  - *底层机制*
  
    1. 和 *__HashSet__* 类似，但把单向链表换作双向链表后，前后添加数据之间会有 *__pre__* 和 *__next__* 互相指向
    2. 第一次添加，数组 *__Table__* 大小扩容至16，存放节点类型是 *__LinkedHashMap$Entry__*
    3. 数组是*__LinkedHashSet$Node__* 存放的元素为 *__LinkedHashMap$Entry__* 类型（用来实现双向链表，它在内部类继承了 *__HashMap.Node<K, V>__*）
  
    

- *__Map接口（非常实用）__*

  - *__Map__ 实现类的特点*

    1. *__Map__* 和 *__Collection__* 并列存在，用于保存具有映射关系的数据：*__Key - Value（双列元素，这点和 C++  STL 中的 map 类似）__*

    2. *__Map__* 中的 *__Key__* 和 *__Value__* 可以是任何引用类型的数据，会封装到*__HashMap$Node__* 对象中

    3. *__Map__* 中的 *__Key__* 不允许重复，当有重复的 *__Key__* 时， 旧的 *__Value__* 会被新的替换，但 *__Map__* 中的 *__Value__* 可以重复

    4. *__Map__* 中的 *__Key__* 和 *__Value__* 都可以 为 *__Null值__*

    5. *__Key__* 大多会用 *__String__* 类数据，当然也可以用其他类型的数据

    6. *__Map__* 中 *__Key__* 和 *__Value__* 存在一对一对应关系，即通过*__Key.get 方法会返回对应的 Value__*

    7. *__Map__* 中一对 *__K-V__* 是放在一个 *__HashMap$Node__* 节点中的，而*__Node__* 实现了 *__Entry__* 接口，因此一对 *__K-V__* 也可被称为一个 *__Entry__*， 而 *__Entry__* 又被转成了 *__EntrySet__*，但除了 *__Node__* 节点中的为存放，其他都只是指向该空间而已。而*__Key 、 Value 的 get 方法在EntrySet中，且可以利用 EntrySet 来遍历 K-V，__*

    8. ```java
       Map map = new HashMap();
       map.put("1", "xt");
       map.put("1.parallel worlds", "xjxt");
       
       Set set = map.entrySet();
       System.out.println(set.getClass());
       //获取类型
       for (Object obj : set){
           //            System.out.println(obj.getClass());//class java.util.HashMap$Node
           //向下转型
           Map.Entry entry = (Map.Entry) obj;
           System.out.println(entry);
       }/*
       输出：
       class java.util.HashMap$EntrySet
       1=xt
       1.parallel worlds=xjxt
       */
       
       ```

  - *常用方法 - > 腾讯文档*

    - *遍历方法*

      ```java
      Map map = new HashMap();
      map.put("1", "xt");
      map.put("parallel worlds", "xjxt");
      map.put(3, "PO8");
      
      Set set = map.keySet();// 通过 keySet 方法获取 key 值
      // 1， 利用增强 for 循环 遍历
      for (Object obj : set){
          System.out.println(obj +" "+ map.get(obj));
      }
      //2. 利用迭代器
      Iterator itr = set.iterator();
      while(itr.hasNext()){
          Object ans = itr.next();
          System.out.println(ans + " " + map.get(ans)) ;
      }
      /*
      输出
      1 xt
      3 PO8
      parallel worlds xjxt
      1 xt
      3 PO8
      parallel worlds xjxt
      
      
      
      
      
      */  
      // 3. 利用 Collection
              //3.1 增强 for 循环
              Collection cll = map.values();
              for (Object obj : cll){
                  System.out.println(cll);
              }
              //3.2 iterator
              Iterator itr1 = cll.iterator();
              while (itr1.hasNext()){
                  System.out.println(itr1.next());
              }
      
              //4. EntrySet
              Set st = map.entrySet();
              //4.1 增强 for 循环
              for (Object oj : st){
                  Map.Entry entry = (Map.Entry) oj;
                  System.out.println(entry.getKey() + " " + entry.getValue());
              }
              //4.2 iterator
              Iterator ir = st.iterator();
              while (ir.hasNext()){
                  Object ojt = ir.next();
                  Map.Entry et = (Map.Entry) ojt;
                  System.out.println(et.getKey() + " " + et.getValue());
              }
      /*
      输出：
      [xt, PO8, xjxt]
      [xt, PO8, xjxt]
      [xt, PO8, xjxt]
      xt
      PO8
      xjxt
      1 xt
      3 PO8
      parallel worlds xjxt
      1 xt
      3 PO8
      parallel worlds xjxt
      */
      ```

  - _**HashMap**小结_

    1. *__Map__* 接口常用实现类：*__HashMap__*、*__Hashtable__*、*__Properties__*
    2. *__HashMap__* 是使用频率最高的
    3. *__HashMap__*，不能保证映射顺序，因为底层是 哈希表
    4. *__HashMap__* 没有实现同步，因此是线程不安全的

  - *__HashMap__ 底层机制及源码解析*

    扩容机制和 *__HashSet__* 完全相同

    底层机制和*__HashSet__* 非常类似
  
  - *__HashTable | Map__的一个实现类*
  
    - _介绍_
  
      1. 存放的仍是键对，即*__K - V__*
      2. *__Hashtable__* 的键和值都不能为 *__null__*
      3. *__Hashtable__* 的使用方法和 *__HashMap__* 基本一样
      4. *__Hashtable__* 线程安全，而 *__HashMap__* 线程不安全
    - _底层说明_
  
      1. 底层是一个数组 *__Hashtable$Entry []__*， 初始化大小为 11
      2. **threshold** 即扩容阈值为 0.75 * 11 = 8
      3. 扩容：按照自己的扩容机制，即新的数组大小为原数组大小 * 2 + 1，即 11 扩容一次后变成 23
  
  - *__Properties | Map__的一个实现类*
  
    - _介绍_
  
      1. *__Properties__* 类 继承自 *__Hashtable__*，并且实现了 *__Map__* 接口，也是以键值对形式来保存数据
      2. 它的使用特点和 *__Hashtable__* 类似
      3. 更多的应用场景是 从 *__xxx. Properties__* 的文件中，  加载数据到 *__Properties__* 类对象，并作读取和修改
      4. 工作后，*__xxx. Properties__* 文件通常作为配置文件（会在 IO流中讲到）   
  
      
  
  - *__TreeSet__*
  
    - *介绍*
  
      1. *__TreeSet__* 可以实现排序功能（与 *__TreeSet__* 最大的区别）
  
      2. 当我们使用无参构造器创建 *__TreeSet__* 时，数据仍然是无序的
  
      3. 可以使用 *__TreeSet__* 提供的构造器，传入一个 *__Comparator__* 比较器来指定排序规则（**匿名内部类**），底层是将匿名对象传给 *__TreeMap 的 Comparator 属性__*
  
    - *排序比较的底层逻辑*（也是 *__TreeMap__* 的比较底层逻辑）*
  
      ```Java
              //创建 TreeSet 对象，并利用 匿名内部类 自定义 Comparator 排序规则
              TreeSet ts = new TreeSet(new Comparator() {
                  @Override
                  public int compare(Object o1, Object o2) {
                      String s1 = (String)o1;
                      String s2 = (String)o2;
                      return (s1.length() - s2.length());//这里是自定义排序，按照字符串长度从大到小排序
                  }
              });
              //开始添加数据 t -> true 添加成功  f -> false 添加失败
              //这里的添加失败与否和比较的底层代码有关
              ts.add("a");//t
              ts.add("b");//f
              ts.add("xt");//t
              ts.add("jr");//f
              ts.add("abc");//t
              ts.add("vtb");//f
      
              for (Object obj : ts){
                  System.out.println(obj);
              }
      //        输出：
      //        a
      //        xt
      //        abc
              
              
      //        底层代码核心
      //		  这里是第二次添加数据后，开始比较的代码，第一次添加数据时不会启用比较器
      //        Comparator<? super K> cpr = comparator; 将比较器对象传给 cpr
      //        if (cpr != null) { // 当 cpr 不为空 ， 即定义了比较器对象、自定义了排序规则时
      //            do {
      //                parent = t;
      //                cmp = cpr.compare(key, t.key);// cmp 表示对前后两个值进行对比
      //                if (cmp < 0)
      //                    t = t.left;
      //                else if (cmp > 0)
      //                    t = t.right;
      //                else {
      //如果两个值相等则直接 return ，并不会添加新的数据，因此前面当新的字符串和旧的字符串有长度相等时，新的字符串不会被添加到 TreeSet 中
      //                    V oldValue = t.value;
      //                    if (replaceOld || oldValue == null) {
      //                        t.value = value;
      //                    }
      //                    return oldValue;
      //                }
      //            } while (t != null);
      ```
  
- *__总结(如何选择集合实现类) - > 腾讯文档__*

  

- __*Collections* 工具类__

  - *介绍*
    1.  __*Collections*__ 是一个操作 __*Set|List|Map*__  等集合的工具类
    2. __*Collections*__ 提供了一系列的静态方法来对集合元素进行排序、查询、修改等操作
  - *常用方法 -> 腾讯文档*

  

  

### 泛型 generic

- **我们为什么需要泛型？**

  **问题：将数据写入到 _ArrayList_ 中**

  **分析：**

  1. 不能对加入到 **_ArrayList_** 中的数据类型进行约束，即当两种不同类型的数据都被加入到  **_ArrayList_** 中时，编译器并不会报错，但这种操作会有隐患
  2. 遍历的时候，需要进行类型转换，如果集合中的数据量大，则会对效率有影响，因为 一个类型的元素要先转成  **_Object_** 类型，再转成相应类型存放

- **介绍**

  1.  泛型又称参数化类型，是 **_JDK5.0_** 出现的新特性，解决数据的安全性问题
  2.  在类声明或实例化时只要指定好需要的具体的类型即可
  3.  __*Java*__ 泛型可以保证如果程序在编译时没有发出警告，则运行时不会产生__*ClassCastException*__ 异常。同时代码更简洁
  4.  泛型的作用是：可以在类声明时通过一个标识（ **<>**）表示类中的某个属性的类型，或者某个方法的返回值的类型，或者是参数类型
  
- __声明__

  - *语法*

   `interface Itf<T>{} 和 class Cl<K,V>{}`

  - *注意*
    1. 以上的 _**T 、K、V**_ 都不代表值，而是表示类型
    2. 任何字母都可以，常用  **_Type 的缩写 T表示_**
    3. 一般在创建对象时对泛型进行实例化，且要在 **<> ** 中指定具体的数据类型

- **细节及注意事项**

  1. `interface List<T>{}. public class HashSet<E>{}..等等`

     **以上的 *T 和 E* 只能是引用类型，因此不能是基本数据类型**

     例如，`List<Integer> list = new ArrayList<Integer>();`是正确的

     但，`List<int> list2 = new ArrayList<int>();`是错误的

  2. 在给泛型指定数据类型后，可以传入该类型本身或其子类型。

  3. 实际开发中往往使用泛型的简写，`ArrayList<String> ayl = new ArrayList<>();` 即后面尖括号中的泛型可以省略，编译器会自动推导成前面尖括号中的指定类型。

  4. 如果是这样写，泛型默认是 **_Object_**

     `ArrayList ayl = new ArrayList();`

- **自定义泛型类**

  - *语法*

    `class 类名 <T,R.....>{} 此处也可以是接口 `

  - *细节*
    
    1. 普通成员可以使用泛型（属性、方法中都可以用）
    2. 使用泛型的数组不能初始化（**只能定义，因为在初始化时没有确定泛型的类型，因此不知道开辟空间的大小**）
    3. **静态方法中不能**使用类的泛型（类加载时对象没有被创建因此没有确定泛型的类型，所以静态方法中不能使用类的泛型）
    4. 泛型类的类型，是在创建对象时才确定的（上面有提过）
    5. 如果在创建对象时没有指定类型，则会默认为 **_Object_**

- **自定义泛型接口**

  - *语法*

    `interface 接口名 <T,R.....>{}  `
  
  - *细节*
    
    1. 接口中的静态成员不能使用泛型（和上面类的一样）
    2. 泛型接口的具体数据类型，是在**继承接口或实现接口时确定的**
    3. 没有指定泛型类型时，默认为***Object***
  
- __自定义泛型方法__

  - *语法*

    `修饰符 <T,R.....> 返回类型 方法名（参数列表）{}`

  - *细节*

    1. 泛型方法可以定义在普通类中，也可以定义在泛型类中
    2. 当泛型方法被调用时，**类型必须被确定**

    3. 方法修饰符后没有<T,R...>,这种方法不是泛型方法，而是使用了泛型的普通方法

       例如：`public void hi(T t, R r){}`

    4. 泛型方法可以使用类中已声明的泛型，也可以使用自己声明的泛型

- **泛型的继承和通配符**

  - *说明*

    1. 泛型不具备继承性

       如，`List<Object> list = new ArrayList<String>();`是错误的写法

    2. < ? >: 支持任意类型的泛型

    3. < ? extends A >: 支持A类以及A类的子类，规定了泛型的上限

    4. < ? super A >: 支持A类以及A类的父类，不限于直接父类，规定了泛型的下限

       

###  JUnit的简单使用

- **介绍**

  1. 它是 ***Java*** 语言的单元测试框架
  2. 它可以测试运行局部单个类

- **使用方法**

  1. 在类的前一行写`@Test`
  2. 然后利用快捷键 ***Alt + Enter*** 导入 ***JUnit5.4***
  3. 在左侧会出现绿色标志，可以利用它测试运行单独的类
  
  

 
