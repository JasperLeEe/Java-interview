## Java线程

1. 线程的状态

   + 就绪（Runnable）
   + 运行中（Running）
   + 等待中（Waiting）
   + 睡眠中（Sleeping）
   + I/O阻塞（Blocked on I/O）
   + 同步阻塞（Blocked on Synchronization）
   + 死亡（Dead）

2. Monitor

   Monitor是一个同步工具，相当于操作系统中的互斥量，即值为1的信号量。

   它内置与每一个Object对象中，相当于一个许可证。拿到许可证即可以进行操作，没有拿到则需要阻塞等待。

3. Synchronized实现原理

   又叫内置锁。使用synchronized加锁的同步代码块在字节码引擎中执行时，其实是通过锁对象的monitor的取用与释放来实现的。由上面我们知道monitor是内置与任何一个对象中的，synchronized利用monitor来实现加锁解锁，故synchronized又叫内置锁。




## Java集合类

1. 为什么集合类没有实现Cloneable和Serializable接口？

   Collection是一个抽象表现。重要的是实现。

   当与具体实现打交道的时候，克隆或序列化的语义和含义才发挥作用。所以具体实现应该决定对它进行克隆或序列化。

2. Iterator迭代器

   Iterator接口提供遍历任何Collection的接口。可以从一个Collection中使用迭代器方法来获取迭代器实例。

3. HashSet和TreeSet的区别

   HashSet是由一个hash表来实现的，他的元素是无序的。add() remove() contains()方法的时间复杂度是O(1)。

   TreeSet是由一个树形的结构来实现的，它里面的元素是有序的。add() remover() contains方法的时间复杂度为O(logn)。




## Java基本类型

### 整形

|  类型   | 存储需求 |      取值范围      |
| :---: | :--: | :------------: |
|  int  | 4字节  | -2^31 ~ 2^31-1 |
| short | 2字节  | -2^15 ~ 2^15-1 |
| long  | 8字节  | -2^63 ~ 2^63-1 |
| byte  | 1字节  |  -2^7 ~ 2^7-1  |

### 浮点型

|   类型   | 存储需求 |    取值范围    |
| :----: | :--: | :--------: |
| float  | 4字节  | 有效位数6 ～ 7位 |
| double | 8字节  |  有效位数15位   |



float单精度

double双精度

### char类型

|  类型  | 存储需求 |   取值范围    |
| :--: | :--: | :-------: |
|      | 2字节  | unicode编码 |



### boolean类型

|   类型    | 存储需求 |    取值范围    |
| :-----: | :--: | :--------: |
| boolean | 1bit | true／false |



## JVM加载字节码文件

虚拟机把描述类的数据从Class文件加载到内存，并对数据进行校验、转换解析和初始化，最终形成可被虚拟机直接使用的Java类型，这就是虚拟机的类加载机制。

Java语言中类的加载、连接和初始化过程都是在程序运行期间完成的，令Java具备高度灵活性。

类加载的过程：加载、连接（验证、准备、解析）、初始化。

+ 加载：通过一个类的名字获取此类的二进制字节流；将这个字节流代表的静态存储结构转换为方法区的运行时结构；在内存中生成一个java.lang.Class对象，最为方法区这个类各种数据结构的访问入口。
+ 验证：文件格式验证、元数据验证、字节码验证、符号引用验证。
+ 准备：正式为类变量分配内存并设置初始值，这里的类变量指的是被static修饰的变量。如果类字段是常量，则在这里会被初始化为表达式指定的值。
+ 解析：将常量池的符号引用替换为直接引用。
+ 初始化：真正开始执行类中定义的Java程序代码；初始化用于执行Java类的构造方法。类初始化的过程是不可逆的，如果中间一步出错，则无法执行下一步。




## GC

__"Stop the world"__：会在任何一种GC算法中发生。Stop-the-world意味着JVM因为要执行GC而停止了应用程序的执行。当Stop-the-world发生时，除了GC以外的所有线程都处于等待状态，直到GC任务完成。GC优化很多时候就是指减少Stop-the-world发生的时间。

#### 按代的垃圾回收机制

垃圾回收器会在下面两种前提成立的情况下被创建。

+ 大多数对象会很快变得不可达
+ 只有很少的由老对象指向新对象的引用

__新生代__：绝大多数最新被创建的对象会被分配到这里，由于大多数对象被创建后很快变的不可达，所以很多对象被创建在新生代，然后消失。对象从这个区域消失的过程我们称为__“minor GC”__。

__老年代__：对象没有变的不可达，并且从新生代中存活下来，会被拷贝到这里。其所占用的空间比新生代多。也正由于其相对较大的空间，发生在老年代上的GC要比新生代少得多。对象从老年代消失的过程，称为__“major GC”__。

![GC 空间 & 数据流](http://www.importnew.com/wp-content/uploads/2012/12/Figure-1-GC-Area-Data-Flow.png)

上图中持久带（Permanent Generation）也被称为方法区。用来保存类常量以及字符串常量。因此，这个区域不是用来永久存储哪些从老年代存活下来的对象。这个区域也可能发生GC。并且发生在这个区域上的GC事件也会被算为__major GC__。

![](http://www.importnew.com/wp-content/uploads/2012/12/Figure2-Card-Table-Structure.png)

__Question：如果老年代的对象需要引用一个新生代的对象，会发生什么？__

为了解决这个问题，老年代中存在一个“card table”，他是一个512 byte大小的块。所有老年代的对象指向新生代的引用都会记录在这张表里。当新生代执行GC的时候，只需要查询card table来决定是否可以被收集。

#### 新生代的构成

新生代是用来保存那些第一次被创建的对象的，可以分为三个空间：

+ 一个伊甸园空间（Eden）
+ 两个幸存者空间（Survivor）

每个空间的执行顺序：

1. 绝大多数刚被创建的对象会存放在Eden中。
2. 在Eden执行了第一次GC后，存活的对象会被转移到其中一个Survivor。
3. 之后Eden执行GC后，存活的对象会被堆积在同一个Survivor。
4. 当一个Survivor饱和，其中还在存活的对象会移动到另一个Survivor。之后清空已经饱和的那个Survivor。
5. 以上步骤重复几次依然存活的对象，会被移动到老年代。

__其中一个Survivor必须保证为空。__

#### 老年代GC处理机制

5种GC类型：

1. Serial GC
2. Parallel GC
3. Parallel Old GC
4. Concurrent Mark & Sweep GC（or "CMS"）
5. Garbage First（G1） GC

Serial GC不应被用在服务器上，使用Serial GC会显著降低应用的性能指标。

1. __Serial GC__

   老年代空间中的GC采取称之为“mark-sweep-compact”算法。

   1. 标记老年代中依然存活的对象。（标记）
   2. 从头开始检查内存空间，并且只留下依然幸存的对象。（清理）
   3. 从头开始顺序的填满内存空间，并且将内存空间分成两部分：一个保存着对象，另一个空着

2. __Parallel GC__

   ![](http://www.importnew.com/wp-content/uploads/2012/12/Figure-4-Difference-between-the-Serial-GC-and-Parallel-GC.png)

   Serial GC只使用一个线程执行GC，而Parallel GC使用多个线程，因此Parallel更高效。

3. __Parallel Old GC__

   与Parallel GC相比，唯一的区别在于针对老年代的GC算法。Parallel Old GC分为: mark-summary-compact。Summary步骤与Sweep的不同之处在于，其将依然幸存的对象分发到GC预先处理好的不同区域。

4. __CMS GC__

   ![](http://www.importnew.com/wp-content/uploads/2012/12/Figure-5-Serial-GC-CMS-GC.png)

   1. 初始化标记：只查找那些距离类加载器最近的幸存对象。因此，停顿的事件非常短暂。
   2. 并行标记：所有幸存对象引用的对象会被确认是否已经被追踪和校验。其它线程依旧执行。
   3. 重新标记：再次检查那些并行标记步骤中增加活着删除的与幸存对象引用的对象。
   4. 并行清扫：转交垃圾回收过程处理。

   一旦采用这种GC类型，由GC导致的暂停时间会极其段战。CMS GC也被称为低延迟GC经常被用在那些对于响应时间要求十分苛刻的应用之上。

   __缺点：__

   + 比其它GC占用更多的内存和CPU
   + 默认情况下不支持压缩步骤

5. __G1 GC__

   ![](http://www.importnew.com/wp-content/uploads/2012/12/Figure-6-Layout-of-G1-GC.png)

   每个对象被分在不同格子，随后GC执行。当一个区域装满之后，对象被分配到另一个区域，并执行GC。不再有从新生代移动到老年代的三个步骤。