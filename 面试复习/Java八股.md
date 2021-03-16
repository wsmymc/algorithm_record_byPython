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

