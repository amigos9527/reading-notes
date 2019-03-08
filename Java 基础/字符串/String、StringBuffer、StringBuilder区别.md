## String、StringBuffer、StringBuilder区别

任何一个系统在开发的过程中, 相信都不会缺少对字符串的处理。

在 java 语言中, 用来处理字符串的的类常用的有 3 个: String、StringBuffer、StringBuilder。

 

它们的异同点:

1) 都是 final 类, 都不允许被继承;

2) String 长度是不可变的, StringBuffer、StringBuilder 长度是可变的;

3) StringBuffer 是线程安全的, StringBuilder 不是线程安全的。

 

本篇随笔意在漫游 StringBuffer 与 StringBuilder。

其实现在网络上谈论 String、StringBuffer、StringBuilder 的文章已经多到不可胜数了。小瓜牛不才, 蜗行牛步, 慢了半个世纪。。。

StringBuilder 与 StringBuffer 支持的所有操作基本上是一致的, 不同的是, StringBuilder 不需要执行同步。同步操作意味着

要耗费系统的一些额外的开销, 或时间, 或空间, 或资源等, 甚至可能会造成死锁。从理论上来讲, StringBuilder 的速度要更快一些。

 

串联字符串的性能小测:

```java
public class Application {

    private final int LOOP_TIMES = 200000;
    private final String CONSTANT_STRING = "min-snail";
    
    public static void main(String[] args) {
        
        new Application().startup();
    }
    
    public void testString(){
        String string = "";
        long beginTime = System.currentTimeMillis();
        for(int i = 0; i < LOOP_TIMES; i++){
            string += CONSTANT_STRING;
        }
        long endTime = System.currentTimeMillis();
        System.out.print("String : " + (endTime - beginTime) + "\t");
    }
    
    public void testStringBuffer(){
        StringBuffer buffer = new StringBuffer();
        long beginTime = System.currentTimeMillis();
        for(int i = 0; i < LOOP_TIMES; i++){
            buffer.append(CONSTANT_STRING);
        }
        buffer.toString();
        long endTime = System.currentTimeMillis();
        System.out.print("StringBuffer : " + (endTime - beginTime) + "\t");
    }
    
    public void testStringBuilder(){
        StringBuilder builder = new StringBuilder();
        long beginTime = System.currentTimeMillis();
        for(int i = 0; i < LOOP_TIMES; i++){
            builder.append(CONSTANT_STRING);
        }
        builder.toString();
        long endTime = System.currentTimeMillis();
        System.out.print("StringBuilder : " + (endTime - beginTime) + "\t");
    }
    
    public void startup(){
        for(int i = 0; i < 6; i++){
            System.out.print("The " + i + " [\t    ");
            testString();
            testStringBuffer();
            testStringBuilder();
            System.out.println("]");
        }
    }
}
```

上面示例是频繁的去串联一个比较短的字符串, 然后反复调 6 次。测试是一个很漫长的过程, 在本人的笔记本电脑上总共花去了 23 分钟之多, 下面附上具体数据:

| Number | String | StringBuffer | StringBuilder |
| ------ | ------ | ------------ | ------------- |
| 0      | 231232 | 17           | 14            |
| 1      | 233207 | 6            | 6             |
| 2      | 231294 | 8            | 6             |
| 3      | 235481 | 7            | 6             |
| 4      | 231987 | 9            | 6             |
| 5      | 230132 | 8            | 7             |
| 平均   | 3'52'' | 9.2          | 7.5           |

从表格数据可以看出, 使用 String 的 "+" 符号串联字符串的性能差的惊人, 大概会维持在 3分40秒 的时候可以看到一次打印结果;

其次是 StringBuffer, 平均花时 9.2 毫秒; 然后是 StringBuilder, 平均花时 7.5 毫秒。

 

1) 耗时大的惊人的 String 到底是干嘛去了呢? 调出 cmd 窗口, 敲 jconsole 调出 java 虚拟机监控工具, 查看堆内存的使用情况如下:

![img](https://images0.cnblogs.com/blog/513028/201304/22165137-0da2da5743b048729aaf72415cc485ce.png)

实际上这个已经在上一篇 [小瓜牛漫谈 — String](http://www.cnblogs.com/fancydeepin/archive/2013/04/22/min-snail-speak_String.html) 中提到过, 底层实际上是将循环体内的 string += CONSTANT_STRING; 语句转成了:

string = (new StringBuilder(String.valueOf(string))).append("min-snail").toString();

所以在二十万次的串联字符串中, 每一次都先去创建 StringBuilder 对象, 然后再调 append() 方法来完成 String 类的 "+" 操作。

这里的大部分时间都花在了对象的创建上, 而且每个创建出来的对象的生命都不能长久, 朝生夕灭, 因为这些对象创建出来之后没有引用变量来引用它们,

那么它们在使用完成时候就处于一种不可到达状态, java 虚拟机的垃圾回收器(GC)就会不定期的来回收这些垃圾对象。因此会看到上图堆内存中的曲线起伏变化很大。

 

但如果是遇到如下情况:

```java
String concat1 = "I" + " am " + "min-snail";

String concat2 = "I";
concat2 += " am ";
concat2 += "min-snail";
```

java 对 concat1 的处理速度也是快的惊人。本人在自己的笔记本上测试多次, 耗时基本上都是 0 毫秒。这是因为 concat1 在编译期就可以被确定是一个字符常量。

当编译完成之后 concat1 的值其实就是 "I am min-snail", 因此, 在运行期间自然就不需要花费太多的时间来处理 concat1 了。如果是站在这个角度来看, 使用

StringBuilder 完全不占优势, 在这种情况下, 如果是使用 StringBuilder 反而会使得程序运行需要耗费更多的时间。

但是 concat2 不一样, 由于 concat2 在编译期间不能够被确定, 因此, 在运行期间 JVM 会按老一套的做法, 将其转换成使用 StringBuilder 来实现。

 

2) 从表格数据可以看出, StringBuilder 与 StringBuffer 在耗时上并不相差多少, 只是 StringBuilder 稍微快一些, 但是 StringBuilder 是

冒着多线程不安全的潜在风险。这也是 StringBuilder 为赚取表格数据中的 1.7 毫秒( 若按表格的数据来算, 性能已经提升 20% 多 )所需要付出的代价。

 

3) 综合来说:

StringBuilder 是 java 为 StringBuffer 提供的一个等价类, 但不保证同步。在不涉及多线程的操作情况下可以简易的替换 StringBuffer 来提升

系统性能; StringBuffer 在性能上稍略于 StringBuilder, 但可以不用考虑线程安全问题; String 的 "+" 符号操作起来简单方便,

String 的使用也很简单便捷, java 底层会转换成 StringBuilder 来实现, 特别如果是要在循环体内使用, 建议选择其余两个。 

 

使用 StringBuffer、StringBuilder 的无参构造器产生的对象默认拥有 16 个字符长度大小的字符串缓冲区, 如果是调参数为 String 的构造器,

默认的字符串缓冲区容量是 String 对象的长度 + 16 个长度的大小(留 16 个长度大小的空缓冲区)。详细信息可见 StringBuilder 源码:

![img](https://images0.cnblogs.com/blog/513028/201304/23012944-291050545382422eae72e9e5b99732f4.png)

当使用 append 或 insert 方法向源字符串追加内容的时候, 如果内部缓冲区的大小不够, 就会自动扩张容量, 具体信息看 AbstractStringBuilder 源码:

![img](https://images0.cnblogs.com/blog/513028/201304/23010603-7c20689f9e0c465f80bb6178cbc2a55c.png)

StringBuffer 与 StringBuilder 是相类似的, 这里就不贴 StringBuffer 的源码了。

 

不同构造器间的差异:

```java
public static void main(String[] args) {
    
    StringBuilder builder1 = new StringBuilder("");
    StringBuilder builder2 = new StringBuilder(10);
    StringBuilder builder3 = new StringBuilder("min-snail"); // [ 9个字符  ]

    System.out.println(builder1.length());    // 0
    System.out.println(builder2.length());    // 0
    System.out.println(builder3.length());    // 9
    
    System.out.println(builder1.capacity());  // 16
    System.out.println(builder2.capacity());  // 10
    System.out.println(builder3.capacity());  // 25 [ 25 = 9 + 16 ]
    
    builder2.append("I am min-snail");        // [ 14个字符  ]
    
    System.out.println(builder2.length());    // 14
    System.out.println(builder2.capacity());  // 22 [ 22 = (10 + 1) * 2 ]
}
```

从上面的示例代码可以看出, length() 方法计算的是字符串的实际长度, 空字符串的长度为 0 (这个和 String 是一样的: "".length() == 0)。

capacity() 方法是用来计算对象字符串缓冲区的总容量大小:

builder1 为: length + 16 = 0 + 16 = 16;

builder3 为: length + 16 = 9 + 16 = 25;

builder2 由于是直接指定字符串缓冲区的大小, 因此容量就是指定的值 10, 这个从源码的构造器中就能很容易的看出;

当往 builder2 追加 14 个字符长度大小的字符串时, 这时候原有的缓冲区容量不够用, 那么就会自动的扩容: (10 + 1) * 2 = 22

这个从源码的 expandCapacity(int) 方法的第一行就能够看的出。

 

不同构造器的性能小测:

```java
public class Application {

    private final int LOOP_TIMES = 1000000;
    private final String CONSTANT_STRING = "min-snail";
    
    public static void main(String[] args) {
        
        new Application().startup();
    }
    
    public void testStringBuilder(){
        StringBuilder builder = new StringBuilder();
        long beginTime = System.currentTimeMillis();
        for(int i = 0; i < LOOP_TIMES; i++){
            builder.append(CONSTANT_STRING);
        }
        builder.toString();
        long endTime = System.currentTimeMillis();
        System.out.print("StringBuilder : " + (endTime - beginTime) + "\t");
    }
    
    public void testCapacityStringBuilder(){
        StringBuilder builder = new StringBuilder(LOOP_TIMES * CONSTANT_STRING.length());
        long beginTime = System.currentTimeMillis();
        for(int i = 0; i < LOOP_TIMES; i++){
            builder.append(CONSTANT_STRING);
        }
        builder.toString();
        long endTime = System.currentTimeMillis();
        System.out.print("StringBuilder : " + (endTime - beginTime) + "\t");
    }
    
    public void startup(){
        for(int i = 0; i < 10; i++){
            System.out.print("The " + i + " [\t    ");
            testStringBuilder();
            testCapacityStringBuilder();
            System.out.println("]");
        }
    }
}
```



示例中是频繁的去调 StringBuilder 的 append() 方法往源字符串中追加内容, 总共测试 10 次, 下面附上测试的结果的数据:

| Number | StringBuilder() | StringBuilder(int) |
| ------ | --------------- | ------------------ |
| 0      | 60              | 33                 |
| 1      | 43              | 26                 |
| 2      | 41              | 25                 |
| 3      | 42              | 24                 |
| 4      | 51              | 30                 |
| 5      | 92              | 24                 |
| 6      | 55              | 24                 |
| 7      | 40              | 24                 |
| 8      | 55              | 21                 |
| 9      | 44              | 21                 |
| 平均   | 52.3            | 25.2               |

从表格数据可以看出, 合理的指定字符串缓冲区的容量可以大大的提高系统的性能(若按表格的数据来算, 性能约提升了 108%), 这是因为 StringBuilder 在

缓冲区容量不足的时候会自动扩容, 而扩容就会涉及到数组的拷贝(StringBuilder 和 StringBuffer 底层都是使用 char 数组来实现的), 这个也可以在源码

的 expandCapacity(int) 方法中看的出。这些额外的开销都是需要花费掉一定量的时间的。

 

在上示代码中, 如果将 StringBuilder 换成 StringBuffer, 其余保持不变, 测试的结果的数据如下:

| Number | SstingBuffer() | StringBuffer(int) |
| ------ | -------------- | ----------------- |
| 0      | 85             | 58                |
| 1      | 70             | 56                |
| 2      | 73             | 56                |
| 3      | 71             | 55                |
| 4      | 73             | 58                |
| 5      | 117            | 55                |
| 6      | 84             | 55                |
| 7      | 69             | 55                |
| 8      | 70             | 52                |
| 9      | 73             | 52                |
| 平均   | 78.5           | 55.2              |

与 StringBuilder 相类似的, 指定容量的构造器在性能上也得到了较大的提升(若按表格数据来算, 性能约提升了 42%), 但由于 StringBuffer 需要

执行同步, 因此性能上会比 StringBuilder 差一些。