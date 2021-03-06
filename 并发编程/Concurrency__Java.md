# Concurrency__Java

## EASY

### 1.  [1114. 按序打印](https://leetcode-cn.com/problems/print-in-order/)

```java
//我们提供了一个类： 
//
// 
//public class Foo {
//  public void first() { print("first"); }
//  public void second() { print("second"); }
//  public void third() { print("third"); }
//} 
//
// 三个不同的线程 A、B、C 将会共用一个 Foo 实例。 
//
// 
// 一个将会调用 first() 方法 
// 一个将会调用 second() 方法 
// 还有一个将会调用 third() 方法 
// 
//
// 请设计修改程序，以确保 second() 方法在 first() 方法之后被执行，third() 方法在 second() 方法之后被执行。 
//
// 
//
// 示例 1: 
//
// 
//输入: [1,2,3]
//输出: "firstsecondthird"
//解释: 
//有三个线程会被异步启动。
//输入 [1,2,3] 表示线程 A 将会调用 first() 方法，线程 B 将会调用 second() 方法，线程 C 将会调用 third() 方法。
//正确的输出是 "firstsecondthird"。
// 
//
// 示例 2: 
//
// 
//输入: [1,3,2]
//输出: "firstsecondthird"
//解释: 
//输入 [1,3,2] 表示线程 A 将会调用 first() 方法，线程 B 将会调用 third() 方法，线程 C 将会调用 second() 方法。
//正确的输出是 "firstsecondthird"。 
//
// 
//
// 提示： 
//
// 
// 尽管输入中的数字似乎暗示了顺序，但是我们并不保证线程在操作系统中的调度顺序。 
// 你看到的输入格式主要是为了确保测试的全面性。 
// 
// 👍 248 👎 0


import java.util.concurrent.atomic.AtomicInteger;

//leetcode submit region begin(Prohibit modification and deletion)
// 原子类
class Foo {
    private AtomicInteger firstStep = new AtomicInteger(0);
    private AtomicInteger secondStep = new AtomicInteger(0);


    public Foo() {
        
    }

    public void first(Runnable printFirst) throws InterruptedException {
        
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        firstStep.incrementAndGet();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        while (firstStep.get() != 1){
            // 死循环
        }
        
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        secondStep.incrementAndGet();
    }

    public void third(Runnable printThird) throws InterruptedException {
        while (secondStep.get() != 1){
            // 死循环
        }
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
//leetcode submit region end(Prohibit modification and deletion)


// lokc &condition
class Foo {

    int num;
    Lock lock;
    //精确的通知和唤醒线程
    Condition condition1, condition2, condition3;

    public Foo() {
        num = 1;
        lock = new ReentrantLock(); // 针对一个锁，建立三个监视器
        condition1 = lock.newCondition();
        condition2 = lock.newCondition();
        condition3 = lock.newCondition();
    }

    public void first(Runnable printFirst) throws InterruptedException {
        lock.lock(); //获取锁
        try {
            while (num != 1) { // 死循环，只有符合条件才向下走，否则释放锁
                condition1.await();
            }
            // printFirst.run() outputs "first". Do not change or remove this line.
            printFirst.run();
            num = 2;
            condition2.signal();// 通知2
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock(); //需要释放锁，避免死锁
        }
    }

    public void second(Runnable printSecond) throws InterruptedException {
        lock.lock();
        try {
            while (num != 2) {
                condition2.await();
            }
            // printSecond.run() outputs "second". Do not change or remove this line.
            printSecond.run();
            num = 3;
            condition3.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }

    public void third(Runnable printThird) throws InterruptedException {
        lock.lock();
        try {
            while (num != 3) {
                condition3.await();
            }
            // printThird.run() outputs "third". Do not change or remove this line.
            printThird.run();
            num = 1;
            condition1.signal();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}

// 信号量
import java.util.concurrent.Semaphore;

class Foo {

    Semaphore semaphore12, semaphore23;

    public Foo() {
        //初始的允许请求均设为0
        semaphore12 = new Semaphore(0);
        semaphore23 = new Semaphore(0);
    }

    public void first(Runnable printFirst) throws InterruptedException {
        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        //释放一个12的信号量
        semaphore12.release();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        //获取一个12的信号量，没有则阻塞
        semaphore12.acquire();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        //释放一个23的信号量
        semaphore23.release();
    }

    public void third(Runnable printThird) throws InterruptedException {
        //获取一个23的信号量，没有则阻塞
        semaphore23.acquire();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}

// CountDownLatch
import java.util.concurrent.CountDownLatch;

class Foo {

    CountDownLatch countDownLatch12, countDownLatch23;

    public Foo() {
        countDownLatch12 = new CountDownLatch(1);
        countDownLatch23 = new CountDownLatch(1);
    }

    public void first(Runnable printFirst) throws InterruptedException {

        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        countDownLatch12.countDown();
    }

    public void second(Runnable printSecond) throws InterruptedException {
        //等待计数器归零再向下执行
        countDownLatch12.await();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        countDownLatch23.countDown();
    }

    public void third(Runnable printThird) throws InterruptedException {
        //等待计数器归零再向下执行
        countDownLatch23.await();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}
// 阻塞队列
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.SynchronousQueue;

class Foo {
    BlockingQueue<String> blockingQueue12, blockingQueue23;

    public Foo() {
        //同步队列,没有容量，进去一个元素，必须等待取出来以后，才能再往里面放一个元素
        blockingQueue12 = new SynchronousQueue<>();
        blockingQueue23 = new SynchronousQueue<>();
    }

    public void first(Runnable printFirst) throws InterruptedException {

        // printFirst.run() outputs "first". Do not change or remove this line.
        printFirst.run();
        blockingQueue12.put("stop");
    }

    public void second(Runnable printSecond) throws InterruptedException {
        blockingQueue12.take();
        // printSecond.run() outputs "second". Do not change or remove this line.
        printSecond.run();
        blockingQueue23.put("stop");
    }

    public void third(Runnable printThird) throws InterruptedException {
        blockingQueue23.take();
        // printThird.run() outputs "third". Do not change or remove this line.
        printThird.run();
    }
}


```









## medium



### 1. [1115. 交替打印FooBar](https://leetcode-cn.com/problems/print-foobar-alternately/)

```Java
// 信号量解法
class FooBar {
    private int n;
    private Semaphore foo = new Semaphore(1), bar = new Semaphore(0);

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            foo.acquire();

            
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
            bar.release();

        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            bar.acquire();
            // printBar.run() outputs "bar". Do not change or remove this line.
        	printBar.run();
            foo.release();
        }
    }
}

// 2. 阻塞队列
class FooBar {
    private int n;
    private BlockingQueue<Integer> bar = new LinkedBlockingQueue<>(1);
    private BlockingQueue<Integer> foo = new LinkedBlockingQueue<>(1);
    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            foo.put(i);

            
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
            bar.put(i);

        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            bar.take();
            // printBar.run() outputs "bar". Do not change or remove this line.
        	printBar.run();
            foo.take();
        }
    }
}
// 3 . CyclicBarrier
class FooBar {
    private int n;
    CyclicBarrier cb = new CyclicBarrier(2);
    volatile boolean fin =true;
    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
           while (!fin); // 自旋等待

            
        	// printFoo.run() outputs "foo". Do not change or remove this line.
        	printFoo.run();
            fin = false;
            try{
                cb.await();
            }catch (BrokenBarrierException e){}

        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            try {
                cb.await();
            } catch (BrokenBarrierException e) {}
            printBar.run();
            fin = true;

        }
    }
}

// 4. yield
//手少阴心经 自旋 + 让出CPU
class FooBar5 {
    private int n;

    public FooBar5(int n) {
        this.n = n;
    }

    volatile boolean permitFoo = true;

    public void foo(Runnable printFoo) throws InterruptedException {     
        for (int i = 0; i < n; ) {
            if(permitFoo) {
        	    printFoo.run();
            	i++;
            	permitFoo = false;
            }else{
                Thread.yield();
            }
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {       
        for (int i = 0; i < n; ) {
            if(!permitFoo) {
        	printBar.run();
        	i++;
        	permitFoo = true;
            }else{
                Thread.yield();
            }
        }
    }
}


作者：idasmilence
链接：https://leetcode-cn.com/problems/print-foobar-alternately/solution/duo-xian-cheng-liu-mai-shen-jian-ni-xue-d220n/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

//手少阳三焦经 可重入锁 + Condition
class FooBar4 {
    private int n;

    public FooBar4(int n) {
        this.n = n;
    }
    Lock lock = new ReentrantLock(true);
    private final Condition foo = lock.newCondition();
    volatile boolean flag = true;
    public void foo(Runnable printFoo) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            lock.lock();
            try {
            	while(!flag) {
                    foo.await();
                }
                printFoo.run();
                flag = false;
                foo.signal();
            }finally {
            	lock.unlock();
            }
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n;i++) {
            lock.lock();
            try {
            	while(flag) {
                    foo.await();
            	}
                printBar.run();
                flag = true;
                foo.signal();
            }finally {
            	lock.unlock();
            }
        }
    }
}

//手厥阴心包经 synchronized + 标志位 + 唤醒
class FooBar3 {
    private int n;
    // 标志位，控制执行顺序，true执行printFoo，false执行printBar
    private volatile boolean type = true;
    private final Object foo=  new Object(); // 锁标志

    public FooBar3(int n) {
        this.n = n;
    }
    public void foo(Runnable printFoo) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            synchronized (foo) {
                while(!type){
                    foo.wait();
                }
                printFoo.run();
                type = false;
                foo.notifyAll();
            }
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        for (int i = 0; i < n; i++) {
            synchronized (foo) {
                while(type){
                    foo.wait();
                }
                printBar.run();
                type = true;
                foo.notifyAll();
            }
        }
    }
}

作者：idasmilence
链接：https://leetcode-cn.com/problems/print-foobar-alternately/solution/duo-xian-cheng-liu-mai-shen-jian-ni-xue-d220n/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 2. [1195. 交替打印字符串](https://leetcode-cn.com/problems/fizz-buzz-multithreaded/)

```java
class FizzBuzz {
    private int n;
    private int i;
    private Object lock = new Object();

    public FizzBuzz(int n) {
        this.n = n;
        this.i = 1;
    }

    // printFizz.run() outputs "fizz".
    public void fizz(Runnable printFizz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 == 0 && i % 5 != 0) {
                    printFizz.run();
                    i++; 
                }
            }
        }
        
    }

    // printBuzz.run() outputs "buzz".
    public void buzz(Runnable printBuzz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 != 0 && i % 5 == 0) {
                    printBuzz.run();
                    i++;
                } 
            }
        }
        
    }

    // printFizzBuzz.run() outputs "fizzbuzz".
    public void fizzbuzz(Runnable printFizzBuzz) throws InterruptedException {
        while (i <= n) {
            synchronized(lock) {
                while (i <= n && i % 3 == 0 && i % 5 == 0) {
                    printFizzBuzz.run();
                    i++;
                }
            }
        }
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void number(IntConsumer printNumber) throws InterruptedException {
        while (i <= n) {
             synchronized(lock) {
                while (i <= n && i % 3 != 0 && i % 5 != 0) {
                    printNumber.accept(i);
                    i++; 
                }
            }
        }
       
    }
}


```



### 3. [1226. 哲学家进餐](https://leetcode-cn.com/problems/the-dining-philosophers/)

```Java
class DiningPhilosophers {
    ReentrantLock[] forks = new ReentrantLock[5];

    public DiningPhilosophers() {
        for (int i=0;i<5;i++){
            forks[i] = new ReentrantLock();
        }
        
    }

    // call the run() method of any runnable to execute its code
    public void wantsToEat(int philosopher,
                           Runnable pickLeftFork,
                           Runnable pickRightFork,
                           Runnable eat,
                           Runnable putLeftFork,
                           Runnable putRightFork) throws InterruptedException {
                                int fork1 = philosopher; 
                                int fork2 = (philosopher + 1) % 5;

                                forks[Math.min(fork1, fork2)].lock();
                                forks[Math.max(fork1, fork2)].lock();
                                pickLeftFork.run();
                                pickRightFork.run();
                                eat.run();
                                putLeftFork.run();
                                putRightFork.run();
                                forks[Math.min(fork1, fork2)].unlock();
                                forks[Math.max(fork1, fork2)].unlock();


        
    }
}
```

### 4. [1116. 打印零与奇偶数](https://leetcode-cn.com/problems/print-zero-even-odd/)

```java
//假设有这么一个类： 
//
// class ZeroEvenOdd {
//  public ZeroEvenOdd(int n) { ... }      // 构造函数
//  public void zero(printNumber) { ... }  // 仅打印出 0
//  public void even(printNumber) { ... }  // 仅打印出 偶数
//  public void odd(printNumber) { ... }   // 仅打印出 奇数
//}
// 
//
// 相同的一个 ZeroEvenOdd 类实例将会传递给三个不同的线程： 
//
// 
// 线程 A 将调用 zero()，它只输出 0 。 
// 线程 B 将调用 even()，它只输出偶数。 
// 线程 C 将调用 odd()，它只输出奇数。 
// 
//
// 每个线程都有一个 printNumber 方法来输出一个整数。请修改给出的代码以输出整数序列 010203040506... ，其中序列的长度必须为 2n
//。 
//
// 
//
// 示例 1： 
//
// 输入：n = 2
//输出："0102"
//说明：三条线程异步执行，其中一个调用 zero()，另一个线程调用 even()，最后一个线程调用odd()。正确的输出为 "0102"。
// 
//
// 示例 2： 
//
// 输入：n = 5
//输出："0102030405"
// 
// 👍 90 👎 0


import java.util.concurrent.Semaphore;

//leetcode submit region begin(Prohibit modification and deletion)
class ZeroEvenOdd {
    // 信号量解法
    private int n;
    private Semaphore zero = new Semaphore(1);
    private Semaphore even = new Semaphore(0);
    private Semaphore odd = new Semaphore(0);
    
    public ZeroEvenOdd(int n) {
        this.n = n;
    }

    // printNumber.accept(x) outputs "x", where x is an integer.
    public void zero(IntConsumer printNumber) throws InterruptedException{
        for (int i=1;i<=n;i++){
            zero.acquire();
            printNumber.accept(0);
            if( (i&1) ==1){
                odd.release();
            }else{
                even.release();
            }
        }
        
    }

    public void even(IntConsumer printNumber) throws InterruptedException {
        for(int i=2;i<=n;i+=2){
            even.acquire();
            printNumber.accept(i);
            zero.release();
        }
        
    }

    public void odd(IntConsumer printNumber) throws InterruptedException {
        for(int i=1;i<=n;i+=2){
            odd.acquire();
            printNumber.accept(i);
            zero.release();
        }
        
    }
}
//leetcode submit region end(Prohibit modification and deletion)

```





### 5. [1117. H2O 生成](https://leetcode-cn.com/problems/building-h2o/)

```java
//现在有两种线程，氧 oxygen 和氢 hydrogen，你的目标是组织这两种线程来产生水分子。 
//
// 存在一个屏障（barrier）使得每个线程必须等候直到一个完整水分子能够被产生出来。 
//
// 氢和氧线程会被分别给予 releaseHydrogen 和 releaseOxygen 方法来允许它们突破屏障。 
//
// 这些线程应该三三成组突破屏障并能立即组合产生一个水分子。 
//
// 你必须保证产生一个水分子所需线程的结合必须发生在下一个水分子产生之前。 
//
// 换句话说: 
//
// 
// 如果一个氧线程到达屏障时没有氢线程到达，它必须等候直到两个氢线程到达。 
// 如果一个氢线程到达屏障时没有其它线程到达，它必须等候直到一个氧线程和另一个氢线程到达。 
// 
//
// 书写满足这些限制条件的氢、氧线程同步代码。 
//
// 
//
// 示例 1: 
//
// 输入: "HOH"
//输出: "HHO"
//解释: "HOH" 和 "OHH" 依然都是有效解。
// 
//
// 示例 2: 
//
// 输入: "OOHHHH"
//输出: "HHOHHO"
//解释: "HOHHHO", "OHHHHO", "HHOHOH", "HOHHOH", "OHHHOH", "HHOOHH", "HOHOHH" 和 "OH
//HOHH" 依然都是有效解。
// 
//
// 
//
// 提示： 
//
// 
// 输入字符串的总长将会是 3n, 1 ≤ n ≤ 50； 
// 输入字符串中的 “H” 总数将会是 2n 。 
// 输入字符串中的 “O” 总数将会是 n 。 
// 
// 👍 82 👎 0





//leetcode submit region begin(Prohibit modification and deletion)

// ReentrantLock  + Condition
import java.util.concurrent.locks.ReentrantLock;
class H2O {
    volatile int h_cnt = 0;
    ReentrantLock lock = new ReentrantLock();
    Condition O = lock.newCondition();
    Condition H = lock.newCondition();

    public H2O() {
        
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        lock.lock();
		try{
		    while(h_cnt == 2){
                O.signal();
		        H.await();  // 会释放锁
            }
            // releaseHydrogen.run() outputs "H". Do not change or remove this line.
            releaseHydrogen.run();
		    h_cnt ++;
		    if( h_cnt ==2){
		        O.signal();
            }
        }finally{
            lock.unlock();
        }
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        lock.lock();
        try {
            while(h_cnt != 2) O.await();

            // releaseOxygen.run() outputs "O". Do not change or remove this line.
            releaseOxygen.run();
            h_cnt = 0;
            H.signalAll();
        }finally {
            lock.unlock();
        }
    }
}
//leetcode submit region end(Prohibit modification and deletion)


class H2O {
    Semaphore H = new Semaphore(2);
    Semaphore O = new Semaphore(1);
    CyclicBarrier cb = new CyclicBarrier(3, ()->{ //归零后,重新生成
        H.release(2);
        O.release(1);
    });

    public H2O() {
        
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        H.acquire();
		
        // releaseHydrogen.run() outputs "H". Do not change or remove this line.
        releaseHydrogen.run();
        try { 
            cb.await();  // 每一次await 就 -1
        }catch (Exception e){}
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        O.acquire();
        // releaseOxygen.run() outputs "O". Do not change or remove this line.
		releaseOxygen.run();
        try {
            cb.await();
        }catch (Exception e){}

    }
}

//现在有两种线程，氧 oxygen 和氢 hydrogen，你的目标是组织这两种线程来产生水分子。 
//
// 存在一个屏障（barrier）使得每个线程必须等候直到一个完整水分子能够被产生出来。 
//
// 氢和氧线程会被分别给予 releaseHydrogen 和 releaseOxygen 方法来允许它们突破屏障。 
//
// 这些线程应该三三成组突破屏障并能立即组合产生一个水分子。 
//
// 你必须保证产生一个水分子所需线程的结合必须发生在下一个水分子产生之前。 
//
// 换句话说: 
//
// 
// 如果一个氧线程到达屏障时没有氢线程到达，它必须等候直到两个氢线程到达。 
// 如果一个氢线程到达屏障时没有其它线程到达，它必须等候直到一个氧线程和另一个氢线程到达。 
// 
//
// 书写满足这些限制条件的氢、氧线程同步代码。 
//
// 
//
// 示例 1: 
//
// 输入: "HOH"
//输出: "HHO"
//解释: "HOH" 和 "OHH" 依然都是有效解。
// 
//
// 示例 2: 
//
// 输入: "OOHHHH"
//输出: "HHOHHO"
//解释: "HOHHHO", "OHHHHO", "HHOHOH", "HOHHOH", "OHHHOH", "HHOOHH", "HOHOHH" 和 "OH
//HOHH" 依然都是有效解。
// 
//
// 
//
// 提示： 
//
// 
// 输入字符串的总长将会是 3n, 1 ≤ n ≤ 50； 
// 输入字符串中的 “H” 总数将会是 2n 。 
// 输入字符串中的 “O” 总数将会是 n 。 
// 
// 👍 82 👎 0


import java.util.concurrent.CyclicBarrier;
import java.util.concurrent.Semaphore;

//leetcode submit region begin(Prohibit modification and deletion)
class H2O {

    Semaphore hSmp = new Semaphore(2);
    Semaphore oSmp = new Semaphore(0);

    volatile int hNum = 0;

    public H2O() {

    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        hSmp.acquire();
        releaseHydrogen.run();
        hNum++;
        if (hNum == 2) {
            oSmp.release(1);
        }
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        oSmp.acquire();
        releaseOxygen.run();
        hNum = 0;
        hSmp.release(2);
    }
}

//leetcode submit region end(Prohibit modification and deletion)

```

