# jvm
栈管运行 堆管存储

运行时数据区抽象成了 java.lang.Runtime 类

```java
// 获取当前机器核数
Runtime.getRuntime().availableProcessors();
// JVM中的内存总量 -Xms配置
Runtime.getRuntime().totalMemory();
// JVM试图使用的最大内存 -Xmx配置
Runtime.getRuntime().maxMemory();
```