

[TOC]



# 1.内存模型



## 1.1内存模型概述

运行时内存模型，分为线程私有和共享数据区两大类，其中线程私有的数据区包含程序计数器、虚拟机栈、本地方法区，所有线程共享的数据区包含Java堆、方法区，在方法区内有一个常量池。



![jvm_memory_1](http://gityuan.com/images/jvm/jvm_memory_1.png)

（1）线程私有区：

- 程序计数器，记录正在执行的虚拟机字节码的地址；
- 虚拟机栈：方法执行的内存区，每个方法执行时会在虚拟机栈中创建栈帧；
- 本地方法栈：虚拟机的Native方法执行的内存区；



（2）线程共享区：

- Java堆：对象分配内存的区域；
- 方法区：存放类信息、常量、静态变量、编译器编译后的代码等数据；
  - 常量池：存放编译器生成的各种字面量和符号引用，是方法区的一部分。



## 1.2 详细模型

运行时内存分为五大块区域（常量池属于方法区，算作一块区域），前面简要介绍了每个区域的功能，那接下来再详细说明每个区域的内容，Java内存总体结构图如下：

![stack_heap_info](http://gityuan.com/images/jvm/stack_heap_info.png)

### 1.2.1 程序计数器

程序计数器PC，当前线程所执行的字节码行号指示器。每个线程都有自己计数器，是私有内存空间，该区域是整个内存中较小的一块。

当线程正在执行一个Java方法时，PC计数器记录的是正在执行的虚拟机字节码的地址；当线程正在执行的一个Native方法时，PC计数器则为空（Undefined）。



### 1.2.2 虚拟机栈

虚拟机栈，生命周期与线程相同，是Java方法执行的内存模型。每个方法(不包含native方法)执行的同时都会创建一个栈帧结构，方法执行过程，对应着虚拟机栈的入栈到出栈的过程。

**栈帧(Stack Frame)结构**

栈帧是用于支持虚拟机进行方法执行的数据结构，是属性运行时数据区的虚拟机站的栈元素。见上图， 栈帧包括：

1. 局部变量表 (locals大小，编译期确定)，一组变量存储空间， 容量以slot为最小单位。
2. 操作栈(stack大小，编译期确定)，操作栈元素的数据类型必须与字节码指令序列严格匹配
3. 动态连接， 指向运行时常量池中该栈帧所属方法的引用，为了动态连接使用。
   - 前面的解析过程其实是静态解析；
   - 对于运行期转化为直接引用，称为动态解析。
4. 方法返回地址
   - 正常退出，执行引擎遇到方法返回的字节码，将返回值传递给调用者
   - 异常退出，遇到Exception,并且方法未捕捉异常，那么不会有任何返回值。
5. 额外附加信息，虚拟机规范没有明确规定，由具体虚拟机实现。

因此，一个栈帧的大小不会受到

**异常(Exception)**

Java虚拟机规范规定该区域有两种异常：

- StackOverFlowError：当线程请求栈深度超出虚拟机栈所允许的深度时抛出
- OutOfMemoryError：当Java虚拟机动态扩展到无法申请足够内存时抛出



### 1.2.3 本地方法栈

本地方法栈则为虚拟机使用到的Native方法提供内存空间，而前面讲的虚拟机栈式为Java方法提供内存空间。有些虚拟机的实现直接把本地方法栈和虚拟机栈合二为一，比如非常典型的Sun HotSpot虚拟机。

**异常(Exception)**：Java虚拟机规范规定该区域可抛出StackOverFlowError和OutOfMemoryError。



### 1.2.4 堆

Java堆，是Java虚拟机管理的最大的一块内存，也是GC的主战场，里面存放的是几乎所有的对象实例和数组数据。JIT编译器有栈上分配、标量替换等优化技术的实现导致部分对象实例数据不存在Java堆，而是栈内存。

- 从内存回收角度，Java堆被分为新生代和老年代；这样划分的好处是为了更快的回收内存；
- 从内存分配角度，Java堆可以划分出线程私有的分配缓冲区(Thread Local Allocation Buffer,TLAB)；这样划分的好处是为了更快的分配内存；

对象创建的过程是在堆上分配着实例对象，那么对象实例的具体结构如下：

![java_object](http://gityuan.com/images/jvm/java_object.png)

对于填充数据不是一定存在的，仅仅是为了字节对齐。HotSpot VM的自动内存管理要求对象起始地址必须是8字节的整数倍。对象头本身是8的倍数，当对象的实例数据不是8的倍数，便需要填充数据来保证8字节的对齐。该功能类似于高速缓存行的对齐。

另外，关于在堆上内存分配是并发进行的，虚拟机采用CAS加失败重试保证原子操作，或者是采用每个线程预先分配TLAB内存.

**异常(Exception)**：Java虚拟机规范规定该区域可抛出OutOfMemoryError。



### 1.2.5 方法区

方法区主要存放的是已被虚拟机加载的类信息、常量、静态变量、编译器编译后的代码等数据。GC在该区域出现的比较少。

**异常(Exception)**：Java虚拟机规范规定该区域可抛出OutOfMemoryError。



### 1.2.6 运行时常量池

运行时常量池也是方法区的一部分，用于存放编译器生成的各种字面量和符号引用。运行时常量池除了编译期产生的Class文件的常量池，还可以在运行期间，将新的常量加入常量池，比较常见的是String类的intern()方法。

- 字面量：与Java语言层面的常量概念相近，包含文本字符串、声明为final的常量值等。
- 符号引用：编译语言层面的概念，包括以下3类：
  - 类和接口的全限定名
  - 字段的名称和描述符
  - 方法的名称和描述符

但是该区域不会抛出OutOfMemoryError异常。





# 2.  内存回收机制



## 2.0 预备知识

###2.0.1垃圾对象的判定方法：



- 引用计数算法

  算法分析　

  　　引用计数是垃圾收集器中的早期策略。在这种方法中，堆中每个对象实例都有一个引用计数。当一个对象被创建时，且将该对象实例分配给一个变量，该变量计数设置为1。当任何其它变量被赋值为这个对象的引用时，计数加1（a = b,则b引用的对象实例的计数器+1），但当一个对象实例的某个引用超过了生命周期或者被设置为一个新值时，对象实例的引用计数器减1。任何引用计数器为0的对象实例可以被当作垃圾收集。当一个对象实例被垃圾收集时，它引用的任何对象实例的引用计数器减1。

  优缺点

  优点：

  　　引用计数收集器可以很快的执行，交织在程序运行中。对程序需要不被长时间打断的实时环境比较有利。

  缺点： 

  　　无法检测出循环引用。如父对象有一个对子对象的引用，子对象反过来引用父对象。这样，他们的引用计数永远不可能为0.

  ​

- 根搜索算法

  ![img](https://images0.cnblogs.com/blog2015/694841/201506/141050566294022.jpg)

  　　根搜索算法是从离散数学中的图论引入的，程序把所有的引用关系看作一张图，从一个节点GC ROOT开始，寻找对应的引用节点，找到这个节点以后，继续寻找这个节点的引用节点，当所有的引用节点寻找完毕之后，剩余的节点则被认为是没有被引用到的节点，即无用的节点。

  java中可作为GC Root的对象有

  　　1.虚拟机栈中引用的对象（本地变量表）

  　　2.方法区中静态属性引用的对象

    　　3. 方法区中常量引用的对象

  　　4.本地方法栈中引用的对象（Native对象）



### 2.0.2 垃圾收集算法



- 标记-清除算法 (Mar	k-Sweep)

标记-清除算法将垃圾回收分为两个阶段：标记阶段和清除阶段。一种可行的实现是，在标记阶段首先通过根节点，标记所有从根节点开始的较大对象。因此，未被标记的对象就是未被引用的垃圾对象。然后，在清除阶段，清除所有未被标记的对象。该算法最大的问题是存在大量的空间碎片，因为回收后的空间是不连续的。在对象的堆空间分配过程中，尤其是大对象的内存分配，不连续的内存空间的工作效率要低于连续的空间。



- 复制算法 (Copying)

将现有的内存空间分为两快，每次只使用其中一块，在垃圾回收时将正在使用的内存中的存活对象复制到未被使用的内存块中，之后，清除正在使用的内存块中的所有对象，交换两个内存的角色，完成垃圾回收。

如果系统中的垃圾对象很多，复制算法需要复制的存活对象数量并不会太大。因此在真正需要垃圾回收的时刻，复制算法的效率是很高的。又由于对象在垃圾回收过程中统一被复制到新的内存空间中，因此，可确保回收后的内存空间是没有碎片的。该算法的缺点是将系统内存折半。

Java 的新生代串行垃圾回收器中使用了复制算法的思想。新生代分为 eden 空间、from 空间、to 空间 3 个部分。其中 from 空间和 to 空间可以视为用于复制的两块大小相同、地位相等，且可进行角色互换的空间块。from 和 to 空间也称为 survivor 空间，即幸存者空间，用于存放未被回收的对象。

在垃圾回收时，eden 空间中的存活对象会被复制到未使用的 survivor 空间中 (假设是 to)，正在使用的 survivor 空间 (假设是 from) 中的年轻对象也会被复制到 to 空间中 (大对象，或者老年对象会直接进入老年带，如果 to 空间已满，则对象也会直接进入老年代)。此时，eden 空间和 from 空间中的剩余对象就是垃圾对象，可以直接清空，to 空间则存放此次回收后的存活对象。这种改进的复制算法既保证了空间的连续性，又避免了大量的内存空间浪费。



## 2.1 什么时间

eden满了minor gc，升到老年代的对象大于老年代剩余空间full gc，或者小于时被HandlePromotionFailure参数强制full gc；System.gc()被显示调用 ；gc与非gc时间耗时超过了GCTimeRatio的限制引发OOM，调优诸如通过NewRatio控制新生代老年代比例，通过MaxTenuringThreshold控制进入老年前生存次数



## 2. 2 对什么东西

从root搜索不到（根路径搜索算法），而且经过第一次标记、清理后，仍然没有复活的对象



## 2.3 做了什么事

新生代进行复制清理，老年代进行标记清理。