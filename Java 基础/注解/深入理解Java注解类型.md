# 理解Java注解

实际上Java注解与普通修饰符(public、static、void等)的使用方式并没有多大区别，下面的例子是常见的注解：

```java
public class AnnotationDemo {
    //@Test注解修饰方法A
    @Test
    public static void A(){
        System.out.println("Test.....");
    }
    //一个方法上可以拥有多个不同的注解
    @Deprecated
    @SuppressWarnings("uncheck")
    public static void B(){

    }
}
```
通过在方法上使用@Test注解后，在运行该方法时，测试框架会自动识别该方法并单独调用，@Test实际上是一种标记注解，起标记作用，运行时告诉测试框架该方法为测试方法。而对于@Deprecated和@SuppressWarnings(“uncheck”)，则是Java本身内置的注解，在代码中，可以经常看见它们，但这并不是一件好事，毕竟当方法或是类上面有@Deprecated注解时，说明该方法或是类都已经过期不建议再用，@SuppressWarnings 则表示忽略指定警告，比如@SuppressWarnings(“uncheck”)，这就是注解的最简单的使用方式，那么下面我们就来看看注解定义的基本语法

# 基本语法

## 声明注解与元注解

我们先来看看前面的Test注解是如何声明的：

```java
//声明Test注解
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {

} 
```
我们使用了@interface声明了Test注解，并使用@Target注解传入ElementType.METHOD参数来标明@Test只能用于方法上，@Retention(RetentionPolicy.RUNTIME)则用来表示该注解生存期是运行时，从代码上看注解的定义很像接口的定义，确实如此，毕竟在编译后也会生成Test.class文件。对于@Target和@Retention是由Java提供的元注解，所谓元注解就是标记其他注解的注解，下面分别介绍

- @Target 用来约束注解可以应用的地方（如方法、类或字段），其中ElementType是枚举类型，其定义如下，也代表可能的取值范围

    public enum ElementType {
    
        /**标明该注解可以用于类、接口（包括注解类型）或enum声明*/
        TYPE,
        /** 标明该注解可以用于字段(域)声明，包括enum实例 */
        FIELD,
    
        /** 标明该注解可以用于方法声明 */
        METHOD,
    
        /** 标明该注解可以用于参数声明 */
        PARAMETER,
    
        /** 标明注解可以用于构造函数声明 */
        CONSTRUCTOR,
    
        /** 标明注解可以用于局部变量声明 */
        LOCAL_VARIABLE,
    
        /** 标明注解可以用于注解声明(应用于另一个注解上)*/
        ANNOTATION_TYPE,
    
        /** 标明注解可以用于包声明 */
        PACKAGE,
    
        /**
         * 标明注解可以用于类型参数声明（1.8新加入）
         * @since 1.8
         */
        TYPE_PARAMETER,
    
        /**
         * 类型使用声明（1.8新加入)
         * @since 1.8
         */
        TYPE_USE
    }
请注意，当注解未指定Target值时，则此注解可以用于任何元素之上，多个值使用{}包含并用逗号隔开，如下：

```java
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
```

- @Retention用来约束注解的生命周期，分别有三个值，源码级别（source），类文件级别（class）或者运行时级别（runtime），其含有如下：
  - SOURCE：注解将被编译器丢弃（该类型的注解信息只会保留在源码里，源码经过编译后，注解信息会被丢弃，不会保留在编译好的class文件里）

  - CLASS：注解在class文件中可用，但会被VM丢弃（该类型的注解信息会保留在源码里和class文件里，在执行的时候，不会加载到虚拟机中），请注意，当注解未定义Retention值时，默认值是CLASS，如Java内置注解，@Override、@Deprecated、@SuppressWarnning等

  - RUNTIME：注解信息将在运行期(JVM)也保留，因此可以通过反射机制读取注解的信息（源码、class文件和执行的时候都有注解的信息），如SpringMvc中的@Controller、@Autowired、@RequestMapping等。

## 注解元素及其数据类型

通过上述对@Test注解的定义，我们了解了注解定义的过程，由于@Test内部没有定义其他元素，所以@Test也称为标记注解（marker annotation），但在自定义注解中，一般都会包含一些元素以表示某些值，方便处理器使用，这点在下面的例子将会看到：

```java
/**
 * Created by wuzejian on 2017/5/18.
 * 对应数据表注解
 */
@Target(ElementType.TYPE)//只能应用于类上
@Retention(RetentionPolicy.RUNTIME)//保存到运行时
public @interface DBTable {
    String name() default "";
}
```

上述定义一个名为DBTable的注解，该用于主要用于数据库表与Bean类的映射（稍后会有完整案例分析），与前面Test注解不同的是，我们声明一个String类型的name元素，其默认值为空字符，但是必须注意到对应任何元素的声明应采用方法的声明方式，同时可选择使用default提供默认值，@DBTable使用方式如下：

```java
//在类上使用该注解
@DBTable(name = "MEMBER")
public class Member {
	//.......
}
```

关于注解支持的元素数据类型除了上述的String，还支持如下数据类型

- 所有基本类型（int,float,boolean,byte,double,char,long,short）
- String
- Class
- enum
- Annotation
- 上述类型的数组

倘若使用了其他数据类型，编译器将会丢出一个编译错误，注意，声明注解元素时可以使用基本类型但不允许使用任何包装类型，同时还应该注意到注解也可以作为元素的类型，也就是嵌套注解，下面的代码演示了上述类型的使用过程：



```java
package com.zejian.annotationdemo;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Created by wuzejian on 2017/5/19.
 * 数据类型使用Demo
 */

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Reference{
    boolean next() default false;
}

public @interface AnnotationElementDemo {
    //枚举类型
    enum Status {FIXED,NORMAL};
    
    //声明枚举
    Status status() default Status.FIXED;

    //布尔类型
    boolean showSupport() default false;

    //String类型
    String name()default "";

    //class类型
    Class<?> testCase() default Void.class;

    //注解嵌套
    Reference reference() default @Reference(next=true);

    //数组类型
    long[] value();
}
```
## 编译器对默认值的限制

编译器对元素的默认值有些过分挑剔。首先，元素不能有不确定的值。也就是说，元素必须要么具有默认值，要么在使用注解时提供元素的值。其次，对于非基本类型的元素，无论是在源代码中声明，还是在注解接口中定义默认值，都不能以null作为值，这就是限制，没有什么利用可言，但造成一个元素的存在或缺失状态，因为每个注解的声明中，所有的元素都存在，并且都具有相应的值，为了绕开这个限制，只能定义一些特殊的值，例如空字符串或负数，表示某个元素不存在。

## 注解不支持继承

注解是不支持继承的，因此不能使用关键字extends来继承某个@interface，但注解在编译后，编译器会自动继承java.lang.annotation.Annotation接口，这里我们反编译前面定义的DBTable注解

```java
package com.zejian.annotationdemo;

import java.lang.annotation.Annotation;
//反编译后的代码
public interface DBTable extends Annotation
{
    public abstract String name();

}
```

虽然反编译后发现DBTable注解继承了Annotation接口，请记住，即使Java的接口可以实现多继承，但定义注解时依然无法使用extends关键字继承@interface。

## 快捷方式

所谓的快捷方式就是注解中定义了名为value的元素，并且在使用该注解时，如果该元素是唯一需要赋值的一个元素，那么此时无需使用key=value的语法，而只需在括号内给出value元素所需的值即可。这可以应用于任何合法类型的元素，记住，这限制了元素名必须为value，简单案例如下:

```java
package com.zejian.annotationdemo;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

/**
 * Created by zejian on 2017/5/20.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 */
//定义注解
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface IntegerVaule{
    int value() default 0;
    String name() default "";
}

//使用注解
public class QuicklyWay {

    //当只想给value赋值时,可以使用以下快捷方式
    @IntegerVaule(20)
    public int age;

    //当name也需要赋值时必须采用key=value的方式赋值
    @IntegerVaule(value = 10000,name = "MONEY")
    public int money;

}

```

## Java内置注解与其它元注解

接着看看Java提供的内置注解，主要有3个，如下：

- @Override：用于标明此方法覆盖了父类的方法，源码如下

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

- @Deprecated：用于标明已经过时的方法或类，源码如下，关于@Documented稍后分析：

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {
}

```

- @SuppressWarnnings:用于有选择的关闭编译器对类、方法、成员变量、变量初始化的警告，其实现源码如下：

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

其内部有一个String数组，主要接收值如下：

```txt
deprecation：使用了不赞成使用的类或方法时的警告；
unchecked：执行了未检查的转换时的警告，例如当使用集合时没有用泛型 (Generics) 来指定集合保存的类型; 
fallthrough：当 Switch 程序块直接通往下一种情况而没有 Break 时的警告;
path：在类路径、源文件路径等中有不存在的路径时的警告; 
serial：当在可序列化的类上缺少 serialVersionUID 定义时的警告; 
finally：任何 finally 子句不能正常完成时的警告; 
all：关于以上所有情况的警告。
```

这个三个注解比较简单，看个简单案例即可：

```java
//注明该类已过时，不建议使用
@Deprecated
class A{
    public void A(){ }

    //注明该方法已过时，不建议使用
    @Deprecated()
    public void B(){ }
}

class B extends A{

    @Override //标明覆盖父类A的A方法
    public void A() {
        super.A();
    }

    //去掉检测警告
    @SuppressWarnings({"uncheck","deprecation"})
    public void C(){ } 
    //去掉检测警告
    @SuppressWarnings("uncheck")
    public void D(){ }
}

```

前面我们分析了两种元注解，@Target和@Retention，除了这两种元注解，Java还提供了另外两种元注解，@Documented和@Inherited，下面分别介绍：

- @Documented 被修饰的注解会生成到javadoc中

```java

@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface DocumentA {
}

//没有使用@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface DocumentB {
}

//使用注解
@DocumentA
@DocumentB
public class DocumentDemo {
    public void A(){
    }
}
```

使用javadoc命令生成文档：

```
zejian@zejiandeMBP annotationdemo$ javadoc DocumentDemo.java DocumentA.java DocumentB.java 

```

如下： 

![image](https://img-blog.csdn.net/20170520150623393?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamF2YXplamlhbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以发现使用@Documented元注解定义的注解(@DocumentA)将会生成到javadoc中,而@DocumentB则没有在doc文档中出现，这就是元注解@Documented的作用。

- @Inherited 可以让注解被继承，但这并不是真的继承，只是通过使用@Inherited，可以让子类Class对象使用getAnnotations()获取父类被@Inherited修饰的注解，如下：

```java
@Inherited
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface DocumentA {
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface DocumentB {
}

@DocumentA
class A{ }

class B extends A{ }

@DocumentB
class C{ }

class D extends C{ }

//测试
public class DocumentDemo {

    public static void main(String... args){
        A instanceA=new B();
        System.out.println("已使用的@Inherited注解:"+Arrays.toString(instanceA.getClass().getAnnotations()));

        C instanceC = new D();

        System.out.println("没有使用的@Inherited注解:"+Arrays.toString(instanceC.getClass().getAnnotations()));
    }

    /**
     * 运行结果:
     已使用的@Inherited注解:[@com.zejian.annotationdemo.DocumentA()]
     没有使用的@Inherited注解:[]
     */
}
```
# 注解与反射机制

前面经过反编译后，我们知道Java所有注解都继承了Annotation接口，也就是说　Java使用Annotation接口代表注解元素，该接口是所有Annotation类型的父接口。同时为了运行时能准确获取到注解的相关信息，Java在java.lang.reflect 反射包下新增了AnnotatedElement接口，它主要用于表示目前正在 VM 中运行的程序中已使用注解的元素，通过该接口提供的方法可以利用反射技术地读取注解的信息，如反射包的Constructor类、Field类、Method类、Package类和Class类都实现了AnnotatedElement接口，它简要含义如下（更多详细介绍可以看 深入理解Java类型信息(Class对象)与反射机制）：

```
Class：类的Class对象定义 　 
Constructor：代表类的构造器定义 　 
Field：代表类的成员变量定义 
Method：代表类的方法定义 　 
Package：代表类的包定义
```

下面是AnnotatedElement中相关的API方法，以上5个类都实现以下的方法:

|        **返回值**        |                         **方法名称**                         |                           **说明**                           |
| :----------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| `<A extends Annotation>` |          `getAnnotation(Class<A> annotationClass)`           | 该元素如果存在指定类型的注解，则返回这些注解，否则返回 null。 |
|      `Annotation[]`      |                      `getAnnotations()`                      |         返回此元素上存在的所有注解，包括从父类继承的         |
|        `boolean`         | `isAnnotationPresent(Class<? extends Annotation> annotationClass)` | 如果指定类型的注解存在于此元素上，则返回 true，否则返回 false。 |
|      `Annotation[]`      |                  `getDeclaredAnnotations()`                  | 返回直接存在于此元素上的所有注解，注意，不包括父类的注解，调用者可以随意修改返回的数组；这不会对其他调用者返回的数组产生任何影响，没有则返回长度为0的数组 |

简单案例演示如下：

```java
package com.zejian.annotationdemo;

import java.lang.annotation.Annotation;
import java.util.Arrays;

/**
 * Created by zejian on 2017/5/20.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 */
@DocumentA
class A{ }

//继承了A类
@DocumentB
public class DocumentDemo extends A{

    public static void main(String... args){

        Class<?> clazz = DocumentDemo.class;
        //根据指定注解类型获取该注解
        DocumentA documentA=clazz.getAnnotation(DocumentA.class);
        System.out.println("A:"+documentA);

        //获取该元素上的所有注解，包含从父类继承
        Annotation[] an= clazz.getAnnotations();
        System.out.println("an:"+ Arrays.toString(an));
        //获取该元素上的所有注解，但不包含继承！
        Annotation[] an2=clazz.getDeclaredAnnotations();
        System.out.println("an2:"+ Arrays.toString(an2));

        //判断注解DocumentA是否在该元素上
        boolean b=clazz.isAnnotationPresent(DocumentA.class);
        System.out.println("b:"+b);

        /**
         * 执行结果:
         A:@com.zejian.annotationdemo.DocumentA()
         an:[@com.zejian.annotationdemo.DocumentA(), @com.zejian.annotationdemo.DocumentB()]
         an2:@com.zejian.annotationdemo.DocumentB()
         b:true
         */
    }
}

```
# 运行时注解处理器

了解完注解与反射的相关API后，现在通过一个实例（该例子是博主改编自《Tinking in Java》）来演示利用运行时注解来组装数据库SQL的构建语句的过程

```java
/**
 * Created by wuzejian on 2017/5/18.
 * 表注解
 */
@Target(ElementType.TYPE)//只能应用于类上
@Retention(RetentionPolicy.RUNTIME)//保存到运行时
public @interface DBTable {
    String name() default "";
}


/**
 * Created by wuzejian on 2017/5/18.
 * 注解Integer类型的字段
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface SQLInteger {
    //该字段对应数据库表列名
    String name() default "";
    //嵌套注解
    Constraints constraint() default @Constraints;
}


/**
 * Created by wuzejian on 2017/5/18.
 * 注解String类型的字段
 */
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface SQLString {

    //对应数据库表的列名
    String name() default "";

    //列类型分配的长度，如varchar(30)的30
    int value() default 0;

    Constraints constraint() default @Constraints;
}


/**
 * Created by wuzejian on 2017/5/18.
 * 约束注解
 */

@Target(ElementType.FIELD)//只能应用在字段上
@Retention(RetentionPolicy.RUNTIME)
public @interface Constraints {
    //判断是否作为主键约束
    boolean primaryKey() default false;
    //判断是否允许为null
    boolean allowNull() default false;
    //判断是否唯一
    boolean unique() default false;
}

/**
 * Created by wuzejian on 2017/5/18.
 * 数据库表Member对应实例类bean
 */
@DBTable(name = "MEMBER")
public class Member {
    //主键ID
    @SQLString(name = "ID",value = 50, constraint = @Constraints(primaryKey = true))
    private String id;

    @SQLString(name = "NAME" , value = 30)
    private String name;

    @SQLInteger(name = "AGE")
    private int age;

    @SQLString(name = "DESCRIPTION" ,value = 150 , constraint = @Constraints(allowNull = true))
    private String description;//个人描述

   //省略set get.....
}
```

上述定义4个注解，分别是@DBTable(用于类上)、@Constraints(用于字段上)、 @SQLString(用于字段上)、@SQLString(用于字段上)并在Member类中使用这些注解，这些注解的作用的是用于帮助注解处理器生成创建数据库表MEMBER的构建语句，在这里有点需要注意的是，我们使用了嵌套注解@Constraints，该注解主要用于判断字段是否为null或者字段是否唯一。必须清楚认识到上述提供的注解生命周期必须为@Retention(RetentionPolicy.RUNTIME)，即运行时，这样才可以使用反射机制获取其信息。有了上述注解和使用，剩余的就是编写上述的注解处理器了，前面我们聊了很多注解，其处理器要么是Java自身已提供、要么是框架已提供的，我们自己都没有涉及到注解处理器的编写，但上述定义处理SQL的注解，其处理器必须由我们自己编写了，如下

```java
package com.zejian.annotationdemo;
import java.lang.annotation.Annotation;
import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.List;

/**
 * Created by zejian on 2017/5/13.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 * 运行时注解处理器，构造表创建语句
 */
public class TableCreator {

  public static String createTableSql(String className) throws ClassNotFoundException {
    Class<?> cl = Class.forName(className);
    DBTable dbTable = cl.getAnnotation(DBTable.class);
    //如果没有表注解，直接返回
    if(dbTable == null) {
      System.out.println(
              "No DBTable annotations in class " + className);
      return null;
    }
    String tableName = dbTable.name();
    // If the name is empty, use the Class name:
    if(tableName.length() < 1)
      tableName = cl.getName().toUpperCase();
    List<String> columnDefs = new ArrayList<String>();
    //通过Class类API获取到所有成员字段
    for(Field field : cl.getDeclaredFields()) {
      String columnName = null;
      //获取字段上的注解
      Annotation[] anns = field.getDeclaredAnnotations();
      if(anns.length < 1)
        continue; // Not a db table column

      //判断注解类型
      if(anns[0] instanceof SQLInteger) {
        SQLInteger sInt = (SQLInteger) anns[0];
        //获取字段对应列名称，如果没有就是使用字段名称替代
        if(sInt.name().length() < 1)
          columnName = field.getName().toUpperCase();
        else
          columnName = sInt.name();
        //构建语句
        columnDefs.add(columnName + " INT" +
                getConstraints(sInt.constraint()));
      }
      //判断String类型
      if(anns[0] instanceof SQLString) {
        SQLString sString = (SQLString) anns[0];
        // Use field name if name not specified.
        if(sString.name().length() < 1)
          columnName = field.getName().toUpperCase();
        else
          columnName = sString.name();
        columnDefs.add(columnName + " VARCHAR(" +
                sString.value() + ")" +
                getConstraints(sString.constraint()));
      }


    }
    //数据库表构建语句
    StringBuilder createCommand = new StringBuilder(
            "CREATE TABLE " + tableName + "(");
    for(String columnDef : columnDefs)
      createCommand.append("\n    " + columnDef + ",");

    // Remove trailing comma
    String tableCreate = createCommand.substring(
            0, createCommand.length() - 1) + ");";
    return tableCreate;
  }


    /**
     * 判断该字段是否有其他约束
     * @param con
     * @return
     */
  private static String getConstraints(Constraints con) {
    String constraints = "";
    if(!con.allowNull())
      constraints += " NOT NULL";
    if(con.primaryKey())
      constraints += " PRIMARY KEY";
    if(con.unique())
      constraints += " UNIQUE";
    return constraints;
  }

  public static void main(String[] args) throws Exception {
    String[] arg={"com.zejian.annotationdemo.Member"};
    for(String className : arg) {
      System.out.println("Table Creation SQL for " +
              className + " is :\n" + createTableSql(className));
    }

    /**
     * 输出结果：
     Table Creation SQL for com.zejian.annotationdemo.Member is :
     CREATE TABLE MEMBER(
     ID VARCHAR(50) NOT NULL PRIMARY KEY,
     NAME VARCHAR(30) NOT NULL,
     AGE INT NOT NULL,
     DESCRIPTION VARCHAR(150)
     );
     */
  }
}

```

如果对反射比较熟悉的同学，上述代码就相对简单了，我们通过传递Member的全路径后通过Class.forName()方法获取到Member的class对象，然后利用Class对象中的方法获取所有成员字段Field，最后利用field.getDeclaredAnnotations()遍历每个Field上的注解再通过注解的类型判断来构建建表的SQL语句。这便是利用注解结合反射来构建SQL语句的简单的处理器模型，是否已回想起Hibernate？

# Java 8中注解增强

## 元注解@Repeatable

元注解@Repeatable是JDK1.8新加入的，它表示在同一个位置重复相同的注解。在没有该注解前，一般是无法在同一个类型上使用相同的注解的

```java
//Java8前无法这样使用
@FilterPath("/web/update")
@FilterPath("/web/add")
public class A {}
```

Java8前如果是想实现类似的功能，我们需要在定义@FilterPath注解时定义一个数组元素接收多个值如下

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface FilterPath {
    String [] value();
}

//使用
@FilterPath({"/update","/add"})
public class A { }

```

但在Java8新增了@Repeatable注解后就可以采用如下的方式定义并使用了

```java
package com.zejian.annotationdemo;

import java.lang.annotation.*;

/**
 * Created by zejian on 2017/5/20.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 */
//使用Java8新增@Repeatable原注解
@Target({ElementType.TYPE,ElementType.FIELD,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Repeatable(FilterPaths.class)//参数指明接收的注解class
public @interface FilterPath {
    String  value();
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface FilterPaths {
    FilterPath[] value();
}

//使用案例
@FilterPath("/web/update")
@FilterPath("/web/add")
@FilterPath("/web/delete")
class AA{ }
```

我们可以简单理解为通过使用@Repeatable后，将使用@FilterPaths注解作为接收同一个类型上重复注解的容器，而每个@FilterPath则负责保存指定的路径串。为了处理上述的新增注解，Java8还在AnnotatedElement接口新增了getDeclaredAnnotationsByType() 和 getAnnotationsByType()两个方法并在接口给出了默认实现，在指定@Repeatable的注解时，可以通过这两个方法获取到注解相关信息。但请注意，旧版API中的getDeclaredAnnotation()和 getAnnotation()是不对@Repeatable注解的处理的(除非该注解没有在同一个声明上重复出现)。注意getDeclaredAnnotationsByType方法获取到的注解不包括父类，其实当 getAnnotationsByType()方法调用时，其内部先执行了getDeclaredAnnotationsByType方法，只有当前类不存在指定注解时，getAnnotationsByType()才会继续从其父类寻找，但请注意如果@FilterPath和@FilterPaths没有使用了@Inherited的话，仍然无法获取。下面通过代码来演示：

```java
/**
 * Created by zejian on 2017/5/20.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 */
//使用Java8新增@Repeatable原注解
@Target({ElementType.TYPE,ElementType.FIELD,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Repeatable(FilterPaths.class)
public @interface FilterPath {
    String  value();
}


@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface FilterPaths {
    FilterPath[] value();
}

@FilterPath("/web/list")
class CC { }

//使用案例
@FilterPath("/web/update")
@FilterPath("/web/add")
@FilterPath("/web/delete")
class AA extends CC{
    public static void main(String[] args) {

        Class<?> clazz = AA.class;
        //通过getAnnotationsByType方法获取所有重复注解
        FilterPath[] annotationsByType = clazz.getAnnotationsByType(FilterPath.class);
        FilterPath[] annotationsByType2 = clazz.getDeclaredAnnotationsByType(FilterPath.class);
        if (annotationsByType != null) {
            for (FilterPath filter : annotationsByType) {
                System.out.println("1:"+filter.value());
            }
        }

        System.out.println("-----------------");

        if (annotationsByType2 != null) {
            for (FilterPath filter : annotationsByType2) {
                System.out.println("2:"+filter.value());
            }
        }


        System.out.println("使用getAnnotation的结果:"+clazz.getAnnotation(FilterPath.class));


        /**
         * 执行结果(当前类拥有该注解FilterPath,则不会从CC父类寻找)
         1:/web/update
         1:/web/add
         1:/web/delete
         -----------------
         2:/web/update
         2:/web/add
         2:/web/delete
         使用getAnnotation的结果:null
         */
    }
}
```

从执行结果来看如果当前类拥有该注解@FilterPath,则getAnnotationsByType方法不会从CC父类寻找，下面看看另外一种情况，即AA类上没有@FilterPath注解

```java
/**
 * Created by zejian on 2017/5/20.
 * Blog : http://blog.csdn.net/javazejian [原文地址,请尊重原创]
 */
//使用Java8新增@Repeatable原注解
@Target({ElementType.TYPE,ElementType.FIELD,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Inherited //添加可继承元注解
@Repeatable(FilterPaths.class)
public @interface FilterPath {
    String  value();
}


@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Inherited //添加可继承元注解
@interface FilterPaths {
    FilterPath[] value();
}

@FilterPath("/web/list")
@FilterPath("/web/getList")
class CC { }

//AA上不使用@FilterPath注解,getAnnotationsByType将会从父类查询
class AA extends CC{
    public static void main(String[] args) {

        Class<?> clazz = AA.class;
        //通过getAnnotationsByType方法获取所有重复注解
        FilterPath[] annotationsByType = clazz.getAnnotationsByType(FilterPath.class);
        FilterPath[] annotationsByType2 = clazz.getDeclaredAnnotationsByType(FilterPath.class);
        if (annotationsByType != null) {
            for (FilterPath filter : annotationsByType) {
                System.out.println("1:"+filter.value());
            }
        }

        System.out.println("-----------------");

        if (annotationsByType2 != null) {
            for (FilterPath filter : annotationsByType2) {
                System.out.println("2:"+filter.value());
            }
        }


        System.out.println("使用getAnnotation的结果:"+clazz.getAnnotation(FilterPath.class));


        /**
         * 执行结果(当前类没有@FilterPath,getAnnotationsByType方法从CC父类寻找)
         1:/web/list
         1:/web/getList
         -----------------
         使用getAnnotation的结果:null
         */
    }
}
```

注意定义@FilterPath和@FilterPath时必须指明@Inherited，getAnnotationsByType方法否则依旧无法从父类获取@FilterPath注解，这是为什么呢，不妨看看getAnnotationsByType方法的实现源码：

```java
//接口默认实现方法
default <T extends Annotation> T[] getAnnotationsByType(Class<T> annotationClass) {
//先调用getDeclaredAnnotationsByType方法
T[] result = getDeclaredAnnotationsByType(annotationClass);

//判断当前类获取到的注解数组是否为0
if (result.length == 0 && this instanceof Class && 
//判断定义注解上是否使用了@Inherited元注解 
 AnnotationType.getInstance(annotationClass).isInherited()) { // Inheritable
        //从父类获取
       Class<?> superClass = ((Class<?>) this).getSuperclass();
   if (superClass != null) {
      result = superClass.getAnnotationsByType(annotationClass);
       }
   }

   return result;
}
```

## 新增的两种ElementType

在Java8中 ElementType 新增两个枚举成员，TYPE_PARAMETER 和 TYPE_USE ，在Java8前注解只能标注在一个声明(如字段、类、方法)上，Java8后，新增的TYPE_PARAMETER可以用于标注类型参数，而TYPE_USE则可以用于标注任意类型(不包括class)。如下所示:

```java
//TYPE_PARAMETER 标注在类型参数上
class D<@Parameter T> { }

//TYPE_USE则可以用于标注任意类型(不包括class)
//用于父类或者接口
class Image implements @Rectangular Shape { }

//用于构造函数
new @Path String("/usr/bin")

//用于强制转换和instanceof检查,注意这些注解中用于外部工具，它们不会对类型转换或者instanceof的检查行为带来任何影响。
String path=(@Path String)input;
if(input instanceof @Path String)

//用于指定异常
public Person read() throws @Localized IOException.

//用于通配符绑定
List<@ReadOnly ? extends Person>
List<? extends @ReadOnly Person>

@NotNull String.class //非法，不能标注class
import java.lang.@NotNull String //非法，不能标注import
```

这里主要说明一下TYPE_USE，类型注解用来支持在Java的程序中做强类型检查，配合第三方插件工具（如Checker Framework），可以在编译期检测出runtime error（如UnsupportedOperationException、NullPointerException异常），避免异常延续到运行期才发现，从而提高代码质量，这就是类型注解的主要作用。总之Java 8 新增加了两个注解的元素类型ElementType.TYPE_USE 和ElementType.TYPE_PARAMETER ，通过它们，我们可以把注解应用到各种新场合中。

###### ***来源：https://blog.csdn.net/javazejian/article/details/71860633***