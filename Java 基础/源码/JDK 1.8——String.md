# String 类的修饰

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence
```

首先，它是一个 *final* 类，这表明：该类是不能被继承的。

[Why is String class declared final in Java?](https://stackoverflow.com/questions/2068804/why-is-string-class-declared-final-in-java)

实现了 *Serializable* 接口，表明它是可序列化的 实现了 *Comparable* 接口，表示它是可比较的 实现了 *CharSequence* 接口，看该接口的说明，表示：它是一个可读取的 char 值的序列。该接口提供了不同的 char 序列的统一的形式和只读访问

# String 类的属性

- private final char value[]; 这个存储的是 string 的值，用字符数组来保存
- private int hash; // Default to 0 这个表示该 string 的 哈希码
- private static final long serialVersionUID = -6849794470754667710L; 这个表示该 string 的序列化版本
- private static final ObjectStreamField[] serialPersistentFields = new ObjectStreamField[0]; 这个用来保存要进行序列化的字段。默认情况下，所有的非 transient 非 static 修饰的字段都会被序列化，但可以用这个来进行选择性序列化的字段。

其中 *serialPersistentFields* 主要是被 *java.io.ObjectStreamClass.getDeclaredSerialFields* 方法处理:

```java
ObjectStreamField[] serialPersistentFields = null;
try {
    Field f = cl.getDeclaredField("serialPersistentFields");
    int mask = Modifier.PRIVATE | Modifier.STATIC | Modifier.FINAL;
    if ((f.getModifiers() & mask) == mask) {
        f.setAccessible(true);
        serialPersistentFields = (ObjectStreamField[]) f.get(null);
    }
} catch (Exception ex) {
}
if (serialPersistentFields == null) {
    return null;
} else if (serialPersistentFields.length == 0) {
    return NO_FIELDS;
}
```

看它的注释可知：它返回给定的 class 显式声明的 *serialPersistentFields* 字段定义的可序列化的字段。如果没有适当地定义这个字段的话，就返回 null 。

## transient VS serialPersistentFields

*transient* ：用该关键字修改的字段，表示 *不序列化* 该字段 *serialPersistentFields* : 表示只序列化这里指定的字段。注意，这里的优先级，高于 *transient* 。即，只要这里指定了序列化的，即使在该字段里用了 *transient* 来修饰，该字段也会进行序列化

测试代码:

```java
package org.agoncal.sample.jmh;

import java.io.*;

/**
 * Created by emacsist on 2017/6/30.
 */
public class Test {
    public static void main(String[] args) {
        write();
        read();
    }

    private static final void read() {

        try {
            PersonA personA = new PersonA();
            FileInputStream fileIn =
                    new FileInputStream("/tmp/personA.bin");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            personA = (PersonA) in.readObject();
            in.close();
            fileIn.close();
            System.out.println(personA);
        } catch (IOException i) {
            i.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }


        try {
            PersonB personB = new PersonB();
            FileInputStream fileIn =
                    new FileInputStream("/tmp/personB.bin");
            ObjectInputStream in = new ObjectInputStream(fileIn);
            personB = (PersonB) in.readObject();
            in.close();
            fileIn.close();
            System.out.println(personB);
        } catch (IOException i) {
            i.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    private static final void write() {
        try {
            PersonA personA = new PersonA();
            FileOutputStream fileOut =
                    new FileOutputStream("/tmp/personA.bin");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(personA);
            out.close();
            fileOut.close();
        } catch (IOException i) {
            i.printStackTrace();
        }


        try {
            PersonB personB = new PersonB();
            FileOutputStream fileOut =
                    new FileOutputStream("/tmp/personB.bin");
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            out.writeObject(personB);
            out.close();
            fileOut.close();
        } catch (IOException i) {
            i.printStackTrace();
        }
    }


    public static class PersonA implements Serializable {
        private String hello = "Hello World";
        transient private Integer age = 18;

        public String getHello() {
            return hello;
        }

        public void setHello(String hello) {
            this.hello = hello;
        }

        public Integer getAge() {
            return age;
        }

        public void setAge(Integer age) {
            this.age = age;
        }

        @Override
        public String toString() {
            return "personA[" + hello + ", " + age + "]";
        }
    }

    public static class PersonB implements Serializable {
        private String hello = "Hello World";
        transient private Integer age = 18;


        private static final ObjectStreamField[] serialPersistentFields =
                new ObjectStreamField[1];

        static {
            serialPersistentFields[0] = new ObjectStreamField("age", Integer.class);
        }

        public String getHello() {
            return hello;
        }

        public void setHello(String hello) {
            this.hello = hello;
        }

        public Integer getAge() {
            return age;
        }

        public void setAge(Integer age) {
            this.age = age;
        }

        @Override
        public String toString() {
            return "personB[" + hello + ", " + age + "]";
        }
    }

}
```

输出如下:

```
personA[Hello World, null]
personB[null, 18]
```

可以看到，PersonB 的 age 字段用了 transient 修饰，但它是在 serialPersistentFields 里，但它还是被序列化了。

# 常用的方法

## length()

它返回的是内部表示的 char 数组的长度

```java
value.length
```

## equals()

它判断是否是相同的对象，然后再判断是否是 String 的实例，如果是String实例，就从头到尾进行每一个字符的比较，直到有没的字符为止。

## 处理 String 的方法

通过看源码可知，所有处理 String 的方法，都是通过返回一个新的 String 对象的，而不是在原有的 String 对象上面进行字符 value 数组的修改。

看到网上一些源码分析的文章，说是因为 value 被定义为 final ，所以不能修改。

```java
private final char value[];
```

一个字段被定义为 final ，它的不能 *修改* 是指不能指向其他的对象了，而只能一直指向当前这个 value 的对象。 但 value 对象自身，还是可以修改的，比如下面的代码:

```java
public static void main(String[] args) {
        final char[] hello = "hello world".toCharArray();
        System.out.println(hello);
        hello[0] = 'H';
        System.out.println(hello);
}
```

输出为

```bash
hello world
Hello world
```

但如果这样子的话，就不能被编译通过了:

```java
char[] newHello = "new hello world".toCharArray();
hello = newHello;
```

# String 不能被修改？

网上大多资料，一直在说什么 String 是不可变的。但这种说法，我不知道其实说这些话的人，是不是真的理解了其实的本质。反正，我个人理解起来，是挺费劲的。所以，今天才抽点空来详细看看 String 的源码。

*final* 的类：表示不能被继承。仅此而已 *final* 字段：如果是对象类型，则表示对象的引用不可变（但对象自身的状态还是可变的）；如果是基础类型，表示一旦初始化了，就不能再修改它的值了。 *final* 方法：表示不能被子类覆盖。

通过源码我们知道 *value* 是 String 内部持有的真正的对象，String 是用它来保存它表示其值的，我们知道数组在Java中是对象的一种，也就是说 *final char value[]* 仅仅表示它不能指向其他的 *char[]* 对象引用了，而 *char valuel[]* 自身的内容，其实是可以改变的。那为什么大家都在说，String 是不可变的类型呢，因为 String 类中，根本没有提供 API 给你修改 *char valuel[]* 的内容.

String 中的所有的修改方法，都是复制一个新的 *char value[]* 的值然后作为 String 对象返回的。（为什么呢？这里涉及的比较多了，简单说一下，就是Java的团队认为，字符串复用的好处，大于坏处，所以就使用了字符串复用的方式：即通过 Constant Pool 来维护字符串常量池）

比如有两个字段:

```java
String hello = "abc123";
String hello2 = "abc123";
```

在Java内部， hello 与 hello2 其实都是指向同一个字符串 “abc123” 的内存区域的。比如下面的代码，可以看到:

```java
    public static void main(String[] args) {
        String hello = "abc123";
        String hello2 = "abc123";

        String helloNew = new String("abc123");
        String hello2New = new String("abc123");
        System.out.println("hello 的地址 => " + 	Integer.toHexString(System.identityHashCode(hello)));
        System.out.println("hello2 的地址 => " + Integer.toHexString(System.identityHashCode(hello2)));

        System.out.println("helloNew 的地址 => " + Integer.toHexString(System.identityHashCode(helloNew)));
        System.out.println("hello2New 的地址 => " + Integer.toHexString(System.identityHashCode(hello2New)));
    }
```

在我的 Mac 上输出如下:

```bash
hello 的地址 => 6f94fa3e
hello2 的地址 => 6f94fa3e
helloNew 的地址 => 5e481248
hello2New 的地址 => 66d3c617
```

可以看到 *hello* 与 *hello2* 的地址是一样的，也就说明 *hello == hello2* ，*helloNew* 与 *hello2New* 的地址是不一样的，也就说明 *helloNew != hello2New*

# 让我们来改变 String 的值！

```java
public class Test {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        String hello = "abc";
        System.out.println(Integer.toHexString(System.identityHashCode(hello)));
        System.out.println(Integer.toHexString(System.identityHashCode("abc")));

        Field valueField = String.class.getDeclaredField("value");
        valueField.setAccessible(true);
        valueField.set(hello, "newValue".toCharArray());
        System.out.println("现在 hello 的值为 =>" + hello);
        System.out.println(Integer.toHexString(System.identityHashCode(hello)));
    }
}
```

输出如下:

```bash
6f94fa3e
6f94fa3e
现在 hello 的值为 =>newValue
6f94fa3e
```

# 什么！两个 String “不同”，竟然 == 时返回 true ?

```java
package org.agoncal.sample.jmh;


import java.lang.reflect.Field;

/**
 * Created by emacsist on 2017/6/30.
 */
public class Test {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        String hello = "abc";
        Field valueField = String.class.getDeclaredField("value");
        valueField.setAccessible(true);
        valueField.set(hello, "newValue".toCharArray());

        String hello2 = "abc";
        System.out.println("hello =>" + hello);
        System.out.println("hello == hello2 => " + (hello == hello2));
    }
}
```

输出结果:

```bash
hello =>newValue
hello == hello2 => true
```

虽然我们 *看到* hello2 的值为 “abc”，但其实在JVM内部，并不是 “abc”，而是 “newValue” 了，所以输出为 *true* 这是一个陷阱，因为我们通过反射，修改了 JVM 对字符串 “abc” 的内部表示了（即，对于JVM来说，”abc” 就表示是 “newValue” 了，你可以认为 “abc” 是 “newValue” 的别名！）比如:

```java
System.out.println("abc");
```

JVM 就会输出

```bash
newValue
```

所以，你永远输出不了 “abc” 这个常量字符串了！

注意，这里说的是常量字符串，通过下面的变换，也是可以输出的，但这样子就不是常量字符串”abc”了, 而是对象内容输出的拼接了～:

```bash
System.out.println(new String("a") + new String("bc"));
```

# 从字节码级别看

```bash
        26: ldc           #2                  // String abc
        28: astore_3
        29: getstatic     #10                 // Field java/lang/System.out:Ljava/io/PrintStream;
        32: aload_3
        33: invokevirtual #11                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
```

为什么在字节码里，常量池表项的索引 #2，内容是 “abc”，但输出来的还是 “newValue” 呢？

可以推论：这是因为我们通过反射，修改了JVM的运行时对 “abc” 字符串常量的表示形式了。虽然字节码中反编译时，它显示的是 “abc”，这是静态的，而显示的时候，是运行时动态的内容了，所以才会出现这种情况。

# 结论

所以，为什么一般说 String 不可变？其实只是 API 本身并没有提供让你修改 value 字符数组的接口，而且 String 类，是 final 的，也就是说，它也不允许你通过继承来修改它。

因为 JVM 使用了常量池这个东东（注意，常量池并不仅仅只有字符串常量，Class 文件中的方法符号，字段符号等这些，也是常量池的一部分！） ，所以才要求 String 是不可变的，它们是互为表里关系，一脉相乘的。

通过上面的例子，你就可以知道，如果JVM内部是使用常量池复用的规则，但又允许你修改的话，就会出现上面的问题了。”abc” 输出的不是 “abc” ～ 你不是你，我也不是我了，然后就会有另一个问题了：

```bash
Who are you ?
```

注意：在 Java 里，任何通过 new 出来的对象，都是唯一的！（即物理地址上是唯一的， 即用 *==* 来比较的，只要 *System.identityHashCode()* 输出来的值是相等的，那么 *==* 一定会返回 true）

正因为如此，所以，Java 里有个 *equals* 的方法，允许我们判断两个对象的 *逻辑相等* ，比如我们定义：只要属性中年龄相同的两个对象就是 *相等* 的，那么我们就可以通过实现自定义的 *equals* 方法，来进行逻辑上的相等。

# String.intern()

> 注意，该方法是属于本地方法。它的声明如下:

```java
public native String intern();
```

它的作用是：如果从常量池中，没有与该 String equals（即内容相同）的 String 的话，则将该 String 放进常量池中，然后返回该 String 在常量池中的引用；如果常量池已经存在该 String 的话，则直接返回常量池中的引用。

即：该方法保证返回的是从常量池中返回的唯一的引用。

对于普通的 Java 字符串字面量，Java会自动进行 intern() 操作。

例如:

```java
package org.agoncal.sample.jmh;

public class Test {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException, InterruptedException {
        String hello = "hello";
        String hello2 = new String("hello");
        System.out.println(hello == hello2);
        System.out.println(hello.intern() == hello2.intern());
    }
}
```

它会输出:

```bash
false
true
```

## intern() 在 字符串拼接时的注意事项！

> 注意，是在拼接时才有，非拼接的情况，与假设常量池中已经有该字符串的情况下样，即它返回的是常量池中的地址，但它不会与堆中的地址相同。

在JDK 1.8 中（其他JDK没实验过），经验证，当执行字符串拼接时（即使两个拼接的是变量，而不是字符串字面量），拼接后的字符串的情况：

如果常量池中，还没有该字符串的话，则会把它从堆内存的地址， *提升为常量池中的地址* 如果常量池中，已经有该字符串的话，则 intern() 方法只是返回常量池中的地址，但该对象原地址不变

证明代码：

```java
package org.agoncal.sample.jmh;

public class Test {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException, InterruptedException {
        System.out.println("第一次常量池中没有相同的字符串的情况");
        String name = new String("emac") + new String("sist");
        System.out.println("name 变量的地址:" + Integer.toHexString(System.identityHashCode(name)));
        System.out.println("name 代表的常量池的地址:" + Integer.toHexString(System.identityHashCode(name.intern())));
        System.out.println("emacsist 常量的地址:" + Integer.toHexString(System.identityHashCode("emacsist")));

        System.out.println();
        System.out.println("第一次常量池中已经有相同的字符串的情况");
        String name2Interned = "emacsist2";//Java会自动将它放入常量池，模拟常量池已经有的情况
        String name2 = new String("emacsist2");
        System.out.println("name 变量的地址:" + Integer.toHexString(System.identityHashCode(name2)));
        System.out.println("name 代表的常量池的地址:" + Integer.toHexString(System.identityHashCode(name2.intern())));
        System.out.println("emacsist2 常量的地址:" + Integer.toHexString(System.identityHashCode("emacsist2")));
    }
}
```

输出结果

```bash
第一次常量池中没有相同的字符串的情况
name 变量的地址:6f94fa3e
name 代表的常量池的地址:6f94fa3e
emacsist 常量的地址:6f94fa3e

第一次常量池中已经有相同的字符串的情况
name 变量的地址:5e481248
name 代表的常量池的地址:66d3c617
emacsist2 常量的地址:66d3c617
```

## JDK 1.6, JDK 1.7, JDK 1.8 之间的区别

*<=JDK 1.6* : intern() 的字符串，它会放在 Java 堆中的 Permanent Generation 内存区域 *>= JDK 1.7* : intern() 的字符串，它分配在普通的 Java 堆中，即 Young 和 Old Generation 内存区域( [Oracle JDK 7 changes](http://www.oracle.com/technetwork/java/javase/jdk7-relnotes-418459.html#jdk7changes) ) (这意味着可以被GC)

## 常量池相关的JVM参数

```bash
[12:23:10] emacsist:~ $ java -XX:+PrintFlagsFinal -version | grep StringTable
     bool PrintStringTableStatistics                = false                               {product}
    uintx StringTableSize                           = 60013                               {product}
Java(TM) SE Runtime Environment (build 1.8.0_74-b02)
Java HotSpot(TM) 64-Bit Server VM (build 25.74-b02, mixed mode)
```

- -XX:+PrintStringTableStatistics : 在JVM退出时，打印当前JVM的常量池统计信息
- -XX:StringTableSize=N : 设置 StringTable 的大小

字符中常量池的大小范围:

```bash
StringTable size of 1000 is invalid; must be between 1009 and 2305843009213693951
```

输出例子:(-XX:+PrintStringTableStatistics -XX:StringTableSize=1009)

```bash
SymbolTable statistics:
Number of buckets       :     20011 =    160088 bytes, avg   8.000
Number of entries       :     12127 =    291048 bytes, avg  24.000
Number of literals      :     12127 =    468448 bytes, avg  38.629
Total footprint         :           =    919584 bytes
Average bucket size     :     0.606
Variance of bucket size :     0.607
Std. dev. of bucket size:     0.779
Maximum bucket size     :         6
StringTable statistics:
Number of buckets       :      1009 =      8072 bytes, avg   8.000
Number of entries       :       865 =     20760 bytes, avg  24.000
Number of literals      :       865 =     58048 bytes, avg  67.108
Total footprint         :           =     86880 bytes
Average bucket size     :     0.857
Variance of bucket size :     0.814
Std. dev. of bucket size:     0.902
Maximum bucket size     :         5
```

参考资料：

- [java-performance.info](http://java-performance.info/string-intern-in-java-6-7-8/)
- [美团-深入解析String#intern](http://tech.meituan.com/in_depth_understanding_string_intern.html)

## 什么时候该用 String.intern() ？

一般情况下，我们使用字符串的比较，一般是使用 *String.equals()* ，极少见到 *String.intern() == String.intern()* 。 透过源码可知，equals 是逐个逐个字符从头开始进行比较的，但是 inter() 它是直接比较两个引用的。

所以，通常来说，直接比较引用会比一个一个字符地来比较的性能更高。（即 == 比 equals() 这种方法调用来得更快）

注意，如果决定使用 intern() 来进行字符串的比较，请记得将所有字符串都要进行 intern() 之后再进行比较～

```bash
package org.agoncal.sample.jmh;

public class Test {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException, InterruptedException {
        String hello = "hello world hello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello world";
        String hello2 = new String("hello world hello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello worldhello world");
        final int N = 100000;
        long start = System.currentTimeMillis();

        for (int i = 0; i < N; i++) {
            if (!hello.equals(hello2)) {
                System.out.println("not equals");
            }
        }
        System.out.println("equals cost " + (System.currentTimeMillis() - start) + " ms");

        start = System.currentTimeMillis();
        String hello2Intern = hello2.intern();
        for (int i = 0; i < N; i++) {
            if (hello != hello2Intern) {
                System.out.println("not equals");
            }
        }
        System.out.println("== cost " + (System.currentTimeMillis() - start) + " ms");
    }
}
```

输出结果:

```bash
equals cost 62 ms
== cost 1 ms
```

注意，不要在循环中一直调用 *intern()* ，因为它是本地方法的调用，在循环里调用的话，这样子会比在循环里调用普通的 Java 的方法更耗时。（调用本地方法比较昂贵）

参考资料：

- [stackoverflow Is it good practice to use java.lang.String.intern()?](https://stackoverflow.com/questions/1091045/is-it-good-practice-to-use-java-lang-string-intern)

# String s = new String() + new String()

或者

```java
String hello = "hello";
String world = "world";
String s = hello + world;
```

因为Java编译器的原因，这种代码，在编译的时候，会被编译为:

```java
String s = new StringBuilder().append(new String()).append(new String()).toString();
```

或

```java
String s = new StringBuilder().append(hello).append(world).toString();
```

所以：

```java
String hello = new String("hello") + new String("world");
```

会被编译为:

```java
String hello = new StringBuilder().append(new String("hello")).append(new String("world")).toString();
```

被编译后的字节码如下:

```bash
     stack=4, locals=2, args_size=1
         0: new           #2                  // class java/lang/StringBuilder
         3: dup
         4: invokespecial #3                  // Method java/lang/StringBuilder."<init>":()V
         7: new           #4                  // class java/lang/String
        10: dup
        11: ldc           #5                  // String hello
        13: invokespecial #6                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
        16: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        19: new           #4                  // class java/lang/String
        22: dup
        23: ldc           #8                  // String world
        25: invokespecial #6                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
        28: invokevirtual #7                  // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        31: invokevirtual #9                  // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        34: astore_1
        35: return
```

# String s = “hello” + “world”

它会直接被编译器进行优化为: String s = “helloworld” 并且会将它(“helloworld”)放入常量池中（注意不是 “hello” 和 “world” 分别放入常量池，而是 “helloworld” 一个）

编译后的字节码:

```bash
  Code:
      stack=1, locals=2, args_size=1
         0: ldc           #2                  // String helloworld
         2: astore_1
         3: return
```

# switch 与 String

从JDK 1.7开始，switch 可以用 String 了

```java
    public static final void s(String hello) {
        switch (hello) {
            case "world":
                System.out.println("world");
            case "hello":
                System.out.println("hello");
                break;
            case "hello2":
                System.out.println("hello2");
                break;
            default:
                System.out.println("default");
        }
    }
```

可以看到，它的字节码如下:

```bash
         0: aload_0
         1: astore_1
         2: iconst_m1
         3: istore_2
         4: aload_1
         5: invokevirtual #8                  // Method java/lang/String.hashCode:()I
         8: lookupswitch  { // 3
             -1220935264: 72
                99162322: 58
               113318802: 44
                 default: 83
            }
```

伪代码：

```bash
String tmp = hello; //aload_0 表示将第0个参数，即 hello 压入栈，astore_1 表示将栈顶的元素弹出，保存到第一个局部变量中，这里假设为 tmp
int tmp_i = -1; // iconst_m1 表示将 -1 压入栈，所以这里的伪代码是这样子，tmp_i 就表示是 第1个局部变量

// 即第0个局部变量为 String hello(xx_0的局部变量指令就是它)， 第1个局部变量为 String tmp = hello（xx_1的局部变量就是它），第2个局部变量为 int tmp_i = -1;（xx_2的局部变量指令就是它）

然后根据 tmp.hashCode() 的结果，查找 lookupswitch 表，左边的是 hashCode 的值，右边为要跳转到的代码位置。那么它们是如何比较的？（这里仅以一个为例，假设与 "world" 这个分支比较的情况：

        44: aload_1
        45: ldc           #9                  // String world
        47: invokevirtual #10                 // Method java/lang/String.equals:(Ljava/lang/Object;)Z
        50: ifeq          83
        53: iconst_0
        54: istore_2
        55: goto          83

这段代码的伪代码如下:(在Java字节码中，0 表示 false, 1 表示 true)

if (!tmp.equals("world")){
    goto 83行;
}
tmp_i = 0
goto 83 行;



        83: iload_2
        84: tableswitch   { // 0 to 2
                       0: 112
                       1: 120
                       2: 131
                 default: 142
            }

如果 !tmp.equals("world")，则 tmp_i = -1, 否则 tmp_i = 0，在这里时，就已经是普通的 swith int 类型的代码了（因为 iload_2 就表示的是 tmp_id 这个 int 的值）
```

## 总结

在 swith 中使用 String 时，它步骤如下:

1. 调用 swith 变量中的String 的 *hashCode* 的方法，返回一个 int 值，保存到一个编译器生成的临时变量中，假设为 tmp_i
2. 根据 *hashCode* 的结果，跳转到相应的，*由Java编译器生成的代码* ，它的伪代码就是字符串之间的 *equals* 的方法。（因为单纯地靠 HashCode 并不能决定两个字符串是否真的相同）
3. 然后根据 *equals* 的结果，再设置相应的 tmp_i 的值（每个结果，从上到下依次是从 0 开始递增）
4. 最后，切换为普通的 int 的 swith 代码了～（即直接比较 tmp_i 与 swith 表中各个项的 int 的值是否相等，然后再跳转到相应的代码）