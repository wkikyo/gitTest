# 1 泛型- 概述

JDK 1.5设计了泛型的概念，泛型， 即为**类型参数** ，可以声明在类、接口、方法中，代表未知的参数类型

**举例1：**

集合类在声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以**在JDK5.0之前只能把元素类型设计为Object，JDK5.0时Java引入了“参数化类型（Parameterized type）”的概念，允许我们在创建集合时指定集合元素的类型**。比如：`List<String>`，这表明该List只能保存字符串类型的对象

**举例2：**

`java.lang.Comparable`接口和`java.util.Comparator`接口，是用于比较对象大小的接口。这两个接口只是限定了当一个对象大于另一个对象时返回正整数，小于返回负整数，等于返回0，但是并不确定是什么类型的对象比较大小。JDK5.0之前只能用Object类型表示，使用时既麻烦又不安全，因此 JDK5.0 给它们增加了泛型

![image-20240410174012111](E:\Notes\JavaSE\assets\image-20240410174012111.png)

![image-20240410174017194](E:\Notes\JavaSE\assets\image-20240410174017194.png)

其中`<T>`就是类型参数，即泛型

> 所谓泛型，就是允许在定义类、接口时通过一个`标识`表示类中某个`属性的类型`或者是某个方法的`返回值或参数的类型`。这个类型参数将在使用时（例如，继承或实现这个接口、创建对象或调用方法时）确定（即传入实际的类型参数，也称为类型实参）

# 2 JDK-泛型使用

## 2.1 集合中使用泛型

**集合中没有使用泛型时：**

<img src="E:\Notes\JavaSE\assets\image-20240410174527387.png" alt="image-20240410174527387" style="zoom:80%;" />

**集合中使用泛型时：**

<img src="E:\Notes\JavaSE\assets\image-20240410174540059.png" alt="image-20240410174540059" style="zoom:80%;" />

>Java泛型可以保证如果程序在**编译**时**没有发出警告**，**运行**时就**不会**产生ClassCastException异常。即，把不安全的因素在编译期间就排除了，而不是运行期；既然通过了编译，那么类型一定是符合要求的，就避免了类型转换。
>
>同时，代码更加简洁、健壮。
>
>**把一个集合中的内容限制为一个特定的数据类型，这就是generic背后的核心思想**

```java
@Test
public void test1(){
    ArrayList<Integer> integers = new ArrayList<>();
    integers.add(11);//自动装箱
    integers.add(22);
    integers.add(33);
    integers.add(44);
    integers.add(55);
    Iterator<Integer> iterator = integers.iterator();
    while (iterator.hasNext()){
        Integer i = iterator.next();//无需强制类型转换
        System.out.println(i);
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240410175040095.png" alt="image-20240410175040095" style="zoom:80%;" />

```java
@Test
public void test2(){
    HashMap<String, Integer> map = new HashMap<>();
    map.put("wkikyo",99);
    map.put("rose",11);
    map.put("jack",12);
    map.put("jerry",88);
    //遍历keySet
    System.out.println("keySet----------------------------");
    for (String tmp:map.keySet()) {
        System.out.println(tmp);
    }
    //遍历value集合
    System.out.println("values----------------------------");
    for (Integer i : map.values()) {
        System.out.println(i);
    }
    //遍历entry集合
    System.out.println("entry----------------------------");
    //Map.Entry是map中的一个内部接口，而entrySet()的返回值是一个Set<Map.Entry>类型，
    //而Map.Entry<K,V>有含有泛型，是上文map定义时传入的类型参数，即<String,Integer>
    //所以返回值类型就是Set<Map.Entry<String,Integer>>
    Set<Map.Entry<String, Integer>> entries = map.entrySet();
    //迭代器
    Iterator<Map.Entry<String, Integer>> iterator = entries.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240410180256835.png" alt="image-20240410180256835" style="zoom:67%;" />

## 2.2 比较器中使用泛型

```java
public class Circle{
    private double radius;

    public Circle(double radius) {
        super();
        this.radius = radius;
    }

    public double getRadius() {
        return radius;
    }

    public void setRadius(double radius) {
        this.radius = radius;
    }

    @Override
    public String toString() {
        return "Circle [radius=" + radius + "]";
    }
//写一个需要进行比较的对象circle类
}
```

在比较器中使用泛型：

```java
@Test
public void test3(){
    Comparator<Circle> comparator = new Comparator<>() {
        @Override
        public int compare(Circle o1, Circle o2) {
            //无需强制类型转换，泛型已经确定了类型参数
            return Double.compare(o1.getRadius(),o2.getRadius());
        }
    };
    int compare = comparator.compare(new Circle(1), new Circle(2));
    System.out.println(compare);
}
```

无需像之前一样进行类型的instance判断 顶多输入的o1或o2进行 null的判断

![image-20240410194215585](E:\Notes\JavaSE\assets\image-20240410194215585.png)

## 2.3 使用说明

- 在创建集合对象的时候，可以指明泛型的类型。

  具体格式为：List<Integer> list = new ArrayList<Integer>();

- JDK7.0时，有新特性，可以简写为：

  List<Integer> list = new ArrayList<>(); **//类型推断**

- 泛型，也称为泛型参数，即参数的类型，**只能**使用**引用数据类型**进行赋值。（不能使用基本数据类型，可以使用包装类替换）

- 集合声明时，声明泛型参数。在使用集合时，可以具体指明泛型的类型。一旦指明，类或接口内部，凡是使用泛型参数的位置，都指定为具体的参数类型。如果**没有指明的话，看做是Object类型**

# 3 自定义泛型

## 3.1 泛型的基础说明

**1、<类型>这种语法形式就叫泛型。**

- <类型>的形式我们称为类型参数，这里的"类型"习惯上使用T表示，是Type的缩写。即：<T>。
- <T>：代表未知的数据类型，我们可以指定为<String>，<Integer>，<Circle>等。
  - 类比方法的参数的概念，我们把<T>，称为类型形参，将<Circle>称为类型实参，有助于我们理解泛型

- 这里的T，可以替换成K，V等任意字母

**2、在哪里可以声明类型变量\<T>**

- 声明类或接口时，在**类名或接口名后面**声明泛型类型，我们把这样的类或接口称为`泛型类`或`泛型接口`

```java
【修饰符】 class 类名<类型变量列表> 【extends 父类】 【implements 接口们】{
    
}
【修饰符】 interface 接口名<类型变量列表> 【implements 接口们】{
    
}

//例如：
public class ArrayList<E>    
public interface Map<K,V>{
    ....
}    
```

- 声明方法时，在**【修饰符】与返回值类型之间**声明类型变量，我们把声明了类型变量的方法，称为泛型方法

```java
[修饰符] <类型变量列表> 返回值类型 方法名([形参列表])[throws 异常列表]{
    //...
}

//例如：java.util.Arrays类中的
public static <T> List<T> asList(T... a){
    ....
}
```

## 3.2 自定义泛型类或泛型接口

当我们在类或接口中定义某个成员时，该**成员的相关类型是不确定**的，而这个类型需要在使用这个类或接口时才可以确定，那么我们可以使用泛型类、泛型接口

**1.说明**

① 我们在声明完自定义泛型类以后，可以在类的内部（比如：属性、方法、构造器中）使用类的泛型。

② 我们在创建自定义泛型类的对象时，可以指明泛型参数类型。一旦指明，内部凡是使用类的泛型参数的位置，都具体化为指定的类的泛型类型。

③ 如果在创建自定义泛型类的对象时，没有指明泛型参数类型，那么泛型将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。

- 经验：泛型要使用一路都用。要不用，一路都不要用。

④ 泛型的指定中必须使用引用数据类型。不能使用基本数据类型，此时只能使用包装类替换。

⑤ 除创建泛型类对象外，子类继承泛型类时、实现类实现泛型接口时，也可以确定泛型结构中的泛型参数。

如果我们在给泛型类提供子类时，子类也不确定泛型的类型，则可以继续使用泛型参数。

我们还可以在现有的父类的泛型参数的基础上，新增泛型参数。

**2.注意**

① 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如：<E1,E2,E3>

② JDK7.0 开始，泛型的简化操作：ArrayList<Fruit> flist = new ArrayList<>();

③ 如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。

④ 不能使用new E[]。但是可以：E[] elements = (E[])new Object[capacity];

​        参考：ArrayList源码中声明：Object[] elementData，而非泛型参数类型数组。

⑤ 在类/接口上声明的泛型，在本类或本接口中即代表某种类型，但**不可以在静态方法中使用类的泛型**。可以使用方法的泛型

⑥ 异常类不能是带泛型的。

## 3.3 自定义泛型方法

定义类、接口时没有使用<泛型参数>，但是某个方法形参类型不确定时，这个**方法可以单独定义**<泛型参数>。

#### 3.3.1 说明

- 泛型方法的格式：

```java
[访问权限]  <泛型>  返回值类型  方法名([泛型标识 参数名称])  [抛出的异常]{
    
}
```

- **方法，也可以被泛型化，与其所在的类是否是泛型类没有关系**。
- 泛型方法中的**泛型参数在方法被调用时确定**。
- 泛型方法可以根据需要，声明为static的。

例如：

```java
public class DAO {

    public <E> E get(int id, E e) {

        E result = null;

        return result;
    }
}
```

# 4 通配符

**泛型在继承上的体现**：

如果B是A的一个子类型（子类或者子接口），而G是具有泛型声明的类或接口，G<B>并不是G<A>的子类型！

比如：String是Object的子类，但是List<String>并不是List<Object>的子类

<img src="E:\Notes\JavaSE\assets\image-20240412145242435.png" alt="image-20240412145242435" style="zoom:80%;" />

在一个方法中，例如 public void method(Object obj) 这个方法中

```
既可以传入一个Object类型的数据，也可以传入一个子类，例如string类型的数据（obj = new String() ， 多态在继承上的体现）
```

但是 在public void method(List<Object> obj) 这个方法中就不可以传入List<String> 这个类型的数据，因为这两个类没有关系

**通配符**：

为了解决这种不通用的问题，引入了通配符 	‘？’  一个❓，作为通配符。

在这样的方法中，public void method(List<?> list) 即可以传入List<Object> 这个类型的数据，可以传入List<String> 这个类型的数据

一个<?>，指定了在方法的List类型范围是（-∞，+∞）

即：  `List<?>`是`List<String>`、`List<Object>`等各种**泛型List的父类**

## 4.1 通配符的读与写

**写操作：**

将任意元素加入到其中**不是类型安全**的：

```java
Collection<?> c = new ArrayList<>();

c.add(new Object()); // 编译时错误
//add的是obj，有可能出现子类引用指向父类的情况，这是编译器不允许的
```

因为我们不知道c的元素类型，我们不能向其中添加对象。add方法有类型参数E作为集合的元素类型。我们传给add的任何参数都必须是一个未知类型的子类。因为我们不知道那是什么类型，所以我们无法传任何东西进去。

唯一可以插入的元素是null，因为它是所有引用类型的默认值

**读操作：**

另一方面，读取List<?>的对象list中的元素时，永远是安全的，因为不管 list 的真实类型是什么，它包含的都是Object

```java
public static void main(String[] args) {
    List<?> list = null;
    list = new ArrayList<String>();
    list = new ArrayList<Double>();
    // list.add(3);//编译不通过
    list.add(null);

    List<String> l1 = new ArrayList<String>();
    List<Integer> l2 = new ArrayList<Integer>();
    l1.add("wkikyo");
    l2.add(15);
    read(l1);
    read(l2);
}

public static void read(List<?> list) {
    for (Object o : list) {
        System.out.println(o);
    }
}
```

## 4.2 通配符使用注意点

注意点1：编译错误：**不能用在泛型方法声明**上，返回值类型前面的<>中不能使用?

```java
public static <?> void test(ArrayList<?> list){}
//错误写法
```

注意点2：编译错误：不能用在泛型类的声明上

```java
class GenericTypeClass<?>{}
//错误写法
```

## 4.3 有限制条件的通配符

- `<?>`

  - 允许所有泛型的引用调用

- 通配符指定上限：`<? extends 类/接口 >`

  - 使用时指定的类型必须是继承某个类，或者实现某个接口，即<= 

- 通配符指定下限：`<? super 类/接口 >`

  - 使用时指定的类型必须是操作的类或接口，或者是操作的类的父类或接口的父接口，即>=

```java
<? extends Number>     //(-∞ , Number]
//只允许泛型为Number及Number子类的引用调用

<? super Number>      //[Number , +∞)
//只允许泛型为Number及Number父类的引用调用

<? extends Comparable>
//只允许泛型为实现Comparable接口的实现类的引用调用
```

