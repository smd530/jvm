# JVM参数类型

## 标配参数

- java -version
- java -help

## X参数（了解）

- -Xint（解释执行）
- -Xcomp（第一次使用就编译成本地代码）
- Xmixed（混合模式）

## XX参数（重点）

- 布尔类型

  - 公式
    - -XX:+某个属性值 
    - -XX:-某个属性值 
  - Case
    -  -XX:+PrintGCDetails 
    - -XX:-PrintGCDetails 是否打印GC收集细节

- KV类型

  - 公式

    - -XX:属性key=属性值value

  - Case

    - -XX:MetaspaceSize=128M

      通过 jinfo -flag MetaspaceSize 1933查出来 元空间初始值是21M

      -XX:MetaspaceSize=21807104 (初始值21M)

    - -XX:MaxTenuringThreshold=15

      年轻代存活多少次可以升为老年代 15次

      

- jinfo举例（如何查看当前运行程序的配置）

  - 公式

    - jinfo -flag 查看某一个JVM的属性	

      jinfo -flag 配置项 进程编号

      jinfo -flag PrintGCDetails（属性）3435（进程编号） 

  - Case 

    - jinfo -flag InitialHeapSize 8372 初始化堆大小
    - **jinfo -flags 8372** 当前进程所有JVM属性(包括JVM自己初始化的和用户修改的)

- jps

  - **jps -l** 列出所有的Java进程

- 盘点JVM初始参数值

  - -XX:+PrintFlagsInitial 盘点JVM初始参数
  - 公式
    - **java -XX:+PrintFlagsInitial**

  ![img](http://img.tomato530.com/PrintFlagsInitial.png)

-  盘点修改更新的参数

  - -XX:+PrintFlagsFinal 盘点最终修改过的参数

  - 公式

    - **java -XX:+PrintFlagsFinal -version **

  - 参数值

    ​	bool UseCompilerSafepoints = true 普通“=” 是 JVM初始的

    ​	bool UseCompressedOops := true “:=” 是认为修改过的

  - Case

    ![img](http://img.tomato530.com/WechatIMG3.png)

    - java -XX:+PrintFlagsFinal -XX:MetaspaceSize = 512m T (运行的Java类)

      运行Java命令的同时打印参数

    - java -XX:+PrintFlagsFinal -Xss256k T

- -XX:+PrintCommandLineFlags

  - **java -XX:+PrintCommandLineFlags** (可以查看当前的垃圾回收器)
    - ParallelGC 并行垃圾回收器（Java8默认）

  ![img](http://img.tomato530.com/WechatIMG245.png)

- 常用参数

  - **-Xms 等价于 -XX:InitialHeapSize 初始堆内存 **（初始化堆内存可以和最大堆内存设置为一样大 避免每次垃圾回收完成后JVM重新分配内存）（默认物理内存1/64）

  - **-Xmx 等价于 -XX:MaxHeapSize **最大堆内存 （默认物理内存1/4）

    ![img](http://img.tomato530.com/XmxXms.png)

    ![img](http://img.tomato530.com/XmxXmsOut.png)

  - **-Xmn 新生代大小 **（默认1/3堆空间 老年代2/3） 

  - **-Xss 等价于-XX:ThreadStackSize 每个线程的堆栈大小**（一般默认为512K~1024K）

  - **-XX:MetaspaceSize（设置元空间大小）**元空间（Java8）的本质与永久代（Java7）类似 都是对JVM规范中方法区的实现。不过元空间与永久带之间最大的区别在于元空间不在虚拟机中 而是使用本地内存

    因此 默认情况下 元空间的大小仅受本地内存限制。

    -Xms10m -Xmx10m -XX:MetaspaceSize=1024m -XX:+PrintFlagsFinal -XX:+UseSerialGC（串行垃圾回收器）

  - 常用参数典型设置案例

    - **-Xms128m -Xmx4096m -Xss1024 -XX:MetaspaceSize=512m -XX:+PrintCommandLineFlags **
    - **-XX:+PrintGCDetails -XX:+UseSerialGC（串行垃圾回收器）**

  - **-XX:+PrintGCDetails**（打印出垃圾回收的细节情况）

    - 输出详细GC收集日志信息

    ![img](http://img.tomato530.com/PrintGCDetails.png)

    

    手动OOM 设置参数-Xms10m -Xmx10m 创建个50m的大对象

    ![img](http://img.tomato530.com/PrintGCDetails2.png)

    ![img](http://img.tomato530.com/PrintGCDetails3.png)

    - GC （发生在新生代图为GC参数解读）

    ![img](http://img.tomato530.com/GC.png)

    - FullGC（大部分时间发生在老年代 图中为FullGC参数解读 1.8后将PermGen换成Metaspace）

    ![img](http://img.tomato530.com/FullGC.png)

    JavaHeap图

    ![img](http://img.tomato530.com/Javaheap.png)

  - **-XX:SurvivorRatio=8**

    ![img](http://img.tomato530.com/SurvivorRatio.png)

  - **-XX:NewRatio=2**

    ![img](http://img.tomato530.com/NewRatio.png)

  - **-XX:MaxTenuringThreshold（设置垃圾最大年龄（新to老的次数）**默认15 对象头存这个的最大4位 1111 所以就是0-15）

    