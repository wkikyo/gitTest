

# 1 异常

## 1.1 Java异常体系

**Throwable**

`java.lang.Throwable` 类是Java程序执行过程中发生的异常事件对应的类的**根父类**

**Throwable中的常用方法**

* `public void printStackTrace()`：打印异常的详细信息。

  包含了异常的类型、异常的原因、异常出现的位置、**在开发和调试阶段都得使用printStackTrace。**

* `public String getMessage()`：获取发生异常的原因。

------

**Error 和 Exception**

Throwable可分为两类：Error和Exception。分别对应着`java.lang.Error`与`java.lang.Exception`两个类。

**Error：**Java虚拟机无法解决的严重问题。如：JVM系统内部错误、资源耗尽等严重情况。一般不编写针对性的代码进行处理。

- 例如：StackOverflowError（栈内存溢出）和OutOfMemoryError（堆内存溢出，简称OOM）。

**Exception:** 其它因编程错误或偶然的外在因素导致的一般性问题，需要使用针对性的代码进行处理，使程序继续运行。否则一旦发生异常，程序也会挂掉。例如：

- 空指针访问
- 试图读取不存在的文件
- 网络连接中断
- 数组角标越界

说明：

​	无论是Error还是Exception，还有很多子类，异常的类型非常丰富。当代码运行出现异常时，特别是不熟悉的异常时，不要紧张，把异常的简单**类名**，拷贝到API中去查去认识它即可

------

**编译时异常和运行时异常**

Java程序的执行分为**编译时过程**和**运行时过程**。有的错误只有在`运行时`才会发生。比如：除数为0，数组下标越界等

![image-20240328223038584](E:\Notes\JavaSE\assets\image-20240328223038584.png)

因此，根据异常可能出现的阶段，可以将异常分为：

- **编译时期异常**（即checked异常、受检异常）：在代码编译阶段，编译器就能明确`警示`当前代码`可能发生（不是一定发生）`xx异常，并`明确督促`程序员提前编写处理它的代码。如果程序员`没有编写`对应的异常处理代码，则编译器就会直接判定编译失败，从而不能生成字节码文件。通常，这类异常的发生不是由程序员的代码引起的，或者不是靠加简单判断就可以避免的，例如：**FileNotFoundException**（文件找不到异常）
- **运行时期异常**（即runtime异常、unchecked异常、非受检异常）：在代码编译阶段，编译器完全不做任何检查，无论该异常是否会发生，编译器都不给出任何提示。只有等代码运行起来并确实发生了xx异常，它才能被发现。通常，这类异常是**由程序员的代码编写不当**引起的
  - **java.lang.RuntimeException**类及它的子类都是运行时异常。比如：ArrayIndexOutOfBoundsException数组下标越界异常，ClassCastException类型转换异常

<img src="E:\Notes\JavaSE\assets\image-20240328223255223.png" alt="image-20240328223255223" style="zoom:80%;" />

## 1.2 常见错误和异常

### 1.2.1 Error

最常见的就是VirtualMachineError，它有两个经典的子类：StackOverflowError、OutOfMemoryError

```Java
public class JunitTest {
    @Test
    public void test1(){
        recursion();
    }
    public void recursion(){
        recursion();
    }
}
```

递归调用导致jvm的栈帧溢出，报错

<img src="E:\Notes\JavaSE\assets\image-20240328223652078.png" alt="image-20240328223652078" style="zoom:80%;" />

还有一个OOM 堆溢出，（出错需要调VM设置，暂且知道即可）

### 1.2.2 运行时异常

```Java
	@Test
    public void test2(){
        //NullPointerException
        int[] arr = null;
        System.out.println(arr[0]);
    }
```

空指针异常

![image-20240328224047828](E:\Notes\JavaSE\assets\image-20240328224047828.png)

```java
//ClassCastException
 @Test
    public void test3(){
        //ClassCastException
        Object obj = 15;
        String str = (String) obj;
    }
```

类转换异常 ，解决此异常 需**配合关键字 instanceof** 来使用

![image-20240328224315403](E:\Notes\JavaSE\assets\image-20240328224315403.png)

```java
 @Test
    public void test04(){
        //InputMismatchException
        Scanner input = new Scanner(System.in);
        System.out.print("请输入一个整数：");//输入非整数
        int num = input.nextInt();
        input.close();
    }
```

输入不匹配异常

<img src="E:\Notes\JavaSE\assets\image-20240328224508326.png" alt="image-20240328224508326" style="zoom:80%;" />

### 1.2.3 编译时异常

```Java
@Test
public void test07(){
   Class c = Class.forName("java.lang.String");//ClassNotFoundException
}
@Test
public void test08() {
    Connection conn = DriverManager.getConnection("....");  //SQLException
}
@Test
public void test09()  {
    FileInputStream fis = new FileInputStream("尚硅谷Java秘籍.txt"); //FileNotFoundException
}
```

比较多了 ，列举几个 ，但凡涉及到资源调用的  比如 文件IO， 连接数据库啊 或者 socket网络编程这些 都会有对应异常

## 1.3 异常处理（重点）

**Java异常处理**

Java采用的异常处理机制，是`将异常处理的程序代码集中在一起`，与正常的程序代码分开，使得程序简洁、优雅，并易于维护。

- try-catch-finally
- throws + 异常类型

------

### 1.3.1  方式1：捕获异常 (try-catch-finally)

**语法：**

```java
try{
	......	//可能产生异常的代码
}
catch( 异常类型1 e ){
	......	//当产生异常类型1型异常时的处置措施
}
catch( 异常类型2 e ){
	...... 	//当产生异常类型2型异常时的处置措施
}  
finally{
	...... //无论是否发生异常，都无条件执行的语句
} 
```

------

**1、整体执行过程：**

当某段代码可能发生异常，不管这个异常是编译时异常还是运行时异常，我们都可以使用try块将它括起来，并在try块下面编写catch分支尝试捕获对应的异常对象

- 如果在程序运行时，try块中的代码没有发生异常，那么catch所有的分支都不执行。
- 如果在程序运行时，try块中的代码发生了异常，根据异常对象的类型，将从上到下选择**第一个匹配**的catch分支执行。**此时try中发生异常的语句下面的代码将不执行**，而整个try...catch之后的代码可以继续运行。
- 如果在程序运行时，try块中的代码发生了异常，但是所有catch分支都无法匹配（捕获）这个异常，那么JVM将会终止当前方法的执行，并把异常对象“抛”给调用者。如果调用者不处理，程序就挂了。

<img src="E:\Notes\JavaSE\assets\image-20240328225223384.png" alt="image-20240328225223384" style="zoom:80%;" />

**2、try**

捕获异常的第一步是用`try{…}语句块`选定捕获异常的范围，将可能出现异常的业务逻辑代码放在try语句块中。

**3、catch (Exceptiontype e)**

- catch分支，分为两个部分，catch**()中编写异常类型和异常参数名**，**{}中编写**如果发生了这个异常，要做什么处理的代码。

- 如果明确知道产生的是何种异常，可以用该异常类作为catch的参数；也可以用其父类作为catch的参数。

  比如：可以用ArithmeticException类作为参数的地方，就可以用RuntimeException类作为参数，或者用所有异常的父类Exception类作为参数。但不能是与ArithmeticException类无关的异常，如NullPointerException（catch中的语句将不会执行）。

- 如果有多个catch分支，并且**多个异常类型有父子类关系**，**必须保证小的子异常类型在上，大的父异常类型在下**。否则，报错。

- catch中常用异常处理的方式

  - `public String getMessage()`：获取异常的描述信息，返回字符串

  - `public void printStackTrace()`：打印异常的跟踪栈信息并输出到控制台。包含了异常的类型、异常的原因、还包括异常出现的位置，在开发和调试阶段，都得使用printStackTrace()。

<img src="E:\Notes\JavaSE\assets\image-20240328225620031.png" alt="image-20240328225620031" style="zoom:80%;" />

------

基本上都使用 printStackTrace() 方法来进行捕获异常

### 1.3.2 finally使用

![image-20240328225813227](E:\Notes\JavaSE\assets\image-20240328225813227.png)

- 因为异常会引发程序**跳转**，从而会导致有些语句执行不到。而程序中有一些特定的代码无论异常是否发生，都`需要执行`。例如，**数据库连接、输入流输出流、Socket连接、Lock锁的关闭**等，这样的代码通常就会**放到finally块**中。所以，我们通常将一定要被执行的代码声明在finally中。

  - 唯一的例外，使用 System.exit(0) 来终止当前正在运行的 Java 虚拟机。

- 不论在try代码块中是否发生了异常事件，catch语句是否执行，catch语句是否有异常，catch语句中是否有return，finally块中的语句都会被执行。

- finally语句和catch语句是可选的，但**finally不能单独使用**。

------

 **异常处理的细节**

- 前面使用的异常都是`RuntimeException类`或是它的`子类`，（运行时异常）这些类的异常的特点是：即使没有使用try和catch捕获，**Java自己也能捕获**，并且编译通过 ( 但运行时会发生异常使得程序运行终止 )。所以，对于这类异常，可以不作处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响。

- 如果抛出的异常是IOException等类型的`非运行时异常`，则**必须捕获**，否则`编译错误`。也就是说，我们必须处理编译时异常，**将编译时异常进行捕捉，转化为运行时异常**。

------

### 1.3.3  方式2：声明抛出异常类型（throws）

- 如果在**编写方法体**的代码时，某句代码可能发生某个`编译时异常`，不处理编译不通过，但是在当前方法体中可能`不适合处理`或`无法给出合理的处理方式`，则此方法应`显示地`声明抛出异常，**表明该方法将不对这些异常进行处理，而由该方法的调用者负责处理。**
  <img src="E:\Notes\JavaSE\assets\image-20240328230407111.png" alt="image-20240328230407111" style="zoom:80%;" />

- 具体方式：在**方法声明**中用`throws语句`可以声明抛出异常的列表，throws后面的异常类型可以是方法中产生的异常类型，也可以是它的父类

------

**throws基本格式**

```java
修饰符 返回值类型 方法名(参数) throws 异常类名1,异常类名2…{   }	
```

------

**方法重写中throws的要求**

方法重写时，对于方法签名是有严格要求的：

```
（1）方法名必须相同
（2）形参列表必须相同
（3）返回值类型
	- 基本数据类型和void：必须相同
	- 引用数据类型：<=
（4）权限修饰符：>=，而且要求父类被重写方法在子类中是可见的
（5）不能是static，final修饰的方法
```

对于throws异常列表要求：

- 如果**父类被重写方法**的方法签名后面**没有 “throws**  编译时异常类型”，那么**重写方法时**，方法签名后面也**不能出现“throws**  编译时异常类型”。
- 如果**父类被重写方法**的方法签名后面**有** `throws  编译时异常类型`”，那么**重写方法**时，throws的编译时异常类型必须 **<=** 被重写方法throws的编译时异常类型，或者不throws编译时异常。
- 方法重写，对于“`throws 运行时异常类型`”没有要求。

### 1.3.4 两种异常处理方式的选择

- 如果程序代码中，涉及到**资源的调用**（流、数据库连接、网络连接等），则**必须考虑使用try-catch-finally来处理**，保证不出现内存泄漏。
- 如果**父类被重写的方法没有throws**异常类型，则**子类重写**的方法中如果出现异常，**只能考虑使用try-catch-finally**进行处理，不能throws。
- 开发中，方法a中依次调用了方法b,c,d等方法，方法b,c,d之间是递进关系。此时，如果方法b,c,d中有异常，我们通常选择使用throws，而方法a中通常选择使用try-catch-finally。如下图所示：

<img src="E:\Notes\JavaSE\assets\image-20240328231108659.png" alt="image-20240328231108659" style="zoom:80%;" />

## 1.4 手动抛出异常对象：throw

Java 中异常对象的生成有两种方式：

- 由虚拟机**自动生成**：程序运行过程中，虚拟机检测到程序发生了问题，那么针对当前代码，就会在后台自动创建一个对应异常类的实例对象并抛出。

- 由开发人员**手动创建**：`new 异常类型([实参列表]);`，如果创建好的异常对象不抛出对程序没有任何影响，和创建一个普通对象一样，但是一旦throw抛出，就会对程序运行产生影响了。

格式：

```java
throw new 异常类名(参数);
```

throw语句抛出的异常对象，和JVM自动创建和抛出的异常对象一样。

- 如果是**编译时异常**类型的对象，同样需要使用throws或者try...catch处理，否则编译不通过。

- 如果是运行时异常类型的对象，编译器不提示。

- 可以抛出的异常必须是Throwable或其子类的实例

throw语句会导致程序执行流程被改变，throw语句是明确抛出一个异常对象，因此它`下面的代码将不会执行`。

如果当前方法没有try...catch处理这个异常对象，throw语句就会`代替return语句`提前终止当前方法的执行，并返回一个异常对象给调用者。

## 1.5 自定义异常

用到再学把。。。
