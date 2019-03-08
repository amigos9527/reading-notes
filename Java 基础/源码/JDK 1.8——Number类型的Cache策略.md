# Number类型的Cache策略

Java有8个内置的基础类型，与之对应的也有8个包装类型。为了JDK 1.5以后的泛型支持，Java的基础类型和包装类型在必要时候需要互相转化。JDK源码剖析系列的第一部分将从Java的包装类型开始，一步一步带领大家阅读与分析Java包装类在设计上面的一些技巧，从而使得大家能从源码角度来解答一些常见的令人困惑的问题。

## Number类

Java数值类型的包装类统一继承自Number类。
[![NumberCache-Diagram](https://i.imgur.com/GYwr37l.png)](https://i.imgur.com/GYwr37l.png)

Number类定义了一些用于返回基础类型的抽象方法：

```java
public abstract class Number implements java.io.Serializable {
    public abstract int intValue();
    public abstract long longValue();
    public abstract float floatValue();
    public abstract double doubleValue();
    public byte byteValue() {
        return (byte)intValue();
    }
    public short shortValue() {
        return (short)intValue();
    }
    private static final long serialVersionUID = -8742448824652078965L;
}
```

## 数值类型的常量池缓存策略

JDK为常用的数值-128 ~ 127范围内的数字进行了预先分配，用来优化性能。

### ByteCache

```java
private static class ByteCache {
    private ByteCache(){}

    static final Byte cache[] = new Byte[-(-128) + 127 + 1];

    static {
        for(int i = 0; i < cache.length; i++)
            cache[i] = new Byte((byte)(i - 128));
    }
}
```

在ByteCache的静态初始化块中，会预先为-128 ~ 127范围内的数字进行预定义。

```java
public static Byte valueOf(byte b) {
    final int offset = 128;
    return ByteCache.cache[(int)b + offset];
}
```

接着在`valueOf(byte)`方法中，会从常量池中预先取出预定义的数字从而达到优化性能的目的！那么什么情况下会用到valueOf方法呢？**答案是在JDK1.5以后的自动装箱操作中会用到，因而自动装箱操作会从常量池中选取符合数值范围的预定义数值。**

### ShortCache 和 LongCache需要判断数值是否处于预定义范围

```java
// ShortCache
public static Short valueOf(short s) {
    final int offset = 128;
    int sAsInt = s;
    if (sAsInt >= -128 && sAsInt <= 127) { // must cache
        return ShortCache.cache[sAsInt + offset];
    }
    return new Short(s);
}

// LongCache
public static Long valueOf(long l) {
    final int offset = 128;
    if (l >= -128 && l <= 127) { // will cache
        return LongCache.cache[(int)l + offset];
    }
    return new Long(l);
}
```

### IntegerCache可以配置右区间

```java
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static final Integer cache[];

    static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int, ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }

    private IntegerCache() {}
}
```

从源码可以看出，我们可以为IntegerCache配置右区间，在VM的一个设置`java.lang.Integer.IntegerCache.high`中指定特定值。