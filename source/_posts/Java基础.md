---
title: Java基础
date: 2019-08-29 12:57:13
tags: java
categories: 学习笔记
---
### 数据类型
> Java作为强类型语言，将数据类型分成基本类型和引用类型。
- 基本数据类型（primitive）：byte、short、int、long、float、double、char、boolean(4类8种)；
- 引用数据类型（reference）：array、class、interface（除了基本类型的都是引用类型）；
- 作为面向对象的语言，每种基本数据类型都有其对应的包装类型Number(6种)、Character、Boolean；
### 流程控制
> 和其他语言一样分成顺序、选择和循环结构。
- 选择：if-else、switch-case；
- 循环：do-while、while、for、foreach；
### 面向对象
> 面向过程和面向对象
- 面向过程：强调功能本身，关注的是如何分步的去完成功能，将方法或函数看做是一个基本单位；此时的程序编写者就是一个执行者；
- 面向对象：强调具备该功能的对象，关注的是如何指挥对象去完成功能，将类或者对象看做是一个基本单位；此时的程序编写者就是一个指挥者；
- 面向对象编程：就是找寻对象、创建对象、使用对象、维护对象间关系的过程；

> 类和对象
- 类：将现实世界中的事物在概念世界的抽象；反映到Java语言中就是一个类或者接口。
- 对象：就是那个事物本身，是个实实在在的个体；反映到Java语言中就是类的实例或者接口的具体实现。
- Java中类的组成：类中有属性（Field）、方法（Method）、构造器（Constructor）基础元素，还可以有内部类元素。

> 方法
- 方法可以说就是一个最小的封装单位，通常由方法名、参数列表、方法体、返回值类型和访问权限修饰符组成；
- 方法重载：一个类中出现方法名相同、参数列表不同的现象；
- 方法重写：子父类中出现一模一样的方法的现象（即方法名、参数列表和返回值都一样）；
- 重写的方法不能比父类中的方法的出现更多的异常；也不能比父类中的方法的访问权限更严格；静态方法不能重写；

### 面向对象的三大特性
> 封装（encapsulation）：隐藏对象的属性和实现细节，并提供对外的访问方式；具体就是使用private私有属性并提供setter和getter方法。
- 不可见性：细节不可见；
- 安全性：避免了非法数据的产生；
- 复用性：不论内部如何变，类始终不变；

> 继承（inheritance）：将多个类中的公共部分提取到一个单独的类中，让这个类和多个类产生了“is a”的关系，将这种关系称之为继承；使用extends表示类或者接口之间的继承关系。
- 父类也称之为基类、超类（superclass）；子类也称之为派生类（subclass）；
- 一个类有且只能有一个父类；一个父类可以有多个子类（单继承）；
- 如果B继承A，C继承B，那么C也间接继承了A（多层继承），java.lang.Object类是继承体系中的根父类；
- 可以继承父类中出构造外所有的结构，但因为封装的不可见性，则不能直接访问私有成员；
- 子类实例化必先进行父类的初始化，就是说子类构造器中的必然有一个构造器的第一行是super()；
- 继承减少了代码的冗余，提高了代码复用性；

> 多态（polymorphism）：同一对象在运行时的不同表现形态；表现形式是父类引用指向子类对象。
- 前提：存在继承或实现关系；有方法的重写或方法的实现；
- 对属性来说是不存在多态现象的，就是说成员变量看左边；
- 对方法来说：编译时看父类的引用，运行时看具体的子类实例（动态绑定）；
- 父类不能使用子类中的成员，若想使用则需要向下转型(instanceof)；
- 当使用子类重写的方法而不是父类方法的现象称为虚拟方法调用，此时父类中的那个方法称之为虚拟方法（virtual method）；

> 抽象类与接口
- 当不要类创建对象时，我们可以使用abstract修饰类，这样的类就是一个抽象类；当不确定方法的具体实现时，可以使用abstract修饰，这样的方法称之为抽象方法；有抽象方法的类必须是抽象类，但抽象类不一定有抽象方法（模板设计模式）；
- 接口是类并列的一个结构，使用interface定义；JDK1.8之前，接口中只能有全局常亮和抽象方法；JDK1.8开始可以有静态方法和默认方法且默认方法只能通过接口调用；当默认方法和继承的父类方法一模一样时考虑类优先原则；
- 抽象类中通常定义基本功能，接口中定义扩展功能；接口反映的是“like a”的关系，是一种规则，一种可能性；

> this、super、static、final
- this：当前对象的引用（准确说是this所在的方法的所在类的对象）；
- super：父类对象的引用，和this一样都能用来区分同名成员；
- static：静态，用此修饰的成员变量称之为类变量，同样的用此修饰的成员方法称之为静态方法（类方法）；
- final：提升变量为常量（只能赋值1次）；修饰类则该类不能被继承；修改方法则该方法不能被重写；

### 异常
> 异常就是在程序运行过程中出现语法和逻辑错误之外的其他不正常情况的现象；有抛出和捕捉两种处理方式；
- 在异常的继承体系中，Throwable是Exception和Error的顶级父类，在Exception下有一个特殊的RuntimeException子类；
- 适当的处理异常能提高程序的健壮性和用户体验；

> try-catch-finally、throws、throw
- try-catch-finally：捕获并处理异常；try块中是可能出现问题的代码；chatch块中是对问题的解决方式的代码；finally块中是在虚拟机不退出的情况下都会执行的代码；
- throws：抛出一个或多个异常类型，提示方法调用者该方法可能会出现问题；
- throw：手动抛出一个异常对象，如该对象不是RuntimeException类型的子类，则需要显式throws声明；

> 自定义异常
- 当我们明确的知道如何解决问题时，通常自定义异常类继承Exception，然后throws该异常;
- 当不知道如何解决时，通常自定义类继承RuntimeException，然后好手动抛出一个改异常对象；

### 常用类与方法
> java.lang.Object
- clone()、finalize()、hashCode()、equals()、toString()、wait()、notify()、notifyAll()

> java.lang.Runtime（一个典型的单例设计模式）
- getRuntime()、gc()、maxMemory()、totalMemory

> java.lang.System
- currentTimeMillis()、nanoTime()、arraycopy()、exit(0)、gc()、getProperties()

> java.lang.String
- concat()、join()、split()、subString()
- replace()、replaceFirst()、replaceAll()、compareTo()、equals()、equalsIgnoreCase()、matches()
- length()、charAt()、contains()、indexOf()、lastIndexOf()、startsWith()、endsWith()
- valueOf()、toCharArray()、getBytes()、format()、toUpperCase()、toLowerCase()
- trim()、isEmpty()、intern()
- new String(byte[])、new String(char[])、new String(StringBuilder)

> java.lang.StringBuffer与java.lang.StringBuilder
- append()、insert()、delete()、deleteCharAt()、setCharAt()、replace()、length()、charAt()、indexOf()、lastIndexOf()、toString()、subString()
- 默认容量16，扩容2*16+2

> java.lang.Math
- Math.PI、Math.E
- abs()、ceil()、floor()、round()、max()、min()、random()

> java.util.Arrays
- asList()、sort()、binarySearch()、copyOf()、equals()、toString()、fill()

> java.util.Date与java.sql.Date
- getTime()、setTime()、after()、before()、new Date()、new Date(long)
- new Date(long)、valueOf()

> java.util.Calendar
- getInstance()、get()set()、add()、before()、after()、常量

> java.text.SimpleDateformat
- new SimpleDateFormat(String)、format()、parse()

> java.time.LocalDateTime
- now()、of()、getXxx()、plusXxx()、minusXxx()、withXxx()

> java.time.Instant
- now()、plus()、minus()

> java.time.format.DateTimeFormatter
- DateTimeFormatter.ISO_LOCAL_DATE_TIME
- ofPattern(String)、format()、parse()

> java.lang.Integer
- valueOf()、compare()、compareTo()、parseInt()、xxxValue()、toString()

> java.math.BigDecimal
- divide(BigDecimal divisor, int scale, int roundingMode)

> java.security,].MessageDigest(md5,sha-256)
- getInstance(String algorithm)、digest(byte[])

> java.util.Base64
- getDecode()、getEncode()
- decode()、encode()

### 线程
> 程序、进程和线程
- 程序：为实现某一功能，使用编程语言编写的一段静态代码或者是一组指令的集合；
- 进程：运行起来的程序，是资源分配的基本单位；
- 线程：程序中的一条执行路径，是程序执行的基本单位；
- 如果一个程序在同一时间可以并行的执行多个线程，那个称该程序为多线程程序；

> 创建线程的方式
- extends Thread：覆盖run()
- implements Runbale：覆盖run()
- implements Callable->FutureTask：覆盖call()
- Executors->ThreadPoolExecutor->submit()/execute()/shutdown()

> 线程中常用方法
- currentThread()、getName()、setName()、setPriority()、setDaemon()
- start()、sleep()、join()、yield()、interrupt()

> 线程状态
- 新建（new）
- 就绪（start、yield、notify）
- 运行（cpu分配了执行权）
- 阻塞（sleep、join、等待同步锁、wait）
- 死亡（run结束、出现异常、interrupt）

> 出现多线程问题的原因
- 多线程环境、有共享数据且有多条语句操作共享数据

> 线程同步
- syncronized同步代码块：锁对象任意，多个线程共用同一把锁；
- syncnizzed同步方法：成员方法锁为this、类方法锁为clazz；
- reentrantLock锁：lock(),unlock()
- lock.newCondition()-> await(),signal()

> 线程通信
- wait、notify
- await、signal

### 注解和枚举
> 注解可以理解为源代码的补充代码，与反射技术配合使用
- @interface：定义注解
- 元注解：（描述注解的注解）  
  target：定义使用注解的元素类型，使用ElementType枚举类  
  retention：定义注解的生命周期，使用RetentionPolicy枚举类   
  inherited：
  document：
  repeatable：

> 枚举类就是定义有限个公共的全局对象常量的类。
- enum定义枚举类，最直接父类是Enum；当一个枚举类中只有一个常亮时该枚举类就是一个单例的；

### 集合
> Collection接口为单列集合的根接口，其子接口有List与Set。

> List接口存储有序，元素可重复；实现类有ArrayList、LinkedList、Vector、Stack。
- ArrayList和Vector底层数据结构是数组，数组默认长度是10，扩容方式前者1.5倍后者2倍；查询快，插入和删除慢；
- LinkedList：底层数据结果是双向列表，查询慢，插入和删除快；

> Set接口存储无序，元素不可重复；实现类有HashSet、LinkedHashSet、TreeSet。
- HashSet：底层使用HashMap，即数组+链表+红黑树(1.8)的结构,默认容量16，加载因子0.75，临时容量12；当链表长度超过8，容量超过64则转换为红黑树存储。大概原理取得元素hashCode，通过散列算法计算元素位置；当该位置无元素直接添加，如果有元素那么要判断hashcode和equals的结果，hashcode和equals完全一样则不能添加；
- LinkedHashSet：底层在HashSet的基础上又多了一个双向链表来记录存储数据的顺序，所有取出和存储顺序是一致的；
- TreeSet：底层是红黑树，通过compareTo()或者compare()方法来确定元素位置。

> Collection接口方法
- add、addAll、clear、size、isEmpty、hashcode
- remove、removeALl、contains、containsAll、retainALl、equals
- toArray，iterator

> ArrayList特有方法
- add(int,E)、get(int)、set(int,E)、remove(int)、subList(int,int)

> Map是双列集合的根接口，其实现类有HashMap、LinkedHashMap、TreeMap、Hashtable、Propeties
- 以键值对的方式存储元素，先将键值对存储到entry中然后将entry存到Map中；
- 存储无序，元素不可重复，因为key是Set、value是Collection；

> Map的方法
- put、get、values、keySet、entrySet、size、isEmpty、remove、containsKey、containsValue、clear

### 泛型
> 泛型是参数化类型，就是类型的标记；实例化或者调用方法时需要明确具体类型；
- 可以用在类、接口、方法上；泛型方法可以是静态的；泛型方法的泛型类型不需要和类或接口中的泛型一致；
- 通配符：？，？extends G，？super G；不能做写操作，但可以做读操作；

### 文件和IO流
> 将文件系统抽象成File类，通过该类可以进行文件或目录的创建、删除、属性查看、目录内容查看等操作。

> java.io.File中的常用方法
- File.separator
- isFile()、isDirectory()、exists()、canRead()、canWrite()、isHidden()
- createNewFile()、mkdirs()、delete()、getParent() 、getName()、getAbsolutePath()
- list()、listFiles()、lastModified()、length()

> io流分类
- 按数据单位分为字节流和字符流
- 按流的流向分为输入流和输出流
- 按流的角色分为节点流（直接作用于文件的流）和处理流（对文件流或其他流的处理）

> 

### 网络编程

### 反射

### 正则表达式

### Java8函数式编程

