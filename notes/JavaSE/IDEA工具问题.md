# IDEA工具问题

# 1  快捷键、小技巧

new 类型（） 之后 可使用 alt + enter 补全左侧变量和类型

![](.\images\1.png)

之后就变为

```java
Car car = new Car();
```

* 光标停在构造器旁边 按下 crtl + P  可以查看构造器个数 和 每一个构造器的参数详情
* 修改文件名字的 快捷键时 shift + F6
* 在写方法的文档注释时 ，按下 /  ** （两个） 按下回车 就会生成文档注释的格式
* 单行注意快捷键 ctrl + /   多行注释 选中要注释的  然后 crtl + shift + /  

## 2  JDK相关设置

## 2.1 项目JDK设置

`File-->Project Structure...-->Platform Settings -->SDKs` 设置整个Java项目的JDK

<img src="E:\Notes\JavaSE\images\image-20240307152329865.png" style="zoom: 80%;" />

<img src="E:\Notes\JavaSE\images\image-20240307152513833.png" style="zoom:50%;" />

* SDKs全称是Software Development Kit ，这里一定是选择JDK的安装根目录，不是JRE的目录
* 这里还可以从本地添加多个JDK。使用“+”即可实现

## 2.2 项目的out目录和编译版本

`File-->Project Structure...-->Project Settings -->Project`

![image-20240307153400331](E:\Notes\JavaSE\images\image-20240307153400331.png)

# 3 详细设置

**在 file 下的 settings中可以看到idea的全部设置**

<img src="E:\Notes\JavaSE\images\image-20240307154231429.png" alt="image-20240307154231429" style="zoom: 80%;" />

## 3.1 工具系统设置

### 3.1.1 默认启动项目配置

<img src="E:\Notes\JavaSE\images\image-20240307154824315.png" alt="image-20240307154824315" style="zoom:67%;" />

启动IDEA时，默认自动打开上次开发的项目？还是自己选择？

如果去掉Reopen projects on startup前面的对勾，每次启动IDEA就会出现如下界面：

<img src="E:\Notes\JavaSE\images\image-20240307154947074.png" alt="image-20240307154947074" style="zoom:67%;" />

### 3.3.2 取消自动更新

Settings-->Appearance & Behavior->System Settings -> Updates

<img src="E:\Notes\JavaSE\images\image-20240307155409747.png" alt="image-20240307155409747" style="zoom:67%;" />

默认都打√了，建议检查IDE更新的√去掉，检查插件更新的√选上。

## 3.2 工具整体主题设置

### 3.2.1 IDE的整体色调

settings 下的appearance 右侧的 theme 可以调，建议黑色长时间看（啊哈哈哈。。。）

<img src="E:\Notes\JavaSE\images\image-20240307160020523.png" alt="image-20240307160020523" style="zoom: 67%;" />

### 3.2.2 字体大小

![image-20240307161231605](E:\Notes\JavaSE\images\image-20240307161231605.png)

个人不习惯这样，因为有些系统插件会误触

![image-20240307161716893](E:\Notes\JavaSE\images\image-20240307161716893.png)

在settings下的editor编辑器中的font中可以调节字体设置的相关问题

* 中间font 可以设置字体，习惯默认的 Mono了，size是大小，line height是行高
* 右侧可以进行预览

### 3.2.3 注释字体颜色

<img src="E:\Notes\JavaSE\images\image-20240307162513196.png" alt="image-20240307162513196" style="zoom: 67%;" />

- Block comment：修改多行注释的字体颜色
- Doc Comment –> Text：修改文档注释的字体颜色
- Line comment：修改单行注释的字体颜色

### 3.2.4 行号和方法分隔符

<img src="E:\Notes\JavaSE\images\image-20240307163105967.png" alt="image-20240307163105967" style="zoom:50%;" />

## 3.3 写代码相关

### 3.3.1 智能匹配

<img src="E:\Notes\JavaSE\images\image-20240307163604740.png" alt="image-20240307163604740" style="zoom: 67%;" />

IntelliJ IDEA 的代码提示和补充功能有一个特性：`区分大小写`。 如果想不区分大小写的话，就把这个对勾去掉。`建议去掉勾选`。

### 3.3.2 自动导包配置

* 默认需要自己手动导包，Alt+Enter快捷键

<img src="C:\Users\wb\AppData\Roaming\Typora\typora-user-images\image-20240307163742777.png" alt="image-20240307163742777" style="zoom: 67%;" />

* 自动导包设置
  * 动态导入明确的包：Add unambiguous imports on the fly，该设置具有全局性；
  * 优化动态导入的包：Optimize imports on the fly，该设置只对当前项目有效；

<img src="E:\Notes\JavaSE\images\image-20240307164004012.png" alt="image-20240307164004012" style="zoom:67%;" />

### 3.3.3 项目文件编码

<img src="E:\Notes\JavaSE\images\image-20240307164354508.png" alt="image-20240307164354508" style="zoom: 50%;" />

说明： Transparent native-to-ascii conversion主要用于转换ascii，显式原生内容。一般都要勾选。

* 控制台的字符集编码

  <img src="E:\Notes\JavaSE\images\image-20240307164851644.png" alt="image-20240307164851644" style="zoom:50%;" />

### 3.3.4 类头的文档注释信息

<img src="E:\Notes\JavaSE\images\image-20240307165530803.png" alt="image-20240307165530803" style="zoom: 67%;" />

# 4 工程与模块管理

## 4.1 IDEA项目结构

**层级关系:**

```
project(工程) - module(模块) - package(包) - class(类)
```

**具体的：**

```
一个project中可以创建多个module

一个module中可以创建多个package

一个package中可以创建多个class
```

在 IntelliJ IDEA 中Project是`最顶级的结构单元`，然后就是Module。目前，主流的大型项目结构基本都是多Module的结构，这类项目一般是`按功能划分`的，比如：user-core-module、user-facade-module和user-hessian-module等等，模块之间彼此可以`相互依赖`，有着不可分割的业务关系。因此，对于一个Project来说：

- 当为单Module项目的时候，这个单独的Module实际上就是一个Project。
- 当为多Module项目的时候，多个模块处于同一个Project之中，此时彼此之间具有`互相依赖`的关联关系。
- 当然多个模块没有建立依赖关系的话，也可以作为单独一个“小项目”运行。

## 4.2 创建模块module

![image-20240307171130750](E:\Notes\JavaSE\images\image-20240307171130750.png)

<img src="E:\Notes\JavaSE\images\image-20240307172328333.png" alt="image-20240307172328333" style="zoom:67%;" />

这样就拥有两个模块了

<img src="E:\Notes\JavaSE\images\image-20240307180304741.png" alt="image-20240307180304741" style="zoom:80%;" />

## 4.3 删除模块module

<img src="E:\Notes\JavaSE\images\image-20240307180444659.png" alt="image-20240307180444659" style="zoom: 80%;" />



remove module之后 只是将模块变成普通的文件夹，并没有删除，之后需要对文件夹进行delete删除才是彻底将该module删除掉

<img src="E:\Notes\JavaSE\images\image-20240307180648015.png" alt="image-20240307180648015" style="zoom:80%;" />

## 4.4 导入模块module

查看Project Structure，选择import module

![image-20240307181027082](E:\Notes\JavaSE\images\image-20240307181027082.png)

选择要导入的module

<img src="E:\Notes\JavaSE\images\image-20240307181117997.png" alt="image-20240307181117997" style="zoom: 67%;" />

<img src="E:\Notes\JavaSE\images\image-20240307181153506.png" alt="image-20240307181153506" style="zoom:50%;" />

接着可以一路Next下去，最后选择Overwrite

# 5 IntelliJ IDEA 常用快捷键一览表

##  5.1-IDEA的日常快捷键

### 第1组：通用型

| 说明            | 快捷键           |
| --------------- | ---------------- |
| 复制代码-copy   | ctrl + c         |
| 粘贴-paste      | ctrl + v         |
| 剪切-cut        | ctrl + x         |
| 撤销-undo       | ctrl + z         |
| 反撤销-redo     | ctrl + shift + z |
| 保存-save all   | ctrl + s         |
| 全选-select all | ctrl + a         |

### 第2组：提高编写速度（上）

| 说明                                               | 快捷键           |
| -------------------------------------------------- | ---------------- |
| 智能提示-edit                                      | alt + enter      |
| 提示代码模板-insert live template                  | ctrl+j           |
| 使用xx块环绕-surround with ...                     | ctrl+alt+t       |
| 调出生成getter/setter/构造器等结构-generate ...    | alt+insert       |
| 自动生成返回值变量-introduce variable ...          | ctrl+alt+v       |
| 复制指定行的代码-duplicate line or selection       | ctrl+d           |
| 删除指定行的代码-delete line                       | ctrl+y           |
| 切换到下一行代码空位-start new line                | shift + enter    |
| 切换到上一行代码空位-start new line before current | ctrl +alt+ enter |
| 向上移动代码-move statement up                     | ctrl+shift+↑     |
| 向下移动代码-move statement down                   | ctrl+shift+↓     |
| 向上移动一行-move line up                          | alt+shift+↑      |
| 向下移动一行-move line down                        | alt+shift+↓      |
| 方法的形参列表提醒-parameter info                  | ctrl+p           |

### 第3组：提高编写速度（下）

| 说明                                        | 快捷键       |
| ------------------------------------------- | ------------ |
| 批量修改指定的变量名、方法名、类名等-rename | shift+f6     |
| 抽取代码重构方法-extract method ...         | ctrl+alt+m   |
| 重写父类的方法-override methods ...         | ctrl+o       |
| 实现接口的方法-implements methods ...       | ctrl+i       |
| 选中的结构的大小写的切换-toggle case        | ctrl+shift+u |
| 批量导包-optimize imports                   | ctrl+alt+o   |

### 第4组：类结构、查找和查看源码

| 说明                                                      | 快捷键                          |
| --------------------------------------------------------- | ------------------------------- |
| 如何查看源码-go to class...                               | ctrl + 选中指定的结构 或 ctrl+n |
| 显示当前类结构，支持搜索指定的方法、属性等-file structure | ctrl+f12                        |
| 退回到前一个编辑的页面-back                               | ctrl+alt+←                      |
| 进入到下一个编辑的页面-forward                            | ctrl+alt+→                      |
| 打开的类文件之间切换-select previous/next tab             | alt+←/→                         |
| 光标选中指定的类，查看继承树结构-Type Hierarchy           | ctrl+h                          |
| 查看方法文档-quick documentation                          | ctrl+q                          |
| 类的UML关系图-show uml popup                              | ctrl+alt+u                      |
| 定位某行-go to line/column                                | ctrl+g                          |
| 回溯变量或方法的来源-go to implementation(s)              | ctrl+alt+b                      |
| 折叠方法实现-collapse all                                 | ctrl+shift+ -                   |
| 展开方法实现-expand all                                   | ctrl+shift+ +                   |

### 第5组：查找、替换与关闭

| 说明                                               | 快捷键       |
| -------------------------------------------------- | ------------ |
| 查找指定的结构                                     | ctlr+f       |
| 快速查找：选中的Word快速定位到下一个-find next     | ctrl+l       |
| 查找与替换-replace                                 | ctrl+r       |
| 直接定位到当前行的首位-move caret to line start    | home         |
| 直接定位到当前行的末位 -move caret to line end     | end          |
| 查询当前元素在当前文件中的引用，然后按 F3 可以选择 | ctrl+f7      |
| 全项目搜索文本-find in path ...                    | ctrl+shift+f |
| 关闭当前窗口-close                                 | ctrl+f4      |

### 第6组：调整格式

| 说明                                         | 快捷键           |
| -------------------------------------------- | ---------------- |
| 格式化代码-reformat code                     | ctrl+alt+l       |
| 使用单行注释-comment with line comment       | ctrl + /         |
| 使用/取消多行注释-comment with block comment | ctrl + shift + / |
| 选中数行，整体往后移动-tab                   | tab              |
| 选中数行，整体往前移动-prev tab              | shift + tab      |

##  5.2-Debug快捷键

| 说明                                                  | 快捷键        |
| ----------------------------------------------------- | ------------- |
| 单步调试（不进入函数内部）- step over                 | F8            |
| 单步调试（进入函数内部）- step into                   | F7            |
| 强制单步调试（进入函数内部） - force step into        | alt+shift+f7  |
| 选择要进入的函数 - smart step into                    | shift + F7    |
| 跳出函数 - step out                                   | shift + F8    |
| 运行到断点 - run to cursor                            | alt + F9      |
| 继续执行，进入下一个断点或执行完程序 - resume program | F9            |
| 停止 - stop                                           | Ctrl+F2       |
| 查看断点 - view breakpoints                           | Ctrl+Shift+F8 |
| 关闭 - close                                          | Ctrl+F4       |

















 







