# Java高级

利用Java 基本语法和 前述的OOP思想，学习一些orcla官方 和第三方提供的API库类

# 1 多线程

## 1 .1基本概念

- **程序（program）**：为完成特定任务，用某种语言编写的`一组指令的集合`。即指`一段静态的代码`
- **进程（process）**：程序的一次执行过程，或是正在内存中运行的应用程序。如：运行中的QQ
  - 每个进程都有一个独立的内存空间，系统运行一个程序即是一个进程从创建、运行到消亡的过程。（生命周期）
  - 程序是静态的，进程是动态的
  - **进程**作为`操作系统调度和分配资源的最小单位`（亦是系统运行程序的基本单位），系统在运行时会为每个进程分配不同的内存区域
- **线程（thread）**：进程可进一步细化为线程，是程序内部的`一条执行路径`。**一个进程中至少有一个线程**。
  - 一个进程同一时间若`并行`执行多个线程，就是支持多线程的
  - 线程作为`CPU调度和执行的最小单位`。
  - 一个进程中的多个线程共享相同的内存单元，它们从同一个堆中分配对象，可**以访问相同的变量和对象**。这就使得**线程间通信**更简便、高效。但多个线程操作共享的系统资源可能就会带来`安全的隐患`。

------

**注意**：

**不同的进程之间是不共享内存**的。

进程之间的数据交换和通信的成本很高。

## 1.2 线程调度

- **分时调度**

  所有线程`轮流使用` CPU 的使用权，并且平均分配每个线程占用 CPU 的时间。

- **抢占式调度**

  让`优先级高`的线程以`较大的概率`优先使用 CPU。如果线程的优先级相同，那么会随机选择一个(线程随机性)，**Java使用的为抢占式调度。**

------

**补充：并行和并发**

- **并行（parallel）**：指两个或多个事件在`同一时刻`发生（同时发生）。指在同一时刻，有`多条指令`在`多个CPU`上`同时`执行。比如：多个人同时做不同的事。
  ![image-20240329212957102](E:\Notes\JavaSE\assets\image-20240329212957102.png)
  ![image-20240329213007318](E:\Notes\JavaSE\assets\image-20240329213007318.png)
- **并发（concurrency）**：指两个或多个事件在`同一个时间段内`发生。即在一段时间内，有`多条指令`在`单个CPU`上`快速轮换、交替`执行，使得在宏观上具有多个进程同时执行的效果。
  ![image-20240329213047473](E:\Notes\JavaSE\assets\image-20240329213047473.png)
  ![image-20240329213056814](E:\Notes\JavaSE\assets\image-20240329213056814.png)

# 2 创建和启动线程

- Java语言的JVM允许程序运行多个线程，使用`java.lang.Thread`类代表**线程**，所有的线程对象都必须是Thread类或其子类的实例
- Thread类的特性
  - 每个线程都是通过某个特定Thread对象的run()方法来完成操作的，因此把**run()方法体**称为`线程执行体`。
  - 通过该**Thread对象**的**start()方法来启动这个线程**，而非直接调用run()
  - 要想实现多线程，必须在主线程中创建新的线程对象。

## 2.1 方式1：继承Thread类

Java通过继承Thread类来**创建**并**启动多线程**的步骤如下：

1. 定义Thread类的子类，并重写该类的run()方法，该run()方法的方法体就代表了线程需要完成的任务
2. 创建Thread子类的实例，即创建了线程对象
3. 调用线程对象的start()方法来启动该线程

```java
// 创建一个线程  mythread  run方法写  输出100以内的偶数 
class MyThread extends Thread{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                //Thread.currentThread().getName() 是得到当前线程的名称
                System.out.println(Thread.currentThread().getName()+"--->"+i);
            }
        }
    }
}
```

```Java
//入口函数 main（）也是一个线程 ，我在其中再写一个输出偶数，同样前缀线程名称
public class MyThreadTest {
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                System.out.println(Thread.currentThread().getName()+"--->"+i);
            }
        }
    }
}
```

结果如下：

<img src="E:\Notes\JavaSE\assets\image-20240329214227321.png" alt="image-20240329214227321" style="zoom:80%;" />

可以看到  两个线程 main（）和 thread-0 交替执行  实现了并发的初体验

------

**为什么会出现这种交替执行的现象呢？** 以下答案存疑，需要去看书找答案！

这是因为 在JVM中  多个线程的 堆内存和方法区是共享的，但是 **栈空间独立** ，一个线程一个栈  ，一个线程抢到 CPU时间片之后 执行各自的代码，**时间片到了之后 各个线程再继续争夺时间片** 因此出现了 一个线程内 循环只执行了一次 然后其他线程的代码就执行

------

## 2.2 方式2：实现Runnable接口

Java有单继承的限制，当我们无法继承Thread类时，那么该如何做呢？在核心类库中提供了Runnable接口，我们可以实现Runnable接口，重写run()方法，然后再通过Thread类的对象代理启动和执行我们的线程体run()方法

步骤如下：

1. 定义Runnable接口的**实现类**，并实现该接口的run()方法，该run()方法的方法体同样是该线程的线程执行体。
2. 创建Runnable实现类的**实例**，并以此实例作为**Thread的target参数**来创建Thread对象，该Thread对象才是真正的线程对象。

3. 调用线程对象的start()方法，启动线程。调用Runnable接口实现类的run方法。

注意 target参数 是 Thread类的一个构造器参数

```java
public Thread (Runnable target)
```

把Runnable**接口的实现类** 当作参数传入到构造器中 ：

- 体现了多态 ，父类的引用指向了子类对象  即： Runnable target = 子类()
- 通过构造器 创建了 Thread类对象 ，即线程对象，可以调用start()方法 来启动线程

```java
//接口Runnable的实现类 MyRunnable 类 实现了run()方法
class MyRunnable implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                System.out.println(Thread.currentThread().getName()+"--->"+i);
            }
        }
    }
}
```

```java
//将接口实现类的对象 作为Thread类构造器参数 创建线程对象 调用start方法，这里写了两种不同方式
public class MyRunnableTest {
    public static void main(String[] args) {
        //接口的匿名对象
        new Thread(new MyRunnable()).start();
        //匿名接口的匿名对象写法 666 之前真没注意到
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i%2 == 1){
                        System.out.println(Thread.currentThread().getName()+":"+i);
                    }
                }
            }
        }).start();
    }
}
```

结果如下：

<img src="E:\Notes\JavaSE\assets\image-20240329223521949.png" alt="image-20240329223521949" style="zoom:80%;" />

在启动的多线程的时候，需要先通过Thread类的构造方法Thread(Runnable target) 构造出对象，然后调用Thread对象的start()方法来运行多线程代码。

实际上，所有的多线程代码都是通过运行Thread的start()方法来运行的。因此，不管是继承Thread类还是实现
Runnable接口来实现多线程，最终还是通过Thread的对象的API来控制线程的，熟悉Thread类的API是进行多线程编程的基础。

说明：Runnable对象仅仅作为Thread对象的target，Runnable实现类里包含的run()方法仅作为线程执行体。
而实际的线程对象依然是Thread实例，只是该Thread线程负责执行其target的run()方法。

![image-20240329223906485](E:\Notes\JavaSE\assets\image-20240329223906485.png)

## 两种方式比较和联系

**联系**

Thread类实际上也是实现了Runnable接口的类。 如上图所示

```java
public class Thread extends Object implements Runnable
```

**区别**

- 继承Thread：线程代码存放Thread子类run方法中。

- 实现Runnable：线程代码存在接口的子类的run方法。

------

# 3 Thread类的常用结构

## 3.1 构造器

- public Thread() :分配一个新的线程对象。
- public Thread(String name) :分配一个指定名字的新的线程对象。
- public Thread(Runnable target) :指定创建线程的目标对象，它实现了Runnable接口中的run方法
- public Thread(Runnable target,String name) :分配一个带有指定目标新的线程对象并指定名字。

## 3.2 常用方法系列1

* public void run() :此线程要执行的任务在此处定义代码。
* public void start() :导致此线程开始执行; Java虚拟机调用此线程的run方法。
* public String getName() :获取当前线程名称。
* public void setName(String name)：设置该线程名称。 get/set
* public static Thread currentThread() :返回对当前正在执行的线程对象的引用。在Thread子类中就是this，通常用于主线程和Runnable实现类
* public static void sleep(long millis) :使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。
* public static void yield()：yield只是让当前线程暂停一下，丢失cpu的执行权
  让系统的线程调度器重新调度一次，希望优先级与当前线程相同或更高的其他线程能够获得执行机会，但是这个不能保证，完全有可能的情况是，当某个线程调用了yield方法暂停之后，线程调度器又将其调度出来重新执行。

## 3.3 常用方法系列2

* public final boolean isAlive()：测试线程是否处于活动状态。如果线程已经启动且尚未终止，则为活动状态。 

* void join() ：等待该线程终止。 

  void join(long millis) ：等待该线程终止的时间最长为 millis 毫秒。如果millis时间到，将不再等待。 

  void join(long millis, int nanos) ：等待该线程终止的时间最长为 millis 毫秒 + nanos 纳秒。 
  **join 这样理解，在A线程中 调用B线程的 B.join() ，相当于让B来帮忙，自然先执行B线程的操作**

* public final void stop()：`已过时`，**不建议使用**。强行结束一个线程的执行，直接进入死亡状态，可能会导致一些清理性的工作得不到完成，如文件，数据库等的关闭。同时，会立即释放该线程所持有的所有的锁，导致数据得不到同步的处理，出现数据不一致的问题。

* void suspend() / void resume() : 这两个操作就好比播放器的暂停和恢复。二者必须成对出现，否则非常容易发生死锁。suspend()调用会导致线程暂停，但不会释放任何锁资源，导致其它线程都无法访问被它占用的锁，直到调用resume()。`已过时`，**不建议使用**。

也就是常用方法2 只需要记得 **isAlive() 和 join()**两个方法就行 ，其他三个了解即可

## 3.3 常用方法系列3

每个线程都有一定的优先级，同优先级线程组成先进先出队列（先到先服务），使用分时调度策略。优先级高的线程采用抢占式策略，获得较多的执行机会。每个线程默认的优先级都与创建它的父线程具有相同的优先级。

- Thread类的三个优先级常量：
  - MAX_PRIORITY（10）：最高优先级 
  - MIN _PRIORITY （1）：最低优先级
  - NORM_PRIORITY （5）：普通优先级，默认情况下main线程具有普通优先级。

* public final int getPriority() ：返回线程优先级 
* public final void setPriority(int newPriority) ：改变线程的优先级，范围在[1,10]之间。

相当于 get/set方法

# 4 多线程的生命周期

## 4.1 JDK1.5之前：5种状态

线程的生命周期有五种状态：新建（New）、就绪（Runnable）、运行（Running）、阻塞（Blocked）、死亡（Dead）。CPU需要在多条线程之间切换，于是线程状态会多次在运行、阻塞、就绪之间切换。

![image-20240330004108953](E:\Notes\JavaSE\assets\image-20240330004108953.png)

**1.新建**

当一个Thread类或其子类的对象**被声明并创建时**（new Thread()），新生的线程对象处于新建状态。此时它和其他Java对象一样，仅仅由JVM为其分配了内存，并初始化了实例变量的值。此时的线程对象并没有任何线程的动态特征，程序也不会执行它的线程体run()。

**2.就绪**

但是当**线程对象调用了start()方法**之后，就不一样了，线程就从新建状态转为就绪状态。JVM会为其创建方法调用栈和程序计数器，当然，处于这个状态中的线程并没有开始运行，只是表示已具备了运行的条件，随时可以被调度。至于什么时候被调度，**取决于JVM里线程调度器的调度**。

------

**3.运行**

如果处于就绪状态的线程获得了CPU资源时，开始执行run()方法的线程体代码，则该线程处于运行状态。如果计算机只有一个CPU核心，在任何时刻只有一个线程处于运行状态，如果计算机有多个核心，将会有多个线程并行(Parallel)执行。

对于抢占式策略的系统而言，系统会给每个可执行的线程一个小时间段来处理任务，当该时间用完，系统会剥夺该线程所占用的资源，让其回到就绪状态等待下一次被调度。此时其他线程将获得执行机会，而在选择下一个线程时，系统会适当考虑线程的优先级。

------

**4.阻塞**

当在运行过程中的线程遇到如下情况时，会让出 CPU 并临时中止自己的执行，进入阻塞状态：

* 线程调用了**sleep()**方法，主动放弃所占用的CPU资源；
* 线程试图获取一个同步监视器，但该同步监视器正被其他线程持有；
* 线程执行过程中，同步监视器调用了**wait()**，让它等待某个通知（notify）；
* 线程执行过程中，同步监视器调用了wait(time)
* 线程执行过程中，遇到了其他线程对象的加塞（**join**）；

线程被调用**suspend**方法被挂起（已**过时**，因为容易发生死锁）也会进入阻塞状态。

当前正在执行的线程被阻塞后，其他线程就有机会执行了。针对如上情况，当发生如下情况时会**解除阻塞**，让该线程重新进入就绪状态，等待线程调度器再次调度它：

* 线程的**sleep()时间到**；
* 线程成功获得了同步监视器；
* 线程等到了通知(**notify**)；
* 线程**wait**的时间到了
* 加塞的线程结束了 **join**的方法实现结束；

------

**5.死亡**

线程会以以下三种方式之一结束，结束后的线程就处于死亡状态：

* run()方法执行完成，线程正常结束
* 线程执行过程中抛出了一个未捕获的异常（Exception）或错误（Error）
* 直接调用该线程的stop()来结束该线程（已过时）

## 4.2 JDK1.5及之后：6种状态

在java.lang.Thread.State的枚举类中这样定义：

```java
public enum State {
	NEW,
	RUNNABLE,
	BLOCKED,
	WAITING,
	TIMED_WAITING,
	TERMINATED;
}
```

![image-20240330005253795](E:\Notes\JavaSE\assets\image-20240330005253795.png)

RUNNABLE 相当于 jdk1.5之前的 就绪和运行两个状态，， 1.5之后 将阻塞状态划分的更细致了 

# 5 线程安全问题及解决

使用多个线程访问**同一资源**（可以是同一个变量、同一个文件、同一条记录等）的时候，若多个线程`只有读操作`，那么不会发生线程安全问题。但是如果多个线程中对资源有`读和写`的操作，就容易出现线程安全问题。

## 5.1 同一个资源问题和线程安全问题

案例：

火车站要卖票，模拟火车站的卖票过程。本次列车的座位共100个（即，只能出售100张火车票）。来模拟车站的售票窗口，实现3个窗口同时售票的过程。注意：不能出现错票、重票

### 5.1.1 局部变量不能共享

ok 先写线程代码 

```java
//线程类 window 继承Thread
class Window extends Thread{
    int ticket = 100;
    @Override
    public void run() {
        try {
            Thread.sleep(300);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        while (true){
            if (ticket > 0){
                System.out.println(getName()+" sale one ticket and the num is " + ticket--);
            }else {
                break;
            }
        }
    }
}
```

主线程：

```Java
public class SaleTicketDemo1 {
    public static void main(String[] args) {
        Window window = new Window();
        Window window1 = new Window();
        Window window2 = new Window();
        window.setName("window0");
        window1.setName("window1");
        window2.setName("window2");
        window.start();
        window1.start();
        window2.start();
    }
}
```

结果如下图所示：

<img src="E:\Notes\JavaSE\assets\image-20240330053402185.png" alt="image-20240330053402185" style="zoom:80%;" />

分析：由于我的 ticket是写在window类中的 是成员变量，而在主线程main()中 创建了3个子线程，每一个线程都有一个ticket = 100 所以每一个线程都从100开始卖票，即卖了300张 

### 5.1.2 静态变量共享 

在上述代码 声明ticket 变量 前加修饰符 static 使之成为类变量 这样所有对象共享一个ticket

```java
static int ticket = 100；
```

结果如下：

![image-20240330054438397](E:\Notes\JavaSE\assets\image-20240330054438397.png)

![image-20240330054509208](E:\Notes\JavaSE\assets\image-20240330054509208.png)

可以看到 虽然结果是卖出了100张左右的票，但是有**重复票和0票问题**（本意是票号1-100）

这就是由于线程不安全引起的，前一个线程已经修改完共享数据了，后一个线程拿的还是没有修改的数据 出现了重复票

一个线程已经判断完ticket是合法的（大于0），其他的线程已经将其修改为0了，出现了非法票号问题

### 5.1.3 同一个对象的实例变量共享

使用Runnable接口的实现类创建线程，ticket不用加static就可以实现共享 因为 创建的线程使用的对象都是**同一个**，成员变量自然只有一份。

![image-20240330055721695](E:\Notes\JavaSE\assets\image-20240330055721695.png)

结果：发现卖出近100张票。

问题：但是有重复票或负数票问题。

原因：线程安全问题

## 5.2 同步机制解决线程安全问题

要解决上述多线程并发访问一个资源的安全性问题:也就是解决重复票与不存在票问题，Java中提供了同步机制
(**synchronized**)来解决

### 5.2.1 同步机制原理

同步机制的原理，其实就相当于给某段代码加“锁”，任何线程想要执行这段代码，都要先获得“锁”，我们称它为同步锁。因为Java对象在堆中的数据分为分为**对象头、实例变量、空白的填充**。而对象头中包含：

- Mark Word：记录了和当前对象有关的GC、锁标记等信息。
- 指向类的指针：每一个对象需要记录它是由哪个类创建出来的。
- 数组长度（只有数组对象才有）

哪个线程获得了“同步锁”对象之后，”同步锁“对象就会记录这个**线程的ID**，这样其他线程就只能等待了，除非这个线程”释放“了锁对象，其他线程才能重新获得/占用”同步锁“对象。

### 5.2.2 同步代码块和同步方法

**同步代码块**：synchronized 关键字可以用于某个区块前面，表示只对这个区块的资源实行互斥访问。
格式:

```java
synchronized(同步锁){
     需要同步操作的代码
}
```

注意，同步锁 是一个对象，要确保**全局唯一**

**同步方法：**synchronized 关键字直接修饰方法，表示同一时刻只有一个线程能进入这个方法，其他线程在外面等着。

```java
public synchronized void method(){
    可能会产生线程安全问题的代码
}
```

### 5.2.3 synchronized的锁是什么

同步锁对象可以是任意类型，但是必须保证竞争“同一个共享资源”的多个线程必须使用同一个“同步锁对象”。

对于**同步代码块**来说，同步锁对象是由程序员手动指定的（很多时候也是指定为**this或类名.class**），但是对于**同步方法**来说，同步锁对象只能是默认的：

- **静态方法：当前类的Class对象（类名.class）**  唯一

- **非静态方法：this**  在实现Runnable接口的线程中用，因为其唯一

### 5.2.4 同步操作的思考顺序

1、如何找问题，即代码是否存在线程安全？
（1）明确多个线程是否**有共享数据**
（2）明确多线程运行代码中有多条语句**操作共享数据**

2、如何解决呢？
对多条操作共享数据的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以参与执行。
即**所有操作共享数据的这些语句都要放在同步范围中**

3、切记：

范围太小：不能解决安全问题

范围太大：因为一旦某个线程抢到锁，其他线程就只能等待，所以范围太大，效率会降低，不能合理利用CPU资源。

### 5.2.4 代码演示

#### 示例一：同步代码块 && 继承Thread类方式创建的线程

创建线程代码：

```java
class Window3 extends Thread{
    static int ticket = 100;
    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (Window3.class) {//XXX.class 是唯一的 先这样记住
                if ( ticket > 0 ){
                    System.out.println(getName() + " sale one ticket and the num is " + ticket--);
                }else {
                    break;
                }
            }
        }
    }
}
```

需要注意的是 **Thread.sleep() 需要在synchronized同步锁之前**用，这样才能看到多个线程交替执行的结果。。要是再同步代码块中执行，没有意义，因为线程已经拿到锁了。。

主线程：

```Java
public class SaleTicketDemo3 {
    public static void main(String[] args) {
        Window3 window = new Window3();
        Window3 window1 = new Window3();
        Window3 window2 = new Window3();
        window.setName("window0");
        window1.setName("window1");
        window2.setName("window2");
        window.start();
        window1.start();
        window2.start();
    }
}
```

![image-20240330064936254](E:\Notes\JavaSE\assets\image-20240330064936254.png)

基本看到多个线程实现同步操作

#### 示例二：同步方法 && 继承 Thread类方式创建的线程

线程：

```java
class Window4 extends Thread{
    static int ticket = 100;
    @Override
    public void run() {//直接锁这里，肯定不行，会导致，只有一个窗口卖票
        while (ticket > 0){
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            saleTicket();
        }
    }

    //锁对象是Window4类的Class对象，而一个类的Class对象在内存中肯定只有一个
    //注意  静态方法啊  ，因为不加static  锁的对象就是this了，而继承thread的方式this 不唯一
    public static synchronized void saleTicket(){
        //不加条件，相当于条件判断没有进入锁管控，线程安全问题就没有解决
        if (ticket > 0 ){
            System.out.println(Thread.currentThread().getName()+ " sale one ticket and the num is " + ticket--);
        }
    }
}
```

主线程：

```java
public class SaleTicketDemo4 {
    public static void main(String[] args) {
        Window4 window = new Window4();
        Window4 window1 = new Window4();
        Window4 window2 = new Window4();
        window.setName("window0");
        window1.setName("window1");
        window2.setName("window2");
        window.start();
        window1.start();
        window2.start();
    }
}
```

结果：

<img src="E:\Notes\JavaSE\assets\image-20240330070324940.png" alt="image-20240330070324940" style="zoom:80%;" />

多个线程 同步操作共享数据

#### 示例三：同步代码块 && 实现Runnable接口类

子线程：

```java
class Window5 implements Runnable{
    int ticket = 100;
    @Override
    public void run() {
        while ( ticket > 0 ){
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //同步锁 对象唯一  这个在几个线程中就创建一次
            synchronized (this){
                if (ticket > 0){
                    System.out.println(Thread.currentThread().getName()+" sale one ticket and num is :"+ ticket--);
                }
            }
        }
    }
}
```

主线程：

```java
public class SaleTicketDemo5 {
    public static void main(String[] args) {
        Runnable window = new Window5();
        Thread thread1 = new Thread(window, "thread-1");
        Thread thread2 = new Thread(window, "thread-2");
        Thread thread3 = new Thread(window, "thread-3");
        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

结果：

<img src="E:\Notes\JavaSE\assets\image-20240330071504079.png" alt="image-20240330071504079" style="zoom:80%;" />

#### 示例四：同步方法 && 实现Runnable接口类

子线程：

```java
class Window6 implements Runnable{
    int ticket = 100;
    @Override
    public void run() {//直接锁这里，肯定不行，会导致，只有一个窗口卖票
        while ( ticket > 0 ){
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            saleTicket();
        }
    }
    //锁对象是this，这里就是Window6对象，因为上面3个线程使用同一个Window6对象，所以可以
    public synchronized void  saleTicket(){
        if (ticket > 0){//不加条件，相当于条件判断没有进入锁管控，线程安全问题就没有解决
            System.out.println(Thread.currentThread().getName()+" sale one ticket and num is :"+ ticket--);
        }
    }
}
```

主线程：

```java
public class SaleTicketDemo6 {
    public static void main(String[] args) {
        Runnable window = new Window6();
        Thread thread1 = new Thread(window, "thread-1");
        Thread thread2 = new Thread(window, "thread-2");
        Thread thread3 = new Thread(window, "thread-3");
        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```

结果：

![image-20240330072024585](E:\Notes\JavaSE\assets\image-20240330072024585.png)

# 6 再谈同步

## 6.1 单例设计模式的线程安全问题

### 6.1.1 饿汉式是线程安全的

实现一个类的饿汉单例模式

```java
class HungrySingle{
    //饿汉嘛， 就要先new 填饱肚子
    private static HungrySingle instance = new HungrySingle();
	//构造器 私有 暴漏一个静态的获取实例的方法
    private HungrySingle(){}

    public static HungrySingle getInstance(){
        return instance;
    }
}
```

测试类 多线程创建单例

```Java
public class HungrySingleTest {
    static HungrySingle hungrySingle1 = null;
    static HungrySingle hungrySingle2 = null;

    public static void main(String[] args) {
        Thread thread = new Thread() {
            @Override
            public void run() {
                hungrySingle1 = HungrySingle.getInstance();
            }
        };
        Thread thread1 = new Thread() {
            @Override
            public void run() {
                hungrySingle2 = HungrySingle.getInstance();
            }
        };
        thread.start();
        thread1.start();
        //主线程中让两个子线程先执行 ，利用 join()函数
        try {
            thread1.join();
            thread.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        //输出两个对象 并判断是否相等
        System.out.println(hungrySingle1);
        System.out.println(hungrySingle2);
        System.out.println(hungrySingle1 == hungrySingle2);
    }
}
```

结果：

![image-20240330201859944](E:\Notes\JavaSE\assets\image-20240330201859944.png)

拿到的永远是 同一个对象，因为**饿汉在全局已经创建好了同一个对象**，需要用就拿去

### 6.1.2 懒汉式线程安全问题

实现一个懒汉单例模式：

```Java
class LazySingle {
    //懒汉嘛，先不创建 需要用再创建
    private static LazySingle instance = null;
    private LazySingle(){}
    public static LazySingle getInstance(){
        if (instance == null){
            //为了看到效果 ，线程进入到这里 先阻塞一会 sleep()
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //创建实例 ，不同的线程 都判断为null进入分支，然后各自创建了一个实例对象返回
            instance = new LazySingle();
        }
        return instance;
    }
}
```

测试代码：

```java
public class LazySingleTest {
    static LazySingle lazySingle1 = null;
    static LazySingle lazySingle2 = null;

    public static void main(String[] args) {
        Thread t1 = new Thread() {
            @Override
            public void run() {
                lazySingle1 = LazySingle.getInstance();
            }
        };
        Thread t2 = new Thread() {
            @Override
            public void run() {
                lazySingle2 = LazySingle.getInstance();
            }
        };
        t1.start();
        t2.start();

        try {
            t1.join();
            t2.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(lazySingle1);
        System.out.println(lazySingle2);
        System.out.println(lazySingle1 == lazySingle2);
    }
}
```

结果：

<img src="E:\Notes\JavaSE\assets\image-20240330203558762.png" alt="image-20240330203558762" style="zoom:80%;" />

两个不同的线程 创建了两个不同的对象，这违背了单例模式的设计原则，因此懒汉单例模式 是线程不安全的

### 6.1.3 懒汉式线程安全解决方式

**方式一**：

利用同步代码方法,  在两个线程中都是调用的 getinstance（）方法获取的对象 因此可以给该方法加同步锁 synchronized

```java
class LazySingle {
    //懒汉嘛，先不创建 需要用再创建
    private static LazySingle instance = null;
    private LazySingle(){}
    public static synchronized LazySingle getInstance(){
        //synchronized修饰的静态方法，同步锁对象为 LazySingle.class 全局唯一

        if (instance == null){
            //为了看到效果 ，线程进入到这里 先阻塞一会 sleep()
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            //获取实例
            instance = new LazySingle();
        }
        return instance;
    }
}
```

在懒汉单例模式 的getInstance() 方法 加 synchronized修饰 使其同步，，测试代码不变，结果如下

![image-20240330204308703](E:\Notes\JavaSE\assets\image-20240330204308703.png)

------

**方式二**：

利用同步代码块

```Java
class LazySingle {
    //懒汉嘛，先不创建 需要用再创建
    private static LazySingle instance = null;
    private LazySingle(){}
    public static LazySingle getInstance(){
        //同步块外边包一层判断，是为了使其他线程 拿不到这个同步锁 （已经创建好了 直返返回就行）
        //减少资源浪费
        if (instance == null){
            //同步锁 还是使用类名.class 全局唯一
            synchronized (LazySingle.class){
                //内层 判断不能少 否则 就和没有同步一样  因为都是null进来创建 一个instance
                if (instance == null){
                    //为了看到效果 ，线程进入到这里 先阻塞一会 sleep()
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    //获取实例
                    instance = new LazySingle();
                }
            }
        }
        return instance;
    }
}
```

结果如下：

![image-20240330205002217](E:\Notes\JavaSE\assets\image-20240330205002217.png)

## 6.2  锁 Lock

### 6.2.1 死锁

不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁。

![image-20240331163410787](E:\Notes\JavaSE\assets\image-20240331163410787.png)

**代码举例**

```Java
public class DeadLock {
    public static void main(String[] args) {
        StringBuilder stringBuilder1 = new StringBuilder();
        StringBuilder stringBuilder2 = new StringBuilder();
	//该线程 先拿锁s1  添加字符，输出  休眠一下拿锁s2
        new Thread(){
            @Override
            public void run() {
                synchronized (stringBuilder1){
                    stringBuilder1.append("a");
                    stringBuilder2.append("1");
                    System.out.println(stringBuilder1);
                    System.out.println(stringBuilder2);
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (stringBuilder2){
                        stringBuilder1.append("b");
                        stringBuilder2.append("2");
                    }

                }
            }
        }.start();
        //线程先拿锁 s2 append字符 之后休眠一下 再拿锁s1
        new Thread(){
            @Override
            public void run() {
                synchronized (stringBuilder2){
                    stringBuilder1.append("c");
                    stringBuilder2.append("3");
                    System.out.println(stringBuilder1);
                    System.out.println(stringBuilder2);
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    synchronized (stringBuilder1){
                        stringBuilder1.append("d");
                        stringBuilder2.append("4");
                    }

                }
            }
        }.start();
    }
}
//两个线程形成死锁，互相等待对方释放锁 
```

结果如下所示：

![image-20240331165336955](E:\Notes\JavaSE\assets\image-20240331165336955.png)

注意 程序还没结束 但是看上去 “静止不动了” 结果可以看到 第一个线程 拿到了同步锁s1，第二个线程 拿到了同步锁s2  互相等待

### 6.2.2 JDK5.0新特性：Lock(锁)

- java.util.concurrent.locks.Lock接口是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对Lock对象加锁，线程开始访问共享资源之前应先获得Lock对象
- 在实现线程安全的控制中，比较常用的是`ReentrantLock`，可以显式加锁、释放锁。
  - ReentrantLock类实现了 Lock 接口，它拥有与 synchronized 相同的并发性和内存语义
- Lock锁也称同步锁，加锁与释放锁方法，如下：

  * public void lock() :加同步锁。
  * public void unlock() :释放同步锁。
- Lock锁对象 在并发的线程之间要求是唯一的

代码如下，利用可重入锁 解决买票问题：

```Java
public class LockTest {
    public static void main(String[] args) {
        //三个线程卖票
        Window window = new Window();
        Window window1 = new Window();
        Window window2 = new Window();
        window.setName("window0");
        window1.setName("window1");
        window2.setName("window2");
        window.start();
        window1.start();
        window2.start();
    }
}
class Window extends Thread{
    private static int ticket = 100;
    //定义唯一的锁对象 lock
    private static final ReentrantLock LOCK = new ReentrantLock();
    @Override
    public void run() {

        while (ticket > 0 ){

            try {
                Thread.sleep(10);
                //在关键的买票环节 进行加锁 ，不能加在while外边 不然就是一个线程把票卖完了
                LOCK.lock();
                if (ticket > 0){
                    System.out.println(getName()+" sale one ticket and the num is " + ticket--);
                }
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }finally {
                //加锁 一定要解锁 ，因此放在finally中  避免不被执行
                LOCK.unlock();
            }
        }
    }
}
```

更多的锁知识 在**JUC**中进行讲解 ，在**JUC**进行深入学习

# 7 线程通信

## 7.1 基本概念

**线程间通信的理解**
当我们`需要多个线程`来共同完成一件任务，并且我们希望他们有规律的执行，那么多线程之间需要一些通信机制，
可以协调它们的工作，以此实现多线程共同操作一份数据。

比如：线程A用来生产包子的，线程B用来吃包子的，包子可以理解为同一资源，线程A与线程B处理的动作，一个是生产，一个是消费，此时B线程必须等到A线程完成后才能执行，那么线程A与线程B之间就需要线程通信，即—— **等待唤醒机制

## 7.2 等待唤醒机制

这是多个线程间的一种`协作机制`。谈到线程我们经常想到的是线程间的`竞争（race）`，比如去争夺锁，但这并不是故事的全部，线程间也会有协作机制。

在一个线程满足某个条件时，就进入等待状态（`wait() / wait(time)`）， 等待其他线程执行完他们的指定代码过后再将其唤醒（`notify()`）;或可以指定wait的时间，等时间到了自动唤醒；在有多个线程进行等待时，如果需要，可以使用 `notifyAll()`来唤醒所有的等待线程。wait/notify 就是线程间的一种协作机制。

**涉及到三个方法的使用**：
wait():线程一旦执行此方法，就进入等待状态。同时，会释放对同步监视器的调用
notify():一旦执行此方法，就会唤醒被wait()的线程中优先级最高的那一个线程。（如果被wait()的多个线程的优先级相同，则
      随机唤醒一个）。被唤醒的线程从当初被wait的位置继续执行。
notifyAll():一旦执行此方法，就会唤醒所有被wait的线程。

## 7.3 线程通信简单代码示例

例题：使用两个线程打印 1-100。线程1, 线程2 交替打印

```java
public class PrintNumberTest {
    public static void main(String[] args) {
        //创建线程
        PrintNumber printNumber = new PrintNumber();
        new Thread(printNumber,"thread-1").start();
        new Thread(printNumber,"thread-2").start();
    }
}

class PrintNumber implements Runnable{
    private int number = 1;
    @Override
    public void run() {
        while (true){
            // 等待唤醒机制
            synchronized (this){
                notify();//拿到锁之后 才能唤醒其他线程
                if (number < 101){
                    System.out.println(Thread.currentThread().getName()+":"+number++);
                }else { break; }
                try {
                    wait();//线程进入阻塞状态，失去同步锁
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

结果：

<img src="E:\Notes\JavaSE\assets\image-20240331190625871.png" alt="image-20240331190625871" style="zoom:80%;" />

## 7.4 wait和notify注意细节

1. wait方法与notify方法必须要由`同一个锁对象调用`。因为：对应的锁对象可以通过notify唤醒使用同一个锁对象调用的wait方法后的线程。
2. wait方法与notify方法是属于Object类的方法的。因为：锁对象可以是任意对象，而任意对象的所属类都是继承了Object类的。
3. wait方法与notify方法必须要在`同步代码块`或者是`同步函数`中使用。因为：必须要`通过锁对象`调用这2个方法。否则会报java.lang.IllegalMonitorStateException异常。

**wait和sleep比较**：

相同点：一旦执行，当前线程都会进入阻塞状态

不同点：
> - 声明的位置：wait():声明在Object类中
>          sleep():声明在Thread类中，静态方法
> - 使用的场景不同：wait():只能使用在同步代码块或同步方法中
>             sleep():可以在任何需要使用的场景
> - 使用在同步代码块或同步方法中：**wait():一旦执行，会释放同步锁**
>                        **sleep():一旦执行，不会释放同步监视器**
> - 结束阻塞的方式：wait(): 到达指定时间自动结束阻塞 或 通过被notify唤醒，结束阻塞
>             sleep(): 到达指定时间自动结束阻塞

## 7.5 生产者消费者模式

等待唤醒机制可以解决经典的“生产者与消费者”的问题。生产者与消费者问题（英语：Producer-consumer problem），也称有限缓冲问题（英语：Bounded-buffer problem），是一个多线程同步问题的经典案例

**举例：**

生产者(Productor)将产品交给店员(Clerk)，而消费者(Customer)从店员处取走产品，店员一次只能持有固定数量的产品(比如:20），如果生产者试图生产更多的产品，店员会叫生产者停一下，如果店中有空位放产品了再通知生产者继续生产；如果店中没有产品了，店员会告诉消费者等一下，如果店中有产品了再通知消费者来取走产品。

**生产者与消费者问题中其实隐含了两个问题：**

* 线程安全问题：因为生产者与消费者共享数据缓冲区，产生安全问题。不过这个问题可以使用**同步**解决。
* 线程的协调工作问题：
  * 要解决该问题，就必须让生产者线程在缓冲区满时等待(wait)，暂停进入阻塞状态，等到下次消费者消耗了缓冲区中的数据的时候，通知(notify)正在等待的线程恢复到就绪状态，重新开始往缓冲区添加数据。同样，也可以让消费者线程在缓冲区空时进入等待(wait)，暂停进入阻塞状态，等到生产者往缓冲区添加数据之后，再通知(notify)正在等待的线程恢复到就绪状态。通过这样的通信机制来解决此类问题。

代码如下：

```java
public class producerConsumerTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();
        Producer producer = new Producer(clerk);
        Consumer consumer = new Consumer(clerk);
        producer.start();
        consumer.start();
    }
}
//共享数据店员
class Clerk{
    private int productNum = 0;
    private static final int MAX_PRODUCT = 20;
    private static final int MIN_PRODUCT = 0;

    //添加方法
    public synchronized void addProduct(){
        if (productNum < MAX_PRODUCT){
            productNum++;
            System.out.println(Thread.currentThread().getName()+"-producer : produces one goods!");
            notify();
        }else {
            System.out.println("producer all goods  waiting for consumer!");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    //消费方法
    public synchronized void subProduct(){
        if (productNum > MIN_PRODUCT){
            System.out.println(Thread.currentThread().getName()+"-consumer : consumes one goods!");
            productNum--;
            notify();
        }else {
            System.out.println("consumer all goods  waiting for producer!");
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Producer extends Thread{
    private Clerk clerk;

    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        while (true){
            System.out.println("producer=============================");
            try {
                //半秒制造一个商品
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.addProduct();
        }
    }
}

class Consumer extends Thread{
    private Clerk clerk;

    public Consumer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        while (true){
            System.out.println("consumer=============================");
            try {
                //1秒 消费一个商品
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.subProduct();
        }

    }
}
```

结果如下：

<img src="E:\Notes\JavaSE\assets\image-20240331194628363.png" alt="image-20240331194628363" style="zoom:80%;" />

# 8 JDK5.0新增线程创建方式

## 8.1 实现Callable接口

```
1. 创建 Callable 接口的实现类，并实现 call() 方法，该 call() 方法将作为线程执行体，并且有返回值。

2. 创建 Callable 实现类的实例，使用 FutureTask 类来包装 Callable 对象，该 FutureTask 对象封装了该 Callable 对象的 call() 方法的返回值。

3. 使用 FutureTask 对象作为 Thread 对象的 target 创建并启动新线程。

4. 调用 FutureTask 对象的 get() 方法来获得子线程执行结束后的返回值。
```

与Runnable接口相比较：

> call() 可以有返回值，更灵活
>
> call() 可以使用throws方式抛出异常 更灵活
>
> callable() 使用了泛型参数  可以指明call方法的返回值

- 缺点 ：如果在主线程中需要获取分线程call() 的返回值，此时主线程是处于阻塞状态

代码展示：

```Java
public class NumPrintTest {
    public static void main(String[] args) {
        //2
        NumPrint numPrint = new NumPrint();
        //3
        FutureTask futureTask = new FutureTask<>(numPrint);
        Thread wkikyo = new Thread(futureTask, "wkikyo");
        wkikyo.start();

        Object sum = null;
        try {
            //4 主线程会进入阻塞状态，不一定非要步骤4
             sum = futureTask.get();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            throw new RuntimeException(e);
        }
        System.out.println("sum:"+ sum);
    }
}
//1
class NumPrint implements Callable{
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i < 101; i++) {
            if (i%2 == 1){
                System.out.println(Thread.currentThread().getName()+"--->"+i);
                sum += i;
            }
        }
        return sum;
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240403221512163.png" alt="image-20240403221512163" style="zoom: 50%;" />



## 8.2 使用线程池

**现有问题：**

如果并发的线程数量很多，并且每个线程都是执行一个时间很短的任务就结束了，这样频繁创建线程就会大大降低系统的效率，因为频繁创建线程和销毁线程需要时间。

那么有没有一种办法使得线程可以复用，即执行完一个任务，并不被销毁，而是可以继续执行其他的任务？

**思路：**提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。类似生活中的公共交通工具。

![线程池的理解](E:\Notes\JavaSE\assets\线程池的理解.jpg)

**好处：**

- 提高响应速度（减少了创建新线程的时间）

- 降低资源消耗（重复利用线程池中线程，不需要每次都创建）

- 便于线程管理
  - corePoolSize：核心池的大小
  - maximumPoolSize：最大线程数
  - keepAliveTime：线程没有任务时最多保持多长时间后会终止
  - …

**线程池相关API**

- JDK5.0之前，我们必须手动自定义线程池。从JDK5.0开始，Java内置线程池相关的API。在java.util.concurrent包下提供了线程池相关API：`ExecutorService` 和 `Executors`。
- `ExecutorService`：真正的线程池接口。常见子类ThreadPoolExecutor
  - `void execute(Runnable command)` ：执行任务/命令，没有返回值，一般用来**执行Runnable的实现类**
  - `<T> Future<T> submit(Callable<T> task)`：执行任务，有返回值，一般又来执行Callable
  - `void shutdown()` ：关闭连接池
- `Executors`：一个线程池的**工厂类**，通过此类的静态工厂方法可以创建多种类型的线程池对象。
  - `Executors.newCachedThreadPool()`：创建一个可根据需要创建新线程的线程池
  - `Executors.newFixedThreadPool(int nThreads)`; 创建一个可重用固定线程数的线程池
  - `Executors.newSingleThreadExecutor()` ：创建一个只有一个线程的线程池
  - `Executors.newScheduledThreadPool(int corePoolSize)`：创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。

代码演示：

```java
public class ExecutorTest {
    public static void main(String[] args) {
        //1 线程工厂 executors 创建线程池 ：提供指定线程数量的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(5);
        //2  线程池执行任务  需要提供实现Runnable接口或Callable接口实现类的对象
        executorService.execute(new NumThread1());
        executorService.execute(new NumThread2());
        //3 关闭线程池
        executorService.shutdown();
    }
}

class NumThread1 implements Runnable{
    @Override
    public void run() {
        for (int i = 1; i < 101; i++) {
            if (i%2 == 0){
                System.out.println(Thread.currentThread().getName()+"-->"+i);
            }
        }
    }
}

class NumThread2 implements Runnable{
    @Override
    public void run() {
        for (int i = 1; i < 101; i++) {
            if (i%2 == 1){
                System.out.println(Thread.currentThread().getName()+":"+i);
            }
        }
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240403223717252.png" alt="image-20240403223717252" style="zoom: 80%;" />
