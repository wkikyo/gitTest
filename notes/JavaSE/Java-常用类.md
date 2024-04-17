

# 1 Object

类 `java.lang.Object`是类层次结构的根类，即所有其它类的父类。每个类都使用 `Object` 作为超类。

* Object类型的变量与除Object以外的任意引用数据类型的对象都存在多态引用

  ```java
  method(Object obj){…} //可以接收任何类作为其参数
  
  Person o = new Person();  
  method(o);
  
  ```

* 所有对象（包括数组）都实现这个类的方法。

* 如果一个类没有特别指定父类，那么默认则继承自Object类。例如：

  ```java
  public class Person {
  	...
  }
  //等价于：
  public class Person extends Object {
  	...
  }
  ```

Object类中声明的结构（属性、方法）具有通用性

* Object中没有声明属性
* Object提供一个空参的构造器
* 重点需要关注一下Object的方法

## 1.1 Object中的方法

根据JDK源代码及Object类的API文档，Object类当中包含的方法有11个。这里我们主要关注其中的6个：

### 1 clone() 	了解

提供person.clone()，提供一个person的copy副本，**一个全新的对象**，与原对象地址不同。因此 **创建对象的方式有两种**了，一种是 **new** 创建对象，还有一种是 通过**clone()**函数copy一个

### 2 finalize()  	了解

- 当对象被回收时，系统自动调用该对象的 finalize() 方法。（不是垃圾回收器调用的，是本类对象调用的）
  - 永远不要主动调用某个对象的finalize方法，应该交给垃圾回收机制调用。
- 什么时候被回收：当某个对象没有任何引用时，JVM就认为这个对象是垃圾对象，就会在之后不确定的时间使用垃圾回收机制来销毁该对象，在销毁该对象前，会先调用 finalize()方法。 
- 子类可以重写该方法，目的是在对象被清理之前执行必要的清理操作。比如，在方法内断开相关连接资源。
  - 如果重写该方法，让一个新的引用变量重新引用该对象，则会重新激活对象。
- 在JDK 9中此方法已经被`标记为过时`的

### 3 equals()	 重点

**= =：** 

- 基本类型比较值:只要两个变量的值相等，即为true。

  ```java
  int a=5; 
  if(a==6){…}
  ```

- 引用类型比较引用(是否指向同一个对象)：只有指向同一个对象时，==才返回true。

  ```java
  Person p1=new Person();  	    
  Person p2=new Person();
  if (p1==p2){…}
  ```

  - 用“==”进行比较时，符号两边的`数据类型必须兼容`(可自动转换的基本数据类型除外)，否则编译出错

**equals()：**所有类都继承了Object，也就获得了equals()方法。还可以重写。

- 只能比较引用类型，Object类源码中equals()的作用与“==”相同：比较是否指向同一个对象。	 

  <img src="E:\Notes\JavaSE\assets\image-20220226101655293.png" alt="image-20220503104750655" style="zoom:67%;" />

- 格式:obj1.equals(obj2)

- 特例：当用equals()方法进行比较时，对类File、String、Date及包装类（Wrapper Class）来说，是比较类型及内容而不考虑引用的是否是同一个对象；

  - 原因：在这些类中重写了Object类的equals()方法。

- 当自定义使用equals()时，可以重写。用于比较两个对象的“内容”是否都相等   IDEA可以自动生成

### 4 toString()	重点

方法签名：public String toString()

① 默认情况下，toString()返回的是“对象的运行时类型 @ 对象的hashCode值的十六进制形式"

② 在进行String与其它类型数据的连接操作时，自动调用toString()方法

```java
Date now=new Date();
System.out.println(“now=”+now);  //相当于
System.out.println(“now=”+now.toString()); 
```

③ 如果我们直接System.out.println(对象)，默认会自动调用这个对象的toString()

> 因为Java的引用数据类型的变量中存储的实际上时对象的内存地址，但是Java对程序员隐藏内存地址信息，所以不能直接将内存地址显示出来，所以当你打印对象时，JVM帮你调用了对象的toString()。

④ 可以根据需要在用户自定义类型中重写toString()方法
	如String 类重写了toString()方法，返回字符串的值。

```java
s1="hello";
System.out.println(s1);//相当于System.out.println(s1.toString());
```



# 2 包装类

Java提供了两个类型系统，`基本数据类型`与`引用数据类型`。使用基本数据类型在于效率，然而针对面向对象设计的API或新特性（o op），基本数据类型无法使用，因此将基本数据类型 写了对应的包装类 可使用新特性

```java
Java针对八种基本数据类型定义了相应的引用类型：包装类（封装类）。有了类的特点，就可以调用类中的方法，Java才是真正的面向对象。
```

<img src="E:\Notes\JavaSE\assets\image-20240318220130231.png" alt="image-20240318220130231" style="zoom:80%;" />

## 2.1 基本数据类型和包装类 转换

 **装箱：把基本数据类型转为包装类对象**

基本数值---->包装对象

```Java
Integer obj1 = new Integer(4);//使用构造函数函数
Float f = new Float(“4.56”);
Long l = new Long(“asdf”);  //NumberFormatException

Integer obj2 = Integer.valueOf(4);// 推荐 ：使用包装类中的valueOf方法
```

**拆箱：把包装类对象拆为基本数据类型**

转为基本数据类型，一般是因为需要运算，Java中的大多数运算符是为基本数据类型设计的。比较、算术等

```Java
Integer obj = new Integer(4);
int num1 = obj.intValue();
```

| 包装类的默认值是null，这一点需要注意 |
| ------------------------------------ |

**自动装箱与拆箱：**

从`JDK5.0 `开始，基本类型与包装类的装箱、拆箱动作可以自动完成。例如：

```Java
Integer i = 4;//自动装箱。相当于Integer i = Integer.valueOf(4);
i = i + 5;//等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
//加法运算完成后，再次装箱，把基本数值转成对象。
int i1 = 10；
Integer ii1 = i1； //自动装箱，将int类型赋值给Integer类
    
int i2 = ii1	//自动拆箱 ，不用Interger.intValue()方法
```

##  2.2 String类、包装类、基本数据 转换

![image-20240318221521670](E:\Notes\JavaSE\assets\image-20240318221521670.png)

**（1）基本数据类型转为字符串**

**方式1：**调用字符串重载的valueOf()方法

```java
int a = 10;
//String str = a;//错误的

String str = String.valueOf(a);
```

**方式2：**更直接的方式

```Java
int a = 10;
String str = a + "";
```

**（2）字符串转为基本数据类型**

除了**Character**类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型，例如：

* `public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。
* `public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。
* `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。

也可以**字符串转为包装类**，然后可以自动拆箱为基本数据类型

* ```public static Integer valueOf(String s)```：将字符串参数转换为对应的Integer包装类，然后可以自动拆箱为int基本类型
* ```public static Long valueOf(String s)```：将字符串参数转换为对应的Long包装类，然后可以自动拆箱为long基本类型
* ```public static Double valueOf(String s)```：将字符串参数转换为对应的Double包装类，然后可以自动拆箱为double基本类型

注意:如果字符串参数的内容无法正确转换为对应的基本类型，则会抛出`java.lang.NumberFormatException`异常。

# 3 字符串

## 3.1 不可变字符序列 String

### 3.1.1 String特性

- `java.lang.String` 类代表字符串。Java程序中所有的字符串文字（例如`"hello"` ）都可以看作是实现此类的实例。

- 字符串是常量，用双引号引起来表示。**它们的值**在创建之后不能更改

- 字符串String类型本身是**final**声明的，意味着我们**不能继承String**

- String对象的字符内容是存储在一个字符数组value[]中的。`"abc"` 等效于 `char[] data={'h','e','l','l','o'}`

![image-20240404202705195](E:\Notes\JavaSE\assets\image-20240404202705195.png)

```java
//jdk8中的String源码：
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[]; //String对象的字符内容是存储在此数组中
 
    /** Cache the hash code for the string */
    private int hash; // Default to 0
```

- **private**意味着外面无法直接获取字符数组，而且String**没有提供value的get和set方法**。

- **final**意味着字符数组的引用不可改变，而且String也没有提供方法来修改value数组某个元素值

- 因此字符串的**字符数组内容也不可变**的，即String代表着不可变的字符序列。即，**一旦对字符串进行修改，就会产生新对象**。

- JDK9只有，底层使用byte[]数组。

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence { 
    @Stable
    private final byte[] value;
}

//官方说明：... that most String objects contain only Latin-1 characters. Such characters require only one byte of storage, hence half of the space in the internal char arrays of such String objects is going unused.

//细节：... The new String class will store characters encoded either as ISO-8859-1/Latin-1 (one byte per character), or as UTF-16 (two bytes per character), based upon the contents of the string. The encoding flag will indicate which encoding is used.
```

* Java 语言提供对字符串串联符号（"+"）以及将其他对象转换为字符串的特殊支持（toString()方法）

### 3.1.2 String的内存结构

#### **概述**

因为字符串对象设计为不可变，那么所以字符串有**常量池**来**保存很多常量对象**。

JDK6中，字符串常量池在方法区。JDK7开始，就移到**堆空间**，直到目前JDK17版本。

举例内存结构分配：

<img src="E:\Notes\JavaSE\assets\image-20240404203055293.png" alt="image-20240404203055293" style="zoom:80%;" />

#### **练习类型1 ：拼接**

```java
String s1 = "hello";
String s2 = "hello";
// 内存中只有一个"hello"对象被创建，同时被s1和s2共享。
System.out.println(s1 == s2);
```

<img src="E:\Notes\JavaSE\assets\image-20240404203549710.png" alt="image-20240404203549710" style="zoom: 80%;" />

对应内存结构为：（以下内存结构以`JDK6为例`绘制）：

<img src="E:\Notes\JavaSE\assets\image-20240404203622760.png" alt="image-20240404203622760" style="zoom:67%;" />

进一步：

<img src="E:\Notes\JavaSE\assets\image-20240404203644241.png" alt="image-20240404203644241" style="zoom:67%;" />

```java
Person p1 = new Person();
p1.name = “Tom";

Person p2 = new Person();
p2.name = “Tom";

System.out.println(p1.name.equals( p2.name)); // string 的equals 被重写过 比较的是内容 true
System.out.println(p1.name == p2.name); //字符串常量只有一个  true
System.out.println(p1.name == "Tom"); // 比较地址 true
```

<img src="E:\Notes\JavaSE\assets\image-20240404203946218.png" alt="image-20240404203946218" style="zoom:67%;" />

<img src="E:\Notes\JavaSE\assets\image-20240404204020824.png" alt="image-20240404204020824" style="zoom:67%;" />

#### **练习类型2：new**

String str1 = “abc”; 与 String str2 = new String(“abc”);的区别？

<img src="E:\Notes\JavaSE\assets\image-20240404204117190.png" alt="image-20240404204117190" style="zoom:50%;" />

**str2** 首先指向堆中的一个**字符串对象**，然后堆中**字符串的value数组**指向常量池中**常量对象的value数组**。

> - 字符串常量存储在字符串常量池，目的是共享
>
> - 字符串对象存储在堆中

练习：

```java
String s1 = "javaEE";
String s2 = "javaEE";
String s3 = new String("javaEE");
String s4 = new String("javaEE");

System.out.println(s1 == s2);//true
System.out.println(s1 == s3);//false
System.out.println(s1 == s4);//false
System.out.println(s3 == s4);//false
```

<img src="E:\Notes\JavaSE\assets\image-20240404204356375.png" alt="image-20240404204356375" style="zoom:50%;" />

练习：String str2 = new String("hello"); 在内存中创建了几个对象？

```
两个，一个字符串对象在堆中，还有一个字符串常量 在常量池中
```

#### 练习类型3：intern()

- **String s1 = "a";** 

说明：在字符串常量池中创建了一个字面量为"a"的字符串。

- **s1 = s1 + "b";** 

说明：实际上原来的“a”字符串对象已经丢弃了，现在在堆空间中产生了一个字符串s1+"b"（也就是"ab")。如果多次执行这些改变串内容的操作，会导致大量副本字符串对象存留在内存中，降低效率。如果这样的操作放到循环中，会极大影响程序的性能。

- **String s2 = "ab";**

说明：直接在字符串常量池中创建一个字面量为"ab"的字符串。

- **String s3 = "a" + "b";**

说明：s3指向字符串常量池中已经创建的"ab"的字符串。

- **String s4 = s1.intern();**

说明：堆空间的s1对象在调用intern()之后，会将常量池中已经存在的"ab"字符串赋值给s4

```java
String s1 = "hello";
String s2 = "world";
String s3 = "hello" + "world";
String s4 = s1 + "world";
String s5 = s1 + s2;
String s6 = (s1 + s2).intern();

System.out.println(s3 == s4); // false
System.out.println(s3 == s5); // false
System.out.println(s4 == s5); // false
System.out.println(s3 == s6); // ture
```

> **结论：**
>
> （1）常量+常量：结果是常量池。且常量池中不会存在相同内容的常量。
>
> （2）常量与变量 或 变量与变量：结果在堆中
>
> （3）拼接后调用intern方法：返回值放到常量池中

**注意 ： String 类型的变量 用final修饰 按照常量处理**

```java
@Test
    public void test5(){ 
        final String s1 = "hello";  // final修饰的String 变为常量
        final String s2 = "world";
        String s3 = "helloworld";
        String s4 = s1 + s2; // 常量
        String s5 = s1 + "world";  // 常量
        String s6 = "hello" + "world"; //常量
        //结果都是true
        System.out.println(s3 == s4);
        System.out.println(s3 == s5);
        System.out.println(s3 == s6);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240404205929013.png" alt="image-20240404205929013" style="zoom: 80%;" />

**注意：concat方法拼接，哪怕是两个常量对象拼接，结果也是在堆**

```java
@Test
    public void test6(){
        String s1 = "hello";
        String s2 = "world";
        String s3 = "helloworld";
        String s4 = s1.concat("world");
        String s5 = "hello" + "world";
        System.out.println(s3 == s5);//true
        System.out.println(s3 == s4); // false
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240404210436570.png" alt="image-20240404210436570" style="zoom: 80%;" />

## 3.2 String常用API-1

### 3.2.1 构造器

* `public String() ` ：初始化新创建的 String对象，以使其表示空字符序列。
* ` String(String original)`： 初始化一个新创建的 `String` 对象，使其表示一个与参数相同的字符序列；换句话说，新创建的字符串是该参数字符串的副本。
* `public String(char[] value) ` ：通过当前参数中的字符数组来构造新的String。
* `public String(char[] value,int offset, int count) ` ：通过字符数组的一部分来构造新的String。
* `public String(byte[] bytes) ` ：通过使用平台的**默认字符集**解码当前参数中的字节数组来构造新的String。
* `public String(byte[] bytes,String charsetName) ` ：通过使用指定的字符集解码当前参数中的字节数组来构造新的String。

```java
public class StringMethodTest {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String s1 = new String();
        String s2 = new String("hello,world");
        String s3 = new String(new char[]{'a', 'b', 'c'});
        String s4 = new String(new byte[]{97, 98, 99},"utf-8");
        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
        System.out.println(s4);
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240405004457324.png" alt="image-20240405004457324" style="zoom:67%;" />

### 3.2.2 String与其他结构的转换

**字符串 --> 基本数据类型、包装类：**

- Integer包装类的public static int parseInt(String s)：可以将由“数字”字符组成的字符串转换为整型。
- 类似地，使用java.lang包中的Byte、Short、Long、Float、Double类调相应的类方法可以将由“数字”字符组成的字符串，转化为相应的基本数据类型。

**基本数据类型、包装类 --> 字符串：**

- 调用String类的public String valueOf(int n)可将int型转换为字符串
- 相应的valueOf(byte b)、valueOf(long l)、valueOf(float f)、valueOf(double d)、valueOf(boolean b)可由参数的相应类型到字符串的转换。

> String类的 valueOf方法和  包装类的parseXXX方法 可以实现两者互相转化

 **字符数组 -->  字符串：**

- String 类的构造器：String(char[]) 和 String(char[]，int offset，int length) 分别用字符数组中的全部字符和部分字符创建字符串对象。

> 使用字符串的构造器

 **字符串 -->  字符数组：**

- public char[] toCharArray()：将字符串中的全部字符存放在一个字符数组中的方法。

- public void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)：提供了将指定索引范围内的字符串存放到数组中的方法。

> 使用字符串的toCharArray方法

**字符串 --> 字节数组：（编码）**

- public byte[] getBytes() ：使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
- public byte[] getBytes(String charsetName) ：使用指定的字符集将此 String 编码到 byte 序列，并将结果存储到新的 byte 数组。

> 使用字符串的 getBytes方法

 **字节数组 --> 字符串：（解码）**

- String(byte[])：通过使用平台的默认字符集解码指定的 byte 数组，构造一个新的 String。
- String(byte[]，int offset，int length) ：用指定的字节数组的一部分，即从数组起始位置offset开始取length个字节构造一个字符串对象。
- String(byte[], String charsetName ) 或 new String(byte[], int, int,String charsetName )：解码，按照指定的编码方式进行解码。

> 使用字符串的构造器  其中 第一个和第三个比较常见

```java
public class StringMethodTest {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String s = "中国";
        char[] charArray = s.toCharArray();
        for (int i = 0; i < charArray.length; i++) {
            System.out.print(charArray[i]+" ");
        }
        System.out.println();
        System.out.println("charArray length:"+charArray.length);
        // utf-8--->1个中文3个字节 , gbk--->1个中文2个字节
        byte[] bytes = s.getBytes("utf-8");
        for (int i = 0; i < bytes.length; i++) {
            System.out.print(bytes[i]+" ");
        }
        System.out.println();
        System.out.println("byte length:"+bytes.length);
        System.out.println("-------------------------------");
        String s1 = new String(charArray);
        System.out.println("charArray---->String:"+s1);
        String s2 = new String(bytes, "utf-8");
        System.out.println("bytesArray--->String"+s2);

    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240405010043226.png" alt="image-20240405010043226" style="zoom: 80%;" />

## 3.3 String的常用API-2

### 3.3.1 系列1：常用方法

（1）boolean isEmpty()：字符串是否为空
（2）int length()：返回字符串的长度
（3）String concat(xx)：拼接
（4）boolean equals(Object obj)：比较字符串是否相等，区分大小写
（5）boolean equalsIgnoreCase(Object obj)：比较字符串是否相等，不区分大小写
（6）int compareTo(String other)：比较字符串大小，区分大小写，按照Unicode编码值比较大小
（7）int compareToIgnoreCase(String other)：比较字符串大小，不区分大小写
（8）String toLowerCase()：将字符串中大写字母转为小写
（9）String toUpperCase()：将字符串中小写字母转为大写
（10）String trim()：去掉字符串前后空白符
（11）public String intern()：结果在常量池中共享

```java
@Test
    public void test7(){
        String s1 = "hello";
        String s2 = "HellO";
        System.out.println(s1.equals(s2));
        System.out.println(s1.equalsIgnoreCase(s2));

        String s3 = "abcd";
        String s4 = "aBcd";
        System.out.println(s3.compareTo(s4));
        System.out.println(s3.compareToIgnoreCase(s4));
        String s5 = "    hel  lo     ";
        System.out.println("**********"+s5.trim()+"************");
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405011202628.png" alt="image-20240405011202628" style="zoom:80%;" />

### 3.3.2 系列2：查找

（11）boolean contains(xx)：是否包含xx
（12）int indexOf(xx)：从前往后找当前字符串中xx，即如果有返回**第一次**出现的下标，要是没有返回-1
（13）int indexOf(String str, int fromIndex)：返回指定子字符串在此字符串中**第一次**出现处的索引，从指定的索引开始
（14）int lastIndexOf(xx)：从**后往前找**当前字符串中xx，即如果有返回**最后一次**出现的下标，要是没有返回-1
（15）int lastIndexOf(String str, int fromIndex)：返回指定子字符串在此字符串中最后一次出现处的索引，从**指定的索引开始反向搜索**。

> 14 和 15 的最后一次 指的是正序的最后一次， 按照逆序来看的话  是 倒着找的第一次出现的索引

```java
@Test
    public void test8(){
        String s1 = "w-kikyo-w-kikyo";
        System.out.println(s1.contains("kik"));
        System.out.println(s1.indexOf("kik"));
        System.out.println(s1.indexOf("kik", 3));
        System.out.println(s1.indexOf("wkik"));
        System.out.println(s1.lastIndexOf("kik"));
        System.out.println(s1.lastIndexOf("kik", 8));
        System.out.println(s1.lastIndexOf("wkik"));
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405012434918.png" alt="image-20240405012434918" style="zoom:80%;" />

### 3.3.3 系列3：字符串截取

（16）String substring(int beginIndex) ：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。 
（17）String substring(int beginIndex, int endIndex) ：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。 

> 左闭右开 [ begin，end）

### 3.3.4 系列4：和字符/字符数组相关

（18）char charAt(index)：返回[index]位置的字符
（19）char[] toCharArray()： 将此字符串转换为一个新的字符数组返回
（20）static String valueOf(char[] data)  ：返回指定数组中表示该字符序列的 String
（21）static String valueOf(char[] data, int offset, int count) ： 返回指定数组中表示该字符序列的 String
（22）static String copyValueOf(char[] data)： 返回指定数组中表示该字符序列的 String
（23）static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String

### 3.3.5 系列5：开头与结尾

（24）boolean startsWith(xx)：测试此字符串是否以指定的前缀开始 
（25）boolean startsWith(String prefix, int toffset)：测试此字符串从指定索引开始的子字符串是否以指定前缀开始
（26）boolean endsWith(xx)：测试此字符串是否以指定的后缀结束 

### 3.3.6 系列6：替换

（27）String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 不支持正则。
（28）String replace(CharSequence target, CharSequence replacement)：使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串。 
（29）String replaceAll(String regex, String replacement)：使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 
（30）String replaceFirst(String regex, String replacement)：使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 

## 3.4 **StringBuffer、StringBuilder**

因为String对象是不可变对象，虽然可以共享常量对象，但是对于频繁字符串的修改和拼接操作，效率极低，空间消耗也比较高。因此，JDK又在java.lang包提供了可变字符序列StringBuffer和StringBuilder类型

### 3.4.1 buffer和builder的理解

-  java.lang.StringBuffer代表`可变的字符序列`，JDK1.0中声明，可以对字符串内容进行增删，此时不会产生新的对象。比如：

```java
//情况1:
String s = new String("我喜欢学习"); 
//情况2：
StringBuffer buffer = new StringBuffer("我喜欢学习"); 
buffer.append("数学"); 
```

![image-20240405025001274](E:\Notes\JavaSE\assets\image-20240405025001274.png)

<img src="E:\Notes\JavaSE\assets\image-20240405025013591.png" alt="image-20240405025013591" style="zoom:80%;" />

- 继承结构：
  ![image-20240405025046501](E:\Notes\JavaSE\assets\image-20240405025046501.png)  buffer 和builder 有公共的父类 abstractStringBuilder
- ![image-20240405025137702](E:\Notes\JavaSE\assets\image-20240405025137702.png)

- StringBuilder 和 StringBuffer 非常类似，均代表可变的字符序列，而且提供**相关功能的方法也一样**。
- 区分String、StringBuffer、StringBuilder
  - String:不可变的字符序列； 底层使用char[]数组存储(JDK8.0中)
  - StringBuffer:可变的字符序列；**线程安全**（方法有synchronized修饰），**效率低**；底层使用char[]数组存储 (JDK8.0中)
  - StringBuilder:可变的字符序列； jdk1.5引入，**线程不安全的，效率高**；底层使用char[]数组存储(JDK8.0中)

### 3.4.2  buffer和builder的API

StringBuilder、StringBuffer的API是完全一致的，并且很多方法与String相同

**1、常用API**

（1）StringBuffer append(xx)：提供了很多的append()方法，用于进行字符串追加的方式拼接
（2）StringBuffer delete(int start, int end)：删除[start,end)之间字符
（3）StringBuffer deleteCharAt(int index)：删除[index]位置字符
（4）StringBuffer replace(int start, int end, String str)：替换[start,end)范围的字符序列为str
（5）void setCharAt(int index, char c)：替换[index]位置字符
（6）char charAt(int index)：查找指定index位置上的字符
（7）StringBuffer insert(int index, xx)：在[index]位置插入xx
（8）int length()：返回存储的字符数据的长度
（9）StringBuffer reverse()：反转

> - 当append和insert时，如果原来value数组长度不够，可扩容。每次扩容 ： 扩大2倍 +2 
>
> - 如上(1)(2)(3)(4)(9)这些方法支持`方法链操作`。原理：
>
>   ![image-20240405025439601](E:\Notes\JavaSE\assets\image-20240405025439601.png)

**2、其它API**

（1）int indexOf(String str)：在当前字符序列中查询str的第一次出现下标
（2）int indexOf(String str, int fromIndex)：在当前字符序列[fromIndex,最后]中查询str的第一次出现下标
（3）int lastIndexOf(String str)：在当前字符序列中查询str的最后一次出现下标
（4）int lastIndexOf(String str, int fromIndex)：在当前字符序列[fromIndex,最后]中查询str的最后一次出现下标
（5）String substring(int start)：截取当前字符序列[start,最后]
（6）String substring(int start, int end)：截取当前字符序列[start,end)
（7）String toString()：返回此序列中数据的字符串表示形式
（8）void setLength(int newLength) ：设置当前字符序列长度为newLength

**用到了再看就行**

# 4 日期时间

## 4.1 JDK8之前的日期时间API

JDK 8 之前 主要了解 两个Date 类， 一个格式化类SimpleDateFormat  和 Calender日历类 

每个类了解两个方法和构造器 ，内容不多  其余API 用的时候再去查看

### 4.1.1 java.lang.System类的方法

- System类提供的public static long currentTimeMillis()：用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。

  - 此方法适于计算时间差。

- 计算世界时间的主要标准有：

  - UTC(Coordinated Universal Time)
  - GMT(Greenwich Mean Time)
  - CST(Central Standard Time)

  > 在国际无线电通信场合，为了统一起见，使用一个统一的时间，称为通用协调时(UTC, Universal Time Coordinated)。UTC与格林尼治平均时(GMT, Greenwich Mean Time)一样，都与英国伦敦的本地时相同。这里，UTC与GMT含义完全相同

### 4.1.2 两个Date类

 **java.util.Date** ：

表示特定的瞬间，精确到毫秒。

- 构造器：
  - Date()：使用无参构造器创建的对象可以**获取本地当前时间**
  - Date(long 毫秒数)：把该毫秒值换算成日期时间对象，从1970-1-1 00:00:00 开始计算
- 常用方法
  - **getTime()**: 返回自 1970 年 1 月 1 日 00:00:00 GMT  Date 对象表示的**毫秒数**   从开始到创建对象的时间戳
  - **toString()**: 把此 Date 对象转换为以下形式的 String： **dow mon dd hh:mm:ss zzz yyyy** 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)，zzz是时间标准。
  - 其它很多方法都过时了。

```java
@Test
    public void test1(){
        Date date = new Date();
        System.out.println(date.toString());
        //从开始到创建对象的时间戳 ，不会改变
        System.out.println(date.getTime());
        //系统当前时间戳 会改变的
        System.out.println(System.currentTimeMillis());

        long time = date.getTime();
        Date date1 = new Date(time);
        System.out.println(date1);
        System.out.println(new Date(System.currentTimeMillis()));
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405203546872.png" alt="image-20240405203546872" style="zoom:67%;" />

**java.sql.Date**：

构造器和方法同 util包下Date一样， 因为 **sql.Date 是 util.Date的子类** 

不同的是（目前仅需要关心的地方） ，**显示格式不同**  sql.Date只有 yyyy-MM-dd 格式

<img src="E:\Notes\JavaSE\assets\image-20240405204558850.png" alt="image-20240405204558850" style="zoom: 80%;" />

```java
//java.sql.Date
    @Test
    public void test2(){
        long l = System.currentTimeMillis();    //1712320893822L
        System.out.println(l);
        java.sql.Date date = new java.sql.Date(l);
        System.out.println(date.toString());
        System.out.println(date.getTime());
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405204501527.png" alt="image-20240405204501527" style="zoom:80%;" />

### 4.1.3 SimpleDateFormat类

- java.text.SimpleDateFormat类是一个不与语言环境有关的方式来格式化和解析日期的具体类。
- 可以进行格式化：日期 --> String
- 可以进行解析：String --> 日期
- **构造器**：
  - SimpleDateFormat() ：默认的模式和语言环境创建对象
  - public SimpleDateFormat(String pattern)：该构造方法可以用参数pattern**指定的格式**创建一个对象
- **格式化：**
  - public String format(Date date)：方法格式化时间对象date（按照指定格式 pattern）
- **解析：**
  - public Date parse(String source)：从给定字符串的开始解析文本，以生成一个日期  （给定字符串要符合指定格式，否则异常）

具体的格式pattern可以查看相关API 

<img src="E:\Notes\JavaSE\assets\image-20240405205447799.png" alt="image-20240405205447799" style="zoom:67%;" />

还有现成的例子：

<img src="E:\Notes\JavaSE\assets\image-20240405205242163.png" alt="image-20240405205242163" style="zoom:67%;" />

习惯用的pattern ： 	"yyyy-MM-dd HH:mm:ss" 	年-月-日 时:分:秒   大妹妹(MM)-小弟弟(dd) 记忆一下

```java
	@Test
    public void test3() throws ParseException {
        //格式化日期 为字符串
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date = new Date();
        System.out.println("格式化前:"+date);
        String format = sdf.format(date);
        System.out.println("格式化后:"+format);
        //解析字符串为日期对象
        String tmp = "2099-11-12 12:34:45";
        System.out.println("解析前:"+tmp);
        Date date1 = sdf.parse(tmp);
        System.out.println("解析后:"+date1);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405210121300.png" alt="image-20240405210121300" style="zoom:80%;" />

### 4.1.4  java.util.Calendar 

Calendar:日历类 

① 实例化 由于Calendar是一个**抽象类**，所以我们需要创建其子类的实例。这里我们通过Calendar的**静态方法getInstance()**即可获取

②常用方法：get(int field) / set(int field,xx) / add(int field,xx) / getTime() / setTime()

- 一个Calendar的实例是系统时间的抽象表示，可以修改或获取 YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY 、MINUTE、SECOND等 `日历字段`对应的时间值。

  - public int get(int field)：返回给定日历字段的值

  - public void set(int field,int value) ：将给定的日历字段设置为指定的值

  - public void add(int field,int amount)：根据日历的规则，为给定的日历字段添加或者减去指定的时间量

  - public final Date getTime()：**将Calendar转成Date对象**

  - public final void setTime(Date date)：使用指定的Date对象重置Calendar的时间

- 常用字段
  <img src="E:\Notes\JavaSE\assets\image-20240405210744633.png" alt="image-20240405210744633" style="zoom:80%;" />

- 注意：

  - 获取月份时：一月是0，二月是1，以此类推，12月是11
  - 获取星期时：周日是1，周二是2 ， 。。。。周六是7

```java
public void test4(){
        Calendar calendar = Calendar.getInstance();
        System.out.println(calendar.toString());
}
```

![image-20240405211234298](E:\Notes\JavaSE\assets\image-20240405211234298.png)

calender对象中包含许多内容 如上图所示

```java
public void test4(){
        Calendar calendar = Calendar.getInstance();
        //System.out.println(calendar.toString());
        int year = calendar.get(Calendar.YEAR);
        int month = calendar.get(Calendar.MONTH)+1;
        int day = calendar.get(Calendar.DAY_OF_MONTH);
        System.out.println(year+"-"+month+"-"+day);
        System.out.println("---------------------------------");
        //将日历类转化为日期类 ，格式化后输出结果
        Date date = calendar.getTime();
        System.out.println(new SimpleDateFormat("yyyy-MM-dd").format(date));
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405211809113.png" alt="image-20240405211809113" style="zoom: 67%;" />



## 4.2 JDK8 新的日期时间API

JDK 1.0中包含了一个java.util.Date类，但是它的大多数方法已经在JDK 1.1引入Calendar类之后被弃用了。而Calendar并不比Date好多少。它们面临的问题是：

- 可变性：像日期和时间这样的类应该是不可变的。

- 偏移性：Date中的年份是从1900开始的，而月份都从0开始。

- 格式化：格式化只对Date有用，Calendar则不行。

- 此外，它们也不是线程安全的；

第三次引入的API是成功的，并且Java 8中引入的java.time API 已经纠正了过去的缺陷，将来很长一段时间内它都会为我们服务。

Java 8 以一个新的开始为 Java 创建优秀的 API。新的日期时间API包含：

* `java.time` – 包含值对象的基础包
* `java.time.chrono` – 提供对不同的日历系统的访问。
* `java.time.format` – 格式化和解析时间和日期
* `java.time.temporal` – 包括底层框架和扩展特性
* `java.time.zone` – 包含时区支持的类

说明：新的 java.time 中包含了所有关于时钟（Clock），本地日期（LocalDate）、本地时间（LocalTime）、本地日期时间（LocalDateTime）、时区（ZonedDateTime）和持续时间（Duration）的类。

尽管有68个新的公开类型，但是大多数开发者只会用到基础包和format包，大概占总数的三分之一

### 4.2.1 本地日期时间

LocalDate、LocalTime、LocalDateTime 	--->	类似于calender

localdate 本地日期-年月日、localtime-时分秒、localdatetime-年月日时分秒

> 实例化：now() / of(xxx,xx,xx)
> 方法：get() / withXxx() / plusXxx() / minusXxx() ...

| 方法                                                         | **描述**                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `now() `/ now(ZoneId zone)                                   | 静态方法，根据当前时间创建对象/指定时区的对象                |
| `of(xx,xx,xx,xx,xx,xxx)`                                     | 静态方法，根据指定日期/时间创建对象                          |
| getDayOfMonth()/getDayOfYear()                               | 获得月份天数(1-31) /获得年份天数(1-366)                      |
| getDayOfWeek()                                               | 获得星期几(返回一个 DayOfWeek 枚举值)                        |
| getMonth()                                                   | 获得月份, 返回一个 Month 枚举值                              |
| getMonthValue() / getYear()                                  | 获得月份(1-12) /获得年份                                     |
| getHours()/getMinute()/getSecond()                           | 获得当前对象对应的小时、分钟、秒                             |
| withDayOfMonth()/withDayOfYear()/withMonth()/withYear()      | 将月份天数、年份天数、月份、年份修改为指定的值并返回新的对象 |
| with(TemporalAdjuster  t)                                    | 将当前日期时间设置为校对器指定的日期时间                     |
| plusDays(), plusWeeks(), plusMonths(), plusYears(),plusHours() | 向当前对象添加几天、几周、几个月、几年、几小时               |
| minusMonths() / minusWeeks()/minusDays()/minusYears()/minusHours() | 从当前对象减去几月、几周、几天、几年、几小时                 |
| plus(TemporalAmount t)/minus(TemporalAmount t)               | 添加或减少一个 Duration 或 Period                            |
| isBefore()/isAfter()                                         | 比较两个 LocalDate                                           |
| isLeapYear()                                                 | 判断是否是闰年（在LocalDate类中声明）                        |
| format(DateTimeFormatter  t)                                 | 格式化本地日期、时间，返回一个字符串                         |
| parse(Charsequence text)                                     | 将指定格式的字符串解析为日期、时间                           |

```Java
public void test(){
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println(localDate);//2024-04-05
        System.out.println(localTime);//22:09:59.678218
        System.out.println(localDateTime);//2024-04-05T22:09:59.678218
    }
```

```java
@Test
    public void test1(){
        //根据指定时间创建本地日期对象
        LocalDate localDate = LocalDate.of(2023, 5, 11);
        System.out.println(localDate);
        //下面几行都是get方法，得到不同的属性值
        int dayOfYear = localDate.getDayOfYear();
        DayOfWeek dayOfWeek = localDate.getDayOfWeek();
        System.out.println(dayOfYear);
        System.out.println(dayOfWeek);
        System.out.println(localDate.getMonth());
        System.out.println(localDate.getYear());
        System.out.println("-----------------");
        //local系列 为不可变对象，当日期改变时 返回新的对象
        LocalDate localDate1 = localDate.plusDays(100);
        System.out.println("plusDays-100:"+localDate1);
        LocalDate minus = localDate.minusDays(180);
        System.out.println("minusDays-180"+minus);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240405221943474.png" alt="image-20240405221943474" style="zoom:67%;" />

### 4.2.2 瞬时：instance

-  Instant：时间线上的一个瞬时点。 这可能被用来记录应用程序中的事件时间戳。
   -  时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。
-  `java.time.Instant`表示时间线上的一点，而不需要任何上下文信息，例如，时区。概念上讲，`它只是简单的表示自1970年1月1日0时0分0秒（UTC）开始的秒数。`

| **方法**                        | **描述**                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| `now()`                         | **静态方法**，返回默认UTC时区的Instant类的对象               |
| `ofEpochMilli(long epochMilli)` | 静态方法，返回在1970-01-01 00:00:00基础上加上指定毫秒数之后的Instant类的对象 |
| atOffset(ZoneOffset offset)     | 结合即时的偏移来创建一个 OffsetDateTime                      |
| `toEpochMilli()`                | 返回1970-01-01 00:00:00到当前时间的毫秒数，即为时间戳        |

> 中国大陆、中国香港、中国澳门、中国台湾、蒙古国、新加坡、马来西亚、菲律宾、西澳大利亚州的时间与UTC的时差均为+8，也就是UTC+8。
>
> instant.atOffset(ZoneOffset.ofHours(8));

### 4.2.3 日期时间格式化

**DateTimeFormatter**：用于格式化和解析LocalDate,LocalTime,LocalDateTime，类似于SimpleDateFormat 解析Date对象

该类提供了三种格式化方法：

- (了解)预定义的标准格式。如：ISO_LOCAL_DATE_TIME、ISO_LOCAL_DATE、ISO_LOCAL_TIME


- (了解)本地化相关的格式。如：ofLocalizedDate(FormatStyle.LONG)

  ```java
  // 本地化相关的格式。如：ofLocalizedDateTime()
  // FormatStyle.MEDIUM / FormatStyle.SHORT :适用于LocalDateTime
  				
  // 本地化相关的格式。如：ofLocalizedDate()
  // FormatStyle.FULL / FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT : 适用于LocalDate
  ```

- 自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)   **这个经常用要熟悉方法 ofPattern**

| **方**   **法**                    | **描**   **述**                                     |
| ---------------------------------- | --------------------------------------------------- |
| **ofPattern(String**  **pattern)** | 静态方法，返回一个指定字符串格式的DateTimeFormatter |
| **format(TemporalAccessor** **t)** | 格式化一个日期、时间，返回字符串                    |
| **parse(CharSequence**  **text)**  | 将指定格式的字符序列解析为一个日期、时间            |

```java
public void test2(){
        //localDateTime
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println("before format:"+localDateTime);
        //DateTimeFormatter
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String format = dateTimeFormatter.format(localDateTime);
        System.out.println("after format :"+format);
        //parse string ---> format
        TemporalAccessor parsed = dateTimeFormatter.parse(format); //实现接口 temporalAccessor
        LocalDateTime localDateTime1 = LocalDateTime.from(parsed); //使用静态方法from 来parse字符串为 localDateTime
        System.out.println("after parse:"+localDateTime1);
}
```

<img src="E:\Notes\JavaSE\assets\image-20240405224127382.png" alt="image-20240405224127382" style="zoom:80%;" />

<img src="E:\Notes\JavaSE\assets\image-20240405223700793.png" alt="image-20240405223700793" style="zoom: 67%;" />

localdatetime 实现了 temporalAccessor接口  ，使用 localDateTime.from 静态方法 来parse字符串为 localdatetime对象

### 4.2.4 其他API用到再看

# 5 Java比较器



**基本数据类型**的数据（除boolean类型外）需要比较大小的话，之间使用**比较运算符**即可，但是**引用数据类型**是不能直接使用比较运算符来比较大小的。那么，如何解决这个问题呢？

- 在Java中经常会涉及到对象数组的排序问题，那么就涉及到对象之间的比较问题。


- Java实现对象排序的方式有两种：
  - 自然排序：java.lang.Comparable
  - 定制排序：java.util.Comparator

## 5.1 comparable自然排序

>  java.lang.Comparable

- Comparable接口强行对实现它的每个类的对象进行整体排序。这种排序被称为类的自然排序。
- 实现 Comparable 的类必须实现 `compareTo(Object obj) `方法，两个对象即通过 compareTo(Object obj) 方法的返回值来比较大小。如果当前对象this大于形参对象obj，则返回正整数，如果当前对象this小于形参对象obj，则返回负整数，如果当前对象this等于形参对象obj，则返回零

```java
package java.lang;

public interface Comparable{
    int compareTo(Object obj);
}
```

- 实现Comparable接口的**对象列表**（和数组）可以通过 Collections.sort 或 Arrays.sort进行自动排序。实现此接口的对象可以用作有序映射中的键或有序集合中的元素，无需指定比较器。
- Comparable 的典型实现：(`默认都是从小到大排列的`)
  - String：按照字符串中字符的Unicode值进行比较
  - Character：按照字符的Unicode值来进行比较
  - 数值类型对应的包装类以及BigInteger、BigDecimal：按照它们对应的数值大小进行比较
  - Boolean：true 对应的包装类实例大于 false 对应的包装类实例
  - Date、Time等：后面的日期时间比前面的日期时间大

```java
class Goods implements Comparable{
    private String name;
    private double price;

    public Goods() {
    }

    public Goods(String name, double price) {
        this.name = name;
        this.price = price;
    }

   /*get 和 set 方法 我先删去了  要不太长了*/

    @Override
    public String toString() {
        return "Goods{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    @Override //主要看实现了 comparable接口的 compareTo方法 进行比较
    public int compareTo(Object o) {
        if (o == this)
            return 0;
        if (o instanceof Goods){
            Goods temp = (Goods) o;
            return Double.compare(this.price,temp.price);
        }
        throw new RuntimeException("ClassCastException");
    }
}

//测试类
@Test
public void test1(){
    Goods[] all = new Goods[4];
    all[0] = new Goods("红楼梦", 100);
    all[1] = new Goods("西游记", 80);
    all[2] = new Goods("三国演义", 140);
    all[3] = new Goods("水浒传", 120);

    Arrays.sort(all);

    for (int i = 0; i < all.length; i++) {
        System.out.println(all[i].toString());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240405233718078.png" alt="image-20240405233718078" style="zoom:80%;" />

## 5.2 Comparator定制排序

> 思考

- 当元素的类型**没有实现**java.lang.Comparable接口而又**不方便修改代码**（例如：一些第三方的类，你只有.class文件，没有源文件）
- 如果一个类，实现了Comparable接口，也指定了两个对象的比较大小的规则，但是此时此刻我**不想按照它预定义**的方法比较大小，但是我又不能随意修改，因为会影响其他地方的使用，怎么办？

JDK在设计类库之初，也考虑到这种情况，所以又增加了一个java.util.Comparator接口。强行对多个对象进行整体排序的比较

- 重写compare(Object o1,Object o2)方法，比较o1和o2的大小：如果方法返回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示o1小于o2。

```java
实现步骤：
① 创建一个实现了Comparator接口的实现类A
② 实现类A要求重写Comparator接口中的抽象方法compare(Object o1,Object o2)，在此方法中指明要
   比较大小的对象的大小关系。（比如，String类、Product类）
③ 创建此实现类A的对象，并将此对象传递给 sort 方法（如 Collections.sort 或 Arrays.sort），从而允许在排序顺序上实现精确控制
//可以使用接口的匿名实现类的匿名对象这种写法
```

对 5.1的例子进行降序排序，利用comparator接口 重写compare方法

```java
@Test
public void test1(){
//        comparator接口 定制排序
    Goods[] all = new Goods[4];
    all[0] = new Goods("红楼梦", 100);
    all[1] = new Goods("西游记", 80);
    all[2] = new Goods("三国演义", 140);
    all[3] = new Goods("水浒传", 120);
	//sort方法 后者的比较器 是写了一个comparator接口的匿名实现类的匿名对象
    Arrays.sort(all, new Comparator<Goods>() {
        @Override
        public int compare(Goods o1, Goods o2) {
            if (o1 instanceof Goods && o2 instanceof Goods){
                Goods tmp1,tmp2;
                tmp1 = (Goods) o1;
                tmp2 = (Goods) o2;
                //按照 price进行降序排序
                return -Double.compare(tmp1.getPrice(),tmp2.getPrice());
            }
            throw new RuntimeException("class cast exception");
        }
    });

    for (int i = 0; i < all.length; i++) {
        System.out.println(all[i].toString());
    }
}
```



# 6 系统相关类

## 6.1 System类

- System类代表系统，系统级的很多属性和控制方法都放置在该类的内部。该类位于`java.lang包`。

- 由于该类的构造器是private的，所以无法创建该类的对象。其内部的成员变量和成员方法都是`static的`，所以也可以很方便的进行调用。

- 成员变量   Scanner scan = new Scanner(System.in);

  - System类内部包含`in`、`out`和`err`三个成员变量，分别代表标准输入流(键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)。

- 成员方法

  - `native long currentTimeMillis()`：
    该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。

  - `void exit(int status)`：
    该方法的作用是退出程序。其中status的值为0代表正常退出，非零代表异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等。

  - `void gc()`：
    该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。

  - `String getProperty(String key)`：
    该方法的作用是获得系统中属性名为key的属性对应的值。系统中常见的属性名以及属性的作用如下表所示：

    ![image-20240406002459980](E:\Notes\JavaSE\assets\image-20240406002459980.png)

## 6.2 Runtime类

每个 Java 应用程序都有一个 `Runtime` 类实例，使应用程序能够与其运行的环境相连接。

`public static Runtime getRuntime()`： 返回与当前 Java 应用程序相关的运行时对象。应用程序不能创建自己的 Runtime 类实例。

`public long totalMemory()`：返回 Java 虚拟机中初始化时的内存总量。此方法返回的值可能随时间的推移而变化，这取决于主机环境。默认为物理电脑内存的1/64。

`public long maxMemory()`：返回 Java 虚拟机中最大程度能使用的内存总量。默认为物理电脑内存的1/4。

`public long freeMemory()`：回 Java 虚拟机中的空闲内存量。调用 gc 方法可能导致 freeMemory 返回值的增加

**看看就行 也用不到**

# 7 数学相关类

## 7.1 Math类

`java.lang.Math` 类包含用于执行基本数学运算的方法，如初等指数、对数、平方根和三角函数。类似这样的工具类，其所有方法均为静态方法，并且不会创建对象，调用起来非常简单

* `public static double abs(double a) ` ：返回 double 值的绝对值。 

```java
double d1 = Math.abs(-5); //d1的值为5
double d2 = Math.abs(5); //d2的值为5
```

* `public static double ceil(double a)` ：返回大于等于参数的最小的整数。

```java
double d1 = Math.ceil(3.3); //d1的值为 4.0
double d2 = Math.ceil(-3.3); //d2的值为 -3.0
double d3 = Math.ceil(5.1); //d3的值为 6.0
```

* `public static double floor(double a) ` ：返回小于等于参数最大的整数。

```java
double d1 = Math.floor(3.3); //d1的值为3.0
double d2 = Math.floor(-3.3); //d2的值为-4.0
double d3 = Math.floor(5.1); //d3的值为 5.0
```

* `public static long round(double a)` ：返回最接近参数的 long。(相当于四舍五入方法)  

```java
long d1 = Math.round(5.5); //d1的值为6
long d2 = Math.round(5.4); //d2的值为5
long d3 = Math.round(-3.3); //d3的值为-3
long d4 = Math.round(-3.8); //d4的值为-4
```

* public static double pow(double a,double b)：返回a的b幂次方法
* public static double sqrt(double a)：返回a的平方根
* `public static double random()`：返回[0,1)的随机值  
* public static final double PI：返回圆周率
* public static double max(double x, double y)：返回x,y中的最大值
* public static double min(double x, double y)：返回x,y中的最小值
* 其它：acos,asin,atan,cos,sin,tan 三角函数

## 7.2 java.math包

有BigIntger 和BigDecimal 两个常用类 用到了查API就行 

## 7.3 Random类

java.util.Random：用于产生随机数

* `boolean nextBoolean()`:返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 boolean 值。 

* `void nextBytes(byte[] bytes)`:生成随机字节并将其置于用户提供的 byte 数组中。 

* `double nextDouble()`:返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 double 值。 

* `float nextFloat()`:返回下一个伪随机数，它是取自此随机数生成器序列的、在 0.0 和 1.0 之间均匀分布的 float 值。 

* `double nextGaussian()`:返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（“正态”）分布的 double 值，其平均值是 0.0，标准差是 1.0。 

* `int nextInt()`:返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值。 

* `int nextInt(int n)`:返回一个伪随机数，它是取自此随机数生成器序列的、在 0（包括）和指定值（不包括）之间均匀分布的 int 值。 

* `long nextLong()`:返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。 



