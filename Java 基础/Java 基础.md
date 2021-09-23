# Java 基础

这里主要记录 Java 基础，关于多线程编程的相关知识在并发编程笔记中。

Java 可以简单的认为是去除指针 C++。

它主要有如下特点：

* 跨平台性
* 面向对象
* 安全性
* 多线程
* 简单易用

Java 是一个完全面向的对象的语言，而面向对象的语言具有如下三大特性：

* 封装
* 继承
* 多态

这里的记录的笔记主要基于 JDK8，它具有如下新特性：

* Lambda表达式
* 函数式接口
* *方法引用和构造器调用
* Stream API
* 接口中的默认方法和静态方法
* 新时间日期API

同时要注意一个细节，JDK8 后 HashMap 引入了红黑树。

## 基础

### 变量

变量名以字符、下划线开头，由字符、数字、下划线组成的字符串。

不可以与保留字相同。


### 关键字

关键字又称保留字。这里主要记录一些重要的保留字，其它的保留字会在后面的笔记中记录。

#### final

可以用来修饰引用、方法和类。可以简单的理解为表示不可变、常量的意思。

修饰引用时：

* 如果引用为基本数据类型，则该引用为常量，该值无法修改；
* 如果引用为引用数据类型，比如对象、数组，则该对象、数组本身可以修改，但指向该对象或数组的地址的引用不能修改。
* 如果引用为类的成员变量，则必须当场赋值，否则编译会报错。

修饰一个方法时：

* 当使用 final 修饰方法时，这个方法将成为最终方法，无法被子类重写。但是，该方法仍然可以被继承。

修饰类时：

* 当用 final 修改类时，该类成为最终类，无法被继承。简称为“断子绝孙类”。


#### static

可以修饰变量、方法、代码块和类。可以理解为表示全局的意思。

与 final 组合在一起修饰一个类中的属性时，表示该属性是类的全局常量。

static 方法：

* 静态方法，由于静态方法不依赖于任何对象就可以进行访问（可以通过类名直接调用），因此对于静态方法来说，是没有 this 的，因为它不依附于任何对象，既然都没有对象，就谈不上 this 了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。
* 需要注意，在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。

static 变量：

* 称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。
* static 成员变量的初始化顺序按照定义的顺序进行初始化。
* 静态变量可以通过类名直接调用。

static 代码块：

* 静态代码块可以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

static 类：

* 这种用法要点很少，几乎只用于静态内部类。


#### abstract

可以修饰方法、类。表示该方法或类是抽象的，没有具体的实现。

抽象方法：

* 它没有自己的主体（没有 `{}` 包起来的业务逻辑），跟接口中的方法一样。所以不能直接调用抽象方法。
* 抽象方法不能用 `private` 修饰，因为抽象方法必须被子类实现（重写），而 `private` 权限对于子类来说是不能访问的，所以就会产生矛盾。但是抽象类是没有限制访问修饰符的。
* 抽象方法也不能用 `static` 修饰，如果用 `static` 修饰了，那么就可以直接通过类名调用，而抽象方法压根就没有主体，没有任何业务逻辑，这样就毫无意义。

抽象类：

* 表达形式为：`（public）abstract class 类名{}`
* 抽象类不能被实例化，也就是说我们没法直接 new 一个抽象类。抽象类本身就代表了一个类型，无法确定为一个具体的对象，所以不能实例化就合乎情理了，只能有它的继承类实例化。
* 抽象类虽然不能被实例化，但有自己的构造方法。
* 抽象类与接口（interface）有很大的不同之处，接口中不能有实例方法去实现业务逻辑，而抽象类中可以有实例方法，并实现业务逻辑，比如我们可以在抽象类中创建和销毁一个线程池。同时接口的属性默认为 `public static final`，而抽象类没有限制。
* 抽象类不能使用 `final` 关键字修饰，这样的类是无法被继承，而对于抽象类来说就是需要通过继承去实现抽象方法，这会产生矛盾。

抽象类与抽象方法的关联：

* 如果一个类中至少有一个抽象方法，那么这个类一定是抽象类，但反之则不然。也就是说一个抽象类中可以没有抽象方法。这样做的目的是为了此类不能被实例化。
* 如果一个类继承了一个抽象类，那么它必须全部覆写抽象类中的抽象方法，当然也可以不全部覆写，如果不覆写全部抽象方法则这个子类也必须是抽象类。


### 传值和传引用

[Java 到底是值传递还是引用传递？](https://www.zhihu.com/question/31203609/answer/50992895)

简单的来所，在函数调用时，参数传递就是一个赋值的过程。所以所有的传引用都是传值。只不过，对于基本数据类型传递的是变量代表的值；而对于引用数据类型（包括数组）传递的是引用地址的值。


### 数据类型

#### 基本数据类型

| 数据类型 | 字节数 | 最小值 |  最大值   | 默认值 |      备注       |
| :------: | :----: | :----: | :------: | :----: | :-------------: |
|   byte   |   1    |  -2^7  |  2^7-1   |   0    |                 |
| boolean  |   1    |        |          | false  |                 |
|   char   |   2    | u0000  |  uffff   | u0000  |                 |
|  short   |   2    | -2^15  | 2^15 - 1 |   0    |                 |
|   int    |   4    | -2^31  | 2^31 - 1 |   0    |    最大约21亿    |
|   long   |   8    | -2^63  |  -2^63   |   0L   |                 |
|  float   |   4    |        |          |  0.0f  | 不能表示精确的值 |
|  double  |   8    |        |          |  0.0d  | 不能表示精确的值 |

基本数据类型不能赋予 null。

初始化一个基本类型数组时，会赋予默认值。

char 的默认值为 Unicode 编码中的 u0000，查 Unicode 编码表得出，\u0000 的编码值为 NUL。这个 Unicode 码转为字符串后就是""（空字符）。所以控制台输出结果就是""（空字符）。也即官方文档中说明的 Default Values（缺省值）。


#### 引用数据类型

也就是类的实例。

默认值都为null。

所有类都是 Object 类的子类。


#### null

所有赋值为 null 的对象，可以认为其指向同一个空间。

null 具有以下 9 个要点：

* null 是 java 中的关键字，像 public、static、final。它是大小写敏感的，你不能将 null 写成 Null 或 NULL，编译器将不能识别他们然后报错。
* null 是任何引用类型的默认值，不严格的说是所有 object 类型的默认值。这对所有变量都是适用的，如成员变量、局部变量、实例变量、静态变量。
* null 既不是对象也不是一种类型，它仅是一种特殊的值，你可以将其赋予任何引用类型，你也可以将 null 转化成任何类型。
* null 可以赋值给引用变量，你不能将 null 赋给基本类型变量，例如 int、double、float、boolean。如果你那样做了，编译器将会报错；但是如果将 null 赋值给包装类object，然后将 object 赋给各自的基本类型，编译器不会报错。
* 任何含有 null 值的包装类在 Java 拆箱生成基本数据类型时候都会抛出一个空指针异常。
* 如果使用了带有 null 值的引用类型的变量，instanceof 操作会返回 false。
* 不能调用非静态方法来使用一个值为 null 的引用类型变量。它将会抛出空指针异常；可以使用静态方法来使用一个值为 null 的引用类型变量。因为静态方法使用静态绑定，不会抛出空指针异常。
* 可以将 null 传递给方法使用，这时方法可以接收任何引用类型。例如 `public void print(Object obj)` 可以这样调用 `print(null)`。从编译角度来看这是可以的，但结果完全取决于方法。null 安全的方法，如在这个例子中的 print 方法，不会抛出空指针异常，只是优雅的退出。如果业务逻辑允许的话，推荐使用 null 安全的方法。
* 可以使用 `==` 或者 `!=` 操作来比较 null 值，但是不能使用其他算法或者逻辑操作，如大于、小于。与 SQL 不同，Java中的 `null==null` 会返回 `true`。

总而言之，记住，null 是任何一个引用类型变量的默认值，在 Java 中你不能使用 null 引用来调用任何的 instance 方法或者 instance 变量。

null 的其他用途和注意事项：

* 释放内存，让一个非 null 的引用类型变量指向 null。这样这个对象就不再被任何对象应用了。等待 JVM 垃圾回收机制去回收。
* 集合类中使用 null 需要注意：
    * List：允许重复元素，可以加入任意多个 null。(这里添加的对象如果为空，拿出来用属性的时候就要注意非空验证了)
    * Set：不允许重复元素，最多可以加入一个 null。
    * Map：Map 的 key 最多可以加入一个 null，value 字段没有限制。


### 数组

使用连续内存空间存储某一数据类型。

它在 JVM 是作为一个对象存储在堆空间的。

数组可以存储在集合类中。

可以使用迭代器：`Iterator<int> iterator = Arrays.stream(int).iterator();`

对于基本数据类型的数组，初始化时默认值是该数据类型对应的默认值；对于引用数据类型，初始化时默认为 null。

基本数据类型数组是不能赋值 null 的，如果一定要使用 null，那么使用对应的包装类数组。

#### 工具类 Arrays

|               方法                |        说明         |
| -------------------------------- | ------------------- |
| fill(int[] arr, int value)       | 使用指定元素填充数组 |
| asList(new LinkedList(int size)) | 转换为集合类         |


#### 对象数组

对象数组内的每个元素都是一个引用值。默认值为 null，其构造函数不会执行，需要一个手动 new 才会执行构造函数。

```java
// Bucket是一个类
Bucket[] buckets = new Bucket[10];
for(int i = 0; i < buckets.length; i++)
{
    buckets[i] = new Bucket();
}
```


#### 复制数组

Java 中数组复制的方式，有以下几种：

* `System.arraycopy()`：这是一个本地方法
* `clone()`：由 Object 定义
* `Arrays.copyOf()`；
* for 循环

四者效率如下依次递减。

需要注意：

* 原始数组长度不管是多少的时候，`Arrays.copyOf()` 的效率都比 `System.arraycopy()` 差。
* 原始数组长度比较小的时候，几百以内，for 循环表现十分优异，并随着数组长度的增加，效率越来越低，因此，for 循环适合于小型数组。
* 原始数组长度中等的时候，比如几千的时候，两个本地方法的效率差不多。
* 原始数组长度比较大的时候，以万为单位，这时候本地方法 `System.arraycopy()` 方法的优势体现出来了，力压其他三种方式。

因此，需要根据操作的数组的长度，灵活地选择数组复制方式，会使得我们的程序得到性能的略微提升。


### 数的二进制表示

这个问题也可以回答，为什么 int 型的取值范围是$2^{32}-1$~$-2^{32}$。

在 Java 中，最高位为符号位。正数由原码表示，负数由补码表示。所以 int 型最大的正数的原码表示为 01111111111111111111111111111111，最小的负数的原码表示为 11111111111111111111111111111111，补码表示为 10000000000000000000000000000001。

原码转补码：符号位不变，数值位按位取反,末位再加 1。

补码转原码：符号位不变，数值位按位取反,末位再加 1，即补码的补码等于原码。


### 运算符

数值运算符：

* 加 +
* 减 -
* 乘 *
* 除 /
* 取余 %

逻辑运算符：

* 与 &&
* 或 ||
* 非 !

逻辑运算符与和或存在短路现象。

位运算符：

| 运算符 |             说明             |
| ------ | ---------------------------- |
| &      | 所有位按位与，全1才1           |
| \|     | 所有位按位或，有1就1           |
| ~      | 所有位按位非                  |
| ^      | 所有为按位异或，相同为0不同为1 |
| >>     | 符号位不动，逻辑右移，高位补0  |
| <<     | 符号位不动，逻辑左移，低位补0  |
| >>>    | 无符号逻辑右移                |
| <<<    | 无符号逻辑左移                |

位运算符之间的关系：



### 逻辑运算符短路

```java
boolean[] dp = new boolean[1];
dp[0] = true;
System.out.println(dp[0] || dp[-1]);
```

上述代码是可以正常运行的，只要前一个为true，后一个不会判断，即使越界也行。

同样，对于&&，只要前一个为false，后一个不会判断，即使越界也行。


### 问号冒号表达式

A：布尔表达式(真/假)，B：执行语句 ，C ：执行语句

最直观的表达是： A ？ B ：C （如果 A 为真执行 B，否则执行 C）。


## 面对对象

类：对现实中事务的抽象，包含属性和操作。

对象：类的一个实例，一般使用 new 关键字获取，也可以通过反射获取。

### 构造函数

这里主要考虑子类与父类构造函数的关系：

* 如果没有自定义构造函数，编译器会自动生成一个无参的构造函数。
* 子类构造函数中最开始要使用`super(this)`或`super(参数列表)`显示调用父类构造函数，如果没有显示调用，编译器会自动调用父类的无参构造函数。这样定义是为了防止父类中的某些变量未初始化。
* 如果自定义了一个类的有参构造函数，而没有定义无参构造函数，在泛化这个类时，子类的构造函数会报错。原因是，自定义了构造函数，编译器就不会再自动生成无参构造函数，此时由于没有自定义无参构造函数，子类构造函数中就无法使用父类的无参构造函数，而这又是必须的。解决方法是，如果自定义了构造函数，就一定要记得自定义一个无参的构造函数；或者在子类中显式地使用父类的有参构造函数。


### 访问修饰符

内部类对外部类是完全可见的，外部类对内部类也相同。

|   作用域   | 当前类 | 同包 | 子类 | 其他包 |
| --------- | ------ | ---- | ---- | ------ |
| public    | ✔     | ✔   | ✔   | ✔     |
| protected | ✔     | ✔   | ✔   | ✖     |
| default   | ✔     | ✔   | ✖   | ✖     |
| private   | ✔     | ✖   | ✖   | ✖     |

可以将上表理解为，一个类的某个对象，它的方法或属性的可见域。实际上应该更加严谨的阐述（以 protected 为例）：

* 基类的 protected 成员是包内可见的，并且对子类可见；
* 若子类与基类不在同一包中，那么在子类中，子类实例可以访问其从基类继承而来的 protected 方法，而不能访问基类实例的 protected 方法。

[详细阐述](https://www.runoob.com/w3cnote/java-protected-keyword-detailed-explanation.html)


### 匿名内部类

适用于只使用一次的场景

```
PriorityQueue<Integer> queue = new PriorityQueue(new Comparator<Integer>
{
    @Override
    public int compare(Integer o1, Integer o2)
    {
        // 倒序
        return o2-o1;
    }
});
```

### 静态内部类

不能在静态内部类中写抽象方法。

用于外部类会使用内部类的属性方法，而内部类不会使用外部类的属性。

* 外部类可以通过实例化内部类使用其非静态方法和变量，可以直接使用其静态方法和变量。
* 内部类可以通过实例化外部类使用其非静态方法和变量，可以直接使用其静态方法和变量。
* 内部类和外部类互相具有全部权限。
* 非外部类可以不实例化外部类而使用内部类。`A.B b = new A.B();`

代码示例如下：

```
public class A
{
    private static B
    {}
}
```


## 接口与继承

在 Java 中，为了防止滥用泛化关系，类是不支持多继承的，但有时又必须使用到多继承，于是接口应运而生。一个类可以实现多个接口。

接口是一种特殊的类，它可以包含属性，也可以声明方法，但有如下要点：

* 接口的访问修饰符默认是 public，且不能使用其它访问修饰符。
* 接口中只能声明公有的静态常量，且必须当场赋值。
* 接口中可以声明方法，但不能有方法体，也就是方法后面不能有 `{}`；接口中的方法默认是 public，且不能使用其它访问修饰符。
* 一个类如果实现某个接口，必须实现它的全部方法。

基于上面这些性质，在编写接口时，最佳实践是无论属性、方法还是接口本身都不要使用访问修饰符号。

继承是面对对象编程的三个核心概念之一，它具有如下性质：

* 子类对象可以被赋值给父类对象。
* 子类可以覆盖可见的父类同名属性。
* 子类可以重写父类方法；但注意不存在重载父类方法的概念。


### 接口与抽象类

抽象类是使用`abstruct`修饰的类，它具有如下性质：

* 不能被实例化，只能被继承。
* 可以声明实例方法，但抽象方法不有方法体，即不能有 `{}`。抽象方法默认是 public 的，且可以是 protected 的。
* 它可以想普通类那样声明属性。
* 一个类如果继承了某个抽象类，要实现它的所有抽象方法，否则这个子类也会变成抽象类。

注意类的实例方法必须无论是否有返回值都必须加 `{}`。

抽象类与接口是两个非常相似的概念，它们的差异同如下：

|    性质    |            接口            |                          抽象类                          |
| ---------- | -------------------------- | ------------------------------------------------------- |
| 属性       | 只能声明全局常量            | 与普通类相同                                             |
| 方法       | 只能是公有的                | 可以有实例方法，实例方法没有限制；抽象方法只能是公有或保护的 |
| 访问修饰符 | 默认是公有的，且只能是公有的 | 与普通类相同                                             |


## 多态

### 运行时多态

通过方法子类重写父类方法实现。


### 编译时多态

通过方法重载实现。只有参数列表不同才是正确的重载，返回值不同或者访问等级不同不能重载，会报错。


## 对象引用模型

对象引用模型应该算是 JVM 相关的知识，但对于理解 Java 中的引用体系有帮助。

大致可以描述为，栈中存放对象的引用、堆存放对象的实例。类的模板放方法区；静态变量和全局变量放常量池。局部变量和函数参数放栈的栈帧的局部变量表。

堆中对象的对象头记录了对象的类型信息、锁信息、偏向对象、GC 情况、GC 代、hashcode 等。

下面通过一段代码来理解对象的内存结构：

```
public class c1
{
    public static void fun1(int i)
    {
        // 放入栈空间
        int a = 1;
        // 调用方法区的类模板；创建一个为"s"的String常量实例放入常量池，在栈中创建一个引用指向堆中的实例。
        String s1 = new String("s");
        // 创建一个为"s2"的String常量实例放入常量池；在栈中创建一个引用指向常量池。
        String s2 = "s2";
        // 在栈中创建一个引用指向s1指向的堆中的实例。
        String s3 = s1;
    }

    public static void main(String[] args)
    {
        // 在栈中创建一个栈帧，存放方法的引用以及局部变量a，参数i。同时方法名动态链接到方法区的该类的该方法。
        c1.fun1(2);
        // 传参实际就是赋值，一切传引用都是传值。
    }
}

```


## 枚举

所有的枚举都继承自 java.lang.Enum 类。


## 常用类

### String、StringBuilder、StringBuffer


String：采用 final 修饰的字符数组进行字符串保存，因此不可变。如果对 String 类型对象修改，需要新建对象，将老字符和新增加的字符一并存进去。

StringBuilder：采用无 final 修饰的字符数组进行保存，因此可变。但线程不安全。

StringBuffer：采用无 final 修饰的字符数组进行保存，可理解为实现线程安全的 StringBuilder。

#### 常用方法

String：

|              方法               |                 说明                  |
| ------------------------------ | ------------------------------------- |
| String(char[]):String          | 构造方法，通过字符数组构建字符串         |
| charAt(int)  :char             | 返回索引位置的字符                     |
| indexOf(char):int              | 返回某个字符在串中第一次出现时的索引     |
| toCharArray():char[]           | 返回字符串对应的字符数组                |
| length():int                   | 返回串长                               |
| substring(int i, int l):String | 返回从索引位置开始，指定长度的子串       |
| split(String):String[]         | 根据给定字符串切割原串，返回字符串数组。 |

StringBuffer：

|         方法         |    说明    |
| ------------------- | ---------- |
| append(String):void | 添加字符串 |


### 封装类

基本数据类型都有其对应的封装类，它们之间的转换被称为装箱与拆箱。

封装类的作用：

* 用于集合存储
* String转基本数据类型间相互转换

基本数据类型与封装类的对应关系：

| 基本数据类型 |   封装类   |
| ----------- | --------- |
| byte        | Byte      |
| boolean     | Boolean   |
| char        | Character |
| short       | Short     |
| int         | Integer   |
| long        | Long      |
| float       | Float     |
| double      | Double    |


#### 常用方法

|           方法           |                        说明                         |
| ----------------------- | --------------------------------------------------- |
| valueOf(String):Integer | 将字符串转会为对应封装类                              |
| toString():String       | 输出对应字符串                                       |
| parseInt(String):int    | 使用封装类将字符串转换为对应基本数据类型，主要是Integer |


#### 自动拆装箱



#### 使用示例

进行基本数据类型与基本数据类型之间、基本数据类型与字符串之间的相互转换。

例如将 double 转换为 int。

```java
//不进行四舍五入操作：

(int)x

//进行四舍五入操作：

Integer.parseInt(new java.text.DecimalFormat("0").format(x))
```


### Object

Object 是所有类的父类。

一个类在没有父类的情况下会自动继承基类，如果有父类，就不会继承Object（基类）。

#### 重要方法

|          方法           |                                说明                                |
| ---------------------- | ------------------------------------------------------------------ |
| equals(Object):boolean | 比较两个对象是否相等                                                 |
| clone():Object         | 克隆一个对象，分为浅克隆和深克隆                                      |
| hashcode():int         | 返回一个对象的哈希编码                                               |
| toString():String      | 返回对象对应的字符串                                                 |
| getClass():Class       | 返回此 Object 的运行时类                                            |
| wait()                 | 当前线程等待                                                        |
| notify()               | 唤醒在此对象监视器上等待的单个线程                                    |
| notifyAll()            | 唤醒在此对象监视器上等待的所有线程                                    |
| finalize()             | 当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法 |

#### equals() 与 hashcode()

重写 equals() 时必须重写 hashcode()。2 个对象的 equals() 方法返回 true，其 hashCode() 必须返回相同的值。否则对于 HashSet、HashMap、HashTable 等基于 hashcode 的集合类就会出现问题。例如 HashSet 插入对象时，如果发现某个位置已经有一个具有相同 hashcode 的元素，就是使用 equals() 进行比较，如果返回 true，则放弃插入，因为已经存在；如果返回 false，则使用冲突解决方式插入。

java.lang.Object 中对 hashCode 的约定：

1. 在一个应用程序运行期间，假设一个对象的 equals 方法做比较所用到的信息没有被改动的话。则对该对象调用 hashCode 方法多次，它必须始终如一地返回同一个整数。
2. 假设两个对象依据 equals(Object o) 方法是相等的，则调用这两个对象中任一对象的 hashCode 方法必须产生同样的整数结果。
3. 假设两个对象依据 equals(Object o) 方法是不相等的。则调用这两个对象中任一个对象的 hashCode 方法。不要求产生不同的整数结果。但假设能不同，则可能提高散列表的性能。

hashCoe() 方法是传统的方法，hashCode2() 和 hashCode3() 这 2 种办法都是 JDK7 以后支持的，可以简化代码。

Object.hashCode() 不能够代表内存地址。


#### equals() 与 ==

equals() 是判断两个变量或者实例指向同一个内存空间的值是不是相同。实际上 Object 默认的实现就是==，但一般都会重写。

== 是判断两个变量或者实例是不是指向同一个内存空间。对基本数据类型而言就是比较栈中的值，引用数据类型实际就是比较栈中的引用。


#### 浅克隆与深克隆

浅克隆只复制当前对象，而对于其依赖（组合）的对象不分配新的内存空间。

深克隆会将当前对象和其依赖的对象都分配新的空间，可以通过比特流实现。

深拷贝的具体方法：

```
```


### Math

是一个数学工具类，常用方法如下：

|                      方法                      |      描述       |
| --------------------------------------------- | --------------- |
| Math.ceil(double):double                      | 向上取整用       |
| Math.floor(double):double                     | 向下取整用       |
| Math.max(double, double):double               | 获取二者的最大值 |
| Math.min(double, double):double               | 获取二者的最小值 |
| Math.abs():                                   | 获取绝对值       |
| Math.pow(double base, double exponent):double | 获取乘方         |


### Comparable 与 Comparator

Comparable 是排序接口，实现它就意味着该类可以自然排序，属于内部排序。

接口定义如下：

```java
package java.lang;
import java.util.*;

public interface Comparable<T>
{
    public int compareTo(T o);
}
```

compareTo 方法的返回值有三种情况：

* e1.compareTo(e2) > 0 即 e1 > e2

* e1.compareTo(e2) = 0 即 e1 = e2

* e1.compareTo(e2) < 0 即 e1 < e2

Comparator 也是一个接口，用于定制排序，是外部排序。

接口定义如下：

```java
package java.util

public interface Comparator<T>
{
    public int compare(T lhs, T rhs);

    public boolean equals(Object object);
}
```

二者的区别是使用场景不同，Comparable 用于定义类时，规定该类对象的大小关系如何判别；Comparator 用于自定义排序方式。如果一个类没有实现 Comparable ，那么也可以通过 Comparator 来将它排序。

Comparable 使用示例如下：

```java
/**
 * @author 黄超
 */
public class UseComparable implements Comparable<UseComparable>
{
    private int weight;

    public int getWeight() {
        return weight;
    }

    public void setWeight(int weight) {
        this.weight = weight;
    }

    @Override
    public int compareTo(UseComparable o)
    {
        return this.weight - o.weight;
    }
}
```

Comparator 使用示例如下：

```java
/**
 * @author 黄超
 */
public class UseComparator
{
    public static void main(String[] args)
    {
        List<Integer> nums = new ArrayList();
        nums.add(3);
        nums.add(1);
        nums.add(2);
        Collections.sort(nums, new Comparator<Integer>()
        {
            @Override
            public int compare(Integer o1, Integer o2)
            {
                return o1-o2;
            }
        });
    }
}
```

### 对比 equals()

如果一个类实现了 Comparaable 同时重写了 equals 方法，需要注意二者的比较的属性最好一致，否则可能会出现问题。例如，使用集合 List 的 `indexOf(T o)` 查找一个对象，基于的是 equals 方法，而使用工具类 Collections.binarySearch(Collection c, T o) 查询则是基于该对象的 compareTo 方法。如果二者比较时基于的属性不一致，会导致使用以上两个方法查询的对象不同。


## 集合

集合类，包括线性表、队列、栈、集合、图。

### 集合框架

接口层：Collection、Map、List、Set、Queue、Dequeue 等。

实现层：TreeMap、HashMap、LinkedHashMap、ArrayList、LinkedList、HashSet、PriorityQueue。

工具层：Arrays、Collections。

![集合框架示意图](vx_images/3028303219191.gif)

### 常用集合

#### List

是一个接口，通常使用 ArrayList 或 LindkedList 赋值给它。

常用方法如下：

|            方法             |        说明         |
| -------------------------- | ------------------- |
| add(Object)                | 添加一个元素         |
| remove(Object)             | 删除指定元素         |
| indexOf(Object)            | 返回一个元素的索引   |
| size():int                 | 返回元素个数         |
| toArray(Object[]):Object[] | 转换为一个对象数组   |
| clear()                    | 清空                |
| contains(Object):boolean   | 判断是否包含某个元素 |

#### ArrayList

ArrayList 继承了 AbstractList，实现了 List 接口，底层实现基于动态数组，因此可以认为是一个可变长度的数组。

默认的容量是 10，在添加一个元素前，会检查添加后的长度是否超过容量，如果超过，就会扩容。

扩容的具体步骤如下：

1. 创建一个新数组，容量为原来容量的 1.5 倍（如果原来的容量是偶数就是 1.5 倍，否则是 1.5 倍左右，因为使用了位运算）。
2. 然后使用 Arrays.copyOf() 将原来数组中的元素复制到新数组。

常用方法如下

|      方法       |                                                                  描述                                                                   |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| add(int, E)    | 在特定索引处将值插入 ArrayList，此方法将移动列表的后续元素.但是你无法保证 List 会保持排序状态,因为你插入的新对象可能会根据排序顺序位于错误的位置。 |
| set(int, E):E  | 替换指定位置的元素。                                                                                                                      |
| get(int):E     | 获取指定位置元素                                                                                                                         |
| remove(Object) | 删除给定元素，和 get() 配合可以删除指定位置的元素。                                                                                         |

#### LinkedList

可以认为是链表。

常用方法如下：

|              方法              |                            描述                             |
| ----------------------------- | ----------------------------------------------------------- |
| remove(int)                   | 移除此列表中指定位置处的元素。                                |
| remove(Object)                | 从此列表中移除首次出现的指定元素（如果存在）。                 |
| removeFirst()                 | 移除并返回此列表的第一个元素。                                |
| removeFirstOccurrence(Object) | 从此列表中移除第一次出现的指定元素（从头部到尾部遍历列表时）。   |
| removeLast()                  | 移除并返回此列表的最后一个元素。                              |
| removeLastOccurrence(Object)  | 从此列表中移除最后一次出现的指定元素（从头部到尾部遍历列表时）。 |
| get(int):Object               | 获取特定位置元素                                             |
| add(Object)                   | 添加一个元素                                                 |
| clear()                       | 清空                                                        |


#### Queue

队列接口，一般使用 LinkedList 实例化它。

|      方法      |          描述           |
| ------------- | ---------------------- |
| offer(Object) | 在队尾添加一个元素       |
| poll()        | 在队头获取并删除一个元素 |


#### Stack

栈的实现类，它是不属于集合框架的，同时是线程安全的。

使用实例如下：

`Stack<Object> stack = new Stack();`

现在推荐使用 DeQueue 代替它。

|          方法           |                描述                |
| ---------------------- | ---------------------------------- |
| push()                 | 压栈                                |
| pop()                  | 弹栈                                |
| peek()                 | 获取栈顶元素                        |
| empty():boolean        | 测试堆栈是否为空。                   |
| search(Object):boolean | 返回对象在堆栈中的位置，以 1 为基数。 |


#### Deque

双端队列，既可以当栈用，又可以当队列用。


#### PriorityQueue

优先级队列，使用时需要设置比较器。

#### Map

图的接口，一般使用的是 HashMap。

#### Set

集合接口，一般使用的是 HashSet。

#### HashSet

无序集，不允许元素重复。

常用方法如下：

|   方法    | 说明 |
| -------- | ---- |
| add      |      |
| clear    |      |
| clone    |      |
| contains |      |
| isEmpty  |      |
| iterator |      |
| remove   |      |
| size     |      |


#### HashMap

使用得非常广的一种图。

##### 常用方法

|                  方法                   |                     描述                      |
| -------------------------------------- | -------------------------------------------- |
| put(Object key,Object value)           | 加入元素                                      |
| putAll(Collection c)                   | 加入集合中的所有元素                           |
| get(Object key):Object                 | 获取key对应的value                            |
| containsKey(Object key)                | 判断key是否存在                               |
| containsValue(Object value)            | 判断value是否存在                             |
| remove(Object key)                     | 根据key删除元素                               |
| values():                              | 获取所有value                                 |
| isEmpty():boolean                      | 判读是否为空                                  |
| entrySet():Set\<Entry>                 | 获取所有元素，即 Entry 对像                    |
| keySet():Object                        | 获取所有key                                   |
| getOrDefault(Object key, Object value) | 获取一个 key 对应的 value，如果没有就使用默认的 |

##### 实现原理

**什么时候扩容？**

哈希表长度默认16，可以扩容。必须是2的倍数，为了服务 hashcode 函数。

第一次put的时候，做一次判断，如果长度为0或者为null的话，就做一次扩容，数组也是在这个时候初始化的

大于阈值会扩充哈希表，阈值由加载因子获得，加载因子为0.75。

第13次put的时候自动扩容。

**怎么扩容？**

JDK 1.7 中 HashMap 的 resize() 源码如下：

```java
    void resize(int newCapacity) {   //传入新的容量
        Entry[] oldTable = table;    //引用扩容前的Entry数组
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {  //扩容前的数组大小如果已经达到最大(2^30)了
            threshold = Integer.MAX_VALUE; //修改阈值为int的最大值(2^31-1)，这样以后就不会扩容了
            return;
        }

        Entry[] newTable = new Entry[newCapacity];  //初始化一个新的Entry数组
        transfer(newTable);                         //！！将数据转移到新的Entry数组里
        table = newTable;                           //HashMap的table属性引用新的Entry数组
        threshold = (int) (newCapacity * loadFactor);//修改阈值
    }
```


**什么时候使用红黑树？**

在链表长度大于 8 并且 表的长度大于 64 的时候会转化红黑树，红黑树长度小于6时会退化为链表。

**线程安全的 HashMap？**

currenthashmap 线程安全。

**记录插入顺序的 HashMap？**

LinkedHashMap 记录插入顺序。

##### 通过 value 获取 key

只能通过遍历的方式实现，一种是遍历所有 key 对应的 value 判断是否与给定的相等；一种是遍历所有的 entry 判断 value 是否与给定的相等。

##### 遍历

* 通过 entrySet() 配合迭代器
* 直接使用迭代器
* for( : ) 遍历

按插入顺序遍历

##### entry

是 HashMap 的一个内部类，定义了存储在 HashMap 中的键值对。

使用 `getEntrySet():Set` 可以获取，主要有两个属性：key、value。


#### Vector

Vector 类实现了一个动态数组。和 ArrayList 很相似，但是两者是不同的：

* Vector 是线程安全的。
* Vector 包含了许多传统的方法，这些方法不属于集合框架。
* Vector 主要用在事先不知道数组的大小，或者只是需要一个可以改变大小的数组的情况。

#### 线程安全的集合类

线程安全即不用编程人员主动上锁。

HashTable：哈希表的线程安全版，效率低。

ConcurrentHashMap：哈希表的线程安全版，效率高，用于替代 HashTable。

Vector：线程安全版 ArrayList。

Stack：线程安全版栈。

BlockingQueue 及其子类：线程安全版队列。


### 公共方法

这些是集合类常用的公共方法：

|                 方法                 |                    描述                     |
| ----------------------------------- | ------------------------------------------- |
| toArray(T[])：T[]                    | ArrayList、LinkedList中使用，将集合解析为数组 |
| Collection.sort(List,Comparator<T>) | 对集合进行排序，不知道 HashMap 是否可以使用。  |


#### toArray 示例

```java
List r = LinkedList<Integer>();
Interget[] d = (Integer[]) r.toArray(new Integer[r.size()]);
```


## 泛型

当不确定一个位置将会使用哪个类，或者希望代码具有更强的普适性以及更低的耦合性时，就需要使用泛型。

### 泛型类

泛型类即使声明了泛型参数的类，实例如下：

```
public class Test <T>
{
    T t;
    public void set(T t)
    {
        this.t = t;
    }
}
```

这样声明的泛型参数在整个类中都是可见的。

### 泛型方法

在函数声明时可以声明泛型参数，这样泛型参数只在当前方法中有效。同时方法单独声明泛型参数可以覆盖类声明的泛型参数。

示例如下：

```
public T static void test(T t)
{
    // do something
}
```

### 泛型数组

可以声明被泛型参数修饰的属性的数组。

泛型数组的声明方式：

* `T[] temp = (T[])new Object[];`
* `T[] temp = Array.newInstance(clazz, length);`

需要注意的是，对于实现了Comparable 接口的泛型变量：

* `T[] temp = (T[])new Object[];`不能使用，因为 T 继承了 Comparable。
* `T[] temp = Array.newInstance(clazz, length);`也不能使用，因为无法获取类名 clazz。

只能使用集合： `List temp = new ArrayList<T>(data.length);`


### 泛型比较

所谓泛型比较，并不是一个官方的概念，这里我要表达的意思就是，如何让一个泛型参数能够代表一个可以被比较（排序）的类。

一个可以被比较的类，可以理解为一个实现了 Comparable 接口的类。

一种最简单的泛型比较方法就是使用下述代码：

```java
public class Test <T extends Comparable<T>>{}
```

在这个类的声明中，定义了一个泛型参数 T ，T 后面的代码表示，限定 T 是一个实现了`Comparable<T>`的类。可能有人会疑惑，这里为什么使用`extends`而不是`implements`；泛型类的英文是 generic class，是 class 不是 interface，但是这里用了 extands Comparable，只有接口才会 extands 接口，那泛型类难道是接口吗？官方给出的答案如下：

> `<T extands BoundingType>`表示 T 应该是绑定类型（BoundingType）的子类型（subType）。
> T 和绑定类型可以是类，也可以是接口。
> 选择关键字 extands 的原因是它更接近子类的概念，并且 java 的设计者也不打算在语言中添加一个新的关键字（如sub）。
> 所以，这里的泛型用`<T extends Interface>`中extends　的关键字的意思，其实是在给泛型设置限定（bound）的时候, 让extends = extends or implements。

上面的方法可以解决绝大部分泛型比较的问题了，但是也有不足之处。例如，当我们使用一个他人编写的类，这个类没有实现比较接口，而是它的父类实现了。这种情况下就要使用下面的方法：

```java
public class Test <T extends Comparable<? super T>>{}
```

一句话阐述就是，泛型参数 T 代表着这样一个类：这个类实现了一个以它的父类作为参数的比较接口。

下面是对于这种写法的详细解释。

解释一：

从基本的继承实现概念出发：
若有：
class Father implements Comparable<Father>
以及：
class Son extends Father
则有：
Son implements Comparable<Father>(子类自动实现父类所实现的接口）
如果定义Son时：
Son 没有implements Comparable<son>
那么：
对于这种情况：
class A <T extends Comparable<T>>
new A<Son>是非法的，因为Son 没有implements Comparable<Son>
而对于这种情况：
class A <T extends Comparable< ？ super T>>
new A<Son>是合法的，上述表达式中？可以匹配Father，因为Son implements Comparable<Father>
从上可以看出这种写法的好处是父类若实现了Comparable<父类>，那么即使子类没有实现Comparable<子类>，一样可以new A<子类>


解释二：

[解释来源](https://www.zhihu.com/question/25548135)

首先这是运用了java的泛型

1. extends后面跟的类型如<任意字符 extends 类/接口>表示泛型的上限

```java
import java.util.*;
class Demo<T extends AbstractList>{}
public class Test
{
    public static void main(String[] args)
    {
        Demo<ArrayList> p = null; // 编译正确
        //这里因为ArrayList是AbstractList的子类所以通过
        //如果改为Demo<AbstractCollection> p = null;就会报错这样就限制了上限
    }
}
```

2. 同样的super表示泛型的下限
3. <T extends Comparable<? super T>>这里来分析T表示任意字符名，extends对泛型上限进行了限制即T必须是Comparable<? super T>的子类，然后<? super T>表示Comparable<>中的类型下限为T！这样来看一段代码辅助理解

```java
import java.util.GregorianCalendar;

class Demo<T extends Comparable<? super T>>{}

public class Test1
{
    public static void main(String[] args)
    {
        Demo<GregorianCalendar> p = null; // 编译正确
    }
}
```

这个可以理解为<GregorianCalendar extends Comparable<Calendar>>是可以运行成功的！因为Calendar为GregorianCalendar 的父类并且GregorianCalendar 实现了Comparable<Calendar>,可查看api！.如果是如下代码则运行不成功

```java
import java.util.GregorianCalendar;
class Demo<T extends Comparable<T>>{}
//这里把? super去掉了
public class Test
{
    public static void main(String[] args)
    {
        Demo<GregorianCalendar> p = null;
    }
}
```

编译会报错！因为<T extends Comparable<T>>相当于<GregorianCalendar extends Comparable<GregorianCalendar>>但是GregorianCalendar并没有实现Comparable<GregorianCalendar>而是实现的Comparable<Calendar>，这里不在限制范围之内所以会报错！


### 泛型 与 Object

如果不清楚某个位置到底回使用哪个类的实例时，可以使用泛型或 Object 代替，但它们二者是有差别的：

* 使用泛型后，最后编译为字节码时，该位置会被替换为真实使用的类。
* 使用 Object 后，最后字节码中该位置仍然是 Object。

可以认为使用泛型对于该位置的类型具有更强的约束力。


### 泛型擦除

泛型变量在最后编译为字节码时会被替换为真正使用的类，这被称为泛型擦除，这一点与 C++ 是不同。

可以认为 Java 中使用的泛型并非真正的泛型，但这并不是因为无法实现，而是考虑到兼容历史代码的无奈妥协。


## 反射

反射就是获取类的字节码文件，可以使我们在运行状态获取任何一个类的方法和属性。对于任意一个对象，都能够调用它的任意方法和属性。

使用反射获取一个类的实例比 new 关键字获取慢。

### Class 类

在java世界里，一切皆对象。从某种意义上来说，java有两种对象：实例对象和Class对象。每个类的运行时的类型信息就是用Class对象表示的。它包含了与类有关的信息。其实我们的实例对象就通过Class对象来创建的。Java使用Class对象执行其RTTI（运行时类型识别，Run-Time Type Identification），多态是基于RTTI实现的。

每一个类都有一个Class对象，每当编译一个新类就产生一个Class对象，**基本类型** (boolean, byte, char, short, int, long, float, and double)有Class对象，**数组**有Class对象，就连**关键字**void也有Class对象（void.class）。Class对象对应着java.lang.Class类，如果说类是对象的抽象和集合的话，那么Class类就是对类的抽象和集合。

Class类没有公共的构造方法，Class对象是在类加载的时候由Java虚拟机以及通过调用类加载器中的 defineClass 方法自动构造的，因此不能显式地声明一个Class对象。一个类被加载到内存并供我们使用需要经历如下三个阶段：

1. 加载：这是由类加载器（ClassLoader）执行的。通过一个类的全限定名来获取其定义的**二进制字节流**（Class字节码），将这个字节流所代表的静态存储结构转化为方法区的运行时数据接口，根据字节码在java堆（元空间的方法区）中生成一个代表这个类的java.lang.Class对象。

2. 链接：在链接阶段将验证Class文件中的字节流包含的信息是否符合当前虚拟机的要求，为静态域分配存储空间并设置类变量的初始值（默认的零值），并且如果必需的话，将常量池中的符号引用转化为直接引用。

3. 初始化：到了此阶段，才真正开始执行类中定义的java程序代码。用于执行该类的静态初始器和静态初始块，如果该类有父类的话，则优先对其父类进行初始化。

所有的类都是在对其第一次使用时，动态加载到JVM中的（懒加载）。当程序创建第一个对类的静态成员的引用时，就会加载这个类。使用new创建类对象的时候也会被当作对类的静态成员的引用。因此java程序程序在它开始运行之前并非被完全加载，其各个类都是在必需时才加载的。这一点与许多传统语言都不同。动态加载使能的行为，在诸如C++这样的静态加载语言中是很难或者根本不可能复制的。

在类加载阶段，类加载器首先检查这个类的Class对象是否已经被加载。如果尚未加载，默认的类加载器就会根据类的全限定名查找.class文件。在这个类的字节码被加载时，它们会接受验证，以确保其没有被破坏，并且不包含不良java代码。一旦某个类的Class对象被载入内存，我们就可以它来创建这个类的所有对象。


### Class类的方法

* 带有Declared修饰的方法可以反射到私有的方法。

* 没有Declared修饰的只能用来反射公有的方法。

* 对类的操作

    ![Class类的类相关方法](vx_images/5329758198947.png)

* 获得类的属性

    ![Class类获取类的属性](vx_images/4672100204083.png)

* 获得类的构造方法

    ![Class类获得类的构造方法](vx_images/3970401197629.png)

* 获得类的方法

    ![Class类获得类的方法](vx_images/3599103181293.png)

* 获得类的注解

    ![Class类获得类的注解](vx_images/1961602190763.png)

* 其他重要方法

    ![Class类的其他重要方法](vx_images/3351305183797.png)

* Field类

    ![Field类的方法](vx_images/4822306192744.png)

* Method类

    ![Method类的方法](vx_images/2368907185629.png)

* Constructor类

    ![Constructor类](vx_images/5634607186238.png)


### 示例

```
class Customer {

    private Long id;
    private String name;
    private int age;

    public Customer() {}

    public Customer(String name,int age) {
        this.name = name;
        this.age = age;
    }

    public Long getId() {
        return id;
    }
    public void setId(Long id) {
        this.id=id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name=name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age=age;
    }

}

public class ReflectTester {

    public Object copy(Object object) throws Exception{
        //获得对象的类型
        Class classType=object.getClass();
        System.out.println("Class:"+classType.getName());

        //通过默认构造方法创建一个新的对象
        Object objectCopy=classType.getConstructor(new Class[]{}).newInstance(new Object[]{});

        //获得对象的所有属性
        Field fields[]=classType.getDeclaredFields();

        for(int i=0; i<fields.length;i++){
              Field field=fields[i];

              String fieldName=field.getName();
              String firstLetter=fieldName.substring(0,1).toUpperCase();
              //获得和属性对应的getXXX()方法的名字
              String getMethodName="get"+firstLetter+fieldName.substring(1);
              //获得和属性对应的setXXX()方法的名字
              String setMethodName="set"+firstLetter+fieldName.substring(1);

              //获得和属性对应的getXXX()方法
              Method getMethod=classType.getMethod(getMethodName,new Class[]{});
              //获得和属性对应的setXXX()方法
              Method setMethod=classType.getMethod(setMethodName,new Class[]{field.getType()});

              //调用原对象的getXXX()方法
              Object value=getMethod.invoke(object,new Object[]{});
              System.out.println(fieldName+":"+value);
              //调用拷贝对象的setXXX()方法
              setMethod.invoke(objectCopy,new Object[]{value});
        }
        return objectCopy;
     }

}
```


## 代理

代理为控制要访问的目标对象提供了一种途径。当访问对象时，它引入了一个间接的层。JDK 自从 1.3 版本开始，就引入了动态代理，并且经常被用来动态地创建代理。 JDK 的动态代理用起来非常简单，当它有一个限制，就是使用动态代理的对象必须实现一个或多个接口。如果想代理没有实现接口的继承的类，我们可以使用 CGLIB 包。

动态代理技术就是用来产生一个对象的代理对象的。它是学习 Java 框架的基础。

代理对象的两个概念：

1. 代理对象存在的价值主要用于拦截对真实业务对象的访问。

2. 代理对象应该具有和目标对象(真实业务对象)相同的方法。

在 Java 中，动态代理有两种实现方式，反射和 CGLIB。

### 动态代理

Java 反射包中提供了一个实现动态代理的方法，就是使用`"java.lang.reflect.Proxy"`类。

主要使用它的`static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h) `方法。

这个方法有三个参数，ClassLoader loader 用来指明生成代理对象使用哪个类装载器，Class<?>[] interfaces用来指明生成哪个对象的代理对象，通过接口指定，InvocationHandler h 用来指明产生的这个代理对象要做什么事情。

通过调用 newProxyInstance 方法就可以得到某一个对象的代理对象。

Java 的代理完全按照代理模式设计。所以如果想要一个类能被代理，那么这个类必须是面向抽象编程的，即它必须是实现了某个接口。

#### 示例

* 抽象主题类

```
package Design_pattern.Proxy_pattern;

/**
* 抽象主题角色
* @author hc
*/
public interface AbstractSubject {

    /** 请求方法 */
    public void request();
}

```

* 主题类A

```
package Design_pattern.Proxy_pattern;

/**
* @author hc
*/
public class RealSubjectA implements AbstractSubject{

    @Override
    public void request() {
        System.out.println("真实主题类A");
    }
}

```

* 主题类B

```
package Design_pattern.Proxy_pattern;

/**
* @author hc
*/
public class RealSubjectB implements AbstractSubject{

    @Override
    public void request() {

        System.out.println("真实主题类B");
    }
}

```

* 代理类

```
package Design_pattern.Proxy_pattern;

import sun.misc.ProxyGenerator;

import java.io.FileOutputStream;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

/**
* @author hc
*/
public class DynamicProxy implements InvocationHandler {

    /** 存放被代理对象 */
    Object obj;

    public DynamicProxy(){};

    public DynamicProxy(Object obj){
        this.obj = obj;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        preRequest();

        /*
        看看这个proxy到底是哪个类。
        结果是class com.sun.proxy.$Proxy0
        作用：
        1. 可以使用反射获取代理对象的信息（也就是proxy.getClass().getName()）。
        2. 可以将代理对象返回以进行连续调用，这就是proxy存在的目的。因为this并不是代理对象。
        https://www.zhihu.com/question/52551525/answer/132978844
        结论：
        这里的invoke()方法不由用户调用，
        每一个动态代理类的调用处理程序都必须实现InvocationHandler接口，
        并且每个代理类的实例（即本代码中的subject对象）都关联到了实现该接口的动态代理类调用处理程序中，
        关联是通过Proxy.newProxyInstance()方法实现的，
        当我们通过动态代理对象调用一个方法时候，
        这个方法的调用就会被转发到实现InvocationHandler接口类的invoke方法来调用，
        同时这个方法对应的类和方法的名称会被包含到InvocationHandler的invoke()方法的method参数中，
        借助Method类的invoke()方法来反射调用这个方法。
        */
        System.out.println(proxy.getClass().toString());
        byte[]  b= ProxyGenerator.generateProxyClass(proxy.getClass().getSimpleName(),proxy.getClass().getInterfaces());
        FileOutputStream out = new FileOutputStream("./"+proxy.getClass().getSimpleName()+".class");
        out.write(b);
        out.flush();
        out.close();

        method.invoke(obj, args);
        postRequest();
        return null;
    }

    public void preRequest(){
        System.out.println("调用之前");
    }

    public void postRequest(){
        System.out.println("调用之后");
    }
}

```

* 调用

```
package Design_pattern.Proxy_pattern;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

/**
* @author hc
*/
public class Client {
    public static void main(String args[]){
        InvocationHandler handler = null;
        AbstractSubject subject = null;

        handler = new DynamicProxy(new RealSubjectA());
        // 获取A的代理对象
        subject = (AbstractSubject) Proxy.newProxyInstance(AbstractSubject.class.getClassLoader(), new Class[]{AbstractSubject.class}, handler);
        subject.request();
        // 分界线
        System.out.println("-----------------");
        handler = new DynamicProxy(new RealSubjectB());
        // 获取B的代理对象
        subject = (AbstractSubject) Proxy.newProxyInstance(AbstractSubject.class.getClassLoader(), new Class[]{AbstractSubject.class}, handler);
        subject.request();
    }
}

```

* 应用

    [通过动态代理解决Get访问页面时中文乱码](https://blog.csdn.net/m0_38039437/article/details/77970633)


### CGLIB

#### CGLIB是什么

Cglib是一个强大的、高性能的代码生成包，它广泛被许多AOP框架使用，为他们提供方法的拦截。

1. Spring AOP和dynaop，为他们提供方法的interception（拦截）；

2. hibernate使用CGLIB来代理单端single-ended(多对一和一对一)关联（对集合的延迟抓取，是采用其他机制实现的）；

3. EasyMock和jMock是通过使用模仿（moke）对象来测试java代码的包。

    它们都通过使用CGLIB来为那些没有接口的类创建模仿（moke）对象。

#### 底层

CGLIB包的底层是通过使用一个小而快的字节码处理框架ASM(Java字节码操控框架)，来转换字节码并生成新的类。除了CGLIB包，脚本语言例如 Groovy和BeanShell，也是使用ASM来生成java的字节码。当不鼓励直接使用ASM，因为它要求你必须对JVM内部结构包括class文件的格式和指令集都很熟悉。所以cglib包要依赖于asm包，需要一起导入。

Spring AOP和Hibernate同时使用JDK的动态代理和CGLIB包。Spring AOP，如果不强制使用CGLIB包，默认情况是使用JDK的动态代理来代理接口。

下图为cglib与一些框架和语言的关系（CGLIB Library and ASM Bytecode Framework）

![Cglib与框架的关系](vx_images/4075017130844.png =700x)

对此图总结一下：

* 最底层的是字节码Bytecode，字节码是Java为了保证“一次编译、到处运行”而产生的一种虚拟指令格式，例如iload_0、iconst_1、if_icmpne、dup等

* 位于字节码之上的是ASM，这是一种直接操作字节码的框架，应用ASM需要对Java字节码、Class结构比较熟悉

* 位于ASM之上的是CGLIB、Groovy、BeanShell，后两种并不是Java体系中的内容而是脚本语言，它们通过ASM框架生成字节码变相执行Java代码，这说明在JVM中执行程序并不一定非要写Java代码----只要你能生成Java字节码，JVM并不关心字节码的来源，当然通过Java代码生成的JVM字节码是通过编译器直接生成的，算是最“正统”的JVM字节码

* 位于CGLIB、Groovy、BeanShell之上的就是Hibernate、Spring AOP这些框架了，这一层大家都比较熟悉

* 最上层的是Applications，即具体应用，一般都是一个Web项目或者本地跑一个程序


#### 使用方法

使用CGLIB进行动态代理时，被代理类可以不用面向抽象编写。下面是一个需要被代理的类。

```
public class Dao
{
    public void update()
    {
        System.out.println("PeopleDao.update()");
    }

    public void select()
    {
        System.out.println("PeopleDao.select()");
    }
}
```

下面是代理类，需要实现 **MethodInterceptor** 接口，重写 **intercept** 方法，这个方法与使用Java原生动态代理时重写的 invoke 方法很像。

* Object表示要进行增强的对象

* Method表示要拦截的方法

* Object[]数组表示该参数列表，基本数据类型需要传入其包装类型，如int-->Integer、long-Long、double-->Double

* MethodProxy表示对方法的代理，invokeSuper方法表示对被代理对象方法的调用

```
public class DaoProxy implements MethodInterceptor
{
    @Override
    public Object intercept(Object object, Method method, Object[] objects, MethodProxy proxy) throws Throwable
    {
        System.out.println("Before Method Invoke");
        // 调用被代理对象
        proxy.invokeSuper(object, objects);
        System.out.println("After Method Invoke");
        return object;
    }
}
```

最后在客户端通过 **Enhancer** 类实现代理。当然也可以**在代理类中创建一个工程方法实现代理**。

```
public class Client
{
    @Test
    public void client()
    {
        // 代理对象模板
        DaoProxy daoProxy = new DaoProxy();
        Enhancer enhancer = new Enhancer();
        // 设置被代理对象
        enhancer.setSuperclass(Dao.class);
        // 设置代理对象
        enhancer.setCallback(daoProxy);
        // 创建代理对象
        Dao dao = (Dao)enhancer.create();
        // 使用代理对象
        dao.update();
        dao.select();
    }
}
```

另外 **Enhancer** 类还有一个`enhancer.setCallbackFilter(CallbackFilter filter);`方法，可以定义不同的拦截策略。

在上述代码基础上，添加另一个代理类。

```
public class DaoAnotherProxy implements MethodInterceptor
{
    @Override
    public Object intercept(Object object, Method method, Object[] objects, MethodProxy proxy) throws Throwable
    {
        System.out.println("StartTime=[" + System.currentTimeMillis() + "]");
        method.invoke(object, objects);
        System.out.println("EndTime=[" + System.currentTimeMillis() + "]");
        return object;
    }
}
```

然后创建一个继承 **CallbackFilter** 类的拦截器。

```
public class DaoFilter implements CallbackFilter
{
    @Override
    public int accept(Method method)
    {
        // 如果使用select方法，就用DaoProxy代理
        if ("select".equals(method.getName()))
        {
            return 0;
        }
        // 否则用DaoAnotherProxy代理
        return 1;
    }
}
```

最后在客户端使用。

```
public class Client
{

    @Test
    public void client()
    {
        // 两个代理类实例
        DaoProxy daoProxy = new DaoProxy();
        DaoAnotherProxy daoAnotherProxy = new DaoAnotherProxy();
        // 创建代理对象
        Enhancer enhancer = new Enhancer();
        enhancer.setSuperclass(Dao.class);
        // 在设置代理类时放置两个代理类
        enhancer.setCallbacks(new Callback[]{daoProxy, daoAnotherProxy, NoOp.INSTANCE});
        enhancer.setCallbackFilter(new DaoFilter());
        // 使用代理对象
        Dao dao = (Dao)enhancer.create();
        // 不是select，会使用DaoAnotherProxy中的规则
        dao.update();
        // select，会使用DaoProxy中的规则
        dao.select();
    }
}
```


## 线程

### 创建线程

在Java中实现多线程有三种手段：

* 继承Thread类

* 实现Runnable接口

* 继承 Callable 类


#### Runnable

```
/** 实现Runnable接口，作为线程的实现类 */
class MyThread implements Runnable
{
    // 表示线程的名称
    private String name ;
    // 通过构造方法配置name属性
    public MyThread(String name)
    {
        this.name = name ;
    }
    // 覆写run()方法，作为线程 的操作主体
    public void run()
    {
        for(int i=0;i<10;i++)
        {
            System.out.println(name + "运行，i = " + i) ;
        }
    }
}

public class RunnableDemo01
{
    public static void main(String[] args)
    {
        // 实例化对象
        MyThread mt1 = new MyThread("线程A ");
        MyThread mt2 = new MyThread("线程B ");
        // 实例化Thread类对象
        Thread t1 = new Thread(mt1);
        Thread t2 = new Thread(mt2);
        // 启动多线程
        t1.start();
        t2.start();
    }
}

```

#### Thread

```
// 继承Thread类，作为线程的实现类
class MyThread extends Thread
{
    // 表示线程的名称
    private String name ;
    // 通过构造方法配置name属性
    public MyThread(String name)
    {
        this.name = name ;
    }
    // 覆写run()方法，作为线程 的操作主体
    public void run()
    {
        for(int i=0;i<10;i++)
        {
            System.out.println(name + "运行，i = " + i) ;
        }
    }
}

public class ThreadDemo02
{
    public static void main(String[] args)
    {
        // 实例化对象
        MyThread mt1 = new MyThread("线程A ");
        MyThread mt2 = new MyThread("线程B ");
        // 调用线程主体
        mt1.start();
        mt2.start();
    }
}

```

#### 线程生命周期

1. 创建状态

    在程序中用构造方法创建了一个线程对象后，新的线程对象便处于新建状态，此时它已经有了相应的内存空间和其他资源，但还处于不可运行状态。新建一个线程对象可采用 Thread 类的构造方法来实现，例如“Thread thread=new Thread()”。

2. 就绪状态

    新建线程对象后，调用该线程的 start() 方法就可以启动线程。当线程启动时，线程进入就绪状态。此时，线程将进入线程队列排队，等待 CPU 服务，这表明它已经具备了运行条件。

3. 运行状态

    当就绪状态被调用并获得处理器资源时，线程就进入了运行状态。此时，自动调用该线程对象的 run() 方法。run() 方法定义该线程的操作和功能。

4. 阻塞状态

    一个正在执行的线程在某些特殊情况下，如被人为挂起或需要执行耗时的输入/输出操作，会让 CPU 暂时中止自己的执行，进入阻塞状态。在可执行状态下，如果调用sleep()，suspend()，wait() 等方法，线程都将进入阻塞状态，发生阻塞时线程不能进入排队队列，只有当引起阻塞的原因被消除后，线程才可以转入就绪状态。

5. 死亡状态

    线程调用 stop() 方法时或 run() 方法执行结束后，即处于死亡状态。处于死亡状态的线程不具有继续运行的能力。


#### Java 程序每次运行至少启动几个线程？

至少启动两个线程，每当使用 Java 命令执行一个类时，实际上都会启动一个 JVM，每一个JVM实际上就是在操作系统中启动一个线程，Java 本身具备了垃圾的收集机制。所以在 Java 运行时至少会启动两个线程，一个是 main 线程，另外一个是垃圾收集线程。


#### 取得和设置线程的名称

```
//实现Runnable接口
class MyThread implements Runnable
{
    public void run()
    {
        for(int i=0;i<3;i++)
        {
            //取得当前线程的名称
            System.Out.println(Thread.currentThread().getName()+"运行, i="+i);
        }
    }
}

public class ThreadDemo
{
    public static void main(String[] args)
    {
        //定义Runnable子类对象
        MyThread my = new MyThread();
        //系统自动设置线程名称
        new Thread(my).start;
        //手工设置线程名称
        new Thread(my,"线程A").start();
    }
}

```


### 操作线程

#### 强制运行

在线程操作中，可以使用 join()方法让一个线程强制运行，线程强制运行期间，其他线程无法运行，必须等待此线程完成之后才可以继续执行。
`new Thread(myThread, "我的线程").join();`


#### 休眠

在程序中允许一个线程进行暂时的休眠，直接使用Thread.sleep()即可实现休眠。


#### 中断

当一个线程运行时，另外一个线程可以直接通过interrupt()方法中断其运行状态。

#### 后台线程

在 Java 程序中，只要前台有一个线程在运行，则整个 Java 进程都不会消失，所以此时可以设置一个后台线程，这样即使 Java 线程结束了，此后台线程依然会继续执行，要想实现这样的操作，直接使用 setDaemon() 方法即可。

```
// 实现Runnable接口
class MyThread implements Runnable
{
    // 覆写run()方法
    public void run()
    {
        while(true)
        {
            System.out.println(Thread.currentThread().getName() + "在运行。") ;
        }
    }
}

public class ThreadDaemonDemo
{
    public static void main(String[] args)
    {
        // 实例化Runnable子类对象
        MyThread mt = new MyThread();
        // 实例化Thread对象
        Thread t = new Thread(mt,"线程");
        // 此线程在后台运行
        t.setDaemon(true);
        // 启动线程
        t.start();
    }
}

```

#### 线程的优先级

在 Java 的线程操作中，所有的线程在运行前都会保持在就绪状态，那么此时，哪个线程的优先级高，哪个线程就有可能会先被执行。

```
// 实现Runnable接口
class MyThread implements Runnable
{
    // 覆写run()方法
    public void run()
    {
        for(int i=0;i<5;i++)
        {
            try
            {
                // 线程休眠
                Thread.sleep(500);
            }
            catch(InterruptedException e)
            {
                // 取得当前线程的名字
                System.out.println(Thread.currentThread().getName() + "运行，i = " + i);
            }
        }
    }
}

public class ThreadPriorityDemo{
    public static void main(String[] args)
    {
        // 实例化线程对象
        Thread t1 = new Thread(new MyThread(),"线程A");
        Thread t2 = new Thread(new MyThread(),"线程B");
        Thread t3 = new Thread(new MyThread(),"线程C");
        // 优先级最低
        t1.setPriority(Thread.MIN_PRIORITY);
        // 优先级最高
        t2.setPriority(Thread.MAX_PRIORITY);
        // 优先级最中等
        t3.setPriority(Thread.NORM_PRIORITY);
        // 启动线程
        t1.start();
        t2.start();
        t3.start();
    }
}

```

#### 礼让

在线程操作中，也可以使用 yield() 方法将一个线程的操作暂时让给其他线程执行。

```
// 实现Runnable接口
class MyThread implements Runnable
{
    // 覆写run()方法
    public void run()
    {
        for(int i=0;i<5;i++)
        {
            try
            {
                Thread.sleep(500) ;
            }
            catch(Exception e)
            {}
            System.out.println(Thread.currentThread().getName() + "运行，i = " + i) ;  // 取得当前线程的名字
            if(i==2)
            {
                System.out.print("线程礼让：");
                // 线程礼让
                Thread.yield();
            }
        }
    }
}

public class ThreadYieldDemo
{
    public static void main(String[] args)
    {
        // 实例化MyThread对象
        MyThread my = new MyThread();
        Thread t1 = new Thread(my,"线程A");
        Thread t2 = new Thread(my,"线程B");
        t1.start();
        t2.start();
    }
}

```


### 同步与死锁

一个多线程的程序如果是通过 Runnable 接口实现的，则意味着类中的属性被多个线程共享，那么这样就会造成一种问题，如果这多个线程要操作同一个资源时就有可能出现资源同步问题。

* 同步代码块

    ```
    synchronized(同步对象)
    {
        需要同步的代码;
    }
    ```

* 同步方法

    可以使用 synchronized 关键字将一个方法声明为同步方法。

    ```
    synchronized 方法返回值 方法名称(参数列表){};
    ```

* 死锁

    同步可以保证资源共享操作的正确性，但是过多同步也会产生问题。例如，现在张三想要李四的画，李四想要张三的书，张三对李四说“把你的画给我，我就给你书”，李四也对张三说“把你的书给我，我就给你画”两个人互相等对方先行动，就这么干等没有结果，这实际上就是死锁的概念。
    所谓死锁，就是两个线程都在等待对方先完成，造成程序的停滞，一般程序的死锁都是在程序运行时出现的。


## 引用

强引用是最普通的引用。如果一个对象被强引用，其绝对不会被 JVM 回收。可以通过将引用设为 null 解除。

### 软引用

内存空间充足时，不会被回收；不足时，会被回收。使用方式如下：

```java
// 强引用
String strongReference = new String("abc");
// 软引用
String str = new String("abc");
SoftReference<String> softReference = new SoftReference<String>(str);
```

通常和引用队列(ReferenceQueue)联合使用。如果软引用所引用对象被垃圾回收，JVM 会把这个软引用加入到与之关联的引用队列中。

```java
ReferenceQueue<String> referenceQueue = new ReferenceQueue<>();
String str = new String("abc");
SoftReference<String> softReference = new SoftReference<>(str, referenceQueue);

// 主动释放引用，促进回收
str = null;
// 提醒 GC，不一定会回收，只有内存不够时才回收。
System.gc();

System.out.println(softReference.get()); //输出 abc

Reference<? extends String> reference = referenceQueue.poll();
System.out.println(reference); //假设软引用已经被回收，则输出 null
```


### 弱引用

比软引用生命更短，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。

示例如下：

```java
String str = new String("abc");
WeakReference<String> weakReference = new WeakReference<>(str);
str = null;
```

弱引用可以变为强引用：

```java
String str = new String("abc");
WeakReference<String> weakReference = new WeakReference<>(str);
// 弱引用转强引用
String strongReference = weakReference.get();
```

同样，弱引用可以和一个引用队列(ReferenceQueue)联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。

### 虚引用

最弱的引用，与没引用没区别。

虚引用主要用来跟踪对象被垃圾回收器回收的活动。

虚引用与软引用和弱引用的一个区别在于，虚引用必须和引用队列(ReferenceQueue)联合使用。当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。

示例如下：

```java
String str = new String("abc");
ReferenceQueue queue = new ReferenceQueue();
// 创建虚引用，要求必须与一个引用队列关联
PhantomReference pr = new PhantomReference(str, queue);
```

程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要进行垃圾回收。如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。



## JUC


在 Java 5.0 提供了 java.util.concurrent(简称JUC)包，在此包中增加了在并发编程中很常用的工具类，用于定义类似于线程的自定义子系统，包括线程池，异步 IO 和轻量级任务框架；还提供了设计用于多线程上下文中的 Collection 实现等。

### 理论基础

并发三要素：可见性、原子性、有序性

可见性：cpu加缓存导致的

原子性：进程和线程分时复用cpu导致

有序性：编译器优化导致的。编译器优化、指令级优化、内存优化

解决并发问题：

sychronlize、ReentranLock：可见性、原子性、有序性

voilate：可见性、一定的有序性、部分原子性

JMM的Happens-before原则：有序性

JVM保证基本数据类型的读取和赋值是原子操作。

线程的安全程度分级：
不可变：final修饰的基本数据类型、String、枚举、Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。但同为 Number 的原子类 AtomicInteger 和 AtomicLong 则是可变的。对于集合类型，可以使用 Collections.unmodifiableXXX() 方法来获取一个不可变的集合。
绝对线程安全：
相对线程安全：有些情况会不安全。在 Java 语言中，大部分的线程安全类都属于这种类型，例如 Vector、HashTable、Collections 的 synchronizedCollection() 方法包装的集合等。
线程兼容：要手动上锁。ArrayList 和 HashMap 等
线程对立：

互斥同步（阻塞同步）：悲观的，sychronized、ReentronLock
非阻塞同步：乐观的，无锁同步方案，java.util.atomic使用的。原子类->CAS->unsafe
无同步方案：炸封闭、Thread Local、可重入代码

原子类：使用CAS+violate实现并发安全的，CAS保证原子性，violate保证可见性。有13个延申类。操作基本数据类型、数组、对象。

### 线程基础

currentTread()

join() yield() intertupt() interupted() wait() notify() notifyAll() sleep() await() signal() signalAll()

### Synchronized、violate、final

### TreadLocal

为每个线程保存共享变量的副本。每个ThreadLocal都有一个ThreadLocalMap，key就是当前线程。

### CAS

compare and swap，基于硬件的非阻塞同步。

CAS 指令需要有 3 个操作数，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B。自旋等待的，自旋锁也是通过它实现的。

实现：

对于基本数据类型，再 unsafe 类中有实现。AtomicInteger、AtomicLong
对于引用数据类型，在 AtomicReference 中。


导致的问题：

ABA问题：可以通过 AtomicStampedReference，内置一个版本号。AtomicMarkableReference，它不是维护一个版本号，而是维护一个boolean类型的标记，标记值有修改，了解一下。

循环时间长开销大:

只能保证一个共享变量的原子操作：Java 1.5开始，JDK提供了AtomicReference类来保证引用对象之间的原子性，可以把多个变量放在一个对象里来进行CAS操作


### Unsafe

Unsafe是位于sun.misc包下的一个类，主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等

Unsafe类中发现，原子操作其实只支持下面三个方法。

```java
public final native boolean compareAndSwapObject(Object paramObject1, long paramLong, Object paramObject2, Object paramObject3);

public final native boolean compareAndSwapInt(Object paramObject, long paramLong, int paramInt1, int paramInt2);

public final native boolean compareAndSwapLong(Object paramObject, long paramLong1, long paramLong2, long paramLong3);
```

### Atomic



### AQS

使用了模板方法设计模式

AQS定义两种资源共享方式 Exclusive(独占)：只有一个线程能执行，如ReentrantLock。又可分为公平锁和非公平锁：
公平锁：按照线程在队列中的排队顺序，先到者先拿到锁 非公平锁：当线程要获取锁时，无视队列顺序直接去抢锁，谁抢到就是谁的 Share(共享)：多个线程可同时执行，如Semaphore/CountDownLatch。Semaphore、CountDownLatCh、 CyclicBarrier、ReadWriteLock 我们都会在后面讲到。

自定义时需要实现以下方法，默认情况下，每个方法都抛出 UnsupportedOperationException。：

```java
isHeldExclusively()//该线程是否正在独占资源。只有用到condition才需要去实现它。
tryAcquire(int)//独占方式。尝试获取资源，成功则返回true，失败则返回false。
tryRelease(int)//独占方式。尝试释放资源，成功则返回true，失败则返回false。
tryAcquireShared(int)//共享方式。尝试获取资源。负数表示失败；0表示成功，但没有剩余可用资源；正数表示成功，且有剩余资源。
tryReleaseShared(int)//共享方式。尝试释放资源，成功则返回true，失败则返回false。

```

CLH：一个虚拟的双向队列(虚拟的双向队列即不存在队列实例，仅存在结点之间的关联关系)。AQS是将每条请求共享资源的线程封装成一个CLH锁队列的一个结点(Node)来实现锁的分配。其中Sync queue，即同步队列，是双向链表，包括head结点和tail结点，head结点主要用作后续的调度。而Condition queue不是必须的，其是一个单向链表，只有当使用Condition时，才会存在此单向链表。并且可能会有多个Condition queue。

Sync：

### ReentrantLock

是通过关联关系来实现 AQS的，有三个继承 AQS 的子类： Sync、NonfairSync、FairSync。

默认不公平，可以设置为公平。

#### synchronized 和 ReentrantLock 的异同

两者的共同点：
1. 都是用来协调多线程对共享对象、变量的访问
2. 都是可重入锁，同一线程可以多次获得同一个锁
3. 都保证了可见性和互斥性
两者的不同点：
1. ReentrantLock 显示的获得、释放锁，synchronized 隐式获得释放锁
2. ReentrantLock 可响应中断、可轮回，synchronized 是不可以响应中断的，为处理锁的
不可用性提供了更高的灵活性
3. ReentrantLock 是 API 级别的，synchronized 是 JVM 级别的
4. ReentrantLock 可以实现公平锁
5. ReentrantLock 通过 Condition 可以绑定多个条件
6. 底层实现不一样， synchronized 是同步阻塞，使用的是悲观并发策略，lock 是同步非阻
塞，采用的是乐观并发策略
7. Lock 是一个接口，而 synchronized 是 Java 中的关键字，synchronized 是内置的语言
实现。
8. synchronized 在发生异常时，会自动释放线程占有的锁，因此不会导致死锁现象发生；
而 Lock 在发生异常时，如果没有主动通过 unLock()去释放锁，则很可能造成死锁现象，
因此使用 Lock 时需要在 finally 块中释放锁。
9. Lock 可以让等待锁的线程响应中断，而 synchronized 却不行，使用 synchronized 时，
等待的线程会一直等待下去，不能够响应中断。
10. 通过 Lock 可以知道有没有成功获取锁，而 synchronized 却无法办到。
11. Lock 可以提高多个线程进行读操作的效率，既就是实现读写锁等

### ReentranReadWriteLock


## I\O

I\O：输入和输出的简称，主要通过流实现。

Stream：流，一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。

### 流的分类

按数据流的方向不同：输入流，输出流。

按处理数据单位不同：字节流，字符流。

1. 字节流：数据流中最小的数据单元是字节。

2. 字符流：数据流中最小的数据单元是字符， Java中的字符是Unicode编码，一个字符占用两个字节。

按功能不同：节点流，处理流。

1. 程序用于直接操作目标设备所对应的类叫节点流。

2. 程序通过一个间接流类去调用节点流类，以达到更加灵活方便地读写各种类型的数据，这个间接流类就是处理流。

Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。

Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。

这些流都是由四个类衍生出来的：InputStream、OutputStream、Reader、Writer。

流的分类示意图：

![Java IO](vx_images/3095135210851.png)

![Java IO (1)](vx_images/4664035229277.png)

既然有了字节流,为什么还要有字符流？

问题本质：不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么I/O流操作要分为字节流操作和字符流操作呢?

回答：字符流是由Java 虚拟机将字节转换得到的，问题就出在这个过程还算是非常耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。所以，I/O流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。


### BIO、NIO、AIO

BIO：同步阻塞IO

NIO：同步非阻塞IO

AIO：异步IO

Java 中的 BIO、NIO和 AIO 理解为是 Java 语言对操作系统的各种 IO 模型的封装。程序员在使用这些 API 的时候，不需要关心操作系统层面的知识，也不需要根据不同操作系统编写不同的代码。只需要使用Java的API就可以了。

在讲 BIO,NIO,AIO 之前先来回顾一下这样几个概念：同步与异步，阻塞与非阻塞。

同步与异步：

* 同步： 同步就是发起一个调用后，被调用者未处理完请求之前，调用不返回。

* 异步： 异步就是发起一个调用后，立刻得到被调用者的回应表示已接收到请求，但是被调用者并没有返回结果，此时我们可以处理其他的请求，被调用者通常依靠事件，回调等机制来通知调用者其返回结果。

同步和异步的区别最大在于异步的话调用者不需要等待处理结果，被调用者会通过回调等机制来通知调用者其返回结果。

阻塞和非阻塞：

* 阻塞： 阻塞就是发起一个请求，调用者一直等待请求结果返回，也就是当前线程会被挂起，无法从事其他任务，只有当条件就绪才能继续。

* 非阻塞： 非阻塞就是发起一个请求，调用者不用一直等着结果返回，可以先去干其他事情。

那么同步阻塞、同步非阻塞和异步非阻塞又代表什么意思呢？

举个生活中简单的例子，你妈妈让你烧水，小时候你比较笨啊，在哪里傻等着水开（同步阻塞）。等你稍微再长大一点，你知道每次烧水的空隙可以去干点其他事，然后只需要时不时来看看水开了没有（同步非阻塞）。后来，你们家用上了水开了会发出声音的壶，这样你就只需要听到响声后就知道水开了，在这期间你可以随便干自己的事情，你需要去倒水了（异步非阻塞）。




### 控制台输入输出

Java 的控制台输入由 System.in 完成。

为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。

下面是创建 BufferedReader 的基本语法：

```java
BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
```

BufferedReader 对象创建后，我们便可以使用 read() 方法从控制台读取一个字符，或者用 readLine() 方法读取一个字符串。

#### 从控制台读取多字符输入

从 BufferedReader 对象读取一个字符要使用 read() 方法，它的语法如下：

```java
int read( ) throws IOException
```

每次调用 read() 方法，它从输入流读取一个字符并把该字符作为整数值返回。 当流结束的时候返回 -1。该方法抛出 IOException。

下面的程序示范了用 read() 方法从控制台不断读取字符直到用户输入 q。

```java
//使用 BufferedReader 在控制台读取字符

import java.io.*;

public class BRRead {
    public static void main(String[] args) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}
```

#### 从控制台读取字符串

从标准输入读取一个字符串需要使用 BufferedReader 的 readLine() 方法。

它的一般格式是：

```java
String readLine( ) throws IOException
```

下面的程序读取和显示字符行直到你输入了单词"end"。

```java
//使用 BufferedReader 在控制台读取字符
import java.io.*;
 
public class BRReadLines {
    public static void main(String[] args) throws IOException {
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str;
        System.out.println("Enter lines of text.");
        System.out.println("Enter 'end' to quit.");
        do {
            str = br.readLine();
            System.out.println(str);
        } while (!str.equals("end"));
    }
}
```

### 控制台输出

控制台的输出由 print( ) 和 println() 完成。这些方法都由类 PrintStream 定义，System.out 是该类对象的一个引用。

PrintStream 继承了 OutputStream类，并且实现了方法 write()。这样，write() 也可以用来往控制台写操作。

PrintStream 定义 write() 的最简单格式如下所示：

```java
void write(int byteval)
```

该方法将 byteval 的低八位字节写到流中。

下面的例子用 write() 把字符 "A" 和紧跟着的换行符输出到屏幕：

```java
import java.io.*;

//演示 System.out.write().
public class WriteDemo {
    public static void main(String[] args) {
        int b;
        b = 'A';
        System.out.write(b);
        System.out.write('\n');
    }
}
```

注意：write() 方法不经常使用，因为 print() 和 println() 方法用起来更为方便。


### 读写文件

#### FileInputStream

该流用于从文件读取数据，它的对象可以用关键字 new 来创建。

有多种构造方法可用来创建对象。

可以使用字符串类型的文件名来创建一个输入流对象来读取文件：

```java
InputStream f = new FileInputStream("C:/java/hello");
```

也可以使用一个文件对象来创建一个输入流对象来读取文件。我们首先得使用 File() 方法来创建一个文件对象：

```java
File f = new File("C:/java/hello");
InputStream in = new FileInputStream(f);
```

创建了InputStream对象，就可以使用下面的方法来读取流或者进行其他的流操作。

|                      方法                       |                                               描述                                               |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| public void close() throws IOException{}       | 关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常。                                 |
| protected void finalize()throws IOException {} | 这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。         |
| public int read(int r)throws IOException{}     | 这个方法从 InputStream 对象读取指定字节的数据。返回为整数值。返回下一字节数据，如果已经到结尾则返回-1。 |
| public int read(byte[] r) throws IOException{} | 这个方法从输入流读取r.length长度的字节。返回读取的字节数。如果是文件结尾则返回-1。                     |
| public int available() throws IOException{}    | 返回下一次对此输入流调用的方法可以不受阻塞地从此输入流读取的字节数。返回一个整数值。                    |

除了 InputStream 外，还有一些其他的输入流：

* ByteArrayInputStream

* DataInputStream

#### FileOutputStream

该类用来创建一个文件并向文件中写数据。

如果该流在打开文件进行输出前，目标文件不存在，那么该流会创建该文件。

有两个构造方法可以用来创建 FileOutputStream 对象。

使用字符串类型的文件名来创建一个输出流对象：

```java
OutputStream f = new FileOutputStream("C:/java/hello")
```

也可以使用一个文件对象来创建一个输出流来写文件。我们首先得使用File()方法来创建一个文件对象：

```java
File f = new File("C:/java/hello");
OutputStream fOut = new FileOutputStream(f);
```

创建OutputStream 对象完成后，就可以使用下面的方法来写入流或者进行其他的流操作。

|                      方法                       |                                           描述                                           |
| ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| public void close() throws IOException{}       | 关闭此文件输入流并释放与此流有关的所有系统资源。抛出IOException异常。                         |
| protected void finalize()throws IOException {} | 这个方法清除与该文件的连接。确保在不再引用文件输入流时调用其 close 方法。抛出IOException异常。 |
| public void write(int w)throws IOException{}   | 这个方法把指定的字节写到输出流中。                                                          |
| public void write(byte[] w)                    | 把指定数组中w.length长度的字节写到OutputStream中。                                         |

除了OutputStream外，还有一些其他的输出流：

* ByteArrayOutputStream

* DataOutputStream

#### 示例

```java
import java.io.*;
 
public class fileStreamTest {
    public static void main(String[] args) {
        try {
            byte bWrite[] = { 11, 21, 3, 40, 5 };
            OutputStream os = new FileOutputStream("test.txt");
            for (int x = 0; x < bWrite.length; x++) {
                os.write(bWrite[x]); // writes the bytes
            }
            os.close();
 
            InputStream is = new FileInputStream("test.txt");
            int size = is.available();
 
            for (int i = 0; i < size; i++) {
                System.out.print((char) is.read() + "  ");
            }
            is.close();
        } catch (IOException e) {
            System.out.print("Exception");
        }
    }
}
```

上面的程序首先创建文件test.txt，并把给定的数字以二进制形式写进该文件，同时输出到控制台上。

以上代码由于是二进制写入，可能存在乱码，你可以使用以下代码实例来解决乱码问题：

```java
//文件名 :fileStreamTest2.java
import java.io.*;
 
public class fileStreamTest2 {
    public static void main(String[] args) throws IOException {
 
        File f = new File("a.txt");
        FileOutputStream fop = new FileOutputStream(f);
        // 构建FileOutputStream对象,文件不存在会自动新建
 
        OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
        // 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk
 
        writer.append("中文输入");
        // 写入到缓冲区
 
        writer.append("\r\n");
        // 换行
 
        writer.append("English");
        // 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入
 
        writer.close();
        // 关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉
 
        fop.close();
        // 关闭输出流,释放系统资源
 
        FileInputStream fip = new FileInputStream(f);
        // 构建FileInputStream对象
 
        InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
        // 构建InputStreamReader对象,编码与写入相同
 
        StringBuffer sb = new StringBuffer();
        while (reader.ready()) {
            sb.append((char) reader.read());
            // 转成char加到StringBuffer对象中
        }
        System.out.println(sb.toString());
        reader.close();
        // 关闭读取流
 
        fip.close();
        // 关闭输入流,释放系统资源
 
    }
}
```

还有一些关于文件和I/O的类，我们也需要知道：

* File.Class

* FileReader.Class

* FileWriter.Class


### 文件和 IO

#### 创建目录

File类中有两个方法可以用来创建文件夹：

mkdir( )方法创建一个文件夹，成功则返回true，失败则返回false。失败表明File对象指定的路径已经存在，或者由于整个路径还不存在，该文件夹不能被创建。

mkdirs()方法创建一个文件夹和它的所有父文件夹。

下面的例子创建 "/tmp/user/java/bin"文件夹：

```java
import java.io.File;
 
public class CreateDir {
    public static void main(String[] args) {
        String dirname = "/tmp/user/java/bin";
        File d = new File(dirname);
        // 现在创建目录
        d.mkdirs();
    }
}
```

编译并执行上面代码来创建目录 "/tmp/user/java/bin"。

注意： Java 在 UNIX 和 Windows 自动按约定分辨文件路径分隔符。如果你在 Windows 版本的 Java 中使用分隔符 (/) ，路径依然能够被正确解析。

#### 读取目录

一个目录其实就是一个 File 对象，它包含其他文件和文件夹。

如果创建一个 File 对象并且它是一个目录，那么调用 isDirectory() 方法会返回 true。

可以通过调用该对象上的 list() 方法，来提取它包含的文件和文件夹的列表。

下面展示的例子说明如何使用 list() 方法来检查一个文件夹中包含的内容：

```java
import java.io.File;

public class DirList {
    public static void main(String args[]) {
        String dirname = "/tmp";
        File f1 = new File(dirname);
        if (f1.isDirectory()) {
            System.out.println("目录 " + dirname);
            String s[] = f1.list();
            for (int i = 0; i < s.length; i++) {
                File f = new File(dirname + "/" + s[i]);
                if (f.isDirectory()) {
                    System.out.println(s[i] + " 是一个目录");
                } else {
                    System.out.println(s[i] + " 是一个文件");
                }
            }
        } else {
            System.out.println(dirname + " 不是一个目录");
        }
    }
}
```


#### 删除目录或文件

删除文件可以使用 java.io.File.delete() 方法。

以下代码会删除目录 /tmp/java/，需要注意的是当删除某一目录时，必须保证该目录下没有其他文件才能正确删除，否则将删除失败。

测试目录结构：

```cmd
/tmp/java/
|-- 1.log
|-- test
```

```java
import java.io.File;
 
public class DeleteFileDemo {
    public static void main(String[] args) {
        // 这里修改为自己的测试目录
        File folder = new File("/tmp/java/");
        deleteFolder(folder);
    }
 
    // 删除文件及目录
    public static void deleteFolder(File folder) {
        File[] files = folder.listFiles();
        if (files != null) {
            for (File f : files) {
                if (f.isDirectory()) {
                    deleteFolder(f);
                } else {
                    f.delete();
                }
            }
        }
        folder.delete();
    }
}
```


### 管道

管道操作可以使用 PipedReader、PipedWriter、PipedInputStream、PipedOutputStream。

PipedReader 和 PipedWriter即管道输入流和输出流，可用于线程间管道通信。它们和 PipedInputStream/PipedOutputStream 区别是前者操作的是字符后者是字节。

#### 方法介绍

PipedReader提供的API如下：

```java
//构造方法
PipedReader(PipedWriter src)    //使用默认的buf的大小和传入的pw构造pr
PipedReader(PipedWriter src, int pipeSize)      //使用指定的buf的大小和传入的pw构造pr
PipedReader()       //使用默认大小构造pr
PipedReader(int pipeSize)       //使用指定大小构造pr

//关闭流
void close()
//绑定Writer
void connect(PipedWriter src)
//是否可读
synchronized boolean ready()
//读取一个字符
synchronized int read()
//读取多个字符到cbuf
synchronized int read(char cbuf[], int off, int len)
//Writer调用, 向Reader缓冲区写数据
synchronized void receive(int c)
synchronized void receive(char c[], int off, int len)
synchronized void receivedLast()
```

PipedWriter提供的API如下：

```java
//构造方法
PipedWriter(PipedReader snk)
PipedWriter()

//绑定Reader Writer
synchronized void connect(PipedReader snk)
//关闭流
void close()
//刷新流,唤醒Reader
synchronized void flush()
//写入1个字符,实际是写到绑定Reader的缓冲区
void write(int c)
//写入多个字符到Reader缓冲区
void write(char cbuf[], int off, int len)
```

#### 示例

```java
/**
 * 写线程
 */
public class Producer extends Thread {
    //输出流
    private PipedWriter writer = new PipedWriter();
    public Producer(PipedWriter writer) {
        this.writer = writer;
    }

    @Override
    public void run() {
        try {
            StringBuilder sb = new StringBuilder();
            sb.append("Hello World!");
            writer.write(sb.toString());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

/**
 * 读取线程
 */
public class Consumer extends Thread{
    //输入流
    private PipedReader reader = new PipedReader();

    public Consumer(PipedReader reader) {
        this.reader = reader;
    }

    @Override
    public void run() {
        try {
            char [] cbuf = new char[20];
            reader.read(cbuf, 0, cbuf.length);
            System.out.println("管道流中的数据为: " + new String(cbuf));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

@org.junit.Test
public void testPipedReaderWriter() {
    /**
     * 管道流通信核心是,Writer和Reader公用一块缓冲区,缓冲区在Reader中申请,
     * 由Writer调用和它绑定的Reader的Receive方法进行写.
     *
     * 线程间通过管道流通信的步骤为
     * 1 建立输入输出流
     * 2 绑定输入输出流
     * 3 Writer写
     * 4 Reader读
     */
    PipedReader reader = new PipedReader();
    PipedWriter writer = new PipedWriter();
    Producer producer = new Producer(writer);
    Consumer consumer = new Consumer(reader);

    try {
        writer.connect(reader);
        producer.start();
        consumer.start();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

[源码分析](https://blog.csdn.net/sk199048/article/details/51260757)


### 合并流

合并流使用 SequenceInputStream。

两个流合并时：

```java
package stream.sequence;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.SequenceInputStream;

/**
 * SequenceInputStream 表示其他输入流的逻辑串联。
 * 它从输入流的有序集合开始，并从第一个输入流开始读取，直到到达文件末尾；
 * 接着从第二个输入流读取，依次类推，直到到达包含的最后一个输入流的文件末尾为止。
 *
 * @author 半步疯子
 *
 */
public class SequenceInputStreamDemo01 {

    // SequenceInputStream(InputStream s1, InputStream s2) 

	public static void main(String[] args) throws IOException {
		InputStream s1 = new FileInputStream("src/stream/sequence/SequenceInputStreamDemo01.java");
		InputStream s2 = new FileInputStream("src/stream/sequence/SequenceInputStreamDemo02.java");

		SequenceInputStream sis = new SequenceInputStream(s1, s2);

		InputStreamReader isr = new InputStreamReader(sis);
		BufferedReader br = new BufferedReader(isr);

		BufferedWriter bw = new BufferedWriter(new FileWriter("CopySequence.java"));

		String line = null;
		while((line = br.readLine()) != null) {
		// while(br.ready()) {  /* 为什么合并流之后，不能使用ready方法？结果只有S1 */
			// line = br.readLine();
			bw.write(line);
			bw.newLine();
			bw.flush();
		}
		s1.close();
		s2.close();
		br.close();
		bw.close();

		/*
		BufferedReader br = new BufferedReader(
				new InputStreamReader(
						new SequenceInputStream(
								new FileInputStream("src/stream/sequence/SequenceInputStreamDemo01.java"), 
								new FileInputStream("src/special/RandomAccessFileDemo02.java")
								)
						)
				);
		BufferedWriter bw = new BufferedWriter(new FileWriter("CopySequence.java"));
		br.close();
		bw.close();
		*/
	}
}
```

多个流的时候存放到Vector中后进行合并：

```java
package stream.sequence;

import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.SequenceInputStream;
import java.util.Vector;

/**
 * SequenceInputStream 表示其他输入流的逻辑串联。
 * 它从输入流的有序集合开始，并从第一个输入流开始读取，直到到达文件末尾；
 * 接着从第二个输入流读取，依次类推，直到到达包含的最后一个输入流的文件末尾为止。
 *
 * @author 半步疯子
 *
 */
public class SequenceInputStreamDemo02 {

	// SequenceInputStream(Enumeration<? extends InputStream> e)
	// 通过枚举类：进行多个流的合并
	public static void main(String[] args) throws IOException {
		InputStream s1 = new FileInputStream("src/stream/sequence/SequenceInputStreamDemo02.java");
		InputStream s2 = new FileInputStream("src/stream/sequence/SequenceInputStreamDemo01.java");
		InputStream s3 = new FileInputStream("src/stream/sequence/SequenceInputStreamDemo02.java");

		Vector<InputStream> v = new Vector<InputStream>();
		v.addElement(s1);
		v.addElement(s2);
		v.addElement(s3);
		SequenceInputStream sis = new SequenceInputStream(v.elements());

		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copySequence02.java"));

		byte[] buf = new byte[1024];
		int len = buf.length;
		while((len = sis.read(buf, 0, len)) != -1) {
			bos.write(buf, 0, len);
		}

		sis.close();
		bos.close();
	}
}
```

### 参考

[菜鸟教程 java io](https://www.runoob.com/java/java-files-io.html)





intStream


## 输入输出

输入输出可以分为标准输入输出、标准错误输入输出。
在java中标准输入为键盘输入，标准输出为屏幕。
其他输入输出有：文件。

### 输入

方法如下：

```java
Scanner cin = new Scanner(System.in)
```

这是一个流式输入。

获取输入内容有如下方法：

|      方法      |                                                       描述                                                       |
| ------------- | ---------------------------------------------------------------------------------------------------------------- |
| hasNext()     | 判断接下来是否有非空字符.如果有,则返回 true,否则返回 false。它会忽略换行符\n直接访问下一行，如果下一行是空行返回false。  |
| hasNextLine() | 根据行匹配模式去判断接下来是否有一行(包括空行),如果有,则返回 true,否则返回 false。它会认为换行符\n是一个空行，返回true。 |
| hasNextInt()  | 判断下一个字符是否为 Int。                                                                                         |
| next()        | 读取一个有效字符，自动去除空白。                                                                                   |
| nextLine()    | 读取下一行字符串，以回车为结束符号，不包括回车。可以获得空行。                                                        |
| nextInt       | 获取下一个 Int，以回车为结束。多个数字用空格区分也能依次识别。如果下一个字符不是 int 就会报错。                         |

下面用一个例子来直观说明 hasNext() 和 hasNextLine() 的区别。重定向标准输入为文件输入流，输入文本如下，注意最后有一个带换行符的空行。

```txt
1 2
3 4

```

如果使用如下代码，可以正常运行。因为 hasNext() 会忽略 \n ，读到第三行时会继续向下读，然后发现文件结束，则返回 false。这样 nextInt() 就不会读取空行。

```java
while(cin.hasNext())
{
    System.out.println(cin.nextInt());
}
```

将代码改为如下，则会报错`NoSuchElementException`。原因是，hasNextLine() 会读取空行，所以读到第三行时，仍然返回 true。然而第三行后面没有下一个 int。

```java
while(cin.hasNextLine())
{
    System.out.println(cin.nextInt());
}
```

注意在 idea 的控制台输入中，hasNext() 和 hasNextLine() 会一直是 true。因为标准输入是一个无限流。可以使用 hasNext() 设置结束符号来结束无限接收。

```java
Scanner cin = new Scanner(System.in);
while(cin.hasNext('#'))
{}
```

示例：

```java
import java.util.Scanner;

public class Main
{
    public static void main(String[] args)
    {
        Scanner cin = new Scanner(System.in);
        int index = 0;
        int ahead = cin.nextInt();
        int after = 0;
        while (cin.hasNext("\n"))
        {
            System.out.println(cin.hasNextLine());
            after = cin.nextInt();
            int temp = ahead-after;
            if(temp<0)
            {
                temp = -temp;
            }
            if(temp>index)
            {
                index=temp;
            }
            ahead = after;
        }
        System.out.println(index);
    }
}
```



### 重定向

语法如下：

```java
FileInputStream fin = new FileInputStream("C:\\Users\\27578\\Desktop\\test.txt");
System.setIn(fin);
Scanner cin = new Scanner(System.in);
```

示例：

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class Main
{
    public static void main(String[] args)
    {

        try
        {
            FileInputStream fin = new FileInputStream("C:\\Users\\27578\\Desktop\\test.txt");
            System.setIn(fin);
            Scanner cin = new Scanner(System.in);
            int index = 0;
            int ahead = cin.nextInt();
            int after = 0;
            while (cin.hasNextLine())
            {
                System.out.println(cin.hasNextLine());
                after = cin.nextInt();
                int temp = ahead-after;
                if(temp<0)
                {
                    temp = -temp;
                }
                if(temp>index)
                {
                    index=temp;
                }
                ahead = after;
            }
            System.out.println(index);
        }
        catch (FileNotFoundException e)
        {
            e.printStackTrace();
        }

    }
}
```

## 格式化

### 日期与格式化

示例代码如下：

```java
import java.util.Date;
import java.util.Calendar;
import java.text.SimpleDateFormat;

public class TestDate
{
    public static void main(String[] args){
        Date now = new Date();
        //可以方便地修改日期格式
        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
        String curDate = dateFormat.format( now );
        System.out.println(curDate);
        /*
        上述代码与下面的等效
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
        System.out.println(df.format(new Date()));// new Date()为获取当前系统时间
        */

        //可以对每个时间域单独修改
        Calendar c = Calendar.getInstance();
        int year = c.get(Calendar.YEAR);
        int month = c.get(Calendar.MONTH);
        int date = c.get(Calendar.DATE);
        int hour = c.get(Calendar.HOUR_OF_DAY);
        int minute = c.get(Calendar.MINUTE);
        int second = c.get(Calendar.SECOND);
        System.out.println(year + "/" + month + "/" + date + " " +hour + ":" +minute + ":" + second);
    }
}
```

同时，可以使用 SimpleDateFormat 将数据库中获取的日期格式化输出：

```java
import java.util.Date;
import java.text.SimpleDateFormat;

// 1
String sqlStr = "select bookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
SimpleDateFormat sdf = new SimpleDateFormat(yy-MM-dd);
// rs 为数据库查询结果集，这里是是 Date 型
System.out.println(df.format(rs.getDate("bookDate")));

// 2 使用convert()
String sqlst = "select convert(varchar(10),bookDate,126) as convertBookDate from roomBook where bookDate between '2007-4-10' and '2007-4-25'";
System.out.println(rs.getString("convertBookDate"));
```


### 格式化输出 double

有下列6种方法：

```java
public class DoubleFormat
{
    @Test
    public void test()
    {
        double pi = 3.1415926;
        // 1
        java.text.DecimalFormat df = new java.text.DecimalFormat("#.##");
        System.out.println(df.format(pi));

        // 2 如果小数部分是0的话，1就只显示整数，2始终显示两位小数
        String piStr = String.valueOf(pi);
        BigDecimal piBD = new BigDecimal(piStr);
        piBD = piBD.setScale(2, BigDecimal.ROUND_HALF_UP);
        System.out.println(piBD);

        // 3 不推荐使用
        long l = Math.round(pi * 100); // 四舍五入
        double ret = l / 100.0; // 注意：使用 100.0 而不是 100
        System.out.println(ret);

        // 4 如果不乘以100，小数部分会被舍弃，而不是四舍五入 不推荐使用
        pi4 = ((int) (pi * 100)) / 100;
        System.out.println(pi4);

        // 5
        DecimalFormat df2 = new DecimalFormat("#.00");
        //df2.setRoundingMode(RoundingMode.HALF_UP);
        System.out.println(df2.format(pi));

        // 6
        System.out.println(String.format("%.2f",pi));
    }
}
```

也可以使用 DoubleFormat 类先将 double 进行修饰。


## 网络编程

网络编程是指编写运行在多个设备（计算机）的程序，这些设备都通过网络连接起来。

java.net 包中 J2SE 的 API 包含有类和接口，它们提供低层次的通信细节。你可以直接使用这些类和接口，来专注于解决问题，而不用关注通信细节。

java.net 包中提供了两种常见的网络协议的支持：

* TCP：TCP（英语：Transmission Control Protocol，传输控制协议） 是一种面向连接的、可靠的、基于字节流的传输层通信协议，TCP 层是位于 IP 层之上，应用层之下的中间层。TCP 保障了两个应用程序之间的可靠通信。通常用于互联网协议，被称 TCP / IP。

* UDP：UDP （英语：User Datagram Protocol，用户数据报协议），位于 OSI 模型的传输层。一个无连接的协议。提供了应用程序之间要发送数据的数据报。由于UDP缺乏可靠性且属于无连接协议，所以应用程序通常必须容许一些丢失、错误或重复的数据包。

### socket 编程

socket，网络套接字。

套接字使用TCP提供了两台计算机之间的通信机制。 客户端程序创建一个套接字，并尝试连接服务器的套接字。

当连接建立时，服务器会创建一个 Socket 对象。客户端和服务器现在可以通过对 Socket 对象的写入和读取来进行通信。

java.net.Socket 类代表一个套接字，并且 java.net.ServerSocket 类为服务器程序提供了一种来监听客户端，并与他们建立连接的机制。

以下步骤在两台计算机之间使用套接字建立TCP连接时会出现：

* 服务器实例化一个 ServerSocket 对象，表示通过服务器上的端口通信。

* 服务器调用 ServerSocket 类的 accept() 方法，该方法将一直等待，直到客户端连接到服务器上给定的端口。

* 服务器正在等待时，一个客户端实例化一个 Socket 对象，指定服务器名称和端口号来请求连接。

* Socket 类的构造函数试图将客户端连接到指定的服务器和端口号。如果通信被建立，则在客户端创建一个 Socket 对象能够与服务器进行通信。

* 在服务器端，accept() 方法返回服务器上一个新的 socket 引用，该 socket 连接到客户端的 socket。

连接建立后，通过使用 I/O 流在进行通信，每一个socket都有一个输出流和一个输入流，客户端的输出流连接到服务器端的输入流，而客户端的输入流连接到服务器端的输出流。

TCP 是一个双向的通信协议，因此数据可以通过两个数据流在同一时间发送.以下是一些类提供的一套完整的有用的方法来实现 socket。

#### ServerSocket 类的方法

服务器应用程序通过使用 java.net.ServerSocket 类以获取一个端口,并且侦听客户端请求。

ServerSocket 类有四个构造方法：

|                                      构造方法                                       |                             描述                              |
| ---------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| public ServerSocket(int port) throws IOException                                   | 创建绑定到特定端口的服务器套接字。                              |
| public ServerSocket(int port, int backlog) throws IOException                      | 利用指定的 backlog 创建服务器套接字并将其绑定到指定的本地端口号。 |
| public ServerSocket(int port, int backlog, InetAddress address) throws IOException | 使用指定的端口、侦听 backlog 和要绑定到的本地 IP 地址创建服务器。 |
| public ServerSocket() throws IOException                                           | 创建非绑定服务器套接字。                                        |

创建非绑定服务器套接字。 如果 ServerSocket 构造方法没有抛出异常，就意味着你的应用程序已经成功绑定到指定的端口，并且侦听客户端请求。

这里有一些 ServerSocket 类的常用方法：

|                        方法                        |                       描述                        |
| ------------------------------------------------- | ------------------------------------------------ |
| public int getLocalPort()                         | 返回此套接字在其上侦听的端口。                      |
| public Socket accept() throws IOException         | 侦听并接受到此套接字的连接。                       |
| public void setSoTimeout(int timeout)             | 通过指定超时值启用/禁用 SO_TIMEOUT，以毫秒为单位。   |
| public void bind(SocketAddress host, int backlog) | 将 ServerSocket 绑定到特定地址（IP 地址和端口号）。 |

#### Socket 类的方法

java.net.Socket 类代表客户端和服务器都用来互相沟通的套接字。客户端要获取一个 Socket 对象通过实例化 ，而 服务器获得一个 Socket 对象则通过 accept() 方法的返回值。

Socket 类有五个构造方法：

|                                                构造方法                                                 |                         描述                         |
| ------------------------------------------------------------------------------------------------------ | --------------------------------------------------- |
| public Socket(String host, int port) throws UnknownHostException, IOException.                         | 创建一个流套接字并将其连接到指定主机上的指定端口号。     |
| public Socket(InetAddress host, int port) throws IOException                                           | 创建一个流套接字并将其连接到指定 IP 地址的指定端口号。   |
| public Socket(String host, int port, InetAddress localAddress, int localPort) throws IOException.      | 创建一个套接字并将其连接到指定远程主机上的指定远程端口。 |
| public Socket(InetAddress host, int port, InetAddress localAddress, int localPort) throws IOException. | 创建一个套接字并将其连接到指定远程地址上的指定远程端口。 |
| public Socket()                                                                                        | 通过系统默认类型的 SocketImpl 创建未连接套接字         |

当 Socket 构造方法返回，并没有简单的实例化了一个 Socket 对象，它实际上会尝试连接到指定的服务器和端口。

下面列出了一些感兴趣的方法，注意客户端和服务器端都有一个 Socket 对象，所以无论客户端还是服务端都能够调用这些方法。

|                                   方法                                   |                       描述                        |
| ----------------------------------------------------------------------- | ------------------------------------------------- |
| public void connect(SocketAddress host, int timeout) throws IOException | 将此套接字连接到服务器，并指定一个超时值。            |
| public InetAddress getInetAddress()                                     | 返回套接字连接的地址。                              |
| public int getPort()                                                    | 返回此套接字连接到的远程端口。                       |
| public int getLocalPort()                                               | 返回此套接字绑定到的本地端口。                       |
| public SocketAddress getRemoteSocketAddress()                           | 返回此套接字连接的端点的地址，如果未连接则返回 null。 |
| public InputStream getInputStream() throws IOException                  | 返回此套接字的输入流。                              |
| public OutputStream getOutputStream() throws IOException                | 返回此套接字的输出流。                              |
| public void close() throws IOException                                  | 关闭此套接字。                                     |


#### InetAddress 类的方法

这个类表示互联网协议(IP)地址。下面列出了 Socket 编程时比较有用的方法：

|                            方法                            |                       描述                        |
| --------------------------------------------------------- | ------------------------------------------------ |
| static InetAddress getByAddress(byte[] addr)              | 在给定原始 IP 地址的情况下，返回 InetAddress 对象。 |
| static InetAddress getByAddress(String host, byte[] addr) | 根据提供的主机名和 IP 地址创建 InetAddress。        |
| static InetAddress getByName(String host)                 | 在给定主机名的情况下确定主机的 IP 地址。             |
| String getHostAddress()                                   | 返回 IP 地址字符串（以文本表现形式）。              |
| String getHostName()                                      | 获取此 IP 地址的主机名。                           |
| static InetAddress getLocalHost()                         | 返回本地主机。                                     |
| String toString()                                         | 将此 IP 地址转换为 String。                        |

#### 示例

如下的 GreetingClient 是一个客户端程序，该程序通过 socket 连接到服务器并发送一个请求，然后等待一个响应。

```java
import java.net.*;
import java.io.*;

public class GreetingClient
{
   public static void main(String [] args)
   {
      String serverName = args[0];
      int port = Integer.parseInt(args[1]);
      try
      {
         System.out.println("连接到主机：" + serverName + " ，端口号：" + port);
         Socket client = new Socket(serverName, port);
         System.out.println("远程主机地址：" + client.getRemoteSocketAddress());
         OutputStream outToServer = client.getOutputStream();
         DataOutputStream out = new DataOutputStream(outToServer);
         // 设置使用utf-8编码字符
         out.writeUTF("Hello from " + client.getLocalSocketAddress());
         InputStream inFromServer = client.getInputStream();
         DataInputStream in = new DataInputStream(inFromServer);
         System.out.println("服务器响应： " + in.readUTF());
         client.close();
      }catch(IOException e)
      {
         e.printStackTrace();
      }
   }
}
```

如下的GreetingServer 程序是一个服务器端应用程序，使用 Socket 来监听一个指定的端口。

```java
import java.net.*;
import java.io.*;

public class GreetingServer extends Thread
{
   private ServerSocket serverSocket;

   public GreetingServer(int port) throws IOException
   {
      serverSocket = new ServerSocket(port);
      serverSocket.setSoTimeout(10000);
   }

   public void run()
   {
      while(true)
      {
         try
         {
            System.out.println("等待远程连接，端口号为：" + serverSocket.getLocalPort() + "...");
            Socket server = serverSocket.accept();
            System.out.println("远程主机地址：" + server.getRemoteSocketAddress());
            DataInputStream in = new DataInputStream(server.getInputStream());
            System.out.println(in.readUTF());
            DataOutputStream out = new DataOutputStream(server.getOutputStream());
            out.writeUTF("谢谢连接我：" + server.getLocalSocketAddress() + "\nGoodbye!");
            server.close();
         }catch(SocketTimeoutException s)
         {
            System.out.println("Socket timed out!");
            break;
         }catch(IOException e)
         {
            e.printStackTrace();
            break;
         }
      }
   }
   public static void main(String [] args)
   {
      int port = Integer.parseInt(args[0]);
      try
      {
         Thread t = new GreetingServer(port);
         t.run();
      }catch(IOException e)
      {
         e.printStackTrace();
      }
   }
}
```

编译以上两个 java 文件代码，并执行以下命令来启动服务，使用端口号为 6066：

```cmd
$ javac GreetingServer.java 
$ java GreetingServer 6066
等待远程连接，端口号为：6066...
```

新开一个命令窗口，执行以上命令来开启客户端：

```cmd
$ javac GreetingClient.java
$ java GreetingClient localhost 6066
连接到主机：localhost ，端口号：6066
远程主机地址：localhost/127.0.0.1:6066
服务器响应： 谢谢连接我：/127.0.0.1:6066
Goodbye!
```

### UDP编程

UDP是无连接的，不需要服务器一直监听某个端口，所以其实没有服务器和客户端的区别。

实现UDP通信主要用到了两个类：DatagramPacket和DatagramSocket。

DatagramSocket 用来表示发送和接收数据包的套接字。

DatagramPacket 表示数据报包，用来实现无连接的包的投递服务。这些数据包选择不同的路由，经过计算机的存储转发，最终到达目的计算机。所以到达的数据包和发送时的顺序不一定会相同。

![UDP服务原理示意图](vx_images/489848140851.png)

#### 示例

服务器端：

```java
/*
 * 服务器端，实现基于UDP的用户登陆
 */
public class UDPServer {
    public static void main(String[] args) throws IOException {
        /*
         * 接收客户端发送的数据
         */
        // 1.创建服务器端DatagramSocket，指定端口
        DatagramSocket socket = new DatagramSocket(8800);
        // 2.创建数据报，用于接收客户端发送的数据
        byte[] data = new byte[1024];// 创建字节数组，指定接收的数据包的大小
        DatagramPacket packet = new DatagramPacket(data, data.length);
        // 3.接收客户端发送的数据
        System.out.println("****服务器端已经启动，等待客户端发送数据");
        // 此方法在接收到数据报之前会一直阻塞
        socket.receive(packet);
        // 4.读取数据
        String info = new String(data, 0, packet.getLength());
        System.out.println("我是服务器，客户端说：" + info);

        /*
         * 向客户端响应数据
         */
        // 1.定义客户端的地址、端口号、数据
        InetAddress address = packet.getAddress();
        int port = packet.getPort();
        byte[] data2 = "欢迎您!".getBytes();
        // 2.创建数据报，包含响应的数据信息
        DatagramPacket packet2 = new DatagramPacket(data2, data2.length, address, port);
        // 3.响应客户端
        socket.send(packet2);
        // 4.关闭资源
        socket.close();
    }
}
```

客户端：

```java
/*
 * 客户端
 */
public class UDPClient {
    public static void main(String[] args) throws IOException {
        /*
         * 向服务器端发送数据
         */
        // 1.定义服务器的地址、端口号、数据
        InetAddress address = InetAddress.getByName("localhost");
        int port = 8800;
        byte[] data = "用户名：admin;密码：123".getBytes();
        // 2.创建数据报，包含发送的数据信息
        DatagramPacket packet = new DatagramPacket(data, data.length, address, port);
        // 3.创建DatagramSocket对象
        DatagramSocket socket = new DatagramSocket();
        // 4.向服务器端发送数据报
        socket.send(packet);

        /*
         * 接收服务器端响应的数据
         */
        // 1.创建数据报，用于接收服务器端响应的数据
        byte[] data2 = new byte[1024];
        DatagramPacket packet2 = new DatagramPacket(data2, data2.length);
        // 2.接收服务器响应的数据
        socket.receive(packet2);
        // 3.读取数据
        String reply = new String(data2, 0, packet2.getLength());
        System.out.println("我是客户端，服务器说：" + reply);
        // 4.关闭资源
        socket.close();
    }
}
```

### URL处理

## 序列化

[参考](https://www.cnblogs.com/9dragon/p/10901448.html)

### 序列化的意义

* 序列化：将对象写入到IO流中

* 反序列化：从IO流中恢复对象

* 意义：序列化机制允许将实现序列化的Java对象转换位字节序列，这些字节序列可以保存在磁盘上，或通过网络传输，以达到以后恢复成原来的对象。序列化机制使得对象可以脱离程序的运行而独立存在。

* 使用场景：所有可在网络上传输的对象都必须是可序列化的，比如RMI（remote method invoke,即远程方法调用），传入的参数或返回的对象都是可序列化的，否则会出错；所有需要保存到磁盘的java对象都必须是可序列化的。通常建议：程序创建的每个JavaBean类都实现Serializeable接口。

### 序列化的实现方法

如果需要将某个对象保存到磁盘上或者通过网络传输，那么这个类应该实现Serializable接口或者Externalizable接口之一。

#### Serializable

##### 普通序列化

Serializable接口是一个标记接口，不用实现任何方法。一旦实现了此接口，该类的对象就是可序列化的。

序列化：

```java
public class Person implements Serializable {
  private String name;
  private int age;
  // 我不提供无参构造器
  public Person(String name, int age) {
      this.name = name;
      this.age = age;
  }

  @Override
  public String toString() {
      return "Person{" +
              "name='" + name + '\'' +
              ", age=" + age +
              '}';
  }
}

public class WriteObject {
  public static void main(String[] args) {
      try (//创建一个ObjectOutputStream输出流
           ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("object.txt"))) {
          //将对象序列化到文件s
          Person person = new Person("9龙", 23);
          oos.writeObject(person);
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
}
```

反序列化：

```java
public class Person implements Serializable {
  private String name;
  private int age;
  //我不提供无参构造器
  public Person(String name, int age) {
      System.out.println("反序列化，你调用我了吗？");
      this.name = name;
      this.age = age;
  }

  @Override
  public String toString() {
      return "Person{" +
              "name='" + name + '\'' +
              ", age=" + age +
              '}';
  }
}

public class ReadObject {
  public static void main(String[] args) {
      try (//创建一个ObjectInputStream输入流
           ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.txt"))) {
          Person brady = (Person) ois.readObject();
          System.out.println(brady);
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
}
//输出结果
//Person{name='9龙', age=23}
```

反序列化并不会调用构造方法。反序列的对象是由JVM自己生成的对象，不通过构造方法生成。

##### 成员是引用的序列化

如果一个可序列化的类的成员不是基本类型，也不是String类型，那这个引用类型也必须是可序列化的；否则，会导致此类不能序列化。

##### 同一对象序列化多次的机制

同一对象序列化多次，会将这个对象序列化多次吗？答案是否定的。

```java
public class WriteTeacher {
    public static void main(String[] args) throws Exception {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("teacher.txt"))) {
            Person person = new Person("路飞", 20);
            Teacher t1 = new Teacher("雷利", person);
            Teacher t2 = new Teacher("红发香克斯", person);
            //依次将4个对象写入输入流
            oos.writeObject(t1);
            oos.writeObject(t2);
            oos.writeObject(person);
            oos.writeObject(t2);
        }
    }
}
```

依次将t1、t2、person、t2对象序列化到文件teacher.txt文件中。

```java
public class ReadTeacher {
    public static void main(String[] args) {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("teacher.txt"))) {
            Teacher t1 = (Teacher) ois.readObject();
            Teacher t2 = (Teacher) ois.readObject();
            Person p = (Person) ois.readObject();
            Teacher t3 = (Teacher) ois.readObject();
            System.out.println(t1 == t2);
            System.out.println(t1.getPerson() == p);
            System.out.println(t2.getPerson() == p);
            System.out.println(t2 == t3);
            System.out.println(t1.getPerson() == t2.getPerson());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
//输出结果
//false
//true
//true
//true
//true
```

从输出结果可以看出，Java序列化同一对象，并不会将此对象序列化多次得到多个对象。

* Java序列化算法

    1. 所有保存到磁盘的对象都有一个序列化编码号

    2. 当程序试图序列化一个对象时，会先检查此对象是否已经序列化过，只有此对象从未（在此虚拟机）被序列化过，才会将此对象序列化为字节序列输出。

    3. 如果此对象已经序列化过，则直接输出编号即可。

图示上述序列化过程：

![序列化示意图](vx_images/1280843189277.png)


##### java序列化算法潜在的问题

由于java序利化算法不会重复序列化同一个对象，只会记录已序列化对象的编号。如果序列化一个可变对象（对象内的内容可更改）后，更改了对象内容，再次序列化，并不会再次将此对象转换为字节序列，而只是保存序列化编号。

```java
public class WriteObject {
    public static void main(String[] args) {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
             ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {
            //第一次序列化person
            Person person = new Person("9龙", 23);
            oos.writeObject(person);
            System.out.println(person);

            //修改name
            person.setName("海贼王");
            System.out.println(person);
            //第二次序列化person
            oos.writeObject(person);

            //依次反序列化出p1、p2
            Person p1 = (Person) ios.readObject();
            Person p2 = (Person) ios.readObject();
            System.out.println(p1 == p2);
            System.out.println(p1.getName().equals(p2.getName()));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
//输出结果
//Person{name='9龙', age=23}
//Person{name='海贼王', age=23}
//true
//true
```

##### 可选的自定义序列化

有些时候，我们有这样的需求，某些属性不需要序列化。使用transient关键字选择不需要序列化的字段。

```java
public class Person implements Serializable {
   //不需要序列化名字与年龄
   private transient String name;
   private transient int age;
   private int height;
   private transient boolean singlehood;
   public Person(String name, int age) {
       this.name = name;
       this.age = age;
   }
   //省略get,set方法
}

public class TransientTest {
   public static void main(String[] args) throws Exception {
       try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
            ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {
           Person person = new Person("9龙", 23);
           person.setHeight(185);
           System.out.println(person);
           oos.writeObject(person);
           Person p1 = (Person)ios.readObject();
           System.out.println(p1);
       }
   }
}
//输出结果
//Person{name='9龙', age=23', singlehood=true', height=185cm}
//Person{name='null', age=0', singlehood=false', height=185cm}
```

从输出我们看到，使用transient修饰的属性，java序列化时，会忽略掉此字段，所以反序列化出的对象，被transient修饰的属性是默认值。对于引用类型，值是null；基本类型，值是0；boolean类型，值是false。

使用transient虽然简单，但将此属性完全隔离在了序列化之外。java提供了可选的自定义序列化。可以进行控制序列化的方式，或者对序列化数据进行编码加密等。

```java
private void writeObject(java.io.ObjectOutputStream out) throws IOException；
private void readObject(java.io.ObjectIutputStream in) throws IOException,ClassNotFoundException;
private void readObjectNoData() throws ObjectStreamException;
```

通过重写writeObject与readObject方法，可以自己选择哪些属性需要序列化， 哪些属性不需要。如果writeObject使用某种规则序列化，则相应的readObject需要相反的规则反序列化，以便能正确反序列化出对象。这里展示对名字进行反转加密。

```java
public class Person implements Serializable {
   private String name;
   private int age;
   //省略构造方法，get及set方法

   private void writeObject(ObjectOutputStream out) throws IOException {
       //将名字反转写入二进制流
       out.writeObject(new StringBuffer(this.name).reverse());
       out.writeInt(age);
   }

   private void readObject(ObjectInputStream ins) throws IOException,ClassNotFoundException{
       //将读出的字符串反转恢复回来
       this.name = ((StringBuffer)ins.readObject()).reverse().toString();
       this.age = ins.readInt();
   }
}
```

当序列化流不完整时，readObjectNoData()方法可以用来正确地初始化反序列化的对象。例如，使用不同类接收反序列化对象，或者序列化流被篡改时，系统都会调用readObjectNoData()方法来初始化反序列化的对象。

还有一种更彻底的自定义序列化策略。通过重写如下方法实现：

```java
ANY-ACCESS-MODIFIER Object writeReplace() throws ObjectStreamException;
ANY-ACCESS-MODIFIER Object readResolve() throws ObjectStreamException;
```

writeReplace：在序列化时，会先调用此方法，再调用writeObject方法。此方法可将任意对象代替目标序列化对象。

```java
public class Person implements Serializable {
  private String name;
  private int age;
  //省略构造方法，get及set方法

  private Object writeReplace() throws ObjectStreamException {
      ArrayList<Object> list = new ArrayList<>(2);
      list.add(this.name);
      list.add(this.age);
      return list;
  }

   public static void main(String[] args) throws Exception {
      try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
           ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {
          Person person = new Person("9龙", 23);
          oos.writeObject(person);
          ArrayList list = (ArrayList)ios.readObject();
          System.out.println(list);
      }
  }
}
//输出结果
//[9龙, 23]
```

readResolve：反序列化时替换反序列化出的对象，反序列化出来的对象被立即丢弃。此方法在readeObject后调用。

```java
public class Person implements Serializable {
    private String name;
    private int age;
    //省略构造方法，get及set方法
     private Object readResolve() throws ObjectStreamException{
        return new ("brady", 23);
    }
    public static void main(String[] args) throws Exception {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
             ObjectInputStream ios = new ObjectInputStream(new FileInputStream("person.txt"))) {
            Person person = new Person("9龙", 23);
            oos.writeObject(person);
            HashMap map = (HashMap)ios.readObject();
            System.out.println(map);
        }
    }
}
//输出结果
//{brady=23}
```

readResolve常用来反序列单例类，保证单例类的唯一性。

注意：readResolve与writeReplace的访问修饰符可以是private、protected、public，如果父类重写了这两个方法，子类都需要根据自身需求重写，这显然不是一个好的设计。通常建议对于final修饰的类重写readResolve方法没有问题；否则，重写readResolve使用private修饰。


#### Externalizable

Externalizable 强制自定义序列化。通过实现 Externalizable 接口序列化，必须实现 writeExternal、readExternal 方法。

```java
public interface Externalizable extends java.io.Serializable {
     void writeExternal(ObjectOutput out) throws IOException;
     void readExternal(ObjectInput in) throws IOException, ClassNotFoundException;
}
```

示例：

```java
public class ExPerson implements Externalizable {

    private String name;
    private int age;
    //注意，必须加上pulic 无参构造器
    public ExPerson() {
    }

    public ExPerson(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public void writeExternal(ObjectOutput out) throws IOException {
        //将name反转后写入二进制流
        StringBuffer reverse = new StringBuffer(name).reverse();
        System.out.println(reverse.toString());
        out.writeObject(reverse);
        out.writeInt(age);
    }

    @Override
    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        //将读取的字符串反转后赋值给name实例变量
        this.name = ((StringBuffer) in.readObject()).reverse().toString();
        System.out.println(name);
        this.age = in.readInt();
    }

    public static void main(String[] args) throws IOException, ClassNotFoundException {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("ExPerson.txt"));
             ObjectInputStream ois = new ObjectInputStream(new FileInputStream("ExPerson.txt"))) {
            oos.writeObject(new ExPerson("brady", 23));
            ExPerson ep = (ExPerson) ois.readObject();
            System.out.println(ep);
        }
    }
}
//输出结果
//ydarb
//brady
//ExPerson{name='brady', age=23}
```

注意：Externalizable接口不同于Serializable接口，实现此接口必须实现接口中的两个方法实现自定义序列化，这是强制性的；特别之处是必须提供pulic的无参构造器，因为在反序列化的时候需要反射创建对象。

### 两种序列化对比

|                    实现Serializable接口                    | 实现Externalizable接口  |
| --------------------------------------------------------- | ---------------------- |
| 系统自动存储必要的信息                                      | 系统自动存储必要的信息   |
| Java内建支持，易于实现，只需要实现该接口即可，无需任何代码支持 | 必须实现接口内的两个方法 |
| 性能略差                                                   | 性能略好                |

虽然Externalizable接口带来了一定的性能提升，但变成复杂度也提高了，所以一般通过实现Serializable接口进行序列化。


### 序列化版本号serialVersionUID

我们知道，反序列化必须拥有class文件，但随着项目的升级，class文件也会升级，序列化怎么保证升级前后的兼容性呢？

java序列化提供了一个private static final long serialVersionUID 的序列化版本号，只有版本号相同，即使更改了序列化属性，对象也可以正确被反序列化回来。

```java
public class Person implements Serializable {
    //序列化版本号
    private static final long serialVersionUID = 1111013L;
    private String name;
    private int age;
    //省略构造方法及get,set
}
```

如果反序列化使用的class的版本号与序列化时使用的不一致，反序列化会报InvalidClassException异常。

序列化版本号可自由指定，如果不指定，JVM会根据类信息自己计算一个版本号，这样随着class的升级，就无法正确反序列化；不指定版本号另一个明显隐患是，不利于jvm间的移植，可能class文件没有更改，但不同jvm可能计算的规则不一样，这样也会导致无法反序列化。

什么情况下需要修改serialVersionUID呢？分三种情况。

* 如果只是修改了方法，反序列化不容影响，则无需修改版本号；

* 如果只是修改了静态变量，瞬态变量（transient修饰的变量），反序列化不受影响，无需修改版本号；

* 如果修改了非瞬态变量，则可能导致反序列化失败。如果新类中实例变量的类型与序列化时类的类型不一致，则会反序列化失败，这时候需要更改serialVersionUID。如果只是新增了实例变量，则反序列化回来新增的是默认值；如果减少了实例变量，反序列化时会忽略掉减少的实例变量。


### 总结

1. 所有需要网络传输的对象都需要实现序列化接口，通过建议所有的javaBean都实现Serializable接口。

2. 对象的类名、实例变量（包括基本类型，数组，对其他对象的引用）都会被序列化；方法、类变量、transient实例变量都不会被序列化。

3. 如果想让某个变量不被序列化，使用transient修饰。

4. 序列化对象的引用类型成员变量，也必须是可序列化的，否则，会报错。

5. 反序列化时必须有序列化对象的class文件。

6. 当通过文件、网络来读取序列化后的对象时，必须按照实际写入的顺序读取。

7. 单例类序列化，需要重写readResolve()方法；否则会破坏单例原则。

8. 同一对象序列化多次，只有第一次序列化为二进制流，以后都只是保存序列化编号，不会重复序列化。

9. 建议所有可序列化的类加上serialVersionUID 版本号，方便项目升级。


### transient

## 异常处理

### 异常的分类

![异常的分类](vx_images/4712843189970.png)

Error：是程序中无法处理的错误，表示运行应用程序中出现了严重的错误。此类错误一般表示代码运行时JVM出现问题。通常有Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如说当jvm耗完可用内存时，将出现OutOfMemoryError。此类错误发生时，JVM将终止线程。非代码性错误。因此，当此类错误发生时，应用不应该去处理此类错误。

Exception：：程序本身可以捕获并且可以处理的异常。

运行时异常(不受检异常)：RuntimeException类极其子类表示JVM在运行期间可能出现的错误。编译器不会检查此类异常，并且不要求处理异常，比如用空值对象的引用（NullPointerException）、数组下标越界（ArrayIndexOutBoundException）。此类异常属于不可查异常，一般是由程序逻辑错误引起的，在程序中可以选择捕获处理，也可以不处理。

非运行时异常(受检异常)：Exception中除RuntimeException极其子类之外的异常。编译器会检查此类异常，如果程序中出现此类异常，比如说IOException，必须对该异常进行处理，要么使用try-catch捕获，要么使用throws语句抛出，否则编译不通过。

### 异常处理
抛出异常：throw，throws
捕获异常：try-catch-finally

### throw

throw用在方法内，用来抛出一个异常对象，将这个异常对象传递到调用者处，并结束当前方法的执行。

使用的格式：`throw new 异常类名(参数);`


### throws

运用于方法声明之上，用于表示当前方法不处理异常，而是提醒该方法的调用者来处理异常。

`修饰符 返回值类型 方法名（参数） throws 异常类名1，异常类名2 ... { }`

### try-catch-finally

一个try可以对应多个catch。一旦发生异常，会根据异常执行对应的catch，且不向下传递。

无论是否发生异常，finally都会执行。

```
try{}
catch(Exception e1){}
cathc(Exception e2){}
finally{};
```

### 自定义异常

继承Exception类，实现其方法。

```
public class CustomException extends Exception
{
    // 无参构造方法
    public CustomException()
    {
        super();
    }
    // 有参的构造方法
    public CustomException(String message)
    {
        super(message);
    }
    // 用指定的详细信息和原因构造一个新的异常
    public CustomException(String message, Throwable cause)
    {
        super(message, cause);
    }
    // 用指定原因构造一个新的异常
    public CustomException(Throwable cause)
    {
        super(cause);
    }
}

```


## 注解

[参考](https://www.runoob.com/w3cnote/java-annotation.html)

Java 注解（Annotation）又称 Java 标注，是 JDK5.0 引入的一种注释机制。
Java 语言中的类、方法、变量、参数和包等都可以被标注。和 Javadoc 不同，Java 标注可以通过反射获取标注内容。在编译器生成类文件时，标注可以被嵌入到字节码中。Java 虚拟机可以保留标注内容，在运行时可以获取到标注内容 。 当然它也支持自定义 Java 标注。

### 内置的注解

Java 定义了一套注解，共有 7 个，3 个在 java.lang 中，剩下 4 个在 java.lang.annotation 中。

作用在代码的注解是

* @Override - 检查该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
* @Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
* @SuppressWarnings - 指示编译器去忽略注解中声明的警告。

作用在其他注解的注解(或者说 元注解)是:

* @Retention - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
* @Documented - 标记这些注解是否包含在用户文档中。
* @Target - 标记这个注解应该是哪种 Java 成员。
* @Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)

从 Java 7 开始，额外添加了 3 个注解:

* @SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
* @FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。
* @Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

### Annotation 架构

![Annotation 架构](vx_images/5746658179274.png)
从中，我们可以看出：

1. 1 个 Annotation 和 1 个 RetentionPolicy 关联。
可以理解为：每1个Annotation对象，都会有唯一的RetentionPolicy属性。
2. 1 个 Annotation 和 1~n 个 ElementType 关联。
可以理解为：对于每 1 个 Annotation 对象，可以有若干个 ElementType 属性。
3. Annotation 有许多实现类，包括：Deprecated, Documented, Inherited, Override 等等。

### Annotation 组成部分

java Annotation 的组成中，有 3 个非常重要的主干类。它们分别是：
* Annotation.java

```java
package java.lang.annotation;
public interface Annotation {

    boolean equals(Object obj);

    int hashCode();

    String toString();

    Class<? extends Annotation> annotationType();
}
```

* ElementType.java

```java
package java.lang.annotation;

public enum ElementType {
    TYPE,               /* 类、接口（包括注释类型）或枚举声明  */

    FIELD,              /* 字段声明（包括枚举常量）  */

    METHOD,             /* 方法声明  */

    PARAMETER,          /* 参数声明  */

    CONSTRUCTOR,        /* 构造方法声明  */

    LOCAL_VARIABLE,     /* 局部变量声明  */

    ANNOTATION_TYPE,    /* 注释类型声明  */

    PACKAGE             /* 包声明  */
}
```

* RetentionPolicy.java

```java
package java.lang.annotation;
public enum RetentionPolicy {
    SOURCE,            /* Annotation信息仅存在于编译器处理期间，编译器处理完之后就没有该Annotation信息了  */

    CLASS,             /* 编译器将Annotation存储于类对应的.class文件中。默认行为  */

    RUNTIME            /* 编译器将Annotation存储于class文件中，并且可由JVM读入 */
}
```

说明：

1. Annotation 就是个接口。
"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联，并且与 "1～n 个 ElementType" 关联。可以通俗的理解为：每 1 个 Annotation 对象，都会有唯一的 RetentionPolicy 属性；至于 ElementType 属性，则有 1~n 个。

2. ElementType 是 Enum 枚举类型，它用来指定 Annotation 的类型。
"每 1 个 Annotation" 都与 "1～n 个 ElementType" 关联。当 Annotation 与某个 ElementType 关联时，就意味着：Annotation有了某种用途。例如，若一个 Annotation 对象是 METHOD 类型，则该 Annotation 只能用来修饰方法。

3. RetentionPolicy 是 Enum 枚举类型，它用来指定 Annotation 的策略。通俗点说，就是不同 RetentionPolicy 类型的 Annotation 的作用域不同。

"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联。
* 若 Annotation 的类型为 SOURCE，则意味着：Annotation 仅存在于编译器处理期间，编译器处理完之后，该 Annotation 就没用了。 例如，" @Override" 标志就是一个 Annotation。当它修饰一个方法的时候，就意味着该方法覆盖父类的方法；并且在编译期间会进行语法检查！编译器处理完后，"@Override" 就没有任何作用了。
* 若 Annotation 的类型为 CLASS，则意味着：编译器将 Annotation 存储于类对应的 .class 文件中，它是 Annotation 的默认行为。
* 若 Annotation 的类型为 RUNTIME，则意味着：编译器将 Annotation 存储于 class 文件中，并且可由JVM读入。

这时，只需要记住"每 1 个 Annotation" 都与 "1 个 RetentionPolicy" 关联，并且与 "1～n 个 ElementType" 关联。学完后面的内容之后，再回头看这些内容，会更容易理解。

### java 自带的 Annotation

接下来可以讲解 Annotation 实现类的语法定义了。

#### Annotation 通用定义

```java
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation1 {
}
```

说明：

上面的作用是定义一个 Annotation，它的名字是 MyAnnotation1。定义了 MyAnnotation1 之后，我们可以在代码中通过 "@MyAnnotation1" 来使用它。 其它的，@Documented, @Target, @Retention, @interface 都是来修饰 MyAnnotation1 的。下面分别说说它们的含义：

1. @interface
使用 @interface 定义注解时，意味着它实现了 java.lang.annotation.Annotation 接口，即该注解就是一个Annotation。
定义 Annotation 时，@interface 是必须的。
注意：它和我们通常的 implemented 实现接口的方法不同。Annotation 接口的实现细节都由编译器完成。通过 @interface 定义注解后，该注解不能继承其他的注解或接口。

2. @Documented
类和方法的 Annotation 在缺省情况下是不出现在 javadoc 中的。如果使用 @Documented 修饰该 Annotation，则表示它可以出现在 javadoc 中。
定义 Annotation 时，@Documented 可有可无；若没有定义，则 Annotation 不会出现在 javadoc 中。

3. @Target(ElementType.TYPE)
前面我们说过，ElementType 是 Annotation 的类型属性。而 @Target 的作用，就是来指定 Annotation 的类型属性。
@Target(ElementType.TYPE) 的意思就是指定该 Annotation 的类型是 ElementType.TYPE。这就意味着，MyAnnotation1 是来修饰"类、接口（包括注释类型）或枚举声明"的注解。
定义 Annotation 时，@Target 可有可无。若有 @Target，则该 Annotation 只能用于它所指定的地方；若没有 @Target，则该 Annotation 可以用于任何地方。

4. @Retention(RetentionPolicy.RUNTIME)
前面我们说过，RetentionPolicy 是 Annotation 的策略属性，而 @Retention 的作用，就是指定 Annotation 的策略属性。
@Retention(RetentionPolicy.RUNTIME) 的意思就是指定该 Annotation 的策略是 RetentionPolicy.RUNTIME。这就意味着，编译器会将该 Annotation 信息保留在 .class 文件中，并且能被虚拟机读取。
定义 Annotation 时，@Retention 可有可无。若没有 @Retention，则默认是 RetentionPolicy.CLASS。


#### java自带的Annotation

通过上面的示例，我们能理解：@interface 用来声明 Annotation，@Documented 用来表示该 Annotation 是否会出现在 javadoc 中， @Target 用来指定 Annotation 的类型，@Retention 用来指定 Annotation 的策略。

理解这一点之后，我们就很容易理解 java 中自带的 Annotation 的实现类，即 Annotation 架构图的右半边。

* java 常用的 Annotation：

```java
@Deprecated  -- @Deprecated 所标注内容，不再被建议使用。
@Override    -- @Override 只能标注方法，表示该方法覆盖父类中的方法。
@Documented  -- @Documented 所标注内容，可以出现在javadoc中。
@Inherited   -- @Inherited只能被用来标注“Annotation类型”，它所标注的Annotation具有继承性。
@Retention   -- @Retention只能被用来标注“Annotation类型”，而且它被用来指定Annotation的RetentionPolicy属性。
@Target      -- @Target只能被用来标注“Annotation类型”，而且它被用来指定Annotation的ElementType属性。
@SuppressWarnings -- @SuppressWarnings 所标注内容产生的警告，编译器会对这些警告保持静默。
```

由于 "@Deprecated 和 @Override" 类似，"@Documented, @Inherited, @Retention, @Target" 类似；下面，我们只对 @Deprecated, @Inherited, @SuppressWarnings 这 3 个 Annotation 进行说明。


##### @Deprecated

@Deprecated 的定义如下：

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
public @interface Deprecated {
}
```

说明：

1. @interface -- 它的用来修饰 Deprecated，意味着 Deprecated 实现了 java.lang.annotation.Annotation 接口；即 Deprecated 就是一个注解。 

2. @Documented -- 它的作用是说明该注解能出现在 javadoc 中。

3. @Retention(RetentionPolicy.RUNTIME) -- 它的作用是指定 Deprecated 的策略是 RetentionPolicy.RUNTIME。这就意味着，编译器会将Deprecated 的信息保留在 .class 文件中，并且能被虚拟机读取。

4. @Deprecated 所标注内容，不再被建议使用。


##### @Inherited

@Inherited 的定义如下：

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
}
```

说明：

1. @interface -- 它的用来修饰 Inherited，意味着 Inherited 实现了 java.lang.annotation.Annotation 接口；即 Inherited 就是一个注解。

2. @Documented -- 它的作用是说明该注解能出现在 javadoc 中。

3. @Retention(RetentionPolicy.RUNTIME) -- 它的作用是指定 Inherited 的策略是 RetentionPolicy.RUNTIME。这就意味着，编译器会将 Inherited 的信息保留在 .class 文件中，并且能被虚拟机读取。

4. @Target(ElementType.ANNOTATION_TYPE) -- 它的作用是指定 Inherited 的类型是 ANNOTATION_TYPE。这就意味着，@Inherited 只能被用来标注 "Annotation 类型"。

5. @Inherited 的含义是，它所标注的Annotation将具有继承性。如果一个类的某个注解 A 被标注了 @Inherited，这意味着它的子类也具有 A 的属性。


##### @SuppressWarnings

@SuppressWarnings 的定义如下：

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

说明：

1. @interface -- 它的用来修饰 SuppressWarnings，意味着 SuppressWarnings 实现了 java.lang.annotation.Annotation 接口；即 SuppressWarnings 就是一个注解。

2. @Retention(RetentionPolicy.SOURCE) -- 它的作用是指定 SuppressWarnings 的策略是 RetentionPolicy.SOURCE。这就意味着，SuppressWarnings 信息仅存在于编译器处理期间，编译器处理完之后 SuppressWarnings 就没有作用了。

3. @Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE}) -- 它的作用是指定 SuppressWarnings 的类型同时包括TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE。

    * TYPE 意味着，它能标注"类、接口（包括注释类型）或枚举声明"。

    * FIELD 意味着，它能标注"字段声明"。

    * METHOD 意味着，它能标注"方法"。

    * PARAMETER 意味着，它能标注"参数"。

    * CONSTRUCTOR 意味着，它能标注"构造方法"。

    * LOCAL_VARIABLE 意味着，它能标注"局部变量"。

4. String[] value(); 意味着，SuppressWarnings 能指定参数

5. SuppressWarnings 的作用是，让编译器对"它所标注的内容"的某些警告保持静默。例如，"@SuppressWarnings(value={"deprecation", "unchecked"})" 表示对"它所标注的内容"中的 "SuppressWarnings 不再建议使用警告"和"未检查的转换时的警告"保持沉默。

### Annotation 的作用

Annotation 是一个辅助类，它在 Junit、Struts、Spring 等工具框架中被广泛使用。

我们在编程中经常会使用到的 Annotation 作用有：

* 编译检查

    Annotation 具有"让编译器进行编译检查的作用"。

    例如，@SuppressWarnings, @Deprecated 和 @Override 都具有编译检查作用。

    1. 关于 @SuppressWarnings 和 @Deprecated，已经在"第3部分"中详细介绍过了。这里就不再举例说明了。

    2. 若某个方法被 @Override 的标注，则意味着该方法会覆盖父类中的同名方法。如果有方法被 @Override 标示，但父类中却没有"被 @Override 标注"的同名方法，则编译器会报错。

* 在反射中使用 Annotation

    在反射的 Class，Method，Field 等函数中，有许多于 Annotation 相关的接口。这也意味着，我们可以在反射中解析并使用 Annotation。

* 根据 Annotation 生成帮助文档

    通过给 Annotation 注解加上 @Documented 标签，能使该 Annotation 标签出现在 javadoc 中。

* 能够帮忙查看查看代码

    通过 @Override, @Deprecated 等，我们能很方便的了解程序的大致结构。

另外，我们也可以通过自定义 Annotation 来实现一些功能。


## Javadoc



