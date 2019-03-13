# Java 内部类的好处和缺点

一、什么是内部类
内部类是指在一个外部类的内部再定义一个类，类名不需要和文件夹相同。内部类可以声明 public 、protected 、private 等访问限制，可以声明为 abstract的供其他内部类或外部类继承与扩展，或者声明为static 、final 的，也可以实现特定的接口（而外部顶级类即类名和文件名相同的只能使用 public 和 default）。static 的内部类行为上象一个独立的类，非 static 在行为上类似类的属性或方法且禁止声明 static 的方法。内部类可以访问外部类的所有方法与属性，但 static 的内部类只能访问外部类的静态属性与方法。

Java 的设计者在内部类身上的确是专心良苦。学会使用内部类，是把握Java 高级编程的一部分，它可以让你更优雅地设计你的程序结构。

注意：

内部类是一个编译时的概念，一旦编译成功，就会成为完全不同的两类。
对于一个名为 outer 的外部类和其内部定义的名为 inner 的内部类。编译完成后出现 outer.class 和 outer$inner.class 两类。
所以内部类的成员变量/方法名可以和外部类的相同。
二、内部类的分类
内部类主要分为普通内部类（成员内部类）、局部内部类、匿名内部类、嵌套内部类（静态内部类）。
非静态内部类中不能定义静态成员，静态内部类不能访问外部类普通成员变量（非静态成员）。

1. 普通内部类（成员内部类）

```java
public class Test {
    //不对外开放的
    class memberInnerClass{
        public void memberInner(){
            System.out.println("成员内部类");
        }
    }
}

```

编译一下，我们看到目录中出现了两个 class 文件在我们的工作目录里,可以看到多出一个 Test$memberInClass.class 的文件，这是就是内部类编译后的 class 文件。

![img](https://upload-images.jianshu.io/upload_images/2139461-55a04aa94f1a97b2.png)



成员内部类的特点：

内部类就像一个实例成员一样存在于外部类中。
内部类可以访问外部类的所有成员就想访问自己的成员一样没有限制。
内部类中的 this 指的是内部类的实例对象本身，如果要用外部类的实例对象就可以用类名 .this 的方式获得。
内部类对象中不能有静态成员，原因很简单，内部类的实例对象是外部类实例对象的一个成员。
2. 方法内部类（局部内部类）

```java
public class A {
 
    public void A(){
        System.out.println("方法内部类");
    }
 
}

```

```java
public class Test {
    
    public void methodInner(){
        //短暂性的
        class B extends A{
            
        }
        new B().A();
    }
}

```

方法内部类特点：

方法中的内部类没有访问修饰符， 即方法内部类对包围它的方法之外的任何东西都不可见。
方法内部类只能够访问该方法中的局部变量，所以也叫局部内部类。而且这些局部变量一定要是final修饰的常量。

3. 匿名内部类(在Android里最常见的一种)
  当我们把内部类的定义和声明写到一起时，就不用给这个类起个类名而是直接使用了，这种形式的内部类根本就没有类名，因此我们叫它匿名内部类。

```java
public abstract class A implements B{
 
    public void A(){
        System.out.println("A");
    }
 
}

```

```java
 public interface B{
     
     public void B();
 
 }

```

```java
public class Test {
 
    public static void main(String[] args) {
        //new出接口或者实现类
        A a= new A (){
            //实现接口里未实现的方法
            public void B() {
                System.out.println("匿名内部类");
            }
        };
        a.A();
        a.B();
}

```

匿名内部类的特点：
一个类用于继承其他类或是实现接口，并不需要增加额外的方法，只是对继承方法的事先或是覆盖。
只是为了获得一个对象实例，不需要知道其实际类型。
类名没有意义，也就是不需要使用到。

4. 嵌套内部类（静态内部类）
  嵌套内部类，就是修饰为static的内部类。声明为 static 的内部类，不需要内部类对象和外部类对象之间的联系，就是说我们可以直接引用outer.inner，即不需要创建外部类，也不需要创建内部类。

　生成静态内部类对象的方式为：
　OuterClass.InnerClass inner = new OuterClass.InnerClass();

```java
 
class StaticInner {
    private static int a = 4;
 
    // 静态内部类
    public static class Inner {
        public void test() {
// 静态内部类可以访问外部类的静态成员
// 并且它只能访问静态的
            System.out.println(a);
        }
    }
}
 
public class StaticInnerClassTest {
    public static void main(String[] args) {
        StaticInner.Inner inner = new StaticInner.Inner();
        inner.test();
    }
}

```

嵌套类和普通的内部类还有一个区别：普通内部类不能有 static 数据和 static 属性，也不能包含嵌套类，但嵌套类可以。而嵌套类不能声明为 private ，一般声明为 public，方便调用。

三、使用内部类的好处
1. 内部类可以有多个实例，每个实例都有自己的状态信息，并且与其外围类对象那个的信息相互独立；
2. 在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或继承同一个类；
3. 方便将存在一定逻辑关系的类组织在一起，又可以对外界隐藏
4. 方便编写事件驱动程序；
5. 方便编写线程代码。

