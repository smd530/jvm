# JVM参数类型

## 标配参数

- java -version
- java -help

## X参数（了解）

+ -Xint（解释执行）
+ -Xcomp（第一次使用就编译成本地代码）
+ Xmixed（混合模式）

## XX参数（重点）

+ 布尔类型

  + 公式
    + -XX:+某个属性值 
    + -XX:-某个属性值 
  + Case
    +  -XX:+PrintGCDetails 
    + -XX:-PrintGCDetails 是否打印GC收集细节

+ KV类型

  + 公式

    + -XX:属性key=属性值value

  + Case

    + -XX:MetaspaceSize=128M

      通过 jinfo -flag MetaspaceSize 1933查出来 元空间初始值是21M

      -XX:MetaspaceSize=21807104 (初始值21M)

    + -XX:MaxTenuringThreshold=15

      年轻代存活多少次可以升为老年代 15次

      

+ jinfo举例（如何查看当前运行程序的配置）

  + 公式

    + jinfo -flag 查看某一个JVM的属性	
    
      jinfo -flag 配置项 进程编号
      
      jinfo -flag PrintGCDetails（属性）3435（进程编号） 
    
  + Case 

    + jinfo -flag InitialHeapSize 8372 初始化堆大小
    + jinfo -flags 8372 当前进程所有JVM属性(包括JVM自己初始化的和用户修改的)

+ jps

  + jps -l 列出所有的Java进程

+ 经典参数

  + -Xms 等价于 -XX:InitialHeapSize 初始堆内存 （初始化堆内存可以和最大堆内存设置为一样大 避免每次垃圾回收完成后JVM重新分配内存）
+ -Xmx 等价于 -XX:MaxHeapSize 最大堆内存 （机器物理内存1/4）
  + -Xmn 年轻代大小
  + -Xss 每个线程的堆栈大小 256k

+ 盘点JVM初始参数值

  +  -XX:+PrintFlagsInitial 盘点JVM初始参数
  + 公式
    + java -XX:+PrintFlagsInitial

  ![](http://img.tomato530.com/PrintFlagsInitial.png)

+  盘点修改更新的参数

  + -XX:+PrintFlagsFinal 盘点最终修改过的参数

  + 公式

    + java -XX:+PrintFlagsFinal -version

  + 参数值

    ​	bool UseCompilerSafepoints = true 普通“=” 是 JVM初始的

    ​	bool UseCompressedOops := true “:=” 是认为修改过的

+ 