# Java栈（Java Stack）

栈管运行 堆管存储

栈 跑我们各种方法的地方

程序 = 算法 + 数据结构

后进先出 例如 弹匣压子弹

栈更多的是装载方法 

栈也叫栈内存 主管Java程序的运行 是在线程创建时创建 它的生命周期是跟随线程的生命周期 线程结束栈内存也就释放 **对于栈来说 不存在垃圾回收的问题** 只要线程一结束 栈就over 生命周期和线程一致 是线程私有的

**8中基本数据类型的变量 + 对象的引用变量 + 实例方法都是在函数的栈中存放**

栈帧中主要保存3种数据：

+ 本地变量：输入参数和输出参数以及方法内的变量
+ 栈操作：记录出栈 入栈的操作
+ 栈帧数据：包括类文件 方法等

java方法 = 栈帧

main方法是一切程序的入口 永远被压在栈底

![](http://img.tomato530.com/javaStack1.png)

![](http://img.tomato530.com/javaStack2.png)

***

递归栈溢出 不断压栈

```java
public static void m1() {
    m1();
}

public static void main(String[] args) {
        m1();
}

// 会报StackOverFlowError
Exception in thread "main" java.lang.StackOverflowError
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)
	at JVMNote.m1(JVMNote.java:9)

```

