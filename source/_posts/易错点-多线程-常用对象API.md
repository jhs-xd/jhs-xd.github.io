---
title: 易错点-多线程+常用对象API
copyright: true
categories: 
  - 我的错题库
  - 牛客错题整理

---
**多线程；String类、StringBuffer类、StringBuilder类；数组；基本数据类型包装类；集合类；其他对象API（System类、Runtime类、Math类、Date类、Calendar类）**
![](/myimages/4.jpg)

<!--more-->

---

# 多线程 #
创建并启动线程的过程为：定义线程——>实例化线程——>启动线程
1. 定义线程：
（1）扩展java.lang.Thread类
（2）实现java.lang.Runnable接口
2. 实例化线程：
（1）如果是扩展java.lang.Thread类的线程，则直接new即可。
（2）如果是实现了java.lang.Runnable接口的类，则用Thread的构造方法：


		Thread(Runnable target)  eg: new Thread(new MyRunnable()).start()
		Thread(Runnable target, String name) 
		Thread(ThreadGroup group, Runnable target) 
		Thread(ThreadGroup group, Runnable target, String name) 
		Thread(ThreadGroup group, Runnable target, String name, long stackSize)

3. 启动线程：
在线程的Thread对象上调用start()方法，而不是run()或者别的方法


	1 run()方法用来执行线程体中具体的内容
	  start()方法用来启动线程对象，使其进入就绪状态
	  sleep()方法用来使线程进入睡眠状态
	  suspend()方法用来使线程挂起，要通过resume()方法使其重新启动

	2 线程停止的三种方式
	  （1）调用stop()
	  （2）异常抛出
	  （3）线程执行完毕

	3 锁：函数使用的锁是this(即对象本身)，若函数被static修饰则锁为 类名.class

	4 ConcurrentHashMap使用ReentrantLock来保证线程安全。
	  （1）hashMap在单线程中使用大大提高效率，在多线程的情况下使用hashTable来确保安全。
	  （2）hashTable中使用synchronized关键字来实现安全机制，但是synchronized是对整张hash表
	  进行锁定即让线程独享整张hash表，在安全同时造成了浪费。
	  （3）concurrentHashMap采用分段加锁的机制来确保安全

	5 Java并发
	  （1）CopyOnWriteArrayList适合使用在读操作远远大于写操作的场景里，比如缓存。
	  （2）ReadWriteLock当写操作时，其他线程无法读取或写入数据，
	  而当读操作时，其它线程无法写入数据，但却可以读取数据 。适用于读取远远大于写入的操作。
	  （3）ConcurrentHashMap是一个线程安全的HashMap，它的主要功能是提供了一组和HashMap
	  功能相同但是线程安全的方法。ConcurrentHashMap可以做到读取数据不加锁，不用对整个ConcurrentHashMap加锁

	6 线程调度算法是平台独立的

	7 前台线程与后台线程
	  （1）jre 判断程序是否执行结束的标准是所有的前台线程执行完毕。 属于某个进程的所有前台线程都终止后，该进程就会被终止。
	  （2）可以在任何时候将前台线程修改为后台线程，方式是设置Thread.IsBackground 属性。
	  （3）不管是前台线程还是后台线程，如果线程内出现了异常，都会导致进程的终止。
	  （4）托管线程池中的线程都是后台线程，使用new Thread方式创建的线程默认都是前台线程。

	  说明：一般后台线程用于处理时间较短的任务，如在一个Web服务器中可以利用后台线程来处理客户端发过来的请求信息。
      而前台线程一般用于处理需要长时间等待的任务，如在Web服务器中的监听客户端请求的程序，或是定时对某些系统资源进行扫描的程序

	8 截止JDK1.8版本,java并发框架支持锁包括：读写锁、自旋锁、乐观锁

	9 同步器是一些使线程能够等待另一个线程的对象，允许它们协调动作。最常用的同步器是CountDownLatch和Semaphore，不常用的是Barrier 和Exchanger

	10 ThreadLocal
	  （1）ThreadLocal用于创建线程的本地变量，该变量是线程之间不共享的
	  （2）ThreadLocal是采用哈希表的方式来为每个线程都提供一个变量的副本
	  （3）ThreadLocal保证各个线程间数据安全，每个线程的数据不会被另外线程访问和破坏
	  ThreadLocal的作用是提供线程内的局部变量（类似于private static的作用，但是多个线程之间数据又不共享），这种变量在线程的生命周期内起作用，
	  减少同一个线程内多个函数或者组件之间一些公共变量的传递的复杂度

---

# String类、StringBuffer类、StringBuilder类 #
	1 string是值传递，stringbuffer是引用传递

	2 String a="Hello";
	  String b=a.substring(0,2);
	  //b的值为He，substring( )方法截取的字符串“左闭右开”

	3 String类默认是final类型的，不能继承和修改这个类（String属于值传递）

	4 String s =new String(“xyz”);创建了几个string object？
	  答：两个， 第一个在堆中，第二个在字符串常量池中！如果在Java字符串常量池中已经存在，就只会创建一个

---

# 数组 #
	1 判断：int a[][]=new int[][]        （×）
	  解析：二维数组的声明必须指定第一个维度的初始值

	2 在java 中，声明一个数组时，不能直接限定数组长度，只有在创建实例化对象时，才能对给定数组长度

	3 判断：Java实现了真数组，避免了覆盖数据类型的可能   （×）
	  解析：Java实现了真数组，避免了数据覆盖的可能，而不是数据覆盖类型

---

# 基本数据类型包装类 #
	1 ==和.equals()运算
	  （1）基本型和基本型封装型进行“==”运算符的比较，基本型封装型将会自动拆箱变为基本型后再进行比较
	  （2） 两个Integer类型（不是new出来的）进行“==”比较，如果其值在-128至127，那么返回true，否则（会new对象，然后比较地址值）返回false
	  （3） 两个基本型的封装型进行equals()比较，首先equals()会比较类型，如果类型相同，则继续比较值，如果值也相同，返回true
	  （4） 基本型封装类型调用equals()，但是参数是基本类型，这时候，先会进行自动装箱，基本型转换为其封装类型，再进行（3）中的比较

	2 可以把任何一种数据类型的变量赋给Object类型的变量。其中，基本数据类型会自动装箱

	3 Java中的byte，short，char进行计算时都会先提升为int类型
	  注意：没有final修饰的变量相加后，才会被自动提升为int型；
	  	 而被fianl修饰的变量不会自动改变类型，当两个final修饰的变量相操作时，结果会根据左边变量的类型而转化

	4 instanceof：可以用来判断某个实例变量是否属于某种类的类型，还可以判断某个类是否属于某个类的子类的类型

---

# 集合类 #

## 集合框架、Collection、Iterator ##

## List ##
1. List集合有序（添加顺序）且可重复
2. List继承自Collection接口，List也是一个接口。
3. ArrayList、LinkedList、Vector类实现了List接口。用的时候一般都用ArrayList
4. ArrayList、LinkedList的选择：
   （1）对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针；
   对于新增和删除操作add和remove，LinkedList比较占优势，因为ArrayList要移动数据
   （2）当操作是在一列数据的后面添加数据而不是在前面或中间，并且需要随机地访问其中的元素时，使用ArrayList会提供比较好的性能；
   当你的操作是在一列数据的前面或中间添加或删除数据，并且按照顺序访问其中的元素时，就应该使用LinkedList了
5. ArrayList是实现List接口的大小可变数组
   ArrayList删除元素后，剩余元素会依次向前移动，因此下标一直在变，size()也会减小

## Set ##
1. List一定有序，Set不一定无序
   我们经常听说List是有序且重复的，Set是无序不重复的。这里有个误区，这里说的顺序有两个概念，一是按添加的顺序排列，二是按自然顺序a-z排列。 
   Set并不是无序的，传统说法中的Set无序是指HashSet,它不能保证元素的添加顺序，更不能保证自然顺序。
   而Set的其他实现类是可以实现这两种顺序的（TreeSet可以实现自然顺序有序，LinkedHashSet可以实现添加顺序有序）
2. AbstractSet类实现了Set接口
   HashSet继承了AbstractSet类，所以相当于也实现了Set接口（Java中子类会继承父类对于接口的实现）
3. ResultSet跟普通的数组不同，索引从1开始而不是从0开始

## Map ##
	1 key-value对，键值对，相同键值会放在一处，后一个会覆盖前一个的数值

	2 HashMap可以插入null的key或value
	  插入的时候，检查是否已经存在相同的key，如果不存在，则直接插入；
	  如果存在，则用新的value替换旧的value

	3 在Java中，HashMap中是用链地址法来解决哈希冲突的

	4 判断：HashTable使用Enumeration，HashMap使用Iterator   （√） 
	  解析：Hashtable、HashMap都使用了Iterator。而由于历史原因，Hashtable还使用了Enumeration的方式

	5 判断：Hashtable直接使用对象的hashCode，HashMap重新计算hash值，而且用与代替求模 （√）

## 泛型 ##

## 集合框架工具类（Collections、Arrays） ##
	1 Arrays.asList()
	  （1）将一个数组转化为一个List对象，这个方法会返回一个ArrayList类型的对象
	  但这个ArrayList类并非java.util.ArrayList类，而是Arrays类的静态内部类！
	  （2）用这个对象对列表进行添加删除更新操作，就会报UnsupportedOperationException异常

---

# 其他对象API #


## System类 ##
	1 判断：我们在程序中经常使用“System.out.println()”来输出信息，语句中的System是包名，out是类名，println是方法名 （×）
	  解析：System是java.lang中的一个类，out是System内的一个成员变量，
	  这个变量是一个java.io.PrintStream类的对象，println()是它的一个方法


## Runtime类 ##


## Number类 ##
	1 Number类可以被继承，Integer，Float，Double等都继承自Number类


## Math类 ##
	1 ceil()/floor()
	  （1）Math.ceil(d1)  //如果参数小于0且大于-1.0，结果为-0
	  （2）Math.floor(d1)  //如果参数是无穷、正0、负0，那么结果与参数相同（如果是-0.0，那么其结果是-0.0）


## Date类 ##


## Calendar类 ##


