## Java编程思想读书笔记

### 第一章对象导论

\>>>>封装<<<<

被隐藏（也即封装）的部分通常代表对象内部脆弱的部分，它们很容易被程序员所毁坏，因此将实现隐藏起来可以减少程序的bug。

隐藏是通过访问控制修饰符（public、protected、包访问、private）来实现的。

访问控制的第一个存在原因就是让调用者无法直接触及他们不应该触及的部分，但从另一方面来看，其实这不失为一项服务，因为他们可以很容易地看出哪些东西对他们来说很重要，而哪些东西可能不关心；访问控制的第二个存在原因就是允许库设计者可以改变类的内部的工作方式而不用担心会影响到调用者。

\>>>>继承<<<<

代码复用：复用是面向对象程序设计所提供最了不起的优点之一。

最简单的代码复用就是直接调用类的方法，此外，我们还可以将该类置于某个新类中，使它成为新类的属性成员。新的类也可由任意数量、任意类型的其他对象以任意可以实现新的类中想要功能的方式所组成，这种使用现有的类合成新的类方式称为组合复用。

组合复用带来了极大的灵活性，使得它能在运行时动态的修改其实现行为，但继承并不具备这样的灵活性，因为继承是在编译时期就已经确定其行为，在运行时期是不能修改的。

继承两种实现方式，第一种方式非常直接：直接在子类中添加新的方法，即扩展父类接口。第二种方式就是子类覆写父类方法，但不新增父类没有接口。

“is-a是一个”与“is-like-a像是一个”。继承时，我们使用第一种还是第二种方式呢？这可能引起争论：继承应该只覆盖基类的方法，而并不添加在基类中没有的新方法吗？如果这样做，就意味着子类与基类是完全相同的类型，因为它们具有完全相同的接口，结果可以用一个子类对象来完全替代一个基类对象，这可被认为是纯粹替代，通常称之为替代原则，这也是一种理想的方式，我们经常称这种情况下的子类与基类的关系是“is-a是一个”；有时必须在子类型中添加新的接口，这样也就扩展了接口，这个新的类型仍可以替代基类，但是这种替代并不完美，因为基类无法访问新添加的方法，这种情况下我们可以描述为“is-like-a像是一个”关系。

\>>>>多态<<<<

一个非面向对象编程的编译器产生的函数调用会引起所谓的前期绑定，而向对象程序设计语言使用了后期绑定的概念。

方法的调用就是编译器将产生对一个具体函数名字的调用，前期绑定是在运行时将这个调用解析到将要被执行的代码的绝对地址。然而在OOP中，程序直到运行时才能够确定代码的地址，为了执行后期绑定，Java编译器使用了一小段特殊代码来替代绝对地址调用，这段代码使用对象中存储的信息来计算方法体的地址，根据这段特殊代码，每个对象都可以具有不同的行为表现。

在某些语言中，必须明确地声明某个方法具备后期绑定属性所带来的灵活性，如C++使用virtual关键字来实现，在默认情况下，C++不是动态绑定的，而在Java中，动态绑定是默认行为，不需要添加额外的关键字来实现多态。

\>>> Java语言支持四种类型<<<

接口（interface）、类（class）、数组（array）和基本类型（primitive）。前三种类型通常被称为引用类型（reference type），类实例和数组是对象（object），而基本类型的值则不是对象。类的成员（member）由它的域（field）、方法（method）、成员类（member class）和成员接口（member interface）组成。方法签名（signature）由它的名称和所有参数类型组成；签名不包括它的返回类型。

\>>>>运行Java<<<<

用javac命令编译一个打包的类时，如果没有加参数"-d"时，则编译出的类不会放在包中（即相应的文件夹中），是没有包路径的，除非用参数"-d"指定类存放的位置，–d 指示的是编译后的class文件放在哪个目录下，并且会自动创建包名文件夹。

比如现有如下类：

```Java
package a.b;

class A{}
```

javac A.java 时会在当前工作目录下产生一个A.class文件，不会创建包目录结构。

javac –d . A.java ，则会在当前工作目录产生 a/b/A.class 目录与文件。

所以使用javac编译时要想产生相应的类包名上当结构，则需要带上“–d .”这样的参数。

用java命令运行一个类时，如果该类是存放在包中的，则运行时一定要带上包名，并且在环境变量要有该包存放的路径。

　    *java -classpath . a.Ａ*

注，如果用java命令运行时，没有配置classpath环境变量，则这里的classpath不能缺少。

 

\>>>类与类之间的关系<<<

类和类、类和接口、接口和接口之间有如下几种关系：泛化关系、实现关系、关联关系（聚合、合成）、依赖关系。

l  泛化：表示类与类之间的继承关系，使用extends关键字来表示。在图形上使用虚线三角形箭头表示。

l  实现：表示类与接口之间的实现关系，使用implements关键字来表示。在图形上使用实线三角形箭头表示。

l  关联：类与类之间的联接。关联可以是双向的，也可以是单向的，双向的关联可以有两个箭头或都没有箭头。单向的关联有一个箭头，表示关联的方向。在Java里，关联关系是使用实例变量实现的。在每一个关联的端点，还可以有一个基数，表时这一端的类可以有几个实例。常见的基数有：0..1（零个或者一个实例）、0..*或者*（没限制，可以是零）、1（只有一个实例）、1..*（至少有一个实例）。一个关联关系可以进一步确定为聚合与合成关系。在图形上使用实线的箭头表示。

l  聚合：是关联关系的一种，是强的关联关系，聚合是整体和个体之间的关系。关联与聚合仅仅从Java语法是上是分辨不出的，需要考察所涉及的类之间的逻辑关系。如果不确定是聚合关系，可以将之设置为关联关系。图形是在实线的箭头的尾部多一个空心菱形。

l  合成：是关联关系的一种，是比聚合关系强的关系。它要求普通的聚合关系中代表整体的对象负责代表部分的对象的生命周期。整体消亡，则部分与消亡。图形是在实线的箭头的尾部多一个黑心菱形。

l  依赖：类与类之间的连接，依赖总是单向的。表示一个类依赖于另一个类的定义。一般而言，依赖关系在Java里体现为局部变量、方法的参数、以及对静态方法的调用。但如果对方出现在实例变量中，那关系就超过了依赖关系，而成了关联关系了。在图形上使用虚线的箭头表示。



### 第二章一切都是对象

\>>>>对象存放位置与生命周期<<<<

C++创建的对象可以存放在栈、静态存储区与堆（heap）中，放在栈中的对象用完后不需手动释放，会自动销毁，但放在堆中的对象需手动释放，栈中的对象所需空间与生命周期都是确定的，堆中的对象内存分配是动态的，在运行时才知道需要多少内存以及生命周期，如果说在堆上创建对象，编译器就会对它的生命周期一无所知，C++就需要以编程的方式来确定何时销毁对象，这可能因不正确处理而导致内存泄漏，而Java则提供了自动垃圾回收机制。

Java对象的创建采用了动态内存分配策略，即创建的堆都是放在堆中的。

\>>>>数据内存分配<<<<

寄存器——位于处理器内部，这是最快的存储区，大小极其有限，一般不能直接控制，但C和C++允许你向编译器建议寄存器分配方式。

堆栈——位于通用RAM（随机访问存储器）中，堆栈指针向下移动，则分配新的内存；若向上移动，则释放内存。这是一种快速有效的分配方法，速度仅次于寄存器。创建程序时，Java系统必须知道存储在堆栈内所有项的确切生命周期，以便上下移动堆栈指针。这一约束限制了程序的灵活性，所以虽然某些Java数据存储于堆栈中（如对象引用），但是Java对象并不存储于其中。

堆——一种通用的内存池（也位于RAM区），用于存放所有的Java对象，堆不同于堆栈的好处是，编译器不需要知道存储的数据在堆里存活多长时间。因此，在堆栈分配存储有很大的灵活性。当然，这种灵活性导致了分配需要更多的时间，时间效率上不如堆栈。

常量存储：常量值通常直接存放到程序代码内部，这样做是安全的，因为它们永远不会被改变。

非RAM存储：数据可完全存活于程序之外，在没有运行机制时也可以存在，如持久化对象的存放。

JVM有两类存储区：常量缓冲池和方法区。常量缓冲池用于存储类名称、方法和字段名称以及串常量。方法区则用于存储Java方法的字节码。

 

[![image003](https://images0.cnblogs.com/blog/717614/201502/132208114485231.gif)](http://www.cnblogs.com/$image003[3].gif)

Java字节码的执行有两种方式：

　　1.即时编译方式：解释器先将字节码编译成机器码，然后再执行该机器码。

　　2.解释执行方式：解释器通过每次解释并执行一小段代码来完成Java字节码程 序的所有操作。

通常采用的是第二种方法。由于JVM规格描述具有足够的灵活性，这使得将字节码翻译为机器代码的工作具有较高的效率。对于那些对运行速度要求较高的应用程序，解释器可将Java字节码即时编译为机器码，从而很好地保证了Java代码的可移植性和高性能。

 

#### C/C++内存分配： 

堆和栈的区别

一、预备知识—程序的内存分配

一个由C/C++编译的程序占用的内存分为以下几个部分

1、栈区（stack）— 由编译器自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。

2、堆区（heap） — 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收 。注意它与数据结构中的堆是两回事，分配方式倒是类似于链表，呵呵。

3、全局区（静态区）（static）—，全局变量和静态变量的存储是放在一块的，初始化的全局变量和静态变量在一块区域， 未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。 - 程序结束后有系统释放

4、文字常量区—常量字符串就是放在这里的。 程序结束后由系统释放

5、程序代码区—存放函数体的二进制代码。

二、例子程序

这是一个前辈写的，非常详细

```c++
//main.cpp

int a = 0; 全局初始化区

char *p1; 全局未初始化区

main()

{

    int b; 栈

    char s[] = "abc"; 栈

    char *p2; 栈

    char *p3 = "123456"; "123456"在常量区，p3在栈上。

    static int c =0； 全局（静态）初始化区

    p1 = (char *)malloc(10);

    p2 = (char *)malloc(20);

    //上面分配得来得10和20字节的区域就在堆区。

    strcpy(p1, "123456"); "123456"放在常量区，编译器可能会将它与p3所指向的"123456"优化成一个地方，即指向同一个字符串。

}
```

 

二、堆和栈的理论知识

2.1申请方式

stack:

由系统自动分配。 例如，声明在函数中一个局部变量 int b; 系统自动在栈中为b开辟空间

heap:

需要程序员自己申请，并指明大小，在c中malloc函数

如p1 = (char *)malloc(10);

在C++中用new运算符

如p2 = (char *)malloc(10);

但是注意p1、p2本身是在栈中的。

 

2.2

申请后系统的响应

栈：只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。

堆：首先应该知道操作系统有一个记录空闲内存地址的链表，当系统收到程序的申请时，

会遍历该链表，寻找第一个空间大于所申请空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序，另外，对于大多数系统，会在这块内存空间中的首地址处记录本次分配的大小，这样，代码中的delete语句才能正确的释放本内存空间。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会自动的将多余的那部分重新放入空闲链表中。

 

2.3申请大小的限制

栈：在Windows下,栈是向低地址扩展的数据结构，是一块连续的内存的区域。这句话的意思是栈顶的地址和栈的最大容量是系统预先规定好的，在 WINDOWS下，栈的大小是2M（也有的说是1M，总之是一个编译时就确定的常数），如果申请的空间超过栈的剩余空间时，将提示overflow。因此，能从栈获得的空间较小。

堆：堆是向高地址扩展的数据结构，是不连续的内存区域。这是由于系统是用链表来存储的空闲内存地址的，自然是不连续的，而链表的遍历方向是由低地址向高地址。堆的大小受限于计算机系统中有效的虚拟内存。由此可见，堆获得的空间比较灵活，也比较大。

 

2.4申请效率的比较：

栈由系统自动分配，速度较快。但程序员是无法控制的。

堆是由new分配的内存，一般速度比较慢，而且容易产生内存碎片,不过用起来最方便.

另外，在WINDOWS下，最好的方式是用VirtualAlloc分配内存，他不是在堆，也不是在栈是直接在进程的地址空间中保留一快内存，虽然用起来最不方便。但是速度快，也最灵活。

 

2.5堆和栈中的存储内容

栈： 在函数调用时，第一个进栈的是主函数中后的下一条指令（函数调用语句的下一条可执行语句）的地址，然后是函数的各个参数，在大多数的C编译器中，参数是由右往左入栈的，然后是函数中的局部变量。注意静态变量是不入栈的。

当本次函数调用结束后，局部变量先出栈，然后是参数，最后栈顶指针指向最开始存的地址，也就是主函数中的下一条指令，程序由该点继续运行。

堆：一般是在堆的头部用一个字节存放堆的大小。堆中的具体内容有程序员安排。

 

2.6存取效率的比较

char s1[] = "aaaaaaaaaaaaaaa";

char *s2 = "bbbbbbbbbbbbbbbbb";

aaaaaaaaaaa是在运行时刻赋值的；

而bbbbbbbbbbb是在编译时就确定的；

但是，在以后的存取中，在栈上的数组比指针所指向的字符串(例如堆)快。比如：

```c++
#include

void main()

{

    char a = 1;

    char c[] = "1234567890";

    char *p ="1234567890";

    a = c[1];

    a = p[1];

    return;

}
```

对应的汇编代码

```
10: a = c[1];

00401067 8A 4D F1 mov cl,byte ptr [ebp-0Fh]

0040106A 88 4D FC mov byte ptr [ebp-4],cl

11: a = p[1];

0040106D 8B 55 EC mov edx,dword ptr [ebp-14h]

00401070 8A 42 01 mov al,byte ptr [edx+1]

00401073 88 45 FC mov byte ptr [ebp-4],al
```

第一种在读取时直接就把字符串中的元素读到寄存器cl中，而第二种则要先把指针值读到edx中，在根据edx读取字符，显然慢了。

 

\>>>>基本类型<<<<

void属于基本类型，但只能用来修饰方法，不能用来修饰变量。

[![image004](https://images0.cnblogs.com/blog/717614/201502/132208160425428.jpg)](http://www.cnblogs.com/$image004[3].jpg)

 

只要两个操作数中有一个是double类型的，另一个将会被转换成double类型，并且结果也是double类型；

否则，只要两个操作数中有一个是float类型的，另一个将会被转换成float类型，并且结果也是float类型；

否则，只要两个操作数中有一个是long类型的，另一个将会被转换成long类型，并且结果也是long类型；

否则，两个操作数（包括byte、short、int、char）都将会被转换成int类型，并且结果也是int类型。

Java提供了两个用于高精度计算类：BigInteger和BigDecimal，大体属于“包装器类”范畴，但都没有对应的基本类型。

BigInteger支持任意精度的整数，可表示任何大小的整数值。

BigDecimal支持任意精度的定点数，例如，可以用它进行精确的货币计算。

 

\>>>>引用与对象生存周期<<<<

{

​       String s = new String("a string");

}

引用s在作用域终点就消失了，然而，s指向的String对象继续占据内存，最后由垃圾回收器回收。 

\>>>>方法签名<<<<

方法名和参数列表（合起来被称为“方法签名”）唯一地标识出某个方法。

\>>>>static修饰字段与方法的区别<<<<

一个static字段对每个对象来说都只有一份空间，而非static字段则是对每个对象都有一个存储空间，但是如果static作用于某个方法时，差别却没有那么大，static方法的一个重要的用法就是在不创建任何对象的前提下就可以调用它，这一点对定义main()方法很重要，该方法是运行时程序的一个入口。

 

### 第三章操作符

char c = 0xffff;//最大字符串

byte b = -0x80;//最小字节型 或byte b = (byte)0x80; 因为0x80形式为正，即第一位不是符号位而是数字位，所以超过byte范围，但-0x80却明确说明第一位是符号位，即为负，没有超过byte范围类型

byte b = 0x7f;//最大字节型

如果将比较小的类型传递给Integer.toBinaryString()方法，则该类型将自动被提升为int再执行。

如果对char、byte或short类型的数值进行移位处理，那么在移位进行之前，它们会被转换为int类型，并且得到的结果也是一个int类型的值；如果为int与long，则结果类型不变。

“移位”可与“等号”（<<=、>>=、>>>=）组合使用，此时，操作符左边的值会移动由右边的值指定的位数，再将得到的结果赋给左边的变量，但在进行“无符号”右移结合赋值操作时，可能会遇到一个问题：如果对byte或short值进行这样的位移运算时，得到的可能不是正确的结果，它们会先转换成int类型，再进行右移操作，然后被截断，赋值给原来的类型，在这种情况下可能得到-1的结果。 

```java
**long** l = -1;

System.*out*.println(Long.*toBinaryString*(l));// 64个1

l >>>= 10;

System.*out*.println(Long.*toBinaryString*(l));// 54个1

 

**int** i = -1;

System.*out*.println(Integer.*toBinaryString*(i));// 32个1

i >>>= 10;

System.*out*.println(Integer.*toBinaryString*(i));// 22个1

 

**byte** b = -1;

// 字节型-1提升到整型-1，所以结果为32个1

System.*out*.println(Integer.*toBinaryString*(b));// 32个1

// b先提升到int 即在原来8个1前再补24个1，然后无符号右移，再截最后8位并赋值给b

b >>>= 10;

// b会先提升为int，再运算

System.*out*.println(Integer.*toBinaryString*(b));// 32个1

b = -1;

// 不使用 >>>= 时，结果为正确，虽然在执行前b提升为int，但计算后没有截断，直接输出整型结果

System.*out*.println(Integer.*toBinaryString*(b >>> 10));// 22个1
```



将float或double转型为整型值时，总是对该数字执行截尾（(**int**) 0.7结果为0），如果想要得到舍入的结果，就需要使用java.lang.Math中的round()方法（Math.*round*(0.7)结果为1）。

一般来说，如果在程序里使用了“直接常量”，编译器可以准确地知道要生成什么样的类型，但有时候却是不明确的，此时可以在常量后附加上一些特定类型字符来明确。

```java
/*

 * 计算表达式中如果都整型数常量，则计算过程中还是整型，这有可能引起计算

 * 溢出问题，但编译与运行都不会报错，对于此种情况，在表达式中的某个常量

 * 后加上L即可使表达式计算过程中以long型来计算

*/

**long** l = 2147483647 * 2;// -2

l = 2147483647L * 2;// 4294967294

 

// 编译运行都不会错

**int** i = 2147483647 * 2;// -2

 

// !! 以下编译时就出错

// short s1 = 32767 * 2;

// byte b1 = 127 * 2;

 

// short s2 = 16384 * 2;

// byte b2 = 64 * 2;

 

// short s3 = 2147483647 * 3;//乘以奇数就不行，偶数就可以？

// byte b3 = 2147483647 * 3;

 

// 但下面可编译运行

**short** s = 2147483647 * 2;// -2

**byte** b = 2147483647 * 2;// -2
```

 

### 第四章流程控制

Foreach循环可用于数组，以及实现了java.util.Iterator接口的对象。

```java
public interface Iterable<T> {

	Iterator<T> iterator();

}
```

如果在返回void的方法中没有return语句，那么在该方法的结尾处会有一个隐式的return，因此在方法中并非总是必须要有一个return语句。但是，如果一个方法声明它将返回void之外的其他东西，那么必须确保每一条代码路径都将返回一个值。

```java
int i;

for (i = 0; i <= 5; i++) {

       i (i == 2) {

              break;//退出时i不会再递增，但continue会

       }

}

System.out.println("i=" + i);//2
```

for(;;)与while(ture)等效。

尽管goto仍是Java中的一个保留字，但在语言中并未使用它，Java没有goto。但可使用带标签的 continue或break来完成类似的跳转操作。

带标签与不带标签的continue、break用于迭代语句时规则：

一般的continue会退回到最内层循环的开头，并继续执行循环。

带标签的continue会到标签的位置，并重新进入紧接在那个标签后面的循环。

一般的break会中断并跳出当前循环。

带标签的break会中断并跳出标签所指的循环。

在Java里需要使用标签的唯一理由就是因为有循环嵌套存在，而且想从多层嵌套中break或continue到外层循环外。

标签只能紧跟在循环语句前（注：如果中间还有其他语句，则continue与break语句编译出错，但如果标签不应用到continue与break中，则不会有问题）

```java


int i = 0, j = 0;

outer:

// !! 注，标签的下面不能写任何其他非迭代语句

for (; i < 5; i++) { // 死循环

       inner:

       // !! 注，标签的下面不能写任何其他非迭代语句

       for (; j < 10; j++) {

              System.out.println(("i=" + i + " j = " + j));

              if (j == 2) {

                     System.out.println("continue");

                     continue;// 回到内层循环起始处继续执行内层循环，j会自动递增

              }

              if (j == 3) {

                     System.out.println("break");

                     // 为了下次循环不再走该分支，则要使用i递增1，因为break后j不会自动递增

                     j++;

                     break;// 跳出内层循环，回到外层循环起始处继续执行外层循环

              }

              if (j == 7) {

                     System.out.println("continue outer");

                     // 由于带标签的continue跳到了外层循环起始处，所以j不会自动递增，但为了

                     // 下一次不再走该分支，所以要手动递增1

                     j++;

                     continue outer;

              }

              if (j == 8) {

                     System.out.println("break outer");

                     break outer;// 当j为8时，退出内外层循环，实质上执行最后打印语句

              }

              for (int k = 0; k < 5; k++) {

                     if (k == 3) {

                            System.out.println("continue inner");

                            // j为0、1、4、5、6时分别会执行一遍

                            continue inner;

                     }

              }

       }

}

// 由于 break outer 跳出，所以i不会递增，最后还是2

System.out.println(("i = " + i));

 

switch(integer-selector){
	case integer-value1: statement;break;
    case integer-value2: statement;break;
    case integer-value3: statement;break;
    //…
    default: statement;break;

}
```

switch语句是可用的选择数据类型有 int、char、enum。

 

case均以一个break结尾，这样可使执行流程跳转至switch主体的末尾。这是构建switch语句的一种传统方式，但break是可选的。若省略break，会继续执行后面的case语句，直到遇到一个break为止。注意最后的default语句没有break，因为执行流程已到了break的跳转目的地。当然，如果考虑到编程风格方面的原因，完全可以在default语句的末尾放置一个break，尽管它并没有任何实际的用处。

 

### 第五章初始化与清理

重载：方法名相同，参数列表不同（参数类型、个数、顺序）。

以基本类型参数重载方法时，会优先调用该类型的参数的方式，如果没有找到相同类型时，则提升最近的参数类型。

this(type x)用来在构造函数中调用其他构造函数，也只能用于构造函数中，并且只能用在第一行。

如果你的对象（并非使用new）获得了一块“特殊”的内存区域中，由于垃圾回收器只知道释放那些经由new分配的内存，所以它不知道该如何释放该对象的这块“特殊”内存。

finalize()：该方法并不是用来释放由Java程序本身分配的空间，因为它有可能不被调用，由Java程序本身分配的空间会由JVM在适当的时机释放，有可能不调用，分配的空间直到程序运行完时一次性还给操作系统，那其作用是什么呢？由于在分配内存时可能采用了类似C语言中的做法，而非Java中的通常做法。这种情况主要发生在使用“本地方法”的情况下，在非Java代码中，也许会调用C的malloc()函数系列来分配存储空间，而且除非调用了free()函数，否则存储空间将得不到释放，从而造成内存泄露。当然free()是C与C++中的函数，所以需要在finalize()中用本地方法来调用它。

C++中在堆栈中创建的对象相当于局部对象（Java是不可以在栈上创建对象的），会在方法调用完毕时自动销毁对象。但如果是采用的new方式创建的对象（类似Java），那么程序员调用C++的delete操作符时（Java中没有该命令），就会调用相应的析构函数。如果没有调用delete，那么就不会调用析构函数，这样就会引起内存泄露。

\>>>>可变参数<<<<

如果有多个参数，则可变参数只能放在最后，否则编译不能通过。

可变参数接收到所有参数时，实质上是一个数组，所以可变参数的所有参数都是同一类型的。

即然是可变的，当然我们也可不输出任何参数。

 

```java
public static void main(String[] args) {

       print("you input:", 1, 2, 3);

       //与下面等效，即可变参数也可使用数组进行传递

       print("you input:", new int[] { 1, 2, 3 });

}

private static void print(String msg, int... is) {

       System.out.print(msg + " ");

       for (int i : is) {

              System.out.print(i + " ");

       }

}
```

可变参数重载时，有时有问题，如下：

static void f(float i, Character... args) {}

static void f(Character... args) {}

因为在只传一个参数时可能引起模糊，所以这样是不允许的，如果你给这两个方法都添加一个非可变参数，就可以解决问题：

static void f(float i, Character... args) {}

static void f(char c, Character... args) {}

 

### 第六章访问权限控制

| 继承与访问   | public | protected | 缺省 | private |
| ------------ | ------ | --------- | ---- | ------- |
| 同包同类     | Y      | Y         | Y    | Y       |
| 同包子类     | Y      | Y         | Y    | N       |
| 同包不子类   | Y      | Y         | Y    | N       |
| 不同包子类   | Ｙ     | Ｙ        | Ｎ   | Ｎ      |
| 不同包不子类 | Ｙ     | Ｎ        | Ｎ   | Ｎ      |

要学会把变动的代码与保持不变的代码区分开来。

如果有必要，你尽可能将一切方法都定为private。

非public类在其它包中是访问不到的。

所有默认包的类都是属于同一个包，尽管它们在不同的文件夹下面。

private，只允许本类所有对象可访问，其他任何类都不能访问，哪怕是它的子类或同一包中的类都不行。

如果一类继承自不同包中的类，则该子类不能继承与访问父类中的“包访问权限或默认访问权限”的属性与方法，只能继承与访问protected或public的属性与方法。

设计一个类时一般按照public、protected、默认、private的顺序来定义属性与方法，这样让人最先注意到公开的，当然，我们一般把接口与实现分离更好，这样用户只关心他所需要的接口，而对实现不必理会。

控制对成员的访问权限有两个原因：第一是为了使用户不必关心那此不必关心的private部分，而只关心类提供了哪些服务接口，即public修饰的部分。第二就是让类库设计者可以更改类的内部工作方式，而不必担心这样会客户程序产生重大的影响。

 

\>>>关于protected修饰符<<<

定义规则前，我这里约定有三个类，一个是Base类，一个是Base类的子类Sub类，一个是Sub类的子类SubSub类，另一个是Other类且与Base、Sub、SubSub没有继承关系，并假设Base中有protected方法与属性，都叫YYY吧。

先看看protected规则：首先要搞清楚什么叫访问？这里在讲到的访问是有二种的：

一、就是在类中通过“XXX x = new XXX(); x.YYY;”的形式来访问（不妨叫此种形式为“外部访问”吧，此种访问形式除了可以应用到自己与子类中外，还可以应用在其他类中访问，其中XXX表示定义的类型，这里可为Base与Sub、SubSub，YYY为方法或属性）；

二、就是this.YYY的形式来访问（不妨叫此种形式为“内部访问”吧，不过这种访问形式只能应用在在自己的类或是子类中）。

**protected方法与属性可访问的地方有三个：**

1. 在自己的类Base中：上面的“XXX x = new XXX(); x.YYY;”与“this.YYY”两种访问形式都可以访问的到自己定义的portected方法或属性；
2. 二是子类Sub、SubSub中，这要分三种访问方式：
   -  在Sub、SubSub 中的“this.YYY”内部访问形式：在此种方式形式下，不管是否重写或重新定义过父类Base中protected方法与属性，子类Sub、SubSub一定可以访问的。
   - 在Sub、SubSub 中“Base x = new XXX (); x.YYY;”外部访问形式：此种形式就不一定的能访问的到了，这要看父类Base与子类Sub、SubSub是否在同一包（注意，此时与是否重写或重新定义过这些protedted方法与属性没有关系）；
   - 在SubSub 中“Sub x = new XXX (); x.YYY;” 外部访问形式：此种访问形式能否访问关键看Sub是否重写或重新定义过Base的属性与方法：
     -  如果重写或重新定义过，则看Sub与SubSub是否在同包中
     -  如果没有，则看Base与SubSub是否在同包中
3. 在其他类Other中：此时只支持外部访问形式，不过到底是要求Other与Base同包还是要求Other与Sub同包，则要依你访问方式而定了：
   -  如果是通过父类引用“Base x = new XXX (); x.YYY;”形式来访问的，则要求Other与Base同包；
   - 如果是通过子类引用“Sub x = new Sub (); x.YYY;”形式来访问的，情况又会比较复杂了，此时关键是看子类Sub是否重写或重新定义过父类Base中的protected方法与属性：
     - 如果重写或重新定义过了，则要求Other与Sub同包即可；
     -  如果没有重写或重新定义过了，则要求Other与Base同包即可；

 

规则总结肯定有遗漏的地方，不过我只想到这些，希望大家一起看看。看文字比较绕，下面来应用上面的规则看看这些实例也许会好理解一些：

```java
package pk1.a;

public class Base {

       protected int i = 1;

       protected void protect() {

              System.out.println("Base::protect");

       }

}

 

package pk1.a;

import pk1.b.Sub;

public class SubSub extends Sub {

       void g() {

              Sub s = new SubSub();

              //!! s.protect();//规则2.c.i

              System.out.println(s.i);//规则2.c.ii

       }

}

 

package pk1.b;

import pk1.a.Base;

public class Sub extends Base {

       private void prt() {}

       protected void protect() {

              System.out.println("Base::protect");

       }

       void f() {

              //规则2.a

              this.protect();

              this.i = 2;

 

              //规则2.b

              Base a2 = new Sub();

              //!! a2.protect();

              //!! System.out.println(a2.i);

 

              //规则1

              Sub b = new Sub();

              b.protect();

              b.i = 1;

              b.prt();

       }

}

 

package pk1.b;

public class SubSub extends Sub {

       void g() {

              Sub s = new SubSub();

              s.protect();//规则2.c.i

              //!! System.out.println(s.i);//规则2.c.ii

       }

}

 

package pk1.c;

import pk1.a.Base;

import pk1.b.Sub;

public class SubSub extends Sub {

       void g() {

              this.protect();//规则2.a

 

              //规则2.b

              Base b = new SubSub();

              //!! b.protect();

              //!! System.out.println(b.i);

 

              //规则2.b

              Sub s = new SubSub();

              //!! s.protect();

              //!! System.out.println(s.i);

 

       }

}

 

package pk2.a;

public class Base {

       protected int i = 1;

 

       protected void protect() {

              System.out.println("Base::protect");

       }

}

 

package pk2.a;

import pk2.b.Sub;

public class Other {

       void g() {

              //规则3.a

              Base b = new Sub();

              b.protect();

              System.out.println(b.i);

 

              //规则3.b.ii

              Sub s = new Sub();

              s.protect();

              System.out.println(s.i);

       }

}

 

package pk2.b;

import pk2.a.Base;

public class Other {

       void g() {

              //规则3.a

              Base b = new Sub();

              //!! b.protect();

              //!! System.out.println(b.i);

 

              //规则3.b.ii

              Sub s = new Sub();

              //!! s.protect();

              //!! System.out.println(s.i);

       }

}

 

package pk2.b;

import pk2.a.Base;

public class Sub extends Base {}

 

package pk3.a;

import pk3.b.Sub;

public class Base {

       protected int i = 1;

       protected void protect() {

              System.out.println("Base::protect");

       }

      

       static protected int i_s = 1;

       static protected void protect_s() {

              System.out.println("Static:Base::protect");

       }

      

       void f() {

              //!! Sub.i_s = 2; //规则3.b.i

              Sub.protect_s();//规则3.b.ii

       }

}

 

package pk3.a;

import pk3.b.Sub;

public class Other {

       void g() {

              Sub s = new Sub();

              //!! s.protect();//规则3.b.i

              System.out.println(s.i);//规则3.b.ii

       }

 

       void f() {

 

              //!! Sub.i_s = 2; //规则3.b.i

              Sub.protect_s();//规则3.b.ii

 

              Base.i_s = 2;//规则3.a

              Base.protect_s();//规则3.a

 

       }

}

 

package pk3.b;

import pk3.a.Base;

public class Other {

       void f() {

              Sub.i_s = 2;//规则3.b.i

              //!! Sub.protect1();//规则3.b.ii

             

              //!! Base.i1 = 2;//规则3.a

              //!! Base.protect1();//规则3.a

       }

}

 

package pk3.b;

import pk3.a.Base;

public class Sub extends Base {

       protected void protect() {

              System.out.println("Base::protect");

       }

       static protected int i_s = 2;

 

       void f() {

             

              /*

               * 在子类中可能通过子类类型或父类类型来来访问父类中protected静态

               * 成员，而不管子类与父类是否在同一包中，或是子类重新定义了这些成员

               *

               * 注，在父类或子类中访问时后面的规则不再适用

               */

              System.out.println(Sub.i_s);//2

              Sub.protect_s();

      

              System.out.println(Base.i_s);//1

              Base.protect_s();

       }

}
```

最后，通过上面的规我们可以很好的解释Object.clone()类似的问题：Object.clone()访问修饰符为protected，如果某个类没有重写此方法，则Object中的clone()方法除被自己与子类能调用方法外，其他不管与这个类在同一包还是不同包都是不可见的，因为未重写，还是属于Object中的方法，又Object在java.lang包中，与我们定义的包又不在java.lang包中，所以不能访问到（这也与你在在程序里定义了Object o = **new**Object();你还是不能在当前类中调用o.clone();一样）。所以如果要能被不同包中的非子类克隆，则需重写Object.clone()并设置访问权限为public（如果重写后还是protected，则还是只能被同一包访问）。

 

```java
package a;

public class A {

       protected void f() {}

      

       void g() {}

      

       private void p(A a) {}

      

       private void m(A a) {

              p(a);

       }

 

       public static void main(String[] args) {

              A a = new A();

              a.m(a);

       }

}

 

package b;

import a.A;

public class B extends A {

       protected void f() {}

 

       private void p() {}

 

       public void call1(B b) {

              b.f();// 此种访问方式属于访问同类中方式，是没有问题的

              f();// 属于访问自已的方法        

              // !! g();// 父类中的包访问权限方法不能被子类继承访问

              b.p();//在同一类中是可以访问私有方法的，访问控制是针对整个类来说的，而不是某个对象

       }

 

       public void call12(A a) {

              /*

               * 即使是f()是父类中的protected方法，并且子类还重写过了 ，但

               * 是这种以父类类型实例访问父类中的protected方式属于不同包非

               * 子 类访问方式，所以不能访问，即使访问的代码在子类中!

               */

              // !! a.f();

              A a1 = new B();

              // 此种访问与上面是一样也不能编译

              // !! a1.f();

             

             

       }

 

       public static void main(String[] args) {

              B b = new B();

              b.f();

 

              A a = new B();

              // 与上面 call2方法中的调用一样，也是不能访问的

              // !! a.f();

       }

}
```



### 第七章复用

代码复用可分为组合复用与继承复用。

属性成员初始化的4个时机：定义时、构造器中、块中、使用时。

main方法所在的类不一定要是public的（但方法一定要定义成public，如果不是public方法，则启动时会报找不到main方法）为包访问也是可以的，照样可以用作程序入口。另外，main方法也可以由另一个main方法来调用，这也是允许的。

继承并不只是复制基类的接口，当创建了一个子类的对象时，该对象包含了一个基类的子对象，这个子对象与你用基类直接创建的对象是一样的，二者区别在于，后者来自于外部，而基类的子对象被包装在子类对象内部。

当我们亲自处理清理时，要加以小心，因为，一旦涉及垃圾回收，能够信赖的事就不会很多了，垃圾回收器可能永远也无法被调用，即使用被调用，它可能以任何它想要的顺序来回收对象。最好的办法是除了内存以外，不能依赖垃圾回收器去做任何事，如果需要进行清理，最好编写你自己的清理方法，但不要使用finalize()，最好是在finally块调用自己的清理方法。

组合和继承都允许在新的类中放置子对象，组合是显示地这样做，而继承则是隐藏地做。

组合技术通常用于想在新类中使用现有类的功能而非它的接口这种情形。即，在新类中嵌入某个对象，让其实现所需要的功能，但新类的用户看到的只是为新类所定义的接口，而非所嵌入对象的接口，一般需在新类中嵌入一个现有类的private对象。

向上转型是从一个较专用类型向较通用类型转换，所以总是很安全的。向上转型的过程中，类接口唯一可能发生的事情是丢失方法，而不能访问它们。

到底是该用组合还是用继承，一个最清晰的判断办法就是问一问自己是否需要从新类向基类进行向上转型。如果必须向上转换，则继承是必要的，但如果不需要，则应当好好考虑自己是否需要继承。

使用static与final域只占据一段不能改变的存储空间。

final数据在编译时可能还不能确定其值，比如值是通过某个方法计算才能得到的，需在运行时才能确定。

对于编译期常量这种情况，编译器可以将该常量值代入任何可能用到它的计算式中，也就是说，可以在编译时执行计算式，这可减轻一些运行时的负担。在Java中，这类常量必须是基本数据类型。

在Java早期实现中，如果将一个方法指明为final，就是同意编译器将针对该方法的所有调用都转为内嵌调用。当编译器发现一个final方法调用命令时，它会跳过插入程序代码这种正常方式而执行方法调用机制，并且以方法体中的实际代码的副本来替代方法调用，这将消除方法调用的开销。当然，如果方法本身很大，这种效率提升不是很明显，反而可能下降。在最近的Java版本中，虚拟机可以探测到这些情况，并优化去掉这些效率反而降低的额外的内嵌调用，因此不再需要使用final方法来进行优化了，事实上，这种做法正在逐渐受到劝阻，在使用JavaSE5/6时，应该让编译器和虚拟机去处理效率问题，只有在想要明确禁止覆盖时，才将方法设置为final。

类中所有的private方法都隐式地指定为final，可以对private方法添加final修饰词，但这并不能给该方法增加任何额外的意义。

final类中定义的属性成员可以是final的，也可以不是的。但是，所有final类的中方法默认都是final的，因此在final类中的方法前添加final修饰符没有什么意义。

有关final、finally、finalize区别请参考XXX

### 第八章多态

多态通过分离做什么和怎么做，从另一角度将接口和实现分离开来，多态不但能够改善代码的组织结构和可读性，还能够创建可扩展的程序。

前期绑定在编译时期（如C），后期绑定是在运行时期（即多态，C++与Java），多态得程序的后期扩展。

Java中除了static方法和final方法（private方法属于final方法），其他所有的方法都是后期绑定。这意味着通常情况下，我们不必判定是否应该进行后期绑定——它会自动发生。

final方法会关闭动态绑定。

类的构造方法隐式的为static，它们实际上是static方法。

如果父类构造函数抛出异常，子类构造函数一定要抛出，不能被捕获。

覆写/重写父类方法时，子类方法异常处理有以下几种：

```java
import java.io.IOException;

public class Parent {

	public void overwrite(int i) throws IOException {}

	public void overwrite() throws NullPointerException {}

}
```

 

```java


import java.io.ObjectStreamException;

class SubSub extends Parent {

       /*

        * 父类抛出捕获型异常，子类却抛出运行时异常，这是可以，因为

        * 抛出运行时就相当于没有抛出任何异常

        */

       public void overwrite(int i) throws RuntimeException {}

       /*

        * 如果父类抛出的是非捕获型异常，则子类可以抛出任意非捕获型异

        * 常，没有扩大异常范围这一问题

        */

       public void overwrite() throws RuntimeException {}

}

 

class Sub2 extends Parent {

       //也可以不抛出任何异常

       public void overwrite(int i) {}   

       /*

        * 如果父类抛出的是非捕获异常，子类也可以不用抛出，这与父类为捕

        * 获型异常是一样的

        */  

       public void overwrite() {}

}

 

class Sub3 extends Parent {

       //如果抛出的是捕获型异常，则只能是IOException的子类

       //!!public void overwrite(int i) throws ObjectStreamException{}   

       /*

        * 如果父类抛出的是非捕获异常，子类就不能抛出任何捕获型异常，因

        * 为这样会扩大异常的范围

        */

       //!! public void overwrite() throws IOException {}

}

 
```

 

组合更加灵活，因为它可以动态选择类型，运行时可以动态的修改具体的行为，而继承在编译时就知道确切类型。“用继承表达行为间的差异，并用字段（组合）表达状态上的变化。”下面实例中，两者都用到了：通过继承得到了两个不同的类，用于表达act()方法的差异；而Stage通过运用组合使用自己的状态发生变化。

```java
//演员

class Actor {

  public void act() {}

}
```

```java
//继承（is-a关系）：HappyActor（喜剧演员）确实是一种Actor（演员）

class HappyActor extends Actor {

  public void act() { System.out.println("HappyActor"); }

}

 

//继承（is-a关系）：HappyActor（悲剧演员）确实是一种Actor（演员）

class SadActor extends Actor {

  public void act() { System.out.println("SadActor"); }

}

 

//舞台

class Stage {

  private Actor actor = new HappyActor();//组合（has-a关系）：舞台上有演员

  public void change() { actor = new SadActor(); }

  public void performPlay() { actor.act(); }

}

 

public class Transmogrify {

  public static void main(String[] args) {

    Stage stage = new Stage();

    stage.performPlay();

    stage.change();//运行过程中改变具体性为

    stage.performPlay();

  }

} /* Output:

HappyActor

SadActor

*///:~

 
```

private方法不能被覆写，即使子类覆写也不会起作用，根据调用它的引用类型来调用相应的方法，实质上private方法就是final方法，所以在编译期就已确定调用哪个方法。

```java
public class PrivateOverride {

  private void f() { System.out.println("private f()"); }

  public static void main(String[] args) {

    PrivateOverride po = new Derived();

    po.f();

  }

}

 

class Derived extends PrivateOverride {

  public void f() { System.out.println("public f()"); }

} /* Output:

private f()

*///:~

```

只有非静态的方法才有可能构造多态，属性成员（即使是public）与静态方法不会造成多态，即使用子类重写了这些属性成员与静态方法，因为这此是在编译期进行解析的，而不是在运行时间确定的。因此在调用属性成员与静态方法时只与调用它的引用类型相关。

```java
class Super {

       public int field = 0;

 

       public int getField() {

              return field;

       }

}

 

class Sub extends Super {

       public int field = 1;

 

       public int getField() {

              return field;

       }

 

       public int getSuperField() {

              return super.field;

       }

}

 

public class FieldAccess {

       public static void main(String[] args) {

              Super sup = new Sub(); // Upcast

              System.out.println("sup.field = " + sup.field + ", sup.getField() = "

                            + sup.getField());

              Sub sub = new Sub();

              System.out.println("sub.field = " + sub.field + ", sub.getField() = "

                            + sub.getField() + ", sub.getSuperField() = "

                            + sub.getSuperField());

       }

} /*

* Output:

* sup.field = 0, sup.getField() = 1

* sub.field = 1, sub.getField() = 1, sub.getSuperField() = 0

*/// :~
```

上面实例中，为Super.field和Sub.field分配了不同的存储空间。这样，Sub实际上包含两个称为field的属性成员：它自己的和它从Super类中得到的。因此在子类中要得到父类中的同名属性成员时，则要使用super.field来获取。

```java
class StaticSuper {

  public static String staticGet() {

    return "Base staticGet()";

  }

  public String dynamicGet() {

    return "Base dynamicGet()";

  }

}

 

class StaticSub extends StaticSuper {

  public static String staticGet() {

    return "Derived staticGet()";

  }

  public String dynamicGet() {

    return "Derived dynamicGet()";

  }

}

 

public class StaticPolymorphism {

  public static void main(String[] args) {

    StaticSuper sup = new StaticSub(); // Upcast

    System.out.println(sup.staticGet());

    System.out.println(sup.dynamicGet());

  }

} /* Output:

Base staticGet()

Derived dynamicGet()

*///:~

 
```

如果自己处理清理动作，则销毁的顺序应该和初始化顺序相反。对于字段，则意味着与声明的顺序相反（因为字段的初始化是按照声明的顺序进行的）。对于基类，应该首先对其子类进行清理，然后才是基类，这是因为子类的清理可能会调用基类中的某些方法，所以需要使用基类中的构件仍起作用而不应过早地销毁它们，这与C++中的析构函数形式是一样的。

在父类的构造函数中调用多态方法时，可能会引发问题：

```java
class Glyph {

       void draw() {

              System.out.println("Glyph.draw()");

       }

 

       Glyph() {

              System.out.println("Glyph() before draw()");

              draw();//在父类构造函数中调用多态方法

              System.out.println("Glyph() after draw()");

       }

}

 

class RoundGlyph extends Glyph {

       private int radius = 1;

 

       RoundGlyph(int r) {

              radius = r;

              System.out.println("RoundGlyph.RoundGlyph(), radius = " + radius);

       }

 

       void draw() {

              System.out.println("RoundGlyph.draw(), radius = " + radius);

       }

}

 

public class PolyConstructors {

       public static void main(String[] args) {

              new RoundGlyph(5);

       }

} /* Output:

Glyph() before draw()

RoundGlyph.draw(), radius = 0

Glyph() after draw()

RoundGlyph.RoundGlyph(), radius = 5

*///:~
```

在构造函数内唯一能够安全调用的那些方法是基类中的final方法（当然也适用于private方法，它们自动属于final方法）。

子类覆盖父类方法时，子类的返回类型可以是父类返回类型的某个子类型，但返过来不行。

注，没有父子关系时返回类型要一样，如果允许子类返回类型是父类返回类型的子类型，则要求在J2SE5或以上版本

```java
class Grain {}

class Wheat extends Grain {}

 

class Mill {

       Grain p() { return new Grain(); }

       Wheat f() { return new Wheat(); }

       float g() { return 0;}

}

 

class WheatMill extends Mill {

       Wheat p() { return new Wheat(); }

      

       //子类覆盖父类方法时返回类型不能比子类宽

       //!! Grain f() { return new Grain(); }

      

       //虽然int可以隐式的转换成float，但int不是float的子类，所以编译不能通过

       //!! int g() { return 0;}

}

 
```

向上转型是安全的，因为基类不会具有大于子类的接口，因此，通过基类接口发送的消息保证都能被接；由于向下转型是不安全的，所以在Java语言中，所有向下转型都会得到检查！所以即使我们只是进行一次普通宾加括弧形式的类型转换，但在进入运行期时仍然会对其进行检查，以便保证它的确是我们希望的那种类型，如果不是，运行时就会抛出一个ClassCastException的类型转换异常，这种在运行期间对类型进行检查的行为称作“运行时类型识别”（RTTI），这种我们也可以通过编程的方式来保证，在后面反射章节我们会看到。

 

### 第九章接口

一个类同时继承与实现一个类与接口时，extends要在implements前。

接口中的方法前可以加abstract关键字，也可以不加，这都将是一种抽象方法。

接口中可以什么都没有，这时是一个标志性接口，如Serializable接口。

接口其实是一种很特殊的抽象类，是一种纯的抽象类。

接口中法前不能用static关键字，所以接口中没有静态的方法。

abtract关键词不能与static，private同时使用。

接口与抽象类不能用final来修饰。

接口中的有成员都是public，即使没有明确指出。

抽象类中抽象方法前默认访问修饰符为包访问限制，而不是public，这不象接口抽象方法前一定是public。

接口中不能有具体的方法，接口中的数据成员默认为public static final，数据成员如果写上访问控制符，只能是public，接口中的方法默认为public abstract，但可以省略。

接口中的数据成员一定要初始化，因为他是final型，但也只能在定义时就初始化，不能在静态区中赋值，因为在接口中是不能定义静态与非静态块的。

\>>>>完全解耦<<<<

只要一个方法操作的是类而非接口，那么你就只能使用这个类及其子类。如果你想要将这个方法应用于不在此继承结构中的某个类，那么你就会发现在不修改当然程序应用的情况下很难做到功能动态的扩展。但接口可以在很大程度上具有灵活性，因此，它使得我们可以编写可复用性更好的代码。

下面是程序的第一个版本：

刚开始时没有考虑到后期会有不同Processor处理器扩展，对当然应用来说，它已经是完美的了，程序基本上已做到了可扩展，我们可以随时添加一种其它字符处理器。

```java
//处理器类

class Processor {

   public String name() {

      return getClass().getSimpleName();

   }

 

       Object process(Object input) {

      return input;

   }

}

 

//大写处理器

class Upcase extends Processor {

       String process(Object input) {

      return ((String) input).toUpperCase();

   }

}

 

//小写处理器

class Downcase extends Processor {

       String process(Object input) {

      return ((String) input).toLowerCase();

   }

}

 

//应用

public class Apply {

   public static void process(Processor p, Object s) {

      System.out.println("Using Processor " + p.name());

      System.out.println(p.process(s));

   }

 

   public static String s = "Disagreement with beliefs is by definition incorrect";

 

   public static void main(String[] args) {

      process(new Upcase(), s);

      process(new Downcase(), s);

   }

}/* Output:

Using Processor Upcase

DISAGREEMENT WITH BELIEFS IS BY DEFINITION INCORRECT

Using Processor Downcase

disagreement with beliefs is by definition incorrect

*///:~
```

上面例子中，Apply.process()方法可以接受任何类型的Processor，并将其应用到一个Object对象上，然后输出结果，在本例中，创建一个能够根据所传递参数对象的不同而具有不同行为的方法被称为策略模式。这一类方法包含所要执行的算法是不变的，在这里不变的为Apply.process方法中的算法，而“策略”包含了变化的部分。策略就是传递进去的参数对象，它包含了动态执行的代码，在这里Processor对象就暗一个策略类，所有Processor类型的对象都能应用到Apply.process中。

 

下面我们来看看另一种与Processor功能相似的过滤器Filter（滤波器）：

```java
//波形

public class Waveform {

  private static long counter;

  private final long id = counter++;

  public String toString() { return "Waveform " + id; }

}

 

//滤波器，与Processor接口相同

public class Filter {

  public String name() {

    return getClass().getSimpleName();

  }

  public Waveform process(Waveform input) { return input; }

}

 

//低通滤波器

public class LowPass extends Filter {

  double cutoff;

  public LowPass(double cutoff) { this.cutoff = cutoff; }

  public Waveform process(Waveform input) {

    return input; // 什么都不做，直接返回

  }

}

 

//高通滤波器

public class HighPass extends Filter {

  double cutoff;

  public HighPass(double cutoff) { this.cutoff = cutoff; }

  public Waveform process(Waveform input) { return input; }

}
```

上面是另一种处理器，与第一个版本“字符处理器”用途相当，都是用来过滤的，这就意味着我们能否重用Apply.process()代码呢？第一个版本的Processor肯定是不行的，因为Filter并非继承自Processor（假设它来自于其它厂商），因为Filter类的创建者压根不清楚你想要将它用作Processor，因此你不能将Filter用于Apply.process()方法。这主要原因是Apply.process()与Processor之间的耦合过紧，已经超出了所需要的程度，这就无法复用Apply.process()的代码。

但是，如果Processor是一个接口，那么这些限制就会变得宽松，就可以重用Apply.process()方法算法。

 

下面是Processor和Apply的修改版本：

```java
//处理器接口，只要是处理器，都会遵循此接口，那么就可以重用Apply.process

public interface Processor {

  String name();

  Object process(Object input);

}

 

public class Apply {

  public static void process(Processor p, Object s) {

    System.out.println("Using Processor " + p.name());

    System.out.println(p.process(s));

  }

}
```

下面是重写“字符处理器”后的应用：

```java
//字符处理器，实现处理接口

public abstract class StringProcessor implements Processor{

  public String name() {

    return getClass().getSimpleName();

  }

  //处理接口

  public abstract String process(Object input);

  public static String s ="If she weighs the same as a duck, she's made of wood";

  public static void main(String[] args) {

    Apply.process(new Upcase(), s);

    Apply.process(new Downcase(), s);

  }

} 

 

//字符大写处理器

class Upcase extends StringProcessor {

  public String process(Object input) {

    return ((String)input).toUpperCase();

  }

}

 

//字符小写处理器

class Downcase extends StringProcessor {

  public String process(Object input) {

    return ((String)input).toLowerCase();

  }

}/* Output:

Using Processor Upcase

IF SHE WEIGHS THE SAME AS A DUCK, SHE'S MADE OF WOOD

Using Processor Downcase

if she weighs the same as a duck, she's made of wood

*///:~
```

 

下面来将Filter应用到Apply.process()方法中。由于在前面Filter已经开发完毕，假如我们现在不能修改Filter的源码，在这种情况下，我们可以使用适配器设计模式：

```java
//适配器，适配成Processor接口

class FilterAdapter implements Processor {

  Filter filter;//对象适配器

  public FilterAdapter(Filter filter) {

    this.filter = filter;

  }

  public String name() { return filter.name(); }

  public Waveform process(Object input) {

    return filter.process((Waveform)input);

  }

} 

 

//重用Apply.process**算法

public class FilterProcessor {

  public static void main(String[] args) {

    Waveform w = new Waveform();

Apply.process(new FilterAdapter(new LowPass(1.0)), w);

w = new Waveform();

    Apply.process(new FilterAdapter(new HighPass(2.0)), w);

  }

} /* Output:

Using Processor LowPass

Waveform 0

Using Processor HighPass

Waveform 1

*///:~
```

在这种使用适配器的方式中，FilterAdapter的构造器接受接口Filter，然后返回你所需要的Processor接口对象，另外，在FilterAdapter类中用到了代理。

将接口从具体实现中解耦使得接口可以应用于多种不同的具体实现，因此代码也就更具可复用性了。

\>>>>Java中的多重继承<<<<

有时我们需要“一个X是一个A和一个B以及一个C”，在C++中，组合多个类的接口的行为被称为“多重继承”，这可能会出现隐藏的问题，因为每个类都有一个具体实现，但在Java中，你只能继承一个类，因些，组合多个接口，C++中的问题是不会在Java发生的。

如果一个类继承了一个类，同时实现了某个接口，父类中有一个方法与接口中的方法签名是一样，则这个类默认就已实现了这个接口，无需再实现接口中的方法，这就是默认适配器模式。

我们应该如何选择接口还是抽象类？如果要创建不带任何方法定义和成员变量的基类，那么就应该选择接口而不是抽象类，一般接口灵活性高。

接口（interface）可以继承（extends）多个接口，但只能继承一个类。



组合多个接口时名字冲突：

```java
interface I1 { void f(); }

interface I2 { int f(int i); }

interface I3 { int f(); }

class C { public int f() { return 1; } }

 

class C2 implements I1, I2 {

  public void f() {}

  public int f(int i) { return 1; } // 重载

}

 

class C3 extends C implements I2 {

  public int f(int i) { return 1; } // 重载

}

 

class C4 extends C implements I3 {

  // 重写C中的方法，实现I3接口中的方法，这里可以省去该方法

  public int f() { return 1; } //重写

}

 

// 仅根据返回类型不同是不行的

//! class C5 extends C implements I1 {}

//! interface I4 extends I1, I3 {}
```

在打算组合的不同接口中使用相同的方法名通常会造成代码可读性混乱，请尽量避免这种情况。

\>>>>嵌套接口<<<<

```java
class A {

  interface B {//包访问权限接口

    void f();

  }

  public class BImp implements B {

    public void f() {}

  }

  private class BImp2 implements B {

    public void f() {}

  }

 

  public interface C {//公有访问权限接口

    void f();

  }

  class CImp implements C {

    public void f() {}

  }

  private class CImp2 implements C {

    public void f() {}

  }

 

  private interface D {//私有接口

    void f();

  }

  private class DImp implements D {

    public void f() {}

  }

  public class DImp2 implements D {

    public void f() {}

  }

 

  //虽然方法是公有的，但返回类型却是私有的，所以

  //该方法只能被有权的对象调用它，如：receiveD

  public D getD() { return new DImp2(); }

  private D dRef;

  public void receiveD(D d) {

    dRef = d;

    dRef.f();

  }

} 

 

interface E {

  // 接口中的内部接口默认就是 "public":

  interface G {

    void f();

  }

  // 多余的 "public":

  public interface H {

    void f();

  }

  void g();

  // 接口中的内部接口只能是public的

  //! private interface I {}

  //! protected interface I {}

}

 

public class NestingInterfaces {

  public class BImp implements A.B {

    public void f() {}

  }

  class CImp implements A.C {

    public void f() {}

  }

  // 不能实现私有接口，即private接口不能在定义它的类外实现

  //! class DImp implements A.D {

  //!  public void f() {}

  //! }

  //实现某个接口时，不一定要实现其嵌套接口

  class EImp implements E {

    public void g() {}

  }

  class EGImp implements E.G {

    public void f() {}

  }

  class EImp2 implements E {

    public void g() {}

    class EG implements E.G {

      public void f() {}

    }

  }

  public static void main(String[] args) {

    A a = new A();

    // 不能访问 A.D:

    //! A.D ad = a.getD();

   

    // 能这样，虽然返回的是私有类型，但外界没有访问

    a.getD();

   

    //虽然a.getD()返回的是私有类型A.D，但这里

    //强转成公有类型A.DImp2，这是没有问题

    A.DImp2 di1 = (A.DImp2) a.getD();

   

    // 即使A.D是public，但不能将父类型赋值给子类型，除非强转

    //! A.DImp2 di2 = a.getD();

   

    // 因为a.getD()返回的是私有类型A.D，所以不能访问

    //! a.getD().f();

   

    //这样也是可以的

    ((A.DImp2)a.getD()).f();

   

    //将a.getD()返回的私有类型A.D对象交给有权使用它的对象

    A a2 = new A();   

    a2.receiveD(a.getD());

  }

}
```



### 第十章内部类

内部类对象可以直接访问外围对象的所有成员（包括私有的），而不需要任何特殊条件，就像调用自己的方法与属性成员一样。但外围类不能直接访问内部类中的方法，除非使用内部类的实例来访问（也能访问私有的）。

内部类自动拥有对其外围类所有成员的访问权。这是如何做到的呢？当某个外围类的对象创建了一个内部类对象时，此内部对象会有一个指向外围类对象的引用，然后在你访问此外围类的成员时，就是用那个引用来选择外围类的成员，编译器会帮你处理所有的细节。注，这只限于非静态的内部类。

构建内部类对象时，需要一个指向其外围类对象的引用，如果编译器访问不到这个引用就会报错。

静态的内部类也叫嵌套类。

匿名内部类与普通的内部类继承相比是有些受限的，虽然匿名内部类既可以继承类，也可以实现接口，但是不能两者兼备，而且如果实现接口，也只能实现一个接口。

非静态的内部类中不能定义static成员，但final staitc成员是可以的。因为一个成员类实例必然与一个外部类实例关联，这个static定义完全可以移到其外部类中去。

内部（类中或接口中）接口一定static的。

接口中的内部类一定是public与static，但不一定是final（与数据成员不同），因为省略final时可以被继承

非静态的内部类里不能定义接口，因为内部接口（嵌套接口）默认为static，是无法改变的，所以内部类里的接口只能定义在静态的内部类里面。

方法与块里定义的内部类只能是非静态的，不能加static，所以局部内部类里只能定义非静态的成员。局部内部类也能直接访问外部内所有成员。

静态的内部类可以定义非静态与静态的东西。

不能在静态内部类中直接(实例化外部类再访问是可以的)访问外部类中的非静态成员与方法。而非静态内部类是可以访问外部类的静态成员。

在方法与作用域内都可定义内部类。如果一个内部类在if条件语句中定义，则不管条件是否成立都会编译成类，但这个类的使用域只限于该if语句中，if外面不能访问。

设计模式总是将变化的事物与保持不变的事物分离开，比如模板方法模式中，模板方法是保持不变的事物，而可覆盖的方法就是变化的事物。

内部类不能被重写：父类中的内部类不能被子类中的同名内部类重写。

为什么加上final后的局部变量就可以在内部类中使用了？因为加上final后，编译器是这样处理内部类的：如果这个外部局部变量是常量，则在内部类代码中直接用这个常量；如果是类的实例，则编译器将产生一个内部类的构造参数，将这个final变量传到内部类里，这样即使外部局部变量无效了，还可以使用，所以调用的实际是自己的属性而不是外部类方法的参数或局部变量。这样理解就很容易得出为什么要用final了，因为两者从外表看起来是同一个东西，实际上却不是这样，如果内部类改掉了这些参数的值也不可能影响到原参数，然而这样却失去了参数的一致性，因为从编程人员的角度来看他们是同一个东西，如果编程人员在程序设计的时候在内部类中改掉参数的值，但是外部调用的时候又发现值其实没有被改掉，这就让人非常的难以理解和接受，为了避免这种尴尬的问题存在，所以编译器设计人员把内部类能够使用的参数设定为必须是final来规避这种莫名其妙错误的存在。

一个类对外提供一个公共接口的实现（接口与实现完全分离，而且隐藏了实现）是内部类的典型应用，以JDK Collection类库为例，每种Collection的实现类必须提供一个与其对应的Iterator实现，以便客户端能以统一的方式（Iterator接口）遍历任一Collection实例。每种Collection类的Iterator实现就被定义为该Collection类的私有的内部类。

内部类作用：

1、内部类方法可以访问该类所在的作用域中的数据，包括私有数据。

2、内部类可以对同一个包中的其他类隐藏起来。

3、当想要定义一个回调函数且不想编写大量代码时，使用匿名内部类比较便捷。

4、实现多继承。



\>>>内部类机制<<<

如果有如下内部类：

```java
public class Outer {

   private boolean b;

   private int i;

 

   //内部类

   private class Inner {

      private boolean b;

      Inner(boolean b) {

         this.b = b;

      }

      public void print() {

         System.out.println(Outer.this.b & this.b);

      }

   }

 

   public void f(final String str) {

      class Inner {//局部内部类

         public void print() {

            System.out.println(Outer.this.i);

         }

      }

   }

}
```

 

我们来使用Reflection反射外部及内部类，看看内部类是怎样访问外部类成员的：

D:\work\Test\bin>java Reflection Outer

```java
class Outer

{

    //构造器

    public Outer();

    //字段

    private boolean b;

    private int i;

    //方法

    static boolean access$0(Outer);// 内部类Outer$Inner通过该方法访问外部类b成员，这样就可以访问一个私有成员了

    static int access$1(Outer); // 局部类Outer$1Inner通过该方法访问外部类i成员

    public void f(java.lang.String);

}

 

class Outer$Inner

{

    //构造器

    Outer$Inner(Outer, boolean);// 在编译期会自动传入外部类实例

    //字段

    private boolean b;

    final Outer this$0;//指向外部类实例

    //方法

    public void print();

}

 

class Outer$1Inner

{

    //构造器

    Outer$1Inner(Outer, java.lang.String);//第二个参数是引用的final类型的局部变量，也是通过构造器传入的

    //字段

    final Outer this$0;

    private final java.lang.String val$sr;//存储引用的局部final类型变量

    //方法

    public void print();

}
```

非静态内部类创建方式：

```java
OuterClassName.InnerClassName inner = new OuterClassName().new InnerClassName();
```

静态内部类创建方式：

```java
OuterClassName.InnerClassName inner = new OuterClassName.InnerClassName();
```

继承内部类语法规则：

```java
class WithInner {

  class Inner {}

}

 

public class InheritInner extends WithInner.Inner {

  //! InheritInner() {} // 不能编译

  /*

   * 这里的super指InheritInner类的父类WithInner.Inner

   * 的默认构造函数，而不是WithInner的父类构造函数，这

   * 种特殊的语法只在继承一个非静态内部类时才用到，表示

   * 继承非静态内部类时，外围对象一定要存在，并且只能在

   * 第一行调用，而且一定要调用一下。为什么不能直接使用

   * super()或不直接写出呢？最主要原因就是每个非静态的内部类

   * 都会与特有的一个外围类实例对应，这个外围类实例是运行时传到内部类里去的，所以在内部类里可以直接使用那个对象（比如Outer.this），但这里是在外部内外，使用时还是需要存在外围类实例对象，所以这里就显示的通过构造器传递进来，并且在外围对象上显示的调用一下内部类的构造器，这样就确保了在继承至一个类部类的情况下，外围对象一类会存在这样一个约束。

   */

  InheritInner(WithInner wi) {

    wi.super();

  }

  public static void main(String[] args) {

    WithInner wi = new WithInner();

    InheritInner ii = new InheritInner(wi);

  }

}
```

静态内部类里可以使用this（而不像静态方法或块里是不能使用this的），此时的this指向静态内部类，而不是外部类，下面为LinkedList类里的内部类，代表一个节点的实现：

```java
private static class Entry {

   Object element;

   Entry next;

   Entry previous;

 

       Entry(Object element, Entry next, Entry previous) {

       this.element = element;

       this.next = next;

       this.previous = previous;

   }

}
```

在非静态的内部类里访问外围类相同属性成员时，需在this前加上外围类型（采用Outer.this.XX 来访问，其中Outer为外围类的类型），一般在访问自己成员时，可以省略this前自身的类型，但默认应该是有的：

```java
public class Outer {

   private int i = 1;

   public void f(){

      System.out.println("f Outer.this.i=" + Outer.this.i);//1

   }

  

   private /static/class Inner {

      private int i = 2;

      public void p() {

         System.out.println("p this.i=" + this.i);//2

         System.out.println("p Inner.this.i=" + Inner.this.i);//2

         //!!注，如果是静态的内部类时，下面语句不能编译通过，因为静态的内部类没与外部类实例关联

         System.out.println("p Outer.this.i=" + Outer.this.i);//1

      }

   }

 

   public static void main(String[] args) {

      Outer outer = new Outer();

      outer.f();

      Outer.Inner inner = outer.new Inner();

      inner.p();

   }

}
```

匿名内部类（方法或块中的内部类一样）使用外部类作用域内的局部变量需定义成final，但这个变量需在匿名内部类中直接使用，如果是作为匿名构造器的参数时不需要：

```java
public class Wrapping {

  private int i;

  public Wrapping() {}

  public Wrapping(int x) { i = x; }

  public int value() { return i; }

}

 

public class Parcel {

    // 可以直接内部类或匿名的内部类访问，不需要定义成final

    private int ii = 1;

   /*

    * 这里的参数 x 不需要定义成final，因为它只是

    * 作为构建匿名对象时传递的一个参数，而没有直接

    * 在匿名内部类中使用

    */

   public Wrapping wrapping1(int x) {

              // 调用匿名内部类的基类的带参构造函数

      return new Wrapping(x) { // 传递参数

         public int value() {

            //调用外部内的域成员时可直接调用，但加this时就需在

            //this前加外部类名，因为该内部类没有定义ii

            return super.value() * 47 * Parcel.this.ii;

         }

      };

   }

 

   /*

    * 注，这里的参数 x 一定要定义成final，因为

    * 它被匿名内部类直接使用了

    */

   public Wrapping wrapping2( final int x ) {

      final int y = 1;

      return new Wrapping() {

         //不管是在定义时还是方法中使用都需要定义成final

         private int i=y;

         public int value() {

            return i * x * 47;

         }

      };

   }

   public static void main(String[] args) {

      Wrapping w = new Parcel().wrapping1(10);

      w = new Parcel().wrapping2(10);

   }

}
```

匿名类中不可能有命名的构造器，因为它根本没有名字。但通过块初始化，就能够达到为匿名内部类创建一个构造器的效果，当然它受到了限制——你不能重载块方法，这不像普通内部类的构造函数：

```java
abstract class Base {

  public Base(int i) {

   System.out.println("Base constructor, i = " + i);

  }

  public abstract void f();

} 

 

public class AnonymousConstructor {

  public static Base getBase(int i) {

    return new Base(i) {

      {   //初始化块

         System.out.println("Inside instance initializer");

      }

      public void f() {

         System.out.println("In anonymous f()");

      }

    };

  }

  public static void main(String[] args) {

    Base base = getBase(47);

    base.f();

  }

} /* Output:

Base constructor, i = 47

Inside instance initializer

In anonymous f()

*///:~

```

 

实现接口的匿名内部类：

```java
public class Outer {

   public Comparable getComp() {

      return new Comparable() {// Comparable为比较器接口

                     //实现接口

         public int compareTo(Object o) {

            return 0;

         }

      };

   }
```

但要注意，匿名内部类实现一个接口时，构造时不能带参数:

```java
interface InnerI {}

public InnerI getII() {

    // 匿名内部内实现一个接口时，构造器不能带参数，

    // 因为接口根本就没有构造器，更没有带参的构造器

    // !!return new InnerI(int i) {};

}

}

 
```

 

\>>>java实现多重继承<<<

java不支持多继承。即没有extends Class1，Class2的语句形式。这里的多继承是指继承类的属性和行为，并且是编译时就决定的静态行为。

广义的继承是指除了组合之外的的第二种代码复用方法，只要满足“像一个”或者“像是一个”、“里面有一个”条件都可以看做继承。

 

java的非静态内部类可以使用外部类的所有成员方法和变量。这给继承多个类的同名成员并共享带来可能。同时非匿名内部类可以继承一个父类和实现多个接口，因此外部类想要多继承的类可以分别由内部类继承，并进行Override或者直接复用。然后外部类通过创建内部类的对象来使用该内部对象的方法和成员，从而达到复用的目的，这样外部内就具有多个父类的所有特征。

这里的多继承可以说是外部类继承自一个内部类对象，而不是类，内部类 is in a 外部类，外部内的所有行为都是通过内部类对象动态获得的。

下面是采用组合多个内部类的方式模拟多继承的实例（基于对象层面，即组合多个内部类对象）：

```java
//手机

abstract class Mobile {

   public abstract void call();

}

// MP3播放器

abstract class Mp3Palyer {

   public abstract void play();

}

// 智能手机

class SmartPhone {

   private Mobile mb = new SmartMobile();

   private Mp3Palyer mp3 = new PhoneMp3();

 

   public Mobile getMobile() {

      return mb;

   }

   public Mp3Palyer getMp3() {

      return mp3;

   }

   public class SmartMobile extends Mobile {

      @Override // 不同的智能机有的call方式

      public void call() {

         System.out.println("Call phone!");

      }

   }

   public class PhoneMp3 extends Mp3Palyer {

      @Override // 不同的智能机有的play方式

      public void play() {

         System.out.println("Play music!");

      }

   }

}

public class MutiImpTest1 {

   static void call(Mobile m) {

      m.call();

   }

   static void play(Mp3Palyer p) {

      p.play();

   }

   public static void main(String[] args) {

      SmartPhone sp = new SmartPhone();

      call(sp.getMobile());// 智能手机具有手机功能

      play(sp.getMp3());// 又具有Mp3的功能

   }

}
```

下面采用继承与匿名类的方式模拟（一个是类层面的，一个是对象层面，即外部类首先继承一个类，然后通过引用内部类对象来继承另外一个类）：

```java
//手机

abstract class Mobile {

   public abstract void call();

}

// MP3播放器

abstract class Mp3Palyer {

   public abstract void play();

}

// 智能手机，继承自Mobile

class SmartMobile extends Mobile {

   @Override // 不同的智能机有的call方式

   public void call() {

      System.out.println("Call phone!");

   }

   // 智能手机也有播放音乐的功能

   public Mp3Palyer getMp3() {

      return new Mp3Palyer() {

         @Override // 不同的智能机有的play方式

         public void play() {

            System.out.println("Play music!");

         }

      };

   }

}

public class MutiImpTest2 {

   static void call(Mobile m) {

      m.call();

   }

   static void play(Mp3Palyer p) {

      p.play();

   }

   public static void main(String[] args) {

      // 现在智能手机即是手机类型，又具有Mp3的功能

      SmartMobile sp = new SmartMobile();

      call(sp);// 智能手机即是手机类型

      play(sp.getMp3());// 又具有Mp3的功能

   }

}

 
```

上面的多继承是站在外部类的角度来看的，即它们是通过外部类引用内部类来达到多态与复用的目的。反过来，内部类继承了一个类，同时拥有了外部类的所有成员方法和属性，我们是否可以认为内部类集成了两个类呢？——一个是类层面的，一个是对象层面的（因为非静态内部类使用前一定有外部类的对象来创建它，它持有外部类某个对象的引用）。如果外部类还继承了其他类呢？内部类还是可以访问其他类的方法与属性。现在从内部类的角度来模拟多继承：

```java
//智能机抽象类

abstract class SmartMobile {

   public abstract void call();

 

   public void play() {

      System.out.println("Play music!");

   }

}

 

// 手机

class Mobile {

   /*

    * 该方法是私有的，与SmartMobile类中方法同名，所以

    * Mobile不能直接继承自SmartMobile，因为重写时不能

    * 缩小访问权限，所以只能使用一个内部类来重写。

    */

   private void call() {

      System.out.println("Call phone!");

   }

 

   public SmartMobile getSmartMobileImp() {

      // 智能机的实现，好比多继承（继承外部类与SmartMobile）

      return new SmartMobile() {

         // 调用“继承”自外部类的相应方法来实现call

         public void call() {

            // 回调外部类真真的实现

            Mobile.this.call();

         }

      };

   }

}

 

public class MutiImpTest3 {

   static void call(SmartMobile m) {

      m.call();

   }

   static void play(SmartMobile p) {

      p.play();

   }

   public static void main(String[] args) {

      // 智能机即是手机也是Mp3播放器

      SmartMobile sp = new Mobile().getSmartMobileImp();

      call(sp);// 智能机即是手机

      play(sp);// 也是是Mp3播放器

   }

}
```

另外，从上面程序可看出内部类的另一作用：如果你想继承一个类或实现一个接口，但是这个接口或类中的一个方法和你构想的这个类中的一个方法的名称，参数相同，但访问权限缩小了，所以你不能直接继承与实现它，你应该怎么办？这时候，你可以建一个内部类继承这个类或实现这个接口（当然你可以修改访问权限是可以的）。由于内部类对外部类的所有内容都是可访问的，内部类可以通过调用外部类的这个方法来重写那个类或接口。上面的Mobile类中的call方法就是这种情况，所以你不能直接让Mobile去继承SmartMobile，固只能采用内部类来达到重写的目的。

 

\>>>>闭包与回调<<<<

动态语言的闭包是一个永恒的话题。闭包在编码过程的方便和快捷使得动态语言的拥护者对它津津乐道，而静态语言特别是Java语言的扇子们会拿出匿名内部类来说Java语言也有类似的功能。

 

JavaScript 中闭包的产生是由于 JavaScript 中允许内部 function，也就

是在一个 function 内部声明的 function 。内部 function 可以访问外部 function 中的局部变量、传入的参数和其它内部 function 。当内部 function 可以在包含它的外部 function 之外被引用时，就形成了一个闭包。这个时候，即便外部 function 已经执行完成，该内部 function仍然可以被执行，并且其中所用到的外部 function 的局部变量、传入的参数等仍然保留外部 function 执行结束

时的值。下面是一个例子：

```javascript
function Outer(){

var i=0;

function Inner(){

alert(++i);

}

return Inner;

}

var inner = Outer();

inner();
```

因为函数Outer外的变量inner引用了函数Outer内的函数Inner，就是说：当函数Outer的内部函数Inner被函数Outer外的一个变量inner引用的时候，就创建了一个闭包。

闭包有什么作用：简而言之，闭包的作用就是在Outer执行完并返回后，闭包使得Javascript的垃圾回收机制GC不会收回Outer所占用的资源，因为Outer的内部函数Inner的执行需要依赖Outer中的变量。

闭包是一个可调用的对象，它记录了一些信息，这些信息来自于创建它的作用域。通过这个定义，可以看出Java中内部类是面向对象的闭包，因为它不仅包含创建内部类的作用域的信息，还自动拥有一个指向此外围类对象的引用，在此作用域内，内部类有权操作所有的成员，包括private成员。

C++有指针函数，可以实现回调。通过回调，对象能够携带一些信息，这些信息允许它在稍后的某个时刻调用初始的对象。Java中没有指针，回调是通过匿名类来实现的。

 

以下是TIJ中的闭包与回调实例：

```java
//具有自增能力的接口，也是回调接口

interface Incrementable {

  void increment();

}

 

// 回调框架

class Caller {

  // 回调接口对象引用

  private Incrementable callbackReference;

  // 动态的传进回调接口实现

  Caller(Incrementable cbh) { callbackReference = cbh; }

  void go() {

      callbackReference.increment();//回调

  }

}

 

//自增接口简单实现:

class Callee1 implements Incrementable {

  privateinti = 0;

  publicvoid increment() {

    i++;

    System.out.println(i);

  }

} 
```

另一个具有类似功能的自增具体类，但功能不是Incrementable所要现实的功能

```java
class MyIncrement {

  //与类Callee1中的increment方法具有相同的签名

  publicvoid** increment() { System.out.println("Other operation"); }

  staticvoid f(MyIncrement mi) { mi.increment(); }

}

 

// 如果你想实现Incrementable接口而功能又不是MyIncrement的功能实现，

// 即我们又想要MyIncrement中的increment功能实现，同时又要想实现Incrementable接口，

// 此时我们只能使用一个类内部类来实现Incrementable接口

class Callee2 extends MyIncrement {

  privateinti = 0;

  // 重写了父类的功能

  publicvoid increment() {

    super.increment();//同时又保留了父类的功能

    // 以下是Callee2具体类自己的实现

    i++;

    System.out.println(i);

  }

  // 内部类，与外部类形成一个闭包，因为内部类有一个指向外围类的引用，且在内部类作

  // 用域内可以调用外围类对象一切成员

  privateclass Closure implements Incrementable {//实现Incrementable接口

    publicvoid increment() {

      // 指定调用外部类的increment方法，否则会发生递归调用

      Callee2.this.increment();//闭包，可以访问其作用域以外的外围类对象信息

    }

  }

  // 提供回调对象引用

  Incrementable getCallbackReference() {

    returnnew Closure();

  }

} 
```

 

```java
// 调用

public class Callbacks {

  public static void main(String[] args) {

    Callee1 c1 = new Callee1();//简单实现者

    Callee2 c2 = new Callee2();//MyIncrement子类，但同时实现了Incrementable功能

    MyIncrement.f(c2);

    Caller caller1 = new Caller(c1);//动态传进回调对象

    Caller caller2 = new Caller(c2.getCallbackReference());//动态传进回调对象

    caller1.go();//开始执行回调

    caller1.go();

    caller2.go();

    caller2.go();

  }

} /* Output:

Other operation

1

1

2

Other operation

2

Other operation

3

*/
```

以下是孙妹妹在《JAVA面向对象编程》写的回调，基本上是模仿上面那个例子，但觉得她对回调理解的有问题，因为根本不像上面有回调框架类Caller存在，唉，国人写的书啊，这里那能看的出是回调？我咋就没有悟出来呢？我看到的只是一种闭包。请看她所列举的例子：

在以下Adjustable接口和Base类中都定义了adjust()方法，这两个方法的参数签名相同，但是有着不同的功能。

```java
interface Adjustable{//个人认为这是回调接口

   //功能：调节温度

   public void adjust(int temperature);

}

class Base{

   private int speed;

   //功能：调节速度

   public void adjst(int spped){

      this.speed = speed;

   }

}
```

如果有一个Sub类同时具有调节温度和调节速度的功能，那么Sub类需要继承Base类，并且实现Adjustable接口，但是以下代码并不能满足这一需求：

```java
class Sub extends Base implements Adjustable{

   private int temperature;

   public void adjust(int temperature) {

      this.temperature = temperature;

   }

}
```

以上Sub类实现了Adjustable接口中的adjust()方法，并且把Base类中的adjust()方法覆盖了，这意味着Sub类仅仅有调节温度的功能，但失去了调节速度的功能。或以使用内部类来解决这一问题：

```java
class Sub extends Base {

   private int temperature;

 

   // 为了防止覆盖父类的adjst，所以取了不同的名字

   private void adjustTemperature(int temperature) {

      this.temperature = temperature;

   }

 

   // 实现回调接口

   private class Closure implements Adjustable {

      public void adjust(int temperature) {

         // 这里是回调？

         adjustTemperature(temperature);

      }

   }

 

   // 提供回调对象引用

   public Adjustable getCallBackReference() {

      return new Closure();

   }

 

   public static void main(String[] args) {

      Sub sub = new Sub();

      // 具有调节速度的功能

      sub.adjst(1);

  

      //又具有调节温度的功能，这里就是回调？

      sub.getCallBackReference().adjust(2);

   }

}
```

上面使Sub类既不覆盖Base类的adujst()方法，又实现了Adjustable接口的adjust()方法。

客户类先调用sub实例的getCallBackReference()方法，获得内部类的Closure实例，然后再调用Closure实例的adjust()方法，该方法又调用Sub实例的adjustTemperature()方法。这种调用过程称为回调（这就叫回调？不理解）。

回调实质上是指一个类尽管实际上实现了某种功能，但是没有直接提供相应的接口，客户类可以通过这个类的内部类的接口来获得这种功能。而这个内部类本身并没有提供真正的实现，仅仅调用外部类的实现。可见，回调充分发挥了内部类所具有的访问外部类的实现细节的优势。

 

以下回调来源于网络：

回调的基本原理跟好莱坞原则一样，Don't call me,I'll call you.

编程上来说，一般使用一个库或类时，是你主动调用人家的API，这个叫Call，有的时候这样不能满足需要，需要你注册（注入）你自己的程序（比如一个对象)，然后让人家在合适的时候来调用你，这叫Callback。设计模式中的Observer就是例子：所有的观察者都需要向自己关心的主题Observable注册，然后主题在适当时机（主题类对象的属性发生变化时）通知所有订阅它的观察者并更新，其中观察者都实现了一个统一的Observer接口中的Update方法。

/**下面应用中ICallBack接口与Printer类好比是别人提供的API，*/

public interface ICallBack {//回调接口

​	public void print();

}

public class Printer {

​    ICallBack ic;

 

​    void setCallBack(ICallBack ic) {

​        this.ic = ic;  

​    }

  /*供外界调用，即自己提供一个接口ICallBack，由外界PrintHandler去实现，再在适当时机回头调用外界所提供的实现print方法。

我没有实现接口，但是我取得了一个实现接口的对象，而这个对象是外界类调用我的方法setCallBack()时所赋给我的，因此我可以在业务需要的地方来调用外界所提供的实现print方法

*/

void execute() {

   //固定算法do some thing…

​        ic.print(); //抽取变化的部分，由外界去实现

//固定算法 do some thing…

​    }

}

 

/**下面是外界应用*/

public class PrintHandler {

​    public static void main(String[] args) {

​        Printer printer = new Printer();

/*注意下面的这项代码片段，它给printer对象传递了一个实现ICallBack接口的匿名类，这样Printer类的对象就取得了一个实现回调接口的类，因此Printer可以在任何时候调用接口中的方法*/

​        printer.setCallBack(new ICallBack() {

​         /* print 方法在PrintHandler类中实现，但不在PrintHandler 类对象中调用，而是在Printer类对象中调用，这就是回调*/

​            public void print() {

​                System.out.println("This is a callback");

​            }

​        });

​         //  这句话可以设置成当满足某条件时再执行  

​        printer.execute();  

​    }  

}

 

 

观察者模式也符合这一种理解：请参见XXXXXXXXXXXXX

 

 

 

 

 

内部类生成的class文件名规则：

```java
public class A {//A.class

   class B {//A$B.class

      class C {}//ABBC.class

   }

 

   {

      class B {}//A$1B.class

   }

 

       B f() {

      class D {}//A$1D.class

      return new B() {};//A$1.class

   }

 

       B g() {

      class E {//A$1E.class

                     B h() {

            return new B() {};//A1E1E1.class

         }

      }

      return new B() {};//A$2.class

   }

   static class F{}//A$F.class

 

   public static void main(String[] args) {

      A a = new A();

      System.out.println(a.f().getClass().getName());

      System.out.println(a.g().getClass().getName());

   }

}
```

\>>>再论工厂模式<<<

接口与工厂模式

接口典型的应用就是多继承，而工厂方法设计模式能生成实现同一接口的对象，这与我们直接在使用的地方new某个实现对象是不同的，我们通过工厂对象上调用是业务实现对象创建方法，而该工厂对象将生成接口的某个业务实现的对象，理念上，我们的代码将完全与接口分离，这使得我们可以透明地将某个实现替换为另一个实现。下面的实例展示了工厂方法的结构：

```java
// 业务接口

interface Service {

  void method1();

  void method2();

}

// 业务工厂

interface ServiceFactory {

  Service getService();

}

// 业务1的实现

class Implementation1 implements Service {

  Implementation1() {} // 包访问

  public void method1() {System.out.println("Implementation1 method1");}

  public void method2() {System.out.println("Implementation1 method2");}

} 

// 业务1的工厂

class Implementation1Factory implements ServiceFactory {

  public Service getService() {

    return new Implementation1();

  }

}

//业务2的实现

class Implementation2 implements Service {

  Implementation2() {} // 包访问

  public void method1() {System.out.println("Implementation2 method1");}

  public void method2() {System.out.println("Implementation2 method2");}

}

//业务2的工厂

class Implementation2Factory implements ServiceFactory {

  public Service getService() {

    return new Implementation2();

  }

} 

//工厂应用

public class Factories {

  public static void serviceConsumer(ServiceFactory fact) {

   // 通过不同的工厂获得不同的业务实现

    Service s = fact.getService();

    s.method1();

    s.method2();

  }

  public static void main(String[] args) {

    serviceConsumer(new Implementation1Factory());

    // 业务实现可以透明的改变:

    serviceConsumer(new Implementation2Factory());

  }

} /* Output:

Implementation1 method1

Implementation1 method2

Implementation2 method1

Implementation2 method2

*/
```

如果不使用工厂方法，你的代码就必须在使用处指定将要创建的Service的确切类型，以便调用合适的构造器。为什么需要这种额外的间接性呢？一个常见的原因就是想要创建框架，业务实例的生产过程对客户是透明的，而且还可以在工厂方法返回实例前对业务对象进行额外处理。

 

内部类与工厂模式

看看再使用匿名内部类来修改上面的程序：

```java
interface Service {

  void method1();

  void method2();

}

interface ServiceFactory {

  Service getService();

} 

class Implementation1 implements Service {

  private Implementation1() {}// 私有的，只能通过工厂方法来返回

  public void method1() {System.out.println("Implementation1 method1");}

  public void method2() {System.out.println("Implementation1 method2");}

  public static ServiceFactory factory = // 静态成员，只需一个工厂即可

    new ServiceFactory() {// 使用匿名类来实现工厂接口

      public Service getService() {

       // 但需多个业务对象，每次调用工厂方法都会获得一个业务实现对象

        return new Implementation1();

      }

    };

} 

class Implementation2 implements Service {

  private Implementation2() {}// 私有的，只能通过工厂方法来返回

  public void method1() {System.out.println("Implementation2 method1");}

  public void method2() {System.out.println("Implementation2 method2");}

  public static ServiceFactory factory =

    new ServiceFactory() {

      public Service getService() {

        return new Implementation2();

      }

    };

} 

public class Factories {

  public static void serviceConsumer(ServiceFactory fact) {

    Service s = fact.getService();

    s.method1();

    s.method2();

  }

  public static void main(String[] args) {

    serviceConsumer(Implementation1.factory);

    serviceConsumer(Implementation2.factory);

  }

}
```

修改后Implementation1与Implementation2的构造器都可以是private的，并且没有任何必要去创建具有具体名字的工厂类，另外，你经常只需要一个工厂对象即可，因此在本例中它被创建为Service实现的一个static域，这样更具有实际意义。修改后的程序是多么的完美!

### 第十一章持有对象

请参考十七章

 

### 第十二章通过异常处理错误

使用异常的好处：一是使用程序更加的健壮。二是它往往能够降低错误处理的复杂度。如果不使用异常，那么就必须检查特定的错误，并在程序中的许多地方去处理它，而如果使用异常，那就不必在方法调用处进行检查，因为异常机制保证能够捕获这个错误，并且，只需在一个地方处理错误，即所谓的异常处理程序块中。这种方式不仅节省代码，而且把“描述在正常执行过程中做什么事”的代码和“出了问题怎么办”的代码相分离，总之，与以前的错误处理方法相比，异常机制使用代码的阅读、编写和调试工作更加高效。

当抛出异常后，有几件事会随之发生。首先，同Java中其他对象的创建一样，将使用new在堆上创建异常对象。然后，当前的执行路径（它不能继续下去了）被终止，并且从当前环境中弹出对异常对象的引用。此时，异常处理机制接管程序，并开始寻找一个恰当的地方来继续执行程序。这个恰当的地方就是异常处理程序，它的任务是将程序从错误状态中恢复，以使用程序能要么换一种方式运行，要么继续运行下去。

通常把错误信息输出到e.printStackTrace(System.out)要比直接e.printStackTrace()要好，因为System.out也许会重定向，而e.printStackTrace()默认则是将信息输出到操作系统标准错误流，所以一般我们使用e.printStackTrace(System.out)打印异常信息。如果把结果送到System.err，它就会把会随System.out一起被重定向，这样更容易被用户注意。

使用程序包的客户端程序员可能仅仅只是查看一下抛出的异常类型，其他的就不管了（大多数Java库里的异常都是这么用的），所以对异常所添加的其他功能也许根本用不上，名称代表发生的问题，并且异常的名称应该可以望文知意。

可以声明方法将抛出异常，实际上却不抛出，这样做的好处是，为异常先占个位子，以后就可以抛出这种异常而不用修改已有的代码。在定义抽象基类和接口时这种能力很重要，这样派生类或接口实现就能够抛出这些预先声明的异常。

Exception的方法

```java
public class ExceptionMethods {

   public static void main(String[] args) {

      try {

         throw new Exception("My Exception");

      } catch (Exception e) {

         System.out.println("getMessage():" + e.getMessage());

         System.out.println("getLocalizedMessage():" + e.getLocalizedMessage());

         System.out.println("toString():" + e);

         System.out.println("printStackTrace():");

         e.printStackTrace(System.out);

      }

   }

}

/*

getMessage():My Exception

getLocalizedMessage():My Exception

toString():java.lang.Exception: My Exception

printStackTrace():

java.lang.Exception: My Exception

   at excep.ExceptionMethods.main(ExceptionMethods.java:10)

*/
```

 

使用JDK日志器记录日志

```java
import java.io.PrintWriter;

import java.io.StringWriter;

import java.util.logging.Logger;

 

class MyException extends Exception {

   String errKey;//错误键

 

   public MyException setErrKey(String errKey) {

      this.errKey = errKey;

      return this;

   }

 

   //重写父类方法，输出详细信息

   public String getMessage() {

      return "errKey = " + errKey + " " + super.getMessage();

   }

}

 

public class LoggingExceptions {

   // JDK日志记录器

   private static Logger logger = Logger.getLogger("LoggingExceptions");

 

   static void logException(Exception e) {// 记录异常日志

      // 字符串缓存

      StringWriter trace = new StringWriter();

      // 将日志输出到缓存

      e.printStackTrace(new PrintWriter(trace));

      // 输出异常日志

      logger.severe(trace.toString());

   }

 

   public static void main(String[] args) {

      try {

         throw new MyException().setErrKey("100");

      } catch (MyException e) {

         logException(e);

      }

   }

}

/*

2010-2-9 11:17:38 excep.LoggingExceptions logException

严重: excep.MyException: errKey = 100 null

   at excep.LoggingExceptions.main(LoggingExceptions.java:37)

*/

```

printStackTrace()方法所提供的信息可以通过getStackTrace()方法来直接访问，这个方法返回一个由栈轨迹中的元素所构成的数组，其中每一个元素都表示栈中的一桢。元素0是栈顶元素，并且是调用序列中的最后一个方法调用。数组中的最后一个元素即栈底是调用序列中的第一个方法调用。

```java
public class WhoCalled {

   static void f() {

      // Generate an exception to fill in the stack trace

      try {

         throw new Exception();

      } catch (Exception e) {

         for (StackTraceElement ste : e.getStackTrace())

            System.out.println("getClassName: " + ste.getClassName()

                   + " getFileName: " + ste.getFileName()

                   + " getLineNumber: " + ste.getLineNumber()

                   + " getMethodName: " + ste.getMethodName()

                   + " isNativeMethod: " + ste.isNativeMethod());

      }

   }

 

   static void g() {

      f();

   }

 

   static void h() {

      g();

   }

 

   public static void main(String[] args) {

      f();

      System.out.println("--------------------------------");

      g();

      System.out.println("--------------------------------");

      h();

   }

}

/*

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 8 getMethodName: f isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 28 getMethodName: main isNativeMethod: false

--------------------------------

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 8 getMethodName: f isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 20 getMethodName: g isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 30 getMethodName: main isNativeMethod: false

--------------------------------

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 8 getMethodName: f isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 20 getMethodName: g isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 24 getMethodName: h isNativeMethod: false

getClassName: WhoCalled getFileName: WhoCalled.java getLineNumber: 32 getMethodName: main isNativeMethod: false

*/

 
```

 

如果只是把当前异常对象重新抛出，那么printStackTrace()方法显示的将是原来异常抛出点的调用栈信息，而并非重新抛出点的信息。要想更新这个信息，可以调用fillInStackTrace()方法，这将返回一个Throwable对象，它是通过把当前调用栈信息填入原来那个异常对象而建立的：

```java
public class Rethrowing {

  public static void f() throws Exception {

    System.out.println("originating the exception in f()");

    throw new Exception("thrown from f()");

  }

  public static void g() throws Exception {

    try {

      f();

    } catch(Exception e) {

      System.out.println("Inside g(),e.printStackTrace()");

      e.printStackTrace(System.out);

      throw e;//再次抛出

    }

  }

  public static void h() throws Exception {

    try {

      f();

    } catch(Exception e) {

      System.out.println("Inside h(),e.printStackTrace()");

      e.printStackTrace(System.out);

      //重新抛出，这一行将成为异常的新发生行了，好比在这里重新包装new后抛出：throw new Exception();

      throw (Exception)e.fillInStackTrace();

    }

  }

  public static void main(String[] args) {

    try {

      g();

    } catch(Exception e) {

      System.out.println("main: printStackTrace()");

      e.printStackTrace(System.out);

    }

    try {

      h();

    } catch(Exception e) {

      System.out.println("main: printStackTrace()");

      e.printStackTrace(System.out);

    }

  }

} /* Output:

originating the exception in f()

Inside g(),e.printStackTrace()

java.lang.Exception: thrown from f()

   at Rethrowing.f(Rethrowing.java:4)

   at Rethrowing.g(Rethrowing.java:8)

   at Rethrowing.main(Rethrowing.java:27)

main: printStackTrace()

java.lang.Exception: thrown from f()

   at Rethrowing.f(Rethrowing.java:4)

   at Rethrowing.g(Rethrowing.java:8)

   at Rethrowing.main(Rethrowing.java:27)

originating the exception in f()

Inside h(),e.printStackTrace()

java.lang.Exception: thrown from f()

   at Rethrowing.f(Rethrowing.java:4)

   at Rethrowing.h(Rethrowing.java:17)

   at Rethrowing.main(Rethrowing.java:33)

main: printStackTrace()

java.lang.Exception: thrown from f()

   at Rethrowing.h(Rethrowing.java:22)

   at Rethrowing.main(Rethrowing.java:33)

*///:~

 
```

 

常常会想要在捕获一个异常后抛出另一个异常，并且希望把原始异常的信息保存下来，这个被称为异常链。在JDK1.4以前，程序员必须自己编写代码来保存原始中异常的信息。现在所有Throwable的子类在构造器中都可能接受一个cause对象作为参数（Throwable(Throwable cause)）。这个cause就用来表示原始异常，这样通过把原始异常传递给新的异常，使得即使在当前位置创建并了新的异常，也能通过这个异常链追踪到异常最初发生的位置。并可以使用initCause(Throwable cause)来重样设置异常根原因，但此方法至多可以调用一次，如果抛出的异常是通过 Throwable(Throwable) 或Throwable(String,Throwable) 创建的，则该异常对象的此方法甚至一次也不能调用。

```java
public class ExcpTest {

 

   public static void nullExc() {

      throw new NullPointerException();

   }

 

   public static void call1() throws Exception {

      try {

         nullExc();

      } catch (Exception e) {

         throw new Exception(e);

      }

 

   }

 

   public static void call2() throws Exception {

      try {

         call1();

      } catch (Exception e) {

         //该行运行时会抛异常，因为e已经设置过 cause 了

         throw (Exception) e.initCause(new ArithmeticException());

      }

   }

  

   public static void call3() throws Exception {

      try {

         nullExc();

      } catch (Exception e) {

         //该行运行没问题，因为还没有给 e 设置 cause

         throw (Exception) e.initCause(new ArithmeticException());

      }

 

   }

 

   public static void main(String[] args) {

      try {

         call1();

      } catch (Exception e) {

         e.printStackTrace();

      }

      try {

         call2();

      } catch (Exception e) {

         e.printStackTrace();

      }

      try {

         call3();

      } catch (Exception e) {

         e.printStackTrace();

      }

   }

}

/*

java.lang.Exception: java.lang.NullPointerException

at ExcpTest.call1(ExcpTest.java:11)

at ExcpTest.main(ExcpTest.java:37)

Caused by: java.lang.NullPointerException

at ExcpTest.nullExc(ExcpTest.java:4)

at ExcpTest.call1(ExcpTest.java:9)

... 1 more

java.lang.IllegalStateException: Can't overwrite cause

at java.lang.Throwable.initCause(Unknown Source)

at ExcpTest.call2(ExcpTest.java:21)

at ExcpTest.main(ExcpTest.java:42)

java.lang.NullPointerException

at ExcpTest.nullExc(ExcpTest.java:4)

at ExcpTest.call3(ExcpTest.java:27)

at ExcpTest.main(ExcpTest.java:47)

Caused by: java.lang.ArithmeticException

at ExcpTest.call3(ExcpTest.java:30)

... 1 more

*/

 

 
```

RuntimeException：属于运行时异常（即非捕获性异常），它包括继承自它的所有子类会自动被Java虚拟机抛出，所以不向在方法声明时抛出异常说明，所以下面的代码也将是多余的：

if(t == null){

   throw new NullPointerException();

}

如果不捕获运行时异常，则异常会穿过（抛出）所有的调用路径直达main()方法，而不会被捕获，并在程序退出前将自动调用异常的printStackTrace()方法。

 

受检查异常是程序可以处理的异常。如果抛出异常的方法本身不能处理它，那么方法调用者应该去处理它，从而使程序运行，不至于终止程序。

 

运行时异常表示无法让程序恢复运行的异常，导致这种异常的原因通常是由于执行了错误操作，一旦出现了错误操作，建议终止程序，因此Java编译器不检查这种异常。如果出现运行时异常，则表示你的程序代码本身的问题，而不是由程序外界引起的，比如读文件时文件不存在，这不是由程序本身引起的，所以文件不存在的抛出的是检查行异常。

 

RuntimeException代表的是编程错误：

1、无法预料的错误，比如从你控制范围之外传递进来的null引用。

2、作为程序员，应该在代码中进行检查错误。（比如对于ArrayIndexOutOfBoundsException，就得注意一下数组的大小了。）在一个地方发生的异常，常常会在另一个地方导致错误。

 

运行时异常是应该尽量避免的（也是完全可能避免的，既然是可以避免的，所以运行时异常不需捕获），在程序调试阶段，遇到这种异常时，正确的做法是程序的设计和实现方式，修改程序中的错误，从而避免这种异常。捕获运行时异常并且使程序恢复运行并不是明智的办法，这主要有两方面的原因：

1、这种异常一旦发生，损失严重。

2、即使程序恢复运行，也可能会导致程序的业务逻辑错乱，甚至导致更严重的异常，或都得到错误的运行结果。

 

Error类及其子类表示程序本身无法修复的错误，它和运行时异常的相同之处是：Java编译器都会检查它们，当程序运行时出现它们时都会终止程序。

 

如果有必要，一般将try放循环里，这样就避免了当Java中的异常出现时我们回到异常抛出地点再次执行的问题，这样可以建立了一个“程序继续执行之前必须要达到”的条件。

 

避免过于庞大的try代码块，因为try代码块越庞大，出现异常的地方就越多，要发生异常的原因就越困难。

 

不要使用catch(Exception ex)子句来捕获所有异常，理由如下：

1、对不同的异常通常有不现的处理方式，不同的错误使用同样的处理方式是不现实的。

2、会捕获本应该抛出的运行异常，掩盖程序中的错误。

什么情况下才用到finally？当要把除内存之外的资源恢复到它们的初始状态时，就要用到finally子名，这种需要清理的资源包括：已经打开的文件或网络连接，在屏幕上画的图形，甚至可以是外部对象某个状态的恢复。

Finllay块的异常丢失

```java
public class ExceptionSilencer {

   public static void f() {

      try {

         throw new RuntimeException();

      } finally {

         // 从这里返回时，异常将丢失，不會再向外拋出了

         return;

      }

   }

   public static void main(String[] args) {

      f();//得不到打印的信息

   }

}

 
```

如果一个类继承了某个类同时又实现了某个接口，他们有同样的接口方法，但都抛出了不同的捕获性异常，则该子类实现与重写该方法时，则方法声明处不能抛出任何捕获性异常了。

如果调用的父类构造器抛出捕获性异常，则子类相应的构造器也只能抛出，不能在构造器里进行捕获。

构造器抛出异常时正确的清理方式：

比如在构造器中打开了一个文件，清理动作只有在对象使用完毕并且用户调用了特殊的清理方法之后才能得以清理，而不能直接在构造器里的finally块上关闭，因为finally块是不管是否有异常都会关闭，而构造器执行成功能外界需要这个文件流。但如果在文件成功打开后才抛出异常，则需要关闭文件，并向外界抛出异常信息：

```java
import java.io.BufferedReader;

import java.io.FileNotFoundException;

import java.io.FileReader;

import java.io.IOException;

 

public class InputFile {

   private BufferedReader in;

 

   // 抛出异常的构造器

   public InputFile(String fname) throws Exception {

      try {

         // 在构造器中打开一个文件流

         in = new BufferedReader(new FileReader(fname));

      } catch (FileNotFoundException e) {

         System.out.println("Could not open " + fname);

         // 如果是文件没有找到，则不需要关闭流，只需重新抛出

         throw e;

      } catch (Exception e) {

         // 如果是其他异常，则需要关闭流，因為文件已打开

         try {

            in.close();

         } catch (IOException e2) {

            System.out.println("in.close() unsuccessful");

         }

         throw e; // 再重新抛出

      } finally {

         // 这里不能关闭流!!!

      }

   }

 

   public String getLine() {

      String s;

      try {

         s = in.readLine();

      } catch (IOException e) {

         throw new RuntimeException("readLine() failed");

      }

      return s;

   }

 

   // 在文件成功打开后，j由外界使用完后调用

   public void dispose() {

      try {

         in.close();

         System.out.println("dispose() successful");

      } catch (IOException e2) {

         throw new RuntimeException("in.close() failed");

      }

   }

}

 

class Cleanup {

   public static void main(String[] args) {

      // 该try是对构造器异常的捕获，如果出现了异常则不需关闭，

      // 因为构造器内部已处理

      try {

         InputFile in = new InputFile("InputFile.java");

         // 如果运行到这里说明文件已正常打开，所以后面需关闭

         try {

            String s;

            int i = 1;

            while ((s = in.getLine()) != null)

                ; // 读文件...

         } catch (Exception e) {

            System.out.println("Caught Exception in main");

            e.printStackTrace(System.out);

         } finally {

            // 不管读取是否正常，用完后一定要关闭

            in.dispose();

         }

      } catch (Exception e) {

         System.out.println("InputFile construction failed");

      }

   }

}

 
```

 

异常处理的一个重要的原则是“只有在你知道如何处理的情况下才捕获异常”。实际上，异常处理的一个重要目标就是把错误处理的代码同错误发生的地点相分离。这使你能在一段代码中专注于要完成的事情，至于如何处理错误，则放在另一段代码中。这样以来，主干代码就不会与错误处理逻辑混在一起，也更容易理解和维护。

 

“被检查的异常”可能使问题变得复杂，因为它们强制你在可能还没有准备好处理错误的时候被迫加上cacth子名，即使我们不知道如何处理的情况下，这就导致了异常的隐藏：

try{

   //… throw …

}catch(Exception e){//什么都不做}

 

 

把“被检查的异常”转换为“不检查的异常”：当在一个普通方法里调用别的方法时，要考虑到“我不知道该怎样处理这个异常，但是也不能把它‘吞’了，或者只打印一些无用的消息”。JDK1.4的异常链提供了一种新的思路来解决这个问题，可以直接把“被检查的异常”包装进RuntimeException里面：

try{

   //… throw 检查异常…

}catch(IDontKnowWhatToDoWithThisCheckedException e){

   Throw new RuntimeException(e);

}

如果想把“被检查的异常”这种功能“屏蔽”掉的话，上面是一个好的办法。不用“吞”掉异常，也不必把它放到方法的异常声明里面，而异常链还能保证你不会丢失任何原始异常的信息。你还可以在“知道如何处理的地方”来处理它，也可以其他上层catch里通过 throw e.getCause(); 再次抛出原始的“被检查的异常”:

```java
public class Test {

   static void f() {

      try {

         throw new Exception();

      } catch (Exception e) {

         //将检测性异常转换成非检测性异常后继续抛出

         throw new RuntimeException(e);

      }

   }

 

   static void g() {

      //不用捕获，因为检测异常转换了运行异常

      f();

   }

 

   static void h() throws Exception {

      try {

         g();

      } catch (Exception e) {

         e.printStackTrace();

         System.out.println("-----");

         //可以将检测异常继续做为检测异常抛出

         throw new Exception(e.getCause());

      }

   }

 

   public static void main(String[] args) {

      try {

         h();

      } catch (Exception e) {

         e.printStackTrace();

      }

   }

}

/*

java.lang.RuntimeException: java.lang.Exception

   at Test.f(Test.java:7)

   at Test.g(Test.java:13)

   at Test.h(Test.java:18)

   at Test.main(Test.java:29)

Caused by: java.lang.Exception

   at Test.f(Test.java:4)

   ... 3 more

-----

java.lang.Exception: java.lang.Exception

   at Test.h(Test.java:23)

   at Test.main(Test.java:29)

Caused by: java.lang.Exception

   at Test.f(Test.java:4)

   at Test.g(Test.java:13)

   at Test.h(Test.java:18)

   ... 1 more

*/

 
```



### 第十三章字符串

字符串是不可变的：是final类故不能继承它；也不能通过指向它的引用来修改它的内容。

StringBuilder是Java SE5引用的，在这之前用的是StringBuffer。后者是线程安全的，因此开销也会大些，所以在Java SE5/6中，字符串操作应该还会更快一点。

在JDK1.5中：String s = "a" + "b" + "c"; 在编译时，编译器会自动引入java.lang.StringBuilder类，并使用StringBuilder.append方法来连接。虽然我们在源码中并没有使用StringBuilder类，但是编译器自动地使用了它，因为它更高效。

虽然在JDK1.5或以上版本中使用“+”连接字符串时为避免产生过多的字符串对象，编译器会自加使用StringBuilder类来优化，但是如果连接操作在循环里，编译器会为每次循环都创建一个StringBuilder对象，所以在循环里一般我们不要直接使用“+”连接字符串，而是自己在循环外显示的创建一个StringBuilder对象，用它来构造最终的结果。但是在使用StringBuilder类时也要注意，不要这样使用：StringBuilder.append(a + ":" + c); ，如果这样，那编译器就会掉入陷井，从而为你另外创建一个StringBuilder对象处理括号内的字符串连接操作。

如果重写了父类的toString方法（一般是Object的toString），当需要打印对象的内存地址时，应该调用super.toString()方法，而不是直接打印this，否则会发生StackOverflowError异常。

Java SE5的PrintStream与PrintWriter对象都引入了format()方法，那我们就要可以使用System.out.format()格式化输出了。format()方法模仿自C语言的printf()，如果你比较怀旧的话，也可以使用printf()，它还是调用format()来实现的，只不过换了个名而已：

```java
public class SimpleFormat {

  public static void main(String[] args) {

    int x = 5;

    double y = 5.332542;

    // 以前我们是这样打印的:

    System.out.println("Row 1: [" + x + " " + y + "]");

    // 现在我们是这样打印的:

    System.out.format("Row 1: [%d %f]\n", x, y);

    // 或者是

    System.out.printf("Row 1: [%d %f]\n", x, y);

  }

} /* Output:

Row 1: [5 5.332542]

Row 1: [5 5.332542]

Row 1: [5 5.332542]

*///:~

 
```

 

在Java中，所有新的格式化功能（PrintStream与PrintWriter的format方法以及String的静态方法format）都是由java.util.Formatter类来处理的，创建时需要指定输出到哪。

Formatter的对齐格式化输出抽象语法：

%[argument_index$][flags][width][.precision]conversion

可选的 *argument_index*是一个十进制整数，用于表明参数在参数列表中的位置。第一个参数由 "`1$`" 引用，第二个参数由 "`2$`" 引用，依此类推。

Width用来控制一个域的最小尺寸，如果输出的参数值不够宽，则添加空格来确保一个域至少达到某个宽度，在默认的情况下，数据是右对齐的，但我们可以使用“-” flags标志来改变对齐方向。

与width相对的是precision，它用来指明输出的最大尺寸。Width可以应用于各种数据类型时其行为方式都是一样，但precision不一样，并不是所有类型都能应用precision，而且，应用于不同类型的数据转换时，precision的意义也不同：将precision应用String时，它表示打印String时输出字符的最大个数；而在将precision应用于浮点数时，它表示小数部分要显示出来的位数（默认是6位小数位），如果小数位过多则舍入，太少则在尾部补零。由于整数没有小数部分，固不能应用于整形数据类型，否则抛异常：

```java
import java.util.Formatter;

 

public class Receipt {

   private double total = 0;

 

   // 这里输出到控制台，我们也可以格式化后输出到文件、StringBuilder、CharBuffer

   private Formatter f = new Formatter(System.out);

 

   public void printTitle() {

      // %-15s表示输出一个最小宽度为15的且左对齐的字符,2$表示左起输出参数位置

      f.format("%2−15s−15s5s %3$10s\n", "Qty", "Item", "Price");

      f.format("%-15s %5s %10s\n", "----", "---", "-----");

   }

 

   public void print(String name, int qty, double price) {

      // %-15.15s在%-15s基础上最多能输出15个字符

      f.format("%-15.15s %5d %10.2f\n", name, qty, price);

      total += price;

   }

 

   public void printTotal() {

      // %10.2f表示输出一个最小宽度为10，右对齐小数点后两位的浮点数

      f.format("%-15s %5s %10.2f = %s\n", "Tax", "", total * 0.06, total + " * 0.06");

      f.format("%-15s %5s %10s\n", "", "", "-----");

      f.format("%-15s %5s %10.2f = %s\n", "Total", "", total * 1.06, total + " * 1.06");

   }

 

   public static void main(String[] args) {

      Receipt receipt = new Receipt();

      receipt.printTitle();

      receipt.print("Jack's Magic Beans", 4, 4.254);

      receipt.print("Princess Peas", 3, 5.1);

      receipt.print("Three Bears Porridge", 1, 14.285);

      receipt.printTotal();

   }

}

/*

* Output:

Item              Qty      Price

------

Jack's Magic Be     4       4.25

Princess Peas       3       5.10

Three Bears Por     1      14.29

Tax                         1.42 = 23.639 * 0.06

                           -----

Total                      25.06 = 23.639 * 1.06

*/

 
```

Fomatter常用类型转换字符

%d ：整数型（十进制）

%c ：Unicode字符

%b ：Boolean值

%s ：String

%f ：浮点数（十进制）

%e ：浮点数（科学计数）

%x ：整数（十六进制）

%h ：散列码（十六进制）

%% ：字符“%”

 

“%b”对于boolean基本类型及Boolean，其转换结果为对应的true或false，但是，对其他类型的参数，只要该参数不为null，那转换的结果就永远都是true，即使是数字0，转换结果依然为true，而这不像其他语言（如C）为false。上面列举的是常用的格式字符，其他可以在JDK文档中的Formatter类部分找到。

String.format()：String.format()是一个Static方法，当我们只需要使用format()方法一次时很方便，其实在String.format()内部，它也是创建一个Formatter对象，格式化后传进的字符串后返回新的字符串。下面是一个十六进制工具：

```java
import java.io.BufferedInputStream;

import java.io.File;

import java.io.FileInputStream;

import java.io.IOException;

 

public class Hex {

   /**

    * 以十六进制格式输出数据

    * @param counts 每行多少个

    * @param data 要格式化的数据

    * @return 格式化后的数据

    */

   public static String format(int counts, byte[] data) {

      StringBuilder result = new StringBuilder();

      int n = 0;

      int rows = 0;

      for (byte b : data) {

         if (n % counts == 0) {

            rows++;

            result.append(String.format("%05d: ", rows));

         }

         result.append(String.format("%02X ", b));

         n++;

         if (n % counts == 0) {

            result.deleteCharAt(result.length() - 1);

            result.append("\n");

         }

      }

      result.append("\n");

      return result.toString();

   }

 

   /**

    * 读取二进制文件

    * @param bFile

    * @return

    * @throws IOException

    */

   public static byte[] read(File bFile) throws IOException {

      BufferedInputStream bf = new BufferedInputStream(new FileInputStream(

            bFile));

      try {

         byte[] data = new byte[bf.available()];

         bf.read(data);

         return data;

      } finally {

         bf.close();

      }

   }

 

   public static void main(String[] args) throws IOException {

      System.out.println(format(16, read(new File("src/Hex.class")

            .getAbsoluteFile())));

   }

}

/*

* Output:

00001: CA FE BA BE 00 00 00 31 00 7F 07 00 02 01 00 03

00002: 48 65 78 07 00 04 01 00 10 6A 61 76 61 2F 6C 61

00003: 6E 67 2F 4F 62 6A 65 63 74 01 00 06 3C 69 6E 69

...

*/

 
```

 

判断一个字符串是否匹配指定的模式，最简单的是使用String对象的matches方法，需传递正则式参数，实质上是调用Pattern.matches(regex, string)来实现的。

String的split()方法也使用到了正则式，该方法的重载版本允许你限制字符串分割次数**split**(String regex,int limit) ：limit 参数控制模式应用的次数，因此影响结果数组的长度，默认就是0。如果该限制 *n*大于 0，则模式将被最多应用 *n* - 1 次，数组的长度将不会大于 *n*，而且数组的最后项将包含超出最后匹配的定界符的所有输入。如果 *n*为非正，则模式将被应用尽可能多的次数，而且数组可以是任意长度。如果 *n*为零，则模式将被应用尽可能多的次数，数组可有任何长度，并且结尾空字符串将被丢弃。例如，字符串 "boo:and:foo" 使用这些参数可生成下列结果：

Regex Limit 结果

: 2 { "boo", "and:foo" }

: 5 { "boo", "and", "foo" }

: -2 { "boo", "and", "foo" }

o 5 { "b", "", ":and:f", "", "" }

o -2 { "b", "", ":and:f", "", "" }

o 0 { "b", "", ":and:f" }

CharBuffer、String、StringBuffer、StringBuilder都实现了CharSequence接口，大多数的正则表达式操作都接受CharSequence类型的参数。

如果使用功能强大的正则表达式对象，我们使用静态的Patter.compile()方法来编译正则表达式即可，它会生成一个Patter对象。接下来将想要检测的字符串传Patter对象的matcher()方法，会生成一个Matcher对象，该对象有很多的功能可用。

另外，Patter类还提供了静态的方法：**static** **boolean** matches(String regex, CharSequence input)

Matcher对象的groupCount方法返回该匹配器的模式中的分组数目，但第0组不包括在内。

String的split()方法实质上是调用Pattern对象的split方法：Pattern.compile(regex).split(string, limit)来实现的。String的replace方法则是通过调用Matcher对象的replace方法来实现的。

Matcher对象的appendReplacement(StringBuffer sbuf,String replacement)执行渐进式的替换，而不是像replaceFirst()和replaceAll()那样只替换第一个匹配或全部匹配。这是一个非常重要的方法，它允许你调用其他方法来生成或处理replacemanet（replaceFirst()和replaceAll()则只能使用一个固定的字符串），使你能够以编程的方式将目标分割成组，从而具备列强大的替换功能，appendTail(StringBuffer sbuf)，在执行了一次或多次appendReplacement之后，调用此方法可以将输入字符串余下的部分复制到sbuf中。

Matcher对象的reset()、reset(CharSequence input)将Matcher对象重新设置到当前字符序列的起始位置，可以重用Pattern与Matcher对象。

Java SE5新增了Scanner类，它可以大大减轻扫描输入的工作。可以通过useDelimiter(String pattern)设置next操作的定界符。还可通过hasNext(String pattern)判断是否还存在指定正则式的串，如果有，则可通过String next(String pattern)来读取。除此之后，它还有很多的读取各种不同基本类型的数据的nextXX方法。

 

### 第十四章类型信息

在运行时识别对象和类的信息有两种方式：一种是“传统的”RTTI（如“(Circle)”），它假设我们在编译时已经知道了所有的类型，但易引起ClassCastException异常，不过我们可以通过 instanceof 先检测具体类型；另一种是“反射”机制（使用类型的Class对象），它允许我们在运行时查询Class对象的信息。

Class对象相关方法：

getName()：回此 `Class`对象所表示的实体（类、接口、数组类、基本类型或 void）名称。如果此类对象表示的是非数组类型的引用类型，则返回该类的二进制名称，包括包名；如果此类对象表示一个基本类型或 void，则返回的名字是一个与该基本类型或void 所对应的 Java 语言关键字相同的串；如果此类对象表示一个数组类，该数组嵌套深度的一个或多个 '[' 字符加元素类型名。元素类型名的编码如下：

元素类型编码

boolean  Z

byte  B

char  C

类或接口  Lclassname;

double  D

float  F

int  I

long  J

short  S

如：

String.class.getName()：java.lang.String

byte.class.getName()：byte

(new Object[3]).getClass().getName()：[Ljava.lang.Object;

(new int[3][4][5][6][7][8][9]).getClass().getName()：[[[[[[[I

getSimpleName()：产生不包括包名的类名。

getInterfaces()：返回所有实现的接口的Class对象数组。

getSuperclass()：返回直接基类。如果此 Class 表示 Object 类、接口、基本类型或 void，则返回 null。如果此对象表示一个数组类，则返回表示该 Object 类的 Class 对象。

newInstance()：创建此`Class`对象所表示的类的一个新实例。如同用一个带有一个空参数列表的 `new`表达式实例化该类。使用该方法创建对象实例时，必须要有默认构造函数。

获取某个类的Class对象：

l  Class.forName(String className)。

l  通过对象的getClass()方法。

l  直接通过类的静态属性class，如Test.class，这样做不仅更简单，而且更安全，因为它在编译时就会受到检查（因此不需要置于try语句块中）。并且不需要调用forName()方法，所以更高效。

另外，对于基本数据类型的包装器类，还有一个标准字段TYPE，该字段是一个引用，指向对象的基本数据类型的Class对象。但还是建议使用“.class”的形式，以保持与普通类型的一致性。

 

\>>>Class.forName、Object.class、classLoader.loadClass异同<<<

Class.forName与“.class”区别在于：前者会初始化Class对象（如静态数据成员的初始化与静态块的执行），而后者不会。“.class”只是去加载类，不会链接（即不会给静态域分配空间）；ClassLoader类的loadClass()方法只是加载一个类，并不会分配内存空间，更不会导致类的初始化:

```java
public class ClassLoadTest {

   // 测试时请调整虚拟机的堆大小：-Xms32M -Xmx1024M

   public static void main(String[] args) {

      // 编译时直接将2替换，不加导致类的加载与初始化

      System.out.println("Bean.y=" + Bean.y);

      sleep("->调用类的静态字面常量不会导致类的加载");

      /*

       * 不会初始化静态块与静态成员，只是加载到内存，也没进行

       * 链接操作（静态成员分配空间），更没有初始化类（静态成

       * 员初始化与静态块的执行）

       */

      Class cl = Bean.class;

      sleep("->Bean.class只会加载类，不会分配内存空间与初始化类");

      ClassLoader cld = ClassLoader.getSystemClassLoader();

      try {

         // 也只是加载到内存，没有进行空间的分配与初始化类

         cld.loadClass("Bean");

      } catch (ClassNotFoundException e) {

         e.printStackTrace();

      }

      sleep("->ClassLoader的loadClass()方法也只会加载类，不会" +

            "分配内存空间与初始化类");

      try {

         // 加载与初始化类，并可看到内存猛增

         cl = Class.forName("Bean");

      } catch (ClassNotFoundException e) {

         e.printStackTrace();

      }

      sleep("->Class.forName()会加载类，且分配内存空间与初始化类");

   }

 

   private static void sleep(String msg) {

      System.out.println(msg);

      try {

         Thread.sleep(5000);

      } catch (InterruptedException e) {

         e.printStackTrace();

      }

   }

}

 

class Bean {

   static {

      System.out.println("static block");

   }

   public static final int y = 2;

   public static final int i = f(1);

   public static int j = f(2);

   // 测试 A.class是否进行了链接操作

   public static long[] l = h(21474836);

   static {

      System.out.println("i=" + i);

      System.out.println("j=" + j);

   }

 

   static int f(int i) {

      System.out.println("static f(" + i + ")");

      return i;

   }

 

   static long[] h(int len) {

     long[] l = new long[len];

      for (int i = 0; i < len; i++) {

         l[i] = i;

      }

      return l;

   }

}
```

使用一个类前需做的三个准备步骤：

1、加载：这是由类加载器执行的。该步骤将查找字节码文件并读取到内存，并从这些字节码中创建一个Class对象。

2、链接：验证被加载类的正确性（验证）、为类的静态变量分配内存空间，并将其初始化为默认值（准备）、把类中的符号引用转换为直接引用（解析），如将方法的调用解析成直接方法在方法区的内存地址调用。

3、类的初始化（初始化Class对象）：初始化静态成员和执行静态初始化块。

 

调用类一个 static final（编译期常量，注一定要是在定义时就初始化了，否则还是会初始化静态块与成员的）成员时，类的Calss对象不会执行初始化操作，也就是说“编译期常量”在类Class对象还没有初始化就可以引用了。

 

如果一个static域不是final的，那么在对它访问时，总是要求在它被读取前，要先对类进行链接（为这个域分配存储空间）和初始化（初始化该存储空间）操作。

 

**类的**初始化**时机发生在类首次主动使用时**，类主动使用发生在以下时机：

1、  创建类的实例。创建实例途经：使用new（构造器隐式也是静态的）、反射、克隆、反序列化

2、  调用类的静态方法。

3、  访问某个类或接口的静态变量（注意，不能是静态的字面常量域，静态的字面常量在编译时就确定，使用之前不会先加载类，更不会初始化类）。

4、  调用Class.forName()加载类。

5、  初始化一个类的子类，会导致父类初始化。

6、  Java虚拟机启动时被标明为启动类的类，例如: “java Test”命令，Test类就是启动类，Java虚拟机会先初始化它。

 

Java程序对类的使用方式可分为两种：主动使用与被动使用。

所有的Java虚拟机实现必须在每个类或接口被Java程序“首次主动使用”时才初始化他们。

 

类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在堆区创建一个java.lang.Class对象，用来封装类在方法区内的数据结构。

 

一个奇怪的初始化问题：

```java
public class  Singleton {

    /*

     * 这里如果为非静态时，会因递归构造而堆栈溢出：

     * private Singleton s = new Singleton();

     * 因为构造器在调用前需要先初始化所有的非静态成员，

     * 而静态成员则不会在此时再被初始化，因为静态成员

     * 是在类加载时就已初始了。

     *

     * 成员field1未初始化，field2需初始化。

     *

     * 初始化的动作是依照编写的顺序执行的，由于s先于

     * field2的初始化，所以在构造函数调用完后，虽然

     * field2为1了，但又会被紧拉着的field2初始化给覆

     * 盖。如果将s的构造放在field2之后又会正常

     */

    private static  Singleton s = new Singleton();

    public static int field1;// 未初始化

    public static int field2 = eval();// 初始化

 

    private Singleton() {

       field1++;

       field2++;

       System.out.println("Sigleton field2=" + field2);

    }

 

    private static int eval() {

       System.out.println("eval()");

       return 0;

    }

 

    public static  Singleton getInStance() {

       return s ;

    }

 

    public static void main(String[] args) {

       Singleton.getInStance();

       System.out.println("Main field1=" + Singleton.field1);// 1

       System.out.println("Main field2=" + Singleton.field2);// 0

    }

}
```

 

当Java虚拟机初始化一个类时，要求它的所有父类都像自己那样被初始，但是这条规则并不适用接口：

1、  在初始化一个类时，并不会先初始化它所实现的接口。

2、  在初始化一个接口时，并不会先初始化它的父接口。

因此，一个父接口并不会因为它的子接口或都实现类的初始化而初始化。只有当程序首次使用真真属于他们的特定接口的静态变量时，才会导致该接口的初始化，或者换句话来说就是只有当程序访问的静态变量或静态方法的确在当前类或接口中定义时，才会引起类的加载与初始化：

```java
class Rd {

       public static int getNumber(String msg) {

              System.out.println(msg);

              return new Random().nextInt(100);

       }

}

interface I1 {

       //注，getNumber不能抛出检测异常，因为不能捕获

       public final int j = Rd.getNumber("init j");

      

}

interface I2 extends I1 {

       public final int i = Rd.getNumber("init i");

       public final int y = Rd.getNumber("init y");

       public final int x = 1*2;//编译时就已计算出结果

}

class Imp implements I2 {

       public static void main(String[] args) {

              //创建子类实现类时不会去初始化父接口

              new Imp();

              System.out.println("------");

              //由于x为静态的字面常量，所以不会引起类加载

              System.out.println(I2.x);

              System.out.println("------");

              //子接口的初始化不会引起父接口的初始化

              System.out.println(I2.i);

              System.out.println("------");

              //只有在使用到接口中的静态域时再初始化接口

              System.out.println(I2.j);

       }

}
```

只有当程序访问的静态变量或静态方法的确在当前类或接口中定义时，才可看做是对类或接口的主动使用：

```java
public class P {

       static int i = prt();

       static int prt(){

              System.out.println("init i");

              return 1;

       }

}

class S extends P {

       static {

              System.out.println("init S");

       }

}

class T {

       public static void main(String[] args) {

              //不会初始化S类

              System.out.println(S.i);

       }

}
```

对于final类型的静态变量，如果在编译时就能计算取变量的取值，那么这种变量被看做编译地时常量，即不需要等到运行时确定（如：private static final int i = 2，注：private static final int i = 2*2 也属于编译时常量，因为表达式是由常量组成，编译后会使用常量 4 替换表达式）；但是，对于那些在编译时无法计算出的final类型的静态变量，则调用他们时需要先加载类并初始化类才能使用（如：private static final int i = new Random().nextInt()）。另外，如果类A中引用了B类的static final int i = 2;成员，则在编译A类时，就会把2直接编译到A中，因此在运行时可以不需要B.class文件都可运行。所以引用一个类的静态字面常量的成员时，该类不会被加载。

 

\>>>类加载器<<<

类表示被执行的代码，而数据则表示与代码相关联的状态信息。状态信息可以改变，而代码则一般不会变更。一个类的代码通常都保存在一个.class为后缀的文件中。

 

在Java中，一个类的固定标识为其完整的具限类名称。该具限类名由该类的包名加上类名组成。但是在JVM中，唯一标识一个类的方式为：其具限类名与装载该类的装载器ClassLoader实例的组合。因此，如果一个包名为Pg，类名为C1的类，被类装载器KlassLoader的实例kl1装载，该类的实例C1（即C1.class）在JVM中的索引值将为（C1, Pg, kl1）。这意味着如果两个类装载器实例，装载了两个完全相同的类，则这两个类在虚拟机中的表示（C1, Pg, kl1）和（C1, Pg, kl2）将完全不一样，并且他们的对象实例也将完全不同，相互之间再也不能类型兼容了。

除了引导类装载器以外，所有的类装载器均有一个父类装载器。此外，所有的类装载器均为类型java.lang.ClassLoader的子类。

ClassLoader的loadClass(String name)实现如下：

```java
public Class<?> loadClass(String name) throws ClassNotFoundException {

    return loadClass(name, false);

}

protected**synchronized** Class<?> loadClass(String name, boolean resolve)

       throws ClassNotFoundException {

    // First, check if the class has already been loaded

    Class c = findLoadedClass(name);

    if (c == null) {

       try {

           if (parent != null) {

              //如果还有父加载，则递归由父加载器去加载

              c = parent.loadClass(name, false);

           } else {

              /*

               * 通过上面递归的找父加载器，最终会因为父加器为null，

               * 即直到父加载器为根（Bootstrap）类加载器时，才结束

               * 递归，并开始从根类加载器开始加载指定的类。又由于根

               * 类加载只能加载核心库，所以肯定会失败，失败后会调用

               * 异常块中的 findClass(name)方法，这个方法默认是抛出

               * ClassNotFoundException异常，这会导致返回上层调用

               * 的 c 为null，即加载类失败，这样会再次由上层调用者，

               * 即子加载器去加载，如果子加载器还是失败，则再让子子

               * 加载去加载，直接自己实现的类加载器去加载，此时就需

               * 要我们去实现 ClassLoader 的 findClass

               */

              c = findBootstrapClass0(name);

           }

       } catch (ClassNotFoundException e) {

           // If still not found, then invoke findClass in order

           // to find the class.

           c = findClass(name);

       }

    }

    //外界调用loadClass(String name)方法时，实际上是执

    //行loadClass(name, false)，所以这里不会执行

    if (resolve) {

       resolveClass(c);

    }

    return c;

}

 
```

 

当要加载一个类时，调用的是ClassLoader的loadClass方法，loadClass方法先查找这个类是否己被加载，如果没有加载则委托其父级类装载器去加载这个类，如果父级的类装载器无法装载这个类，子级类装载器才调用自己内部的findClass方法去进行真正的加载。父级类装载器调用loadClass方法去装载一个类时，它也是先查找其父级类装载器，这徉一直追溯到没有父级的类装载器时（例如ExtClassLoader），则使用Java虚拟机内嵌的Bootstrap类装载器进行装载，当Bootstrap无法加载当前所要加载的类时，然后才一级级回退到子孙类装载器去进行真正的加载。当回退到最初的类装载器时，如果它自己也不能完成类的装载，那就应报告ClassNotFoundException异常。

 

一个类装载器只能创建某个类的一份字节码数据，即只能为某个类创建一个与之对应的Class实例对象，而不能为同样的一个类创建多个Class实例对象。在一个Java虚拟机中可以存在多个类装载器，每个类装载器都拥有自己的名称空间，对于同一个类每个类装载器都可以创建出它的一个Class实例对象，即每个类装载器都可以分别创建出某个类的一份字节码数据。两个类装载器分别创建的同一个类的字节码数据属于两个完全不同的对象，相互之间没有任何关联，例如，在某个类中定义了一个静态成员变量，它在不同的类装载器之间是不可以实现教据共享的。采用委托模式给类的加载管理带来了明显的好处，当父级的类装载器加载了某个类，那么子级的类装载器就不要再去加载这个类，这样就可以避免一个Java虚拟机中的多个类装载器为同一个类创建多份字节码数据的情况。

 

如果在类A中使用出new关键字创建类B, Java虚拟机将使用加载类A的类装载器来加载类B。如果在一个类中调用Class.forName方法来动态加载另外一个类，可以通过传递给Class.forName(String name, boolean initialize, ClassLoader loader)方法的一个参数来指定另外那个类的类装载器，如果没有指定该参数，则使用加载当前类的类装载器来加载。

 

每个运行中的线程都有一个关联的上下文装载器，可以使用Thread.setContextCIassLoader()方法设置线程的上下文类装载器。每个线程默认的上下文类装载器是其父线程的上下文类装载器，而主线程的类装载器初始被设置为 ClassLoader.getSystemC1assLoader()方法返回的系统类装载器。当线程中运行的代码需要使用某个类时，它使用上下文类装载器来装载这个类，上下文类装载器首先会委托它的父级类装载器来装载这个类，如果父级的类装载器无法装载时，上下文类装载器才自己进行装载。

 

Java虚拟机自带了以下几种加载器：

1. 根（Bootstrap）类加载器：该加载器没有父加载器。它负责加载虚拟机核心类库，如java.lang.*等，这些核心的运行期Java类位于<JAVA_HOME>\jre\lib\rt.jar文件中。根类加载器从系统属性sun.boot.class.path所指定的目录中加载类库。根类加载器的实现依赖于底层操作系统，属于虚拟机的实现的一部分，它并没有继承java.lang.ClassLoader类。
2. 扩展（Extension）类加载器（实现为：sun.misc.Launcher$ExtClassLoader）：它的父加载器为根类加载器。它从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的<JAVA_HOME>\jre\lib\ext子目（扩展目录）下加载类库，如果把用户创建的JAR文件放在这个目录下，也会自动由扩展类加载器加载。扩展类加载器是纯Java类，是java.lang.ClassLoader类的子类。
3. 系统（System）类加载器（实现为：sun.misc.Launcher$AppClassLoader，由ClassLoader.getSystemClassLoader()来获取，并且内存中只有一个）：也称为应用类加载器，它的父加载器为扩展类加载器。它从环境变量classpath或者系统属性java.class.path所指定的目录中加载类，它是用户自定义的类加载器的默认父加载器。系统类加载器是纯Java类，是java.lang.ClassLoader类的子类。
4. 自定义类加载器：Java提供了抽象类java.lang.ClassLoader，所有用户自定义的类加载器应该继承ClassLoader类。

```java
public class ClassLoaderTest {

       public static void main(String[] args) {

              ClassLoader cl, cl1;

              // 获取系统类加载器

              cl = ClassLoader.getSystemClassLoader();

              System.out.println("系统类加载器：" + getClassName(cl));

 

              // 打印父加载器

              while (cl != null) {

                     cl1 = cl;

                     cl = cl.getParent();

                     System.out.println(getClassName(cl1) + " 的父加载器：" + getClassName(cl) + "，且由 "

                                   + cl1.getClass().getClassLoader()+ " 加载。");

              }

              // Object类的加载器

              System.out.println("Object类的加载器：" + Object.class.getClassLoader());

 

              // 用户定义应用类的加载器

              System.out.println("应用程序的类加载器："

                            + getClassName(ClassLoaderTest.class.getClassLoader()));

       }

 

       private static String getClassName(Object o) {

              return o == null ? "null" : o.getClass().getSimpleName();

       }

       /*

       系统类加载器：AppClassLoader

       AppClassLoader 的父加载器：ExtClassLoader，且由 null 加载。

       ExtClassLoader 的父加载器：null，且由 null 加载。

       Object类的加载器：null

       应用程序的类加载器：AppClassLoader

        */

}
```

从上面的打印可看出：

l  系统类加载器为AppClassLoader类的实例。

l  系统类加载的父加载器为扩展类加载器，即ExtClassLoader类的实例。

l  扩展类加载器的父加载器为根类加载。但是，VM并不会向Java程序提供根类加载器的引用，而是返回Null，这是为了VM的安全。

l  Object类是由根类加载器加载的。

l  用户自定义类是由系统类加载器加载的。

l  系统类加载器、扩展类加载器由根类加载器来加载。

 

当通过某类加载器加载类时，首先从自己的命名空间查找是否已经加载，如果已加载，则直接返回已加载的Class对象引用。如果没有加载，则请求父类去加载，父类再去请求父类的父类去加载，加载请求就这样一层层向上传，直到根加载器，如果根加载器加载不成功，则将请求往回传，直到有一个加载器能加载为止，再将加载的Class对象返回给最开始发起加载动作的类加载器。

 

加载器之间的父子关系实际上指的是加载器对象之间的关系，而不是类之间的继承关系。

 

可能通过ClassLoader的构造函数指定父加载器（ClassLoader(ClassLoader parent)），如果构造时没有指定，则使用系统类加载器作为父加载器，如果设置成null，则父加载器为根加载器。

 

父亲委托机制的优点是能够提高软件系统的安全性。因数在此机制下，用户自定的类加载器不可能加载应该由父加载器加载的可靠类，从而防止不可靠甚至恶意的代码代替由父加载器加载的可靠代码。如java.lang.String类总是由根类加载器加载，其他任何用户自定义加载器都不可能加载含有恶意代码的自定义的java.lang.String类，因为加载时先会去看上层父加载器是否加载，由于java.lang.String已被根类加载器加载过了，所以不会再加载我们自已定义的java.lang.String类。

 

命名空间：每个类加载器都有自己的命名空间，命名空间由该加载器及所有父加载器所加载的类组成。在同一个命名空间中，不会出现类的完整名字（包括类的包名）相同的两个类。但在不同的命名空间是可以的。

 

运行时包：由同一类加载器加载的属于相同包的类组成了运行时包。决定两个类是不是属于同一个运行时包，不仅要看它们的包名是否相同，还要看定义类加载器是否相同。只有属于同一运行包的类才能互相访问包可见（即默认访问级别）的类和类成员。这样的限制能避免用户自定义的类冒充类库的类，去访问核心类库的包可见成员。假设用户自定义了一个类java.lang.XXX，并由用户自定义的类加载器加载，由于java.lang.XX和核心类库java.lang.*由不同的加载器加载，它们属于不同的运行时包，所以java.lang.XX不能访问核心类库java.lang包中的包可见成员，所以即使我们把类所在包定义成与核心类库的包一样，如将自定义的类放在java.lang包中，但由于加载是我们自定义的类是由系统类加载器或是自己定义类加载加载的，所以运行时所在包是不一样的，所以即使我们冒充在同一包中，但还是不能访问那些具有包访问权限的类及成员。

 

若有一个类加载器能成功加载Sample类，那么这个类加载器被称为定义类加载器，所有能成功返回Class对象的引用的类加载（包括定义类加载器）都被称为初始类加载器。

 

\>>>创建用户自定义的类加载器<<<

要创建用户自己的类加载器，只需要扩展java.lang.ClassLoader类，然后覆盖它的findClass(String name)方法即可，该方法根据参数指定的类的名字，返回对应的Class对象的引用。

下面是自定义的MyClassLoader加载器：

```java
public class MyClassLoader extends ClassLoader {

       private String classLoaderName;// 自定义类加载器的名字

       private String loadPath;// 类加载的加载路径

 

       public MyClassLoader(String classLoaderName) {

              super();// 使用系统类加载器作为父加载器

              this.classLoaderName = classLoaderName;

       }

 

       public MyClassLoader(ClassLoader parentLoader, String classLoaderName) {

              super(parentLoader);// 指定parentLoader为父类加载器

              this.classLoaderName = classLoaderName;

       }

 

       public String toString() {

              return classLoaderName;

       }

 

       public void setPath(String path) {

              this.loadPath = path;

       }

 

       @Override

       protected Class findClass(String className) throws ClassNotFoundException {

              FileInputStream fis = null;

              byte[] data = null;

              ByteArrayOutputStream baos = null;

 

              try {

                     fis = new FileInputStream(new File(loadPath

                                   + className.replaceAll("\.", "\\") + ".class"));

                     baos = new ByteArrayOutputStream();

                     int tmpByte = 0;

                     while ((tmpByte = fis.read()) != -1) {

                            baos.write(tmpByte);

                     }

                     data = baos.toByteArray();

              } catch (IOException e) {

                     throw new ClassNotFoundException("class is not found:" + className,

                                   e);

              } finally {

                     try {

                            if(fis != null){

                                   fis.close();

                            }

                            if(fis != null){

                                   baos.close();

                            }

                           

                     } catch (Exception e) {

                            e.printStackTrace();

                     }

              }

              return defineClass(className, data, 0, data.length);

       }

 

       public static void main(String[] args) throws Exception {

              // loader1的父加载器为系统类加载器

              MyClassLoader loader1 = new MyClassLoader("loader1");

              loader1.setPath("d:/myapp/serverlib/");

              // loader2的父加载器为loader1

              MyClassLoader loader2 = new MyClassLoader(loader1, "loader2");

              loader2.setPath("d:/myapp/clientlib/");

              // loader3的父加载器根加载器

              MyClassLoader loader3 = new MyClassLoader(null, "loader3");

              loader3.setPath("d:/myapp/otherlib/");

              test(loader2);// 使用loader2测试

              System.out.println("-------------------");

              test(loader3);// 使用loader3测试

       }

 

       public static void test(ClassLoader loader) throws Exception {

              // 注，loadClass的参数为类的完整名，即包括包名

              Class objClass = loader.loadClass("pkg.Sample");

              Object obj = objClass.newInstance();

              System.out.println("obj=" + obj);

       }

}

 

package pkg;

public class Sample {

       public int v1 = 1;

       public Sample() {

              // 我们可以通过class对象的getClassLoader方法获取当

              // 前class对象类加载器

              System.out.println("Sample loaded by "

                            + this.getClass().getClassLoader());

              new Dog();// 引用Dog，会导致Dog类的加载动作

       }

}

class Dog {

       public Dog() {

              System.out.println("Dog loaded by " +

                            this.getClass().getClassLoader());

       }

}
```

 

开始编译：

D:\myapp\syslib>javac -d . MyClassLoader.java

D:\myapp\syslib>javac -d . pkg/Sample.java

编译后整个系统的目录结构如下：

D:\MYAPP

├─clientlib

├─otherlib

├─serverlib

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Sample.class

​            Dog.class

 

接下来通过改变Sample类和Dog类的存放路径，或者修改源程序，来演示类加载器的各种特性：

1)        构造以下目录结构：

D:\MYAPP

├─clientlib

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

│  └─pkg

│          Sample.class

│          Dog.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Sample.class

​            Dog.class

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by sun.misc.Launcher$AppClassLoader@82ba41

Dog loaded by sun.misc.Launcher$AppClassLoader@82ba41

obj=pkg.Sample@1a46e30

\-------------------

Sample loaded by loader3

Dog loaded by loader3

obj=pkg.Sample@addbf1

 

由于父类加载的委托机制，加载一个类时，会先从根加载器开始加载，如果根加载器加载不成功或拒绝加载，则由扩展器来加载，同样不成功或拒绝加载则由系统加载器来加载，如果还不成功则由自定义的类加载器来加载。loder2的父加载器结构为 loder2àloder1à AppClassLoaderàExtClassLoaderà Bootstrap，由于我们自己定义的类默认是由系统类加载器AppClassLoader来加载的，系统类加载器会加载环境变量classpath的类，而运行时设置的classpath 为./syslib，且在该路径下有Sample.class与Dog.class，所以loader2.loadClass("pkg.Sample")加载时使用系统类加载器从classpath路径中来加载Sample.class，又由于Sample.class的构造函数中引用Dog.class，所以先默认采用同样的类加载器来加载Dog.class，如果系统类加载器加载Dog.class失败时，将会怎么样，请继承往下看。

loader3.loadClass("pkg.Sample")就更简单了，因为loader3的父加载器结构为loder3à Bootstrap，又根加载器不能加载用户自定义类，所以只能由loder3从D:\myapp\otherlib路径中加载了。

从这个例子还可以看出，在loader1和loader3各自的命名空间中，都在Sample类和Dog类，也就是说，在VM中有两个Sample类Class对象和两个Dog类的Class对象。

 

2)        构造以下目录结构：

D:\MYAPP

├─clientlib

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

│  └─pkg

│          Sample.class

│          Dog.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader1

Dog loaded by loader1

obj=pkg.Sample@3e25a5

\-------------------

Sample loaded by loader3

Dog loaded by loader3

obj=pkg.Sample@42e816

 

由于系统类加载器不能加载Sample.class，所以由loader1来尝试，且加载成功。

 

3)        构造以下目录结构：

D:\MYAPP

├─clientlib

│  └─pkg

│          Sample.class

│          Dog.class

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader2

Dog loaded by loader2

obj=pkg.Sample@3e25a5

\-------------------

Sample loaded by loader3

Dog loaded by loader3

obj=pkg.Sample@42e816

 

4)        构造以下目录结构：

D:\MYAPP

├─clientlib

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

│  └─pkg

│          Sample.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Dog.class

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader1

Exception in thread "main" java.lang.IllegalAccessError: tried to access class pkg.Dog from class pkg.Sample

。。。

虽然Sample.class由loader1来加载，而Dog.class由系统类加载器来加载的，而系统类加载器又是loader1的父加载器，根据后面的规则：“子加载器的命名空间包含所有父加载器的命名空间。因此由子加载器加载的类能看见父加载器加载的类。”，似乎可以正常运行，但这只是说能看到这个类，并不代表你能够访问到这个类（如果是包访问权限的话，而这里的Dog类恰好就是包访问权限的，所以你不能访问这个类）及这个类里的包访问权限的成员及方法；再根据规则“由同一类加载器加载的属于相同包的类组成了运行时包，只有属于同一运行包的类才能互相访问包可见（即默认访问级别）的类和类成员”，由于Sample与Dog由不同的类加载器来加载的，他们不属于同一个运行时包，所以就出现了上面运行时错误。但如果将Dog类访问权限修改成public时，则可以访问，请继续往下看。

 

从上面我们要注意，不是只要两个类在同一个包中就可以相互访问包访问权限的类及类的成员，还要看他们是否是由同一加载器来加载的，即属于同一运行时包的类才真正属于同一包。

 

5)        构造以下目录结构（将Dog类单独写成一个类，并将class定义成public）：

D:\MYAPP

├─clientlib

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

│  └─pkg

│          Sample.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Dog.java

​            Dog.class

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader1

Dog loaded by sun.misc.Launcher$AppClassLoader@82ba41

obj=pkg.Sample@3e25a5

\-------------------

Sample loaded by loader3

Dog loaded by loader3

obj=pkg.Sample@42e816

由于子加载器加载的类（Sample）能看见父加载器加载的类（Dog），所以以上运行正常。

 

6)        构造以下目录结构：

D:\MYAPP

├─clientlib

├─otherlib

│  └─pkg

│          Sample.class

│          Dog.class

├─serverlib

│  └─pkg

│          Dog.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Dog.java

​            Sample.class

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by sun.misc.Launcher$AppClassLoader@82ba41

Exception in thread "main" java.lang.NoClassDefFoundError: pkg/Dog

。。。

由于父加载器加载的类（Sample）不能看见子加载器加载的类（Dog），所以运行错误。

 

7)        不同类加载器的命名空间存在以下关系：

l  同一个命名空间内的类是相互可见的。

l  子加载器的命名空间包含所有父加载器的命名空间。因此由子加载器加载的类能看见父加载器加载的类。例如系统类加载器加载的类能看见根类加载器加载的类。

l  由父加载器加载的类不能看见子加载器加载的类。

l  如果两个加载器之间没有直接或间接的父子关系，那么它们各自加载的类相互不可见。

所谓类A能看见类B，就是指在类A的程序代码中可以引用类B的名字，例如：

Class A{ B b = new B();}

下面把Sample.class和Dog.class仅仅拷贝到D:\myapp\serverlib目录下，然后把MyClassLoader类的main()方法修改为下面代码：

```java
MyClassLoader loader1 = new MyClassLoader("loader1");

loader1.setPath("d:/myapp/serverlib/");

Class objClass = loader1.loadClass("pkg.Sample");

Object obj = objClass.newInstance();

Sample sample = (Sample)obj;//抛出NoClassDefFoundError错误

System.out.println(sample.v1);
```

此时的目录结构如下：

D:\MYAPP

├─clientlib

├─otherlib

├─serverlib

│  └─pkg

│          Dog.class

│          Sample.class

└─syslib

​    │  MyClassLoader.java

​    │  MyClassLoader.class

​    └─pkg

​            Sample.java

​            Dog.java

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader1

Dog loaded by loader1

Exception in thread "main" java.lang.NoClassDefFoundError: pkg/Sample

​        at MyClassLoader.main(MyClassLoader.java:70)

 

由于MyClassLoader类由系统类加载器加载，而Sample类由loader1类加载，因此MyClassLoader类看不见Sample类（根据规则“由父加载器加载的类不能看见子加载器加载的类”）。在MyClassLoader类的main()方法中使用Sample类，会导致NoClassDefFoundError错误。但把Sample类与Dog类的类文件拷贝到D:\myapp\syslib下时，却又能正常，因为此时他们都是由系统类加载器加载，MyClassLoader与Sample、Dog属于同一命名空间中的类，所以MyClassLoader能正常访问Sample类。

 

当两个不同命名空间内的类相互不可见时，可采用反射机制来访问对方实例的属性和方法，如果把MyClassLoader类的main()方法替换为如下代码：

```java
MyClassLoader loader1 = new MyClassLoader("loader1");

loader1.setPath("d:/myapp/serverlib/");

Class objClass = loader1.loadClass("pkg.Sample");

Object obj = objClass.newInstance();

Field f = objClass.getField("v1");

int v1 = f.getInt(obj);

System.out.println(v1);
```

D:\myapp>java -classpath ./syslib MyClassLoader

Sample loaded by loader1

Dog loaded by loader1

v1=1

 

\>>>使用URLClassLoader类<<<

URLClassLoader为在，它扩展了ClassLoader类，它不仅能从本地文件系统中加载类，还可以从网上下载类。下面程序演示了从jar文件中加载Sample类：

```java
URLClassLoader urlLoader = new URLClassLoader(new URL[] { new URL("file:d:/sample.jar") });
Class c = urlLoader.loadClass("pkg.Sample");
System.out.println(c.newInstance());
```

 

输出：

Sample loaded by java.net.URLClassLoader@757aef

Dog loaded by java.net.URLClassLoader@757aef

pkg.Sample@19821f

 

\>>>类的卸载<<<

当Sample类被加载、连接和初始化后，它的生命周期就开始了。当代表Sample类的Class对象不再被引用，即不可达时，Class对象生命就会结束，Sample类在方法区内的数据也会被卸载，从而结束Sample类的生命周期。由此可见，一个类何时结束生命周期，取决于代表它的Class对象何时结束生命周期。

由VM自带的类加载器所加载的类，在VM的生命周期中，始终不会被卸载（如Object类）。VM自带的类加载器包括根类加载器、扩展类加载器和系统类加载器。VM本身会始终引用这此类加载器，而这些类加载器则会始终引用它们所加的类的Class对象，因此这此Class对象始终是可达的，所以说由VM自带的类加载器所加载的类始终不会被卸载。

由用户自定义的类加载器所加载的类是可以被卸载的。

 

下面以MyClassLoader类为例，介绍Sample类被卸载时机。把Sample.class和Dog.class拷贝到D:\myapp\serverlib目录下，然后把MyClassLoader类的main()方法替换为：

```java
MyClassLoader loader1 = new MyClassLoader("loader1");// 1

loader1.setPath("d:/myapp/serverlib/");// 2

Class objClass = loader1.loadClass("pkg.Sample");// 3

System.out.println("objClass hashCode：" + objClass.hashCode());// 4

Object obj = objClass.newInstance();// 5

loader1 = null;// 6

objClass = null;// 7

obj = null; // 8

loader1 = new MyClassLoader("loader1");// 9

loader1.setPath("d:/myapp/serverlib/");//10

objClass = loader1.loadClass("pkg.Sample");// 11

System.out.println("objClass hashCode：" + objClass.hashCode());
```

运行以上程序时，Sample类由loader1加载。在类加载器的内部实现中，用一个Java集合来存放所加载类的引用。另一方面，一个Class对象总是会引用它的类加载器，调用Class对象的getClassLoader()方法，就能获得它的类加载器。由此可见，代表Sample类的Class实例与loader1之间为双向关系。

一个类实例总是引用代表这个类的Class对象。在Object类中定义了getClass()方法，这个方法返回代表对象所属类的Class对象的引用。此外，所有的Java类都有一个静态属性Class，它引用代表这个类的Class对象。

当程序执行完第5步时，引用变量与对象之间的引用关系如图：

[![image005](https://images0.cnblogs.com/blog/717614/201502/132208220584625.jpg)](http://www.cnblogs.com/$image005[3].jpg)

从图可以看出，loader1变量和obj变量间接引用代表Sample类的Class对象，而objClass变量则直接引用它。

当程序执行完第8步，所有的引用变量都置为null，此时Sample对象结束生命周期，MyClassLoader对象结束生命周期，代表Sample类的Class对象也结束生命周期，Sample类在方法区内的二进制数据被卸载（注，但这里并不代表立即被回收了）。

当程序执行完第11步时，Sample类又重新被加载，在堆区会生成一个新的代表Sample类的Class实例：

以上程序输出结果如下：

objClass hashCode：10267414

Sample loaded by loader1

Dog loaded by loader1

objClass hashCode：11394033

 

注，运行之前一定要先删除syslib下面的Sample.class与Dog.class类文件，否则上面的程序会由系统类加载器去加载Sample类，此时Sample类在执行第8行后Sample的Class对象也不会卸载，因为该Class对象是由系统类加载器加的。如果不删除syslib下的Sample.class与Dog.class类文件，则输出结果为：

objClass hashCode：14285251

Sample loaded by sun.misc.Launcher$AppClassLoader@82ba41

Dog loaded by sun.misc.Launcher$AppClassLoader@82ba41

objClass hashCode：14285251

 

#### Class中的getResourceAsStream

===============Class中的getResourceAsStream===================

```java
public InputStream getResourceAsStream(String name) {

    name = resolveName(name);

    ClassLoader cl = getClassLoader0();//获取类加载

    if (cl==null) {//如果类加载器为null，则为根类加载器

        // A system class. 而不是应用类时，使用根类加载器来加载资源

        return ClassLoader.getSystemResourceAsStream(name);

    }

    //通过调用ClassLoader的getResourceAsStream在类加载器搜索路径中加载指定的路径资源

    return cl.getResourceAsStream(name);

}

/*

* 从实现可以看出，传进来的资源路径 name 有两种形式： 一是以 / 开头的路径，

* 它是相对于 <CLASSPATH> 路径的文件路径 二是不以 / 开头的路径，它是相对

* 于当前类所在包的路径。

*/

private String resolveName(String name) {

       if (name == null) {

              return name;

       }

       /*

        * 如果传进来的路径不是以 / 开头，则name表示只能是相对于 包路径的文件

        * 名路径，如某个类的完整类名为 pak1.pak2.ClassXXX 传递进来的 name

        * 为 dir/filename.txt，则最后返回的路径为 pak1/pak2/dir/filename.txt，

        * 它表示在<CLASSPATH>/pak1/pak2/dir 上当下有 filename.txt 文件

        *

        * 如果name是以 / 开头，则会去掉 开头的 / 后返回。要注意的是， 此时的路

        * 径开头的 / 表示的是 <CLASSPATH>，如果此时某个文件是在 某个包目录下，

        * 则 name 一定要带上包路径

        */

       if (!name.startsWith("/")) {

              Class c = this.getClass();

              while (c.isArray()) {

                     c = c.getComponentType();

              }

              String baseName = c.getName();

              int index = baseName.lastIndexOf('.');

              if (index != -1) {

                     name = baseName.substring(0, index).replace('.', '/') + "/"

                                   + name;

              }

       } else {

              name = name.substring(1);

       }

       return name;

}
```

 

===============ClassLoader的getResourceAsStream===================

```java
public InputStream getResourceAsStream(String name) {

       // 获取资源的URL

       URL url = getResource(name);

       try {

              // 打开资源流

              return url != null ? url.openStream() : null;

       } catch (IOException e) {

              return null;

       }

}

 

//获取资源的URL对象

public URL getResource(String name) {

       URL url;

       //如果类加载器还有父加载器，则由父加载器加载

       if (parent != null) {

              url = parent.getResource(name);

       } else {//一直递归到根加载器。按理说根加载器的搜索路径为<JAVA_HOME>/jre/lib/rt.jar，所以url会返回null?????

              url = getBootstrapResource(name);

       }

       if (url == null) {

              url = findResource(name);// ClassLoader中此方法的实现返回 null

       }

       return url;

}
```

上面是ClassLoader类的实现，在不同的类加载器中资源的加载方式实现是不同的，比如ExtClassLoader、AppClassLoader实现就可能不一样。

 

比如在pak1.pak2包下有xx.txt文件，下面的三个语句是等效的：

```java
// 相对于当前类所在的包路径 pak1/pak2

pak1.pak2.Resource.class.getResourceAsStream("xx.txt");

// /pak1/pak2/xx.txt 相对于<JAVA_HOME>

pak1.pak2.Resource.class.getResourceAsStream("/pak1/pak2/xx.txt");

/*

* Class的getResourceAsStream就是调用ClassLoader.getResourceAsStream来实现的，

* 而Class的getResourceAsStream再调用ClassLoader.getResourceAsStream之前，

* 就已经将路径开头的 / 去掉了，所以如果是直接调用ClassLoader.getResourceAsStream

* 时，一定不能以/开头

*/

pak1.pak2.Resource.class.getClassLoader().getResourceAsStream("pak1/pak2/xx.txt");
```



##### 通过Class类的getResourceAsStream获取 jar 包里的资源

可以通过Class类的getResourceAsStream来获取 jar 包里的资源，比如jar包里含有一个 /resource/res.txt 文件，并与Resource 类的class打一个 jar，结构如下：

​    1、src/

​              src/pak1/pak2/Resource.java

​    2、bin/

​              bin/resource/res.txt

​              bin/pak1/pak2/Resource.class

可以通过下面的方法来获取jar包里的资源

```java
public class Resource {

       public void getResource() throws IOException{

        //在<CLASSPATH>路径中搜索资源，这里的classpath为jar包所在的路径

              InputStream is=this.getClass().getResourceAsStream("/resource/res.txt");

              BufferedReader br=new BufferedReader(new InputStreamReader(is));

              String s="";

              while((s=br.readLine())!=null)

                     System.out.println(s);

       }

}
```

上面是资源文件与访问的它的类在同一jar包中，如果它们不在同一jar包中，可以这样访问：

```java
public class Resource {

       public void getResource() throws IOException, Exception {

              URLClassLoader urlLoader = new URLClassLoader(new URL[] { new URL(

                            "file:d:/sample.jar") });

              // Class c = urlLoader.loadClass("pkg.Sample");

              InputStream is = urlLoader.getResourceAsStream("/resource/res.txt");

              BufferedReader br = new BufferedReader(new InputStreamReader(is));

              String s = "";

              while ((s = br.readLine()) != null)

                     System.out.println(s);

       }

}
```

 

##### 通过JarURLConnection获取 jar 包里的资源

JAR URL 的语法为： jar:<url>!/{entry}   如，jar:http://www.foo.com/bar/baz.jar!/COM/foo/Quux.class

JarURLConnection 实例只能用于从 JAR 文件读取内容。

新的转型语法：

```java
class Building {}

class House extends Building {}

public class ClassCasts {

  public static void main(String[] args) {

    Building b = new House();

    Class<House> houseType = House.class;

    House h = houseType.cast(b);// 将b向下转型，这样在Java SE5不会发生警告

h = (House)b; // 以前的强制转法，但这样在Java SE5会发生警告

// 所以为了在Java SE5强转时不发生警告，则请使用新的转型方式

  }

}
```

 

动态的instanceof：Class的isInstance(Object obj)方法与instanceof完全等价，判定指定的`Object`是否与此 `Class`所表示的对象赋值兼容。此方法是 Java 语言 `instanceof`运算符的动态等效方法。如果指定的 `Object`参数非空，且能够在不引发 `ClassCastException`的情况下被强制转换成该 `Class`对象所表示的引用类型，则返回 true，否则返回 `false`。

class.isAssignableFrom(Class<?> cls)：判定此class对象所表示的类或接口与指定的cls参数所表示的类或接口是否相同，或是class否是cls的超类或超接口。如果是则返回`true`，否则返回 `false`。

反射：运行时的类信息

Class类与java.lang.reflect类库一起对反射的进行了支持，该类库包含了Field、Method以及Constructor类（都实现了Member接口）。这些类型的对象是由JVM在运行时创建的，用来表示未知类里对应的成员。这样你就可以使用Constructor创建新的对象，用get()和set()方法读取和修改与Field对象关联的字段，用invoke()方法调用与Method对象关联的方法。另外，还可以调用Class对象的getFields()、getMethods()和getConstructors()等很便利的方法，以返回表示字段、方法以及构造器的对象的数组。这样，匿名对象的类信息就能在运行时被完全确定下来，而在编译时不需要知道任何事情，但是，这个类的.class文件对于JVM来说必须是可获取的，要么在本机上，要么可以通过网络取得。

RTTI与反射之间真正的区别只在于：对于RTTI来说，编译器在编译时打开与检查.class文件，换句话说，我们可以用“普通”方式调用对象的所有方法；而对于反射机制来说，.class文件在编译时是不可获取的，所以是在运行时打开与检查.class文件。

通过反射可以访问类中的所有东西，包括private修饰的。

动态代理：

```java
import java.lang.reflect.InvocationHandler;

import java.lang.reflect.Method;

import java.lang.reflect.Proxy;

//实现调用处理器接口

class MethodSelectorHandler implements InvocationHandler {

       private Object proxied;// 引用被代理的真真对象

       public MethodSelectorHandler(Object proxied) {

              this.proxied = proxied;

       }

 

       // 实现接口，动态代理可以将所有调用重定向到该方法

       public Object invoke(Object proxy, Method method, Object[] args)

                     throws Throwable {

              // 在这里做额外的工作

              if (method.getName().equals("interesting")) {

                     System.out.println("Proxy detected the interesting method");

              }

              return method.invoke(proxied, args);

       }

}

 

// 代理接口，Java中的动态代理对象一类要实现某个接口

interface SomeMethods {

       void boring1();

       void boring2();

       void interesting(String arg);

       void boring3();

}

 

// 真真被代理的类

class Implementation implements SomeMethods {

       public void boring1() {

              System.out.println("boring1");

       }

 

       public void boring2() {

              System.out.println("boring2");

       }

 

       public void interesting(String arg) {

              System.out.println("interesting " + arg);

       }

 

       public void boring3() {

              System.out.println("boring3");

       }

}

 

class SelectingMethods {

       public static void main(String[] args) {

              // 创建代理对象 第一个参数为类加载器；第二个为所实现的接口，可有多个；第

              // 三个为处理器，构建处理器时需指定真真被代理的对象。返回的是代理对象

              SomeMethods proxy = (SomeMethods) Proxy.newProxyInstance(

                            SomeMethods.class.getClassLoader(),

                            new Class[] { SomeMethods.class }, new MethodSelectorHandler(

                                          new Implementation()));

 

              // 通过代理对象调用

              proxy.boring1();

              proxy.boring2();

              proxy.interesting("bonobo");

              proxy.boring3();

       }

}
```

如果使用反射创建一个对象时，而需要调用带参的构造函数，则可以使用Constructor的newInstance(Object[] initargs)方法来代替Class的newInstance()方法。

 

 

\>>>反射工具<<<

```java
//反射类信息

public class Reflection {

   //打印构造器

   public static void printConstructors(Class cl) {

      /*

       * 返回一个包含某些 Constructor 对象的数组，这些对象反映此 Class 对象所表

       * 示的类的所有公共构造方法。

       */

      Constructor[] constructors = cl.getDeclaredConstructors();

      for (Constructor c : constructors) {

         String name = c.getName();//构造方法的名称

         // 输出构造器前所有修饰符

         System.out.print(" " + Modifier.toString(c.getModifiers()));

         System.out.print(" " + name + "(");

 

         // 输出参数类型与名称

         Class[] paramTypes = c.getParameterTypes();

         for (int j = 0; j < paramTypes.length; j++) {

            if (j > 0) {

                System.out.print(", ");

            }

            System.out.print(paramTypes[j].getName());

         }

         System.out.println(");");

      }

   }

 

   //打印方法

   public static void printMethods(Class cl) {

      /*

       * getDeclaredMethods():

       * 返回 Method 对象的一个数组，这些对象反映此 Class 对象表示的类或接口声明

       * 的所有方法，包括公共、保护、包访问和私有方法，但不包括继承的方法。

       */

      Method[] methods = cl.getDeclaredMethods();

      //准备输出方法

      for (Method m : methods) {

         Class retType = m.getReturnType();//方法返回类型

         String name = m.getName();//方法的名字

 

         //输出方法的修饰符、返回类型以及方法名

         System.out.print(" " + Modifier.toString(m.getModifiers()));

         System.out.print(" " + retType.getName() + " " + name + "(");

 

         //输出参数类型

         Class[] paramTypes = m.getParameterTypes();

         for (int j = 0; j < paramTypes.length; j++) {

            if (j > 0) {

                System.out.print(", ");

            }

            System.out.print(paramTypes[j].getName());

         }

         System.out.println(");");

      }

   }

 

   //打印字段

   public static void printFields(Class cl) {

      Field[] fields = cl.getDeclaredFields();

      for (Field f : fields) {

         Class type = f.getType();//字段类型

         String name = f.getName();//字段名

         System.out.print(" " + Modifier.toString(f.getModifiers()));

         System.out.println(" " + type.getName() + " " + name + ";");

      }

   }

 

   public static void main(String[] args) {

      String name;

      if (args.length > 0) {

         name = args[0];

      } else {

         Scanner in = new Scanner(System.in);

         System.out.println("Enter class name(e.g. java.util.Date)");

         name = in.next();

      }

      try {

         Class cl = Class.forName(name);

         Class supercl = cl.getSuperclass();

         //打印类的定义

         System.out.print("class " + name);

         if (supercl != null && supercl != Object.class) {

            System.out.println(" extends " + supercl.getName());

         }

 

         System.out.println("\n{\n //构造器");

         printConstructors(cl);//打印构造器

         System.out.println(" //字段");

         printFields(cl);//打印字段

         System.out.println(" //方法");

         printMethods(cl);//打印方法

         System.out.println("}");

 

      } catch (Exception e) {

         e.printStackTrace();

      }

   }

}
```

应用反射机制打印对象成员值信息请看XXXXAbstractDTO



### 第十五章泛型

参见《XXXXXX》

 

类型推断：

```java
import java.util.HashMap;

import java.util.List;

import java.util.Map;

 

public class LimitsOfInference {

   /*

    * 外界创建一个Map对象时只需执行

    * Map<String, List<String>> map = newMap();

    *

    * 类似的语句，而不必麻烦地使用

    * Map<String, List<String>> map = new Map<String, List<String>>();

    *

    * 所以可以试着把这些创建集合的代码集中封装到一个公共类中，省去创建时指

    * 定类型，它可根据赋值语句前部分声明来推导出类型参数

    */

   static <K, V> Map<K, V> newMap() {

      /*

       * 编译时会根据赋值语句来推断 K,V 的参数类型。

       *

       * 所谓的方法类型推断是指：

       * 方法是泛型的，但在执行过程中方法体中不知道确切的参数类型，即泛型

       * 方法不带泛型参数，就像该方法是泛型方法但没有传递参数类型，但如果

       * 该方法带类型参数时（如 newMap(K k,V v)）,调用时就不存在类型推

       * 断问题了，因为在调用时参数类型已经传进了，执行时就已确定了，则最

       * 后泛型方法返回的结果类型就可以确定了。

       */

      return new HashMap<K, V>();

   }

 

   static void f(Map<String, List<String>> map) {}

  

   static Map<String, List<String>> h() {

      //将泛型方法的结果作为某方法的返回值，此时也会发生类型推断

      return newMap();

   }

  

 

   public static void main(String[] args) {

      /*

       * 类型推断发生在两个时机，第一个就是直接赋值语句中。

       * 第二就是将泛型方法的结果作为某方法的返回值

       */

      // 赋值语句能推断出newMap方法中的类型参数类型

      Map<String, List<String>> map = newMap();

     

      // 编译没问题，因为h()返回的类型是确定的

      f(h());

     

      //!! Does not compile，因为类型推断只发生在赋值语句与返回时

      // f(newMap());

     

      // 但可以显示的指定返回参数类型

      f(LimitsOfInference.<String, List<String>>newMap());

   }

}
```

泛型类型参数将擦除到它的第一个边界（因为可能会有多个边界），而普通的类型变量在未指定边界的情况下被擦除为Object。使用与不使用泛型生成的字节码是一样的。

推荐使用Array.newInstance()方式创建泛型数组：

T[] arrayMaker(Class<T> kind,int size) {

  return (T[])Array.newInstance(kind, size);

}

但也可这样：

T[] arrayMaker(int size) {

  return (T[])new Object[size];

}

这与上面相同的是最后创建出的数组类型表面上（返回给别人的）都是Object类型（因为擦除关系），但前者的真真数组类型还是由传递进来的Class类类型来决定。

边界：即对象进入和离开方法的地点，这些也是编译器在编译期执行类型检查并插入转型代码的地点。泛型中的所有动作都发生在边界处——对象传递进来的值进行额外的编译检查，并插入对传递出去的值的转型。

extends边界：因为擦除了类型信息，所以，能通过无界（Colored<T>）泛型参数调用的方法只是那些Object中的方法（因为未使用extends定界时上界默认就是Object）。但是，如果将这个参数T限制为某个类型的子类型，那么我们就可以用这些类型子类的相关方法，所以extends的作用在于定界，而定界又是为了调用某个泛型类的方法：

```java
//边界接口

interface HasColor { java.awt.Color getColor(); }

class Colored<T extends HasColor> {

  T item;

  Colored(T item) { this.item = item; }

  T getItem() { return item; }

  // 允许调用边界接口的方法:

  java.awt.Color color() { return item.getColor(); }

}

 

//边界类

class Dimension { public int x, y, z; }

 

// 当同时有边界类与接口时，接口放在类的后面，与继承一样:

//!! class ColoredDimension<T extends HasColor & Dimension> {}

 

// 多个边界时，边界接口要放在边界类后面:

class ColoredDimension<T extends Dimension & HasColor> {

  T item;

  ColoredDimension(T item) { this.item = item; }

  T getItem() { return item; }

  //访问边界接口方法

  java.awt.Color color() { return item.getColor(); }

  //访问边界类中的成员

  int getX() { return item.x; }

  int getY() { return item.y; }

  int getZ() { return item.z; }

}

 

//另一边界接口

interface Weight { int weight(); }

 

// 与继承一样，多个边界时，只允许一个边界类，但允许多个边界接口:

class Solid<T extends Dimension & HasColor & Weight> {

  T item;

  Solid(T item) { this.item = item; }

  T getItem() { return item; }

  //访问边界接口

  java.awt.Color color() { return item.getColor(); }

  //访问边界类中的成员

  int getX() { return item.x; }

  int getY() { return item.y; }

  int getZ() { return item.z; }

  //访问边界接口

  int weight() { return item.weight(); }

}

 

//Solid类中的类型参数实现类

class Bounded

extends Dimension implements HasColor, Weight {

  public java.awt.Color getColor() { return null; }

  public int weight() { return 0; }

} 

 

public class BasicBounds {

  public static void main(String[] args) {

    Solid<Bounded> solid =

      new Solid<Bounded>(new Bounded());

    solid.color();

    solid.getY();

    solid.weight();

  }

}
```

 

边界与继承：可以在继承的每个层次上逐渐添加边界限制。使用继承的方式修改上面泛型类，这样不必在每个类中重复定义与实现某些方法：

```java
class HoldItem<T> {

  T item;

  HoldItem(T item) { this.item = item; }

  T getItem() { return item; }

}

 

//添加HasColor边界接口，此时的类型参数T要是实现了HasColor的类

class Colored2<T extends HasColor> extends HoldItem<T> {

  Colored2(T item) { super(item); }

  //访问边界接口中的方法

  java.awt.Color color() { return item.getColor(); }

}

 

//添加Dimension边界类，此时的类型参数T要是继承了Dimension类并是

//实现HasColor接口的类

class ColoredDimension2<T extends Dimension & HasColor>

extends Colored2<T> {

  ColoredDimension2(T item) {  super(item); }

  //访问新添加的边界类中的成员

  int getX() { return item.x; }

  int getY() { return item.y; }

  int getZ() { return item.z; }

}

 

//添加Weight边界类，此时的类型参数T要是继承了Dimension类并

//实现HasColor接口与Weight接口的类

class Solid2<T extends Dimension & HasColor & Weight>

extends ColoredDimension2<T> {

  Solid2(T item) {  super(item); }

  //访问新添加的边界接口中的方法weight()

  int weight() { return item.weight(); }

}

 

//继承边界类测试

public class InheritBounds {

  public static void main(String[] args) {

   //Bounded符合Solid2中类型参数T

    Solid2<Bounded> solid2 =

      new Solid2<Bounded>(new Bounded());

    solid2.color();//访问父类Colored2中的方法

    solid2.getY();//访问父类ColoredDimension2中的方法

    solid2.weight();//访问自身的方法

  }

}
```

通配符?的疑问：

```java
class Fruit {}

class Apple extends Fruit {}

class Jonathan extends Apple {}

class Orange extends Fruit {}

 

public class CompilerIntelligence {

  public static void main(String[] args) {

   //? 通配符，flist指向存放Fruit及任何子类List容器

    List<? extends Fruit> flist =Arrays.asList(new Apple());

    //使用通配符定义的引用，不能通过该引用调用任何带有泛型参数的方法

    //!! flist.add(new Apple());

//!! flist.add(new Fruit());

 

    //但可调用返回参数类型是泛型的方法，尽管返回类型为泛型

    //但不管返回的是什么，至少是Fruit类型，所以是合理的

    Apple a = (Apple)flist.get(0); // No warning

   

    //但可调用参数不是泛型参数的方法，通过查看源码，我们发现

    //contains与indexOf的参数类型都是Object

    flist.contains(new Jonathan());

    flist.indexOf(new Fruit());

  }

}
```

从上面程序可以知道，在使用通配符定义的引用后，为什么add方法不能使用，而contains与indexOf却可以。该限制不是用编译器去检查特定的方法是否修改了它的对象。其实编译器并没有这么聪明。add()接受的是一个具有泛型参数类型的参数，但是contains()和indexOf()接受的是一个Object类型的参数。因此当你定义一个ArrayList<? extends Fruit>时，add(E o)的参数 E 就变成了“? extends Fruit”，因此编译器并不知道这里需要Fruit的哪个具体子类型，所以它不能接受任何类型的Fruit，编译器直接拒绝对参数列表中涉及通配符的方法（例如add(E o)）的调用。在使用contains(Object elem)和indexOf(Object elem)时，参数类型是Object，因此不涉及任何通配符，所以编译器将允许这样调用。因此，为了在类型中使用了通配符的情况下禁止这个类的调用，我们需要在参数列表中使用类型参数。

从下面程序也可看出这一点：

```java
public class Holder<T> {

  private T value;

  public Holder() {}

  public Holder(T val) { value = val; }

  public void set(T val) { value = val; }

  public T get() { return value; }

  //参数不是泛型类型参数

  public boolean equals(Object obj) {

    return value.equals(obj);

  }

  public static void main(String[] args) {

    Holder<Apple> apple = new Holder<Apple>(new Apple());

    Apple d = apple.get();

    apple.set(d);

   

    //Holder<Apple>类型不是Holder<Fruit>的子类

    //!! Holder<Fruit> Fruit = apple; // Cannot upcast

   

    //但使用通配符是可以的

    Holder<? extends Fruit> fruit = apple; // OK

   

    //可以调用它的get方法，因为该方法不带参数，尽管返回类型为泛型

    //因为不管返回的是什么，但至少是Fruit类型，所以是合理的

    Fruit p = fruit.get();

   

    //因为本身就是Apple类型，所以能安全强制向下转型

    d = (Apple)fruit.get();

    try {

      //编译时不会警告，但运行时发生ClassCastException

      Orange c = (Orange)fruit.get();

    } catch(Exception e) { System.out.println(e); }

   

    //因为fruit是通过通配符方式定义的，所以不能调用带类型参数的方法

    //!! fruit.set(new Apple());

    //!! fruit.set(new Fruit());

   

    //但是equals方法参数是Object类型，所以可以调用

    System.out.println(fruit.equals(d)); // OK

  }

}
```

<T extends MyClass>是用来解决不能调用泛型类型参T及实例的方法的问题，即解决了类型边界问题。

<? extends MyClass>是用来解决 ArrayList<Fruit> list = new ArrayList< Apple>();的问题或者是方法参数的传递问题。

超类型通配符：通配符是由某个特定类的任何基类来界定，方法是指定<? super MyClass>，甚至使用类型参数：<? super T>，但你不能对泛型参数指定一个超类型边界（如<T super MyClass>，但<T extends MyClass>却是可以的）。只能用于方法的参数类型说明与变量的定义，不能用于类，也不可用来定义某个类型参数T，因为没有<T super MyClass>形式。

定义变量：List<? super Apple> l = new ArrayList<Fruit>();

方法的参数类型说明：Collections.static <T extends Object & Comparable<? super T>> T max(Collection<? extends T> coll)

 

##### **ArrayList<? extends Fruit>与ArrayList<? super Jonathan>的区别：**

```java
class Fruit {}

class Apple extends Fruit {}

class Jonathan extends Apple {}

class OtherJonathan extends Jonathan {}

public class SuperTypeWildcards {

   static void writeTo(List<? extends Apple> apples) {

      /*

       * ArrayList<? extends Fruit>表示它定义的list1引用可指向能存放

       * Fruit及其子类的容器实例，但不能真真的向容器里放入任何东西除了

       * null。

       *

       * 这里使用的是 extends ，所以new ArrayList<XXX>()中的XXX只能是

       * Fruit 或其子类。

       */

      ArrayList<? extends Fruit> list1 = new ArrayList<Apple>();

      // !! list1.add(new Apple());

      // !! list1.add(new Fruit());

 

      /*

       * ArrayList<? super Jonathan>表示它定义的list2引用可指向能存

       * 放Jonathan及子类实例的容器，与上面不同的是可以向其中放入对象。

       *

       * 这里使用的是 super ，所以new ArrayList<XXX>()中的XXX只能是

       * Jonathan 或其父类，因为只有这样才能确保创建出来的容器能存放

       * Jonathan及子类实例

       */

      ArrayList<? super Jonathan> list2 = new ArrayList<Fruit>();

      // !! list2.add(new Fruit());//不能存入Fruit

      // !! list2.add(new Apple());//不能存入Apple

      list2.add(new Jonathan());

      list2.add(new OtherJonathan());

 

      // 类型参数中含有super关键字所定义的引用只能指向类型参数及父类的实例

      // !! ArrayList<? super Apple> list13 = new ArrayList<Jonathan>();

   }

}
```

 

再看另一个实例：

```java
class GenericWriteReading {

 

   //----------写入

   static <T> void writeExact(List<T> list, T item) {

      list.add(item);

   }

 

   /*

    * 返回的参数类型为 t1与t2的公共父类，因为Object为任何类的父类，所

    * 以t1与t2可以是任何类型参数都可以

    */

   static <T> T writeExact1(T t1, T t2) {

      return t1 == null ? t2 : t1;

   }

 

   static void write1() {

      writeExact(new ArrayList<Apple>(), new Apple());

      writeExact(new ArrayList<Fruit>(), new Fruit());

 

      //书说这行不能编译通过，源码已被注释掉了，但运行了一下可以，why?

      writeExact(new ArrayList<Fruit>(), new Apple());

 

      /*

       * 编译不能通过，上面行可以，编译器是如何做得到的?

       *

       * 经过自己推敲，上下两行不同的是，list的参数类型如果是后面item参数类

       * 型的父类就可以，把List换成自己创建的类也是这样的，编译器也许就是根

       * 据 List<XXX> 中的类型XXX 是否与item的类型相同或是父类来判断的。

       *

       * 另外，从writeExact1方法可知，如果类型参数不是作为其他类型的类型参数

       * 使用（如writeExact1方法中的类型参数）时，这此参数的类型之间可以说没

       * 有任何限制，可以传递任何类型的参数，因为Object为他们的公共父类。

       *

       * 但如果把类型参数作为某个类的类型参数使用时（如writeExact中T被应用到

       * 了List<T>中），则参数间就会有直接关系了：两个T要么相同，要么List<T>

       * 中的T类型是第二个T的父类（注，不能反过来），这其实与

       * writeWithWildcard(List<? super T> list, T item)作用是一样的，在下

       * 面我们将会看到。

       * 在此种情况下，为什么参数类型要有直接的父子关系呢？其实也是有道理的，

       * 因为第一个List<T>类型的参变量list对象的某些方法还有可能要使用到第二

       * 个参变量item，只有在第二个参数的类型T是第一个参数类型T的子类或本身时

       * ，才能将item传到需要它的相应方法中去。

       *

       */

      //!! writeExact(new ArrayList<Apple>(), new Fruit());

      Apple app = writeExact1(new Apple(), new Apple());

 

      /*

       * 下面两行都可以，方法声明的是t1与t2的类型一样，但下面为父子关系也可

       * 以，原因就是他们的父类为Object，所以可以两者任意交换

       */

      Fruit fru = writeExact1(new Fruit(), new Apple());

      fru = writeExact1(new Apple(), new Fruit());

      //!! 返回类型只能是参数类型的公共父类，即Fruit

      //!! app = writeExact1( new Apple(),new Fruit());

 

      // ArrayList与String的公共类型有Serializable与Object，所以返回类型有两种

      Serializable s = writeExact1(new ArrayList(), new String());

      Object o = writeExact1(new ArrayList(), new String());

   }

 

   static <T> void writeWithWildcard(List<? super T> list, T item) {

      list.add(item);

   }

 

   static void write2() {

      writeWithWildcard(new ArrayList<Apple>(), new Apple());

      writeWithWildcard(new ArrayList<Fruit>(), new Fruit());

      // 父类类型的容器可以存储子类对象，并且取出时的类型至少为父类类型

      writeWithWildcard(new ArrayList<Fruit>(), new Apple());

      //!! writeWithWildcard(new ArrayList<Apple>(), new Fruit());

   }

 

   //----------读取

 

   static <T> T readExact(List<T> list) {

      return list.get(0);

   }

 

   static List<Apple> apples = Arrays.asList(new Apple());

   static List<Fruit> fruit = Arrays.asList(new Fruit());

 

   // 通过方法直接读取:

   static void f1() {

      Apple a = readExact(apples);

      Fruit f = readExact(fruit);

      f = readExact(apples);

   }

 

   /*

    * 然而，如果你定义的是一个泛型类，而不是方法时，当你实例化这个类

    * 时，参数类型就已经确定，调用它的方法时就不能改变了，这与泛型方

    * 法是不一样的：

    */

   static class Reader<T> {

      T readExact(List<T> list) {

         return list.get(0);

      }

   }

 

   // 通过类来读取

   static void f2() {

      //泛型类创建时需指定类型，即创建时类型就已确定

      Reader<Fruit> fruitReader = new Reader<Fruit>();

      Fruit f = fruitReader.readExact(fruit);

      //因为创建时类型就已定为Fruit，所以不能传递Apple：

      //!! Fruit a = fruitReader.readExact(apples);

   }

 

   static class CovariantReader<T> {

      /*

       * 可以通过通配符边界<? extends T>在运行时传递子类类型。下面方法与

       * readCovariant(List<T> list, T t) 是不一样的，该方法中隐含着两类

       * 型参数有直接的父子关系或相同，因为只有这样list对象才能使用t对象。

       * 而下面的方法第一个参数list的类型参数声明成List<? extends T>，这

       * 就已经明显的表明了第一个类型参数与第二个类型参数的关系，即第二个

       * 类型参数要是第一个类型参数的父类或相同，这与readCovariant(List<T>

       *  list, T t)形式的方法恰好相反。同样

       *  readCovariant2(List<? super T> list, T t)也明确说明了两个参数的

       *  的父子关系，第一个T为第二个T的父类或相同。

       */

      T readCovariant(List<? extends T> list, T t) {

 

         /*

          * 假如定义如下：List<? extends Apple> list = new ArrayList<Jonathan>();

          * 则不可以通过引用list调用任何泛型方法，为什么？

          * 原因就是这些泛型方法的真真参数类型比定义时类型要窄，如这里的Jonathan就要

          * 比Apple类型就要窄，所你不能通过一个Jonathan类型的变量来接收一个Apple类型

          * 的实例吧，所以不能通过被<? extends T>的引用来调用其任何泛型方法。

          *

          * 经过上面我们会很清楚的知道下面语句为什么不行了

          */

        

         //!! list.add(t);

        

         /*

          * 假如定义如下：List<? extends Apple> list = new ArrayList<Jonathan>();

          * 则可以通过引用list调用返回类型为泛型的方法呢（当然方法参数不能是泛型的），

          * 并且返回类型为Apple，为什么？因为即使你将list引用指向成ArrayList<Jonathan>

          * 类型的实例，还是将它指向ArrayList<OtherJonathan>类型的实例，但他们的都

          * 不会超过上界类型Apple，所以返回的类型至少为上界类型Apple。

          *

          * 经过上面我们会很清楚的知道下面语句为什么返回的类型自然就是T了。

          */

         return list.get(0);// 但返回类型至少是T

      }

 

      //可以通过通配符边界<? super T>在运行时传递父类类型，但不能作为返回类型，即可以通过泛型方法传进，但不能通过泛型方法返回

      T readCovariant2(List<? super T> list, T t) {

         /*

          * 假如定义如下：List<? super Jonathan> list = new ArrayList<Apple>();

          * 则可以通过引用list调用其泛型方法，并且传递的参数只能是Jonathan或其子类

          * 为什么我们可以通过<? super Jonathan>类型的引用list来调用<Apple>类型

          * 实例的泛型方法？原因就是这些泛型方法的真真参数类型比定义时类型要宽，

          * 如这里的Apple就要比Jonathan类型就要宽，所以我们传递给泛型方法真实现类

          * 的子类或本身是可以的，平时我们也是这样做的，即使用父类的引用指向子类对象

          *

          * 经过上面我们会很清楚的知道下面语句为什么可行了

          */

         list.add(t);

 

         /*

          * 假如定义如下：List<? super Jonathan> list = new ArrayList<Apple>();

          * 为什么调用返回类型为泛型类型时，返回的类型却只能是Object呢？因为你可

          * 以将list引用指向成ArrayList<Apple>类型的实例，你也可能将它指向ArrayList

          * <Object>类型的实例，这将导致通过list引用调用返回结果为泛型类型的方法时，

          * 有可能是Jonathan、Apple、还有 Object，所以最终只能为Object类型。最主

          * 要因为类型参数被<? super T> 修饰时，该类型参数的最上界就是Object类了

          * ，所以通过被<? super T>修饰的引用调用返回类型为泛型方法时，返回的类型

          * 只能是最上界Object，而不能是下界T，也不能为它们之间的某个类型。

          *

          * 从上述就可以很清楚的知道了为什么list.get(0)返回的是Object类型了。

          */

         // !! return list.get(0);

         return (T) list.get(0);//但可以强转

      }

   }

 

   static void f3() {

      CovariantReader<Fruit> fruitReader = new CovariantReader<Fruit>();

      Fruit f = fruitReader.readCovariant(fruit, new Fruit());

      f = fruitReader.readCovariant(fruit, new Apple());

      Fruit a = fruitReader.readCovariant(apples, new Apple());

   }

 

   public static void main(String[] args) {

      write1();

      write2();

      f1();

      f2();

      f3();

   }

}
```



无界通配符：

List<?> list1：没有明确的边界，可以引用存放任何类型的List，List<?> list1 = new ArrayList();不会发生警告，与有边界的通配符一样，也不能通过list1向容器中添加除null的任何类型的对象。此种情况的边界实质上为Object，但又与List<? extends Object> list1不一样。

List<? extends Object> list1: 可以引用存放任何类型的List，List<? extends Object > list1 = new ArrayList();会发生警告，因为它指定了明确的边界，所以赋值时要指定明确的边界。

 

编译器处理List<?>与List<? extends Object>是不一样的，前者没有明确的边界，后者有，但两者的边界都是Object。

 

List list1与List<?> list1的区别：

由于擦除操作，List<?>看起来等价于List<Object>（但又不完全相同，因为至少List<Object> l = new ArrayList<String>();是有问题的，而List<?> l = new ArrayList<String>();却是可以的）。而List实际上也是List<Object>。

List实际上表示“可以存放任何Object类型的原生List”，而List<?>表示“只能存放某种类型的非原生List，只是我们不知道那种类型是什么”。

 

原生类型List和参数化类型List<Object>是不一样的。如果使用了原生类型，编译器不会知道在list允许接受的元素类型上是否有任何限制，它会允许你添加任何类型的元素到list中。这不是类型安全的，但如果使用了参数化类型List<Object>，编译器便会明白这个list可以包含任何类型的元素，所以你添加任何对象都是安全的。

\>>>泛型问题<<<

不能将基本类型用作类型参数，如果是基本类型时只能使用其包装类型或自动包装机制。

 

实现参数化接口：一个类不能实现同一个泛型接口多次，下面的Hourly编译不能通过：

```java
interface Payable<T> {}

class Employee implements Payable<Employee> {}

class Hourly extends Employee implements Payable<Hourly> {}
```

但去掉泛型参数编译就可以通过了：

```java
class Employee implements Payable{}

class Hourly extends Employee implements Payable{}
```

或者是将泛型参数置为相同也可：

```java
class Employee implements Payable<Employee> {}

class Hourly extends Employee implements Payable<Employee> {}
```

在使用某些更基本的Java接口，例如Comparable<T>时，这个问题可能会变得十分头痛。

在Java泛型中，有一个好像是经常使用的语法，但它有点令人费解：

class A<T extends A<T>>{}，这是允许的，这个说明了extends关键字用于边界与用来创建子类明显是不同的。A类接受泛型参数T，而T是由一个边界类来限定，这个边界就是接受T作为类型参数的A类。这种自限定所做的，就是要求在继承关系中，像下面这样使用这个类：

class B extends A<B>{}

这会强制要求将正在定义的类当作参数传递给基类，它可以保证类型参数必须与正在被定义的类相同。

class A<T>{}

class B extends A<B>{}

以上也是允许的，它表示“我在创建一个新类，它继承自一个泛型类型，这个泛型类型接受我的类的名字作为其参数”，即父类使用子类替代其类型参数。

参数协变：

方法参数类型会随子类而变化。尽管自限定类型可以产生子类类型相同的返回类型，但在JavaSE 5中已引入参数协变：

```java
interface Base {

  Base get();

}

 

interface Derived extends Base {

  /*

   * 从Java SE5开始子类方法可以返回比它重写的基类方法更

   * 具体的类型，但是这在早先的Java版本是不允许——重写

   * 时子类的返回类型一定要与基类相同。

   *

   * 但要注意的是：子类方法返回类型要是父类方法返回类型

   * 的子类，而不能反过来，即父类 Derived get(); 而重

   * 写时子类为Base get();是不行的。

   */

   Derived get();

}

 

public class CovariantReturnTypes {

  void test(Derived d) {

     Derived d1 = d.get();

     Base d2 = d.get();

  }

}

使用自限定泛型修改上面程序：

interface Base<T extends Base<T>> {

  T get();

}

interface Derived extends Base<Derived> {}

 

public class CovariantReturnTypes {

  void test(Derived d) {

         Derived d1 = d.get();

         Base d2 = d.get();//也可返回基类类型

  }

}
```

上面程序是方法返回类型协变，返回类型协变在Java SE5中得到了支持——方法可以返回比它重写的基类方法更具体的类型。但如果子类的方法参数是更具体类型时，这时是重载而不是重写了（注，以前版本就是这样）：

```java
class Base {

   void get(HashMap l) {

      System.out.println("Base get()");

   }

}

 

class Derived extends Base {

   // 这是重载，而不是重写，重载

   void get(LinkedHashMap l) {

      System.out.println("Derived get()");

   }

 

   public static void main(String[] args) {

      Derived d = new Derived();

      d.get(new HashMap());//Base get()

      d.get(new LinkedHashMap());//Derived get()

   }

}
```

 

### 第十六章数组

数组与容器之间的区别在三个方面：效率、类型（保持存放元素的类型）、保存基本类型的能力。但从Java SE5后泛型与自动装箱的出现，数组的优点就只是效率了。

无论使用哪种类型的数组，数组标识符其实只是一个引用，指向在堆中创建的一个真实对象，这个（数组）对象又可能保存指向其他对象的引用。

新创建的数组未初始化时，如果存储的是对象，则所有元素自动初始化为null，基本类型初始化为0，字符型自动初始为(char)0，布尔型自动初始化为false。

不能创建泛型数组以及带类型参数的数组：

```java
//T[] array = new T[SIZE]; // 不能创建泛型数组

//ArrayList<String> [] list = new ArrayList<String>[1]; // 也不能创建带类型参数的数组
```

但可以定义一个泛型数组的引用：

```java
T[] array;

ArrayList<String> [] list;
```

虽然你不能创建泛型数组，但是可以创建非泛型数组然后将其转型：

```java
public class ArrayOfGenerics {

  public static void main(String[] args) {

    List<String>[] ls;

    List[] la = new List[10];

    ls = (List<String>[])la; //会发生 "Unchecked" 警告

    ls[0] = new ArrayList<String>();

   

    //上面其实相当于下面一条语句

    //List<String>[] ls1 = (List<String>[])new List[10];

   

    // 编译时会产生错误:

    //! ls[1] = new ArrayList<Integer>();

 

    // 问题是: List<String> 是Object子类

    Object[] objects = ls; //所以可以赋值给Object数组引用

    // 编译与运行都没有错误，因为创建的List数组本身就是原生数组:

    objects[1] = new ArrayList<Integer>();

  }

}
```

Random实例对象可随机返回各种基本类型的数，可设置种子。但Math中的random方法只能返回[0.0, 1)之间的double型小数，实质上该方法就是调用Random实例的nextDouble()来实现的。

Arrays类提供了重载的equals()方法。数组相等的条件是元素个数必须相等，并且对应位置的元素也相等。

 

\>>>动态创建数组<<<

现有如下应用，Employee[]数组满后扩容，该如何做？

```java
Employee[] a = new Employee[100]

…

//arry is full

a = (Employee[])arrayGrow(a);
```

 

错误作法：

```java
static Object[] arrayGrow (Object[] a) {

       int newLength = a.length * 11 / 10 + 10;

       Object[] newArray = new Object[newLength];

       System.arraycopy(a, 0, newArray, 0, a.length);

       return newArray;

}
```

上面在实际应用中会遇到一个问题，这段代码返回的数组类型是Object[]，这是由于使用下面这行代码创建的数组：new Object[newLength]。如果现在我们要对Employee[]数组进行扩展时，则在扩充后我们不能将返回的Object[]类型的对象数组转换成Employee[]数组了（当然如果从Object[]类型对象数组中取出一个个元素后再强转为Employee是没问题）。将一个Employee[]临时地转换成Object[]数组，然后再把它转换回来是可以的，但一个从开始就是Object[]的数组却永远不能转换成Employee[]数组。为了编写这类通用的数组代码，需要能够创建与原数组类型相同的新数组。因此需用到反射包中的Array类的静态方法newInstance.：

```java
static Object arrayGrow (Object a) {
//参数是Object而不是Object[]，因为整型数组类型int[]可以被转换成Object，但不能转换成对象数组

       Class cl = a.getClass();

       if (!cl.isArray()) {

              return null;

       }

       Class componentType = cl.getComponentType();

       int length = Array.getLength(a);

       int newLength = length * 11 / 10 + 10;

       Object newArray = Array.newInstance(componentType, newLength);

       System.arraycopy(a, 0, newArray, 0, newLength);

       return newArray;

}
```

另外，以下转换也是可以的：

```java
String[] strArr =  new String[10];

Object o = strArr;

strArr= (String[]) o;
```

 

### 第十七章容器的深入研究

可以使用Arrays.asList将数组或可变参数转换成AbstractList列表，其底层数据实现就是我们传进的参数数组，因此不能像其他List列表那样调整尺寸，如果你试图用add()或delete()方法在这种列表中添加或删除元素，就有可能会引发去修改数组尺寸的尝试，因此你将在运行时获得“Unsupported Operation(不支持的操作)”错误，该列表只支持读取与修改操作（另外，我们还可以通Collections.unmodifiableList(List)来对一个列表包装之后，只能进行读取操作，即使写是不行了）。

```java
class Snow {}

class Powder extends Snow {}

class Light extends Powder {}

class Heavy extends Powder {}

class Crusty extends Snow {}

class Slush extends Snow {}

 

public class AsListInference {

  public static void main(String[] args) {

    List<Snow> snow1 = Arrays.asList(

      new Crusty(), new Slush(), new Powder());

 

    // Won't compile:

    // List<Snow> snow2 = Arrays.asList(

    //   new Light(), new Heavy());

    // Compiler says:

    // found   : java.util.List<Powder>

    // required: java.util.List<Snow>

 

    // Collections.addAll() doesn't get confused:

    List<Snow> snow3 = new ArrayList<Snow>();

    Collections.addAll(snow3, new Light(), new Heavy());

 

    // Give a hint using an

    // explicit type argument specification:

    List<Snow> snow4 = Arrays.<Snow>asList(

       new Light(), new Heavy());

  }

}
```

当试图创建snow2时，Arrays.asList()中只有Powder类型，因此它会创建List<Powder>而不是List<Snow>，尽管Collections.addAll()工作的很好，因为它从第一个参数中了解到了目标类型是什么。

正如你从创建snow4的操作中所看到的，可以在Arrays.asList()中间插入具体的类型，以告诉编译器对于由Arrays.asList()产生的List类型，实际的类型应该是什么。这称为显示类型参数说明。

Queue接口与List、Set同一级别，都是继承了Collection接口。LinkedList实现了Queue接口。Queue接口窄化了对LinkedList的方法的访问权限（即在方法中的参数类型如果是Queue时，就完全只能访问Queue接口所定义的方法了，而不能直接访问LinkedList的非Queue的方法），以使得只有恰当的方法才可以使用。

offer()，将一个元素插入到队尾，或者返回false。

peek()和element()都将在不移除的情况下返回队头，但是peek()方法在队列为空时返回null，而element()会抛出NoSuchElementExcetption异常。

poll()和remove()方法将移除并返回队头，但是poll()在队列为空时返回null，而remove()会抛出NoSuchElementExcetption异常。

PriorityQueue：优先级队列。先进先出描述了最典型的队列规则，但优先级队列声明下一个弹出元素是最需要的元素（具有最高的优先级）。如果构建一个消息系统，某些消息比其他消息更重要，因而应该更快地得到外理，那么它们何时得到处理就与它们何时到达无关。PriorityQueue添加到JSE5中，是为了提供这种行为的一种自动实现。当你在PriorityQueue上调用offer()方法来插入一个对象时，这个对象会在队列中被排序。默认的排序将使用对象在队列中的自然顺序，但是你可能通过提供自己的Comparator来修改这个顺序。PriorityQueue可以确保当你调用peek()、poll()和remove()方法时，获取元素将是队列中优先级最高的元素。

Collection接口继承了Iterable接口，该接口包含一个能够产生java.util.Iterator的iterator()方法，可用于foreach语句中。

foreach语句可以用于数组或其他任何Iterable，但是这并不意味着数组是一个Iterable，数组与Iterable接口没有直接的关系。

你必须为散列存储和树型存储都创建一个equals()方法，但是hashCode()只有在这个类将会存放到HashMap、HashSet或者LinkedHashMap、LinkedHashSet中时才是必需的，因为这类Hash最终都是能过HashMap的containsKey方法使用if (e.hash == hash && eq(k, e.key))来实现对比的。

Set —— 存入Set的每个元素都必须是唯一的，因为Set不允许保存重复的元素。放入到Set的元素必须定义equals()方法以确保对象的唯一性。Set与Collection有完全一样的接口。Set接口不保证维护元素的次序。

HashSet（优先选择） —— 底层以HashMap来实现。为快速查找而设计的Set。存入HashSet的元素必须定义hashCode()。

TreeSet —— 保持次序的Set，底层为树结构。使用它可以从Set中读取有序的序列。放入的元素必须实现Comparable接口。

LinkedHashSet —— 底层以LinkedHashMap来实现。继承自HashSet，具有HashSet的查询速度，且内部使用链表维护元素顺序（插入的次序）。于是在使用迭代器遍历Set时，结果会按元素的插入次序显示。存入的元素也必须定义hashCode()方法。

HashMap（优先选择） —— Map基于散列表的实现（它取代了Hashtable）。插入和查询“键值对”的开销是固定的。可以通过构造器设置容量和负载因子，以调容器的性能。

LinkedHashMap —— 继承自HashMap，但是迭代遍历它时，取得“键值对”的顺序是其插入次序，或者是最近最小使用（LRU）的次序。只比HashMap慢一点，而在迭代访问时反而更快，因为它使用链表维护内部次序。

TreeMap ——　基于红黑树的实现。查看“健”或“键值对”时，它们会被排序（次序由Comparable或Comparator决定）。TreeMap的特点在于，所得到的结果是经过排序的。TreeMap是唯一带有subMap()方法的Map，它可以返回一个子树。

WeakHashMap ——　弱键映射，允许垃圾回收器回收无外界引用指向象Map中键，这是为解决某类特殊问题而设计的。如果映射之外没有引用指向某个“键”，则此“键”可以被垃圾收集器回收。

ConcurrentHashMap —— 一种线程安全的Map，它不synchronized同步加锁，而是使用新的锁机制。尽管所有操作都是线程安全的，但检索操作不必锁定，并且不支持以某种防止所有访问的方式锁定整个表。

IdentityHashMap —— 使用 == 代替equals()对“键”进行比较的散列映射。

看一个弱引用例子：

```java
class WeakObject {

 

       String name;

 

       public WeakObject(String mwname) {

              this.name = mwname;

       }

 

       public void finalize() {

              System.out.println(name + "对象满足垃圾收集条件，被收集！");

       }

 

       public void show() {

              System.out.println(name + "对象还可以使用！");

       }

}

 

public class WeakReferenceTest {

 

       public static void main(String[] args) {

 

              System.out.println("---对象弱引用---");

 

              WeakObject wo = new WeakObject("weakObject");

 

              //包装成弱引用对象

              WeakReference wr = new WeakReference(wo);

 

              wo = null;

 

              ((WeakObject) wr.get()).show();

 

              System.out.println("第一次垃圾收集！");

 

              System.gc();

 

              try {

                     Thread.sleep(1000);

              } catch (InterruptedException e) {

                     e.printStackTrace();

              }

 

              if (wr.get() != null) {

                     ((WeakObject) wr.get()).show();

              }

 

              System.out.println("---弱引用map---");

              WeakHashMap whm = new WeakHashMap();

              WeakObject wo2 = new WeakObject("weakObjectKey");

              //这里的值不会被回收

              whm.put(wo2, new WeakObject("weakObjectValue"));

              wo2 = null;

              ((WeakObject) whm.keySet().iterator().next()).show();

              System.out.println("第二次垃圾回收！");

              System.gc();

              try {

                     Thread.sleep(1000);

              } catch (InterruptedException e) {

                     e.printStackTrace();

              }

              ((WeakObject) whm.keySet().iterator().next()).show();

       }

}
```

 

对于底层采用数组的ArrayList，无论列表的大小如何，这些访问都很快和一致。而对于LinkedList，访问时间对于较大的列表将明显增加。很显然，如果你需要执行大量的随机访问，链接链表不会是一种好的选择；

插入时，对于ArrayList，当列表变大时，其开销将变得很高昂，但是对于LinkedList，相对来说比较低廉，并且不随列表尺寸而发生变化，这是因为ArrayList在插入时，插入点后面所有元素将后移，如果在插入时超过了数组的最大容量，则会重新创建一个新的数组，并将所有元素复制到新的数组中（不过插入与扩容时使用的都是System.arraycopy方法，效率上比使用循环一个个移动要高），这会随ArrayList的尺寸增加而产生高昂的代价。LinkedList只需链接新的元素，而不必修改列表中剩余的元素，因此可以认为无论列表尺寸如何变化，其代价大致相同。

在LinkedList中的插入和移除代价相当低廉，并且不随列表尺寸发生变化，但是对于ArrayList插入操作代价特别高昂，并且其代价将随列表尺寸增加而增加。

 

HashSet的性能基本上总是比TreeSet好，特别是在添加和查询元素时，而这两个操作也是最重要的操作。TreeSet存在的唯一原因是它可以维持元素的排序状态；所以，只有当需要一个排好序的Set时，才应该使用TreeSet。因为其内部结构支持排序，并且因为迭代是我们更有可能执行的操作，所以，用TreeSet迭代通常比用HashSet要快。

 

除了IdentityHashMap，所有的Map实现的插入操作都会随着Map尺寸变大而明显变慢，但是，查找的代价通常比插入要小得多。

Hashtable的性能大体上与HashMap相当，因为HashMap是用来替代Hashtable的，因为它们使用了相同的底层存储和查找机制，这并没有什么令人奇怪的。

TreeMap通常比HashMap要慢。

LinkedHashMap在插入 时比HashMap慢一点，因为它维护散列数据结构的同时还要维护链表（以保持插入顺序），正是由于这个列表，使得其迭代速度更快一点。

IdentityHashMap则具有完全不同的性能，因为它使用==而不是equals()来比较元素。

 

负载因子小的Hash表产生冲突的可能性小，因此对于插入和查找都是最理想（但是会减慢使用迭代器进行遍历的过程，因为还有很多的空位置，这些空的位置也会在循环中遍历到）。当容量扩大时，现有的对象将重新分布到亲的桶位中（这被称为再散列）。HashMap使用的默认负载因子为0.75（只有当表的饱和度达到四分之三时，才进行再散列），这个因子在时间和空间代价之间达到平衡，更大的负载因子可以提高空间的利用率，但是会增加查找代价。

 

如果你知道将要在HashMap中存储多少项，那么创建一个具有恰当大小的初始容量将可以避免自动再散列的开销。

 

形如Collections.unmodifiableCollection(Collection<? extends T> c)一类方能产生只读容器。

 

形如Collections.synchronizedCollection(Collection<T> c, Object mutex)一类方能产生同步容器。

 

快速报错：Java容器有一种保护机制，能够防止多个进程（或直接通过容器本身而不是迭代器）同时修改同一个容器的内容。如果在你迭代遍历某个容器的过程中，另一个进程介入其中，并且插入、删除或修改此容器内的某个对象，那就会出现问题：也迭代过程已经处理过容器中的该元素，也许还没处理，也许在调用size()之后容器的尺寸缩小了等等，Java容器类类库采用快速报错机制。它会先检测容器上的任何除了你的进程所进行的操作或使用迭代器外的操作所引起的变化，一进发现改变，就会立刻抛ConcurrentModificationExcetion异常。这就是“快速报错”的意思——即，不是使用复杂的算法在事后来检测问题。所以应该在添加、删除、修改完所有元素之后，再获取迭代器。ConcurrentHashMap、CopyOnWriteArrayList和CopyOnWriteArraySet都使用了可以避免ConcurrentModificationExcetion的技术。

# 第十八章I/O

# 第十九章枚举

enum的values()方法返回enum实例的数组，而且该数组中的元素严格保持其在enum中声声明时的顺序。

 

创建enum时，编译器会为你生成一个相关的类，这个类继承自java.lang.Enum。

 

Enum类实例的ordinal()方法返回一个int值，这是每个enum实例在声明时的次序，从0开始。可以使用==来比较enum实例，编译器会自动为你提供equals()和hashCode()方法。

 

Enum类实现了Comparable接口，所以它具有compareTo()方法，同时，它还实现了Serializable接口。

 

name()方法返回enum实例声明的名字，与使用toString()方法效果一样。

 

valueOf(Class<T> enumType, String name)是在Enum中定义的static方法，它根据所给定的名字返回相应的enum实例，如果不存在给定名字的实例，将会抛出异常。

 

除了不能继承自一个enum之外，我们基本上可以将enum看作一个常规的类，也就是说，我们可以向enum中添加方法属性。

 

**public** **enum** OzWitch {

​       // enum实例必须定义在最前，并在方法之前:

​       *WEST*("west."), *NORTH*("north."),

​       *EAST*("east."), *SOUTH*("south.");//如果还有其他方法，则这个分号一定要写上

​      

​       **private** String description;//可以定义属性

 

​       // 注，枚举的构造函数只能是包访问或private访问权限:

​       **private** OzWitch(String description) {

​              **this**.description = description;

​       }

 

​       //也可以有方法

​       **public** String getDescription() {

​              **return** description;

​       }

 

​       //还可以有main方法

​       **public** **static** **void** main(String[] args) {

​              **for** (OzWitch witch : OzWitch.*values*())

​                     System.*out*.println(witch + ": " + witch.getDescription());

​       }

}

 

\>>>values()的神秘之处<<<

**public** **enum** Explore {

​       *HERE*, *THERE*

}

使用javap反编译后：

**public** **final** **class** Explore **extends** java.lang.Enum{

​    **public** **static** **final** Explore HERE;

​    **public** **static** **final** Explore THERE;

​    **public** **static** **final** Explore[] values();// 编译器自动添加的方法

​    **public** **static** Explore valueOf(java.lang.String);// 编译器自动添加的方法

​    **static** {};

}

Enum类没有values()方法，values()是由编译器为enum添加的static方法，在创建Explore的过程中，编译器还为它创建了valueOf(String name)方法，这可能有点怪了，Enum类不是已经有valueOf(Class<T> enumType, String name)了吗？不过Enum中的valueOf()方法需要两个参数，而这个新增的方法只需一个参数。

由于values()方法是由编译器插入到enum定义中的static方法，所以，如果你将enum实例向上转型为Enum，那么values()方法就不可访问了。不过，在Class中有一个getEnumConstants()方法，所以即使Enum接口中没有values()方法，我们仍然可以通过Class对象取得所有enum实例（注：只有是Enum类型的Class才能调用getEnumConstants方法，否则抛空指针异常）：

**enum** Search { *HITHER*, *YON* }

**public** **class** UpcastEnum {

  **public** **static** **void** main(String[] args) {

​    Search[] vals = Search.*values*();

​    Enum e = Search.*HITHER*; // 向上转型

​    // e.values(); // Enum中没有 values()方法

​    // 但我们可以通过Class对象的getEnumConstants()反射出enum实例

​    **for**(Enum en : e.getClass().getEnumConstants())

​      System.*out*.println(en);

  }

}

 

\>>>enum还可实现其它接口<<<

由于enum可以看是一个普通的类，所以他还可以实现其他接口。

**enum** CartoonCharacter **implements** Generator<CartoonCharacter> {

  *SLAPPY*, *SPANKY*, *BOB*;

  **private** Random rand = **new** Random(47);

  **public** CartoonCharacter next() {

​    **return** *values*()[rand.nextInt(*values*().length)];

  }

}

 

 

\>>>枚举实例的“多态”表现<<<

**enum** LikeClasses {

  *WINKEN* { **void** behavior() { System.*out*.println("Behavior1"); } },

  *BLINKEN* { **void** behavior() { System.*out*.println("Behavior2"); } },

  *NOD* { **void** behavior() { System.*out*.println("Behavior3"); } };

  **abstract** **void** behavior();//抽象方法，由每个enum实例实现，当然也可是一个实体方法

  **public** **static** **void** main(String[] args) {

​       **for**(LikeClasses en:*values*()){

​              en.behavior();//好比多态

​       }

  }

// 但不能把枚举实例看作是类，因为它们只是一个实例

  // void f1(LikeClasses.WINKEN instance) {}

}

经过反编译后，我们发现生成了四个类LikeClasses.class、LikeClasses$2.class、

LikeClasses$3.class、LikeClasses$1.class，而且LikeClasses$xx.class继承自LikeClasses.class类，并将各自的behavior方法代码放入到自己相应的类文件中。

 

 

# 第二十章注解

JavaSE5内置了三种标准注解，定义在java.lang中的注解：

@Override，表示当前的方法定义将覆盖超类中的方法。如果没有重写，编译器会发出错误提示。

@Deprecated，如果程序员使用了该注解注解过的元素，那么编译器会发出警告信息。

@SuppressWarnings，关闭不当的编译器警告信息。

 

元注解是负责注解其他的注解，有四种元注解：

@**Target** 表示该注解可以用于什么地方。可能的ElementType（枚举）参数包括：

CONSTRUCTOR：构造方法声明

FIELD：域声明（包括enum实例）

LOCAL_VARIABLE：局部变量声明

METHOD：方法声明

PACKAGE：包声明

PARAMETER：参数声明

TYPE：类，接口（包括注解类型）或enum声明

@**Retention** 表示需要在什么级别保存注解信息。可用的RetentionPolicy（枚举）参数包括：

SOURCE：注解将被编译地器丢弃。

CLASS：注解在class文件中可用，但会被VM丢弃。

RUNTIME：VM将在运行期也保留注解，因此可以通过反射机制读取注解的信息。

@**Documented** 将此注解包含在JavaDoc中。

@**Inherited** 允许子类继承父类中的注解。

 

Class、Method、Field、Constructor类都实现了AnnotatedElement接口，他们的getAnnotation方法都返回指定类型的注解对象，如果没有该类型的注解，则返回null值，以下是这些方法的原型：

class. getAnnotation(Class<T> annotationClass)

method. getAnnotation(Class<T> annotationClass)

field. getAnnotation(Class<T> annotationClass)

constructor. getAnnotation(Class<T> annotationClass)

 

注解的定义看起来很像接口的定义，事实上，与其他任何Java接口一样，注解也会编译成class文件。

 

 

**import** java.lang.annotation.ElementType;

**import** java.lang.annotation.Retention;

**import** java.lang.annotation.RetentionPolicy;

**import** java.lang.annotation.Target;

**import** java.lang.reflect.Method;

**import** java.util.ArrayList;

**import** java.util.Collections;

**import** java.util.List;

/*

\* 这是一个简单的注解，我们可以用它来跟踪一个项目中的用例。如果一个方法

\* 或一组方法实现了某个用例的需求，那么程序员可以为此方法加上该注解。

*/

 

//注解的定义

@Target(ElementType.*METHOD*)//该注解用于方法

@Retention(RetentionPolicy.*RUNTIME*)//注解信息保留到运行期

**public** **@interface** UseCase {

  **public** **int** id();//int型的元素

  **public** String description() **default** "no description";//String型的元素

}

 

//注解的使用

**class** PasswordUtils {

  @UseCase(id = 47, description = "密码必须至少包含一个数字")

  **public** **boolean** validatePassword(String password) {

​    **return** (password.matches("\\w*\\d\\w*"));

  }

  @UseCase(id = 48)//没有描述，使用默认的描述信息

  **public** String encryptPassword(String password) {

   **return** **new** StringBuilder(password).reverse().toString();

  }

  @UseCase(id = 49, description = "新密码不能使用以前使用过的密码")

  **public** **boolean** checkForNewPassword(

​    List<String> prevPasswords, String password) {

​    **return** !prevPasswords.contains(password);

  }

}

 

//注解处理器

**class** UseCaseTracker {

  **public** **static** **void**

  trackUseCases(List<Integer> useCases, Class<?> cl) {

​    **for**(Method m : cl.getDeclaredMethods()) {

​      // 通过反射获取某个方法特定的注解信息

​      UseCase uc = m.getAnnotation(UseCase.**class**);

​      **if**(uc != **null**) {

​        System.out.println("找到用例:" + uc.id() +

​          " " + uc.description());

​        useCases.remove(**new** Integer(uc.id()));

​      }

​    }

​    **for**(**int** i : useCases) {

​      System.*out*.println("警告: 所缺用例-" + i);

​    }

  }

  **public** **static** **void** main(String[] args) {

​    List<Integer> useCases = **new** ArrayList<Integer>();

​    Collections.*addAll*(useCases, 47, 48, 49, 50);

​    *trackUseCases*(useCases, PasswordUtils.**class**);

  }

}

/*

找到用例:47 密码必须至少包含一个数字

找到用例:48 no description

找到用例:49 新密码不能使用以前使用过的密码

警告: 所缺用例-50

*/

 

 

注解里的组成元素类型：

1、  所有基本类型

2、  String

3、  Class

4、  enum

5、  Annotation

6、  以上类型的数组

如果你使用了其他类型，那编译器就会报错。

 

注解组成元素的默认值限制：首先，元素不能有不确定的值，也就是说，元素必须要么具有默认值，要么在使用注解时提供元素的值，不允许即没给定默认值，在使用进也没指定值的情况出现。其次，对于非基本类型的元素，无论是在声明还是在使用时，都不能以null作为其值。

 

注解不支持继承。

# 第二十一章并发

Thread.yield()：对线程调度器（Java线程机制的一部分，可以将CPU从一个线程转移给另一个线程）的一种建议，它是在说“我已经执行完生命期中最重要的部分了，此刻正是切换给其他任务执行一段时间的大好时机”，即对线程的一种让步，暂停当前正在执行的线程，并执行其他线程。

 

\>>>使用Executor<<<

Java.util.concurrent包中的执行器（Executor）将为你管理Thread对象，从而简化了并发编程。

 

Executors：Executor、ExecutorService、ScheduledExecutorService、ThreadFactory 和 Callable 类的工厂和实用方法。

 

Executors.newCachedThreadPool()：创建一个可根据需要创建新线程的线程池，但是在以前构造的线程可用时将重用它们。对于执行很多短期异步任务的程序而言，这些线程池通常可提高程序性能。调用 `execute`将重用以前构造的线程（如果线程可用）。如果现有线程没有可用的，则创建一个新线程并添加到池中。终止并从缓存中移除那些已有 60 秒钟未被使用的线程。因此，长时间保持空闲的线程池不会使用任何资源。注意，可以使用 `ThreadPoolExecutor`构造方法创建具有类似属性但细节不同（例如超时参数）的线程池。

 

**public** **class** LiftOff **implements** Runnable {

  **protected** **int** countDown = 10; // Default

  **private** **static** **int** *taskCount* = 0;

  **private** **final** **int** id = *taskCount*++;

  **public** LiftOff() {}

  **public** LiftOff(**int** countDown) {

​    **this**.countDown = countDown;

  }

  **public** String status() {

​    **return** "#" + id + "(" + (countDown > 0 ? countDown : "Liftoff!") + "), ";

  }

  **public** **void** run() {

​    **while**(countDown-- > 0) {

​      System.*out*.print(status());

​      Thread.*yield*();

​    }

  }

}

 

**import** java.util.concurrent.ExecutorService;

**import** java.util.concurrent.Executors;

**public** **class** CachedThreadPool {

  **public** **static** **void** main(String[] args) {

​    ExecutorService exec = Executors.*newCachedThreadPool*();

​    **for**(**int** i = 0; i < 5; i++)

​      exec.execute(**new** LiftOff());

​    exec.shutdown();

  }

}

/*

\#2(9), #0(9), #4(9), #1(9), #3(9), #2(8), #0(8), #4(8), #1(8), #3(8), #2(7), #0(7), #4(7), #1(7), #3(7), #2(6), #0(6), #4(6), #1(6), #3(6), #2(5), #0(5), #4(5), #1(5), #3(5), #2(4), #0(4), #4(4), #1(4), #3(4), #2(3), #0(3), #4(3), #1(3), #3(3), #2(2), #4(2), #1(2), #3(2), #2(1), #4(1), #1(1), #3(1), #2(Liftoff!), #4(Liftoff!), #1(Liftoff!), #3(Liftoff!), #0(2), #0(1), #0(Liftoff!),

*/

 

 

Executors. newFixedThreadPool (int nThreads)：可一次性预先执行代价高昂的线程分配，因而也就可以限制线程数量。不用为每个任务都固定地付出创建线程的开销。

**import** java.util.concurrent.ExecutorService;

**import** java.util.concurrent.Executors;

**public** **class** FixedThreadPool {

  **public** **static** **void** main(String[] args) {

​    // Constructor argument is number of threads:

​    ExecutorService exec = Executors.*newFixedThreadPool*(5);

​    **for**(**int** i = 0; i < 5; i++)

​      exec.execute(**new** LiftOff());

​    exec.shutdown();

  }

}

/*

\#3(9), #4(9), #0(9), #2(9), #1(9), #3(8), #4(8), #0(8), #2(8), #1(8), #3(7), #4(7), #0(7), #2(7), #3(6), #4(6), #0(6), #2(6), #3(5), #4(5), #0(5), #2(5), #3(4), #4(4), #0(4), #2(4), #3(3), #4(3), #0(3), #2(3), #3(2), #4(2), #0(2), #2(2), #3(1), #4(1), #0(1), #2(1), #3(Liftoff!), #4(Liftoff!), #0(Liftoff!), #2(Liftoff!), #1(7), #1(6), #1(5), #1(4), #1(3), #1(2), #1(1), #1(Liftoff!),

*/

 

Executors. newSingleThreadExecutor (int nThreads)：确保任意时刻在任何都只有唯一的任务在运行，你不需要在共享资源上处理同步，可以让你省去只是为了维持某些事物的原型而进行的各种协调努力。

**public** **class** SingleThreadExecutor {

  **public** **static** **void** main(String[] args) {

​    ExecutorService exec =

​      Executors.*newSingleThreadExecutor*();

​    **for**(**int** i = 0; i < 5; i++)

​      exec.execute(**new** LiftOff());

​    exec.shutdown();

  }

}

/*

\#0(9), #0(8), #0(7), #0(6), #0(5), #0(4), #0(3), #0(2), #0(1), #0(Liftoff!), #1(9), #1(8), #1(7), #1(6), #1(5), #1(4), #1(3), #1(2), #1(1), #1(Liftoff!), #2(9), #2(8), #2(7), #2(6), #2(5), #2(4), #2(3), #2(2), #2(1), #2(Liftoff!), #3(9), #3(8), #3(7), #3(6), #3(5), #3(4), #3(3), #3(2), #3(1), #3(Liftoff!), #4(9), #4(8), #4(7), #4(6), #4(5), #4(4), #4(3), #4(2), #4(1), #4(Liftoff!),

*/

 

 

 

 

Thread.sleep(100);等同于TimeUnit.MILLISECONDS.sleep(100);

 

Daemom线程：后面线程不属于程序中不可缺少的部分，因此，当所有非后台线程结束时，程序也就终止了，同时会杀死进程中的所有后台线程。返过来说，只要有任何非后台线程还在运行，程序就不会终止。

 

必须在线程启动之前调用setDaemon()方法，才能把它设置为后台线程。

 

如果一个线程是后台线程，那么它创建的任何线程将被自动设置成后台线程。

 

后台线程在不执行finally子句的情况下就会终止其run()方法，即后台线程的finally子句不一定执行。

 

在构造器中启动线程可能会有问题，因为线程可能会在构造器结束之前开始执行，这意味着该线程能够访问处于不稳定状态的对象。

 

异常不能跨线程传播给main()，所以你必须在了本地处理所有在线程内部产生的异常。

**public** **class** ExceptionThread **implements** Runnable {

  **public** **void** run() {

​    **throw** **new** RuntimeException();

  }

  **public** **static** **void** main(String[] args) {

​        **try** {

​          ExecutorService exec =

​            Executors.*newCachedThreadPool*();

​          exec.execute(**new** ExceptionThread());

​        } **catch**(RuntimeException ue) {

​          // 这句将不会被执行，因为线程的异常是不会传递到调用它的线程的

​          System.*out*.println("Exception has been handled!");

​        }

​      }

}

 

Thread.UncaughtExceptionHandler是Java SE5中的新接口，它允许你在每个Thread对象上都附着一个异常处理器。Thread.UncaughtExceptionHandler.uncaughtException()会在因未捕获的异常而临近死亡时被调用。为了使用它，我们创建了一个新类型的ThreadFactory，它将在每个新创建的Thread对象上附着一个Thread.UncaughtExceptionHandler，并将这个工厂传递给Exceutors创建新的ExcecutorService方法：

// 线程

**class** ExceptionThread2 **implements** Runnable {

  **public** **void** run() {

​    Thread t = Thread.*currentThread*();

​    System.*out*.println("run() by " + t);

​    System.*out*.println("1.eh = " + t.getUncaughtExceptionHandler());

​    **throw** **new** RuntimeException();//线程运行时一定会抛出运行异常

  }

}

 

// 线程异常处理器

**class** MyUncaughtExceptionHandler **implements**

Thread.UncaughtExceptionHandler {

  // 异常处理方法

  **public** **void** uncaughtException(Thread t, Throwable e) {

​    System.*out*.println("caught " + e);

  }

}

 

// 线程工厂，创建线程时会调用该工厂

**class** HandlerThreadFactory **implements** ThreadFactory {

  **public** Thread newThread(Runnable r) {//线程创建工厂方法

​    System.*out*.println(**this** + " creating new Thread");

​    Thread t = **new** Thread(r);

​    System.*out*.println("created " + t);

​    //设置异常处理器

​    t.setUncaughtExceptionHandler(**new** MyUncaughtExceptionHandler());

​    System.*out*.println("2.eh = " + t.getUncaughtExceptionHandler());

​    **return** t;

  }

}

 

**public** **class** CaptureUncaughtException {

  **public** **static** **void** main(String[] args) {

​    ExecutorService exec = Executors.*newCachedThreadPool*(

​      **new** HandlerThreadFactory());

​    exec.execute(**new** ExceptionThread2());

  }

} /*

HandlerThreadFactory@1a758cb creating new Thread

created Thread[Thread-0,5,main]

2.eh = MyUncaughtExceptionHandler@69b332

run() by Thread[Thread-0,5,main]

1.eh = MyUncaughtExceptionHandler@69b332

caught java.lang.RuntimeException

*/

 

 

如果你知道将要在代码中处处使用相同的异常处理器，那么更简单的方式是在Thread类中设置一个静态域，并将这个处理器设置为默认的未捕获异常处理器：Thread.setDefaultUncaughtExceptionHandler(**new** MyUncaughtExceptionHandler());

 

\>>>使用Lock对象<<<

Lock对象必须被显式地创建、锁定和释放。因此，它与内建的锁形式相比，代码缺乏优雅性，但对于解决某些类型的问题来说，它更加灵活。

  **private** Lock lock = **new** ReentrantLock();

  **public** **int** next() {

​    lock.lock();

​    **try** {

​      //…

​    } **finally** {

​      lock.unlock();

​    }

  }

使用lock()和unlock()方法在next()内部创建了临界资源。还可以尝试获取锁：

  **private** ReentrantLock lock = **new** ReentrantLock();

  **public** **void** untimed() {

​    **boolean** captured = lock.tryLock();

​    **try** {

​      //…

​    } **finally** {

​      **if**(captured)

​        lock.unlock();

​    }

  }

 

\>>>使用volatile对象<<<

原子操作是不能被线程调试机制中断的操作，一旦开始操作，那么它一定会在切换到其他线程前执行完毕。

 

原子操作可以应用于除long和double之外的所有基本类型之上的“简单操作”，对于读取和写入除long和double之外的基本类型变量这种的操作，可以保证它们会被当作原子操作来操作内存。但是JVM可以将64位（long和double变量）的读取和写入当作两个分离的32位操作来执行，这就可能会产生了在一个读取和写入操作中间切换线程，从而导致不同的线程看到不正确结果的可能性。但是，当你定义long或double变量时，如果使用volatile关键字，就会获得（简单的赋值与返回操作）原子性，注：在Java SE5之前，volatile一直未能正确的工作。

 

volatile关键字还确保了应用中的可视性，如果你将一个域声明为volatile的，那么只要对这个域产生了写操作，那么所有的读操作就都可以看到这个修改，即便使用了本地缓存，情况确实如此，volatile域会立即被写入到主存中。

 

在非volatile域上的操作没有刷新到主存中去，因此其他读取该域的线程将不能必看到这个新值。因此，如果多个线程同时访问了某个域，那么这个域就应该是volatile的，否则，这个域应该只能由同步来访问，同步也会导致向主存中刷新，因此如果一个域完全由synchronized方法或语句块来保护，那就不必将其设置为volatile了。

 

什么才属于原子操作呢？对域中的值做赋值和返回操作都是原子性的。但i++; i+=2; 这样的操作肯定不是原子性的，即线程有可能从语句的中间切换。下面来证明i++在java里不是原子性操作的：

**class** SerialNumberGenerator {

​       **private** **static** **volatile** **int** *serialNumber* = 0;

 

​       **public** **static** /* synchronized */**int** nextSerialNumber() {

​              // 不是线程安全，因为i++在Java里不是原子操作，

​              // 即使將serialNumber设置成了volatile

​              **return** *serialNumber*++;

​       }

}

 

**class** CircularSet {

​       **private** **int**[] array;

​       **private** **int** len;

​       **private** **int** index = 0;

 

​       **public** CircularSet(**int** size) {

​              array = **new** **int**[size];

​              len = size;

​              // 初始化为-1

​              **for** (**int** i = 0; i < size; i++) {

​                     array[i] = -1;

​              }

​       }

 

​       **public** **synchronized** **void** add(**int** i) {

​              array[index] = i;

​              // 如果数组满后从头开始填充，好比循环数组:

​              index = ++index % len;

​       }

 

​       **public** **synchronized** **boolean** contains(**int** val) {

​              **for** (**int** i = 0; i < len; i++) {

​                     **if** (array[i] == val) {

​                            **return** **true**;

​                     }

​              }

​              **return** **false**;

​       }

}

 

**public** **class** SerialNumberChecker {

​       **private** **static** **final** **int** *SIZE* = 10;

​       **private** **static** CircularSet *serials* = **new** CircularSet(1000);

​       **private** **static** ExecutorService *exec* = Executors.*newCachedThreadPool*();

 

​       **static** **class** SerialChecker **implements** Runnable {

​              **public** **void** run() {

​                     **while** (**true**) {

​                            **int** serial = SerialNumberGenerator.*nextSerialNumber*();

​                            **if** (*serials*.contains(serial)) {// 如果数组中存在则退出

​                                   System.*out*.println("Duplicate: " + serial);

​                                   System.*exit*(0);

​                            }

​                            *serials*.add(serial);// 如果不存在，则放入

​                     }

​              }

​       }

 

​       **public** **static** **void** main(String[] args) **throws** Exception {

​              SerialChecker sc = **new** SerialChecker();

​              // 启动10线程

​              **for** (**int** i = 0; i < *SIZE*; i++) {

​                     *exec*.execute(sc);

​              }

​       }

}

 

 

 

 

**public** **class** Increament **extends** Thread {

​       **public** **static** **volatile** **int** *x* = 0;

 

​       **public** **void** run() {

//            synchronized (Increament.class) {

​                     // x++与 x = x + 1都不是原子操作

​                     x++;

​                     // *x* = *x* + 1;

//            }

​       }

 

​       **public** **static** **void** main(String[] args) **throws** Exception {

​             

​              Thread threads[] = **new** Thread[10000];

​              **for** (**int** i = 0; i < threads.length; i++) {

​                     threads[i] = **new** Increament();

​              }

 

​              **for** (**int** i = 0; i < threads.length; i++) {

​                     threads[i].start();

​              }

​              **for** (**int** i = 0; i < threads.length; i++) {

​                     // 等待计算线程运行完

​                     threads[i].join();

​              }

​              System.*out*.println("n=" + Increament.*x*);

​       }

}

如果对x的操作是原子级别的，最后输出的结果应该为x=10000，而在执行上面积代码时，很多时侯输出的x都小于10000，这说明x++ 不是原子级别的操作。原因是声明为volatile的简单变量如果当前值由该变量以前的值相关，那么volatile关键字不起作用。

 

 

同一时刻只有一个线程能访问synchronized块，synchronized块并不是一下子要执行完毕，CPU调试可能从synchronized块中的某个语句切换到其它的线程，再其它线程执行完毕后再继续执行该同步块。切换到其他线程时是否释放synchronized块上的锁，这要看切换所采用的方式：如果是CPU自动或调用Thread.yeild切换，则不会释放；如果是调用wait，则会释放；如果是调用的Thread.sleep，则不会；如果是调用的thread.join，则要看synchronized块上的锁是否是thread线程对象，如果不是，则不会释放，如果是，则会释放。

 

只能在同步控制方法或同步控制块里调用wait()、notify()和notifyAll()，并且释放操作锁，但sleep()可以在非同步控制方法里调用，不会释放锁。

 

sleep、yield都是Thread的静态方法，join属于Thread的非静态方式，如果将它们放入在同步块中调用时都不会释放锁。但wait属于Object类的方法，在wait()期间对象锁是释放的。

 

在执行同步代码块的过程中，遇到异常而导致线程终止，锁会释放。

 

执行线程的suspend()方法会导致线程被暂停，并使用resume()可唤醒，但不会释放锁。

 

当线程在运行中执行了Thread类的yield()静态方法，如果此时具有相同优先级的其他线程处于就绪状态，那么yield()方法将把当前运行的线程放到可运行池中并使用中另一线程运行。如果没有相同优先级的可运行进程，则该方法什么也不做。

 

sleep方法与yield方法都是Thread类的静态方法，都会使当前处于运行的线程放弃CPU，把运行机会让给另的线程。两都的区别：

\1.         sleep方法会给其他线程运行的机会以，而不考虑其他线程的优先级，因此会给较低优先级线程一个运行的机会；yield方法只会给相同或更高优先级的线程一个运行的机会。

\2.         当线程执行了sleep方法后，将转到阻塞状态。当线程执行了yield方法后，将转入就绪状态。

\3.         Sleep方法比yield方法具有更好的可移植性。不能依靠yield方法来提高程序的并发性能。对于大多数程序员来说，yield方法的唯一用途是在测试期间人为地提高程序的并发性能，以帮助发现一些隐藏的错误。

 

thread.join()：当前线程调用另一线程thread.join()时，则当前运行的线程将转到阻塞状态，并且等待thread线程运行结束后，当前线程程才会恢复运行（从阻塞状态到就绪状态）。比如有3个线程在执行计算任务，必须等三个线程都执行完才能汇总，那么这时候在主线程里面让三个线程join，最后计算结果既可：

**public** **class** JoinTest {

​       **public** **static** **void** main(String[] args) {

​              Rt[] ct = **new** Rt[3];

​              **for** (**int** i = 0; i < ct.length; i++) {

​                     ct[i] = **new** Rt();

​                     ct[i].start();

​                     **try** {

​                            //主线等待三个线程终止后再继续运行

​                            ct[i].join();

​                     } **catch** (InterruptedException e) {

​                            e.printStackTrace();

​                     }

​              }

​              **int** total = 0;

​              **for** (**int** j = 0; j < ct.length; j++) {

​                     total += ct[j].getResult();

​              }

​              System.*out*.println("total = " + total);

​       }

}

 

**class** Rt **extends** Thread {

​       **private** **int** result;

​       **public** **int** getResult() {

​              **return** result;

​       }

​       **public** **void** run() {

​              **try** {

​                     Thread.*sleep*(1000);

​                     result = (**int**) (Math.*random*() * 100);

​                     System.*out*.println(**this**.getName() + " result=" + result);

​              } **catch** (InterruptedException e) {

​                     e.printStackTrace();

​              }

 

​       }

}

 

join()只能由线程实例调用，如果thread.join()在同步块中调用，并且同步锁对象也是thread对象，由于thread.join()是调用thread.wait()来实现的，wait会释放thread对象锁，则thread.join()与在同步块的锁也会一并释放；如果thread.join()在同步块的锁对象不是thread对象，则thread线程阻塞时不会释放锁：

**public** **class** JoinTest {

​       **public** **static** **void** main(String[] args) **throws** InterruptedException {

​              JThread t = **new** JThread();

​              *start*(t, t);

​              System.*out*.println("--------");

​              t = **new** JThread();

​              *start*(t, JThread.**class**);

​       }

 

​       **static** **void** start(JThread t, Object lock) {

​              t.setLock(lock);

​              t.start();

​              **try** {

​                     Thread.*sleep*(100);

​              } **catch** (InterruptedException e) {

​                     e.printStackTrace();

​              }

​              //如果锁对象是JThread.class时，则主线程会一直阻塞

​              t.f();

​       }

}

 

**class** JThread **extends** Thread {

​       **private** Object lock;

 

​       **void** setLock(Object lock) {

​              **this**.lock = lock;

​       }

 

​       **public** **void** run() {

​              **synchronized** (lock) {

​                     **try** {

​                            System.*out*.println(Thread.*currentThread*().getName() + " - join before");

​                            /*

​                             \* 当前线程阻塞，又要等待自己运行完，这是矛盾的，所以其实该线程永远不会恢复执

​                             \* 行，除非使用 join(long millis)方式。实际上我们看this.join()源码就会

​                             \* 看出，this.join()就是调用了this.wait()方法，因为了this.wait()会释放

​                             \* this对象上的锁，所以当lock对象是自身时，主线程不会被锁住，所以第一个线程

​                             \* 会打印 "main - f()"。第二个线程的锁对象是JThread的Class对象，由于join

​                             \* 时不会释放JThread.class对象上的锁， 第二个线程会一直阻塞，所以第二个线程

​                             \* 不会打印 "main - f()"，

​                             *

​                             */

​                            **this**.join();

​                            /*

​                             \* 这样可以正常结束整个程序，因为this线程一直会阻塞直到对方（也是this的线程）运行完

​                             \* 或者是对方没有运行完等 1 毫秒后thsi线程继续运行，所以以这样的方式一定不会出现死锁

​                             \* 现象

​                             */

​                            //this.join(1);

​                            System.*out*.println(Thread.*currentThread*().getName() + " - join after");

​                     } **catch** (InterruptedException e) {

​                            e.printStackTrace();

​                     }

​              }

​       }

 

​       **public** **void** f() {

​              **synchronized** (lock) {

​                     System.*out*.println(Thread.*currentThread*().getName() + " - f()");

​              }

​       }

}

 

sleep与join都会使当前线程处于阻塞状态，而yield则是进行就绪状态。

 

同步的静态方法的锁对象是方法所属类的Class对象，而同步的非静态方法的锁对象是所调用方法实例所对应的this对象。

 

**继承Runnable****与Thread****的区别**：Thread类本身也实现了Runnable接口。 因此除了构造 Runnable对象并把它作为构造函数的参数传递给Thread类之外，你也可以生成Thread类的一个子类，通过覆盖这个run方法来执行相应的操作。不过，通常最好的策略是把Runnable接口当作一个单独的类来实现，并把它作为参数传递给个Thread的构造函数。通过将代码隔离在单独的类中可以使你不必担心Runnable类中使用的同步方法和同步块与在相应线程类中所使用的其他任何方法之间的潜在操行所带来的影响。更一般地说，这种分离允许独立控制相关的操作和运行这些操作的上下文，同一个Runnable对象既可以传递给多个使用不同方式初抬化的Thread对象，也可以传递给其他的轻量级执行者（executor）。同样需要注意的是，继承了Thread类的刘象不能再同时继承其他类了。

 

如果线程被启动并且没有终止，调用方法isAlive将返回true。如果线程仅仅是因为某个原因阻塞，该方法也会返回true。

 

通过调用线程t的join方法将调用者挂起，直到目标线程t结束运行：t.join方法会在当t.isAlive方法的结果为false时返回。

 

有一些Thread类的方法只能应用于当前正在运行的那个线程中（也就是，调用Thread静态方法的线程），为了强制实施，这些方法都被声明为static：Thread.currentThread、Thread.interrupted、Thread.sleep、Thread.yield。

 

Thread.yield：仅仅是一个建议——放弃当前线程去运行其他的线程，JVM可以使用自己的方式理解这个建议。尽管缺少保证，但yield方法仍旧可以在一些单CPU的JVM实现上起到相应的效果，只要这些实现不使用分时抢占式的调用机制，在这种机制下，只有当一个线程阻塞时，CPU才会切换到其他线程上执行。如果在系统中线程执行了耗时的非阻塞计算任务的会占有更多的CPU时间，因而降低了应用程序的响应，为了安全起见，当执行非阻塞的计算任务的方法时，则可以在执行过程中插入yield方法（甚至是sleep方法）。为了减少不必要的影响，可以只在偶尔的情况下调用yield方法，比如一个包含如下语句的循环：

if(Math.random() < 0.01) Thread.yield();

使用抢占式调度机制的JVM实现，特别是在多处理器的情况下，yield才可能显得没有什么意义。