# OOP

Java-OOP学习三条主线

* Java类及其成员： 属性、方法、构造器（重点）  ；  代码块、内部类（熟悉）
* 面向对象特征：封装、继承、多态、（抽象）
* 关键字： this、super、 package 、import 、 abstract、interface、static、 final ...

# 1 OOP基础

## 1.1  成员变量/属性

```java
[修饰符1] class 类名{
	[修饰符2] 数据类型 成员变量名 [= 初始化值];
}
```

* 位置要求：必须在**类中，方法外**
* 修饰符2：
  * 常用的权限修饰符有：private、缺省、protected、public
  * 其他修饰符：static、final
* 数据类型：任何基本数据类型(如int、Boolean) 或 任何引用数据类型
* 成员变量名：属于标识符，符合命名规则和规范即可
* 初始化值：可以显式赋值；也可以不赋值（使用默认值）

示例：

```java
public class Person{
	private int age; //声明private变量 age
	public String name = “Lila”; //声明public变量 name
}
```

## 成员变量 vs 局部变量

* 位置：
  （1）在**方法体外，类体内**声明的变量称为成员变量    
  （2）在**方法体内部（包括构造器、代码块）**等位置声明的变量称为局部变量
* 内存中存储的位置不同 （1）实例变量：堆 （2）局部变量：栈
* 生命周期 ：
  （1）成员变量：和对象的生命周期一样，随着对象的创建而存在，随着对象被GC回收而消亡，而且每一个对象的实例变量是独立的
  （2）局部变量：和方法调用的生命周期一样，每一次方法被调用而在存在，随着方法执行的结束而消亡， 而且每一次方法调用都是独立。
* 作用域 ：
  （1）成员变量：通过对象就可以使用，本类中直接调用，其他类中“对象.实例变量” 
  （2）局部变量：出了作用域就不能使用
* 修饰符：
  （1）成员变量：public,protected,private,final,volatile,transient等 
  （2）局部变量：final
* 默认值 ：
  （1）实例变量：有默认值 
  （2）局部变量：没有，必须手动初始化。其中的形参比较特殊， 靠实参给它初始化。

<img src="E:\Notes\JavaSE\assets\image-20240311224728336.png" alt="image-20240311224728336" style="zoom: 67%;" />

## 1.2 方法

Java里的方法**不能独立存在** ，所有的方法必须定义在类里

```
Math.random()的random()方法 
Math.sqrt(x)的sqrt(x)方法 
System.out.println(x)的println(x)方法 
new Scanner(System.in).nextInt()的nextInt()方法 
Arrays类中的binarySearch()方法、sort()方法、equals()方法
```

```
[修饰符] 返回值类型 方法名([形参列表])[throws 异常列表]{
	方法体的功能代码
}
```

* 修饰符：可选的。方法的修饰符也有很多，例如：public、protected、private、static、abstract、 native、final、synchronized等
* 返回值类型： 表示方法运行的结果的数据类型
* 方法名：属于标识符，命名时遵循标识符命名规则和规范，“见名知意”
* 形参列表：表示完成方法体功能时需要外部提供的数据列表。可以包含零个，一个或多个参数
  参数的类型可以是**基本数据类型、引用数据类型**

 **方法的重载：**

 定义：在同一个类中，允许存在一个以上的同名方法，只要它们的参数列表不同即可：意味着参数个数或参数类型的不同

特点：与修饰符、返回值类型无关，只看参数列表，且参数列表必须不同

重载方法调用：JVM通过方法的参数列表，调用匹配的方法

	* 先找个数、类型最匹配的
	* 再找个数和类型可以兼容的，如果同时多个方法可以兼容将会报错

 注意，重载的方法在调用时是根据**参数的静态类型**来选择的，而**不是根据运行时对象的实际类型**。如果无法选择合适的方法，编译器会报错  （在参数为引用类型时 用到这个）

**可变个数的形参：**

在JDK 5.0 中提供了Varargs(variable number of arguments)机制。即当定义一个方法时，**形参的类型可以** 确定，但是形参的**个数**不确定，那么可以考虑使用**可变个数的形参**。

```java
//格式：方法名(参数的类型名 ...参数名)

//JDK 5.0以前：采用数组形参来定义方法，传入多个同一类型变量
public static void test(int a ,String[] books);
//JDK5.0：采用可变个数形参来定义方法，传入多个同一类型变量
public static void test(int a ,String...books);
```

**特点：**

* 可变参数：方法参数部分指定类型的参数个数是可变多个：0个，1个或多个
* 可变个数形参的方法与**同名的方法**之间，彼此构成重载
* **可变参数**方法的使用与方法参数部分**使用数组**是一致的，二者不能同时声明，否则报错
* 方法的参数部分**有可变形参**，需要放在形参声明的**最后在一个的形参**中，最多**只能声明一个可变个数形参**

**方法的参数传递机制：**

Java里方法的参数传递方式只有一种： **值传递**    即将实际参数值的副本（复制品）传入方法内，而参数 本身不受影响。

* 形参是基本数据类型：将实参基本数据类型变量的“数据值”传递给形参
* 形参是引用数据类型：将实参引用数据类型变量的“**地址值**”传递给形参

## 1.3 构造器

new对象，并在new对象的时候为实例变量赋值

```Java
[修饰符] class 类名{
	[修饰符] 构造器名(){
		// 实例初始化代码
	}
	[修饰符] 构造器名(参数列表){
		// 实例初始化代码
	}
}

```

* 构造器名必须与它所在的类名必须相同 
* 它没有返回值，所以不需要返回值类型，也不需要void  
* 构造器的修饰符只能是权限修饰符，不能被其他任何修饰。比如，不能被static、final、 synchronized、abstract、native修饰，不能有return语句返回值

当我们**没有显式**的声明类中的构造器时，系统会**默认提供一个无参的构造器**并且该**构造器的修饰 符默认与类的修饰符**相同。

**显式**的定义类的构造器以后，系统就**不再提供默认**的无参的构造器了

在类中，至少会存在**一个**构造器且构造器是**可以重载**的。

## 1.4 封装

把该隐藏的隐藏起来，该暴露的暴露出来

 Java如何实现数据封装：

依赖访问控制修饰符	public 、 protected 、 缺省 、 private 。具体访问范围如下：

![image-20240312152855960](E:\Notes\JavaSE\assets\image-20240312152855960.png)



## 补充： 内存解析 

* stack栈：方法内定义的变量  、 对象引用是对象在堆内存的首地址
* heap堆：new  something()  Java虚拟机规范中的描述是：所有的对象的实例以及数组都要在堆上分配
* Method Area方法区：用于存储已被虚拟机加载的类信息、常量、静态变量

<img src="E:\Notes\JavaSE\assets\image-20240311221842290.png" alt="image-20240311221842290" style="zoom: 50%;" />

**方法调用内存解析：**

* 方法 **没有被调用** 的时候，都在 **方法区** 中的字节码文件(.class)中存储
* 方法 **被调用** 的时候，需要进入到 **栈内存** 中运行。方法每调用一次就会在栈中有一个 **入栈** 动作，即 给当前方法开辟一块独立的内存区域，用于存储当前方法的局部变量的值。
* 当方法执行结束后，会释放该内存，称为 **出栈** ，如果方法有返回值，就会把结果返回调用处，如 果没有返回值，就直接结束，回到调用处继续执行下一条指令。

![image-20240312144639042](E:\Notes\JavaSE\assets\image-20240312144639042.png)

## 补充: JavaBean

<img src="E:\Notes\JavaSE\assets\image-20240312153544103.png" alt="image-20240312153544103" style="zoom:150%;" />

# 2 OOP进阶

##  2.1 this、super

- 在Java中，this关键字指的是本对象的引用。
  - 它在方法（准确的说是实例方法或**非static**的方法）内部使用，表示调用该方法的对象
  - 它在构造器内部使用，表示该构造器正在初始化的对象。 this(参数列表)

- this可以调用的结构：成员变量、方法和构造器

super来调用父类中的指定操作：访问父类中定义的属性、调用父类中定义的成员方法、在子类构造器中调用父类的构造器

**super的使用场景**

- 如果子类没有重写父类的方法，只要权限修饰符允许，在子类中完全可以直接调用父类的方法；
- 如果子类重写了父类的方法，在子类中需要通过`super.`才能调用父类被重写的方法，否则默认调用的子类重写的方法

**<font color='red'>特别说明：应该避免子类声明和父类重名的成员变量</font>**

**子类构造器中调用父类构造器**

① 子类继承父类时，**不会**继承父类的构造器。只能通过“super(形参列表)”的方式调用父类指定的构造器。

② 规定：“super(形参列表)”，必须声明在构造器的首行。

③ 在构造器的首行可以使用"this(形参列表)"，调用本类中重载的构造器，
     结合②，结论：在构造器的首行，"this(形参列表)" 和 "super(形参列表)"只能二选一。

④ 如果在子类构造器的首行既没有显示调用"this(形参列表)"，也没有显式调用"super(形参列表)"，
​     则子类此构造器默认调用"super()"，即调用父类中空参的构造器。

⑤ 由③和④得到结论：子类的任何一个构造器中，要么会调用本类中重载的构造器，要么会调用父类的构造器。
     只能是这两种情况之一。

⑥ 由⑤得到：一个类中声明有n个构造器，最多有n-1个构造器中使用了"this(形参列表)"，则剩下的那个一定使用"super(形参列表)"。

> 开发中常见错误：
>
> 如果子类构造器中既未显式调用父类或本类的构造器，且父类中又没有空参的构造器，则`编译出错`。

## 2.2 继承

- 继承的出现减少了代码冗余，提高了**代码的复用**性。

- 继承的出现，更有利于**功能的扩展**。

- 继承的出现让类与类之间产生了`is-a`的关系，为**多态的使用**提供了前提。
  - 继承描述事物之间的所属关系，这种关系是：`is-a` 的关系。可见，父类更通用、更一般，子类更具体。

> 注意：不要仅为了获取其他类中某个功能而去继承！

通过 **`extends`** 关键字，可以声明一个类B继承另外一个类A，定义格式如下：

```java
[修饰符] class 类A {
	...
}

[修饰符] class 类B extends 类A {
	...
}

```

**继承性的细节说明**

**1、子类会继承父类所有的实例变量和实例方法**

父类是所有子类共同特征的抽象描述。而成员变量和成员方法就是事物的特征，那么父类中声明的成员变量和成员方法代表子类事物也有这个特征

- 当子类对象被创建时，在堆中给对象申请内存时，就要看子类和父类都声明了什么实例变量，这些实例变量都要分配内存。
- 当子类对象调用方法时，编译器**会先在子类模板**中看该类是否有这个方法，如果**没找到**，**会看它的父类甚至超类**是否声明了这个方法，遵循`从下往上`找的顺序，找到了就停止，一直到根父类都没有找到，就会报编译错误。

**2、子类不能直接访问父类中私有的(private)的成员变量和方法**

子类虽会继承父类私有(private)的成员变量，但子类不能对继承的私有成员变量直接进行访问，可通过继承的get/set方法进行访问

## 2.3 重写

**方法重写的要求：**

1. 子类重写的方法`必须`和父类被重写的方法具有相同的`方法名称`、`参数列表`。

2. 子类重写的方法的返回值类型`不能大于`父类被重写的方法的返回值类型。（例如：Student < Person）。

   子类重写方法的返回值 可以和父类返回值类型 相同， 也可以为其子类

> 注意：如果返回值类型是基本数据类型和void，那么必须是相同

3. 子类重写的方法使用的访问权限`不能小于`父类被重写的方法的访问权限。（public > protected > 缺省 > private）

> 注意：① 父类私有方法不能重写   ② 跨包的父类缺省的方法也不能重写

4. 子类方法抛出的异常不能大于父类被重写方法的异常

**也就是说，子类重写父类的方法，返回值类型和抛出异常是越来越小（可以保持不变）的，权限是越来越大的**



**小结：方法的重载与重写**

方法的重载：方法名相同，形参列表不同。不看返回值类型。

## 2.4 子类对象实例化全过程

从过程的角度看：

当用子类构造器实例化时，子类构造器一定会直接或间接调用父类构造器...直到Object类的构造器被调用。

正因为我们调用过子类所有的父类构造器，所以可以将父类中声明的属性和方法加载到内存中，供子类对象使用

```java
Dog dog = new Dog("小花","小红");
```

<img src="E:\Notes\JavaSE\assets\image-20240319210516256.png" alt="image-20240319210516256" style="zoom:80%;" />

<img src="E:\Notes\JavaSE\assets\image-20240319210531549.png" alt="image-20240319210531549" style="zoom:80%;" />

虽然调用了父类的构造器，但是只创建了一个对象（父类并没有被创建），父类的属性和方法在内存中供子类使用。

## 2.5 多态

对象的多种形态，Java中的体现：**父类的引用指向子类的对象**

格式：（父类类型：指子类继承的父类类型，或者实现的接口类型）

```java
	父类类型 变量名 = 子类对象；
```

Java引用变量有两个类型：`编译类型`和`运行类型`。编译时类型由`声明`该变量时使用的类型决定，运行时类型由`实际赋给该变量的对象`决定。简称：**编译时，看左边；运行时，看右边。**

- 若编译时类型和运行时类型不一致，就出现了对象的多态性(Polymorphism)
- 多态情况下，“看左边”：看的是父类的引用（父类中不具备子类特有的方法）
      “看右边”：看的是子类的对象（实际运行的是子类重写父类的方法）
- 多态适用于方法 不适用于属性， 属性就看编译类型

多态的使用前提：① 类的继承关系  ② 方法的重写

### 2.5.1 举例

**方法的形参声明体现多态**

```java
public class Person{
    private Pet pet;
    public void adopt(Pet pet) {//形参是父类类型，实参是子类对象
        this.pet = pet;
    }
    public void feed(){
        pet.eat();//pet实际引用的对象类型不同，执行的eat方法也不同
    }
}
```

```java
public class TestPerson {
    public static void main(String[] args) {
        Person person = new Person();

        Dog dog = new Dog();
        dog.setNickname("小白");
        person.adopt(dog);//实参是dog子类对象，形参是父类Pet类型
        person.feed();

        Cat cat = new Cat();
        cat.setNickname("雪球");
        person.adopt(cat);//实参是cat子类对象，形参是父类Pet类型
        person.feed();
    }
}
```

**方法返回值类型体现多态**

```java
public class PetShop {
    //返回值类型是父类类型，实际返回的是子类对象
    public Pet sale(String type){
        switch (type){
            case "Dog":
                return new Dog();
            case "Cat":
                return new Cat();
        }
        return null;
    }
}
```

```java
public class TestPetShop {
    public static void main(String[] args) {
        PetShop shop = new PetShop();

        Pet dog = shop.sale("Dog");
        dog.setNickname("小白");
        dog.eat();

        Pet cat = shop.sale("Cat");
        cat.setNickname("雪球");
        cat.eat();
    }
}
```

### 2.5.2 为什么需要多态性(polymorphism)

开发中，有时我们在设计一个数组、或一个成员变量、或一个方法的形参、返回值类型时，无法确定它具体的类型，只能确定它是某个系列的类型。

因此将方法的形式参数或者返回值设置为一系列类型的父类，可以提高程序的扩展性。

**好处**：变量引用的子类对象不同，执行的方法就不同，实现**动态绑定**。代码编写更灵活、功能更强大，可维护性和扩展性更好了。

**弊端**：一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属性和方法。

* 开发中：

  **使用父类做方法的形参**，是多态使用最多的场合。即使增加了新的子类，方法也无需改变，提高了扩展性，符合开闭原则。

  【开闭原则OCP】

  - 对扩展开放，对修改关闭
  - 通俗解释：软件系统中的各种组件，如模块（Modules）、类（Classes）以及功能（Functions）等，应该在不修改现有代码的基础上，引入新功能

### 2.5.3 虚方法调用(Virtual Method Invocation)

在Java中虚方法是指在**编译阶段**不能确定方法的调用入口地址，在运行阶段才能确定的方法，即可能被重写的方法

```java
Person e = new Student();
e.getInfo();	//调用Student类的getInfo()方法
```

子类**重写**了父类的方法，在多态情况下，将此时**父类的方法称为虚方法**，父类根据赋给它的不同子类对象，动态调用属于子类的该方法。这样的方法调用在编译期是无法确定的

* 拓展：

  `静态链接（或早起绑定）`：当一个字节码文件被装载进JVM内部时，如果被调用的目标方法在编译期可知，且运行期保持不变时。这种情况下将调用方法的符号引用转换为直接引用的过程称之为静态链接。那么调用这样的方法，就称为非虚方法调用。比如调用静态方法、私有方法、final方法、父类构造器、本类重载构造器等

  `动态链接（或晚期绑定）`：如果被调用的方法在编译期无法被确定下来，也就是说，只能够在程序运行期将调用方法的符号引用转换为直接引用，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。调用这样的方法，就称为虚方法调用。比如调用重写的方法（针对父类）、实现的方法（针对接口）

### 2.5.4 向上转型与向下转型

首先，一个对象在new的时候创建是哪个类型的对象，它从头至尾都不会变。即这个对象的运行时类型，本质的类型用于不会变。但是，把这个对象赋值给不同类型的变量时，这些变量的编译时类型却不同

上 可以理解为： **抽象**程度较高的 ，也就是 父类， 即 **向上转型** 意味着  **父类的引用指向了 子类的类型**

下 可以理解为： 比较**具体**的 ，也即 子类 ，**向下转型** 意味着 父类的引用 通过**强制类型转换 转换为子类类型**

------

**向下转型时**，可能会出现类型转换异常的出现，即ClassCastException，为了避免ClassCastException的发生，Java提供了 `instanceof` 关键字，给引用变量做类型的校验。如下代码格式

```java
//检验对象a是否是数据类型A的对象，返回值为boolean型
对象a instanceof 数据类型A 
```

说明：

- 只要用instanceof判断返回true的，那么强转为该类型就一定是安全的，不会报ClassCastException异常。
- 如果对象a属于类A的子类B，a instanceof A值也为true。
- 要求对象a所属的类与类A必须是子类和父类的关系，否则编译错误。

# 3 OOP高级

## 3.1 static

### 3.1.1 静态变量

**语法格式**

使用static修饰的成员变量就是静态变量（或类变量、类属性）

```Java
[修饰符] class 类{
	[其他修饰符] static 数据类型 变量名;
}
```

 **静态变量的特点**

- 静态变量的默认值规则和实例变量一样。
- 静态变量值是所有对象共享。

- 静态变量在本类中，可以在任意方法、代码块、构造器中直接使用。
- 如果权限修饰符允许，在其他类中可以通过“`类名.静态变量`”直接访问，也可以通过“`对象.静态变量`”的方式访问（但是更推荐使用类名.静态变量的方式）。
- 静态变量的get/set方法也静态的，当局部变量与静态变量`重名时`，使用“`类名.静态变量`”进行区分。

**对应的内存结构**：（以经典的JDK6内存解析为例，此时静态变量存储在方法区）

<img src="E:\Notes\JavaSE\assets\image-20240325015319379.png" alt="image-20240325015319379" style="zoom:80%;" />

JDK8 之后 静态变量转移到堆内存中

### 3.1.2 静态方法

语法格式：

```java
[修饰符] class 类{
	[其他修饰符] static 返回值类型 方法名(形参列表){
        方法体
    }
}
```

特点：

- 静态方法在本类的任意方法、代码块、构造器中都可以直接**被调用**。
- 只要权限修饰符允许，静态方法在其他类中可以通过“类名.静态方法“的方式调用。也可以通过”对象.静态方法“的方式调用（但是更推荐使用**类名.静态方法**的方式）。
- 在**static方法**内部**只能访问类的static修饰的属性或方法**，不能访问类的非static的结构。
- **静态方法**可以被子类继承，但**不能被子类重写**。
- 静态方法的调用都只看编译时类型。
- 因为不需要实例就可以访问static方法，因此static**方法内部不能有this，也不能有super**。如果有重名问题，使用“类名.”进行区别。

### 3.1.3 单例模式

**设计模式概述**

**设计模式**是在大量的`实践中总结`和`理论化`之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模式就像是经典的棋谱，不同的棋局，我们用不同的棋谱。"套路"

**何为单例模式**

采取一定的方法保证在整个的软件系统中，对某个类**只能存在一个对象实例**，并且该类只提供一个取得其对象实例的方法

**实现思路**

首先必须将`类的构造器的访问权限设置为private`，这样，就不能用new操作符在类的外部产生类的对象了，但在类内部仍需要产生该类的对象。因为在类的外部开始还无法得到类的对象，`只能调用该类的某个静态方法`以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的`该类对象的变量也必须定义成静态的`。

**单例模式的两种实现方式**

**饿汉式**

```java
class Singleton {
    // 1.私有化构造器
    private Singleton() {
    }

    // 2.内部提供一个当前类的实例
    // 4.此实例也必须静态化
    private static Singleton single = new Singleton();

    // 3.提供公共的静态的方法，返回当前类的对象
    public static Singleton getInstance() {
        return single;
    }
}
```

**懒汉式**

```java
class Singleton {
    // 1.私有化构造器
    private Singleton() {
    }
    // 2.内部提供一个当前类的实例
    // 4.此实例也必须静态化
    private static Singleton single;
    // 3.提供公共的静态的方法，返回当前类的对象
    public static Singleton getInstance() {
        if(single == null) {
            single = new Singleton();
        }
        return single;
    }
}
```

**饿汉式 vs 懒汉式**

饿汉式：

- 特点：`立即加载`，即在使用类的时候已经将对象创建完毕。
- 优点：实现起来`简单`；**没有多线程安全问题**。
- 缺点：当类被加载的时候，会初始化static的实例，静态变量被创建并分配内存空间，从这以后，这个static的实例便一直占着这块内存，直到类被卸载时，静态变量被摧毁，并释放所占有的内存。因此在某些特定条件下会`耗费内存`。

懒汉式：

- 特点：`延迟加载`，即在调用静态方法时实例才被创建。
- 优点：实现起来比较简单；当类被加载的时候，static的实例未被创建并分配内存空间，当静态方法第一次被调用时，初始化实例变量，并分配内存，因此在某些特定条件下会`节约内存`。
- 缺点：在多线程环境中，这种实现方法是完全错误的，`线程不安全`，根本不能保证单例的唯一性。
  - 说明：在多线程章节，会将懒汉式改造成线程安全的模式。

## 3.2 main()方法理解

* Java程序的入口函数：（格式固定） **public static void main(String[] args){	}**
  JVM需要调用类的main()方法，所以该方法的访问权限必须是public，又因为JVM在执行main()方法时不必创建对象，所以该方法必须是static的，该方法接收一个String类型的数组参数，该数组中保存执行Java命令时传递给所运行的类的参数。
* 一个普通的静态函数 ，权限可以更改为 private，即 private static void main(String[] args)

**命令行参数用法举例**

```Java
public class CommandPara {
    public static void main(String[] args) {
        for (int i = 0; i < args.length; i++) {
            System.out.println("args[" + i + "] = " + args[i]);
        }
    }
}

//cmd运行程序CommandPara.java
java CommandPara "Tom" "Jerry" "Shkstart"


//输出结果
args[0] = Tom
args[1] = Jerry
args[2] = Shkstart
```

**IDEA工具：**

<img src="E:\Notes\JavaSE\assets\image-20240325025902594.png" alt="image-20240325025902594" style="zoom:80%;" />

program argument  即为 main函数的形参传入的地方，，直接输入即可

## 3.3 代码块

如果成员变量想要初始化的值不是一个硬编码的常量值，而是需要通过复杂的计算或读取文件、或读取运行环境信息等方式才能获取的一些值，该怎么办呢？此时，可以考虑代码块（或初始化块）。

- 代码块(或初始化块)的`作用`：
  对Java类或对象的属性（包括静态变量）进行初始化
- 代码块(或初始化块)的`分类`：

  - 一个类中代码块若有修饰符，则只能被static修饰，称为静态代码块(static block)

  - 没有使用static修饰的，为非静态代码块。

### 3.3.1静态代码块

**格式：**

代码块的前面加static，就是静态代码块

```Java
【修饰符】 class 类{
	static{
        静态代码块
    }
}
```

**特点**：

1. 可以有输出语句。

  2. 可以对类的属性、类的声明进行初始化操作。（一般是静态变量进行初始化）

  3. 不可以对**非静态**的属性初始化。即：不可以调用非静态的属性和方法。

  4. 若有多个静态的代码块，那么按照从上到下的顺序依次执行。

  5. 静态代码块的执行要**先于非静态**代码块。

  6. 静态代码块**随着类的加载而加载，且只执行一次**。

### 3.3.2非静态代码块

**格式：**

```java
【修饰符】 class 类{
	{
        非静态代码块
    }
}
```

**特点：**

1. 可以有输出语句。

  2. 可以对类的属性、类的声明进行初始化操作。  一般是类的**成员变量**

  3. 除了调用非静态的结构外，还可以调用静态的变量或方法。

  4. 若有多个非静态的代码块，那么按照从上到下的顺序依次执行。

  5. 每次**创建对象的时候，都会执行一次**。且**先于构造器执行**

### 3.3.3 实例变量赋值顺序

<img src="E:\Notes\JavaSE\assets\image-20240325033639019.png" alt="image-20240325033639019" style="zoom:80%;" />

## 3.4 final

### 3.4.1 final修饰类

表示这个**类不能被继承**，没有子类。提高安全性，提高程序的可读性

例如：String类、System类、StringBuffer类

### 3.4.2 final修饰方法

表示这个方法**不能被子类重写**

### 3.4.3 final修饰变量

final修饰某个变量（成员变量或局部变量），一旦赋值，它的值就不能被修改，即常量，常量名建议使用大写字母

如果某个成员变量用final修饰后，没有set方法，并且必须初始化（可以显式赋值、或在初始化块赋值、实例变量还可以在构造器中赋值）

## 3.5 抽象类与抽象方法 

Java语法规定，包含抽象方法的类必须是**抽象类**。

**语法格式**：

* **抽象类**：被abstract修饰的类。
* **抽象方法**：被abstract修饰没有方法体的方法。

抽象类的语法格式

```java
[权限修饰符] abstract class 类名{
    
}
[权限修饰符] abstract class 类名 extends 父类{
    
}
```

抽象方法的语法格式

```java
[其他修饰符] abstract 返回值类型 方法名([形参列表]);
```

* 注意：抽象方法没有方法体

<img src="E:\Notes\JavaSE\assets\image-20240326215418900.png" alt="image-20240326215418900" style="zoom:150%;" />

### 3.5.1 使用说明

1. 抽象类**不能创建对象**，如果创建，编译无法通过而报错。只能创建其非抽象子类的对象。

   > 理解：假设创建了抽象类的对象，调用抽象的方法，而抽象方法没有具体的方法体，没有意义。
   >
   > 抽象类是用来被继承的，抽象类的子类必须重写父类的抽象方法，并提供方法体。若没有重写全部的抽象方法，仍为抽象类。

2. 抽象类中，也有构造方法，是供子类创建对象时，初始化父类成员变量使用的。

   > 理解：子类的构造方法中，有默认的super()或手动的super(实参列表)，需要访问父类构造方法。

3. 抽象类中，不一定包含抽象方法，但是有抽象方法的类必定是抽象类。

   > 理解：未包含抽象方法的抽象类，目的就是不想让调用者创建该类对象，通常用于某些特殊的类结构设计。

4. 抽象类的子类，必须重写抽象父类中**所有的**抽象方法，否则，编译无法通过而报错。除非该子类也是抽象类。 

   > 理解：假设不重写所有抽象方法，则类中可能包含抽象方法。那么创建对象后，调用抽象的方法，没有意义。

### 3.5.2 模板方法设计模式(TemplateMethod)

略 设计模式学习

## 3.6 接口 interface 

接口的定义，它与定义类方式相似，但是使用 `interface` 关键字。它也会被编译成.class文件，但一定要明确它并不是类，而是另外一种引用数据类型。

> 引用数据类型：数组，类，枚举，接口，注解。

**接口的声明格式**：

```java
[修饰符] interface 接口名{
    //接口的成员列表：
    // 公共的静态常量  修饰必须用 public static final 修饰 （可以省略，默认修饰）
    public static final int MAX_COUNT;
    
    // 公共的抽象方法
    public abstract  //也可以省略 ，默认用此修饰
    
    // 公共的默认方法（JDK1.8以上）
    // 公共的静态方法（JDK1.8以上）
    // 私有方法（JDK1.9以上）
}
```

------

**类** 可以看作是对象的封装，而 **接口**可以看作是 **功能**的封装

------

### 3.6.1 接口的成员说明

**在JDK8.0 之前**，接口中只允许出现：

（1）公共的静态的常量：其中`public static final`可以省略

（2）公共的抽象的方法：其中`public abstract`可以省略

> 理解：接口是从多个相似类中抽象出来的规范，不需要提供具体实现

**在JDK8.0 时**，接口中允许声明`默认方法`和`静态方法`：

（3）公共的默认的方法：其中public 可以省略，建议保留，但是default不能省略

（4）公共的静态的方法：其中public 可以省略，建议保留，但是static不能省略

**在JDK9.0 时**，接口又增加了：

（5）私有方法

除此之外，接口中没有构造器，没有初始化块，因为接口中没有成员变量需要动态初始化。

### 3.6.2 使用规则

**1、类实现接口（implements）**

接口**不能创建对象**，但是可以被类实现（`implements` ，类似于被继承）。

类与接口的关系为实现关系，即**类实现接口**，该类可以称为**接口的实现类**。实现的动作类似继承。

```java
【修饰符】 class 实现类  implements 接口{
	// 重写接口中抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
  	// 重写接口中默认方法【可选】
}

【修饰符】 class 实现类 extends 父类 implements 接口{
    // 重写接口中抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
  	// 重写接口中默认方法【可选】
}
```

**2、接口的多实现（implements）**

一个类只能继承一个父类，对于接口而言，一个类是可以实现多个接口的。

**实现格式：**

```Java
【修饰符】 class 实现类  implements 接口1，接口2，接口3。。。{
	// 重写接口中所有抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
  	// 重写接口中默认方法【可选】
}

【修饰符】 class 实现类 extends 父类 implements 接口1，接口2，接口3。。。{
    // 重写接口中所有抽象方法【必须】，当然如果实现类是抽象类，那么可以不重写
  	// 重写接口中默认方法【可选】
}
```

**3、接口的多继承(extends)**

一个接口能继承另一个或者多个接口，接口的继承也使用 `extends` 关键字，子接口继承父接口的方法。

**4、接口与实现类对象构成多态引用**

接口名 变量名 = new 实现类对象 ; 

实现类实现接口，类似于子类继承父类，因此，接口类型的变量与实现类的对象之间，也可以构成多态引用。通过接口类型的变量调用方法，最终执行的是你new的实现类对象实现的方法体。

### 3.6.3 接口的匿名实现类的匿名对象

666，找找资料，意会写不下来

## 3.7 内部类inner

**定义**：

将一个类A定义在另一个类B里面，里面的那个类A就称为`内部类（InnerClass）`，类B则称为`外部类（OuterClass）`。

**分类**：

根据内部类声明的位置（如同变量的分类），我们可以分为：

<img src="E:\Notes\JavaSE\assets\image-20240327170751916.png" alt="image-20240327170751916"  />

------

**成员内部类是重点**

------

### 3.7.1 成员内部类

语法格式：

```java
[修饰符] class 外部类{
    [其他修饰符] [static] class 内部类{
    }
}
```

**成员内部类的使用特征，概括来讲有如下两种角色：**

- 成员内部类作为`类的成员的角色`：
  - 和外部类不同，Inner class还可以声明为private或protected；
  - 可以调用外部类的结构。（注意：在静态内部类中不能使用外部类的非静态成员）
  - Inner class 可以声明为static的，但此时就不能再使用外层类的非static的成员变量；
- 成员内部类作为`类的角色`：
  - 可以在内部定义属性、方法、构造器等结构
  - 可以继承自己的想要继承的父类，实现自己想要实现的父接口们，和外部类的父类和父接口无关
  - 可以声明为abstract类 ，因此可以被其它的内部类继承
  - 可以声明为final的，表示不能被继承
  - 编译以后生成OuterClass$InnerClass.class字节码文件（也适用于局部内部类）

注意点：

- 外部类访问成员内部类的成员，需要“内部类.成员”或“内部类对象.成员”的方式
- 成员内部类可以直接使用外部类的所有成员，包括私有的数据
- 当想要在外部类的静态成员部分使用内部类时，可以考虑内部类声明为静态的

**创建成员内部类：**

- 实例化静态内部类

  ```java
  // 类型   			  =  new 构造器（）;
  外部类名.静态内部类名 变量 =  new 外部类名.静态内部类名();
  变量.非静态方法();
  ```

- 实例化非静态内部类

  ```java
  外部类名 变量1 = new 外部类();
  外部类名.非静态内部类名 变量2 = 变量1.new 非静态内部类名();
  变量2.非静态方法();
  ```

  注意 先创建外部类， **外部类变量.new 内部类构造器()**；

```Java
public class TestMemberInnerClass {
    public static void main(String[] args) {
        //创建 静态成员内部类
        Outer.InnerStatic innerStatic = new Outer.InnerStatic();
        //分两步 创建非静态成员内部类
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        //成员内部类调用不同位置的参数
        inner.inMethod(100);
    }
}

class Outer{
    private static int a = 1;
     class Inner {
        private int a = 10;
        public void inMethod(int a) {
            System.out.println("param-a = " +a);
            System.out.println("Inner.a = " + this.a);
            System.out.println("Outer.a = " + Outer.a);
        }
    }
     static class InnerStatic{
         
    }

}
```

<img src="E:\Notes\JavaSE\assets\image-20240327173213689.png" alt="image-20240327173213689" style="zoom:80%;" />

**内部类使用外部类成员**：

<img src="E:\Notes\JavaSE\assets\image-20240327172541194.png" alt="image-20240327172541194" style="zoom: 80%;" />

this.变量 表示内部类自己的成员变量   外部类.变量 调用外部类的变量   什么都不加调用形参变量

**结果同上**

### 3.7.2 局部内部类

用的不多 有接口的那四种实现方式

- 接口实现类的对象

- 接口实现类的匿名对象

- 接口匿名实现类的对象 

- 接口匿名实现类的匿名对象   **return  new 接口() { 抽象方法实现 }**    
  （ 用 **接口()** ，来代替 **实现类()** ,叫**接口匿名实现类**  ）   哈哈哈 还有一种写法 是  **new 继承父类() { 抽象方法实现 }**
  
  本质上 和 接口是一样的

 								**能看懂不被吓到就行**

## 3.8 枚举类 enum

枚举类型本质上也是一种类，只不过是这个类的对象是有限的、固定的几个

**enum 关键字声明枚举**：

```java
【修饰符】 enum 枚举类名{
    常量对象列表
}

【修饰符】 enum 枚举类名{
    常量对象列表;
    
    对象的实例变量列表;
}
```

常量对象之间 用逗号 ， 隔开

**举例**：

```java
//声明一个枚举类seasonEnum
public enum SeasonEnum {
    //四个对象 其实省略了 public static final 三个修饰符
    // 还省略了 SPRING = new SeasonEnum(" "," ") 构造器的使用
    //所以 可以简略这样写，（）中为属性其赋值
    SPRING("春天","春风又绿江南岸"),
    SUMMER("夏天","映日荷花别样红"),
    AUTUMN("秋天","秋水共长天一色"),
    WINTER("冬天","窗含西岭千秋雪");
    private final String seasonName;
    private final String seasonDesc;
    private SeasonEnum(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }
}
```

```java
public class SeasonEnumTest {
    public static void main(String[] args) {
        System.out.println(SeasonEnum.SPRING);
    }
}
```

在测试类中进行测试，注意SeasonEnum.SPRING打印的就是其对象的变量名 ，即SPRING.

![image-20240327194947519](E:\Notes\JavaSE\assets\image-20240327194947519.png)

进而，将枚举类的成员变量可一省略（没意义的话），这样 构造器也可以省略（JDK默认提供一个空参的构造器）

这样就简写为:

```JAVA
public enum Season {
    SPRING,
    SUMMER,
    AUTUMN,
    WINTER;
}
```

```java
public class SeasonTest {
    public static void main(String[] args) {
        System.out.println(Season.AUTUMN);
    }
}
```

结果是一样的，

<img src="E:\Notes\JavaSE\assets\image-20240327195332137.png" alt="image-20240327195332137" style="zoom:150%;" />

最后一种写法是最为常见的 666

## 3.9 注解 annotation

注解（Annotation）是从`JDK5.0`开始引入，以“`@注解名`”在代码中存在。例如：

- @override 表示对父类方法的重写
- @deprecated 表示方法、属性过时，不建议使用
- @suppressWarnings 抑制警告

Annotation 可以像修饰符一样被使用，可用于**修饰包、类、构造器、方法、成员变量、参数、局部变量的声明**。还可以添加一些参数值，这些信息被保存在 Annotation 的 “name=value” 对中

### 3.9.1 元注解

JDK1.5在java.lang.annotation包定义了4个标准的meta-annotation类型，它们被用来提供对其它 annotation类型作说明

（1）**@Target：**用于描述注解的使用范围

* 可以通过枚举类型ElementType的10个常量对象来指定
* TYPE，METHOD，CONSTRUCTOR，PACKAGE.....

指的是 target描述的 **注解可以用在哪里**

（2）**@Retention：**用于描述注解的生命周期

* 可以通过枚举类型RetentionPolicy的3个常量对象来指定
* SOURCE（源代码）、CLASS（字节码）、RUNTIME（运行时）
* `唯有RUNTIME阶段才能被反射读取到`。

（3）**@Documented**：表明这个注解应该被 javadoc工具记录。

（4）**@Inherited：**允许子类继承父类中的注解

前两个需要记忆一下，后两个先不必了解

### 3.9.2 自定义注解

一个完整的注解应该包含三个部分：
（1）声明
（2）使用
（3）读取

没啥意义，随后用到再学习~~~~

## 3.10 单元测试

要使用JUnit，必须在项目的编译路径中`引入JUnit的库`，即相关的.class文件组成的jar包。jar就是一个压缩包，压缩包都是开发好的第三方（Oracle公司第一方，我们自己第二方，其他都是第三方）工具类，都是以class文件形式存在的。

### 3.10.1 引入本地 JUnit

<img src="E:\Notes\JavaSE\assets\image-20240327204037214.png" alt="image-20240327204037214" style="zoom:80%;" />

------

在 项目结构**project structure** 中  左侧的 **libraries** 中 点击  ➕， 选择 junit所在的库 点击 Apply 

![image-20240327204930323](E:\Notes\JavaSE\assets\image-20240327204930323.png)

此时可以看到除了JDK17之外，自己的junit 的；两个jar包已经导入到项目中

------

此时 junit单元测试框架在 整个项目中 但是还没有应用到具体的module ，

<img src="E:\Notes\JavaSE\assets\image-20240327204431611.png" alt="image-20240327204431611" style="zoom:80%;" />

选中具体的module后  在右侧的dependencies中 点击 ➕  后选择 library  。 第一个是从本地导入jar包，第二个是从项目中选择库，第三个是从其他模块选择依赖

由于已经将junit的jar包导入到项目中了，选第二个即可。

<img src="E:\Notes\JavaSE\assets\image-20240327204653416.png" alt="image-20240327204653416" style="zoom:80%;" />

选好后 记得修改为compile **编译时生效**， 之后就可以用@Test注解 进行单元测试了

**注意Scope：选择Compile，否则编译时，无法使用JUnit。**

最后：

其他module想要使用junit按下图所示即可

![image-20240327205450330](E:\Notes\JavaSE\assets\image-20240327205450330.png)

之后同上，记得scope中 选择 compile

### 3.10.2 @Test 要求

JUnit4版本，要求@Test标记的方法必须满足如下要求：

- 所在的类必须是public的，非抽象的，包含唯一的无参构造器。
- @Test标记的方法本身必须是public，非抽象的，非静态的，void无返回值，()无参数的。

所以 单元测试类 最好是创建后（创建默认为public的）、 不写构造器

![image-20240327210045847](E:\Notes\JavaSE\assets\image-20240327210045847.png)

### 3.10.3 设置执行JUnit用例时支持控制台输入 Scanner

**1. 设置数据：**

默认情况下，在单元测试方法中使用Scanner时，并不能实现控制台数据的输入。需要做如下设置：

在`idea64.exe.vmoptions配置文件`中加入下面一行设置，重启idea后生效。

```properties
-Deditable.java.test.console=true
```

**2.文件位置：**

<img src="E:\Notes\JavaSE\assets\image-20240327210412920.png" alt="image-20240327210412920" style="zoom: 67%;" />

![image-20240327210535202](E:\Notes\JavaSE\assets\image-20240327210535202.png)

添加完成之后，重启IDEA即可

### 3.10.4 自定义@Test模板测试方法

![image-20240327211234427](E:\Notes\JavaSE\assets\image-20240327211234427.png)

在templates中 其实可以看到Java中已经有了几个常见的模板快捷键  fori这种  

我们先创建一个自定义组，UserDefine  用户自定义组

<img src="E:\Notes\JavaSE\assets\image-20240327211430758.png" alt="image-20240327211430758" style="zoom: 67%;" />

点击➕，后  选择模板组，输入名字 UserDefine 点击确定

![image-20240327212043307](E:\Notes\JavaSE\assets\image-20240327212043307.png)

此时模板已经写好了，注意 模板方法上边也要写 @Test  （这里截图截到）

即，**代码区写的内容**是：

```Java
@Test
public void test$var1$(){
	$END$
}
```

在单元测试类中，写一个test

![image-20240327212140677](E:\Notes\JavaSE\assets\image-20240327212140677.png)

已经有模板了，按下回车就生成模板测试方法了（效率+++）

![image-20240327212342938](E:\Notes\JavaSE\assets\image-20240327212342938.png)