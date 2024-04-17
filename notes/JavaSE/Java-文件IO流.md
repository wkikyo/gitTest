# 1 Java.io.File类的使用

-  File类及本章下的各种流，都定义在java.io包下
-  一个**File对象代表**硬盘或网络中**可能存在的一个文件或者目录**（俗称文件夹）（体会万事万物皆对象）
-  File 能新建、删除、重命名文件和目录，但 **File 不能访问文件内容本身**。如果需要访问文件内容本身，则需要使用输入/输出流
   - File对象可以作为参数传递给流的构造器。
-  想要在Java程序中表示一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象，可能没有一个真实存在的文件或目录

关于路径：

* **绝对路径：**从盘符开始的路径，这是一个完整的路径
* **相对路径：**相对于`项目目录`的路径，这是一个便捷的路径，开发中经常使用
  * IDEA中，main中的文件的相对路径，是相对于"`当前工程`"
  * IDEA中，单元测试方法中的文件的相对路径，是相对于"`当前module`"

## 1.1 构造器

* `public File(String pathname) ` ：以pathname为路径创建File对象，可以是绝对路径或者相对路径，如果pathname是相对路径，则默认的当前路径在系统属性user.dir中存储
* `public File(String parent, String child) ` ：以parent为父路径，child为子路径创建File对象
* `public File(File parent, String child)` ：根据一个父File对象（理解为目录）和子文件路径创建File对象

```java
@Test
public void test(){
    String pathname = "d:\\io\\test.txt";
    String parent = "d:\\io";
    String child = "abc.txt";
    //通过路径创建file对象
    File file = new File(pathname);//文件
    File file1 = new File(parent);//目录
    System.out.println(file.getAbsolutePath());
    System.out.println(file1.getAbsolutePath());
    //通过 父路径和子路径创建file对象
    File file2 = new File(parent, child);
    System.out.println(file1.getAbsolutePath());
    //通过父file对象和子路径创建对象
    File file3 = new File(file1, "kikyo.txt");
    System.out.println(file3.getAbsolutePath());
}
```

<img src="E:\Notes\JavaSE\assets\image-20240412161501265.png" alt="image-20240412161501265" style="zoom:80%;" />

> 注意：
>
> 1. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。
>
> 2. window的路径分隔符使用“\”，而Java程序中的“\”表示转义字符，所以在Windows中表示路径，需要用“\\”。或者直接使用“/”也可以，Java程序支持将“/”当成平台无关的`路径分隔符`。或者直接使用File.separator常量值表示。比如：
>
>    File file2 = new File("d:" + File.separator + "atguigu" + File.separator + "info.txt");

## 1.2 常用方法

### 1、获取文件和目录的基本信息

* public String getName() ：获取名称
* public String getPath() ：获取路径
* `public String getAbsolutePath()`：获取绝对路径
* public File getAbsoluteFile()：获取绝对路径表示的文件
* `public String getParent()`：获取上层文件目录路径。若无，返回null
* public long length() ：获取文件长度（即：字节数）。**不能获取目录的长度**
* public long lastModified() ：获取最后一次的修改时间，毫秒值

> 如果File对象代表的文件或目录存在，则File对象实例初始化时，就会用硬盘中对应文件或目录的属性信息（例如，时间、类型等）为File对象的属性赋值，否则除了路径和名称，File对象的其他属性将会保留默认值

<img src="E:\Notes\JavaSE\assets\image-20240412162001525.png" alt="image-20240412162001525" style="zoom:67%;" />

```java
@Test
public void test1(){
    //物理磁盘中不存在wkikyo.txt这个文件，存在io这个目录
    File file = new File("D:\\io\\wkikyo.txt");
    File file1 = new File("D:\\io");
    System.out.println("文件构造路径:"+file.getPath());
    System.out.println("文件名称:"+file.getName());
    System.out.println("文件长度:"+file.length()+"字节");
    System.out.println("文件最后修改时间：" + LocalDateTime.ofInstant(Instant.ofEpochMilli(file.lastModified()), ZoneId.of("Asia/Shanghai")));
    System.out.println("目录构造路径:"+file1.getPath());
    System.out.println("目录名称:"+file1.getName());
    System.out.println("目录长度:"+file1.length()+"字节");
    System.out.println("文件最后修改时间：" + LocalDateTime.ofInstant(Instant.ofEpochMilli(file1.lastModified()),ZoneId.of("Asia/Shanghai")));
}
```

<img src="E:\Notes\JavaSE\assets\image-20240412162709547.png" alt="image-20240412162709547" style="zoom:67%;" />

### 2、列出目录的下一级

* public String[] list() ：返回一个String数组，表示该File目录中的所有子文件或目录
* public File[] listFiles() ：返回一个File数组，表示该File目录中的所有的子文件或目录

```java
@Test
public void test2(){
    File file = new File("D:/Developing");
    System.out.println("-----------------------list()");
    for (String s : file.list()) {
        System.out.println(s);
    }
    System.out.println("-----------------------listFiles()");
    for (File tmp : file.listFiles()) {
        System.out.println(tmp.getName());
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240412163356001.png" alt="image-20240412163356001" style="zoom: 50%;" />

### 3、File类的重命名功能

- public boolean renameTo(File dest):把文件重命名为指定的文件路径

### 4、判断功能的方法

- `public boolean exists()` ：此File表示的文件或目录是否实际存在
- `public boolean isDirectory()` ：此File表示的是否为目录
- `public boolean isFile()` ：此File表示的是否为文件
- public boolean canRead() ：判断是否可读
- public boolean canWrite() ：判断是否可写
- public boolean isHidden() ：判断是否隐藏

> 如果文件或目录不存在，那么exists()、isFile()和isDirectory()都是返回true

### 5、创建、删除功能

- `public boolean createNewFile()` ：创建文件。若文件存在，则不创建，返回false 
- `public boolean mkdir()` ：创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建
- `public boolean mkdirs()` ：创建文件目录。如果上层文件目录不存在，一并创建
- `public boolean delete()` ：删除文件或者文件夹
  删除注意事项：① Java中的删除不走回收站。② 要删除一个文件目录，请注意该文件目录内不能包含文件或者文件目录
  - 即文件可以直接删除、目录需要保证为空才可删除

```java
@Test
public void test3() throws IOException {
    //相对路径，在当前module下
    File file = new File("readme.txt");
    File dir = new File("newDirA/newDirB");
    System.out.println("readme.txt exist ?--->"+file.exists());
    boolean result = file.createNewFile();
    System.out.println("readme create successfully ? --->"+result);
    System.out.println("dir exist ? --->"+dir.exists());
    boolean mkdir = dir.mkdir();
    System.out.println("dir create successfully by mkdir ? --->"+mkdir);
    boolean mkdirs = dir.mkdirs();
    System.out.println("dir create successfully by mkdirs ? --->"+mkdirs);
    System.out.println("readme.txt delete ? --->"+file.delete());
}
```

<img src="E:\Notes\JavaSE\assets\image-20240412164938882.png" alt="image-20240412164938882" style="zoom:80%;" />

![image-20240412165002797](E:\Notes\JavaSE\assets\image-20240412165002797.png)

# 2 IO流原理及流的分类

## 2.1 Java IO原理

- Java程序中，对于数据的输入/输出操作以“`流(stream)`” 的方式进行，可以看做是一种数据的流动

<img src="E:\Notes\JavaSE\assets\image-20240412192131953.png" alt="image-20240412192131953" style="zoom:80%;" />

I/O流中的I/O是`Input/Output`的缩写， I/O技术是非常实用的技术，用于处理设备之间的数据传输。如读/写文件，网络通讯等

- `输入input`：读取**外部数据**（磁盘、光盘等存储设备的数据）到**程序（内存）**中
- `输出output`：将**程序（内存**）数据输出到磁盘、光盘等**存储设备**中

> 以内存为中心，进来的叫输入，出去的输出~

## 2.2 流的分类

`java.io`包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过`标准的方法`输入或输出数据

- 按数据的流向不同分为：**输入流**和**输出流**。

  - **输入流** ：把数据从`其他设备`上读取到`内存`中的流。 
    - 以InputStream、Reader结尾
  - **输出流** ：把数据从`内存` 中写出到`其他设备`上的流。
    - 以OutputStream、Writer结尾

- 按操作数据单位的不同分为：**字节流（8bit）**和**字符流（16bit）**。

  - **字节流** ：以**字节为单位**，读写数据的流。
    - 以InputStream、OutputStream结尾
  - **字符流** ：以**字符为单位**，读写数据的流。
    - 以Reader、Writer结尾

- 根据IO流的角色不同分为：**节点流**和**处理流**。

  - **节点流**：直接从数据源或目的地读写数据

    ![image-20220412230745170](E:/Notes/01_课件与电子教材/01_课件与电子教材/尚硅谷_第15章_File类与IO流/images/image-20220412230745170.png)

  - **处理流**：不直接连接到数据源或目的地，而是“连接”在已存在的流（节点流或处理流）之上，通过对数据的处理为程序提供更为强大的读写功能。

    ![image-20240412192407908](E:\Notes\JavaSE\assets\image-20240412192407908.png)

## 2.3 流的API

- Java的IO流共涉及40多个类，实际上非常规则，都是从如下4个抽象基类派生的

| （抽象基类） |   输入流    |    输出流    |
| :----------: | :---------: | :----------: |
|    字节流    | InputStream | OutputStream |
|    字符流    |   Reader    |    Writer    |

- 由这四个类派生出来的子类名称都是以其父类名作为子类名后缀

![image-20240412192531267](E:\Notes\JavaSE\assets\image-20240412192531267.png)

**常用的节点流：** 　

* 文件流： FileInputStream、FileOutputStrean、FileReader、FileWriter 
* 字节/字符数组流： ByteArrayInputStream、ByteArrayOutputStream、CharArrayReader、CharArrayWriter 
  * 对数组进行处理的节点流（对应的不再是文件，而是内存中的一个数组）。

**常用处理流：**

* 缓冲流：BufferedInputStream、BufferedOutputStream、BufferedReader、BufferedWriter
  * 作用：增加缓冲功能，避免频繁读写硬盘，进而提升读写效率。
* 转换流：InputStreamReader、OutputStreamReader
  * 作用：实现字节流和字符流之间的转换。
* 对象流：ObjectInputStream、ObjectOutputStream
  * 作用：提供直接读写Java对象功能

# 3 节点流之一：FileReader\FileWriter

## 3.1 Reader与Writer

Java提供一些字符流类，以字符为单位读写数据，专门用于**处理文本**文件。**不能操作图片，视频等非文本文件**

> 常见的文本文件有如下的格式：.txt、.java、.c、.cpp、.py等
>
> 注意：.doc、.xls、.ppt这些都不是文本文件

### 3.1.1 字符输入流：Reader

`java.io.Reader`**抽象类**是表示用于**输入字符流的**所有类的**父类****，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法

- `public int read()`： 从输入流读取一个字符。 虽然读取了一个字符，但是会自动提升为int类型。返回该字符的Unicode编码值。如果已经到达流末尾了，则返回-1
  - 获取后想要输出的话，可以进行强制类型转换
- `public int read(char[] cbuf)`： 从输入流中读取一些字符，并将它们存储到字符数组 cbuf中 。每次最多读取cbuf.length个字符。**返回实际读取的字符个数**。如果已经到达流末尾，没有数据可读，则返回-1。 
  - **常用此方法**
- `public int read(char[] cbuf,int off,int len)`：从输入流中读取一些字符，并将它们存储到字符数组 cbuf中，从cbuf[off]开始的位置存储。每次最多读取len个字符。返回实际读取的字符个数。如果已经到达流末尾，没有数据可读，则返回-1。 
- `public void close()` ：关闭此流并释放与此流相关联的任何系统资源

### 3.1.2 字符输出流：Writer

`java.io.Writer `**抽象类**是表示用于**输出字符流**的所有类的**超类**，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法

- `public void write(int c)` ：写出单个字符
- `public void write(char[] cbuf) `：写出字符数组
  - 常用
- `public void write(char[] cbuf, int off, int len) `：写出字符数组的某一部分。off：数组的开始索引；len：写出的字符个数 
- `public void write(String str) `：写出字符串
  - 常用
- `public void write(String str, int off, int len)` ：写出字符串的某一部分。off：字符串的开始索引；len：写出的字符个数。
- `public void flush() `：刷新该流的缓冲
- `public void close()` ：关闭此流

> 当完成流的操作时，必须调用close()方法，释放系统资源，否则会造成内存泄漏

## 3.2 FileReader与FileWriter

### 3.2.1 FileReader

`java.io.FileReader `类用于读取字符文件，构造时使用系统默认的字符编码和默认字节缓冲区

- `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的File对象
  - 常用   
- `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称

**举例：**读取hello.txt文件中的字符数据，并显示在控制台上

```java
@Test
public void test1() {
    //由于close放在finally中了，所以流的声明在外边
    FileReader fr = null;
    try {
        //1.创建file对象，映射为物理磁盘中的某个文件
        File file = new File("readme.txt");
        //2.创建对应的字符输入流，通过构造器绑定一个file文件
        fr = new FileReader(file);
        //3.通过流的各种操作，文件进行read，输出在控制台
        int len; //每次读入的字符数目
        char[] buffer = new char[5];//相当于暂存区
        while ((len = fr.read(buffer)) != -1) {
            //方式1：
            /*for (int i = 0; i < len; i++) {
                System.out.print(buffer[i]);
            }*/
            //方式2:字符数组拼接为一个字符串 ，其实都一样
            String s = new String(buffer, 0, len);
            System.out.print(s);
        }
        System.out.println("\ninput all the chars!");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        try {
            //4.流使用完毕，切记要关闭
            if (fr != null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<img src="E:\Notes\JavaSE\assets\image-20240412200108468.png" alt="image-20240412200108468" style="zoom:80%;" />

### 3.2.2 FileWriter

`java.io.FileWriter `类用于写出字符到文件，构造时使用系统默认的字符编码和默认字节缓冲区。

- `FileWriter(File file)`： 创建一个新的 FileWriter，给定要输出到的File对象。   
- `FileWriter(String fileName)`： 创建一个新的 FileWriter，给定要输出到的文件的名称。  
- `FileWriter(File file,boolean append)`： 创建一个新的 FileWriter，指明是否在现有文件末尾追加内容
  - 默认为false，如果 file已经存在，则进行文件覆盖，改为true，则在文件末尾追加

```java
@Test
public void test2() {
    FileWriter fw = null;
    try {
        //1.磁盘文件映射
        File file = new File("readme.txt");
        //2.输出流绑定,append = true 进行末尾添加
        fw = new FileWriter(file,true);
        //3.进行输出
        fw.write("东南大学\n");
        fw.write("www.seu.edu.cn\n".toCharArray());
        System.out.println("output all the chars !");
    } catch (IOException e) {
        throw new RuntimeException(e);
    } finally {
        try {
            //4.关闭流
            if (fw != null)
                fw.close();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

![image-20240412201729947](E:\Notes\JavaSE\assets\image-20240412201729947.png)

### 3.2.3 总结

```
① 
因为出现流资源的调用，为了避免内存泄漏，需要使用try-catch-finally处理异常

② 
对于输入流来说，File类的对象必须在物理磁盘上存在，否则执行就会报FileNotFoundException。如果传入的是一个目录，则会报IOException异常。

对于输出流来说，File类的对象是可以不存在的。
   > 如果File类的对象不存在，则可以在输出的过程中，自动创建File类的对象
   > 如果File类的对象存在，
      > 如果调用FileWriter(File file)或FileWriter(File file,false)，输出时会新建File文件覆盖已有的文件
      > 如果调用FileWriter(File file,true)构造器，则在现有的文件末尾追加写出内容。
```

```java
//基于fileReader 和fileWriter进行一个文件复制,源文件readme.txt 复制文件readme_copy.txt
@Test
public void test3(){
    //1.建立映射关系
    File srcFile = new File("readme.txt");
    File destFile = new File("readme_copy.txt");
    FileReader fr = null;
    FileWriter fw = null;
    try {
        //2.字符输入输出流进行绑定
        int len;
        char[] buffer = new char[6];
        fr = new FileReader(srcFile);
        fw = new FileWriter(destFile,true);
        //3.进行copy操作，输入流读取，再输出流写入到目标文件上
        while ((len = fr.read(buffer)) != -1){
            fw.write(buffer,0,len);
        }
        System.out.println("copy all right!");
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        //4.用完的流记得关闭
        try {
            if (fr != null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        try {
            if (fw != null){
                fw.close();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

![image-20240412205349834](E:\Notes\JavaSE\assets\image-20240412205349834.png)

# 4 节点流之二：FileInputStream\FileOutputStream

### 4.1.1 字节输入流：InputStream

`java.io.InputStream `抽象类是表示**字节输入流**的所有类的**超类**，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法

- `public int read()`： 从输入流读取一个字节。返回读取的字节值。虽然读取了一个字节，但是会自动提升为int类型。如果已经到达流末尾，没有数据可读，则返回-1
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。每次最多读取b.length个字节。**返回实际读取的字节个数**。如果已经到达流末尾，没有数据可读，则返回-1
  - 常用此方法
- `public int read(byte[] b,int off,int len)`：从输入流中读取一些字节数，并将它们存储到字节数组 b中，从b[off]开始存储，每次最多读取len个字节 。返回实际读取的字节个数。如果已经到达流末尾，没有数据可读，则返回-1 
- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源

###  4.1.2 字节输出流：OutputStream

`java.io.OutputStream `抽象类是表示**字节输出流**的所有类的**超类**，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法

- `public void write(int b)` ：将指定的字节输出流。虽然参数为int类型四个字节，但是只会保留一个字节的信息写出
- `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流  
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流
  - 常用这个  配合输入流的 字节数组和岗哨   len 
- `public void flush() ` ：刷新此输出流并强制任何缓冲的输出字节被写出
- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源

> 说明：close()方法，当完成流的操作时，必须调用此方法，释放系统资源

## 4.2 FileInputStream 与 FileOutputStream

### 4.2.1 FileInputStream

`java.io.FileInputStream `类是文件输入流，从文件中读取字节。

- `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名
- `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名

### 4.2.2 FileOutputStream

`java.io.FileOutputStream `类是文件输出流，用于将数据写出到文件。

- `public FileOutputStream(File file)`：创建文件输出流，写出由指定的 File对象表示的文件。 
- `public FileOutputStream(String name)`： 创建文件输出流，指定的名称为写出文件。
- `public FileOutputStream(File file, boolean append)`：  创建文件输出流，指明是否在现有文件末尾追加内容

举例：实现一张图片的复制

```java
//利用字节流，完成一张图片的复制,源：coding.jpg   目标文件: coding_copy.jpg
//此处为了查看代码逻辑，直接扔出异常了，应该用try catch finally的
@Test
public void test() throws IOException {
    //1.物理磁盘文件映射
    File srcImage = new File("coding.jpg");
    File destImage = new File("coding_copy.jpg");
    //2.字节输入输出流进行绑定
    FileInputStream fis = new FileInputStream(srcImage);
    FileOutputStream fos = new FileOutputStream(destImage);
    //3.字节输入输出流进行文件复制
    int len;
    byte[] buffer = new byte[1024];
    while ((len = fis.read(buffer)) != -1){
        fos.write(buffer,0,len);
    }
    System.out.println("picture copied !");
    //4.流关闭
    if (fis != null)
        fis.close();
    if (fos != null)
        fos.close();
}
//总体来说，逻辑没有变化，和字符输入输出流的操作思维一致
```

![image-20240412213016616](E:\Notes\JavaSE\assets\image-20240412213016616.png)

# 5 处理流之一：缓冲流

**基础IO流的框架**：

- 抽象基类              4个节点流 (也称为文件流)            4个缓冲流（处理流的一种
- InputStream           FileInputStream                          BufferedInputStream
- OutputStream        FileOutputStream                        BufferedOutputStream
- Reader                   FileReader                                   BufferedReader
- Writer                      FileWriter                                     BufferedWriter

> - **缓冲流**要“套接”在相应的**节点流**之上，根据数据操作单位可以把缓冲流分为：
>   - **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream` 
>   - **字符缓冲流**：`BufferedReader`，`BufferedWriter`
>
> - 缓冲流的基本原理：在创建流对象时，内部会创建一个缓冲区数组（缺省使用`8192个字节(8Kb)`的缓冲区），通过缓冲区读写，减少系统IO次数，从而提高读写的效率

![image-20240413173205982](E:\Notes\JavaSE\assets\image-20240413173205982.png)

<img src="E:\Notes\JavaSE\assets\image-20240413173210219.png" alt="image-20240413173210219" style="zoom:80%;" />

## 5.1 构造器

* `public BufferedInputStream(InputStream in)` ：创建一个 字节型的缓冲输入流
* `public BufferedOutputStream(OutputStream out)`： 创建一个 字节型的缓冲输出流

```java
// 创建字节缓冲输入流
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("abc.jpg"));
// 创建字节缓冲输出流
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("abc_copy.jpg"))
```

* `public BufferedReader(Reader in)` ：创建一个   字符型的缓冲输入流
* `public BufferedWriter(Writer out)`： 创建一个  字符型的缓冲输出流

```java
// 创建字符缓冲输入流
BufferedReader br = new BufferedReader(new FileReader("br.txt"));
// 创建字符缓冲输出流
BufferedWriter bw = new BufferedWriter(new FileWriter("bw.txt"));
```

**使用的方法：**

处理非文本文件的字节流：
BufferedInputStream        read(byte[] buffer)
BufferedOutputStream       write(byte[] buffer,0,len) 、flush()

处理文本文件的字符流：
BufferedReader            read(char[] cBuffer) / String readLine()
BufferedWriter            write(char[] cBuffer,0,len) / write(String )  、flush()

**实现步骤：**

同之前的节点流一致，逻辑都一样，只是多创建了两个缓冲流用来包装

## 5.2 效率测试

缓冲流读写方法与基本的流是一致的，通过复制大文件（352MB），测试它的效率

```java
//先用字节输入输出流
@Test
public void test1() throws IOException{
    long start = System.currentTimeMillis();
    String src = "D:\\BandiCam_mp4\\s13_final_game.mp4";
    String dest = "D:\\BandiCam_mp4\\s13_final_game_copy1.mp4";
    fileStreamCopy(src,dest);
    long end = System.currentTimeMillis();
    System.out.println("spend time :"+(end-start)+"ms");
}

/**
 * 字节流实现视频文件复制，测试耗时
 * @param src 源文件位置和名称
 * @param dest 目标文件位置、名称
 */
//正式开发要用try catch finally 此出省略直接throws
public void fileStreamCopy(String src,String dest) throws IOException {
    //1.
    File srcFile = new File(src);
    File destFile = new File(dest);
    FileInputStream fis = new FileInputStream(srcFile);
    FileOutputStream fos = new FileOutputStream(destFile);
    //2.
    int len;
    byte[] buffer = new byte[1024];
    while ((len = fis.read(buffer)) != -1){
        fos.write(buffer,0,len);
    }
    //3.
    fis.close();
    fos.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413182212565.png" alt="image-20240413182212565" style="zoom:80%;" />

<img src="E:\Notes\JavaSE\assets\image-20240413182249339.png" alt="image-20240413182249339" style="zoom:80%;" />

耗时1.02s完成350MB的复制操作

```java
//用缓冲流进行文件copy
@Test
public void test2() throws IOException{
    long start = System.currentTimeMillis();
    String src = "D:\\BandiCam_mp4\\s13_final_game.mp4";
    String dest = "D:\\BandiCam_mp4\\s13_final_game_copy2.mp4";
    fileBufferCopy(src,dest);
    long end = System.currentTimeMillis();
    System.out.println("spend time :"+(end-start)+"ms");
}
public void fileBufferCopy(String src,String dest) throws IOException{
    //1.
    File srcFile = new File(src);
    File destFile = new File(dest);
    FileInputStream fis = new FileInputStream(srcFile);
    FileOutputStream fos = new FileOutputStream(destFile);
    BufferedInputStream bis = new BufferedInputStream(fis);
    BufferedOutputStream bos = new BufferedOutputStream(fos);
    //2.
    int len;
    byte[] buffer = new byte[1024];
    while ((len = bis.read(buffer)) != -1){
        bos.write(buffer,0,len);
    }
    //3.  处理流关闭了，包装内部的字节流也会关闭
    bis.close();
    bos.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413182928774.png" alt="image-20240413182928774" style="zoom:80%;" />

<img src="E:\Notes\JavaSE\assets\image-20240413182952901.png" alt="image-20240413182952901" style="zoom:80%;" />

耗时0.2s完成任务

## 5.3 字符缓冲流特有方法

字符缓冲流的基本方法与普通字符流调用方式一致，不再阐述，我们来看它们具备的特有方法。

* BufferedReader：`public String readLine()`: 读一行文字，不包括换行符
  * 返回值为读取的字符串，若一行只有换行符 则返回“” ，即**空字符串**，文件内容读完，才返回**null**，用作循环条件判断
* BufferedWriter：`public void newLine()`: 写一行 行分隔符,由系统属性定义符号

```java
@Test
public void test3() throws IOException {
    //字符缓冲流的新方法
    //1.
    File file = new File("dbcp_utf-8.txt");
    FileReader fr = new FileReader(file);
    BufferedReader br = new BufferedReader(fr);
    //2.
    String line;
    while ((line = br.readLine()) != null){
        //readline()不包含'\n'
        System.out.print(line+"\n");
    }
    //3.
    br.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413183954154.png" alt="image-20240413183954154" style="zoom:67%;" />

可以看到内容和文本文件的内容一致

```java
@Test
public void test4() throws IOException{
    //字符缓冲流实现文本文件的copy
    //1.
    File srcFile = new File("dbcp_utf-8.txt");
    File destFile = new File("dbcp_utf-8_copy.txt");
    FileReader fr = new FileReader(srcFile);
    FileWriter fw = new FileWriter(destFile);
    BufferedReader br = new BufferedReader(fr);
    BufferedWriter bw = new BufferedWriter(fw);
    //2.
    String line;
    while ((line = br.readLine()) != null){
        //readline()不包含'\n'
        //System.out.print(line+"\n");
        bw.write(line);
        //手动写入一个换行符，相当于bw.write(line+"\n");
        bw.newLine();
    }
    //3.
    br.close();
    bw.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413184558322.png" alt="image-20240413184558322" style="zoom:67%;" />

> 说明：
>
> 1. 涉及到嵌套的多个流时，如果都显式关闭的话，需要先关闭外层的流，再关闭内层的流
>
> 2. 其实在开发中，只需要关闭最外层的流即可，因为在关闭外层流时，内层的流也会被关闭

# 6 处理流之二：转换流



## 6.1 问题引入

**引入情况1：**

使用`FileReader` 读取项目中的文本文件。由于IDEA设置中针对项目设置了UTF-8编码，当读取Windows系统中创建的文本文件时，如果Windows系统默认的是GBK编码，则读入内存中会出现乱码

```java
@Test
public void test1() throws IOException {
    //1.
    File file = new File("康师傅的话.txt");
    FileReader fileReader = new FileReader(file);
    //2.
    int len;
    char[] buffer = new char[10];
    while ((len = fileReader.read(buffer)) != -1){
        System.out.println(new String(buffer,0,len));
    }
    //3.
    fileReader.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413222020871.png" alt="image-20240413222020871" style="zoom:67%;" />

那么如何读取GBK编码的文件呢？

**引入情况2：**

针对文本文件，现在使用一个**字节流**（fileInputStream）进行数据的读入，希望将数据显示在控制台上。此时针对包含中文的文本数据，可能会出现乱码

## 6.2 转换流的理解

**作用：转换流是字节与字符间的桥梁**

<img src="E:\Notes\JavaSE\assets\image-20240413222127918.png" alt="image-20240413222127918" style="zoom:80%;" />

具体来说：

<img src="E:\Notes\JavaSE\assets\image-20240413222139269.png" alt="image-20240413222139269" style="zoom:80%;" />

## 6.3 InputStreamReader 与 OutputStreamWriter

**InputStreamReader** 

- 转换流`java.io.InputStreamReader`，是**Reader**的子类，是**从字节流到字符流**的桥梁。它**读取字节，并使用指定的字符集将其解码为字符**。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

- 构造器

  - `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。 
  - `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流
    - 用指定字符集打开 内部字节流绑定的文件

- ```java
  //使用默认字符集
  InputStreamReader isr1 = new InputStreamReader(new FileInputStream("in.txt"));
  //使用指定字符集
  InputStreamReader isr2 = new InputStreamReader(new FileInputStream("in.txt") , "GBK");
  ```

```java
@Test
public void test2() throws IOException {
    //用转换流，InputStreamReader
    //1.
    File file = new File("康师傅的话.txt");
    FileInputStream fis = new FileInputStream(file);
    InputStreamReader isr = new InputStreamReader(fis, "GBK");
    //2.
    int len;
    char[] buffer = new char[10];
    while ((len = isr.read(buffer)) != -1){
        System.out.print(new String(buffer,0,len));
    }
    //3.
    isr.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240413223024839.png" alt="image-20240413223024839" style="zoom:80%;" />

这样GBK编码的文件就可以不乱码地读进内存了。。

**OutputStreamWriter**

- 转换流`java.io.OutputStreamWriter` ，是Writer的子类，是**从字符流到字节流**的桥梁。**使用指定的字符集将字符编码为字节**。它的字符集可以由名称指定，也可以接受平台的默认字符集。 

- 构造器

  - `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。 
  - `OutputStreamWriter(OutputStream in,String charsetName)`: 创建一个指定字符集的字符流

- ```java
  //使用默认字符集
  OutputStreamWriter isr = new OutputStreamWriter(new FileOutputStream("out.txt"));
  //使用指定的字符集
  OutputStreamWriter isr2 = new OutputStreamWriter(new FileOutputStream("out.txt") , "GBK")
  ```

```java
@Test
public void test3() throws IOException {
    //使用OutputStreamWriter 将内存中的字符按照指定编码格式，编码到磁盘中，从字符流到字节流
    //1.
    File src = new File("康师傅的话.txt");
    File dest = new File("康师傅的话utf8.txt");
    FileInputStream fis = new FileInputStream(src);
    FileOutputStream fos = new FileOutputStream(dest);
    InputStreamReader isr = new InputStreamReader(fis, "GBK");
    OutputStreamWriter osw = new OutputStreamWriter(fos, "utf8");
    //2.
    int len;
    char[] buffer = new char[20];
    while ((len = isr.read(buffer)) != -1) {
        osw.write(buffer,0,len);
    }
    System.out.println("编码集改变完成！");
    //3.
    isr.close();
    osw.close();
}
```

![image-20240413224138888](E:\Notes\JavaSE\assets\image-20240413224138888.png)

## 6.4 字符编码和字符集

### 6.4.1 编码与解码

计算机中储存的信息都是用`二进制数`表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果。按照某种规则，将字符存储到计算机中，称为**编码** ，反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码** 

**字符编码（Character Encoding）** : 就是一套自然语言的字符与二进制数之间的对应规则

**编码表**：生活中文字和计算机中二进制的对应规则

**乱码的情况**：按照A规则存储，同样按照A规则解析，那么就能显示正确的文本符号。反之，按照A规则存储，再按照B规则解析，就会导致乱码现象

```
编码:字符(人能看懂的)--字节(人看不懂的)

解码:字节(人看不懂的)-->字符(人能看懂的)
```

### 6.4.2 字符集

* **字符集Charset**：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符号、数字等

- 计算机要准确的存储和识别各种字符集符号，需要进行字符编码，一套字符集必然至少有一套字符编码。常见字符集有ASCII字符集、GBK字符集、Unicode字符集等

> 可见，当指定了**编码**，它所对应的**字符集**自然就指定了，所以**编码**才是我们最终要关心的

**下面的看看就行**

* **ASCII字符集** ：

  * ASCII码（American Standard Code for Information Interchange，美国信息交换标准代码）：上个世纪60年代，美国制定了一套字符编码，对`英语字符`与二进制位之间的关系，做了统一规定。这被称为ASCII码
  * ASCII码用于显示现代英语，主要包括控制字符（回车键、退格、换行键等）和可显示字符（英文大小写字符、阿拉伯数字和西文符号）
  * 基本的ASCII字符集，使用7位（bits）表示一个字符（最前面的1位统一规定为0），共`128个`字符。比如：空格“SPACE”是32（二进制00100000），大写的字母A是65（二进制01000001）
  * **缺点：不能表示所有字符**

* **ISO-8859-1字符集**：

  * 拉丁码表，别名Latin-1，用于显示欧洲使用的语言，包括荷兰语、德语、意大利语、葡萄牙语等
  * ISO-8859-1使用单字节编码，兼容ASCII编码。

* **GBxxx字符集**：

  * GB就是国标的意思，是为了`显示中文`而设计的一套字符集
  * **GB2312**：简体中文码表。一个小于127的字符的意义与原来相同，即向下兼容ASCII码。但两个大于127的字符连在一起时，就表示一个汉字，这样大约可以组合了包含`7000多个简体汉字`，此外数学符号、罗马希腊的字母、日文的假名们都编进去了，这就是常说的"全角"字符，而原来在127号以下的那些符号就叫"半角"字符了
  * **GBK**：最常用的中文码表。是在GB2312标准基础上的扩展规范，使用了`双字节`编码方案，共收录了`21003个`汉字，完全兼容GB2312标准，同时支持`繁体汉字`以及日韩汉字等
  * **GB18030**：最新的中文码表。收录汉字`70244个`，采用`多字节`编码，每个字可以由1个、2个或4个字节组成。支持中国国内少数民族的文字，同时支持繁体汉字以及日韩汉字等

* **Unicode字符集** ：

  * Unicode编码为表达`任意语言的任意字符`而设计，也称为统一码、标准万国码。Unicode 将世界上所有的文字用`2个字节`统一进行编码，为每个字符设定唯一的二进制编码，以满足跨语言、跨平台进行文本处理的要求

  - Unicode 的缺点：这里有三个问题：
    - 第一，英文字母只用一个字节表示就够了，如果用更多的字节存储是`极大的浪费`。
    - 第二，如何才能`区别Unicode和ASCII`？计算机怎么知道两个字节表示一个符号，而不是分别表示两个符号呢？
    - 第三，如果和GBK等双字节编码方式一样，用最高位是1或0表示两个字节和一个字节，就少了很多值无法用于表示字符，`不够表示所有字符`。
  - Unicode在很长一段时间内无法推广，直到互联网的出现，为解决Unicode如何在网络上传输的问题，于是面向传输的众多 UTF（UCS Transfer Format）标准出现。具体来说，有三种编码方案，UTF-8、UTF-16和UTF-32。

* **UTF-8字符集**：

  * Unicode是字符集，UTF-8、UTF-16、UTF-32是三种`将数字转换到程序数据`的编码方案。顾名思义，UTF-8就是每次8个位传输数据，而UTF-16就是每次16个位。其中，UTF-8 是在互联网上`使用最广`的一种 Unicode 的实现方式。
  * 互联网工程工作小组（IETF）要求所有互联网协议都必须支持UTF-8编码。所以，我们开发Web应用，也要使用UTF-8编码
  * UTF-8 是一种`变长的编码方式`。它使用1-4个字节为每个字符编码，编码规则：
    1. 128个US-ASCII字符，只需一个字节编码
    2. 拉丁文等字符，需要二个字节编码
    3. 大部分常用字（含**中文**），使用**三个字节**编码
    4. 其他极少使用的Unicode辅助字符，使用四字节编码

# 7 处理流之三/四：数据流、对象流

**数据流及其作用（了解）**

- **数据流：DataOutputStream、DataInputStream**
  - DataOutputStream:可以将内存中的**基本数据类型**的变量、**String类型**的变量写出到具体的文件中
  - DataInputStream:将文件中**保存的数据还原为内存中的基本数据类型的变量、String类型**的变量

- 对象流DataInputStream中的方法：

```java
  byte readByte()                short readShort()
  int readInt()                  long readLong()
  float readFloat()              double readDouble()
  char readChar()				 boolean readBoolean()					
  String readUTF()               void readFully(byte[] b)
```

> 将方法名字的read改为write就是DataOutputStream中的方法
>
> 数据流的弊端：只支持Java基本数据类型和字符串的读写，而不支持其它Java对象的类型

**对象流及其作用（要掌握的）**

- **对象流：ObjectOutputStream、ObjectInputStream**

  - ObjectOutputStream：将 Java 基本数据类型和对象写入字节输出流中。通过在流中使用文件可以实现Java各种基本数据类型的数据以及对象的持久存储。

  - ObjectInputStream：ObjectInputStream 对以前使用 ObjectOutputStream 写出的基本数据类型的数据和对象进行读入操作，保存在内存中

> 说明：对象流的强大之处就是可以把Java中的对象写入到数据源中，也能把对象从数据源中还原回来

## 7.1 对象流API

**ObjectOutputStream中的构造器：**

`public ObjectOutputStream(OutputStream out) `： 创建一个指定的ObjectOutputStream。

```java
FileOutputStream fos = new FileOutputStream("game.dat");
ObjectOutputStream oos = new ObjectOutputStream(fos);
```

**ObjectOutputStream中的方法：**

- public void writeBoolean(boolean val)：写出一个 boolean 值。
- public void writeByte(int val)：写出一个8位字节
- public void writeShort(int val)：写出一个16位的 short 值
- public void writeChar(int val)：写出一个16位的 char 值
- public void writeInt(int val)：写出一个32位的 int 值
- public void writeLong(long val)：写出一个64位的 long 值
- public void writeFloat(float val)：写出一个32位的 float 值。
- public void writeDouble(double val)：写出一个64位的 double 值
- public void writeUTF(String str)：将表示长度信息的两个字节写入输出流，后跟字符串 s 中每个字符的 UTF-8 修改版表示形式。根据字符的值，将字符串 s 中每个字符转换成一个字节、两个字节或三个字节的字节组。注意，将 String 作为基本数据写入流中与将它作为 Object 写入流中明显不同。 如果 s 为 null，则抛出 NullPointerException。
- `public void writeObject(Object obj)`：写出一个obj对象
  - **用这个方法将自定义引用类型对象进行序列化**
- public void close() ：关闭此输出流并释放与此流相关联的任何系统资源

**ObjectInputStream中的构造器：**

`public ObjectInputStream(InputStream in) `： 创建一个指定的ObjectInputStream。

```java
FileInputStream fis = new FileInputStream("game.dat");
ObjectInputStream ois = new ObjectInputStream(fis);
```

**ObjectInputStream中的方法：**

- public boolean readBoolean()：读取一个 boolean 值
- public byte readByte()：读取一个 8 位的字节
- public short readShort()：读取一个 16 位的 short 值
- public char readChar()：读取一个 16 位的 char 值
- public int readInt()：读取一个 32 位的 int 值
- public long readLong()：读取一个 64 位的 long 值
- public float readFloat()：读取一个 32 位的 float 值
- public double readDouble()：读取一个 64 位的 double 值
- public String readUTF()：读取 UTF-8 修改版格式的 String
- `public void readObject(Object obj)`：读入一个obj对象
  - 用这个反序列化对象
- public void close() ：关闭此输入流并释放与此流相关联的任何系统资源

## 7.2 对象序列化机制

**1、何为对象序列化机制？**

`对象序列化机制`允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。//当其它程序获取了这种二进制流，就可以恢复成原来的Java对象

- 序列化过程：用一个字节序列可以表示一个对象，该字节序列包含该`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中`持久保存`了一个对象的信息


- 反序列化过程：该字节序列还可以从文件中读取回来，重构对象，对它进行`反序列化`。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象

<img src="E:\Notes\JavaSE\assets\image-20240413232657008.png" alt="image-20240413232657008" style="zoom: 67%;" />

**2、序列化机制的重要性**

序列化是 RMI（Remote Method Invoke、远程方法调用）过程的参数和返回值都必须实现的机制，而 RMI 是 JavaEE 的基础。因此序列化机制是 JavaEE 平台的基础

序列化的好处，在于可将任何实现了Serializable接口的对象转化为**字节数据**，使其在保存和传输时可被还原

**3、实现原理**

- 序列化：用ObjectOutputStream类保存基本类型数据或对象的机制。方法为：
  - `public final void writeObject (Object obj)` : 将指定的对象写出

- 反序列化：用ObjectInputStream类读取基本类型数据或对象的机制。方法为：
  - `public final Object readObject ()` : 读取一个对象

<img src="E:\Notes\JavaSE\assets\image-20240413232827019.png" alt="image-20240413232827019" style="zoom:67%;" />

**4、如何实现序列化机制**

如果需要让某个对象支持序列化机制，则必须让对象所属的类及其属性是可序列化的，为了让某个类是可序列化的，该类必须实现`java.io.Serializable ` 接口。`Serializable` 是一个`标记接口`，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。

* 如果对象的某个属性也是引用数据类型，那么如果该属性也要序列化的话，也要实现`Serializable` 接口
* 该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。
* `静态（static）变量`的值不会序列化。因为静态变量的值不属于某个对象

**自定义类要想实现序列化机制，需要满足：**
① 自定义类需要实现接口：Serializable
② 要求自定义类声明一个全局常量： static final long serialVersionUID = 42234234L;
   用来唯一的标识当前的类。
③ 要求自定义类的各个属性也必须是可序列化的。

   > 对于基本数据类型的属性：默认就是可以序列化的
   > 对于引用数据类型的属性：要求实现Serializable接口

注意点：
① 如果不声明全局常量serialVersionUID，系统会自动声明生成一个针对于当前类的serialVersionUID。
如果修改此类的话，会导致serialVersionUID变化，进而导致反序列化时，出现InvalidClassException异常。
② 类中的属性如果声明为transient或static，则不会实现序列化

## 7.3 coding

```java
@Test
public void test1() throws IOException {
    //内存中创建一个对象，序列化到磁盘文件中
    File objectFile = new File("object.txt");
    FileOutputStream fos = new FileOutputStream(objectFile);
    ObjectOutputStream oos = new ObjectOutputStream(fos);
    oos.writeObject(new Person("Tom",22,new Account(7999.0)));
    System.out.println("序列化成功");
    oos.flush();
    oos.close();
}
```

![image-20240413234142268](E:\Notes\JavaSE\assets\image-20240413234142268.png)

可以看到 内存对象保存到txt文件中，只不过字节流保存的，我们看不懂而已

```java
@Test
public void test2() throws IOException, ClassNotFoundException {
    //将磁盘中的对象反序列到内存中展示
    File file = new File("object.txt");
    FileInputStream fis = new FileInputStream(file);
    ObjectInputStream ois = new ObjectInputStream(fis);
    Person person = (Person) ois.readObject();
    ois.close();
    System.out.println(person.toString());

}
```

![image-20240413234550970](E:\Notes\JavaSE\assets\image-20240413234550970.png)

可以看到 和之间序列化创建的对象是一致的！ok！

下面贴出person类和Account类：

```java
public class Person implements Serializable {
    private String name;
    private int age;
    private Account account;
    private static final long serialVersionUID = 1324234114444L;

    public Person() {
    }

    public Person(String name, int age, Account account) {
        this.name = name;
        this.age = age;
        this.account = account;
    }
    
    /*get和set属性的方法，省却...*/
    
    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", account=" + account +
                '}';
    }
}
class Account implements Serializable{
    private double balance;

    private static final long serialVersionUID = 1324234L;
    public Account() {
    }
    
    public Account(double balance) {
        this.balance = balance;
    }

    /*get和set方法，省却...*/

    @Override
    public String toString() {
        return "Account{" +
                "balance=" + balance +
                '}';
    }
}
```

# 8 其他流的使用

## 8.1 标准输入、输出流

- System.in和System.out分别代表了系统标准的输入和输出设备
  - 默认输入设备是：键盘，输出设备是：显示器（控制台）
- System.in的类型是**InputStream**
- System.out的类型是PrintStream，**其是OutputStream的子类FilterOutputStream 的子类**
- 重定向：通过System类的setIn，setOut方法对默认设备进行改变。
  - public static void setIn(InputStream in)
  - public static void setOut(PrintStream out)

举例：从键盘输入字符串，要求将读取到的整行字符串转成大写输出。然后继续进行输入操作，直至当输入“e”或者“exit”时，退出程序
（要求不使用scanner类）

```java
@Test
public void test1() throws IOException {
    //1.
    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    //2.
    String words = null;
    System.out.print("please input String:");
    while ((words = br.readLine()) != null) {
        if ("e".equals(words) || "exit".equals(words)){
            System.out.println(words);
            System.out.print("exit success! good bye~");
            break;
        }
        System.out.println(words+"--->"+words.toUpperCase());
        System.out.print("please input String again:");
    }
    //3.
    br.close();
}
```

![image-20240414200328664](E:\Notes\JavaSE\assets\image-20240414200328664.png)

**拓展：**

System类中有三个常量对象：System.out、System.in、System.err

查看System类中这三个常量对象的声明：

```java
public final static InputStream in = null;
public final static PrintStream out = null;
public final static PrintStream err = null;
```

奇怪的是，

- 这三个常量对象有final声明，但是却初始化为null。final声明的常量一旦赋值就不能修改，那么null不会空指针异常吗？
- 这三个常量对象为什么要小写？final声明的常量按照命名规范不是应该大写吗？
- 这三个常量的对象有set方法？final声明的常量不是不能修改值吗？set方法是如何修改它们的值的？

```
final声明的常量，表示在Java的语法体系中它们的值是不能修改的，而这三个常量对象的值是由C/C++等系统函数进行初始化和修改值的，所以它们故意没有用大写，也有set方法
```



## 8.2 打印流

- 实现将基本数据类型的数据格式转化为字符串输出。


- 打印流：`PrintStream`和`PrintWriter`

  - 提供了一系列重载的print()和println()方法，用于多种数据类型的输出
- System.out返回的是**PrintStream**的实例

  - 是outputstream 的一个子类

构造器

- PrintStream(File file) ：创建具有指定文件且不带自动行刷新的新打印流
  - 常用，配合setOut方法可以改变输出的位置
- PrintStream(File file, String csn)：创建具有指定文件名称和字符集且不带自动行刷新的新打印流
- PrintStream(OutputStream out) ：创建新的打印流
  - 常用，包装一个输出字节流，可以指定文件，是不是append
- PrintStream(OutputStream out, boolean autoFlush)：创建新的打印流。 autoFlush如果为 true，则每当写入 byte 数组、调用其中一个 println 方法或写入换行符或字节 ('\n') 时都会刷新输出缓冲区
  - 常用，同上，只是多了个自动刷新，遇到   '\n'
- PrintStream(OutputStream out, boolean autoFlush, String encoding) ：创建新的打印流
- PrintStream(String fileName)：创建具有指定文件名称且不带自动行刷新的新打印流
- PrintStream(String fileName, String csn) ：创建具有指定文件名称和字符集且不带自动行刷新的新打印流

```java
@Test
public void test2() throws FileNotFoundException {
    //打印流printStream
    PrintStream ps = new PrintStream(new FileOutputStream(new File("readme.txt"),true));
    ps.println(100);
    ps.println("china no.1 and hello world~");
    ps.print("你好,中国");
    ps.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240414201106672.png" alt="image-20240414201106672" style="zoom:80%;" />

```java
@Test
public void test3() throws IOException{
    // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
    FileOutputStream fos = new FileOutputStream("test.txt");
    PrintStream ps = new PrintStream(fos, true);
    //改变一个out的输出位置
    if (ps != null){
        System.setOut(ps);
    }
    for (int i = 0; i < 256; i++) {//打印ASCII码
        System.out.print((char) i);
        if (i%50 == 0){ //50个换一次行
            System.out.print('\n');
        }
    }
    ps.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240414201923625.png" alt="image-20240414201923625" style="zoom:80%;" />

```java
public class Logger {
    //记录日志的方法
    public static void log(String msg) throws IOException {
        //输出流重定向为log.txt文件
        PrintStream ps = new PrintStream(new FileOutputStream(new File("log.txt"), true));
        if (ps!=null){
            System.setOut(ps);
        }
        //进行日志的记录
        Date date = new Date();
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String formatDate = sdf.format(date);
        System.out.println(formatDate+":"+msg);
    }
    @Test
    public void test1() throws IOException {
        //测试工具类是否好用
        Logger.log("调用了System类的gc()方法，建议启动垃圾回收");
        Logger.log("调用了TeamView的addMember()方法");
        Logger.log("用户尝试进行登录，验证失败");
    }
}
```

![image-20240414202650050](E:\Notes\JavaSE\assets\image-20240414202650050.png)

## 8.3 Scanner类

构造方法

* Scanner(File source) ：构造一个新的 Scanner，它生成的值是从指定文件扫描的 
* Scanner(File source, String charsetName) ：构造一个新的 Scanner，它生成的值是从指定文件扫描的 
* Scanner(InputStream source) ：构造一个新的 Scanner，它生成的值是从指定的输入流扫描的
* Scanner(InputStream source, String charsetName) ：构造一个新的 Scanner，它生成的值是从指定的输入流扫描的

常用方法：

* boolean hasNextXxx()： 如果通过使用nextXxx()方法，此扫描器输入信息中的下一个标记可以解释为默认基数中的一个 Xxx 值，则返回 true。
* Xxx nextXxx()： 将输入信息的下一个标记扫描为一个Xxx

```java
@Test
public void test() throws FileNotFoundException {
    //测试从控制台输入字符到指定文件,要求用scanner和 printStream流来实现
    Scanner input = new Scanner(System.in);
    PrintStream ps = new PrintStream(new FileOutputStream(new File("scanner.txt"), true));
    /*if (ps!=null){
        System.setOut(ps);
    }*/
    String s = null;
    System.out.print("please input string:");
    while (true){
        s = input.nextLine();
        if ("e".equals(s) || "exit".equals(s)){
            System.out.println(s);
            System.out.print("exit success! good bye~");
            break;
        }
        System.out.println(s);
        ps.println(s);
        System.out.print("please input string again:");
    }
    ps.close();
    input.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240414204204818.png" alt="image-20240414204204818" style="zoom:67%;" />

```java
@Test
public void test2() throws FileNotFoundException {
    //从刚才输出的文件，把内容读取出来,用scanner
    Scanner input = new Scanner(new File("scanner.txt"));
    while (input.hasNext()){
        String s = input.nextLine();
        System.out.println(s);
    }
    input.close();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240414204539678.png" alt="image-20240414204539678" style="zoom: 80%;" />

# 9. apache-common包的使用

## 9.1 介绍

IO技术开发中，代码量很大，而且代码的重复率较高，为此Apache软件基金会，开发了IO技术的工具类`commonsIO`，大大简化了IO开发



Apahce软件基金会属于第三方，（Oracle公司第一方，我们自己第二方，其他都是第三方）我们要使用第三方开发好的工具，需要添加jar包

## 9.2导包及举例

- 在导入commons-io-2.5.jar包之后，内部的API都可以使用
  - 导包方式同 导入Junit包一样，先添加到project中，再选用指定的module进行jar包的使用

- IOUtils类的使用

```java
- 静态方法：IOUtils.copy(InputStream in,OutputStream out)传递字节流，实现文件复制。
- 静态方法：IOUtils.closeQuietly(任意流对象)悄悄的释放资源，自动处理close()方法抛出的异常。
```

- FileUtils类的使用

```java
- 静态方法：void copyDirectoryToDirectory(File src,File dest)：整个目录的复制，自动进行递归遍历
          参数:
          src:要复制的文件夹路径
          dest:要将文件夹粘贴到哪里去
             
- 静态方法：void writeStringToFile(File file,String content)：将内容content写入到file中
- 静态方法：String readFileToString(File file)：读取文件内容，并返回一个String
- 静态方法：void copyFile(File srcFile,File destFile)：文件复制
```

```java
@Test
public void test1() throws IOException {
    //IOUtils的使用 主要用cpoy和close两个方法  方法是静态的！！
    IOUtils.copy(new FileInputStream("coding.jpg"), new FileOutputStream("coding_IOUtils.jpg"));
    IOUtils.closeQuietly();
}
```

<img src="E:\Notes\JavaSE\assets\image-20240414205924681.png" alt="image-20240414205924681"  />

# 10 Java NIO

