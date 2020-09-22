# OOM

<img src="http://img.tomato530.com/Error.png" style="zoom:50%;" />

<img src="http://img.tomato530.com/ErrorAndException.png" style="zoom:50%;" />

## java.lang.StackOverflowError

栈溢出 栈管方法运行

递归方法调用容易报这个错 深度的方法调用

## java.lang.OutOfMemoryError

垃圾回收GC主要回收堆内存的对象 不管直接内存（元空间用的就是直接内存）

+ java.lang.OutOfMemoryError: Java heap space 

  堆内存溢出 对象太多

+ java.lang.OutOfMemoryError: GC overhead limit exceeded 

  98%的时间用来GC 并且回收了不到2%的堆内存

+ java.lang.OutOfMemoryError: Direct buffer memory

  直接内存溢出 写NIO程序容易出这个问题 

  ByteBuffer.allocteDirect(capability); 这个方法是直接分配本地内存 不属于GC管辖范围 但是如果不断的分配本地内存 堆内存很少使用 DirectByteBuffer对象们就不会被回收 这时候堆内存很充足 但本地内存可能已经用光了再次尝试分配本地内存就会OOME 程序直接崩溃 

  配置直接内存 -XX:MaxDirectMemorySize=5m

+ java.lang.OutOfMemeoryError:unable to create new native thread （native thread异常与对应平台有关）

  导致原因

  1. 应用进程创建多个线程 超过系统承载极限
  2. 服务器不允许应用创建这么多的线程 linux系统默认一个进程可以创建1024个线程 应用创建线程超过这个数 就会报 java.lang.OutOfMemeoryError:unable to create new native thread

  解决办法

  1. 想办法降低应用程序创建线程的数量 分析应用是否真的需要这么多线程 如果不是 将线程数降到最低
  2. 对于有的应用 确实需要很多线程 远超过linux系统默认的1024 可以修改linux服务器配置

+ java.lang.OutOfMemeoryError:Metaspace

  Metaspace VM Metadata in native memory

  Java8之后的版本使用元空间代替永久代 Metaspace 是方法区在HotSpot中的实现 它与持久代最大的区别在于 Metaspace并不在虚拟机内存中而使用本地内存 元空间是方法区

  元空间存放了

  1. 虚拟机加载的类信息
  2. 常量池
  3. 静态变量
  4. 即时编译后的代码

  设置元空间大小 -XX:MetaspaceSize=10m

  