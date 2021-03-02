## Java并发

## 2. 并发机制底层原理

### 2.1 volatile

volatile 命令作用 ；

1）将当前处理器缓存行的数据写回到系统内存。

2）这个写回内存的操作会使在其他CPU里缓存了该内存地址的数据无效。

如果对声明了volatile的变量进行写操作，JVM就会向处理器发送一条Lock前缀的指令，将这个变量所在缓存行的数据写回到系统内存。但是，就算写回到内存，如果其他处理器缓存的值还是旧的，再执行计算操作就会有问题。所以，在多处理器下，为了保证各个处理器的缓存是一致的，就会实现缓存一致性协议，每个处理器通过嗅探在总线上传播的数据来检查自己缓存的值是不是过期了，当处理器发现自己缓存行对应的内存地址被修改，就会将当前处理器的缓存行设置成无效状态，当处理器对这个数据进行修改操作的时候，会重新从系统内存中把数据读到处理器缓存里。

### 2.2 syncvhronized

在JDK 1.6 以后，进行了各种优化，synchronized 由重变轻

先来看下利用synchronized实现同步的基础：Java中的每一个对象都可以作为锁。具体表现为以下3种形式。

·对于普通同步方法，锁是当前实例对象。

·对于静态同步方法，锁是当前类的Class对象。

·对于同步方法块，锁是Synchonized括号里配置的对象。

```
从JVM规范中可以看到Synchonized在JVM里的实现原理，JVM基于进入和退出Monitor对象来实现方法同步和代码块同步，但两者的实现细节不一样。代码块同步是使用monitorenter和monitorexit指令实现的，而方法同步是使用另外一种方式实现的，细节在JVM规范里并没有详细说明。但是，方法的同步同样可以使用这两个指令来实现。

monitorenter指令是在编译后插入到同步代码块的开始位置，而monitorexit是插入到方法结束处和异常处，JVM要保证每个monitorenter必须有对应的monitorexit与之配对。任何对象都有一个monitor与之关联，当且一个monitor被持有后，它将处于锁定状态。线程执行到monitorenter指令时，将会尝试获取对象所对应的monitor的所有权，即尝试获得对象的锁。
```

方法同步和代码块同步  ————》 基于进入和退出Monitor对象——》具体可以使用 moniterenter 和 monitorexit ————》 在编译后插入到开始和结束的位置，根据指令获取或释放moniter的所有权

#### 2.2.1 java对象头

* 如果是非数组类型，对象头中有Mark Down （hashcode和锁信息） 和 Class Metadata Address （存储到对象类型数据的指针），如果是数组类型， 还有一个字段是数组长度

  每个字段都是1位（32/64bit）

* Mark Down

  * HashCode , 分带年龄和锁标记位，是否偏向锁
  * 锁又可以具体分为轻量级锁、重量级锁、偏向锁
  * 或者是GC标记

#### 2.2.2 锁的升级和对比

* 为了优化性能节省消耗----> 出现了偏向锁和轻量级锁

* 锁的状态： 无锁 -----> 偏向锁 -----> 轻量级锁---->重量级锁。锁的状态可生不可降，为了提高获取、释放锁的效率

* 偏向锁：

  ```
  大多数情况下，锁不仅不存在多线程竞争，而且总是由同一线程多次获得，为了让线程获得锁的代价更低而引入了偏向锁。当一个线程访问同步块并获取锁时，会在对象头和栈帧中的锁记录里存储锁偏向的线程ID，以后该线程在进入和退出同步块时不需要进行CAS操作来加锁和解锁，只需简单地测试一下对象头的Mark Word里是否存储着指向当前线程的偏向锁。如果测试成功，表示线程已经获得了锁。如果测试失败，则需要再测试一下Mark Word中偏向锁的标识是否设置成1（表示当前是偏向锁）：如果没有设置，则使用CAS竞争锁；如果设置了，则尝试使用CAS将对象头的偏向锁指向当前线程。
  ```

  

  * 撤销：

    ```
    偏向锁使用了一种等到竞争出现才释放锁的机制，所以当其他线程尝试竞争偏向锁时，持有偏向锁的线程才会释放锁。偏向锁的撤销，需要等待全局安全点（在这个时间点上没有正在执行的字节码）。它会首先暂停拥有偏向锁的线程，然后检查持有偏向锁的线程是否活着，如果线程不处于活动状态，则将对象头设置成无锁状态；如果线程仍然活着，拥有偏向锁的栈会被执行，遍历偏向对象的锁记录，栈中的锁记录和对象头的Mark Word要么重新偏向于其他线程，要么恢复到无锁或者标记对象不适合作为偏向锁，最后唤醒暂停的线程。
    ```

* 轻量级锁

  * 加锁

    ```
    线程在执行同步块之前，JVM会先在当前线程的栈桢中创建用于存储锁记录的空间，并将对象头中的Mark Word复制到锁记录中，官方称为Displaced Mark Word。然后线程尝试使用CAS将对象头中的Mark Word替换为指向锁记录的指针。如果成功，当前线程获得锁，如果失败，表示其他线程竞争锁，当前线程便尝试使用自旋来获取锁。
    ```

  * 解锁

    ```
    轻量级解锁时，会使用原子的CAS操作将Displaced Mark Word替换回到对象头，如果成功，则表示没有竞争发生。如果失败，表示当前锁存在竞争，锁就会膨胀成重量级锁。图2-2是两个线程同时争夺锁，导致锁膨胀的流程图。
    因为自旋会消耗CPU，为了避免无用的自旋（比如获得锁的线程被阻塞住了），一旦锁升级成重量级锁，就不会再恢复到轻量级锁状态。当锁处于这个状态下，其他线程试图获取锁时，都会被阻塞住，当持有锁的线程释放锁之后会唤醒这些线程，被唤醒的线程就会进行新一轮的夺锁之争。
    ```

  锁的对比：

  * 偏向锁：
    * 优点：加解锁无额外消耗
    * 如果有线程竞争，会带来额外的所撤销的消耗
    * 适用于一个线程访问的同步块
  * 轻量级锁：
    * 优点：竞争的线程不会阻塞
    * 缺点： 如果始终竞争不到，一直自选，会消耗CPU
    * 适用于追求相应时间，同步块执行速度很快
  * 偏向锁：
    * 优点：线程不使用自选，不会消耗CPU
    * 缺点：线程阻塞，相应时间慢
    * 适用于追求吞吐量，同步块执行速度较长
  
  #### 2.3 原子操作的实现原理
  
  * 实现原子操作的方法：
  
    1. 使用总线锁保证原子性
  
       * 处理器使用总线锁就是来解决这个问题的。所谓总线锁就是使用处理器提供的一个LOCK＃信号，当一个处理器在总线上输出此信号时，其他处理器的请求将被阻塞住，那么该处理器可以独占共享内存。
  
    2. 使用缓存锁保证原子性
  
       * ```
         所谓“缓存锁定”是指内存区域如果被缓存在处理器的缓存行中，并且在Lock操作期间被锁定，那么当它执行锁操作回写到内存时，处理器不在总线上声言LOCK＃信号，而是修改内部的内存地址，并允许它的缓存一致性机制来保证操作的原子性，因为缓存一致性机制会阻止同时修改由两个以上处理器缓存的内存区域数据，当其他处理器回写已被锁定的缓存行的数据时，会使缓存行无效
         ```
  
       * 两种情况不会使用缓存锁定：
  
         1. ```
            当操作的数据不能被缓存在处理器内部，或操作的数据跨多个缓存行（cache line）时，则处理器会调用总线锁定。
            ```
  
         2. ```
            有些处理器不支持缓存锁定
            ```
  
  * Java实现原子操作
  
    1. CAS： 自旋CAS实现的基本思路就是循环进行CAS操作直到成功为止
  
       ```
       从Java 1.5开始，JDK的并发包里提供了一些类来支持原子操作，如AtomicBoolean（用原子方式更新的boolean值）、AtomicInteger（用原子方式更新的int值）和AtomicLong（用原子方式更新的long值）。这些原子包装类还提供了有用的工具方法，比如以原子的方式将当前值自增1和自减1。
       ```
  
       * 会存在的三个问题：
         1. ABA问题：A和最终的A相同，但是实际经过了B的转换，所以会遗漏。解决方法：加上版本号。标记变化次数。比较变量的值和版本号是否符合预期。
         2. 循环时间长开销大：自旋CAS如果长时间不成功，会给CPU带来非常大的执行开销。如果JVM能支持处理器提供的pause指令，那么效率会有一定的提升。
         3. 只能保证一个共享变量的原子操作，如果是多个的话，就无法保证原子性，需要有锁。取巧的方法，多个变量拼接成一个，视为1个变量CAS
  
    2. 锁机制实现原子操作：
  
       ```
       锁机制保证了只有获得锁的线程才能够操作锁定的内存区域。JVM内部实现了很多种锁机制，有偏向锁、轻量级锁和互斥锁。有意思的是除了偏向锁，JVM实现锁的方式都用了循环CAS，即当一个线程想进入同步块的时候使用循环CAS的方式来获取锁，当它退出同步块的时候使用循环CAS释放锁。
       ```
  
       

## 3 Java并发机制的底层实现原理

### 3.1   Java内存模型基础

####  3.1.1 并发编程模型的两个关键问题

* 线程之间的通信机制：

  1. 共享内存：

     ```
     在共享内存的并发模型里，线程之间共享程序的公共状态，通过写-读内存中的公共状态进行隐式通信。
     ```

  2. 消息传递

     ```
     在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过发送消息来显式进行通信
     ```

* 同步是指程序中用于控制不同线程间操作发生相对顺序的机制。在共享内存并发模型里，同步是显式进行的。程序员必须显式指定某个方法或某段代码需要在线程之间互斥执行。在消息传递的并发模型里，由于消息的发送必须在消息的接收之前，因此同步是隐式进行的。

* Java的并发采用的是共享内存模型，Java线程之间的通信总是隐式进行，整个通信过程对程序员完全透明。如果编写多线程程序的Java程序员不理解隐式进行的线程之间通信的工作机制，很可能会遇到各种奇怪的内存可见性问题。

#### 3.1.2 JMM

一句话描述： 共享变量及其副本可能同时存在于不同线程的本地内存和主内存中，由于并发的特性，会导致副本之间的值不一致。

```
如果线程A与线程B之间要通信的话，必须要经历下面2个步骤。

1）线程A把本地内存A中更新过的共享变量刷新到主内存中去。

2）线程B到主内存中去读取线程A之前已更新过的共享变量。

从整体来看，这两个步骤实质上是线程A在向线程B发送消息，而且这个通信过程必须要经过主内存。JMM通过控制主内存与每个线程的本地内存之间的交互，来为Java程序员提供内存可见性保证。
```



#### 3.1.3 源代码到指定序列的重排序

在执行程序时，为了提高性能，编译器和处理器常常会对指令做重排序

```
1）编译器优化的重排序。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序。

2）指令级并行的重排序。现代处理器采用了指令级并行技术（Instruction-Level Parallelism，ILP）来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。

3）内存系统的重排序。由于处理器使用缓存和读/写缓冲区，这使得加载和存储操作看上去可能是在乱序执行。
```

JMM的处理器重排序规则会要求Java编译器在生成指令序列时，插入特定类型的内存屏障（Memory Barriers，Intel称之为Memory Fence）指令，通过内存屏障指令来禁止特定类型的处理器重排序。



#### 3.1.4 并发模型的分类

为了保证内存可见性，Java编译器在生成指令序列的适当位置会插入内存屏障指令来禁止特定类型的处理器重排序。

StoreLoad Barriers是一个“全能型”的屏障，它同时具有其他3个屏障的效果。现代的多处理器大多支持该屏障（其他类型的屏障不一定被所有处理器支持）。执行该屏障开销会很昂贵，因为当前处理器通常要把写缓冲区中的数据全部刷新到内存中（Buffer Fully Flush）。

#### 3.1.5 happens-before

```
与程序员密切相关的happens-before规则如下。

·程序顺序规则：一个线程中的每个操作，happens-before于该线程中的任意后续操作。

·监视器锁规则：对一个锁的解锁，happens-before于随后对这个锁的加锁。

·volatile变量规则：对一个volatile域的写，happens-before于任意后续对这个volatile域的读。

·传递性：如果A happens-before B，且B happens-before C，那么A happens-before C。
```





###  3.2 重排序

重排序是指编译器和处理器为了优化程序性能而对指令序列进行重新排序的一种手段。

#### 3.2.1 数据依赖性

如果两个操作访问同一个变量，且这两个操作中有一个为写操作，此时这两个操作之间就存在数据依赖性。

在重排序的过程中，编译器和处理器会维护这种数据依赖性。

#### 3.2.2 as-if-serial

```
as-if-serial语义的意思是：不管怎么重排序（编译器和处理器为了提高并行度），（单线程）程序的执行结果不能被改变。编译器、runtime和处理器都必须遵守as-if-serial语义

as-if-serial语义把单线程程序保护了起来，遵守as-if-serial语义的编译器、runtime和处理器共同为编写单线程程序的程序员创建了一个幻觉：单线程程序是按程序的顺序来执行的。as-if-serial语义使单线程程序员无需担心重排序会干扰他们，也无需担心内存可见性问题。
```



* 在单线程程序中，对存在控制依赖的操作重排序，不会改变执行结果（这也是as-if-serial语义允许对存在控制依赖的操作做重排序的原因）；但在多线程程序中，对存在控制依赖的操作重排序，可能会改变程序的执行结果。



### 3.3  顺序一致性

#### 3.3.1  数据竞争与顺序一致性

```
JMM对正确同步的多线程程序的内存一致性做了如下保证。

如果程序是正确同步的，程序的执行将具有顺序一致性（Sequentially Consistent）——即程序的执行结果与该程序在顺序一致性内存模型中的执行结果相同。马上我们就会看到，这对于程序员来说是一个极强的保证。这里的同步是指广义上的同步，包括对常用同步原语（synchronized、volatile和final）的正确使用。
```



#### 3.3.3 同步程序的顺序一致性效果

```
顺序一致性模型中，所有操作完全按程序的顺序串行执行。而在JMM中，临界区内的代码可以重排序（但JMM不允许临界区内的代码“逸出”到临界区之外，那样会破坏监视器的语义）。JMM会在退出临界区和进入临界区这两个关键时间点做一些特别处理，使得线程在这两个时间点具有与顺序一致性模型相同的内存视图（具体细节后文会说明）。虽然线程A在临界区内做了重排序，但由于监视器互斥执行的特性，这里的线程B根本无法“观察”到线程A在临界区内的重排序。这种重排序既提高了执行效率，又没有改变程序的执行结果。

从这里我们可以看到，JMM在具体实现上的基本方针为：在不改变（正确同步的）程序执行结果的前提下，尽可能地为编译器和处理器的优化打开方便之门。
```

#### 3.3.4 未同步程序的执行特性

```
对于未同步或未正确同步的多线程程序，JMM只提供最小安全性：线程执行时读取到的值，要么是之前某个线程写入的值，要么是默认值（0，Null，False），JMM保证线程读操作读取到的值不会无中生有（Out Of Thin Air）的冒出来。为了实现最小安全性，JVM在堆上分配对象时，首先会对内存空间进行清零，然后才会在上面分配对象（JVM内部会同步这两个操作）。因此，在已清零的内存空间（Pre-zeroed Memory）分配对象时，域的默认初始化已经完成了。
```

* JMM不保证未同步程序的执行结果与该程序在顺序一致性模型中的执行结果一致。(性能考虑)

```
未同步程序在JMM中的执行时，整体上是无序的，其执行结果无法预知。未同步程序在两个模型中的执行特性有如下几个差异。

1）顺序一致性模型保证单线程内的操作会按程序的顺序执行，而JMM不保证单线程内的操作会按程序的顺序执行（比如上面正确同步的多线程程序在临界区内的重排序）。这一点前面已经讲过了，这里就不再赘述。

2）顺序一致性模型保证所有线程只能看到一致的操作执行顺序，而JMM不保证所有线程能看到一致的操作执行顺序。这一点前面也已经讲过，这里就不再赘述。

3）JMM不保证对64位的long型和double型变量的写操作具有原子性，而顺序一致性模型保证对所有的内存读/写操作都具有原子性。
```



### 3.4 volatile的内存语义

#### 3.4.1 vlolatile的特性

```
一个volatile变量的单个读/写操作，与一个普通变量的读/写操作都是使用同一个锁来同步，它们之间的执行效果相同。
```

锁的happens-before规则保证释放锁和获取锁的两个线程之间的内存可见性，这意味着对一个volatile变量的读，总是能看到（任意线程）对这个volatile变量最后的写入。

简而言之，volatile变量自身具有下列特性。:

```
·可见性。对一个volatile变量的读，总是能看到（任意线程）对这个volatile变量最后的写入。

·原子性：对任意单个volatile变量的读/写具有原子性，但类似于volatile++这种复合操作不具有原子性。
```



#### 3.4.2 volatile建立的读写关系

1. volatile变量的写-读可以实现线程之间的通信。
2. 从内存语义的角度来说，volatile的写-读与锁的释放-获取有相同的内存效果：volatile写和锁的释放有相同的内存语义；volatile读与锁的获取有相同的内存语义。

#### 3.4.3 volatile 写-读的内存语义

1. 当写一个volatile变量时，JMM会把该线程对应的本地内存中的共享变量值刷新到主内存。

#### 3.4.4 volatile 内存语义的实现

```
·当第二个操作是volatile写时，不管第一个操作是什么，都不能重排序。这个规则确保volatile写之前的操作不会被编译器重排序到volatile写之后。

·当第一个操作是volatile读时，不管第二个操作是什么，都不能重排序。这个规则确保volatile读之后的操作不会被编译器重排序到volatile读之前。

·当第一个操作是volatile写，第二个操作是volatile读时，不能重排序。
```

下面是基于保守策略的JMM内存屏障插入策略：

```
·在每个volatile写操作的前面插入一个StoreStore屏障。

·在每个volatile写操作的后面插入一个StoreLoad屏障。

·在每个volatile读操作的后面插入一个LoadLoad屏障。

·在每个volatile读操作的后面插入一个LoadStore屏障。
```

### 3.5 锁的语义

#### 3.5.1 锁的释放-获取建立happens-before关系

#### 3.5.2 锁的释放和获取的内存语义

1. 当线程释放锁时，JMM会把该线程对应的本地内存中的共享变量刷新到主内存中。
2. 当线程获取锁时，JMM会把该线程对应的本地内存置为无效。从而使得被监视器保护的临界区代码必须从主内存中读取共享变量。

下面对锁释放和锁获取的内存语义做个总结。：

```
·线程A释放一个锁，实质上是线程A向接下来将要获取这个锁的某个线程发出了（线程A对共享变量所做修改的）消息。

·线程B获取一个锁，实质上是线程B接收了之前某个线程发出的（在释放这个锁之前对共享变量所做修改的）消息。

·线程A释放锁，随后线程B获取这个锁，这个过程实质上是线程A通过主内存向线程B发送消息。
```

#### 3.5.3 锁内存语义的实现

现在对公平锁和非公平锁的内存语义做个总结。

·公平锁和非公平锁释放时，最后都要写一个volatile变量state。

·公平锁获取时，首先会去读volatile变量。

·非公平锁获取时，首先会用CAS更新volatile变量，这个操作同时具有volatile读和volatile写的内存语义。

从本文对ReentrantLock的分析可以看出，锁释放-获取的内存语义的实现至少有下面两种方式。

1）利用volatile变量的写-读所具有的内存语义。

2）利用CAS所附带的volatile读和volatile写的内存语义。

#### 3.5.4 concurrent包的实现

如果我们仔细分析concurrent包的源代码实现，会发现一个通用化的实现模式：

```
首先，声明共享变量为volatile。

然后，使用CAS的原子条件更新来实现线程之间的同步。

同时，配合以volatile的读/写和CAS所具有的volatile读和写的内存语义来实现线程之间的通信。

AQS，非阻塞数据结构和原子变量类（java.util.concurrent.atomic包中的类），这些concurrent包中的基础类都是使用这种模式来实现的，而concurrent包中的高层类又是依赖于这些基础类来实现的。
```





### 3.6 final的内存语义

#### 3.6.1 final域的重排序规则

```
1）在构造函数内对一个final域的写入，与随后把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。

2）初次读一个包含final域的对象的引用，与随后初次读这个final域，这两个操作之间不能重排序。
```

```
1）JMM禁止编译器把final域的写重排序到构造函数之外。

2）编译器会在final域的写之后，构造函数return之前，插入一个StoreStore屏障。这个屏障禁止处理器把final域的写重排序到构造函数之外。
```

```
读final域的重排序规则是，在一个线程中，初次读对象引用与初次读该对象包含的final域，JMM禁止处理器重排序这两个操作（注意，这个规则仅仅针对处理器）。编译器会在读final域操作的前面插入一个LoadLoad屏障。

初次读对象引用与初次读该对象包含的final域，这两个操作之间存在间接依赖关系。由于编译器遵守间接依赖关系，因此编译器不会重排序这两个操作。大多数处理器也会遵守间接依赖，也不会重排序这两个操作。但有少数处理器允许对存在间接依赖关系的操作做重排序（比如alpha处理器），这个规则就是专门用来针对这种处理器的。

读final域的重排序规则可以确保：在读一个对象的final域之前，一定会先读包含这个final域的对象的引用。在这个示例程序中，如果该引用不为null，那么引用对象的final域一定已经被A线程初始化过了。
```

```
本例final域为一个引用类型，它引用一个int型的数组对象。对于引用类型，写final域的重排序规则对编译器和处理器增加了如下约束：在构造函数内对一个final引用的对象的成员域的写入，与随后在构造函数外把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序。
```





......





## 4 Java并发基础

### 4.1 线程简介

```
现代操作系统调度的最小单元是线程，也叫轻量级进程（Light Weight Process），在一个进程里可以创建多个线程，这些线程都拥有各自的计数器、堆栈和局部变量等属性，并且能够访问共享的内存变量。处理器在这些线程上高速切换，让使用者感觉到这些线程在同时执行。
```

```java
public class MultiThread{
    public static void main(String[] args) {
        // 获取Java线程管理MXBean
        ThreadMXBean threadMXBean = ManagementFactory.getThreadMXBean();
        // 不需要获取同步的monitor和synchronizer信息，仅获取线程和线程堆栈信息
        ThreadInfo[] threadInfos = threadMXBean.dumpAllThreads(false, false);
        // 遍历线程信息，仅打印线程ID和线程名称信息
        for (ThreadInfo threadInfo : threadInfos) {
            System.out.println("[" + threadInfo.getThreadId() + "] " + threadInfo.
            getThreadName());
        }
    }
}
```

输出如下：

```shell
[4] Signal Dispatcher　 // 分发处理发送给JVM信号的线程
[3] Finalizer　　　　   // 调用对象finalize方法的线程
[2] Reference Handler   // 清除Reference的线程
[1] main　  　　　　    // main线程，用户程序入口
```

说明：一个一个Java程序的运行不仅仅是main()方法的运行，而是main线程和多个其他线程的同时运行

### 4.1.2 为何多线程

1. 更多的处理器核心

2. 更快的响应时间

   ```
   在上面的场景中，可以使用多线程技术，即将数据一致性不强的操作派发给其他线程处理（也可以使用消息队列），如生成订单快照、发送邮件等。这样做的好处是响应用户请求的线程能够尽可能快地处理完成，缩短了响应时间，提升了用户体验。
   ```

3. 更好的编程模型

   ```
   Java为多线程编程提供了良好、考究并且一致的编程模型，使开发人员能够更加专注于问题的解决，即为所遇到的问题建立合适的模型，而不是绞尽脑汁地考虑如何将其多线程化。一旦开发人员建立好了模型，稍做修改总是能够方便地映射到Java提供的多线程编程模型上。
   ```

#### 4.1.3 线程优先级

```
在Java线程中，通过一个整型成员变量priority来控制优先级，优先级的范围从1~10，在线程构建的时候可以通过setPriority(int)方法来修改优先级，默认优先级是5，优先级高的线程分配时间片的数量要多于优先级低的线程。设置线程优先级时，针对频繁阻塞（休眠或者I/O操作）的线程需要设置较高优先级，而偏重计算（需要较多CPU时间或者偏运算）的线程则设置较低的优先级，确保处理器不会被独占。在不同的JVM以及操作系统上，线程规划会存在差异，有些操作系统甚至会忽略对线程优先级的设定
```

#### 4.1.4 线程的状态

* NEW: 线程被创建，但是还没有调用start()方法
* RUNNABLE:运行状态（就绪 + 运行中）
* BLOCKED: 阻塞，表示线程阻塞于锁
* WAITING:等待状态，表示该线程需要等待，其他线程做出特定动作，通知或中断
* TIME WAITING超时等待，超时后自动返回
* TERMINATED:  终止状态




#### 4.1.5 Deamon线程

```
Daemon线程是一种支持型线程，因为它主要被用作程序中后台调度以及支持性工作。这意味着，当一个Java虚拟机中不存在非Daemon线程的时候，Java虚拟机将会退出。可以通过调用Thread.setDaemon(true)将线程设置为Daemon线程。
```

```java
public class Daemon {
    public static void main(String[] args) {
        Thread thread = new Thread(new DaemonRunner(), "DaemonRunner");
        thread.setDaemon(true);
        thread.start();
}
    static class DaemonRunner implements Runnable {
        @Override
        public void run() {
            try {
                SleepUtils.second(10);
            } finally {
                System.out.println("DaemonThread finally run.");
            }
        }
    }
}

/*
运行Daemon程序，可以看到在终端或者命令提示符上没有任何输出。main线程（非Daemon线程）在启动了线程DaemonRunner之后随着main方法执行完毕而终止，而此时Java虚拟机中已经没有非Daemon线程，虚拟机需要退出。Java虚拟机中的所有Daemon线程都需要立即终止，因此DaemonRunner立即终止，但是DaemonRunner中的finally块并没有执行。*/
```





### 4.2 启动和终止线程

#### 4.2.1 构造线程

* java.lang.Thread中对线程进行初始化的部分:

```java
private void init(ThreadGroup g, Runnable target, String name,long stackSize, 
AccessControlContext acc) {
      if (name == null) {
             throw new NullPointerException("name cannot be null");
      }
      // 当前线程就是该线程的父线程
      Thread parent = currentThread();
      this.group = g;
      // 将daemon、priority属性设置为父线程的对应属性
      this.daemon = parent.isDaemon();
      this.priority = parent.getPriority();
      this.name = name.toCharArray();
      this.target = target;
      setPriority(priority);
      // 将父线程的InheritableThreadLocal复制过来
      if (parent.inheritableThreadLocals != null) 
      this.inheritableThreadLocals=ThreadLocal.createInheritedMap(parent.
      inheritableThreadLocals);
      // 分配一个线程ID
      tid = nextThreadID();
}
```

```
在上述过程中，一个新构造的线程对象是由其parent线程来进行空间分配的，而child线程继承了parent是否为Daemon、优先级和加载资源的contextClassLoader以及可继承的ThreadLocal，同时还会分配一个唯一的ID来标识这个child线程。至此，一个能够运行的线程对象就初始化好了，在堆内存中等待着运行。
```

#### 4.2.2 启动线程

```
线程对象在初始化完成之后，调用start()方法就可以启动这个线程。线程start()方法的含义是：当前线程（即parent线程）同步告知Java虚拟机，只要线程规划器空闲，应立即启动调用start()方法的线程。
```

#### 4.2.3 立即中断

```
中断可以理解为线程的一个标识位属性，它表示一个运行中的线程是否被其他线程进行了中断操作。中断好比其他线程对该线程打了个招呼，其他线程通过调用该线程的interrupt()方法对其进行中断操作。

线程通过检查自身是否被中断来进行响应，线程通过方法isInterrupted()来进行判断是否被中断，也可以调用静态方法Thread.interrupted()对当前线程的中断标识位进行复位。如果该线程已经处于终结状态，即使该线程被中断过，在调用该线程对象的isInterrupted()时依旧会返回false。
```

#### 4.2.4 过期的方法：

```
通过示例的输出可以看到，suspend()、resume()和stop()方法完成了线程的暂停、恢复和终止工作，而且非常“人性化”。但是这些API是过期的，也就是不建议使用的。
```

```
不建议使用的原因主要有：以suspend()方法为例，在调用后，线程不会释放已经占有的资源（比如锁），而是占有着资源进入睡眠状态，这样容易引发死锁问题。同样，stop()方法在终结一个线程时不会保证线程的资源正常释放，通常是没有给予线程完成资源释放工作的机会，因此会导致程序可能工作在不确定状态下。
```

#### 4.2.5  安全地终止线程

```java
 countThread.interrupt();
two.cancel();
```

### 4.3 线程间通信

#### 4.3.1　volatile和synchronized关键字

* ```
  关键字volatile可以用来修饰字段（成员变量），就是告知程序任何对该变量的访问均需要从共享内存中获取，而对它的改变必须同步刷新回共享内存，它能保证所有线程对变量访问的可见性。
  ```

* ```
  关键字synchronized可以修饰方法或者以同步块的形式来进行使用，它主要确保多个线程在同一个时刻，只能有一个线程处于方法或者同步块中，它保证了线程对变量访问的可见性和排他性。
  ```

* 无论采用哪种方式，其本质是对一个对象的监视器（monitor）进行获取，而这个获取过程是排他的，也就是同一时刻只能有一个线程获取到由synchronized所保护对象的监视器。

任意一个对象都拥有自己的监视器，当这个对象由同步块或者这个对象的同步方法调用时，执行方法的线程必须先获取到该对象的监视器才能进入同步块或者同步方法，而没有获取到监视器（执行该方法）的线程将会被阻塞在同步块和同步方法的入口处，进入BLOCKED状态。

#### 4.3.2　等待/通知机制

等待/通知方法是任意Java对象都具备的，来源于Object对象

* notify() : 通知一个在对象上等待的线程
* notifyAll() : 通知所有在该对象上等待的线程
* wait() : 进入waiting状态，释放锁，只有等待通知或者中断后才能返回
* wait(long) :超时等待一段时间，单位毫秒
* wait(long,int) :超时等待，单位纳秒

#### 4.3.3　等待/通知的经典范式

* 等待方遵循如下原则。

  1）获取对象的锁。

  2）如果条件不满足，那么调用对象的wait()方法，被通知后仍要检查条件。

  3）条件满足则执行对应的逻辑。

  ```java
  synchronized(对象) {
         while(条件不满足) {
                对象.wait();
         }
         对应的处理逻辑
  }
  ```

  

* 通知：

  1）获得对象的锁。

  2）改变条件。

  3）通知所有等待在对象上的线程。

  ```java
  synchronized(对象) {
         改变条件
         对象.notifyAll();
  }
  ```



#### 4.3.4　管道输入/输出流

* 管道输入/输出流和普通的文件输入/输出流或者网络输入/输出流不同之处在于，它主要用于线程之间的数据传输，而传输的媒介为内存。具体包括了如下4中具体的实现：
  1. PipedOutputStream
  2. PipedInputStream
  3. PipedReader
  4. PipedWriter

#### 4.3.5　Thread.join()的使用

* 如果一个线程A执行了thread.join()语句，其含义是：当前线程A等待thread线程终止之后才从thread.join()返回。线程Thread除了提供join()方法之外，还提供了join(long millis)和join(long millis,int nanos)两个具备超时特性的方法。这两个超时方法表示，如果线程thread在给定的超时时间里没有终止，那么将会从该超时方法中返回。

* JDK中Thread.join()方法的源码（进行了部分调整）:

  ```java
  
  // 加锁当前线程对象
  public final synchronized void join() throws InterruptedException {
         // 条件不满足，继续等待
         while (isAlive()) {
                wait(0);
         }
         // 条件符合，方法返回
  }
  ```

* 当线程终止时，会调用线程自身的notifyAll()方法，会通知所有等待在该线程对象上的线程。可以看到join()方法的逻辑结构与4.3.3节中描述的等待/通知经典范式一致，即加锁、循环和处理逻辑3个步骤。

#### 4.3.6　ThreadLocal的使用

* ThreadLocal，即线程变量，是一个以ThreadLocal对象为键、任意对象为值的存储结构。这个结构被附带在线程上，也就是说一个线程可以根据一个ThreadLocal对象查询到绑定在这个线程上的一个值。

* 可以通过set(T)方法来设置一个值，在当前线程下再通过get()方法获取到原先设置的值。

* eg：

  ```java
  public class Profiler {
      // 第一次get()方法调用时会进行初始化（如果set方法没有调用），每个线程会调用一次
      private static final ThreadLocal<Long> TIME_THREADLOCAL = new ThreadLocal<Long>() {
          protected Long initialValue() {
              return System.currentTimeMillis();
          }
      };
      public static final void begin() {
          TIME_THREADLOCAL.set(System.currentTimeMillis());
      }
      public static final long end() {
          return System.currentTimeMillis() - TIME_THREADLOCAL.get();
      }
      public static void main(String[] args) throws Exception {
          Profiler.begin();
          TimeUnit.SECONDS.sleep(1);
          System.out.println("Cost: " + Profiler.end() + " mills");
      }
  }
  ```





### 4.4 线程应用实例

#### 4.4.1 等待超时模式

```java
// 对当前对象加锁
public synchronized Object get(long mills) throws InterruptedException {
       long future = System.currentTimeMillis() + mills;
       long remaining = mills;
       // 当超时大于0并且result返回值不满足要求
       while ((result == null) && remaining > 0) {
              wait(remaining);
              remaining = future - System.currentTimeMillis();
       }
              return result;
}
```

#### 4.4.2 线程池技术及其示例

```
对于服务端的程序，经常面对的是客户端传入的短小（执行时间短、工作内容较为单一）任务，需要服务端快速处理并返回结果。如果服务端每次接受到一个任务，创建一个线程，然后进行执行，这在原型阶段是个不错的选择，但是面对成千上万的任务递交进服务器时，如果还是采用一个任务一个线程的方式，那么将会创建数以万记的线程，这不是一个好的选择。因为这会使操作系统频繁的进行线程上下文切换，无故增加系统的负载，而线程的创建和消亡都是需要耗费系统资源的，也无疑浪费了系统资源。
```

* 一次创建随取随用，不会有太多上下文切换，节省系统资源

* ```java
  public interface ThreadPool<Job extends Runnable> {
         // 执行一个Job，这个Job需要实现Runnable
         void execute(Job job);
         // 关闭线程池
         void shutdown();
         // 增加工作者线程
         void addWorkers(int num);
         // 减少工作者线程
         void removeWorker(int num);
         // 得到正在等待执行的任务数量
         int getJobSize();
  }
  /*客户端可以通过execute(Job)方法将Job提交入线程池执行，而客户端自身不用等待Job的执行完成。除了execute(Job)方法以外，线程池接口提供了增大/减少工作者线程以及关闭线程池的方法。这里工作者线程代表着一个重复执行Job的线程，而每个由客户端提交的Job都将进入到一个工作队列中等待工作者线程的处理。*/
  ```

* ```java
  public class DefaultThreadPool<Job extends Runnable> implements ThreadPool<Job> {
      // 线程池最大限制数
      private static final int    MAX_WORKER_NUMBERS    = 10;
      // 线程池默认的数量
      private static final int    DEFAULT_WORKER_NUMBERS = 5;
      // 线程池最小的数量
      private static final int    MIN_WORKER_NUMBERS    = 1;
      // 这是一个工作列表，将会向里面插入工作
      private final LinkedList<Job>    jobs = new LinkedList<Job>();
      // 工作者列表
      private final List<Worker>    workers    = Collections.synchronizedList(new 
      ArrayList<Worker>());
      // 工作者线程的数量
      private int    workerNum    = DEFAULT_WORKER_NUMBERS;
      // 线程编号生成
      private AtomicLong    threadNum    = new AtomicLong();
      public DefaultThreadPool() {
      initializeWokers(DEFAULT_WORKER_NUMBERS);
      }
      public DefaultThreadPool(int num) {
          workerNum = num > MAX_WORKER_NUMBERS  MAX_WORKER_NUMBERS : num < MIN_WORKER_
          NUMBERS  MIN_WORKER_NUMBERS : num;
          initializeWokers(workerNum);
      }
      public void execute(Job job) {
          if (job != null) {
              // 添加一个工作，然后进行通知
              synchronized (jobs) {
                  jobs.addLast(job);
                  jobs.notify();
              }
          }
      }
      public void shutdown() {
          for (Worker worker : workers) {
              worker.shutdown();
          }
      }
      public void addWorkers(int num) {
          synchronized (jobs) {
              // 限制新增的Worker数量不能超过最大值
              if (num + this.workerNum > MAX_WORKER_NUMBERS) {
                  num = MAX_WORKER_NUMBERS - this.workerNum;
              }
              initializeWokers(num);
              this.workerNum += num;
          }
      }
      public void removeWorker(int num) {
          synchronized (jobs) {
              if (num >= this.workerNum) {
                  throw new IllegalArgumentException("beyond workNum");
              }
              // 按照给定的数量停止Worker
              int count = 0;
              while (count < num) {
                  Worker worker = workers.get(count)
                  if (workers.remove(worker)) {
                  worker.shutdown();
                      count++;
                  }
              }
              this.workerNum -= count;
          }
      }
      public int getJobSize() {
          return jobs.size();
      }
      // 初始化线程工作者
      private void initializeWokers(int num) {
          for (int i = 0; i < num; i++) {
              Worker worker = new Worker();
              workers.add(worker);
              Thread thread = new Thread(worker, "ThreadPool-Worker-" + threadNum.
              incrementAndGet());
              thread.start();
          }
      }
      // 工作者，负责消费任务
      class Worker implements Runnable {
          // 是否工作
          private volatile boolean    running    = true;
          public void run() {
              while (running) {
                  Job job = null;
                  synchronized (jobs) {
                      // 如果工作者列表是空的，那么就wait
                      while (jobs.isEmpty()) {
                          try {
                              jobs.wait();
                          } catch (InterruptedException ex) {
                              // 感知到外部对WorkerThread的中断操作，返回
                              Thread.currentThread().interrupt();
                              return;
                          }
                      }
                      // 取出一个Job
                      job = jobs.removeFirst();
                  }
                  if (job != null) {
                      try {
                          job.run();
                      } catch (Exception ex) {
                          // 忽略Job执行中的Exception
                      }
                  }
              }
          }
          public void shutdown() {
              running = false;
          }
      }
  }
  ```

* 可以看到，线程池的本质就是使用了一个线程安全的工作队列连接工作者线程和客户端线程，客户端线程将任务放入工作队列后便返回，而工作者线程则不断地从工作队列上取出工作并执行。当工作队列为空时，所有的工作者线程均等待在工作队列上，当有客户端提交了一个任务之后会通知任意一个工作者线程，随着大量的任务被提交，更多的工作者线程会被唤醒。

#### 4.4.4 一个基于线程池技术的简单Web服务器

```java
public class SimpleHttpServer {
    // 处理HttpRequest的线程池
    static ThreadPool<HttpRequestHandler>  threadPool    = new DefaultThreadPool
        <HttpRequestHandler>(1);
    // SimpleHttpServer的根路径
    static String    basePath;
    static ServerSocket    serverSocket;
    // 服务监听端口
    static int    port    = 8080;
    public static void setPort(int port) {
        if (port > 0) {
            SimpleHttpServer.port = port;
        }
    }
    public static void setBasePath(String basePath) {
        if (basePath != null && new File(basePath).exists() && new File(basePath).
        isDirectory()) {
            SimpleHttpServer.basePath = basePath;
        }
    }
    // 启动SimpleHttpServer
    public static void start() throws Exception {
        serverSocket = new ServerSocket(port);
        Socket socket = null;
        while ((socket = serverSocket.accept()) != null) {
            // 接收一个客户端Socket，生成一个HttpRequestHandler，放入线程池执行
            threadPool.execute(new HttpRequestHandler(socket));
        }
        serverSocket.close();
    }
    static class HttpRequestHandler implements Runnable {
        private Socket    socket;
        public HttpRequestHandler(Socket socket) {
            this.socket = socket;
        }
        @Override
        public void run() {
            String line = null;
            BufferedReader br = null;
            BufferedReader reader = null;
            PrintWriter out = null;
            InputStream in = null;
            try {
                reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
                String header = reader.readLine();
                // 由相对路径计算出绝对路径
                String filePath = basePath + header.split(" ")[1];
                out = new PrintWriter(socket.getOutputStream());
                // 如果请求资源的后缀为jpg或者ico，则读取资源并输出
                if (filePath.endsWith("jpg") || filePath.endsWith("ico")) {
                    in = new FileInputStream(filePath);
                    ByteArrayOutputStream baos = new ByteArrayOutputStream();
                    int i = 0;
                    while ((i = in.read()) != -1) {
                        baos.write(i);
                    }
                    byte[] array = baos.toByteArray();
                    out.println("HTTP/1.1 200 OK");
                    out.println("Server: Molly");
                    out.println("Content-Type: image/jpeg");
                    out.println("Content-Length: " + array.length);
                    out.println("");
                    socket.getOutputStream().write(array, 0, array.length);
                } else {
                    br = new BufferedReader(new InputStreamReader(new 
                    FileInputStream(filePath)));
                    out = new PrintWriter(socket.getOutputStream());
                    out.println("HTTP/1.1 200 OK");
                    out.println("Server: Molly");
                    out.println("Content-Type: text/html; charset=UTF-8");
                    out.println("");
                    while ((line = br.readLine()) != null) {
                        out.println(line);
                    }
                }
                out.flush();
            } catch (Exception ex) {
                out.println("HTTP/1.1 500");
                out.println("");
                out.flush();
            } finally {
                close(br, in, reader, out, socket);
            }
        }
    }
    // 关闭流或者Socket
    private static void close(Closeable... closeables) {
        if (closeables != null) {
            for (Closeable closeable : closeables) {
                try {
                    closeable.close();
                } catch (Exception ex) {
                }
            }
        }
    }
}
```

## 5 Java中的锁

### 5.1 LOCK接口

```
在Lock接口出现之前，Java程序是靠synchronized关键字实现锁功能的，而Java SE 5之后，并发包中新增了Lock接口（以及相关实现类）用来实现锁功能，它提供了与synchronized关键字类似的同步功能，只是在使用时需要显式地获取和释放锁。虽然它缺少了（通过synchronized块或者方法所提供的）隐式获取释放锁的便捷性，但是却拥有了锁获取与释放的可操作性、可中断的获取锁以及超时获取锁等多种synchronized关键字所不具备的同步特性。

使用synchronized关键字将会隐式地获取锁，但是它将锁的获取和释放固化了，也就是先获取再释放。当然，这种方式简化了同步的管理，可是扩展性没有显示的锁获取和释放来的好。
```

* 不要将获取锁的过程写在try块中，因为如果在获取锁（自定义锁的实现）时发生了异常，异常抛出的同时，也会导致锁无故释放。
* lock相对于syncchronized的有点：
  1. 超时获取锁： 超时后自动释放
  2. 尝试非阻塞的获取：当线程尝试获取锁，如果某一时刻没有被其他线程获取，那么就能成功持有
  3. 能被中断的获取：线程被中断时，中断异常会抛出，同时锁被释放