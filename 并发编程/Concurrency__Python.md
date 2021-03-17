#  Concurrency__Python



## EASY

### 1. [1114. 按序打印](https://leetcode-cn.com/problems/print-in-order/)

```python
# 我们提供了一个类： 
# 
#  
# public class Foo {
#   public void first() { print("first"); }
#   public void second() { print("second"); }
#   public void third() { print("third"); }
# } 
# 
#  三个不同的线程 A、B、C 将会共用一个 Foo 实例。 
# 
#  
#  一个将会调用 first() 方法 
#  一个将会调用 second() 方法 
#  还有一个将会调用 third() 方法 
#  
# 
#  请设计修改程序，以确保 second() 方法在 first() 方法之后被执行，third() 方法在 second() 方法之后被执行。 
# 
#  
# 
#  示例 1: 
# 
#  
# 输入: [1,2,3]
# 输出: "firstsecondthird"
# 解释: 
# 有三个线程会被异步启动。
# 输入 [1,2,3] 表示线程 A 将会调用 first() 方法，线程 B 将会调用 second() 方法，线程 C 将会调用 third() 方法。
# 正确的输出是 "firstsecondthird"。
#  
# 
#  示例 2: 
# 
#  
# 输入: [1,3,2]
# 输出: "firstsecondthird"
# 解释: 
# 输入 [1,3,2] 表示线程 A 将会调用 first() 方法，线程 B 将会调用 third() 方法，线程 C 将会调用 second() 方法。
# 正确的输出是 "firstsecondthird"。 
# 
#  
# 
#  提示： 
# 
#  
#  尽管输入中的数字似乎暗示了顺序，但是我们并不保证线程在操作系统中的调度顺序。 
#  你看到的输入格式主要是为了确保测试的全面性。 
#  
#  👍 248 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
from threading import Lock
class Foo:
    def __init__(self):
        self.firststep = Lock()  # 构造两个锁并直接获取
        self.secondstep = Lock()
        self.firststep .acquire()
        self.secondstep .acquire()

    def first(self, printFirst: 'Callable[[], None]') -> None:
        
        # printFirst() outputs "first". Do not change or remove this line.
        printFirst()
        self.firststep .release()

    def second(self, printSecond: 'Callable[[], None]') -> None:
        self.firststep .acquire()
        # printSecond() outputs "second". Do not change or remove this line.
        printSecond()
        self.secondstep .release()

    def third(self, printThird: 'Callable[[], None]') -> None:
        self.secondstep .acquire()
        # printThird() outputs "third". Do not change or remove this line.
        printThird()
        self.secondstep .release()
# leetcode submit region end(Prohibit modification and deletion)

```





## medium

### 2. [1115. 交替打印FooBar](https://leetcode-cn.com/problems/print-foobar-alternately/)

```python
import threading 
class FooBar:
    def __init__(self, n):
        self.n = n
        self.s1=threading.Semaphore(1)
        self.s2=threading.Semaphore(0)


    def foo(self, printFoo: 'Callable[[], None]') -> None:
        
        for i in range(self.n):
            
            # printFoo() outputs "foo". Do not change or remove this line.
            self.s1.acquire()
            printFoo()
            self.s2.release()


    def bar(self, printBar: 'Callable[[], None]') -> None:
        
        for i in range(self.n):
            
            # printBar() outputs "bar". Do not change or remove this line.
            self.s2.acquire()
            printBar()
            self.s1.release()
```



### 2. [1195. 交替打印字符串](https://leetcode-cn.com/problems/fizz-buzz-multithreaded/)



```python
import threading
class FizzBuzz:
    def __init__(self, n: int):
        self.n = n
        # 创建4把锁
        self.FizzLock = threading.Lock()
        self.BuzzLock = threading.Lock()
        self.FizzBuzzLock = threading.Lock()
        self.NumberLock = threading.Lock()

        # 将3把锁阻塞
        self.FizzLock.acquire()
        self.BuzzLock.acquire()
        self.FizzBuzzLock.acquire()


    # printFizz() outputs "fizz"
    def fizz(self, printFizz: 'Callable[[], None]') -> None:
        for i in range(1,self.n + 1,1):
            if not i%3 and i%5:
                self.FizzLock.acquire()
                printFizz()
                self.NumberLock.release()
    	

    # printBuzz() outputs "buzz"
    def buzz(self, printBuzz: 'Callable[[], None]') -> None:
        for i in range(1,self.n + 1,1):
            if not i%5 and i%3:
                self.BuzzLock.acquire()
                printBuzz()
                self.NumberLock.release()
    	

    # printFizzBuzz() outputs "fizzbuzz"
    def fizzbuzz(self, printFizzBuzz: 'Callable[[], None]') -> None:
        for i in range(1,self.n + 1,1):
            if not i%(3*5):
                self.FizzBuzzLock.acquire()
                printFizzBuzz()
                self.NumberLock.release()
        

    # printNumber(x) outputs "x", where x is an integer.
    def number(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(1,self.n +1,1):
            ...
            self.NumberLock.acquire()
            if not i%(3*5):
                self.FizzBuzzLock.release()
            elif not i%3:
                self.FizzLock.release()
            elif not i%5:
                self.BuzzLock.release()
            else:
                printNumber(i)
                self.NumberLock.release()


        

作者：ProgrammerPlus
链接：https://leetcode-cn.com/problems/fizz-buzz-multithreaded/solution/zheng-si-ba-suo-feng-kuang-qie-huan-by-programmerp/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



### 3. [1226. 哲学家进餐](https://leetcode-cn.com/problems/the-dining-philosophers/)



```python
from threading import Lock, Semaphore
class DiningPhilosophers:
    def __init__(self):
        self.Limit = Semaphore(2) # 限制最多4个人就餐
        self.ForkLocks = [Lock() for _ in range(5)] # 叉子锁

    def wantsToEat(self,
                   philosopher: int,
                   pickLeftFork: 'Callable[[], None]',
                   pickRightFork: 'Callable[[], None]',
                   eat: 'Callable[[], None]',
                   putLeftFork: 'Callable[[], None]',
                   putRightFork: 'Callable[[], None]') -> None:

        # 左右叉子的编号
        right_fork = philosopher
        left_fork = (philosopher + 1) % 5

        # 就餐人数减一
        self.Limit.acquire()

        # 拿起叉子
        self.ForkLocks[right_fork].acquire()
        self.ForkLocks[left_fork].acquire()
        
        pickLeftFork()
        pickRightFork()
        eat()
        putLeftFork()
        putRightFork()

        #放下叉子
        self.ForkLocks[right_fork].release()
        self.ForkLocks[left_fork].release()

        #就餐人数加一
        self.Limit.release()

作者：leon1996
链接：https://leetcode-cn.com/problems/the-dining-philosophers/solution/python3de-san-chong-jie-fa-by-leon1996-3omw/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 4. [1116. 打印零与奇偶数](https://leetcode-cn.com/problems/print-zero-even-odd/)

```python
# 假设有这么一个类： 
# 
#  class ZeroEvenOdd {
#   public ZeroEvenOdd(int n) { ... }      // 构造函数
#   public void zero(printNumber) { ... }  // 仅打印出 0
#   public void even(printNumber) { ... }  // 仅打印出 偶数
#   public void odd(printNumber) { ... }   // 仅打印出 奇数
# }
#  
# 
#  相同的一个 ZeroEvenOdd 类实例将会传递给三个不同的线程： 
# 
#  
#  线程 A 将调用 zero()，它只输出 0 。 
#  线程 B 将调用 even()，它只输出偶数。 
#  线程 C 将调用 odd()，它只输出奇数。 
#  
# 
#  每个线程都有一个 printNumber 方法来输出一个整数。请修改给出的代码以输出整数序列 010203040506... ，其中序列的长度必须为 2n
# 。 
# 
#  
# 
#  示例 1： 
# 
#  输入：n = 2
# 输出："0102"
# 说明：三条线程异步执行，其中一个调用 zero()，另一个线程调用 even()，最后一个线程调用odd()。正确的输出为 "0102"。
#  
# 
#  示例 2： 
# 
#  输入：n = 5
# 输出："0102030405"
#  
#  👍 90 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
import threading


class ZeroEvenOdd:
    def __init__(self, n):
        # 信号量解法，也可以理解成锁
        self.n = n + 1
        self._zero = threading.Semaphore(1)  # 蛮奇怪的信号量貌似不能使用普通的字符，要么首字母大写，
        # 要么加下划线。否则会爆错 TypeError: 'Semaphore' object is not callable
        self._even = threading.Semaphore(0)
        self._odd = threading.Semaphore(0)

    # printNumber(x) outputs "x", where x is an integer.
    def zero(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(1, self.n):
            self._zero.acquire()
            printNumber(0)
            if i % 2 == 1:
                self._odd.release()
            else:
                self._even.release()

    def even(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(1, self.n):
            if i % 2 == 0:
                self._even.acquire()
                printNumber(i)
                self._zero.release()

    def odd(self, printNumber: 'Callable[[int], None]') -> None:
        for i in range(1, self.n):
            if i % 2 == 1:
                self._odd.acquire()
                printNumber(i)
                self._zero.release()

# leetcode submit region end(Prohibit modification and deletion)

```

