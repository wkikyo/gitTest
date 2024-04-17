# 1 Java集合框架体系

Java集合可分为Collection和Map 两大体现：

- Collection接口：用于存储一个一个的数据，也称 **单列数据集合** 
  - List子接口：用来存储有序的、可重复数据（"动态"数组）
    - 实现类：Arraylist（主要）、LinkedList、Vector
  - Set子接口：存储 无序、不可重复数据（高中的集合）
    - 实现类：HashSet（主要）、LinkedHashSet、TreeSet
- Map接口：存储具有映射关系的**Key-Vaule对**的数据，称双列数据集合
  - 实现类：HashMap（主要）、LinkedHashMap、TreeMap、Hashtable、Properties
- 集合API 位于Java.util包下

> Collection接口继承树

<img src="E:\Notes\JavaSE\assets\image-20240407173305827.png" alt="image-20240407173305827" style="zoom:80%;" />

> Map接口继承树

<img src="E:\Notes\JavaSE\assets\image-20240407173326809.png" alt="image-20240407173326809" style="zoom:80%;" />

# 2 Collection接口及方法

- JDK不提供此接口的任何直接实现，而是提供更具体的子接口（如：Set和List）去实现。
- Collection 接口是 List和Set接口的父接口，该接口里定义的方法既可用于操作 Set 集合，也可用于操作 List 集合。方法如下：

## 2.1 添加/删除

> add(E obj)：添加元素对象到当前集合中
>
> addAll(Collection other)：添加other集合中的所有元素对象到当前集合中，即this = this ∪ other (相当于并集)



```java
@Test
public void test1(){
    //ArrayList 是 collection接口下属子接口的实现类，体现多态思想
    Collection coll = new ArrayList();
    //自动装箱为Integer类型，
    coll.add(1);
    coll.add(2);
    coll.add("wkikyo");
    coll.add("红楼梦");
    //查看集合元素，相当于调用集合的toString方法
    System.out.println("coll:"+coll);
    System.out.println("coll-size:"+coll.size());
    Collection coll1  = new ArrayList();
    coll1.add(1);
    coll1.add(2);
    System.out.println("coll1:"+coll1);
    System.out.println("coll1-size:"+coll1.size());
    //addAll(Collection coll)方法调用 ，一个集合 合并另外一个集合
    coll.addAll(coll1);
    System.out.println("coll.addAll(coll1):"+coll);
    System.out.println("after-addAll-size:"+coll.size());
    //这样 ，add(Object obj)  参数中放一个集合，这样这个集合作为一个整体，为1个元素
    coll.add(coll1);
    System.out.println("coll.add(coll1):"+coll);
    System.out.println("after-add-size:"+coll.size());
}
```

<img src="E:\Notes\JavaSE\assets\image-20240407204429268.png" alt="image-20240407204429268" style="zoom:67%;" />

> 注意：coll.add(coll1) 和 coll.addAll(coll1) 的区别

<img src="E:\Notes\JavaSE\assets\image-20240407204520312.png" alt="image-20240407204520312" style="zoom: 67%;" />

> void clear()：清空集合元素，size重置为0，内容全部为null
>
> boolean remove(Object obj)：从当前集合中**删除第一个**找到的与obj对象equals返回true的元素。
>
> boolean removeAll(Collection coll)：当前集合删除所有与coll相同的元素。this = this - this ∩ coll，相当于求两集合的差集
>
> boolean retainAll(Collection coll)：当前集合删除两个集合中不同的元素，使得当前集合仅保留与coll集合中的元素相同的元素，即this  = this ∩ coll；相当于求两个集合的交集

```java
public void test3(){
        Collection coll = new ArrayList();
        coll.add("lily");
        coll.add("jack");
        coll.add("rose");
        System.out.println(coll);
        coll.remove("rose");
        System.out.println("after remove \"rose\", the coll:-->"+coll);
        coll.clear();
        System.out.println("after clear,the coll:--->"+coll);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240407211905954.png" alt="image-20240407211905954" style="zoom:80%;" />

```java
@Test
    public void test4(){
        Collection coll = new ArrayList();
        coll.add("lily");
        coll.add("jack");
        coll.add("rose");
        System.out.println("coll = "+coll);
        Collection other = new ArrayList();
        other.add("rose");
        other.add("jack");
        other.add("juice");
        System.out.println("other = "+other);
        coll.removeAll(other);
        System.out.println("coll.removeAll(other)之后，coll = " + coll);
        System.out.println("coll.removeAll(other)之后，other = " + other);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240407212242197.png" alt="image-20240407212242197" style="zoom:80%;" />

> A.removeAll(B)， A=A-B ,B 不发生变化

```java
@Test
    public void test5(){
        Collection coll = new ArrayList();
        coll.add("lily");
        coll.add("jack");
        coll.add("rose");
        System.out.println("coll = "+coll);
        Collection other = new ArrayList();
        other.add("rose");
        other.add("jack");
        other.add("juice");
        System.out.println("other = "+other);
        coll.retainAll(other);
        System.out.println("coll.retainAll(other)之后，coll = " + coll);
        System.out.println("coll.retainAll(other)之后，other = " + other);
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240407212508678.png" alt="image-20240407212508678" style="zoom:80%;" />

> A = A ∩ B , B 不发生变化

## 2.2 判断

> int size() ：获取当前集合中实际存储的元素个数
>
> boolean isEmpty()：判断当前集合是否为空集合
>
> boolean contains(Object obj)：是否包含obj元素，**使用的是obj的equals方法判断（遍历集合）**
>
> boolean containsAll(Colletion coll)：判断coll集合中的元素是否都在当前集合中，是否coll为子集
>
> boolean equals(Object obj)：判断当前集合是否和obj相等

```java
//写了一个类
class PostGraduate {
    String name;
    int age;

    public PostGraduate() {
    }
    public PostGraduate(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "PostGraduate{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    @Override  // 重写equals 和toString方法
    public boolean equals(Object o) {
        System.out.println("postGraduate equals...");		//一旦调用equals方法 就先输出这条语句
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        PostGraduate that = (PostGraduate) o;
        return age == that.age && Objects.equals(name, that.name);
    }

}
@Test
public void test2(){
    Collection coll = new ArrayList();
    System.out.println("before add-the size of collection:--->"+coll.size());
    coll.add("lily");
    coll.add("jack");
    coll.add("rose");
    coll.add(new PostGraduate("kikyo",18));
    System.out.println("after add-the size of collection:--->"+coll.size());
    System.out.println(coll);
    //contains 下面是通过比较equals方法 的返回值，比较的是内容
    System.out.println("coll contains the obj-kikyo(a new obj)? --->"+coll.contains(new PostGraduate("kikyo", 18)));
    //另外一个集合
    Collection coll1 = new ArrayList();
    coll1.add("lily");
    coll1.add(new PostGraduate("kikyo",18));
    //containsAll(Collection coll)
    System.out.println("coll containsAll coll1 ?--->"+coll.containsAll(coll1));
}
```

<img src="E:\Notes\JavaSE\assets\image-20240407210817987.png" alt="image-20240407210817987" style="zoom: 67%;" />

## 2.3 其他方法

> Object[] toArray()：返回包含当前集合中所有元素的数组
>
> int hashCode()：获取对象的哈希值
>
> Iterator iterator()：返回迭代器对象，用于集合遍历

```java
@Test
public void test6(){
    Collection coll = new ArrayList();
    coll.add("lily");
    coll.add("jack");
    coll.add("rose");
    System.out.println("coll = "+coll);
    Object[] objects = coll.toArray();
    System.out.println("用数组返回coll中所有元素：" + Arrays.toString(objects));

    //对应的，数组转换为集合：调用Arrays的asList(Object ...objs)
    Object[] obj = new Object[]{123,"aa","cc"};
    Collection list = Arrays.asList(obj);
    System.out.println("Arrays.asList(obj) = "+list);
}
```

<img src="E:\Notes\JavaSE\assets\image-20240407213231682.png" alt="image-20240407213231682" style="zoom:80%;" />

# 3 Iterator(迭代器)接口

## 3.1 Iterator接口详情

- 在程序开发中，经常需要遍历集合中的所有元素。针对这种需求，JDK专门提供了一个接口`java.util.Iterator`。`Iterator`接口也是Java集合中的一员，但它与`Collection`、`Map`接口有所不同。
  - Collection接口与Map接口主要用于`存储`元素
  - `Iterator`，被称为迭代器接口，本身并不提供存储对象的能力，主要用于`遍历`Collection中的元素


- Collection接口继承了java.lang.Iterable接口，该接口有一个iterator()方法，那么所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了Iterator接口的对象。
  - `public Iterator iterator()`: 获取集合对应的迭代器，用来遍历集合中的元素的。
  - 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前。

- Iterator接口的常用方法如下：
  - `public E next()`:返回迭代的下一个元素。
  - `public boolean hasNext()`:如果仍有元素可以迭代，则返回 true。

- 注意：在调用it.next()方法之前必须要调用it.hasNext()进行检测。若不调用，且下一条记录无效，直接调用it.next()会抛出`NoSuchElementException异常`。

```java
@Test
    public void test7(){
        Collection coll = new ArrayList();
        coll.add("lily");
        coll.add("jack");
        coll.add("rose");
        System.out.println("coll = "+coll);
        //拿到集合的迭代器
        Iterator iterator = coll.iterator();
        //进行遍历
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
```

<img src="E:\Notes\JavaSE\assets\image-20240407213903288.png" alt="image-20240407213903288" style="zoom:80%;" />

## 3.2 迭代器执行原理

Iterator迭代器对象在遍历集合时，内部采用指针的方式来跟踪集合中的元素，接下来通过一个图例来演示Iterator对象迭代元素的过程：

<img src="E:\Notes\JavaSE\assets\image-20240407213943709.png" alt="image-20240407213943709" style="zoom:80%;" />

使用Iterator迭代器删除元素：java.util.Iterator迭代器中有一个方法：void remove() ;

```java
Iterator iter = coll.iterator();//回到起点
while(iter.hasNext()){
    Object obj = iter.next();
    if(obj.equals("Tom")){
        iter.remove();
    }
}
```

注意：

- Iterator可以删除集合的元素，但是遍历过程中通过迭代器对象的remove方法，不是集合对象的remove方法。
- 如果还未调用next()或在上一次调用 next() 方法之后已经调用了 remove() 方法，再调用remove()都会报IllegalStateException。

- Collection已经有remove(xx)方法了，为什么Iterator迭代器还要提供删除方法呢？因为迭代器的remove()可以按指定的条件进行删除。

例如：要删除以下集合元素中的偶数

```java
@Test
public void test8(){
    Collection coll = new ArrayList();
    coll.add(1);
    coll.add(2);
    coll.add(3);
    coll.add(4);
    coll.add(5);
    coll.add(6);
    System.out.println(coll);//[1, 2, 3, 4, 5, 6]
    Iterator iterator = coll.iterator();
    while (iterator.hasNext()){
        Integer tmp = (Integer) iterator.next();
        if (tmp % 2 == 0){
            iterator.remove();
        }
    }
    System.out.println(coll);//[1, 3, 5]
}
```

## 3.3 foreach循环

- foreach循环（也称增强for循环）是 JDK5.0 中定义的一个高级for循环，专门用来`遍历数组和集合`的。


- foreach循环的语法格式：

```java
for(元素的数据类型 局部变量 : Collection集合或数组){ 
  	//操作局部变量的输出操作
}
//这里局部变量就是一个临时变量，自己命名就可以
```

- 对于集合的遍历，增强for的内部原理其实是个Iterator迭代器。如下图

<img src="E:\Notes\JavaSE\assets\image-20240407222855272.png" alt="image-20240407222855272" style="zoom:80%;" />

- 它用于遍历Collection和数组。通常只进行遍历元素，不要在遍历的过程中对集合元素进行增删操作

# 4. Collection子接口1：List

> 用于存储有序、可重复的数据---> 使用list代替数组，"动态数组"

**特点**

- List集合类中`元素有序`、且`可重复`，集合中的每个元素都有其对应的顺序索引
- JDK API中List接口的实现类常用的有：`ArrayList`、`LinkedList`和`Vector`

## 4.1 List接口方法

List除了从Collection集合继承的方法外，List 集合里添加了一些`根据索引`来操作集合元素的方法。

- 插入元素
  - `void add(int index, Object ele)`:在index位置插入ele元素
  - boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
- 获取元素
  - `Object get(int index)`:获取指定index位置的元素
  - List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
- 获取元素索引
  - int indexOf(Object obj):返回obj在集合中首次出现的位置
  - int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
- 删除和替换元素
  - `Object remove(int index)`:移除指定index位置的元素，并返回此元素

  - `Object set(int index, Object ele)`:**设置指定index位置的元素为ele**
    - 注意返回值


**从父接口Collection继承的十几个方法也都可以使用**

**小结：**

- 增：add(Object obj)、addAll(Collection coll) 、add(int index,Object obj)---（相当于插入） , addAll(int index , Collection coll)
- 删：remove(Object obj)、removeAll(Collection coll)、retain(Collection coll)、 remove(int index)
- 改：set(int index , Object ele)  、
- 查：get(int index)
- 长度 size（）、包含contain（Object obj）和 containAll（Collection coll）等方法
- 遍历：迭代器iterator 或者增强for都行

> 其他的方法用到了查看API即可

**注意**：remove（Object obj） 和remove（int index）两个方法

```java
remove(2) ? ---> 默认使用的是 remove(int index)
想要使用remove(Object obj) 需要remove(Integer.valueOf(2))将 int类型转化为包装类 
```

```java
@Test
public void test1(){
    List list = new ArrayList();
    list.add("眉头奶");
    list.add("不高兴");
    list.add("没头脑");
    list.add(1,new PostGraduate("jack",19));
    System.out.println("after add four  times : list-size--->"+list.size());
    System.out.println("list = "+list);
    list.remove(0);
    System.out.println("after remove index-0:"+list);
    list.set(0,"no happy");
    System.out.println("set index-0 as :"+list.get(0));
    System.out.println("list = "+list);
    System.out.println("get index-1:"+list.get(1));
    Iterator iterator = list.iterator();
    System.out.println("iterator:");
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240408202314700.png" alt="image-20240408202314700" style="zoom:80%;" />

## 4.2 接口实现类：ArrayList

- ArrayList 是 List 接口的`主要实现类`

- 本质上，ArrayList是对象引用的一个”变长”数组  **Object[ ] obj**

- Arrays.asList(…) 方法返回的 List 集合，既不是 ArrayList 实例，也不是 Vector 实例。 Arrays.asList(…) 返回值是一个固定长度的 List 集合

- **方法同 List接口的方法**

## 4.3 接口实现类：LinkedList

- 对于频繁的**插入或删除**元素的操作，建议使用LinkedList类，效率较高。这是由于底层采用**双向链表**存储数据
- 特有方法：
  - void addFirst(Object obj)
  - void addLast(Object obj)	
  - Object getFirst()
  - Object getLast()
  - Object removeFirst()
  - Object removeLast()

## 4.4 接口实现类：Vector

- Vector 是一个`古老`的集合，JDK1.0就有了。大多数操作与ArrayList相同，区别之处在于Vector是`线程安全`的。
- 在各种List中，最好把`ArrayList作为默认选择`。当插入、删除频繁时，使用LinkedList；Vector总是比ArrayList慢，所以尽量避免使用。

# 5 Collection子接口2：Set

- Set接口是Collection的子接口，Set接口相较于Collection接口**没有提供额外的方法**
- Set 集合存储**无序**、**不可重复**的数据
- Set集合支持的遍历方式和Collection集合一样：foreach和Iterator
- Set的常用实现类有：HashSet、TreeSet、LinkedHashSet

> HashSet 是主要实现类，底层使用的是HashMap，即数组+单向链表+红黑树结构进行存储（JDK-8）
>
> LinkedHashSet 是HashSet的子类，在现有的数组+单向链表+红黑树结构上，又添加了一组双向链表，用于记录添加元素的先后顺序，也就是说，可以按照添加元素的先后顺序进行遍历（便于频繁的查询操作）

**开发中**：

- 较List、Map来说，Set的使用场景比较少
- 一定要用的话，可能是用于**过滤重复数据**



## 5.1 Set主要实现类：HashSet

### 5.1.1 HashSet概述

- HashSet按照Hash算法来存储集合中的元素，因此具有很好的存储、查询、删除性能
- HashSet具有以下特点：
  - 不能保证元素的排列顺序，即无序性
    - 无序性指的是元素位置的无序，具体来说：添加每一个元素到数组中时，具体的存储位置是由元素的hashCode()调用后返回的hash值决定的。导致在数组中每个元素不是依次紧密存放的
  - HashSet不是线程安全的
  - 集合元素可以为null
- HashSet**判断两个元素是否相等**：首先，两个元素的hashCode()返回值相等，并且equals方法返回值为true
  - 因此，存放在HashSet中的元素**必须要重写**equals方法和hashCode方法，即相等的对象必须有相同的散列码

### 5.1.2 HashSet添加元素的过程

- 第1步：当向 HashSet 集合中存入一个元素时，HashSet 会调用该对象的 hashCode() 方法得到该对象的 hashCode值，然后根据 hashCode值，通过某个散列函数决定该对象在 HashSet 底层数组中的存储位置

- 第2步：如果要在数组中存储的位置上没有元素，则直接添加成功

- 第3步：如果要在数组中存储的位置上有元素，则继续比较：

  - 如果两个元素的hashCode值不相等，则添加成功
  - 如果两个元素的hashCode()值相等，则会继续调用equals()方法：
    - 如果equals()方法结果为false，则添加成功
    - 如果equals()方法结果为true，则添加失败

  > 第2步添加成功，元素会保存在底层数组中
  >
  > 第3步两种添加成功的操作，由于该底层数组的位置已经有元素了，则会通过`链表`的方式继续链接，存储

```java
@Test
public void test1(){
    HashSet hashSet = new HashSet();
    hashSet.add("wkikyo");
    hashSet.add("jack");
    hashSet.add("rose");
    hashSet.add("jerry");
    hashSet.add("tom");
    hashSet.add("tom");
    System.out.println("set = "+hashSet);
}
```

![image-20240409161006522](E:\Notes\JavaSE\assets\image-20240409161006522.png)

```java
public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    /*
    *	name和age的get和set方法 这里省略了---占位置
    */
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age && Objects.equals(name, student.name);
    }
    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
@Test
public void test2(){
    HashSet hashSet = new HashSet();
    hashSet.add(new Student("tom",17));
    hashSet.add(new Student("tom",17));
    hashSet.add(new Student("tom",18));
    hashSet.add(new Student("jerry",17));
    Iterator iterator = hashSet.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240409161214628.png" alt="image-20240409161214628" style="zoom:80%;" />

**推荐：开发中直接调用IDEA里的快捷键自动重写equals()和hashCode()方法即可**

## 5.2 Set实现类：LinkedHashSet

- LinkedHashSet是HashSet的子类，同样有过滤重复数据的特点
- LinkedHashSet同样根据元素的hashCode返回值决定元素的存储位置
  - 同时使用一组双向链表维护元素的次序，是的元素看起来是按照 **添加顺序** 存储的

<img src="E:\Notes\JavaSE\assets\image-20240409161850358.png" alt="image-20240409161850358" style="zoom: 67%;" />

```java
@Test
public void test3(){
    LinkedHashSet linkedHashSet = new LinkedHashSet();
    linkedHashSet.add("AA");
    linkedHashSet.add(123);
    linkedHashSet.add(123);
    linkedHashSet.add(new Student("wkikyo",18));
    Iterator iterator = linkedHashSet.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240409162218808.png" alt="image-20240409162218808" style="zoom:80%;" />

**添加到HashSet/LinkedHashSet中元素的要求:**

- 要求元素所在的类要重写两个方法：equals() 和 hashCode()
- 同时，要求equals() 和 hashCode()要**保持一致性**！我们只需要在IDEA中自动生成两个方法的重写即可，即能保证两个方法的一致性

## 5.3 Set实现类：TreeSet

- TreeSet是SortedSet接口（**SortedSet**接口是**Set**接口的**子接口**）的实现类
  - TreeSet可以按照元素指定的属性的大小进行排序
- 特点：不允许重复、实现排序（自然排序或者定制排序）
  - 默认使用自然排序
  - 要使用定制排序，**需要将comparator接口的实现类的实例作为形参传给TreeSet的构造器**
- 底层数据结构：**红黑树**

**向TreeSet中添加的元素的要求**：

- 要求添加到TreeSet中的元素必须是同一个类型的对象，否则会报ClassCastExceptio
- 添加的元素需要考虑排序：① 自然排序 ② 定制排序

**判断数据是否相同的标准**：

- 不再是考虑hashCode()和equals()方法了，也就意味着添加到TreeSet中的元素所在的类**不需要重写**hashCode()和equals()方法了
- 比较元素大小的或比较元素是否相等的标准就是考虑自然排序或定制排序中，compareTo()或compare()的返回值。如果compareTo()或**compare()的返回值为0，则认为两个对象是相等的**。由于TreeSet中不能存放相同的元素，则后一个相等的元素就不能添加TreeSet中

```java
@Test
public void test4(){
    TreeSet set = new TreeSet();
    set.add("AA");
    set.add("CC");
    set.add("SS");
    set.add("GG");
    set.add("MM");
    set.add("DD");
    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240409164456756.png" alt="image-20240409164456756" style="zoom: 80%;" />

先自定义一个可以排序的类：

- **实现comparable接口，重写compare方法**

```java
public class User implements Comparable{

    private String name;
    private int age;

    public User() {
    }

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

  /*
  * 	实际文件中有get/set方法 我删去了，占位置
  */

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Object o) {
        if (o instanceof User){
            User u = (User) o;
            //按照年龄降序排列
            return -(age - u.getAge());
        }
        throw new RuntimeException("类型不匹配异常");
    }
}
```

```java
@Test
public void test5(){
    TreeSet set = new TreeSet();
    set.add(new User("tom",17));
    set.add(new User("wkikyo",17));
    set.add(new User("jerry",19));
    set.add(new User("rose",20));
    set.add(new User("jack",21));
    System.out.println("TreeSet = "+set);
    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

可以看到wkikyo和tom都是17岁，按照TreeSet文档和重写的compare方法来看 ，他俩为**同一个元素**

![image-20240409165711529](E:\Notes\JavaSE\assets\image-20240409165711529.png)

可以看到 {wkikyo ，17 }，确实被过滤掉了，其实可以修改compare的方法体来实现wkikyo的插入，比如说先按照年龄排序，（年龄相同的话）再按照名字排序，这样就不会过滤掉这样看上去 “重复” 的数据了

考虑用定制排序来实现一下（嘿嘿，都用一下）

- User类同上述一样

```java
@Test
public void test6(){
    //接口匿名实现类的对象实例 comparator  实现定制排序：1.年龄升序 2.名字升序（年龄一致）
    Comparator comparator = new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if (o1 instanceof User && o2 instanceof User){
                User u1 = (User) o1;
                User u2 = (User) o2;
                //先按照年龄升序排序
                int result = u1.getAge() - u2.getAge();
                if (result != 0){
                    return result;
                }
                //年龄一样，比较名字，利用String类的compareTo方法
                return u1.getName().compareTo(u2.getName());
            }
            throw new RuntimeException("类型不匹配异常");
        }
    };
    TreeSet set = new TreeSet(comparator);
    set.add(new User("jack",21));
    set.add(new User("tom",17));
    set.add(new User("jerry",19));
    set.add(new User("rose",20));
    set.add(new User("wkikyo",17));
    System.out.println("TreeSet = "+set);
    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }

}
```

![image-20240409170833213](E:\Notes\JavaSE\assets\image-20240409170833213.png)

可以看到 之前“重复”的数据，在定制排序下变得不重复了

# 6 Map接口

现实生活与开发中，常会看到这样的一类集合：用户ID与账户信息、学生姓名与考试成绩、IP地址与主机名等，这种**一一对应的关系**，就称作**映射**。Java提供了专门的集合框架用来存储这种映射关系的对象，即`java.util.Map`接口

## 6.1 接口概述

- Map接口与Collection接口并列，用于保存具有映射关系的数据：key-value
- Map中的key和value都可以是任何引用数据类型，但**String类型常用作Map的key**
- Map实现类：HashMap、LinkedHashMap、TreeMap、Hashtable、Properties，其中HashMap是Map接口最常用的实现类

<img src="E:\Notes\JavaSE\assets\image-20240409175214057.png" alt="image-20240409175214057" style="zoom:80%;" />

**Map实现类及对比：**

> HashMap：主要实现类，线程不安全（方法没有加synchronized）、效率高、可以添加null的k和v、底层使用数组+单向链表+红黑树的存储结构
>
> 而Hashtable 是Map接口的古老实现类：线程安全、效率低、不可以添加null的k和v、底层使用数组+单向链表
>
> LinkedHashMap 是HashMap的子类，在HashMap的基础上，增加了一组双向链表，用于记录添加元素的先后顺序，可以使遍历顺序为元素的添加顺序，开发中针对频繁的遍历操作，可以使用此类
>
> Properties：k和v都是String类型，常用来处理属性文件
>
> TreeMap：底层使用红黑树存储、可以按照添加的k-v队中key元素的指定属性的大小顺序进行遍历，需要考虑自然排序或者定制排序

## 6.2 Map中key-value特点

这里主要以HashMap为例说明。HashMap中存储的key、value的特点如下：

<img src="E:\Notes\JavaSE\assets\image-20240409180528843.png" alt="image-20240409180528843" style="zoom: 80%;" />

- 竖着看，key无序、不可重复的  因此 map中的所有key组成了一个set 称之KeySet
  - 既然是set就要保证 重写hashCode方法和equals方法
- 竖着看，value无序、可重复，因此map中所有的value 组成的集合 不是set也不是list 泛泛称之 collection
  - collection集合要重写equals方法
- 横着看，一个key和一个value组成一个entry节点，entry之间无序、不可重复

有时候只要重写equals方法 有时候重写hashCode和equals方法，那么**建议将hashCode和equals方法都重写**，即便用不到

## 6.2 Map接口的常用方法

- **添加、修改操作：**
  - Object put(Object key,Object value)：将指定key-value**添加**到(**或修改**)当前map对象中
  - void putAll(Map m):将m中的所有key-value对存放到当前map中
- **删除操作：**
  - Object remove(Object key)：移除指定key的key-value对，并返回value
  - void clear()：清空当前map中的所有数据
- **元素查询的操作：**
  - Object **get**(Object key)：获取指定key对应的value
  - boolean containsKey(Object key)：是否包含指定的key
  - boolean containsValue(Object value)：是否包含指定的value
  - int size()：返回map中key-value对的个数
  - boolean isEmpty()：判断当前map是否为空
  - boolean equals(Object obj)：判断当前map和参数对象obj是否相等
- **元视图操作的方法：**
  - Set keySet()：返回所有key构成的Set集合
  - Collection values()：返回所有value构成的Collection集合
  - Set entrySet()：返回所有key-value对构成的Set集合

    遍历：
       遍历key集：Set keySet()
       遍历value集：Collection values()
       遍历entry集：Set entrySet()

```java
@Test
public void test2(){
    Map map = new HashMap();
    map.put("黄晓明","杨颖");
    map.put("李晨","李小璐");
    //覆盖掉同一个key的值
    map.put("李晨","范冰冰");
    map.put("邓超","孙俪");
    System.out.println("map-size--->"+map.size());
    System.out.println("map--->"+map);
    //删除指定的key-value
    Object removed = map.remove("黄晓明");
    System.out.println("removed:"+removed);
    System.out.println("map-size--->"+map.size());
    System.out.println("map--->"+map);
    //获取指定的key对应的value值
    Object o = map.get("邓超");
    System.out.println(o);
    //清空map集合
    map.clear();
    System.out.println("map--->"+map);
    System.out.println("map-size--->"+map.size());
    System.out.println("is empty? "+map.isEmpty());
}
```

<img src="E:\Notes\JavaSE\assets\image-20240409182910467.png" alt="image-20240409182910467" style="zoom:80%;" />

```java
//遍历操作
@Test
public void test3(){
    Map map = new HashMap();
    map.put("许仙", "白娘子");
    map.put("董永", "七仙女");
    map.put("牛郎", "织女");
    map.put("许仙", "小青");
    System.out.println("all keys:");
    //foreach 或者 iterator都可以
    for (Object object: map.keySet()){
        System.out.println(object);
    }
    System.out.println("all values:");
    for (Object object:map.values()){
        System.out.println(object);
    }
    System.out.println("all entry:");
    for (Object object : map.entrySet()){
        System.out.println(object);
        Map.Entry entry = (Map.Entry)object;
        System.out.println(entry.getKey()+"-->"+entry.getValue());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240409183608842.png" alt="image-20240409183608842" style="zoom:67%;" />

## 6.3 Map实现类：HashMap

- HashMap是 Map 接口`使用频率最高`的实现类。
- HashMap是线程不安全的。允许添加 null 键和 null 值。
- 存储数据采用的哈希表结构，底层使用`一维数组`+`单向链表`+`红黑树`进行key-value数据的存储。与HashSet一样，元素的存取顺序不能保证一致。
- HashMap `判断两个key相等的标准`是：两个 key 的hashCode值相等，通过 equals() 方法返回 true。（KeySet集合）
- HashMap `判断两个value相等的标准`是：两个 value 通过 equals() 方法返回 true。（Collection集合）

## 6.4 Map实现类：LinkedHashMap

- LinkedHashMap 是 HashMap 的子类
- 存储数据采用的哈希表结构+链表结构，在HashMap存储结构的基础上，使用了一对`双向链表`来`记录添加元素的先后顺序`，可以保证遍历元素时，与添加的顺序一致。
- 通过哈希表结构可以保证键的唯一、不重复，需要键所在类重写hashCode()方法、equals()方法

## 6.5 Map实现类：TreeMap

- TreeMap存储 key-value 对时，需要根据 key-value 对进行排序。TreeMap 可以保证所有的 key-value 对处于`有序状态`。
- TreeSet底层使用`红黑树`结构存储数据
- TreeMap 的 Key 的排序：
  - `自然排序`：TreeMap 的所有的 Key 必须实现 Comparable 接口，而且所有的 Key 应该是同一个类的对象，否则将会抛出 ClasssCastException
  - `定制排序`：创建 TreeMap 时，构造器传入一个 Comparator 对象，该对象负责对 TreeMap 中的所有 key 进行排序。此时不需要 Map 的 Key 实现 Comparable 接口
- TreeMap判断`两个key相等的标准`：两个key通过compareTo()方法或者compare()方法返回0。

User类中的自然排序：实现comparable接口 重写了compareTo方法 ，User类同TreeSet中一样

```java
@Test
public void test2(){
    TreeMap map = new TreeMap();
    //User类中的自然排序，年龄降序，wkikyo应该无法添加，且tom的value应该被替换
    map.put(new User("jack",21),99);
    map.put(new User("tom",17),80);
    map.put(new User("jerry",19),19);
    map.put(new User("rose",20),"rose");
    map.put(new User("wkikyo",17),"wkikyo");
    for (Object object : map.entrySet()) {
        System.out.println(object);
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240410145446948.png" alt="image-20240410145446948" style="zoom:80%;" />

实现定制排序：

```java
@Test
public void test3(){
    //定制排序  先按年龄升序 ，年龄一致按照名字升序
    Comparator comparator = new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if (o1 instanceof User && o2 instanceof User){
                User u1 = (User) o1;
                User u2 = (User) o2;
                int result = u1.getAge() - u2.getAge();
                if (result != 0){
                    return result;
                }
                return u1.getName().compareTo(u2.getName());
            }
            throw new RuntimeException("classCastException");
        }
    };
    TreeMap map = new TreeMap(comparator);
    map.put(new User("jack",21),99);
    map.put(new User("tom",17),80);
    map.put(new User("jerry",19),19);
    map.put(new User("rose",20),"rose");
    map.put(new User("wkikyo",17),"wkikyo");
    for (Object object : map.entrySet()) {
        System.out.println(object);
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240410150136854.png" alt="image-20240410150136854" style="zoom:80%;" />

这样在treemap中 tom和wkikyo就不认为是同一组Key了

## 6.6 Hashtable 仅作为面试题

- Hashtable是Map接口的`古老实现类`，JDK1.0就提供了。不同于HashMap，Hashtable是线程安全的。
- Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构（数组+单向链表），查询速度快。
- 与HashMap一样，Hashtable 也不能保证其中 Key-Value 对的顺序
- Hashtable判断两个key相等、两个value相等的标准，与HashMap一致。
- 与HashMap不同，Hashtable 不允许使用 null 作为 key 或 value。

**面试题：Hashtable和HashMap的区别**

```
HashMap:底层是一个哈希表（jdk7:数组+链表;jdk8:数组+链表+红黑树）,是一个线程不安全的集合,执行效率高
Hashtable:底层也是一个哈希表（数组+链表）,是一个线程安全的集合,执行效率低

HashMap集合:可以存储null的键、null的值
Hashtable集合,不能存储null的键、null的值

Hashtable和Vector集合一样,在jdk1.2版本之后被更先进的集合(HashMap,ArrayList)取代了。所以HashMap是Map的主要实现类，Hashtable是Map的古老实现类。

Hashtable的子类Properties（配置文件）依然活跃在历史舞台
Properties集合是一个唯一和IO流相结合的集合
```

## 6.7 Map实现类：Properties

- Properties 类是 Hashtable 的子类，该对象用于处理属性文件

- 由于属性文件里的 key、value 都是字符串类型，所以 Properties 中要求 key 和 value 都是字符串类型

- 存取数据时，建议使用setProperty(String key,String value)方法和getProperty(String key)方法

```java
@Test
public void test4() throws IOException {
    FileInputStream fls = null;
    File file = new File("info.properties");
    System.out.println(file.getAbsolutePath());
    try {
        //文件输入流，为file文件和 properties对象建立连接
        fls = new FileInputStream(file);
        Properties properties = new Properties();
        properties.load(fls);
        //properties对象的getProperty方法可以通过key拿到value值
        String name = properties.getProperty("name");
        String pwd = properties.getProperty("password");
        System.out.println(name+" : "+pwd);
    } catch (FileNotFoundException e) {
        throw new RuntimeException(e);
    }finally {
        fls.close();
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240410152223123.png" alt="image-20240410152223123" style="zoom:80%;" />

![image-20240410152238781](E:\Notes\JavaSE\assets\image-20240410152238781.png)

# 7 Collections工具类

参考操作数组的工具类：Arrays，Collections 是一个**操作 Set、List 和 Map 等集合的工具类**

## 7.1 常用方法

Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法（均为static方法）：

**排序：**

- reverse(List)：反转 List 中元素的顺序
- shuffle(List)：对 List 集合元素进行随机排序
- sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
- sort(List，Comparator)：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
- swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换

**查找：**

- Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
- Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
- Object min(Collection)：根据元素的自然顺序，返回给定集合中的最小元素
- Object min(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最小元素
- int binarySearch(List list,T key)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且必须是可比较大小的，即支持自然排序的。而且集合也事先必须是有序的，否则结果不确定。
- int binarySearch(List list,T key,Comparator c)在List集合中查找某个元素的下标，但是List的元素必须是T或T的子类对象，而且集合也事先必须是按照c比较器规则进行排序过的，否则结果不确定。
- int frequency(Collection c，Object o)：返回指定集合中指定元素的出现次数

复制、替换
- void copy(List dest,List src)：将src中的内容复制到dest中  **注意：dest的size（） >=  src的size()**
- boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
- 提供了多个unmodifiableXxx()方法，该方法返回指定 Xxx的不可修改的视图。

添加
- boolean addAll(Collection  c,T... elements)将所有指定元素添加到指定 collection 中。

同步
- Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题



**面试题：区分Collection 和 Collections**

```
Collection：集合框架中的用于存储一个一个元素的接口，又分为List和Set等子接口
Collections：用于操作集合框架的一个工具类。此时的集合框架包括：Set、List、Map
```



