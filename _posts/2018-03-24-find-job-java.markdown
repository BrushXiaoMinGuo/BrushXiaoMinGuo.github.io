---
layout:       post
title:        "Java 语言"
subtitle:     "Java"
date:         2018-03-24
author:       "Gxm"
header-img:   "img/post-bg-2015.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - 码农的尊严
---

### 1.基本概念
#### 1.heap和stack的区别
heap是堆，stack是栈；stack空间由操作系统分配和释放，stack则是由用户分配和释放(使用new分配、由java虚拟机回收)； stack空间有限，heap空间则是比较大的。
#### 2.Java的反射机制
反射在程序运行期间，根据类或者对象来获得类的属性、方法。目的是为了增加java的灵活性，通过调用类的Class对象来获得。一个场景，比如：卖水果的商店，每天要进水果，但是水果的类是动态调整的，通过配置文件或者数组来传递进去水果类来获得信息。
#### 3.什么是ACID
原子性、一致性、隔离性、持久性
#### 4.fail-fast 与fail-safe的区别
快速失败和安全失败。
#### 5.Interface和abstract区别
Interface中都是抽象方法。没有方法的实现       
abstract是抽象类，不能实例化对象，可以不包含抽象方法；但包含抽象方法的一定是抽象类。
#### 6.Java类加载器
解决的问题java的灵活性，比如用接口需要在运行期间确定类的信息。java虚拟机将.java文件编译为.class文件。虚拟机使用的是.class文件。在方法区创建静态信息，在堆区创建类的class对象。类加载的过程是加载->连接(验证、准备、解析)->初始化->使用->卸载。     
类加载的机制是双亲委派机制（老子最大机制），首先findloadclass看是否已经加载，没有加载先调用父亲loadclass的加载，如果父亲不能加载。在findclass加载自己的。根据类名加载二进制文件。
#### 7.为什么Java是平台无关的
因为每个平台都有Java虚拟机


### 2.关键字
#### 1.finalize方法是什么、什么时候调用、和析构函数finalization的区别是什么             
finalize方法告诉jvm虚拟机在类被回收的时候该做什么；会在类被垃圾回收器永久回收前调用对象的finalize方法。一般情况下是不需要析构方法finalization的。但是当调用领导native方法的时候就需要调用析构函数
#### 2.final、static的区别
final修饰类指定该类不能被继承；修饰方法，指定方法不能被重载；修饰变量，表示变量不能被修改，是常量；使用final修饰方法的时候，方法会在编译期间嵌入到代码块，加快执行效率。但是如果方法太大的话影响性能。
static表示的全景变量，类变量；修饰方法是类方法，在不实例化对象的时候可以使用；static代码块会在类加载的时候被执行。
#### 3.final、finalize、finally的区别？final修饰引用
final修饰引用的时候，引用不能变，但是引用的对象可以变。
#### 4.atonic
使用atmonic是为了保证多线程环境下的变量共享，使用算法（lock-free）来替代锁。对于原子类的操作保证原子性，在变量没有修改完的情况下不允许其他线程来修改该变量,比如++操作。
#### 4.volatite变量？提供什么保证？能否创建volatile 数组   
voliate是synchronized的轻量级实现，提供了多线程共享变量的可见性(一个线程的修改对其他线程可见)，有序性（防止指令重排序），但不提供原子性。volatile修饰数组只是对数组的引用有volatile效果对数组中的值没有这个效果。
#### 5.synchronized是什么？和lock的区别
synchronized是lock的一个简化版本，锁的话锁一片，是java 的一个关键字。
lock可以更加细粒度的控制锁的范围，是一个类。
假设有一种情况，获取锁的线程，还需要等待IO等资源，而其他阻塞的线程只能干巴巴的等待，造成了浪费。所以使用lock可以更加精确的控制并发。
#### 6.ThreadLocal
线程局部变量，每个Thread有一个ThreadMap用来保存该变量和值。适合线程多实例的场景
#### 7.public,private,protected区别
puliblic所有都可以访问；private只有自己能访问；protected自己，子女，朋友访问。不写的话是默认friendly类。
#### 8.main方法为什么是静态的，能否是非静态的？
main方法是静态的，如果不是静态的就必须实例化这个类，这样容易有歧义。没有main方法是不行的，jvm找不到入口

### 3，操作符
#### 1.&与&&，|与||的区别
&，|位运算，按位与和按位或，没有短路功能
&&，||逻辑与和逻辑或。具有短路功能
#### 2.a=a+b 和 a+=b的区别
后者会做隐士的强制类型转换
#### 3.3 * 0.1 == 0.3 将会返回true 还是 false？
false，因为有些浮点数不能精确的表示出来
#### 4.float f=3.4 是否正确
不正确，因为java中带小数点的数默认是double的，double不能赋值给float。需要用强制类型转换（float）3.4 或者3.4f

### 4.数据结构
#### 1.基本数据类型和封装类型的区别
基本数据结构是值传递，封装类型是引用传递；     
基本数据结构在栈区，封装类型的引用在栈区，对象在堆区；     
#### 2.int和Integer的区别
int是基本类型，默认值0，Integer封装类型，默认值null；    
int和Integer用==比较是相等，Integer会自动拆箱；        
Integer和new Integer==比较是不相等的；     
两个非new出来的Integer，如果值在-128到127之间是相等的，使用的是缓存，不在这个范围内是不等的，是new出来的对象。
#### 3.数组有没有length()这个方法
数组没有这个方法，有这个属性
#### 4.String和StringBuffer
String不可以修改，重新赋值是生成新的对象；StringBuffer变量，可以修改其内容。

### 5.集合、容器
#### 1.BlockingQueue是什么
BlockingQueue是阻塞队列，有ArrayBlockingQueue(固定大小，数组实现)、LinkedBlockingQueue（链表实现）、PriorityBlockingQueue（只含有一个元素的队列）等。实现的原理是使用锁和设置锁条件。
#### 2.ArrayList、LinkedList、Vector、copyOnWriteList
ArrayList基于数组实现、不是线程安全的，扩容的时候每次扩50%。默认大小是10；    
LinkedList基于链表实现，不是线程安全的；     
Vector基于数组实现，加了Synchronized所以是线程安全的，每次扩容扩100%；      
copyOnWriteList基于写时复制的思想，线程可以并发的读容器，但是在写的时候，会复制一个旧的容器到新的容器，并加锁（不然会copy出N个副本），写完再将引用指向这个新的容器。场景是读多写少的场景。缺点：内存占用问题：因为同时存在旧的容器和新的容器，如果旧的容器占用太多的内存，会造成浪费，且会造成频繁的垃圾回收。数据一致性问题。不能保证数据的实时一致性问题。
#### 3.HashMap
HashMap有一个数组+链表组成。当来一个元素的时候，先根据hashcode函数算出桶的位置（索引的位置），当key相同的话根据equal方法来判定是否是同一个元素，如果不是插入到链表。    
hashmap的容量因子到了0.75就会调用rehash重新散步，将容量加倍。     
hashmap的keyset和entryset两种遍历方式。keyset相当于遍历了两遍，第一遍仅仅返回key，然后根据key去找value。而entryset则返回了key，value对。    
hastmap不是线程安全的；     
#### 4.HashMap和HashTable区别
Hashmap允许null值和value，但是hashTable不允许    
Hashmap不是是线程安全的，Hashtable是线程安全的     
hashmap的默认大小是16，每次rehash为原来的两倍，hashtable默认大小是11，
#### 5.Hashtable和ConcurrentHashMap
ConcurrentHashmap先分段，分段加锁。每一个段分别是一个hashtable结构。    
使用线程安全的还可以使用collections.synchronizedMap（） 
#### 6.hashset和hashmap
hashset实际是hashmap的封装，底层基于hashmap来实现。hashset不是线程安全的。    
调用线程安全的vertor或者collections.synchronizedlist。
#### 7.hashset和treeset区别
treeset是有序的或者是自然有序，或者自己实现collections排序。treeset基于二叉树的方式实现。      
#### 8.set使用什么方式判重复
使用equals而不是使用==
#### 9.treemap基于什么实现   
treemap基于红黑树实现    
#### 10.hashcode
hashcode主要用于散列，两个不相等的对象有可能hashcode相同，但是equal不同。重写equal的时候会重写hashcode，为的是加快效率。  
#### 11.linkedhashmap
hashmap的数据插入的无序的，linkedhashmap在hashmap的基础上又维护了一个双向链表来保证元素的有序性。    
#### 12.list
linkedList是双向链表。   
插入数据时候Linkedlist最快，其次是ArrayList最后是vector    
ArrayList默认容量是10，扩容的按照一倍的去扩容。    
vector底层是数组实现，默认容量是10，扩容的按照一倍。   

### 6.对象
#### 1.如何实现对象克隆，深拷贝和浅拷贝的区别
实现cloneable接口，重写clone方法；      
浅拷贝仅仅在重写的clone方法中调用了super。clone方法，深拷贝对属性也重写了clone方法，同时在对象的clone方法中调用了属性的clone方法。    
写clone时，通常有一行代码，这行代码是super.clone      
#### 2.==和equal
==比较引用，equal比较内容；    
当重写equal方法的时候，还需要重写hashcode方法。因为在equal方法中调用了hashcode方法。   
#### 3.什么是不可变对象
创建后内容不可变，可以用于缓存，节约内存。如string，Integer（-128到127）   

### 7.GC
#### 1.GC是什么，为什么GC，什么时候GC，GC如何运行
GC垃圾回收，Java的对象分配在堆中，有Jva虚拟机管理对象的垃圾回收，所以需要GC，不GC会造成内存泄漏。GC首先使用根引用链搜索算法，搜索时候还有引用，如果没有引用了。看对象有没有实现finalize方法或者调用过finalize方法。如果对象实现了finalize方法就去调用，如果调用finalize方法还没有救活自己就会回收这个对象。
#### 2.新老和永久区是什么
方法区主要存放类的加载信息，final变量，方法信息等，方法区是永久区；堆区分为新生代和老生代。新生代又分为Edgen和survivor。新生代的大小用Xmn控制，老生代的大小是Xmx和Xmn之差，-XX:SurvivorRatio来控制Eden和Survivor的比例旧生代。
#### 3.Minor GC,Major GC,Young GC,Full GC分别在什么时候发生。
Minor在新生代收集的时候，Major在老年代收集的时候，Full GC在收集整个堆空间（包含老年代和新生代）。
#### 4.垃圾回收算法原理
有标记清理、标记复制、标记整理
#### 5.JVM内存分区
方法区（永久区）分配类加载的信息，静态变量，常量，类的属性方法等信息，该区域线程共享的；    
虚拟机栈：每个方法调用的时候都会创建一个方法栈帧，该区域是线程私有的，生命周期和线程生命周期相同。    
本地方法栈：和虚拟机栈类似，但是调用native方法。
堆：线程共享，主要分配对象
### 8.多线程
#### 1.多线程的优缺点、实现方式
多线程开销下，共享变量；但是需要加锁等控制；     
可以继承Thread、或者实现Runnable重写里面的run方法。调用线程是start方法启动线程。或者实现callable接口，实现里面的call方法。   
用Runnable比Thread好，因为java的单继承，所以实现Runnable接口可以实现多继承。
#### 2.线程池
线程池基于对象重用的思想，使用executor来创建线程池。线程池的提交有submit和execute方法，submit有返回值，但是不可以抛出异常；execute能抛出异常，但是无返回值。


