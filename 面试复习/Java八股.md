# Java八股

## HashMap

https://www.jianshu.com/p/9b12a52cfceb

* 结构：

  1.7 ： 数组加链表

  1.8： 数组 + 链表/红黑树：

  * 为何变化长度是8：

    * 链表时间复杂度是O(n/2), 红黑树是O(longn), n ==8 时， log8 < (8/2)。此时更快

  * 为何缩回去是6：

    * 频繁的从链表转到红黑树，再从红黑树转到链表，开销会很大，特别是频繁的从链表转到红黑树时，需要旋转

  * 为何是红黑树而不是AVL树？

    * ```
      因为 AVL 树比红黑树保持着更加严格的平衡， AVL 树中从根到最深叶的路径最多为 1.44log（n + 2） ，红黑树中则最多为 2log（ n + 1） ，所以 AVL 树查找效果会比较快，如果是查找密集型任务使用 AVL 树比较好，相反插入密集型任务，使用红黑树效果就比较 nice
      
      AVL 树在每个节点上都会存储平衡因子
      
      AVL 树的旋转比红黑树的旋转更加难以平衡和调试，如果两个都给 O（lgn） 查找， AVL 树可能需要 O（log n） 旋转，而红黑树最多需要两次旋转使其达到平衡
      
      
      ```

* 为何线程不安全？

  * 扩容时发生死循环

    * ```
      扩容时导致的死循环，这个问题只会在 1.7 版本及以前出现，因为在 1.7 版本及以前，扩容时的实现，采用的是头插法，这样就会导致循环链表的问题
      
      什么时候会触发扩容呢？如果存储的数据，大于 当前的 HashMap 长度（ Capacity ） * 负载因子（ LoadFactor ，默认为 0.75） 时，就会发生扩容。比如当前容量是 16 ， 16 * 0.75 = 12 ，当存储第 13 个元素时，经过判断发现需要进行扩容，那么这个时候 HashMap 就会先进行扩容的操作
      
      扩容也不是简简单单的将原来的容量扩大就完事儿了，扩容时，首先创建一个新的 Entry 空数组，长度是原数组的 2 倍，扩容完毕之后还会再进行 ReHash ，也就是将原 Entry 数组里面的数据，重新 hash 到新数组里面去
      
      ```

    * 1.8 采用尾插法解决该问题

  * 数据覆盖问题

    ```
    假设现在线程 A 和线程 B 同时进行 put 操作，特别巧的是这两条不同的数据 hash 值一样，并且这个位置数据为 null ，那么是不是应该让线程 A 和 B 都执行 put 操作。假设线程 A 在要进行插入数据时被挂起，然后线程 B 正常执行将数据插入了，然后线程 A 获得了 CPU 时间片，也开始进行数据插入操作，那么就将线程 B 的数据给覆盖掉了
    
    因为 HashMap 对 put 操作没有进行加锁的操作，那么就不能保证下一个线程 get 到的值，就一定是没有被修改过的值，所以 HashMap 是不安全的
    
    作者：热衷技术的Java程序员
    链接：https://www.jianshu.com/p/9b12a52cfceb
    来源：简书
    著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
    ```

  

  * 因此推荐ConcurrentHashMap

  * ```
    在 1.7 版本， ConcurrentHashMap 采用的是分段锁（ ReentrantLock + Segment + HashEntry ）实现，也就是将一个 HashMap 分成多个段，然后每一段都分配一把锁，这样去支持多线程环境下的访问。但是这样锁的粒度太大了，因为你锁的直接就是一段嘛
    
    所以 1.8 版本又做了优化，使用 CAS + synchronized + Node + 红黑树 来实现，这样就将锁的粒度降低了，同时使用 synchronized 来加锁，相比于 ReentrantLock 来说，会节省比较多的内存空间
    
    ```

    

  

## ConcurrentHashMap

https://www.jianshu.com/p/460bf90b9137

* 基本原理：

  ```
  内部持有一个Node<K,V>[]，用来存放key,value。
  1.1 这个数组的默认长度是16，并且只会在第一次put的时候才会初始化（lazy init）。
  put 的时候要通过运算得到应存放的数组下标，然后根据不同的情况决定初始化数组、插入链表、插入红黑树或者协助扩容。
  2.1 先进行hash扰动。
  2.2 数组如果还未进行初始化，则先进行初始化。初始化默认大小为16，如果指定了初始化大小，则会计算一个>=指定值，且为2的N次幂的数字，且最接近当前参数的数字作为初始长度。
  2.3 当前位置==null，则直接通过CAS插入数据。
  2.4 如果当前数组正在进行扩容，则协助扩容。
  2.6 当前位置!=null。如果当前节点是红黑树，则直接插入树中。否则作为链表插入链表插入或者更新。
  2.7 插入成功后，如果是链表，则检查是否需要转成红黑树。转换条件是链表节点数>=8，且数组长度>64。
  2.8 最后更新size的值，并且检查是否需要扩容。
  get的时候同样通过运算得到应存放的数组下标，然后进行遍历。
  3.1 先进行hash扰动，使用hash&(n-1)得到数组索引。
  3.2 取索引对应的数据进行遍历。可能是链表、红黑树，也可能是FWD节点。
  size++的时候，先尝试用更新volatile baseCount，更新失败再尝试定位到具体的CounteCell，用CAS或者直接更新其volatile value。
  
  ```

  ![img](https://upload-images.jianshu.io/upload_images/10505542-29af0f011925bd4b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

* 组件属性

  * 属性

    ```java
    数组最大长度，2的30次方。之所以不是2的32次方，是因为前两位用于控制
    private static final int MAXIMUM_CAPACITY = 1 << 30;
    默认数组大小16
    private static final int DEFAULT_CAPACITY = 16;
    链表转红黑树的阈值
    static final int TREEIFY_THRESHOLD = 8;
    红黑树转链表的阈值
    static final int UNTREEIFY_THRESHOLD = 6;
    链表转红黑树时需满足的最小数组长度                                // 意思是小于64，冲突说明太挤，不着急用链表法
    static final int MIN_TREEIFY_CAPACITY = 64;
    表示正在转移
    static final int MOVED     = -1; // hash for forwarding nodes     
    红黑树的hash值
    static final int TREEBIN   = -2; // hash for roots of trees
    static final int RESERVED  = -3; // hash for transient reservations
    //hash扰乱时用来将数值转换为正数
    static final int HASH_BITS = 0x7fffffff; // usable bits of normal node hash
    CPU可用核心个数
    static final int NCPU = Runtime.getRuntime().availableProcessors();
    
    
    存放数据的数组
    transient volatile Node<K,V>[] table;
    要转移到的目标数组，只有扩容的时候非空
    private transient volatile Node<K,V>[] nextTable;‘
    计数器conut，用来统计size
    private transient volatile long baseCount;
    -1/初始化 0/默认值  >0 未初始化之前的长度/初始化之后的阈值  <-1 特殊数字+n记录正在扩容的线程数
    private transient volatile int sizeCtl;
    近似于多线程扩容时当前线程可以取到的最大下标
    private transient volatile int transferIndex;
    用来计数的数组
    private transient volatile CounterCell[] counterCells;
    ```

  * 类：

    **类**
    Node-链表类，用来存储key-value

    ```java
    static class Node<K,V> implements Map.Entry<K,V> {
            final int hash;
            final K key;
            volatile V val;
            指向下个元素的指针
            volatile Node<K,V> next;
    
            Node(int hash, K key, V val, Node<K,V> next) {
                this.hash = hash;
                this.key = key;
                this.val = val;
                this.next = next;
            }
    
    ```

    TreeNode-用来构建TreeBin，红黑树的节点

    ```java
    static final class TreeNode<K,V> extends Node<K,V> {
            TreeNode<K,V> parent;  // red-black tree links
            TreeNode<K,V> left;
            TreeNode<K,V> right;
            TreeNode<K,V> prev;    // needed to unlink next upon deletion
            boolean red;
    
            TreeNode(int hash, K key, V val, Node<K,V> next,
                     TreeNode<K,V> parent) {
                super(hash, key, val, next);
                this.parent = parent;
            }
    
    ```

    TreeBin-存放红黑树的首节点和根节点

    ```java
     static final class TreeBin<K,V> extends Node<K,V> {
            TreeNode<K,V> root;
            volatile TreeNode<K,V> first;
            volatile Thread waiter;
            volatile int lockState;
            // values for lockState
            static final int WRITER = 1; // set while holding write lock
            static final int WAITER = 2; // set when waiting for write lock
            static final int READER = 4; /
    ```

    ForwardingNode-占位节点，插入链表头部，表示正在被转移

    ```java
    static final class ForwardingNode<K,V> extends Node<K,V> {
            final Node<K,V>[] nextTable;
            ForwardingNode(Node<K,V>[] tab) {
                super(MOVED, null, null, null);
                this.nextTable = tab;
            }
    ```

    

  * Put

    ```java
    final V putVal(K key, V value, boolean onlyIfAbsent) {
            if (key == null || value == null) throw new NullPointerException();
            //hash扰乱
        /*
          static final int spread(int h) {
            return (h ^ (h >>> 16)) & HASH_BITS;
        }
        运算逻辑很明确，原始hashcode先右移16位，然后与原始hashcode进行异或运算，最后在与HASH_BITS进行与      运算.
        两个hashcode在不进行扰动的时候，可能得到相同的索引即hash冲突。但是经过扰动计算后，可以得到不同的索引。从而我们可以得到结论：hash扰动的目的是让同一个hash值的低位和高位的特征进行混合，从而尽可能得到一个独一无二的hash值，从而减少hash冲突     */
            int hash = spread(key.hashCode());
            int binCount = 0;
            死循环
            for (Node<K,V>[] tab = table;;) {
                Node<K,V> f; int n, i, fh;
                // 懒加载，数组未初始化，则进行初始化
                /*
                构造方法
        public ConcurrentHashMap(int initialCapacity) {
            if (initialCapacity < 0)
                throw new IllegalArgumentException();
            传进来的参数大于最大容量的一倍，则直接使用最大容量。因为默认都是2倍扩容，当大于等于最大值的一半时，再进行2倍扩容反而得不偿失，不如直接使用最大容量。
            小于最大容量的一半，则会计算出一个大于等于1.5*initialCapacity+1，且为2的N次幂的数字，作为初始数组长度。
            int cap = ((initialCapacity >= (MAXIMUM_CAPACITY >>> 1)) ?
                       MAXIMUM_CAPACITY :
                       tableSizeFor(initialCapacity + (initialCapacity >>> 1) + 1));
            在构造函数被调用时，sizeCtl存放的是上面计算得到的初始数组长度
            this.sizeCtl = cap;
        }
        可以看到构造函数仅仅计算了一个需要初始的数组长度，并且赋值给sizeCtl，并未实际进行数组的初始化。这点也很好理解，如果我仅仅是new ConcurrentHashMap()，但是之后并未使用它，在put的时候初始化就可以避免内存的浪费。
                */
                if (tab == null || (n = tab.length) == 0)
                    tab = initTable(); // 实际的初始化数组，解析见下面
                // 所属节点不为空，则CAS插入
                else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                    if (casTabAt(tab, i, null,
                                 new Node<K,V>(hash, key, value, null)))
                        break;                   // no lock when adding to empty bin
                }
                帮助扩容
                else if ((fh = f.hash) == MOVED)
                    tab = helpTransfer(tab, f);
                else {
                    V oldVal = null;
                    存在hash冲突，则锁住头节点
                    synchronized (f) {
                        if (tabAt(tab, i) == f) {
                            链表
                            if (fh >= 0) {
                                binCount = 1;
                                for (Node<K,V> e = f;; ++binCount) {
                                    K ek;
                                    if (e.hash == hash &&
                                        ((ek = e.key) == key ||
                                         (ek != null && key.equals(ek)))) {
                                        oldVal = e.val;
                                        if (!onlyIfAbsent)
                                            e.val = value;
                                        break;
                                    }
                                    Node<K,V> pred = e;
                                    if ((e = e.next) == null) {
                                        pred.next = new Node<K,V>(hash, key,
                                                                  value, null);
                                        break;
                                    }
                                }
                            }
                            红黑树
                            else if (f instanceof TreeBin) {
                                Node<K,V> p;
                                binCount = 2;
                                if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                               value)) != null) {
                                    oldVal = p.val;
                                    if (!onlyIfAbsent)
                                        p.val = value;
                                }
                            }
                        }
                    }
                    if (binCount != 0) {
                        超过阈值，链表转红黑树
                        if (binCount >= TREEIFY_THRESHOLD)
                            treeifyBin(tab, i);
                        if (oldVal != null)
                            return oldVal;
                        break;
                    }
                }
            }
            size+1&检查是否需要扩容
            addCount(1L, binCount);
            return null;
        }
    ```

    put 的流程：

    ```
    hash扰动
    死循环put，直到成功
    2.1 数组未初始化，则进行初始化
    2.2 元素为空，则进行CAS插入
    2.3 元素正在转移，则协助转移
    2.4 存在hash冲突，则锁住头节点，进行插入
    size+1&检查扩容
    
    ```

    * 初始化数组

      ```java
      private final Node<K,V>[] initTable() {
              Node<K,V>[] tab; int sc; // sc 是size record的简称
             //数组为空
              while ((tab = table) == null || tab.length == 0) {
                  //有其它线程正在进行初始化，当前线程放弃本次CPU执行权，等待下次调度
                  if ((sc = sizeCtl) < 0) // 联系下面cas，如果有别的线程初始化，sizeCtl 应该会是 -1
                      Thread.yield(); // lost initialization race; just spin
                  //尝试将SIZECTL更新成-1，更新成功则说明没有其它线程更新，则进行初始化
                  //更新失败，则说明有其它线程已经成功更新，则从新进行循环的检查
                  else if (U.compareAndSwapInt(this, SIZECTL, sc, -1)) {
                      try {
                          if ((tab = table) == null || tab.length == 0) {
                              //如果有传入初始化长度，则使用计算后的初始化长度，否则使用16
                              int n = (sc > 0) ? sc : DEFAULT_CAPACITY;
                              @SuppressWarnings("unchecked")
                              Node<K,V>[] nt = (Node<K,V>[])new Node<?,?>[n];
                              table = tab = nt;
                              设置sizeCtl为3/4*初始长度，即0.75*初始长度。这个时候sizeCtl表示的是触发扩容的阈值。
                              sc = n - (n >>> 2);
                          }
                      } finally {
                          sizeCtl = sc;
                      }
                      break; 
                  }
              }
              return tab;
          }
      ```

      ![img](https://upload-images.jianshu.io/upload_images/10505542-4978f1f724126635.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)





## AQS

https://tech.meituan.com/2019/12/05/aqs-theory-and-apply.html

## GC

https://tech.meituan.com/2020/11/12/java-9-cms-gc.html

## NIO

https://tech.meituan.com/2016/11/04/nio.html

* NIO的主要事件有几个：读就绪、写就绪、有新连接到来。

  ```
  我们首先需要注册当这几个事件到来的时候所对应的处理器。然后在合适的时机告诉事件选择器：我对这个事件感兴趣。对于写操作，就是写不出去的时候对写事件感兴趣；对于读操作，就是完成连接和系统没有办法承载新读入的数据的时；对于accept，一般是服务器刚启动的时候；而对于connect，一般是connect失败需要重连或者直接异步调用connect的时候。
  ```

  ```
  NIO由原来的阻塞读写（占用线程）变成了单线程轮询事件，找到可以进行读写的网络描述符进行读写。除了事件的轮询是阻塞的（没有可干的事情必须要阻塞），剩余的I/O操作都是纯CPU操作，没有必要开启多线程。
  ```

  ```
  举个例子，将有助于理解Reactor与Proactor二者的差异，以读操作为例（写操作类似）。
  
  在Reactor中实现读
  注册读就绪事件和相应的事件处理器。
  事件分发器等待事件。
  事件到来，激活分发器，分发器调用事件对应的处理器。
  事件处理器完成实际的读操作，处理读到的数据，注册新的事件，然后返还控制权。
  
  在Proactor中实现读：
  处理器发起异步读操作（注意：操作系统必须支持异步IO）。在这种情况下，处理器无视IO就绪事件，它关注的是完成事件。
  事件分发器等待操作完成事件。
  在分发器等待过程中，操作系统利用并行的内核线程执行实际的读操作，并将结果数据存入用户自定义缓冲区，最后通知事件分发器读操作完成。
  事件分发器呼唤处理器。
  事件处理器处理用户自定义缓冲区中的数据，然后启动一个新的异步操作，并将控制权返回事件分发器。
  ```

  

## ReentrantLock 和 Synchronized的区别

* 相似点：

  1. 都是锁机制的同步方式
  2. 都是阻塞式同步

* 区别：

  1. API层面：

     1. Synchronized是Java的关键字，是原生语法层面的锁机制，是JVM实现的

        ReentrantLock 是Java1.5 之后提供的API层面的互斥锁，需要Lock(), unlock()方法配合try、finally语句块来完成。本质上是Lock的实现类

     2. synchronized 既可以修饰方法（同步方法），也可以修饰代码块

        ReentrantLock,按照下面这个方法，基本上就是修饰代码块：

        ```
        private ReentrantLock lock = new ReentrantLock();
        public void run() {
            lock.lock();
            try{
                for(int i=0;i<5;i++){
                    System.out.println(Thread.currentThread().getName()+":"+i);
                }
            }finally{
                lock.unlock();
            }
        }
        
        ```

  2. 等待可中断

     * 等待可中断的概念：

       指当持有锁的线程长时间占有时，其他等待的线程可以放弃等待，去处理其他的事情，这种特性，可以对处理执行时间较长的同步块，有一些优化作用，至少对于被迫等待的线程，可以更高效

     * 而两者的区别就是：

       syn是不可中断等待，等待线程只能等

       reentrantlock是可中断等待，超时一定时间后，线程可以中断，去处理其他事物

  3. 公平锁：

     syn是非公平锁，reentrantlock默认是非公平锁，但是可以通过带布尔值的构造函数切换为公平锁

  4. 锁绑定多个条件：

     Reentrantlock 可以同时绑定多个Condition对象，只需要多次调用newCondition即可

     syn中，锁对象的wait()/notify() / notifyAll() 可以实现一个隐含的条件，但是如果要和多于一个的条件的关联时，就需要额外添加一个锁

* 线程安全的几个特性：

  ```
  1、原子性，简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现。
  2、可见性，是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile 就是负责保证可见性的。
  3、有序性，是保证线程内串行语义，避免指令重排等。
  
  ```

* syn底层机制：

  ```
  Synchronized进过编译，会在同步块的前后分别形成monitorenter和monitorexit这个两个字节码指令。在执行monitorenter指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加1，相应的，在执行monitorexit指令时会将锁计算器就减1，当计算器为0时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止。
  
  ```

* ReentrantLock使用方法（其实部分内容和上面重复）：

  ```
  ReentrantLock是java.util.concurrent包下提供的一套互斥锁，相比Synchronized，ReentrantLock类提供了一些高级功能，主要有以下3项：
  
  1.等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized来说可以避免出现死锁的情况。
  2.公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized锁非公平锁，ReentrantLock默认的构造函数是创建的非公平锁，可以通过参数true设为公平锁，但公平锁表现的性能不是很好。
  3.锁绑定多个条件，一个ReentrantLock对象可以同时绑定对个对象。
  
  ```

  ```
  private ReentrantLock lock = new ReentrantLock();
  public void run() {
      lock.lock();
      try{
          for(int i=0;i<5;i++){
              System.out.println(Thread.currentThread().getName()+":"+i);
          }
      }finally{
          lock.unlock();
      }
  }
  
  ```

* 适用ReentrantLock的场景

  * 时间锁等待、 可中断锁等待、无块结构锁、 多个条件变量或者锁投票

* 公平性与非公平性：

  ```
  公平性是指在竞争场景中，当公平性为真时，会倾向于将锁赋予等待时间最久的线程。公平性是减少线程“饥饿”（个别线程长期等待锁，但始终无法获取）情况发生的一个办法。
  
  1、公平锁能保证：老的线程排队使用锁，新线程仍然排队使用锁。
  2、非公平锁保证：老的线程排队使用锁；但是无法保证新线程抢占已经在排队的线程的锁。
  
  ```



* ReentrantLock 和 synchronized使用分析：

  ```
  ReentrantLock是Lock的实现类，是一个互斥的同步器，在多线程高竞争条件下，ReentrantLock比synchronized有更加优异的性能表现。
  1 用法比较
  
  Lock使用起来比较灵活，但是必须有释放锁的配合动作
  Lock必须手动获取与释放锁，而synchronized不需要手动释放和开启锁
  Lock只适用于代码块锁，而synchronized可用于修饰方法、代码块等
  
  
  2 特性比较
  
  ReentrantLock的优势体现在：
  
  具备尝试非阻塞地获取锁的特性：当前线程尝试获取锁，如果这一时刻锁没有被其他线程获取到，则成功获取并持有锁
  能被中断地获取锁的特性：与synchronized不同，获取到锁的线程能够响应中断，当获取到锁的线程被中断时，中断异常将会被抛出，同时锁会被释放
  超时获取锁的特性：在指定的时间范围内获取锁；如果截止时间到了仍然无法获取锁，则返回
  
  
  
  
  3 注意事项
  
  在使用ReentrantLock类的时，一定要注意三点：
  
  在finally中释放锁，目的是保证在获取锁之后，最终能够被释放
  不要将获取锁的过程写在try块内，因为如果在获取锁时发生了异常，异常抛出的同时，也会导致锁无故被释放。
  ReentrantLock提供了一个newCondition的方法，以便用户在同一锁的情况下可以根据不同的情况执行等待或唤醒的动作。
  
  ```





##  集合容器

https://juejin.cn/post/6844903858389401608

* HashMap的一些基本约定：

  ```
  equals 相等，hashCode 一定要相等。
  重写了 hashCode 也要重写 equals。
  hashCode 需要保持一致性，状态改变返回的哈希值仍然要一致。
  equals 的对称、反射、传递等特性
  ```

* 集合分类：

  ```
  Set：代表无序、不可重复的集合，常见的类如HashSet、TreeSet
  List：代表有序、可重复的集合，常见的类如动态数组ArrayList、双向链表LinkedList、可变数组Vector
  Map：代表具有映射关系的集合，常见的类如HashMap、LinkedHashMap、TreeMap
  Queue：代表一种队列集合
  
  ```

  1. List:

     ```
     ArrayList：基于动态数组实现，支持随机访问。
     Vector：和 ArrayList 类似，但它是线程安全的。
     LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。
     ```

  2. Set

     ```
     TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
     HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
     LinkedHashSet：具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。
     ```

  3. Map

     ```
     TreeMap：基于红黑树实现。
     HashMap：基于哈希表实现。
     HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且 ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。博客
     LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。
     ```

  4. Queue:

     ```
     LinkedList：可以用它来实现双向队列。
     PriorityQueue：基于堆结构实现，可以用它来实现优先队列。
     ```

* 集合框架,，使操作集合更为容易，多态？：

  ```
  Collection
  Collection是最基本的集合接口，一个 Collection 代表一组Object，Java不提供直接继承自Collection的类，只提供继承于它的子接口
  
  
  List
  List接口是一个有序的Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引来访问List中的元素，而且允许有相同的元素
  
  
  Set
  Set具有与Collection完全一样的接口，只是行为上不同，Set不保存重复的元素
  
  
  Queue
  Queue通过先进先出的方式来存储元素，即当获取元素时，最先获得的元素是最先添加的元素，依次递推
  
  
  SortedSet
  继承于Set保存有序的集合
  
  
  Map
  将唯一的键映射到值
  
  
  Map.Entry
  描述在一个Map中的一个元素（键/值对），是一个Map的内部类
  
  
  SortedMap
  继承于Map，使Key保持在升序排列
  
  作者：杨充
  链接：https://juejin.cn/post/6844903858389401608
  来源：掘金
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  ```



* ArrayList 扩容

  ```
  当增加数据的时候，如果ArrayList的大小已经不满足需求时，那么就将数组变为原长度的1.5倍，之后的操作就是把老的数组拷到新的数组里面。例如，默认的数组大小是10，也就是说当我们add10个元素之后，再进行一次add时，就会发生自动扩容，数组长度由10变为了15具体情况如下所示。
  ```

  内部使用的arraycopy(),这是一个浅拷贝

* ArrayList的扩容消耗，ArrayList的List可以扩容吗？如何序列化

  1. ```
     由于ArrayList使用elementData = Arrays.copyOf(elementData, newCapacity);进行扩容，而每次都会重新创建一个newLength长度的数组，所以扩容的空间复杂度为O(n),时间复杂度为O(n)
     ```

  2. ```
     不能，asList返回的List为只读的。其原因为：asList方法返回的ArrayList是Arrays的一个内部类，并且没有实现add，remove等操作
     ```

  3. ```
     List怎么实现排序？
     实现排序，可以使用自定义排序：list.sort(new Comparator(){...})
     或者使用Collections进行快速排序：Collections.sort(list)
     ```

  4. ```
     ArrayList 基于数组实现，并且具有动态扩容特性，因此保存元素的数组不一定都会被使用，那么就没必要全部进行序列化。
     保存元素的数组 elementData 使用 transient 修饰，该关键字声明数组默认不会被序列化。
     ArrayList 实现了 writeObject() 和 readObject() 来控制只序列化数组中有元素填充那部分内容。
     
     序列化时需要使用 ObjectOutputStream 的 writeObject() 将对象转换为字节流并输出。而 writeObject() 方法在传入的对象存在 writeObject() 的时候会去反射调用该对象的 writeObject() 来实现序列化。反序列化使用的是 ObjectInputStream 的 readObject() 方法，原理类似。
     
     ```

* 为何hashMap的数组长度保证为2的幂

  因为 hash后的 值 h， 需要 %（length -1)后才能得到 桶的位置。这样只有当 length 为 2的幂的时候， h &(length -1) 才等价于 h % (length -1 )，如此才能提高查询效率

* #### 为什么HashMap中String、Integer这样的包装类适合作为K？如果要用对象最为key，该如何操作？

  1. ```
     String、Integer等包装类的特性能够保证Hash值的不可更改性和计算准确性，能够有效的减少Hash碰撞的几率
     
     都是final类型，即不可变性，保证key的不可更改性，不会存在获取hash值不同的情况
     内部已重写了equals()、hashCode()等方法，遵守了HashMap内部的规范（不清楚可以去上面看看putValue的过程），不容易出现Hash值计算错误的情况
     ```

  2. ```
     重写hashCode()和equals()方法
     
     重写hashCode()是因为需要计算存储数据的存储位置，需要注意不要试图从散列码计算中排除掉一个对象的关键部分来提高性能，这样虽然能更快但可能会导致更多的Hash碰撞；
     
     重写equals()方法，需要遵守自反性、对称性、传递性、一致性以及对于任何非null的引用值x，x.equals(null)必须返回false的这几个特性，目的是为了保证key在哈希表中的唯一性；
     ```

* TreeMap:

  ```
  TreeMap集合结构特点
      键的数据结构是红黑树,可保证键的排序和唯一性
      排序分为自然排序和比较器排序，如果使用的是自然排序,对元素有要求,要求这个元素需要实现 Comparable 接口
      线程是不安全的效率比较高	
  ```

  ```Java
  public TreeMap(): 自然排序
  public TreeMap(Comparator<? super K> comparator):  使用的是比较器排序
  ```

  