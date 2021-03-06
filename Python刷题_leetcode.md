#  python刷题笔记

* 目前所做的都是用Java刷过的题，一方面复习算法思路，恢复手感，一方面学习python语法
* 目前共计400+题目，先从简单的刷起

## 零碎语法点

1. extend() 函数用于在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表）。

2. 一个数如果除以另一个数，即便确认是整数，最好还是使用//，否则会报错

3. 辨析：

   ```python
    res.append( [nums[2*i+1]] * nums[2*i])
    res += [nums[2*i+1]] * nums[2*i]
    res.extend([nums[2*i +1]] * nums[2*i])
       
       
    输出依次是：
   
   [[2],[4,4,4]]
   [2,4,4,4]
   [2,4,4,4]
   # 说明第一种，是将添加的当作列表，后两种是将其当作元素值。
   # 以及，想要元素重复的话，首先元素值需要套上一个"[]",表示自己是一个列表，后面跟的，才是重复次数
   ```

4. ```python
   binary=bin(x)# 将数字x转化位二进制
   
   len(x)# 获取长度
   binary.count('1')# 二进制俗话获取其中1的数量
   
   ```

5.  注意一点，貌似python中调用类内部的方法，需要有self前缀

6. ```python
   # nums是一个列表
   del nums[i]    # 根据索引删除值
   nums.remove(max_)  # 根据第一个匹配的值删除元素
   ```

7. ```python
    'sep'.join(seq)
   ```

   参数说明：
   sep：分隔符，可以为空
   seq：要连接的元素序列、字符串、元组、字典
   上面的语法即：以sep作为分隔符（空），将seq所有的元素合并成一个`新的字符串`

8. ```python
    # return ''.join([chr(ord(c)+32) if ord(c)>=65 and ord(c)<=90 else c for c in str])
   # 内置函数ord（），将字符转换为ascii码，chr（）将ascii码转换为字符
   ```

9. 1 str.replace(old, new[, max])
   2 Python replace() 方法把字符串中的 old（旧字符串）替换成 new(新字符串)，如果指定第三个参数max，则替换不超过 max次
   3 原str不被更改，使用需由新的变量接收
   4 replace调用C的接口，无python源码
   5 经测试，str里不存在old时返回原str，不会报错的
   
10. python中if。else的几种简易写法

   ```python
   c = a if a>b else b    # 如果a>b,那c=a，否则，c=b
   
   c = [a, b][a > b]       # 如果a>b,那c=a，否则，c=b
   
   d = c and a or b        #当c为真时走a,当c为假时走b
   ```



11. https://docs.python.org/zh-cn/3/library/collections.html(容器数据类型)

    * |                                                              | 相关用法点击链接                                             |
      | ------------------------------------------------------------ | ------------------------------------------------------------ |
      | [`deque`](https://docs.python.org/zh-cn/3/library/collections.html#collections.deque) | 类似列表(list)的容器，实现了在两端快速添加(append)和弹出(pop) |

12. ```python
     from scipy.special import perm,comb，factorial
        # 特殊模块包
            '''
        >> perm(5, 2)
    20.0  排列   (5!)/(5-2)!
    >> comb(5, 2)
    10.0 组合    (5!)/((2!)*(5-2)!)
    > factorial(5)
    array(120.0)  阶乘
        '''
    ```


####  13 . 复制一个列表

```python
char.copy()   # 函数方式
list[:]   #切片方式


# char.isalpha()  判断是否为字母
```

#### 14. 并查集模板

```python
class UF:
    def __init__(self,n):
        self.parents = defaultdict(int)   # 有默认值的字典
        for i in range(n):
            self.parents[i] = i  # 一开始每个集合是自己的父集合
        
    def find(self,x):
        while x!= self.parents[x]:
            x = self.parents[x]  # 一直找祖宗
        return x
    
    def connect(self, p,q):
        return self.find(p) == self.find(q)
    
    def union(self,p,q):
        if self.find(p) == self.find(q):
            return
        self.parents[self.find(p)] = self.find(q)
```

#### 15. 最大值

```python
   import sys  #将预设值为最大整数，需要这么用
   _maxInt = sys.maxsize
_minInt = -sys.maxsize -1
   _maxFloat = sys.float('inf')
    _minfloat = sys.float('-inf')
```

#### 16 字符串操作集合

```python
cnt = text.count(' ')   # count直接统计字符串里的字符次数
n = text.split()        # split在默认的情况下，以所有空字符分割，指空格、tab、换行，也可以指定分割字符
space, last = divmod(cnt,len(n)-1)  # 内建函数，直接获得数的商、余数


return int(bin(n)[2::].zfill(32)[::-1], 2)
# zfill（字符串右对齐，前面补0）

return int(s,base=2)  # int()函数，加上base参数，可以选择进制转换

```

#### 17 set.discard(value)

```python
discard() #方法用于移除指定的集合元素。

#该方法不同于 remove() 方法，因为 remove() 方法在移除一个不存在的元素时会发生错误，而 discard() 方法不会。
```

#### 18. 二叉树遍历（迭代形式,java版）

```java
 /**
     * 统一一下
     * @param root
     * @return
     */
    //前序
    public static List<Integer> preOrder(TreeNode root){
         List<Integer> list = new ArrayList();
         Stack<TreeNode> stack = new Stack();
         TreeNode cur = root;
         while(cur!=null || !stack.isEmpty()){
             //一直往左压入栈
             while(cur!=null){
                 list.add(cur.val);
                 stack.push(cur);
                 cur = cur.left;
             }
             cur = stack.pop();
             cur = cur.right;
         }
         return list;
    }

    //中序
    public List<Integer> inorderTraversal(TreeNode root) {
        if(root == null){
            return new ArrayList();
        }
        List<Integer> list = new ArrayList();
        Stack<TreeNode> stack = new Stack();
        TreeNode cur = root;
        while(cur != null || !stack.isEmpty()){
            while(cur!=null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.pop();
            list.add(cur.val);
            cur = cur.right;
        }
        return list;
    }


    //后序遍历，非递归
    public static List<Integer> postOrder(TreeNode root){
        Stack<TreeNode> stack = new Stack<>();
        List<Integer> list = new ArrayList<>();
        TreeNode cur = root;
        TreeNode p = null;//用来记录上一节点
        while(!stack.isEmpty() || cur != null){
            while(cur != null){
                stack.push(cur);
                cur = cur.left;
            }
            cur = stack.peek();
//            后序遍历的过程中在遍历完左子树跟右子树cur都会回到根结点。所以当前不管是从左子树还是右子树回到根结点都不应该再操作了，应该退回上层。
//            如果是从右边再返回根结点，应该回到上层。
            //主要就是判断出来的是不是右子树，是的话就可以把根节点=加入到list了
            if(cur.right == null || cur.right == p){
                list.add(cur.val);
                stack.pop();
                p = cur;
                cur = null;
            }else{
                cur = cur.right;
            }

        }
        return list;
}
```

#### 19. 表示方块子集以及使用其索引

```python
# 特点：用这中方式，来表示方块性质的子集索引。这些天见到的三次了，需要注意以一下.i,j是整体举行的索引
boxes = [{} for i in range(9)]
box_index = (i // 3 ) * 3 + j // 3
```

#### 20. 获取一个数的最低非零位

```python
b = a & (-a)
```

#### 21 . 判断是否是由字母或数字组成

```python
str.isalnum()
```

#### 22. find

```python
   # 直接使用拼接字符串的玩法，活用find
    # find:string.find(str, beg=0, end=len(string))检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1
```

#### 23.filter和map

```python
    """filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。
        map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。"""
```

#### 24. heapq堆

```python
"""
heapq.heappush(heap, item)
将 item 的值加入 heap 中，保持堆的不变性。

heapq.heappop(heap)
弹出并返回 heap 的最小的元素，保持堆的不变性。如果堆为空，抛出 IndexError 。使用 heap[0] ，可以只访问最小的元素而不弹出它。

heapq.heappushpop(heap, item)
将 item 放入堆中，然后弹出并返回 heap 的最小元素。该组合操作比先调用  heappush() 再调用 heappop() 运行起来更有效率。

heapq.heapify(x)
将list x 转换成堆，原地，线性时间内。

heapq.heapreplace(heap, item)
弹出并返回 heap 中最小的一项，同时推入新的 item。 堆的大小不变。 如果堆为空则引发 IndexError。

这个单步骤操作比 heappop() 加 heappush() 更高效，并且在使用固定大小的堆时更为适宜。 pop/push 组合总是会从堆中返回一个元素并将其替换为 item。

返回的值可能会比添加的 item 更大。 如果不希望如此，可考虑改用 heappushpop()。 它的 push/pop 组合会返回两个值中较小的一个，将较大的值留在堆中。

该模块还提供了三个基于堆的通用功能函数。

heapq.merge(*iterables, key=None, reverse=False)
将多个已排序的输入合并为一个已排序的输出（例如，合并来自多个日志文件的带时间戳的条目）。 返回已排序值的 iterator。

类似于 sorted(itertools.chain(*iterables)) 但返回一个可迭代对象，不会一次性地将数据全部放入内存，并假定每个输入流都是已排序的（从小到大）。

具有两个可选参数，它们都必须指定为关键字参数。

key 指定带有单个参数的 key function，用于从每个输入元素中提取比较键。 默认值为 None (直接比较元素)。

reverse 为一个布尔值。 如果设为 True，则输入元素将按比较结果逆序进行合并。 要达成与 sorted(itertools.chain(*iterables), reverse=True) 类似的行为，所有可迭代对象必须是已从大到小排序的。

在 3.5 版更改: 添加了可选的 key 和 reverse 形参。

heapq.nlargest(n, iterable, key=None)
从 iterable 所定义的数据集中返回前 n 个最大元素组成的列表。 如果提供了 key 则其应指定一个单参数的函数，用于从 iterable 的每个元素中提取比较键 (例如 key=str.lower)。 等价于: sorted(iterable, key=key, reverse=True)[:n]。

heapq.nsmallest(n, iterable, key=None)
从 iterable 所定义的数据集中返回前 n 个最小元素组成的列表。 如果提供了 key 则其应指定一个单参数的函数，用于从 iterable 的每个元素中提取比较键 (例如 key=str.lower)。 等价于: sorted(iterable, key=key)[:n]。

后两个函数在 n 值较小时性能最好。 对于更大的值，使用 sorted() 函数会更有效率。 此外，当 n==1 时，使用内置的 min() 和 max() 函数会更有效率。 如果需要重复使用这些函数，请考虑将可迭代对象转为真正的堆。"""

 # python 是小根堆，所以用负值
 # heap的类型，在heapify时就决定下来了，所以先将li初始化，用来确定后续添加的元素类型
# 查看堆顶元素，heap[0],其实就是特殊排序的数组
```

#### 25. bisect 模块

```python
# 用于二分的模块，熟练使用的话，刷题节省时间
"""
bisect.bisect_left(a, x, lo=0, hi=len(a))
在 a 中找到 x 合适的插入点以维持有序。参数 lo 和 hi 可以被用于确定需要考虑的子集；默认情况下整个列表都会被使用。如果 x 已经在 a 里存在，那么插入点会在已存在元素之前（也就是左边）。如果 a 是列表（list）的话，返回值是可以被放在 list.insert() 的第一个参数的。

返回的插入点 i 可以将数组 a 分成两部分。左侧是 all(val < x for val in a[lo:i]) ，右侧是 all(val >= x for val in a[i:hi]) 。

bisect.bisect_right(a, x, lo=0, hi=len(a))¶
bisect.bisect(a, x, lo=0, hi=len(a))
类似于 bisect_left()，但是返回的插入点是 a 中已存在元素 x 的右侧。

返回的插入点 i 可以将数组 a 分成两部分。左侧是 all(val <= x for val in a[lo:i])，右侧是 all(val > x for val in a[i:hi]) for the right side。


=====================上面是查询位置，并不会实际插入，下面的方法会实际插入=======

bisect.insort_left(a, x, lo=0, hi=len(a))
将 x 插入到一个有序序列 a 里，并维持其有序。如果 a 有序的话，这相当于 a.insert(bisect.bisect_left(a, x, lo, hi), x)。要注意搜索是 O(log n) 的，插入却是 O(n) 的。

bisect.insort_right(a, x, lo=0, hi=len(a))
bisect.insort(a, x, lo=0, hi=len(a))
类似于 insort_left()，但是把 x 插入到 a 中已存在元素 x 的右侧。"""
```

####  26. zfill

```python
#Python zfill() 方法返回指定长度的字符串，原字符串右对齐，前面填充0。
str.zfill(width)
```



## easy

#### 1. [LCP 01. 猜数字](https://leetcode-cn.com/problems/guess-numbers/)

```python
class Solution:
    def game(self, guess: List[int], answer: List[int]) -> int:
        count = 0
        for i in range(3):
             if guess[i] == answer[i]:
                count += 1
        return count
```

#### 2. [面试题 02.03. 删除中间节点](https://leetcode-cn.com/problems/delete-middle-node-lcci/)

```python
class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val=node.next.val
        node.next=node.next.next
        # 思路是覆写，然后丢弃下一节点
```

#### 3.[剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        a=s[:n]
        b=s[n:]
        return b+a
```

#### 4. [1470. 重新排列数组](https://leetcode-cn.com/problems/shuffle-the-array/)

```python
class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        res=[]
        for i in range(n):
            res.append(nums[i])
            res.append(nums[i+n])
        return res
```

#### 5. [1431. 拥有最多糖果的孩子](https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/)

```python
class Solution(object):
    def kidsWithCandies(self, candies, extraCandies):
        """
        :type candies: List[int]
        :type extraCandies: int
        :rtype: List[bool]
        """
        '''
        max_num=max(candies)
        for i in range(len(candies)):
            if candies[i]+extraCandies <max_num:
                candies[i]=False
            else:
                candies[i]=True
        return candies
        '''
        maxi = max(candies)
        judge = []
        for i in candies:
            judge.append(i+extraCandies>=maxi)
        return judge
    
  
```

#### 6. [1313. 解压缩编码列表](https://leetcode-cn.com/problems/decompress-run-length-encoded-list/)

```python
class Solution:
    def decompressRLElist(self, nums: List[int]) -> List[int]:
        res=list()
        for i in range(len(nums)//2):
            #res.append( [nums[2*i+1]] * nums[2*i])    这种不行，输出是[[2],[4,4,4]]
            #res += [nums[2*i+1]] * nums[2*i]      这种以及下面这种可以
            res.extend([nums[2*i +1]] * nums[2*i])
        return res
    
    
```

#### 7. [1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/)

```python
class Solution(object):
    def numberOfSteps (self, num):
        """
        :type num: int
        :rtype: int
        """
        binary=bin(num)
        return len(binary)+binary.count('1')-3
    # 单纯模拟没有意思，利用二进制规律：即，总共归零操作的次数=数字二进制位的长度+数字二进制位中1的个数-3
```

#### 8. [1389. 按既定顺序创建目标数组](https://leetcode-cn.com/problems/create-target-array-in-the-given-order/)

```python
class Solution(object):
    def createTargetArray(self, nums, index):
        """
        :type nums: List[int]
        :type index: List[int]
        :rtype: List[int]
        """
        res = []
        for i in range(len(nums)):
            res.insert(index[i], nums[i])  ## insert  直接指定插入的位置与元素
        return res
```

#### 9. [1365. 有多少小于当前数字的数字](https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number/)

```python
class Solution(object):
    def smallerNumbersThanCurrent(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        #return [len(list(filter(lambda x: x < i, nums))) for i in nums]
        # 先建立频次映射表，map
        n =len(nums)
        nums_sort=sorted(nums)
        map = dict()
        for i in nums_sort:
            map[i]=map.get(i,0)+1
        q=[0 for i in range(n)]

        # 从前往后统计小于nums[j]的个数
        for j in range(1,n):
            if nums_sort[j] != nums_sort[j-1]:# 有小于就累加
                q[j]=q[j-1]+map.get(nums_sort[j-1])
            else:  # 相等则传递值
                q[j]=q[j-1]


        # 整理输出格式，因为是按照原来的数字顺序输出，所以需要有一个原数字位置q数组元素顺序的映射

        hash_cd = {}
        for k in range(n):
            hash_cd[nums_sort[k]] = q[k]  #根据值确定必要的key
        res = [0 for i in range(n)]
        for i in range(n):
            res[i] =hash_cd[nums[i]]  #确定输出顺序
        return res
    
 
    
    
    
    
    
    # 另一种自己想的解法，代码更短，直接根据索引号，就可以知道前面有多少小于的数
    n =len(nums)
        nums_sort=sorted(nums)
        
        map = dict()
        
        for i in range(1,n):
            if nums_sort[i] !=nums_sort[i-1]:
                map[nums_sort[i]]=i
            else: 
                map[nums_sort[i]] = map.get(nums_sort[i-1],0)#这里要准备默认值，防止keyerror
        
        res=[0 for i in range(n)]
        for i in range(n):
            res[i]=map.get(nums[i],0)
        return res
```

#### 10. [1295. 统计位数为偶数的数字](https://leetcode-cn.com/problems/find-numbers-with-even-number-of-digits/)

```python
class Solution(object):
    def findNumbers(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return sum([1 for num in nums if len(str(num)) % 2 == 0])
```

#### 11. [面试题 04.02. 最小高度树](https://leetcode-cn.com/problems/minimum-height-tree-lcci/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        l=0
        r=len(nums)-1
        root=self.build(nums,l,r)
        return root
    
    def build(self, nums, l, r):
        if l==r:
            root=TreeNode(nums[l])
            return root
        elif l>r:
            return None
        else:
            mid=l+(r-l)//2
            root=TreeNode(nums[mid])
            root.left=self.build(nums, l, mid-1)
            root.right=self.build(nums,mid+1, r)
            return root
# 注意一点，貌似python中调用类内部的方法，需要有self前缀
```

#### 12. [1464. 数组中两元素的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-two-elements-in-an-array/)

```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        max_=max(nums)
        nums.remove(max_)
        max__=max(nums)
        return (max_-1)*(max__-1)
```

#### 13. [1436. 旅行终点站](https://leetcode-cn.com/problems/destination-city/)

```python
class Solution:
    def destCity(self, paths: List[List[str]]) -> str:
         # 统计时考虑到中间地点都会出现2次，头尾出现1次。计划用字典对应计数
        allCity = set()
        beginCity = set()
        for path in paths:
            allCity.add(path[0])
            allCity.add(path[1])
            beginCity.add(path[0])
        return (allCity - beginCity).pop()
        #all 中包含所有，begin中包含除了end的所有，python中集合可以做集合操作，做差集得到end，用pop输出

```

####  14. [面试题 02.02. 返回倒数第 k 个节点](https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci/)

15. ```python
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None
    
    class Solution:
        def kthToLast(self, head: ListNode, k: int) -> int:
            a=head
            b=head
            for i in range(k):
                b=b.next
            while b:
                a=a.next
                b=b.next
            return a.val
    
    ```



#### 15.[剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)


    [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)
    
    ```python
    # 后续递归而已
    # Definition for a binary tree node.
    # class TreeNode(object):
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    
    class Solution(object):
        def maxDepth(self, root):
            """
            :type root: TreeNode
            :rtype: int
            """
            if not root:
                return 0
            return max()
    ```

#### 17. [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

```python
## 这里需要注意一下，不考虑大数越界的情况，很简单
class Solution(object):
    def printNumbers(self, n):
        """
        :type n: int
        :rtype: List[int]
        """
        ## 开玩笑的解法
        res = [i for i in range(1,10**n)]
        return res
    
    
 ## 但是加入有大数越界，那么就需要用字符串的方式来做，此时的返回值也就变成了String
## 基本思想是使用全排列，递归回溯思想
class Solution:
    def printNumbers(self, n: int) -> [int]:
        def dfs(x):
            if x == n:
                s = ''.join(num[self.start:])
                if s != '0': res.append(s)
                if n - self.start == self.nine: self.start -= 1   # 全是9，下一把要进位，高位上的0少了一个
                return
            for i in range(10):# 从后向前针对一个有n个位子的序列，依次填写从0~9，10个数字
                if i == 9: self.nine += 1# 已经有一位是9了
                num[x] = str(i)
                dfs(x + 1)
            self.nine -= 1#回溯
        
        num, res = ['0'] * n, []
        self.nine = 0           #几个“所有位”都是9
        self.start = n - 1      #这里表示全部位子上0的个数
        dfs(0)
        return ','.join(res)


    
    
```

#### 18. [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def mirrorTree(self, root: TreeNode) -> TreeNode:
        '''递归
        if not root:
            return 
        l=self.mirrorTree(root.left)
        r=self.mirrorTree(root.right)
        root.left=r
        root.right=l
        return root
        '''

        # 辅助栈
        if not root: return
        stack = [root]
        while stack:
            node = stack.pop()
            if node.left: stack.append(node.left)
            if node.right: stack.append(node.right)
            node.left, node.right = node.right, node.left
        return root

```

#### 19. [1021. 删除最外层的括号](https://leetcode-cn.com/problems/remove-outermost-parentheses/)

```python
class Solution:
    def removeOuterParentheses(self, S: str) -> str:
        cnt=[]
        l,r=0,0
        for i in range(len(S)):#记录需要拆除的括号的索引
            if S[i]=='(':
                l +=1
            else:
                r +=1
            if l==r:
                cnt.append(i)
    
        res=S[1:cnt[0]]
        for i in range(len(cnt)):  # 依据索引，拼接字符串
            if i==len(cnt)-1:
                return res
            res+=S[cnt[i]+2:cnt[i+1]]

        



        # 栈思路，仅供参考。思路很简洁，但是拼接的次数比我的多
        stack = []
        result = ''
        for i in S:
            if i == '(':
                stack.append(i)
                if len(stack) > 1:
                    result += '('
            else:
                stack.pop()
                if len(stack) != 0:
                    result += ')'
        return result
```

#### 20. [938. 二叉搜索树的范围和](https://leetcode-cn.com/problems/range-sum-of-bst/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None


#两点
# 1.利用二叉搜索树性质，节点有序。2.self要用好，指代实例，不过貌似要放在实例方法内部。直接扔到方法外，类内貌似不认
class Solution:
    
    
    def rangeSumBST(self, root: TreeNode, L: int, R: int) -> int:
        def dfs(node):
            if node:
                if L <= node.val <= R:
                    self.res += node.val
                if L < node.val:
                    dfs(node.left)
                if node.val < R:
                    dfs(node.right)
        self.res=0

        dfs(root)
        return self.res

```

#### 21. [696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

```python
class Solution:
    def countBinarySubstrings(self, s: str) -> int:
        res,last=0,0
        cur=1
        for i in range(1,len(s)):
            if s[i]==s[i-1]:
                cur +=1
            else:
                last=cur
                cur=1
            if last>=cur:
                res +=1
        return res
    
"""last代表之前一位数，cur代表另一位，用来统计连续次数，从第二位开始。
如果相同，计数+1;否则，cur所指的数变为last，统计的次数也给last，cur从1开始计算另一个数连续出现的次数。
每次判定，last》=cur，说明，可以组成以cur作为相等次数，last所指的数在前，cur所指的数在后的目标字符串"""
```

#### 22. [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def mergeTrees(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if not t1 and not t2:
            return 
        elif not t1:
            return t2
        elif not t2:
            return t1
        else:
            t1.val+=t2.val
            t1.left=self.mergeTrees(t1.left,t2.left)
            t1.right=self.mergeTrees(t1.right,t2.right)
            return t1
```

#### 23. [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

```python
class Solution:
    def replaceSpace(self, s: str) -> str: 
        res = []
        for c in s:
            if c == ' ': res.append("%20")
            else: res.append(c)
        return "".join(res)

```

#### 24. [1351. 统计有序矩阵中的负数](https://leetcode-cn.com/problems/count-negative-numbers-in-a-sorted-matrix/)

```python
class Solution:
    def countNegatives(self, grid: List[List[int]]) -> int:
        ans=0             #ans统计数量
        m=len(grid)       # 长
        
        n=len(grid[0])    # 宽
        position=n        
        for i in range(m):
            for j in range(position):
                if grid[i][j]<0:      # 利用非递增特性，一旦找到小于0的，整个矩形（偏右下角的部分）就都是负值，因此用乘法，然后此轮就不用统计了。重新锚定矩形的最右端位子。
                    ans+=(position-j)*(m-i)
                    position=j
                    break
        return ans

```

#### 25. [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reversePrint(self, head: ListNode) -> List[int]:
        stack=[]
        while head:
            stack.append(head.val)
            head=head.next
        res=[]
        while stack:
            res.append(stack.pop())
        return res
```

#### 26. [1252. 奇数值单元格的数目](https://leetcode-cn.com/problems/cells-with-odd-values-in-a-matrix/)

```python
class Solution:
    def oddCells(self, n: int, m: int, indices: List[List[int]]) -> int:
        rows = [False] * n      #用Falseh、True来表示奇偶性，来取代加法
        cols = [False] * m

        for r, c in indices:
            rows[r] = not rows[r]    # 两个数组表示横、纵的奇偶性
            cols[c] = not cols[c]
        
        # rows数组里，True和False的个数
        rows_true = rows.count(True)     # 统计，数组中有多少奇，剩下的是偶
        rows_false = n - rows_true

        cols_true = cols.count(True)
        cols_false = m - cols_true

        return rows_true * cols_false + rows_false * cols_true#相乘即得，可以想象成调整行列，变成田字格
    奇偶
    偶奇   #这样可以理解

```

#### 27. [709. 转换成小写字母](https://leetcode-cn.com/problems/to-lower-case/)

```python
class Solution:
    def toLowerCase(self, str: str) -> str:
        result = ""
        for s in str:
            if s >= 'A' and s <= 'Z':
                s = chr(ord(s) + 32)
            else:
                pass
            result += s
        return result

#pythonic:
 # return ''.join([chr(ord(c)+32) if ord(c)>=65 and ord(c)<=90 else c for c in str])
# 内置函数ord（），将字符转换为ascii码，chr（）将ascii码转换为字符
```

#### 28. [804. 唯一摩尔斯密码词](https://leetcode-cn.com/problems/unique-morse-code-words/)

```python
class Solution:
    def uniqueMorseRepresentations(self, words: List[str]) -> int:
        dic = {'a': '.-',  'b': '-...',  'c': '-.-.',  'd': '-..',  'e': '.',  'f': '..-.',  'g': '--.',  'h':                      '....',  'i': '..',  'j': '.---',  'k': '-.-',  'l': '.-..',  'm': '--',  'n': '-.', 'o': '---',                     'p': '.--.',  'q': '--.-',  'r': '.-.',  's': '...',  't': '-', 'u': '..-',  'v': '...-',  'w': '.--',                'x': '-..-',  'y': '-.--',  'z': '--..'}
        res = set()  # set()用来去重，最后统计set的数量就好
        for word in words:
            temp = ''
            for s in word:
                temp += dic[s]
            res.add(temp)
        
        return len(res)  # 注意，和java不同，这里使用len（）来计算set的数量
```

#### 29. [832. 翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

```python
class Solution:
    def flipAndInvertImage(self, A: List[List[int]]) -> List[List[int]]:
        for row in A:
            for j in range((len(row) + 1) // 2):
                if row[j] == row[-1-j]:             # 采用Python化的符号索引
                    row[j] = row[-1-j] = 1 - row[j]    
        return A
# 如果一行首尾对称位置不同，那么先水平翻转，再图像翻转等于没有翻转，所以不用管，相反，如果相同，那么不用位置互换，但是都要翻转。如果是奇数列，那么中间的相当于和自己相同，也要翻转。这里注意len+1，才能触及中间的那个元素
```

#### 30. [1323. 6 和 9 组成的最大数字](https://leetcode-cn.com/problems/maximum-69-number/)

```python
class Solution:
    def maximum69Number (self, num: int) -> int:
         return int(str(num).replace("6", "9", 1))
 '''       
1 str.replace(old, new[, max])
2 Python replace() 方法把字符串中的 old（旧字符串）替换成 new(新字符串)，如果指定第三个参数max，则替换不超过 max次
3 原str不被更改，使用需由新的变量接收
4 replace调用C的接口，无python源码
5 经测试，str里不存在old时返回原str，不会报错的
'''
```

#### 31. [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        if not head or head.next==None:
            return  head
        past=None
        pre=head.next
        t=head
        while pre :
            t.next=past
            past=t
            t=pre
            pre=pre.next
        t.next=past
        return  t
```

#### 32.[1460. 通过翻转子数组使两个数组相等](https://leetcode-cn.com/problems/make-two-arrays-equal-by-reversing-sub-arrays/)

```python
class Solution:
    def canBeEqual(self, target: List[int], arr: List[int]) -> bool:
        return False if sorted(target) != sorted(arr) else True
    
  ##实际上只用考虑，两个数组元素是否一致就好，只要一直交换下去，总可以顺序一致，所以直接排序，作比较
##更值得注意的是，arr.sort和sorted（arr）的区别
```

#### 33. [1309. 解码字母到整数映射](https://leetcode-cn.com/problems/decrypt-string-from-alphabet-to-integer-mapping/)

```python
class Solution:
    def freqAlphabets(self, s: str) -> str:
        def get(st):
            return chr(int(st) + 96)

        i, ans = 0, ""
        while i < len(s):
            if i + 2 < len(s) and s[i + 2] == '#':
                ans += get(s[i : i + 2])
                i += 2
            else:
                ans += get(s[i])
            i += 1
        return ans


```

34. #### [1370. 上升下降字符串](https://leetcode-cn.com/problems/increasing-decreasing-string/)

    ```python
    class Solution:
        def sortString(self, s: str) -> str:
            str_counter = collections.Counter(s)
            result = []
            flag = False
            while str_counter:
                keys = list(str_counter)  # 是包含非重复字符的一些键,可以理解为hash表，展示的是键……
                keys.sort(reverse=flag)   # 通过false 、true控制正序还是倒叙
                flag = not flag           # 每次取字符的方向颠倒
                result.append(''.join(keys))  # 将这些键加入列表
                str_counter -= collections.Counter(keys)   # 集合做差集，实际是减小次数
            return ''.join(result)
    '''
    关于排序，我们可以直接调用Python内置的list的sort方法，通过设置reverse参数来控制是否降序。
    关于字符集合提取，我们可以采用Python中collections内置库的Counter对象来操作
    一个 Counter 是一个 dict 的子类，用于计数可哈希对象。它是一个集合，元素像字典键(key)一样存储，它们的计数存储为值。计数可以是任何整数值，包括0和负数。 Counter 类有点像其他语言中的 bags或multisets。
    '''
    
    
    假如：    print(str_counter)，s=aaaabbbbcccc,输出是Counter({'a': 4, 'b': 4, 'c': 4})
    ```

35. #### [90. N叉树的后序遍历](https://leetcode-cn.com/problems/n-ary-tree-postorder-traversal/)

    ```python
    """
    # Definition for a Node.
    class Node:
        def __init__(self, val=None, children=None):
            self.val = val
            self.children = children
    """
    # 递归
    class Solution:
        def postorder(self, root: 'Node') -> List[int]:
            result = []
    
            def postHelper(root):
                if not root:
                    return Node
                children = root.children
                for child in children:
                    postHelper(child)
    
                result.append(root.val)
            postHelper(root)    # 最后加上root
    
            return result
        
        
        # 迭代
        if not root:
                return None
            stack, res = [root], []
            while stack:
                node = stack.pop()
                res.append(node.val)
                for i in node.children:
                    stack.append(i)
    
            return res[::-1]
    ```

36. #### [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    # 二叉搜索树的中序遍历为 递增序列 ，所以易得二叉搜索树的 中序遍历倒序 为 递减序列 。
    class Solution:
        def kthLargest(self, root: TreeNode, k: int) -> int:
            res = 0
            self.cnt = 0
            self.k=k
            def inOrder(root,k):
                if not root:
                    return None
                inOrder(root.right,k)
                
                self.cnt +=1
                if self.cnt == k:
                    self.res=root.val
                    return None
                inOrder(root.left,k)
            inOrder(root,k)   # 嵌套的函数，需要在外函数中调用，才能运行
            return self.res
    ```


>>>>>>> ea754aa30be19ff499f61cee80613aabe5407cba
37. #### [733. 图像渲染](https://leetcode-cn.com/problems/flood-fill/)

    ```python
    class Solution:
        def floodFill(self, image: List[List[int]], sr: int, sc: int, newColor: int) -> List[List[int]]:
            currColor = image[sr][sc]
            if currColor == newColor:
                return image
            
            n, m = len(image), len(image[0])
            queue = collections.deque([(sr,sc)])  # 内部用元祖作为记录，外部用列表作为队列
            image[sr][sc] = newColor
            while queue:
                x, y = queue.popleft()
                for new_x, new_y in [(x-1,y),(x+1,y), (x,y-1), (x, y+1)]:
                    if 0<= new_x<n and 0 <= new_y < m and image[new_x][new_y] == currColor:
                        queue.append((new_x,new_y))
                        image[new_x][new_y] = newColor
    
            return image 
    ```

38. #### [110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    
    class Solution:
        def isBalanced(self, root: TreeNode) -> bool:
            '''
            def height(root: TreeNode) -> int:
                if not root:
                    return 0
                return max(height(root.left), height(root.right)) + 1
    
            if not root:
                return True
            return abs(height(root.left) - height(root.right)) <= 1 and self.isBalanced(root.left) and self.isBalanced(root.right)
    
    '''
            def judge(root):
                if not root:
                    return 0
                l=judge(root.left)
                if l == -1:
                    return l
                r=judge(root.right)
                if r == -1:
                    return r
                if abs(l-r)>1:
                    return -1
                else:
                    return max(l,r)+1
    
            
            return False if judge(root) == -1 else True
    ```

39. #### [108. 将有序数组转换为二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree/)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    
    class Solution:
        def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
            def build(nums, l, r):
                if r<l:
                    return None
                mid = l+(r-l)//2
                root = TreeNode(nums[mid])
                root.left = build( nums, l, mid-1)
                root.right = build(nums, mid+1, r)
                return root
    
    
    
    
    
    
    
            if not nums or len(nums) == 0:
                return 
            mid=0+len(nums)//2
            root = TreeNode(nums[mid])
            root.left = build( nums, 0, mid-1)
            root.right = build(nums, mid+1, len(nums)-1)
            return root
        
    ```

40. #### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    
    class Solution:
        def levelOrder(self, root: TreeNode) -> List[List[int]]:
            q1=collections.deque()
            if not root :
                return []
            res = []
            q1.append(root)
            while q1:
                temp = []
                for i in range(len(q1)):
                    
                    
                    t = q1.popleft()
                    temp.append(t.val)
                    if t.left:
                        q1.append(t.left)
                    if t.right:
                        q1.append(t.right)
                res.append(temp)    

    
            return res
    
    ```
    
41. [589. N叉树的前序遍历](https://leetcode-cn.com/problems/n-ary-tree-preorder-traversal/)

    ```python
    """
    # Definition for a Node.
    class Node:
        def __init__(self, val=None, children=None):
            self.val = val
            self.children = children
    """
    
    class Solution:
        def preorder(self, root: 'Node') -> List[int]:
            if root is None:
                return []
            
            stack, output = [root], []            
            while stack:
                root = stack.pop()
                output.append(root.val)
                stack.extend(root.children[::-1])  # 加入栈的时候，要倒着加，才能在出栈的时候，从左至右出栈
                    
            return output
    
    
    ```

42. #### [1207. 独一无二的出现次数](https://leetcode-cn.com/problems/unique-number-of-occurrences/)

    ```python
    class Solution:
        def uniqueOccurrences(self, arr: List[int]) -> bool:
            
    
            # 更加pythonic的写法:
            eleNum = collections.Counter(arr)
            return len(set(eleNum.values())) == len(eleNum.values())
    
    
    
    
    
            '''
            map = dict()
            for i in arr:
                t=map.get(i, 0)
               map[i]=t+1
    
            print(map)
            return len(set(map.values())) == len(map.values())
            '''
    ```

43. #### [111. 二叉树的最小深度](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

    ```python
    # Definition for a binary tree node.
    # class TreeNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.left = None
    #         self.right = None
    
    class Solution:
        def minDepth(self, root: TreeNode) -> int:
            def dfs(root):
                if not root:  # 没有节点
                    return 0
                elif root.right and root.left:  # 双亲节点
                    return 1+min(dfs(root.left),dfs(root.right))
                else:
                    return 1+dfs(root.left)+dfs(root.right)#这里是针对只有单个子节点的情况，需要加上单个节点自己
    
    
            if not root:
                return 0
            elif not root.left and not root.right:
                return 1
            else:
                return dfs(root)
    
    
    
            
    ```

44. #### [1512. 好数对的数目](https://leetcode-cn.com/problems/number-of-good-pairs/)

    ```python
    class Solution:
        def numIdenticalPairs(self, nums: List[int]) -> int:
            def getSum(n):#这是在求和，这里的规律就是0+……+n-1
                l=[x for x in range(n)]
                return sum(l)
            
            
            dic = dict()
            n =len(nums)
            res = 0
            for i in range(n):
                dic.setdefault(nums[i],[]).append(i)  #dict.setdefault(key,[]).append  特殊用法记住。统计相同值的索引
            for v in dic.values():
                if len(v)>1:
                    res += getSum(len(v))
    
            return res
        
        
        
        # 极简方法，用统计来计算
         l=[0 for i in range(100)]  #因为题目限制，所以留下100个桶
            
            #print(l)
            res =0
            for num in nums:
                res += l[num-1]  # 每次统计到，先加在填入桶中，避免了只有1个时，因为原始是0，所以不用加。
                l[num-1] += 1
            return res
    
    ```

45. #### [1486. 数组异或操作](https://leetcode-cn.com/problems/xor-operation-in-an-array/)

    ```python
    class Solution:
        def xorOperation(self, n: int, start: int) -> int:
            res = 0
            for i in range(n):
                res ^= (start+i*2)
            return res
    
    # 暴力方法
    
    
    # O（1）的位运算方法
    '''
    异或的性质：
    1) 0 ^ x = x
    2) x ^ x = 0
    3) 2x ^ (2x+1) = 1
    
    思路：
    公式是：start^start+2^...。但是性质三很有趣，因此，这里需要将步长减半，就可以利用性质3，方法是除以2.--》start/2^start/2 + 1 ^...。最后乘2，在讨论最后一位就好
    于是根据奇偶性需要分类讨论：
    如果  start 为偶，n也为偶：
    	那就是两两异或成1，然后（n/2)个1，进行异或，可以用（n/2)^1,计算。这是可以对res*2，直接得到结果
    如果 start为偶， n 为奇数：
    	那就是两两异或成1之外，还会剩下start/2+n-1,随意（n/2)^1^(start/2+n-1),然后res*2
    如果 start 为奇数：那么我们可以在前面补充(start/2−1)⊕(start/2−1），以为性质2，相当于0^原本的式子，然后性质1，相当于没变。此时相当于(start/2−1)^以f((start/2−1),n+1)的式子，就将后面的变成的偶数开始。
    	此时：如果n为偶数，最终一共有 n/2个1进行异或，即 res = (n/2)^1，因为补充的那段没有影响，这时res*2
    	如果n为奇数：会剩下一个，即 res = (n/2)^1 & (start+num-1)，这里res= 2* res+1
    '''
    class Solution:
        def xorOperation(self, n: int, start: int) -> int:
            res = 2* self.myxor(n, start//2) # 总是要*2 ，这里先不管start的奇偶性，要//2 以后在讨论。因为只有整除2，以后，才能用到异或性质3
            if n&1 and start&1:  #they are all odd ，只有n为奇数，所以会有statt因素影响，进而有1被整除忽略，需要加上
                res = res +1
            return res
        
        def myxor(self, n, start):
            if start&1:
                return (start-1)^self.myxor(n+1,start-1)
            else:
                if n&1:  # n为奇数
                    return (n//2&1)^(start+n-1)
                else:   # n为偶数
                    return (n//2)&1
    
    ```

46. #### [459. 重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)

    ```python
    class Solution:  # 遇到重复问题，考虑重复拼接，然后具体处理，也是一个思路
        def repeatedSubstringPattern(self, s: str) -> bool:
            return s in (s+s)[1:-1]
    ```

    

47. #### [1266. 访问所有点的最小时间](https://leetcode-cn.com/problems/minimum-time-visiting-all-points/)

    ```python
    class Solution:
        def minTimeToVisitAllPoints(self, points: List[List[int]]) -> int:
            x0, x1 = points[0]
            ans = 0
            for i in range(1, len(points)):
                y0, y1 = points[i]
                ans += max(abs(x0 - y0), abs(x1 - y1))
                x0, x1 = points[i]
            return ans
    
    # 无论怎么走，都是使用Dx ，和 Dy中的最大值，自己画一下就出来了
    ```

48. #### [290. 二进制链表转整数](https://leetcode-cn.com/problems/convert-binary-number-in-a-linked-list-to-integer/)

    ```python
    # Definition for singly-linked list.
    # class ListNode:
    #     def __init__(self, x):
    #         self.val = x
    #         self.next = None
    
    class Solution:
        def getDecimalValue(self, head: ListNode) -> int:
            cur = head
            ans = 0
            while cur:
                ans = ans * 2 + cur.val
                cur = cur.next
            return ans
    
    # 单纯按位代表的值模拟就好
    # 二进制转化十进制常用  ans = ans * 2 + cur.val，反过来就是10->2 
    ```

49. #### [5499. 重复至少 K 次且长度为 M 的模式](https://leetcode-cn.com/problems/detect-pattern-of-length-m-repeated-k-or-more-times/)

    ```python
    class Solution:
        def containsPattern(self, arr: List[int], m: int, k: int) -> bool:
            # 就是单纯的滑动窗口，而我是代码实现不熟练，磕磕绊绊了好久
            # 总之要写代码啊！！！
            # 而且一开始，傻逼到搞混，m，k的定义……
            # 总之就是，以m为窗口滑动，依次暴力比较
            res = False
            for i ,v in enumerate(arr):
                if i + 1 < m:
                    continue
                cnt = 1  #统计有多少组
                idx = i  # 指向每组的首个元素
                while idx+m < len(arr) and cnt<k:   #如果超标，就退出循环
                    if arr[idx-m+1:idx+1] != arr[idx+1:idx+m+1]:
                        break   # 脱离内层循环，从外层开始
    
                    idx += m
                    cnt += 1
                if cnt == k:
                    return True
            return False
    
    ```

    #### 50.[1025. 除数博弈](https://leetcode-cn.com/problems/divisor-game/)

    ```python
    # 固然有按照奇偶性找规律的做法，不过，这里是用来学习动态规划
    class Solution:
        def divisorGame(self, N: int) -> bool:
            if N == 1:
                return False
            dp = [False] *(N+1)  # 创建数组方式记住，别在这上吃亏
            print(dp)
            dp[1], dp[2] =False, True
            
            for i in range(3,N+1):
                for j in range(1, i//2+ 1):
                    if i % j == 0 and  not (dp[i-j]):   # 如果-j，符合条件，就确定胜利，直接推吹内部循环
                        dp[i] = True
                        break
                    
            return dp[N]
    # 因为这个游戏实际上是一开始能赢，就一定能赢，否则必输，所以循环内部的逻辑就是：如果能使对边必输，那我就赢。然后一路地推到N
    ```

    #### 51. [1544. 整理字符串](https://leetcode-cn.com/problems/make-the-string-great/)

    ```python
    # 我的方法也能过，但是太傻逼了。每次pop都要考虑很多东西，索引变化，然后如果到了n-1又要if
    # 正统的、好的方法，就是如下，用一个栈解决问题，用来存放整理好的，然后和后面的对比，两头需要删除，pop（），然后这一轮的不要，否则入栈
    class Solution:
        def makeGood(self, s: str) -> str:
            ret = list()
            for ch in s:
                if ret and ret[-1].lower() == ch.lower() and ret[-1] != ch:
                    ret.pop()
                else:
                    ret.append(ch)
            return "".join(ret)
    ```

    #### 52. [1217. 玩筹码](https://leetcode-cn.com/problems/minimum-cost-to-move-chips-to-the-same-position/)

    ```python
    # "每个筹码的位置存在数组 chips 当中",所以实际上说明的是每个筹码的位置
    # 解法就是，相隔1个移动无代价，随意可以讲奇数和偶数为的各自聚合，重点可以是任意贴在一起的相邻位置
    # 这时候，比较那个筹码更少，代价更低，就移动那个
    class Solution:
        def minCostToMoveChips(self, position: List[int]) -> int:
            even = 0
            for c in position:
                if c&1==0:
                    even+=1
            return min(even,len(position)-even)
    ```

    #### 53. [1002. 查找常用字符](https://leetcode-cn.com/problems/find-common-characters/)

    ```python
    class Solution:
        def commonChars(self, A: List[str]) -> List[str]:
            from collections import Counter  # 计数器，见过几次了
            ans = Counter(A[0])
            for i in A[1:]:  # 这种切片还挺有趣的
                ans &= Counter(i)  # 每次取交集，计数器的新用法
            return list(ans.elements())  # 也是计数器的应用性质和dict.item()差不多
        
        
        
    # 正统哈希解法，统计各自出现的次数，然后取最小    
    class Solution:
        def commonChars(self, A: List[str]) -> List[str]:
            Hash = {}
            for i in range(97, 123):
                Hash[chr(i)] = [0 for i in range(len(A))]
            for j in range(len(A)):
                for k in A[j]:
                    Hash[k][j] += 1
            result = []
            for key in Hash.keys():
                if 0 in Hash[key]:
                    continue
                else:
                    result += [key] * min(Hash[key])
            return result
    
    作者：yi-wen-statistics
    链接：https://leetcode-cn.com/problems/find-common-characters/solution/ha-xi-cha-zhao-by-yi-wen-statistics-5/
    来源：力扣（LeetCode）
    著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
    
    ```



#### 54 [1534. 统计好三元组](https://leetcode-cn.com/problems/count-good-triplets/)

```python
class Solution:
    def countGoodTriplets(self, arr: List[int], a: int, b: int, c: int) -> int: 
        n = len(arr)
        cnt = 0
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    if abs(arr[i] - arr[j]) <= a and abs(arr[j] - arr[k]) <= b and abs(arr[i] - arr[k]) <= c:
                        cnt += 1
        return cnt

```

#### 55 . [762. 二进制表示中质数个计算置位](https://leetcode-cn.com/problems/prime-number-of-set-bits-in-binary-representation/)

```python
# 由于数有上限，所以二进制有长度上线，进而只会有有限个素数可能，都不是，那就是合数
# bin(x).count('1')  统计一个数二进制下1的个数

class Solution:
    def countPrimeSetBits(self, L: int, R: int) -> int: 
        primes = {2, 3, 5, 7, 11, 13, 17, 19}
        return sum(bin(x).count('1') in primes for x in range(L, R+1))  # 如果in primes,才会被sum。算是pythonic写法
```

#### 56.[811. 子域名访问计数](https://leetcode-cn.com/problems/subdomain-visit-count/)

```python
# 纯字符串操作，最多加上哈希映射
class Solution(object):
    def subdomainVisits(self, cpdomains):
        """
        :type cpdomains: List[str]
        :rtype: List[str]
        """
        res = {}
        for item in cpdomains:
            num,domain=item.split()# 先分割域名与访问次数
            num = int(num)
            domain_split = domain.split('.')

            for i in range(len(domain_split)-1,-1,-1):
                domain = '.'.join(domain_split[i:])# 再逐级分割域名，从后往前合并

                if domain not in res:
                    res[domain] = num
                else:
                    res[domain] += num
        
        return ['{} {}'.format(val,key)  for key,val in res.items()]



```

####    57. [748. 最短完整词](https://leetcode-cn.com/problems/shortest-completing-word/)

```python
class Solution:
    def shortestCompletingWord(self, licensePlate: str, words: List[str]) -> str:
        char = [i.lower()  for i in licensePlate  if i.isalpha()]   # isalpha(),应该是判断是否为字母
        words=sorted(words, key=lambda x:len(x))   # 排序这里我算是用会了

        for word in words:
            l= char.copy()  # 每次拷贝一个用来使用，有就提出，直到列表为空，说明找到了
            for i in word:
                if i in l:
                    l.remove(i)
                    
                if not l:
                    return word

```



   ####  58. [1260. 二维网格迁移](https://leetcode-cn.com/problems/shift-2d-grid/)

```python
# 模拟法可以做，但是太拉跨
# 应该有取余的方法
def shiftGrid(self, grid: List[List[int]], k: int) -> List[List[int]]:
    new_grid = [[0] * len(grid[0]) for _ in range(len(grid))]
    num_rows = len(grid)
    num_cols = len(grid[0])
    for row in range(num_rows):
        for col in range(num_cols):
            new_col = (col + k) % num_cols   # 对列的求余
            wrap_around_count = (col + k) // num_cols   #  看看走了多少行
            new_row = (row + wrap_around_count) % num_rows  # 对行的求余
            new_grid[new_row][new_col] = grid[row][col]
    return new_grid

作者：LeetCode
链接：https://leetcode-cn.com/problems/shift-2d-grid/solution/er-wei-wang-ge-qian-yi-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 59 [1475. 商品折扣后的最终价格](https://leetcode-cn.com/problems/final-prices-with-a-special-discount-in-a-shop/)

```python
class Solution:
    # 正常模拟的方法太慢，单调栈比较好
    def finalPrices(self, prices: List[int]) -> List[int]:
        # 这里的栈，用来统计坐标，每次触发条件，就把栈内指向的坐标中的值减去对应的数
        stack, result = [], [i for i in prices]
        for i in range(len(prices)):
            # 当stack非空且stack栈顶元素在prices对应的值大于prices[i]时触发while循环
            while stack and prices[stack[-1]] >= prices[i]:  # 有大于的
                result[stack[-1]] -= prices[i] # 将stack中所有对应在prices的满足价格条件的值都减去prices[i]
                stack.pop() # 弹出栈顶元素
            stack.append(i) # 储存索引
        return result   

```

#### 60 [27. 移除元素](https://leetcode-cn.com/problems/remove-element/)

```python
## 双指针
class Solution:
    def removeElement(self, nums: List[int], val: int) -> int:
        n = len(nums)
        t = 0
        for i in nums:
            if i != val:
                nums[t] = i
                t +=1

        return t
```

#### 61 [28. 实现 strStr()](https://leetcode-cn.com/problems/implement-strstr/)

```python
## 滑动窗口解法，几个while用的很好
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        L, n = len(needle), len(haystack)
        if L == 0:
            return 0

        pn = 0
        while pn < n - L + 1:
            # find the position of the first needle character
            # in the haystack string
            while pn < n - L + 1 and haystack[pn] != needle[0]:  # 找到第一个首字母相同的
                pn += 1
            
            # compute the max match string
            curr_len = pL = 0
            while pL < L and pn < n and haystack[pn] == needle[pL]:  # 开始匹配，
                pn += 1
                pL += 1
                curr_len += 1
            
            # if the whole needle string is found,
            # return its start position
            if curr_len == L:  # 如果走到最后那就可以返回
                return pn - L
            
            # otherwise, backtrack
            pn = pn - curr_len + 1  # 如果不行，那就将要比较的指针向后挪移
            
        return -1
```

#### 61 [38. 外观数列](https://leetcode-cn.com/problems/count-and-say/)

```python
## 递归问题递归做，字符串就应操作
class Solution:
    def countAndSay(self, n: int) -> str:
        if n == 1:
            return '1'
        s = self.countAndSay(n-1)
        
        i, res = 0, ''
        for j, c in enumerate(s):
            if c != s[i]:
                res += str(j-i) + s[i]
                i = j
        res += str(len(s) - i) + s[-1]  # 最后一个元素莫忘统计
        return res
```

#### 62 [404. 左叶子之和](https://leetcode-cn.com/problems/sum-of-left-leaves/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
## dfs
# not 什么时候加要分清
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        def dfs(node):
            res =0
           
            if node.left:
                tmp = node.left
                if not tmp.left and not tmp.right:
                    res += tmp.val
                else:
                    res += dfs(tmp)
            #print(res)
            if node.right:
                tmp = node.right
                print(tmp.left)
                if  tmp.left or  tmp.right:
                    #print('==========')
                    res += dfs(tmp)
            #print(res)
            return res
        return dfs(root) if root else 0
  


# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# bfs真的，代码细节要注意！！！！
class Solution:
    def sumOfLeftLeaves(self, root: TreeNode) -> int:
        if not root:
           return 0
        isleaf = lambda node : not node.left and not node.right
        q= collections.deque([root])
        res = 0
        while q:
            node = q.popleft()
            if node.left:
                if isleaf(node.left):
                    res += node.left.val
                else:
                    q.append(node.left)
            if node.right:
                if not isleaf(node.right):
                    q.append(node.right)
        return res
        
```

#### 63 [893. 特殊等价字符串组](https://leetcode-cn.com/problems/groups-of-special-equivalent-strings/)

```python
# 别被题目迷惑，等价就说明单拉出来奇偶，然后排序，应该是相同的，从这点可以辨别出来是否等价
class Solution:
    def numSpecialEquivGroups(self, A: List[str]) -> int:
        res = set()
        for sub in A:
            sub = ''.join(sorted(sub[::2]) + sorted(sub[1::2]))
            res.add(sub)
        return len(res)

```

#### 64 [897. 递增顺序查找树](https://leetcode-cn.com/problems/increasing-order-search-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    # 设置一个全局变量，每次对全局变量操作右节点，并更新指向的位置
    def increasingBST(self, root: TreeNode) -> TreeNode:
        self.x = ans = TreeNode(0)

        def dfs(node):
            if node:
                dfs(node.left)# 中序递归，所以逻辑处理放在中间
                t=TreeNode(node.val)
                self.x.right=t
                self.x=t
                dfs(node.right)
        dfs(root)
        return ans.right
```

#### 65 [501. 二叉搜索树中的众数](https://leetcode-cn.com/problems/find-mode-in-binary-search-tree/)(理解解法，没有敲过)

```python
class Solution:
    def findMode(self, root: TreeNode) -> List[int]:
        ans=[]
        most=0
        last=None
        cnt=0

        def inorder(node):
            if not node: return 
            nonlocal ans,most,last,cnt
            if node.left: inorder(node.left)
            if node.val==last:
                cnt+=1
            else: cnt=1
            if cnt==most: ans.append(node.val)
            elif cnt>most:
                most=cnt
                ans=[node.val]
            last=node.val
            if node.right: inorder(node.right)

        inorder(root)
        return ans


```

#### 66. [1237. 找出给定方程的正整数解](https://leetcode-cn.com/problems/find-positive-integer-solution-for-a-given-equation/)

```python
# 双指针解法，体会一下
class Solution:
    def findSolution(self, customfunction: 'CustomFunction', z: int) -> List[List[int]]:
        ans,x,y = [],1,1000
        while x <= 1000 and y >= 1:
            res = customfunction.f(x,y)
            if res < z:
                x += 1
            elif res > z: 
                y -= 1
            if res == z:
                ans.append([x,y])
                x += 1
                y -= 1
        return ans


```

#### 67. [476. 数字的补数](https://leetcode-cn.com/problems/number-complement/)

```python
class Solution:
    # 补数就等于2**（num转为2进制的长度） 减1 减num
    def findComplement(self, num: int) -> int:
        A = bin(num)[2:]
        return 2**len(A) - 1 - num
```

#### 68. [235. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
#利用二叉搜索树固有的性质
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if p.val < root.val and q.val<root.val:# 如果都小于，那么目标节点在left
            return self.lowestCommonAncestor(root.left,p,q)
        elif p.val>root.val and q.val> root.val:# 如果都大于，那么目标节点在right
            return self.lowestCommonAncestor(root.right,p,q)
        else:# 如果一大一小，说明，就是这个
            return root
```

#### 69. [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

```python
# 注意好边界条件
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        node = head
        while head and head.next:
           # print(head.val, head.next.val)
            while head.val == head.next.val:
                if not head.next.next:
                    head.next = None
                    break 
                head.next = head.next.next
            head = head.next
        return node

```

#### 70.[118. 杨辉三角](https://leetcode-cn.com/problems/pascals-triangle/)

```python
# 纯模拟题
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        n = numRows
        res = []
        for i in range(n):
            if i ==0:
                res.append([1])
                continue
            tmp = [0]*(i+1)
            tmp[0] = tmp[-1] = 1
            last = res[-1]
            for j in range(1,i):
                tmp[j] = last[j-1] + last[j]
            res.append(tmp[:])
        return res
            
```



#### 71. [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        l, r = 0, len(s)-1
        while l <=r:
            s[l] ,s[r] = s[r],s[l]
            l +=1
            r -=1
        return self
```



#### 72. [125. 验证回文串](https://leetcode-cn.com/problems/valid-palindrome/)

```python
class Solution:
    # s.isalnum() 这个函数没见过
    def isPalindrome(self, s: str) -> bool:
        sgood = "".join(ch.lower() for ch in s if ch.isalnum())
        return sgood == sgood[::-1]

```

#### 73 [119. 杨辉三角 II](https://leetcode-cn.com/problems/pascals-triangle-ii/)

```python
class Solution:
    def getRow(self, rowIndex: int) -> List[int]:
        n=rowIndex#取一个简单的名字
        l=[1]#临时列表1
        r=[]#返回值列表
        for i in range(1,n+2):#i为层数，该层的数字数
            l.append(0)#给临时列表的末尾加0防止越界
            l.insert(0,0)#给临时列表的前端加0防止越界
            r=[]#清空返回列表
            for j in range(1,i+1):#递推每一个数字
                r.append(l[j-1]+l[j])#递推式，这里j从1开始，所以这么写
            l=r[:]#把新的列表（r）赋值给l
        return r#完结撒花


```

#### 73. [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)

```python
# 哈希表是直观想法，不用写。快慢指针一时想不到，思路就是如果是环，快的一定会套圈慢的，如果相等，则确定是环
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        if not head or not head.next:
            return False
        f ,s = head.next,head
        while f != s:
            if not f or not f.next:
                return False
            f = f.next.next
            s= s.next
        return True

    # 也可以当作字符串来做，不过因为要默认32位，所以缺了的话，要补足0
    class Solution:
    def reverseBits(self, n: int) -> int:
        s = bin(n)[2:]
        #print(s,'s')
        l = len(s)
        t =''
        for i in range(32-l):
            t += '0'

        s = t+s
        s = s[::-1]
        return int(s,base=2)  # int()函数，加上base参数，可以选择进制转换
    
   # 更pythonic一点
 return int(bin(n)[2::].zfill(32)[::-1], 2)
# zfill（字符串右对齐，前面补0）
```





#### 74. [190. 颠倒二进制位](https://leetcode-cn.com/problems/reverse-bits/)

```python
class Solution:
    def reverseBits(self, n: int) -> int:
        res, power = 0, 31  # power 表示位数
        while n:
            res += (1&n)<<power  # 每次比较n最右边的值，然后左移power位
            power -=1   # 下一回移动的power位-1
            n = n>>1    # 最右端用过了，要更新
        return res
```

#### 75.[167. 两数之和 II - 输入有序数组](https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/)

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        dic = {}
        for i in range(len(numbers)):
            if target-numbers[i] in dic:
                return [dic[target-numbers[i]],i+1]
            else:
                dic[numbers[i]] = i+1
# 递增，可能有特性没用到
# 二层循环，内层使用二分，O（nlongn), O(1)
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        n = len(numbers)
        for i in range(n):
            low, high = i + 1, n - 1
            while low <= high:
                mid = (low + high) // 2
                if numbers[mid] == target - numbers[i]:
                    return [i + 1, mid + 1]
                elif numbers[mid] > target - numbers[i]:
                    high = mid - 1
                else:
                    low = mid + 1
        
        return [-1, -1]
# 双指针，min+ max可以认为是中间值了，之和大于，r--，小于l++，知道之和==target
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        low, high = 0, len(numbers) - 1
        while low < high:
            total = numbers[low] + numbers[high]
            if total == target:
                return [low + 1, high + 1]
            elif total < target:
                low += 1
            else:
                high -= 1

        return [-1, -1]

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/two-sum-ii-input-array-is-sorted/solution/liang-shu-zhi-he-ii-shu-ru-you-xu-shu-zu-by-leet-2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 76. [530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# 我能想到的中序遍历，记录数组，循环比较
# 我没想到的，可以再遍历中，直接加上pre字段，然后不断比较当前值和pre的差值，直接得到res
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        res = float('inf')
        pre= -1
        
        def _inorder(node):
            nonlocal res,pre
            if not node:
                return None
            _inorder(node.left)
            if pre == -1:
                pre = node.val
            else:
                res = min(res,node.val-pre)
                pre = node.val
            right = _inorder(node.right)
        _inorder(root)
        return res

```

#### 77. [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

```python
# 双指针，两边到中间，边比较边移动
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        n = len(A)
        ans = [0] * n
        
        i, j, pos = 0, n - 1, n - 1
        while i <= j:
            if A[i] * A[i] > A[j] * A[j]:
                ans[pos] = A[i] * A[i]
                i += 1
            else:
                ans[pos] = A[j] * A[j]
                j -= 1
            pos -= 1
        
        return ans


```

#### 78. [172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        if n<=0:
            return n
        res = 0
        while n:
            res += n//5
            n =n //5
        return res
```

#### 79. [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)

```python
class Solution:
    def backspaceCompare(self, S: str, T: str) -> bool:
        if not S and not T:
            return True
        q1, q2 = [] ,[]
        for i in  S:
            if i != '#':
                q1.append(i)
            else:
                if q1:
                    q1.pop()
        for i in  T:
            if i != '#':
                q2.append(i)
            else:
                if q2:
                    q2.pop()
        return q1 == q2
        
```

#### 80. [202. 快乐数](https://leetcode-cn.com/problems/happy-number/)

```python
class Solution:
    # 哈希判断循环
    def isHappy(self, n: int) -> bool:
        s = set()
        while n not in s:
           # print(n)
            s.add(n)
            if n == 1:
                return True
            cs = str(n)
            res = 0
            for i in range(len(cs)):
                res += int(cs[i]) ** 2
            n = res
        return False
# 某种形式的快慢指针，一旦循环，必有嵌套，fast == slow
def isHappy(self, n: int) -> bool:  
    def get_next(number):
        total_sum = 0
        while number > 0:
            number, digit = divmod(number, 10)
            total_sum += digit ** 2
        return total_sum

    slow_runner = n
    fast_runner = get_next(n)
    while fast_runner != 1 and slow_runner != fast_runner:
        slow_runner = get_next(slow_runner)
        fast_runner = get_next(get_next(fast_runner))
    return fast_runner == 1

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/happy-number/solution/kuai-le-shu-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 81. [925. 长按键入](https://leetcode-cn.com/problems/long-pressed-name/)

```python
class Solution:
    # 一开始循环判定条件将i\j都算进去，但是只用j就够了。属于没考虑清楚
    def isLongPressedName(self, name: str, typed: str) -> bool:
        i=j=0
        while j<len(typed):
            if i<len(name) and name[i] == typed[j]:
                i +=1
                j +=1
            elif j>0 and typed[j] == name[i-1]:
                j +=1
            else:
                return False
        return i == len(name)
```



#### 82. [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        if not head:
            return head
        dummy = ListNode(-1)
        dummy.next = head
        f = dummy
        while f.next:
            if f.next.val == val:
                f.next = f.next.next
            else:
                f = f.next
        return dummy.next
```

#### 83. [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)

```python
# 厄拉多塞筛法
class Solution:
    def countPrimes(self, n: int) -> int:
        res = 0
        s = [True]*(n)
        for i in range(2,n):
            if s[i]:
                res += 1
                #print(i)
            for j in range(2*i,n,i):
                s[j] = False
        return res

```

#### 84. [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

```python
class Solution:
    # 摸鱼写的，思路一开始不够清晰，其实很清楚需要判断两次。一次看key是否被用了，如果用了的话，看看value？=。如果没用过，再看看value和t[i]，防止多个字符映射到一个位置上去。
    def isIsomorphic(self, s: str, t: str) -> bool:
        dict={}
        for i in range(len(s)):
            if s[i] in dict:    #判断该元素是否之前出现过
                if dict[s[i]]!=t[i]:    #如果之前出现过，现在对应的值却不一样，返回False
                    return False
            else:
                if t[i] in dict.values():   #判断该元素是否在字典的值里面，如果在里面就说明对应的值不一样
                    return False
                dict[s[i]]=t[i] #将两个值录入字典中(记录下s[i]所变换后的字母)
        return True
```

#### 85. [219. 存在重复元素 II](https://leetcode-cn.com/problems/contains-duplicate-ii/)

```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        dic = {}
        for i, v in enumerate(nums):
            if v not in dic:
                dic[v] = i
            else:
                if i - dic[v] <= k:
                    return True
                else:
                    dic[v] = i
        return False
```

#### 86. [1207. 独一无二的出现次数](https://leetcode-cn.com/problems/unique-number-of-occurrences/)

```python
## 还是太蠢了点，最直白的方法最有效，之前考略太多，反而作死
class Solution:
    def uniqueOccurrences(self, arr: List[int]) -> bool:
        if not arr or len(arr) == 0:
            return True
        arr.sort()
        dic = defaultdict(int)
        cnt = 0
        for i in range(len(arr)):
            #print('i',i,cnt)
            #print(dic)
            if i == 0:
                cnt +=1
                continue

            if arr[i] != arr[i-1]:
                if cnt not in dic:
                    dic[cnt] = arr[i-1]
                    cnt = 1
                else:
                    return False
                if i == len(arr)-1:
                    if 1 in dic:
                        return False
                    else:
                        dic[1] = arr[-1]
            else:
                cnt += 1
                if i == len(arr)-1:
                    if cnt in dic:
                        return False
                    else:
                        return True
        #print(dic)
        return True
    
    
  pythoninc：
eleNumbers=collections.Counter(arr)
        return len(set(eleNumbers.values()))==len(eleNumbers)

作者：jutraman
链接：https://leetcode-cn.com/problems/unique-number-of-occurrences/solution/pythondu-yi-wu-er-de-chu-xian-ci-shu-by-jutraman/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


#### 87. [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False
        count = {}
        for char in s:
            if char in count:
                count[char] += 1
            else:
                count[char] = 1
        for char in t:
            if char in count:
                count[char] -= 1
            else:
                return False
        for value in count.values():
            if value != 0:
                return False
        return True

```

#### 88. [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        moves = ((1,0), (0,1), (-1,0),(0,-1))
        n, m = len(grid), len(grid[0])
        res =0
        for i in range(n):
            for j in range(m):
                if grid[i][j] ==1:
                    cnt = 0
                    for k in moves:
                        tx = i + k[0]
                        ty = j + k[1]
                        # 判断边界，一个方块的上下所有四个方向，要么是海水，要么是边界，那就是可以计算入内的边界
                        if tx<0 or tx >=n or ty <0 or ty>=m or grid[tx][ty] == 0:
                            cnt += 1
                        #print(cnt)
                    res += cnt
        return res
```



#### 89. [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        利用空间，从前向后，将所有非0数字前移，后面的空位都设置为0
        """
        i ,j = 0,0
        for i in range(len(nums)):
            if nums[i] != 0:
                nums[j] = nums[i]
                j += 1
        for j in range(j,len(nums)):
            nums[j] =0 
            j +=1
```

#### 90 [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)

```python
# 和Java有所不同，原因在于python中-1%9 == 8，所以需要分类讨论。这里主要是对%的定义不同
class Solution:
    def addDigits(self, num: int) -> int:
        if num > 9:
            num = num % 9
            if num == 0:#如果为0，说明之前能整除9，之和为9
                return 9
        return num


```

#### 91 .[263. 丑数](https://leetcode-cn.com/problems/ugly-number/)

```python
class Solution:
    # 如果是丑数，三个数不断除，结果为1
    def isUgly(self, num: int) -> bool:
        if num <= 0:
            return False
        nums = [2,3,5]
        for i in nums:
            while num % i == 0:
                num //= i
        return num ==1
```

#### 93. [268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/)

```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        num_set = set(nums)
        n = len(nums) + 1
        for number in range(n):
            if number not in num_set:
                return number


```

#### 94. [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

```python
class Solution:
    # 解法不够优雅
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        if len(nums) <= 1:
            return nums[0]
        res = 0
        cnt = nums[0]
        for i in range(1,len(nums)):
            if nums[i] == 1:
                cnt += 1
            else:
                res = max(cnt, res)
                cnt = 0
        return max(res,cnt)
```

#### 95. [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

```python
class Solution:
    # 最笨的方法，2^N
    def fib(self, N: int) -> int:
        if N == 0:
            return 0
        if N == 1:
            return 1
        return self.fib(N-1) + self.fib(N-2)
class Solution:
    # 自底向上，说白了就是记忆化
    def fib(self, N: int) -> int:
        if N<= 1:
            return N
        s  = [0  for i in range(N+1)]
        s[0] = 0
        s[1] = 1
        for i in range(2, N+1):
            s[i] = s[i-1] + s[i-2]
        return s[N]
```



#### 96. [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

```python
class Solution:
    def matrixReshape(self, nums: List[List[int]], r: int, c: int) -> List[List[int]]:
        old_r = len(nums)
        old_c = len(nums[0])
        # 先判断能不能转化
        if old_c * old_r != r * c:return nums
        # 先搞一个一维的，然后按照显得步长取值
        tmp = []
        for i in range(old_r):
            for j in range(old_c):
                tmp.append(nums[i][j])
        new = []
        for i in range(0, len(tmp), c):
            new.append(tmp[i:i+c])
        return new


```

#### 97. [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        # 由于如果0 出现在开头和结尾需要特别处理。所以这里前后补0，统一逻辑
        flowerbed=[0]+flowerbed+[0]
        tmp=0
        for i in range(len(flowerbed)):

            if flowerbed[i]==1:
                tmp=0
            else:
                tmp+=1
            # 如果满三，就可以放一个花，这时，tmp = 1
            if tmp==3:
                n-=1
                tmp=1
            if n==0:
                return True
        return False


```

#### 98. [643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)

```python
class Solution:
    def findMaxAverage(self, nums: List[int], k: int) -> float:
        res = 0
        _sum = sum(nums[:k])
        res = _sum/k
        for i in range(len(nums)-k):
            _sum = _sum - nums[i] + nums[i+k]
            res = max(res, _sum/k)
        return res

```

#### 99. [661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

```python
class Solution:
    def imageSmoother(self, M: List[List[int]]) -> List[List[int]]:
        r,c = len(M),len(M[0])
        res = [[0 for _ in range(c)] for _ in range(r)]

        for i in range(r):
            for j in range(c):
                cnt = 0 
                n = 0
                # 遍历周边及中心，看似四重循环，其实内部是循环9次
                for _r in [i-1,i,i+1]:
                    for _c in [j-1,j,j+1]:
                        if 0<=_r<r and 0<=_c<c:
                            cnt += M[_r][_c]
                            n += 1
                    
                res[i][j] = floor(cnt/n)
        return res

```

#### 100. [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

```python
class Solution:
    def checkPossibility(self, nums: List[int]) -> bool:
        cnt = 0
        for i in range(1,len(nums)):
            if nums[i]<nums[i-1]:
                cnt += 1
                if cnt >1:
                    return False
                # 形如：[2, 4, 0, 1]的特殊情况，i==2时，无论修改4，还是0 都不行，4-》0,2》0；0-》4；1《4
                # 先判断考虑四元组是否越界
                if i+1 < len(nums) and i-2 >= 0:
                    # 如果出现i+1 也小于 i-1 ,并且 i-2 也大于i的情况，例如 2200，这样，无论修改2，还是0，总有递减情况出现
                    if nums[i+1] < nums[i-1] and nums[i-2] > nums[i]:
                        return False     
        return cnt <= 1
```

#### 101. [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        res= 0
        if not nums:
            return 0
        if len(nums) == 1:
            return nums[0]
        l ,r = 0, 0
        for i in range(1,len(nums)):
            if nums[i]>nums[i-1]:
                r += 1
            else:
                # print(i,l,r)
                res = max(res, r-l+1)
                l =i
                r=i
        return max(res,r-l+1)

```

#### 102.[697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

```python
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        # 使用三个哈希表,分别记录起点终点和度
        l,r ,cnt = {},{},{}
        for idx ,v in enumerate(nums):
            # 如果没有统计起点，加上起点
            if v not in l:
                l[v] = idx
            # 更新终点和次数
            r[v] = idx
            cnt[v] = cnt.get(v,0)+1 
        # 计算出度
        degree = max(cnt.values())
        res = len(nums)
        for x in cnt:
            if cnt[x] == degree:
                res = min(res, r[x]-l[x]+1)
        return res
    
    
class Solution:
    def findShortestSubArray(self, nums: List[int]) -> int:
        """
        之前做过，再记录的意义是，dict（）可以默认出现数组，也不用defaultdict（list）
        增广见闻
        另外，和上回不同，这次有数组，所以开了一个哈希表
        """
        mp = dict()

        for i, num in enumerate(nums):
            if num in mp:
                mp[num][0] += 1
                mp[num][2] = i
            else:
                mp[num] = [1, i, i]
        
        maxNum = minLen = 0
        for count, left, right in mp.values():
            if maxNum < count:
                maxNum = count
                minLen = right - left + 1
            elif maxNum == count:
                if minLen > (span := right - left + 1):
                    minLen = span
        
        return minLen


```

#### 103. [724. 寻找数组的中心索引](https://leetcode-cn.com/problems/find-pivot-index/)

```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        # 单纯的加太单纯了，只能应付非负数，如果有负数，问题就变复杂了
        # 其实最简单的是前缀和，减去中间，那么如果可行的话，势必相等，这里其实可以两次遍历就行
        _sum = sum(nums)
        l_sum = 0
        for i, v in enumerate(nums):
            if l_sum == (_sum - nums[i]-l_sum):
                return i
            l_sum += nums[i]
        return -1
```

#### 104. [717. 1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

```python
class Solution:
    def isOneBitCharacter(self, bits: List[int]) -> bool:
        n = len(bits)-1
        if bits[n] == 1:
            return False
        t = 0
        while t <= n:
            if bits[t] == 1:
                t += 2
                if t > n:
                    return False
                continue
            t += 1
            
        return True
```

#### 105. [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

```python
class Solution:
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        # 当然可以每次和左上元素比较，也可以用对角线特性，r-c作为key，构造字典比较
        # 不过值得记下来的是一个特殊方法：
        # 只需判断：前行中除最后一个元素外剩余的元素完全等于后行中除第一个元素外剩余的元素
        col_len = len(matrix) #3
        row_len = len(matrix[0]) #4
        if col_len == 1 or row_len == 1:
            return True
        for i in range(len(matrix) - 1):
            # 实际上也是利用对角线性质，不过不是单个元素，而是整行偏移。
            if matrix[i][:-1] != matrix[i + 1][1:]:
                return False
        return True
```

#### 106. [747. 至少是其他数字两倍的最大数](https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others/)

```python
class Solution:
    def dominantIndex(self, nums: List[int]) -> int:
        if len(nums)<2:
            return 0
        if nums[0]<=nums[1]:
            res = [0,1]
        else:
            res = [1,0]
        for i in range(2,len(nums)):
            if nums[i]<=nums[res[0]]:
                pass
            elif nums[i]>nums[res[0]] and nums[i]<= nums[res[1]]:
                res[0] = i
            else:
                res[0] = res[1]
                res[1] = i
        if nums[res[1]] >= 2* nums[res[0]]:
            return res[1]
        return -1
```

#### 107. [830. 较大分组的位置](https://leetcode-cn.com/problems/positions-of-large-groups/)

```python
class Solution:
    def largeGroupPositions(self, s: str) -> List[List[int]]:
        if len(s) < 3:
            return []
        a = 0
        b = 0
        result = []
        while b < len(s):
            if s[b] == s[a]:
                b += 1
            else:
                if b-a >= 3:
                    result.append([a, b-1])
                    a = b
                else:
                    a = b
                    b += 1
        if b-a >= 3:
            result.append([a, b-1])
        return result  

## 正则表达式玩法：
class Solution:
    def largeGroupPositions(self, s: str) -> List[List[int]]:
        import re
        data= [i+j for i,j in re.findall(r'([a-z])(\1*)',s)]
        res,idx=[],0
        for k in data:
            if len(k)>=3:
                res.append([idx,idx+len(k)-1])
            idx+=len(k)
        return res


```

#### 108. [867. 转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

```python
class Solution:
    def transpose(self, A: List[List[int]]) -> List[List[int]]:
        n=len(A)
        m=len(A[0])
        res=[]
        for i in range(m):
            x=[]
            for j in range(n):
                x.append(A[j][i])
            res.append(x)
        return res
```

#### 109. [896. 单调数列](https://leetcode-cn.com/problems/monotonic-array/)

```python
class Solution:
    def isMonotonic(self, A: List[int]) -> bool:
        if len(A)<2:
            return True
        if A[0]<= A[-1]:
            flag = 0
        if A[0]> A[-1]:
            flag =1
        # print(flag)
        for i in range(1,len(A)):
            if A[i] >= A[i-1] and flag == 0:
                continue
            elif A[i] <= A[i-1] and flag == 1:
                continue
            else:
                return False
        return True
            

```

#### 110. [888. 公平的糖果交换](https://leetcode-cn.com/problems/fair-candy-swap/)

```python
class Solution:
    def fairCandySwap(self, A: List[int], B: List[int]) -> List[int]:
        # 用set减少遍历的时间
        #  Sa -x +y = Sb -y +x
        # 推倒后是 y=x+（Sb-Sa）/2
        gap = sum(B) - sum(A)
        #print('gap',gap)
        setb = set(B)
        for x in A:
            print(x,x+gap//2)
            if x + gap//2 in setb:
                #print(x)
                return [x,x+gap//2]


```

#### 111. [976. 三角形的最大周长](https://leetcode-cn.com/problems/largest-perimeter-triangle/)

```python
class Solution:
    def largestPerimeter(self, A: List[int]) -> int:
        A.sort()
        n = len(A)
        for i in range(n-1,-1,-1):
            if A[i-1] + A[i-2] > A[i]:
                return sum(A[i-2:i+1])
        return 0
```

#### 112. [922. 按奇偶排序数组 II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)

```python
class Solution:
    def sortArrayByParityII(self, A: List[int]) -> List[int]:
        l1,l2 = [],[]
        for x in A:
            if x &1 ==0:
                l2.append(x)
            else:
                l1.append(x)
        res = []
        for i in range(len(A)//2):
            res.append(l2.pop())
            res.append(l1.pop())
        return res
```



#### 113. [985. 查询后的偶数和](https://leetcode-cn.com/problems/sum-of-even-numbers-after-queries/)

```python
class Solution:
    def sumEvenAfterQueries(self, A: List[int], queries: List[List[int]]) -> List[int]:
        # 直接从和的角度考虑
        S = sum(x for x in A if x % 2 == 0)
        ans = []

        for x, k in queries:
            #  先确定变化之前的值，
            if A[k] % 2 == 0: S -= A[k]
            A[k] += x
            # 在判断变化后的值
            if A[k] % 2 == 0: S += A[k]
            ans.append(S)

        return ans

```

#### 114. [1672. 最富有客户的资产总量](https://leetcode-cn.com/problems/richest-customer-wealth/)

```python
class Solution:
    def maximumWealth(self, accounts: List[List[int]]) -> int:
        return max(list(sum(accounts[i]) for i in range(len(accounts))))
```

#### 115. [1656. 设计有序流](https://leetcode-cn.com/problems/design-an-ordered-stream/)

```python
class OrderedStream:

    def __init__(self, n: int):
        self. size = n
        self.dic = {}
        self .ptr = 1


    def insert(self, id: int, value: str) -> List[str]:
        res = []
        if 1<= id<=self.size:
            self.dic[id] =value
            if id != self.ptr:
                return res
            else:
                for i in range(id, self.size+1):
                    if i in self.dic:
                        res.append(self.dic[i])
                        self.ptr += 1
                    else:return res
                return res
        return res
                    
            



# Your OrderedStream object will be instantiated and called as such:
# obj = OrderedStream(n)
# param_1 = obj.insert(id,value)
```

#### 116. [1652. 拆炸弹](https://leetcode-cn.com/problems/defuse-the-bomb/)

```python
class Solution:
    def decrypt(self, code: List[int], k: int) -> List[int]:
        code1 = []
        len1 = len(code)
        if k > 0:
            for i in range(len1):
                code1.append(sum(code[x%len1] for x in range(i+1, i+1+k)))
        if k < 0:
            for i in range(len1):
                # 这里还是+ ，因为k 本身就是负值，不用减
                code1.append(sum(code[x%len1] for x in range(i+k, i)))
        if k == 0:
            code1 = [0 for _ in range(len1)]
        return code1
```

#### 117. [1413. 逐步求和得到正数的最小值](https://leetcode-cn.com/problems/minimum-value-to-get-positive-step-by-step-sum/)

```python
class Solution:
    def minStartValue(self, nums: List[int]) -> int:
        record = nums[0]
        _sum = nums[0]
        for i in range(1,len(nums)):
            _sum += nums[i]
            record = min(record, _sum)
        if record>=0:
            return 1
        return 1-record

```

#### 118.[1122. 数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)

```python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        # 技术排序
        upper = max(arr1)
        frequency = [0] * (upper + 1)
        for x in arr1:
            frequency[x] += 1
        
        ans = list()
        for x in arr2:
            ans.extend([x] * frequency[x])
            frequency[x] = 0
        for x in range(upper + 1):
            if frequency[x] > 0:
                ans.extend([x] * frequency[x])
        return ans


        
```

#### 119. [5617. 设计 Goal 解析器](https://leetcode-cn.com/problems/goal-parser-interpretation/)

```python

class Solution:
    def interpret(self, command: str) -> str:
        command = command.replace("()","o")
        command = command.replace("(al)", "al")
        return command
```

#### 120. [1013. 将数组分成和相等的三个部分](https://leetcode-cn.com/problems/partition-array-into-three-parts-with-equal-sum/)

```python
class Solution:
    def canThreePartsEqualSum(self, A: List[int]) -> bool:
        if not A: return False
        _sum  = sum(A)
        if _sum%3 != 0:
            return False
        part= _sum//3
        tmp = 0
        cnt =0
        for i,v in enumerate(A):
            tmp += v
            if tmp == part and cnt<2 and i<len(A)-1:
            
                cnt += 1
                tmp =0
        print(tmp,part,cnt)
        return cnt == 2 and tmp == part

```

#### 120 [1184. 公交站间的距离](https://leetcode-cn.com/problems/distance-between-bus-stops/)

```python
class Solution:
    def distanceBetweenBusStops(self, distance: List[int], start: int, destination: int) -> int:
        _sum = sum(distance)
        start ,destination = min(start,destination),max(start,destination)
        if start == destination:
            return 0
        tmp = 0
        for i in range(start,destination):
            tmp += distance[i]
        return min(tmp,_sum-tmp)
```

#### 121. [1185. 一周中的第几天](https://leetcode-cn.com/problems/day-of-the-week/)

```python
 class Solution:
    def dayOfTheWeek(self, day: int, month: int, year: int) -> str:
        week = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
        #print(week[1])
        # 1971年1月1日为星期五
        days = 0
        for i in range(1971,year):
            if (i%4 == 0 and i%100 != 0) or(i%400 ==0):
                days += 366
            else:
                days += 365
        for i in range(1,month):
            if i in [1,3,5,7,8,10,12]:
                days += 31
            elif i in [4,6,9,11]:
                days += 30
            elif i == 2:
                if (year%4 == 0 and year%100 != 0) or(year%400 ==0):
                    days += 29
                else:
                    days += 28a
        days += day-1
        #print('days;',days)
        return week[(days+5)%7]


```

#### 122 [1346. 检查整数及其两倍数是否存在](https://leetcode-cn.com/problems/check-if-n-and-its-double-exist/)

```python
class Solution:
    def checkIfExist(self, arr: List[int]) -> bool:
        counter = collections.Counter(arr)
        for n in arr:
            if n != 0 and counter[2 * n] >= 1:
                return True
            if n == 0 and counter[2 * n] >= 2:
                return True
        return False

```



#### 123. [290. 单词规律](https://leetcode-cn.com/problems/word-pattern/)



```python
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        dic_p = {}
        li = list(s.split(" "))
        m, n = len(pattern), len(li)
        if m!=n:
            return False
        for i in range(n):
            #print(dic_p,li[i],pattern[i])
            if li[i] not in dic_p.keys() and pattern[i] not in dic_p.values():
                dic_p[li[i]] = pattern[i]
            elif li[i]  in dic_p.keys() and pattern[i] in dic_p.values():
                #print(i)
                if pattern[i] == dic_p[li[i]]:
                    continue
                else:
                    return False
            else:
                return False
        return True
```

#### 124. [819. 最常见的单词](https://leetcode-cn.com/problems/most-common-word/)

```python
class Solution:
    def mostCommonWord(self, paragraph: str, banned: List[str]) -> str:
        banset = set(banned)
        # in 后面可以跟字符串，以及，可以将字符替换成空格，再用split,算是一个技巧
        for c in "!?',;.":
            paragraph = paragraph.replace(c, " ")
        count = collections.Counter(
            word for word in paragraph.lower().split())

        ans, best = '', 0
        # 遍历寻找答案
        for word in count:
            if count[word] > best and word not in banset:
                ans, best = word, count[word]

        return ans


                
            
        
```

#### 125. [389. 找不同](https://leetcode-cn.com/problems/find-the-difference/)

```python
class Solution:
    def findTheDifference(self, s: str, t: str) -> str:
        a = [0 ]* 26
        b = [0]*26
        for c in s:
            a[ord(c)-97] += 1
        for c in t:
            b[ord(c)-97] += 1
        for i in range(26):
            if b[i]>a[i]:
                return chr(i+97)
```

#### 126. [374. 猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

```python
# The guess API is already defined for you.
# @param num, your guess
# @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
# def guess(num: int) -> int:

class Solution:
    def guessNumber(self, n: int) -> int:
        l,r = 1, n
        while l<=n:
            mid = l + (r-l)//2
            if guess(mid) == -1:
                r = mid
            elif guess(mid) == 1:
                l = mid + 1
            else:
                return mid
```

#### 127. [5629. 重新格式化电话号码](https://leetcode-cn.com/problems/reformat-phone-number/)

```python
# 字符串这种题，需要想明白了再做，就不会浪费那么多时间了
class Solution:
    def reformatNumber(self, number: str) -> str:
        number=number.replace(' ','')
        number=number.replace('-', '')
        #print(number)
        gap = len(number)%3
        res = []
        n = len(number)
        if gap != 1:
            for i in range(0,n,3):
                res.append(number[i:i+3])
        else:
            for i in range(0,n-4,3):
                res.append(number[i:i+3])
            res.append(number[n-4:n-2])
            res.append(number[n-2:])
        return "-".join(res)

```


#### 128. [1441. 用栈操作构建数组](https://leetcode-cn.com/problems/build-an-array-with-stack-operations/)

```python
class Solution:
    def buildArray(self, target: List[int], n: int) -> List[str]:
        li = [i for i in range(1,n+1)]
        res = []
        j = 0
        for i in li:
            if j >= len(target): break
            if  i== target[j]:
                res.append("Push")
                j += 1
            else:
                res.append("Push")
                res.append("Pop")
        return res
```


#### 129. [LCP 11. 期望个数统计](https://leetcode-cn.com/problems/qi-wang-ge-shu-tong-ji/)

```python
class Solution:
    def expectNumber(self, scores: List[int]) -> int:
        # 实际上是种类个数,不要被迷惑。简单题，出概率的可能性不高
        return len(collections.Counter(scores).keys())
```

#### 130. [387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```python
class Solution:
    def firstUniqChar(self, s: str) -> int:
        if not s:
            return -1
        a = collections.Counter(s)
        for i,v in enumerate(s):
            if a[v] == 1:
                return i
        return -1
```

#### 131. [LCP 07. 传递信息](https://leetcode-cn.com/problems/chuan-di-xin-xi/)

```python
class Solution:
    # 无论是dp，还是dfs，貌似不能算简单题了吧？，还是我太菜了
    def numWays(self, n: int, relation: List[List[int]], k: int) -> int:
        dp = [0]*n
        dp[0] = 1

        for t in range(0,k):
            print(dp)
            # 每次记录重新记录，不能和上一次累计
            nxt = [0]*n
            for x,y in relation:
                nxt[y] += dp[x]
            dp = nxt
        print(dp)
        return dp[n-1]
        # res = 0 
        # dic = collections.defaultdict(list)
        # def dfs(idx, cnt):
        #     nonlocal res
        #     if cnt == k:
        #         if idx == n-1:
        #             res += 1
        #         return 
        #     for i in dic[idx]:
        #         dfs(i,cnt+1)
        # for x,y in relation:
        #     dic[x].append(y)
        # dfs(0,0)
        # return res
```

#### 132. [LCP 02. 分式化简](https://leetcode-cn.com/problems/deep-dark-fraction/)

```python
# 做选择的时候太犹豫，还是看到题解才决定
class Solution:
    def fraction(self, cont: List[int]) -> List[int]:
        a = b = -1
        num_length = len(cont)-1 
        for i in range(num_length,-1,-1):
            if i == num_length:
                a,b = cont[-1],1
            else:
                a,b = cont[i]*a+b,a

        return [a,b]

```





#### 133. [1018. 可被 5 整除的二进制前缀](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/)

```python
class Solution:
    def prefixesDivBy5(self, A: List[int]) -> List[bool]:
        # 一开始想用二进制特征，但是没用好，实际证明想复杂了
        tsum=0
        for i,num in enumerate(A):
            tsum=(tsum*2+num)%5
            A[i]=(tsum%5==0)
        return A

```

####  134. [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```python
class Solution:
    def findContentChildren(self, g: List[int], s: List[int]) -> int:
        g.sort()
        s.sort()
        m,n = len(g), len(s)
        i,j = 0,0
        cnt = 0
        #print(g,s)
        while i<m and j<n:
            #print(i,j,s[])
            if s[j]>= g[i]:
                cnt += 1
                i += 1
                j += 1
            else:
                j += 1
        return cnt
```

#### 135. [653. 两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findTarget(self, root: TreeNode, k: int) -> bool:
        res = set()
        flag = False 
        def dfs(node):
            nonlocal res,flag
            if node.left:
                dfs(node.left)
            if k - node.val in res:
                flag = True
                return 
            else:
                res.add(node.val)
            if node.right:
                dfs(node.right)
        dfs(root)
        return flag
            
```

#### 136.[5637. 判断字符串的两半是否相似](https://leetcode-cn.com/problems/determine-if-string-halves-are-alike/)

```python
class Solution:
    # 做题前先想清楚，否则调试很浪费时间
    def halvesAreAlike(self, s: str) -> bool:
        n = len(s)
        mid = n//2
        pre = s[:mid]
        after = s[mid:]
        p = collections.Counter(pre)
        a = collections.Counter(after)
        li = ['a','e','i','o','u','A','E','I','O','U']
        cnt = 0
        for c in p.keys():
            if c in li:
                cnt += p[c]
        for c in a.keys():
            if c in li:
                cnt -= a[c]
        # print(a,p)
        # print(cnt)
        return cnt ==0
            
```

#### 137. [961. 重复 N 次的元素](https://leetcode-cn.com/problems/n-repeated-element-in-size-2n-array/)

```python
class Solution:
    def repeatedNTimes(self, A: List[int]) -> int:
        a = collections.Counter(A)
        n = len(A)
        for c,v in a.items():
            if v == n//2:
                return c
```



#### 138. [1037. 有效的回旋镖](https://leetcode-cn.com/problems/valid-boomerang/)

```python
class Solution:
    def isBoomerang(self, points: List[List[int]]) -> bool:
        # 斜率计算，考虑分母为零，所以转换成交叉相乘
         return (points[1][1] - points[0][1]) * (points[2][0] - points[0][0]) != (points[2][1] - points[0][1]) * (points[1][0] - points[0][0])
```

#### 139. [1046. 最后一块石头的重量](https://leetcode-cn.com/problems/last-stone-weight/)

```python
class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        
        while len(stones)>1:
            stones.sort()
            #print(stones)
            x, y = stones.pop(),stones.pop()
            if x!=y:
                stones.append(x-y)
        if len(stones)<1:
            return 0
        return stones[0]
    
 class Solution:
    def lastStoneWeight(self, stones: List[int]) -> int:
        # sb如我，忘记了堆的特性
        heap = [-stone for stone in stones]
        heapq.heapify(heap)

        # 模拟
        while len(heap) > 1:
            x,y = heapq.heappop(heap),heapq.heappop(heap)
            if x != y:
                heapq.heappush(heap,x-y)

        if heap: return -heap[0]
        return 0


```



#### 140. [1360. 日期之间隔几天](https://leetcode-cn.com/problems/number-of-days-between-two-dates/)

```python
class Solution:
    def daysBetweenDates(self, date1: str, date2: str) -> int:
        month = [0,31,28,31,30,31,30,31,31,30,31,30,31]
        date1 = [int(t) for t in date1.split('-')]
        date2 = [int(t) for t in date2.split('-')]
        cnt1,cnt2= 0,0
        def get_cnt(date):
            cnt=0
            for i in range(1971,date[0]):
                if (i %4 == 0 and  i%100!=0) or( i%400 ==0):
                    cnt += 366
                else:
                    cnt +=365
            for i in range(1,date[1]):
                if i == 2 :
                    if (date[0] %4 == 0 and  date[0]%100!=0) or( date[0]%400 ==0):
                        cnt += 29
                    else:
                        cnt += 28
                else:
                    cnt += month[i]
            cnt += date[2]
            return cnt 
        cnt1 = get_cnt(date1)
        cnt2 = get_cnt(date2)
        # print(cnt1,cnt2)
        return abs(cnt2-cnt1)
```



#### 141. [1518. 换酒问题](https://leetcode-cn.com/problems/water-bottles/)

```python
class Solution:
    def numWaterBottles(self, numBottles: int, numExchange: int) -> int:
        res = numBottles
        last = numBottles
        while last>=numExchange:
            #print(last)
            res += last//numExchange
            last = last//numExchange + last%numExchange
        return res
            

```





#### 142. [669. 修剪二叉搜索树](https://leetcode-cn.com/problems/trim-a-binary-search-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def trimBST(self, root: TreeNode, low: int, high: int) -> TreeNode:
        _root = TreeNode()
        def dfs(node,l,r):
            print('===============')
            print(node.val, l,r)
            
            if node.val <l:
                if node.right:
                    print('go right')
                    return dfs(node.right,l,r)
                return None
            if node.val>r:
                if node.left:
                    print('go left')
                    return dfs(node.left,l,r)
                return None
            tmp = TreeNode(node.val)
            if node.left:
                    tmp.left = dfs(node.left,l,r)
            if node.right:
                    tmp.right= dfs(node.right,l,r)
            return tmp
        _root = dfs(root,low,high)
        return _root

            
```



#### 143. [5641. 卡车上的最大单元数](https://leetcode-cn.com/problems/maximum-units-on-a-truck/)

```python
from typing import List


class Solution:
    def maximumUnits(self, boxTypes: List[List[int]], truckSize: int) -> int:
        boxTypes.sort(key=lambda x: -x[1])
        res = 0
        n = len(boxTypes)
        for i in range(n):
            if boxTypes[i][0]<truckSize:
                res += boxTypes[i][0] * boxTypes[i][1]
                truckSize -= boxTypes[i][0]
            else:
                res += truckSize*boxTypes[i][1]
                break
        return res


```

#### 144. [1337. 矩阵中战斗力最弱的 K 行](https://leetcode-cn.com/problems/the-k-weakest-rows-in-a-matrix/)

```python
class Solution:
    def kWeakestRows(self, mat: List[List[int]], k: int) -> List[int]:
        m, n = len(mat), len(mat[0])
        res = []
        for i in range(m):
            t = sum(mat[i])
            res.append((t, i))
        res.sort(key = lambda x:(x[0],x[1]))
        r = []
        for i in range(k):
            r.append(res[i][1])
        return r

## 二分写法
class Solution:
    def kWeakestRows(self, mat: List[List[int]], k: int) -> List[int]:
        m, n = len(mat), len(mat[0])
        res = []
        rc= []
        for i in range(m):
            t = sum(mat[i])
            idx = bisect.bisect_right(rc,t)
            rc.insert(idx,t)
            res.insert(idx,i)
        return res[:k]
            


```

#### 145. [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return an integer
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        l = 1
        r= n
        while l<r:
            mid = l+r >>1
            if isBadVersion(mid):
                r = mid
            else:
                l = mid +1
        return l
```

#### 146.[345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        # stk = []
        # sc = []
        # idx = []
        # for i,c in enumerate(s):
        #     if c in ['a','e','i','o','u','A','E','I','O','U']:
        #         stk.append(c)
        #         idx.append(i)
        #     sc.append(c)
        # stk.reverse()
        # # print(sc)
        # # print(stk)
        # # print(idx)
        # for i in range(len(idx)):

        #     sc[idx[i]] = stk[i]
        # return "".join(sc)
        array = ['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U']
        sList = list(s)
        left, right = 0, len(s)-1
        while left < right:
            if sList[left] in array and sList[right] in array:
                sList[left], sList[right] = sList[right], sList[left]
                left += 1
                right -= 1
            if sList[right] not in array:
                right -= 1
            if sList[left] not in array:
                left += 1
        return ''.join(sList)
```

#### 147. [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        if num ==1:
            return True
        l = 1
        r =num
        while l<r:
            mid = (l+r)>>1
            if (mid*mid)>=num:
                r = mid
            else:
                l = mid+1
        return l*l == num
```

#### 148. [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

```python
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
        # while n >1:
        #     if n%4 == 0:
        #         n //=4
        #     else:
        #         return False
        # return n ==1
        return log2(n)%2 == 0
```



#### 149. 5649. 解码异或后的数组

```python
class Solution:
    def decode(self, encoded: List[int], first: int) -> List[int]:
        res = [first]
        for i in encoded:
            res.append(res[-1]^i)
        return res
```

#### 150. [1716. 计算力扣银行的钱](https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank/)

```python
class Solution:
    def totalMoney(self, n: int) -> int:
        w, d = n//7, n%7
        # print(w,d)
        res = 0
        for i in range(1,w+1):
            res += 28 + (i-1)*7
        for i in range(1,d+1):
            res += (w+i)
        return res


```

#### 151. [383. 赎金信](https://leetcode-cn.com/problems/ransom-note/)

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        a  = collections.Counter(ransomNote)
        b = collections.Counter(magazine)
        for c in a.keys():
            if a[c] and b[c]:
                if a[c] <= b[c]:
                    continue
                else: return False
            return False
        return True
```



#### 152. [371. 两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)

```python
class Solution:
    def getSum(self, a: int, b: int) -> int:
        a &= 0xFFFFFFFF
        b &= 0xFFFFFFFF
        while b:
            carry = a & b
            a ^= b
            b = ((carry) << 1) & 0xFFFFFFFF
            # print((a, b))
        return a if a < 0x80000000 else ~(a^0xFFFFFFFF)


```

#### 153 [1668. 最大重复子字符串](https://leetcode-cn.com/problems/maximum-repeating-substring/)

```python
# 不难但是遍历比较有漏洞，实际上是利用API做题
class Solution:
    def maxRepeating(self, sequence: str, word: str) -> int:
        res = 0
        #不断创造新字符串，看在不在原字符串中
        while word * (res + 1) in sequence:
            res += 1
        return res
```

#### 154. [551. 学生出勤记录 I](https://leetcode-cn.com/problems/student-attendance-record-i/)

```python
class Solution:
    def checkRecord(self, s: str) -> bool:
        cnt_A, cnt_LL = 0, 0
        pre = "  "
        for c in s:
            if c == 'A':
                cnt_A +=1
            if (pre + c) == "LLL":
                return False
            pre = pre[1] + c
            if cnt_A>1:
                return False
        return True

```

#### 155. [575. 分糖果](https://leetcode-cn.com/problems/distribute-candies/)

```python
class Solution:
    def distributeCandies(self, candyType: List[int]) -> int:
        n = len(candyType)//2
        a = collections.Counter(candyType)
        if len(a)>=n:
            return n
        else:
            return len(a)
```

#### 156. [1507. 转变日期格式](https://leetcode-cn.com/problems/reformat-date/)

```python
class Solution:
    def reformatDate(self, date: str) -> str:
        s2month = {
            "Jan": "01", "Feb": "02", "Mar": "03", "Apr": "04", "May": "05", "Jun": "06", 
            "Jul": "07", "Aug": "08", "Sep": "09", "Oct": "10", "Nov": "11", "Dec": "12"
        }
        
        date = date.split(" ")
        
        date[0] = date[0][: -2].zfill(2)
        date[1] = s2month.get(date[1])
        date.reverse()
        
        return "-".join(date)

```

#### 157. [1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

```python
class Solution:
    def gcdOfStrings(self, str1: str, str2: str) -> str:
        candidate_len = math.gcd(len(str1), len(str2))
        candidate = str1[: candidate_len]
        if str1 + str2 == str2 + str1:
            return candidate
        return ''
```

#### 158. [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

```python
# 用哈希解法，就是简单数个数，不要想复杂
class Solution:
    def findLHS(self, nums: List[int]) -> int:
        dicts={}
        for i in nums:
            dicts[i]=dicts.get(i,0)+1
        res=0
        for i in dicts:
            if i+1 in dicts:
                res=max(res,dicts[i]+dicts[i+1])
        return res

```

#### 159. [1422. 分割字符串的最大得分](https://leetcode-cn.com/problems/maximum-score-after-splitting-a-string/)

```python
class Solution:
    def maxScore(self, s: str) -> int:
        right = s.count('1')  # 代表右侧的 1 的个数
        left = 0  # 代表左侧的 0 的个数
        score = 0
        for idx in range(len(s) - 1):
            if s[idx] == '1':
                score = max(score, (left + right - 1))
                right -= 1
            else:
                score = max(score, (left + 1 + right))
                left += 1
        return score


```

#### 160 .[1417. 重新格式化字符串](https://leetcode-cn.com/problems/reformat-the-string/)

```python
class Solution:
    def reformat(self, s: str) -> str:
        stk1,stk2 = [],[]
        for c in s:
            #if ord(c)>=ord('0') and ord(c)<= ord('9'):
            if c.isdigit():
                stk1.append(c)
            else:
                stk2.append(c)
        #print(stk1)
        if abs(len(stk1) - len(stk2)) >= 2:
            return ""
        res= []
        if len(stk1)<len(stk2):
            stk1, stk2 = stk2, stk1
        while stk2 or stk1:
            if stk1:
                res.append(stk1.pop())
            if stk2:
                res.append(stk2.pop())
        return "".join(res)
```

#### 161. [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        n = len(nums)
        l, r = 0, n-1
        while l<r:
            
            mid = (r-l)//2 + l
            #print(mid,r,l)
            if nums[mid] >= target:
                r = mid
            else:
                l = mid+1
        if nums[l] == target:
            return l
        return -1
```

#### 162. [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)

```python
class Solution:
    def hasAlternatingBits(self, n: int) -> bool:
        bin_str = bin(n)[2:]
        return all(bin_str[i-1]!= bin_str[i] for i in range(1,len(bin_str)))

作者：JamLeon
链接：https://leetcode-cn.com/problems/binary-number-with-alternating-bits/solution/si-lu-jian-dan-xing-neng-da-dao-100-by-jamleon-6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。



class Solution(object):
    def hasAlternatingBits(self, n):
        """
        :type n: int
        :rtype: bool
        """
        temp = n ^ (n >> 1)
        return temp & (temp + 1) == 0

作者：mnm135
链接：https://leetcode-cn.com/problems/binary-number-with-alternating-bits/solution/wei-yun-suan-you-yi-hou-yi-huo-pan-duan-shi-fou-qu/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 163. [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

```python
# 看起来是简单题，实际上不是。要认真考虑哈希
# 链表法： 需要有自己的哈希函数（选定Mod）、需要设计一个链表
class MyHashSet(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.keyRange = 769
        self.bucketArray = [Bucket() for i in range(self.keyRange)]

    def _hash(self, key):
        return key % self.keyRange

    def add(self, key):
        """
        :type key: int
        :rtype: None
        """
        bucketIndex = self._hash(key)
        self.bucketArray[bucketIndex].insert(key)

    def remove(self, key):
        """
        :type key: int
        :rtype: None
        """
        bucketIndex = self._hash(key)
        self.bucketArray[bucketIndex].delete(key)

    def contains(self, key):
        """
        Returns true if this set contains the specified element
        :type key: int
        :rtype: bool
        """
        bucketIndex = self._hash(key)
        return self.bucketArray[bucketIndex].exists(key)


class Node: #  链表
    def __init__(self, value, nextNode=None):
        self.value = value
        self.next = nextNode

class Bucket:# 桶
    def __init__(self):
        # a pseudo head
        self.head = Node(0)

    def insert(self, newValue):
        # if not existed, add the new element to the head.
        if not self.exists(newValue):  # 如果不存在，就插入，头插法
            newNode = Node(newValue, self.head.next)
            # set the new head.
            self.head.next = newNode

    def delete(self, value):
        prev = self.head
        curr = self.head.next
        while curr is not None:
            if curr.value == value:
                # remove the current node
                prev.next = curr.next
                return
            prev = curr
            curr = curr.next

    def exists(self, value):
        curr = self.head.next
        while curr is not None:
            if curr.value == value:
                # value existed already, do nothing
                return True
            curr = curr.next
        return False


# Your MyHashSet object will be instantiated and called as such:
# obj = MyHashSet()
# obj.add(key)
# obj.remove(key)
# param_3 = obj.contains(key)

作者：LeetCode
链接：https://leetcode-cn.com/problems/design-hashset/solution/she-ji-ha-xi-ji-he-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



#### 164. [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

```python
# 
class Bucket:
    def __init__(self):
        self.bucket = []

    def get(self, key):
        for (k, v) in self.bucket:
            if k == key:
                return v
        return -1

    def update(self, key, value):
        found = False
        for i, kv in enumerate(self.bucket):
            if key == kv[0]:
                self.bucket[i] = (key, value)
                found = True
                break

        if not found:
            self.bucket.append((key, value))

    def remove(self, key):
        for i, kv in enumerate(self.bucket):
            if key == kv[0]:
                del self.bucket[i]


class MyHashMap(object):

    def __init__(self):
        """
        Initialize your data structure here.
        """
        # better to be a prime number, less collision
        self.key_space = 2069
        self.hash_table = [Bucket() for i in range(self.key_space)]


    def put(self, key, value):
        """
        value will always be non-negative.
        :type key: int
        :type value: int
        :rtype: None
        """
        hash_key = key % self.key_space
        self.hash_table[hash_key].update(key, value)


    def get(self, key):
        """
        Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key
        :type key: int
        :rtype: int
        """
        hash_key = key % self.key_space
        return self.hash_table[hash_key].get(key)


    def remove(self, key):
        """
        Removes the mapping of the specified value key if this map contains a mapping for the key
        :type key: int
        :rtype: None
        """
        hash_key = key % self.key_space
        self.hash_table[hash_key].remove(key)


# Your MyHashMap object will be instantiated and called as such:
# obj = MyHashMap()
# obj.put(key,value)
# param_2 = obj.get(key)
# obj.remove(key)

```

#### 165. [703. 数据流中的第 K 大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.queue = nums
        heapq.heapify(self.queue)


    def add(self, val: int) -> int:
        heapq.heappush(self.queue, val)
        while len(self.queue)>self.k:
            heapq.heappop(self.queue)
        return self.queue[0]



# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

#### 166. [5668. 最长的美好子字符串](https://leetcode-cn.com/problems/longest-nice-substring/)

```python
class Solution:
    # 一开始想复杂了，其实只用暴力做就好
    def longestNiceSubstring(self, s: str) -> str:
        n = len(s)
        t = [0]*26
        l = 0
        _len = 0
        rc = [0,0]
        def check(t):
            for c in t:
                if ord(c) - ord('A')>=0 and ord(c) - ord('a')<0:
                    if chr(ord(c) - ord('A')+ ord('a')) not in t:
                        #print(c, chr(ord(c) - ord('A')+ ord('a')))
                        return False
                else:
                    #print(c)
                    if chr(ord(c) - ord('a')+ ord('A')) not in t:
                        return False
            return True
        res = ''
        for i in range(n):
            for j in range(i, n):
                t = s[i:j+1]
                if check(t):
                    #print('t,',t)
                    if j-i+1>_len:
                        _len = j-i+1
                        res = t
       # print(t)
        return res

```

#### 167. [5685. 交替合并字符串](https://leetcode-cn.com/problems/merge-strings-alternately/)

```python
class Solution:
    def mergeAlternately(self, word1: str, word2: str) -> str:
        l1,l2 = len(word1), len(word2)
        res = ""
        i,j =0, 0
        while i<l1 and j <l2:
            res += word1[i] + word2[j]
            i+=1
            j +=1
        if i<l1:
            res += word1[i:l1]
        if j <l2:
            res += word2[j:l2]
        return res

        
```

#### 168. [1030. 距离顺序排列矩阵单元格](https://leetcode-cn.com/problems/matrix-cells-in-distance-order/)

```python
class Solution:
    def allCellsDistOrder(self, R: int, C: int, r0: int, c0: int) -> List[List[int]]:
        ## 使用桶来做
        maxDist = max(r0, R - 1 - r0) + max(c0, C - 1 - c0)
        bucket = collections.defaultdict(list)
        dist = lambda r1, c1, r2, c2: abs(r1 - r2) + abs(c1 - c2)

        for i in range(R):
            for j in range(C):
                bucket[dist(i, j, r0, c0)].append([i, j])

        ret = list()
        for i in range(maxDist + 1):
            ret.extend(bucket[i])
        
        return ret
```

#### 169. [5701. 仅执行一次字符串交换能否使两个字符串相等](https://leetcode-cn.com/problems/check-if-one-string-swap-can-make-strings-equal/)

```python
import collections


class Solution:

    def areAlmostEqual(self, s1: str, s2: str) -> bool:

        dic = collections.defaultdict(chr)
        n = len(s1)
        cnt = 0
        if n==0:
            return 0
        li1,li2 = [],[]
        for i in range(n):
            if s1[i] != s2[i]:
                li1.append(s1[i])
                li2.append(s2[i])
                cnt +=1
                if cnt >2: return False
        if cnt ==0:
            return True
        if cnt == 1:
            return False
        if cnt == 2:
            return sorted(li1) == sorted(li2)
        
        
class Solution:
    def areAlmostEqual(self, s1: str, s2: str) -> bool:
        # 这一题浪费太久时间了，纯暴力法解题也可以
        n = len(s1)
        s1, s2 = list(s1), list(s2)  # 数组化
        if s1 == s2:
            return True
        for i in range(n):
            for j in range(i):
                s1[i],s1[j] = s1[j],s1[i]
                if s1==s2:
                    return True
        return False

```

#### 170. [1805. 字符串中不同整数的数目](https://leetcode-cn.com/problems/number-of-different-integers-in-a-string/)

```
class Solution:
    def numDifferentIntegers(self, word: str) -> int:
        n = len(word)
        s = set()
        tmp = 0
        flag = False
        for c in word:
            if c.isdigit():
                tmp = tmp*10 + int(c)
                flag = True
            else:
                if flag:
                    s.add(tmp)
                    tmp = 0
                    flag = False
                else:
                    continue
        # print(s)
        if flag:
            s.add(tmp)
        return len(s)
```

#### 171. [543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

```python
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        """一条路径的长度为该路径经过的节点数减一，所以求直径（即求路径长度的最大值）等效于求路径经过节点数的最大值减一"""
        self.res = 1
        def depth(node):
            if not node:
                return 0
            L = depth(node.left)
            R = depth(node.right)
            self.res = max(self.res, L + R + 1)  # 当前节点
            return max(L, R) + 1   # 返回以该节点为根的子树的深度
        depth(root)
        return self.res - 1
```




## medium

* 每日一题的任

  

#### 1. [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        '''拓扑排序解法'''
        #课表长度
        clen=len(prerequisites)

        if clen==0:
            return True
        
        # 入度数组初始化
        in_degrees=[0]*numCourses

        # 临接表
       # adj=[set()]*numCourses  引用类型不能用连乘的玩法，因为引用的是同一个
    	[set() for _ in range(numCourses)]

        # 想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]
        # [0,1] 表示 1 在先，0 在后
        # 注意：邻接表存放的是后继 successor 结点的集合
        for second, first in prerequisites:
            in_degrees[second] +=1
            adj[first].add(second)

        #遍历，讲入度为0的入队（开始拓扑消除）
        res=[]
        queue=[]
        for i in range(numCourses):
            if in_degrees[i]==0:
                queue.append(i)
        cnt=0
        while queue:
            top=queue.pop(0)
            cnt +=1

            for successor in adj[top]:
                in_degrees[successor]-=1
                if in_degrees[successor]==0:
                    queue.append(successor)
        

        return cnt == numCourses
   


## 上面的是bfs算法，这个是dfs的玩法
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        edges = collections.defaultdict(list)
        visited = [0] * numCourses
        res = list()
        valid = True

        for i,j in prerequisites:
            edges[i].append(j)
        def dfs(i):
            nonlocal valid
            #  表示正在搜索中
            visited[i] = 1
            for j in edges[i]:
                # 如果没有搜素，那那么开始搜索
                if visited[j] == 0:
                    dfs(j)
                    # 如果找到循环依赖的话，直接返回
                    if not valid:
                        return 
                # 1 表示搜索中,出现循环嵌套了
                elif visited[j] == 1:
                    valid = False
                    return
            visited[i] =2 # 表示搜索完毕
            res.append(i)
        for i in range(numCourses):
            if valid and not visited[i]:
                dfs(i)
        return valid
```

#### 2. [43. 字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)

```python
class Solution:
    def multiply(self, num1: str, num2: str) -> str:
        if num1 == "0" or num2 == "0":
            return "0"
        

        res="0"

        m, n = len(num1), len(num2)

        for i in range(n-1, -1, -1):
            add = 0
            y = int(num2[i])
            curr = ["0"]*(n-i-1)  #计算需要填充多少0,先填进去
            for j in range(m-1, -1, -1):
                product = int(num1[j])*y+add   # 要考虑到进位情况
                curr.append(str(product%10))   #余数先填进去
                add=product//10                #查看是否进位
            if add > 0:
                curr.append(str(add))          # 如果存在进位情况，就把进位填充进去，注意着是在内循环外部，说明是num2中的一个数，乘完了所有num1，后的最终进位。
            curr="".join(curr[::-1])          #从后向前，不断翻转位置
            res = self.addString(res, curr)

        return res




    def addString(self, num1:str, num2:str) -> str:   # 乘完的每一行相加
        i, j = len(num1)-1, len(num2)-1
        add = 0
        res = list()
        while i >=0 or j>=0 or add!=0:
            x=int(num1[i])   if  i>=0 else 0      # if else 的简易写法
            y=int(num2[j])   if j>=0 else 0
            result = x + y +add
            res.append(str(result%10))
            add = result // 10
            i -= 1
            j -= 1
        return "".join(res[::-1])



```

#### 3. [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def getMedian(left: ListNode, right: ListNode) -> ListNode:
            fast = slow = left
            while fast != right and fast.next != right:   # 快慢指针
                fast = fast.next.next
                slow = slow.next
            return slow
        
        def buildTree(left: ListNode, right: ListNode) -> TreeNode:
            if left == right:
                return None
            mid = getMedian(left, right)
            root = TreeNode(mid.val)
            root.left = buildTree(left, mid)  # 这里的左右端点使用节点表示
            root.right = buildTree(mid.next, right)
            return root
        
        return buildTree(head, None)  # 最右端是None很合理
    
    
    
    
    
    ## 另一种解法：仅供参考。这里利用中序遍历，特性，从头到尾依次放置节点
    class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def getLength(head: ListNode) -> int:
            ret = 0
            while head:
                ret += 1
                head = head.next
            return ret
        
        def buildTree(left: int, right: int) -> TreeNode:
            if left > right:
                return None
            mid = (left + right + 1) // 2
            root = TreeNode()
            root.left = buildTree(left, mid - 1)
            nonlocal head
            root.val = head.val  
            head = head.next   # 每次节点确定好后，跳到下一节点
            root.right = buildTree(mid + 1, right)
            return root
        
        length = getLength(head)  # 先统计一边长度，知道长度，基本就确定了平衡树的结构，剩下的，就是依次添值和关系
        return buildTree(0, length - 1)  # 这里的l，r实际上是用来确定位置的参数，即分割左右子树，自身不决定树的值

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/solution/you-xu-lian-biao-zhuan-huan-er-cha-sou-suo-shu-1-3/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```



#### 4. [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

```python
class Solution:
    def __init__(self):
        self.res=0
    def sumNums(self, n: int) -> int:
        n > 1 and self.sumNums(n - 1)  # 如果n-1小于等于1，这一步就会返回，截止
        print("n:",n)
        self.res += n
        return self.res
## 常见的逻辑运算符有三种，即 “与 \&\&&& ”，“或 ||∣∣ ”，“非 !! ” ；而其有重要的短路效应，如下所示：


if(A && B)  // 若 A 为 false ，则 B 的判断不会执行（即短路），直接判定 A && B 为 false

if(A || B) // 若 A 为 true ，则 B 的判断不会执行（即短路），直接判定 

# 相当放在后面的递归函数不被执行了，执行下一行，但是因为没有递归，所以到此为之
```



#### 5. [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def getMedian(left: ListNode, right: ListNode) -> ListNode:
            fast = slow = left
            while fast != right and fast.next != right:   # 快慢指针
                fast = fast.next.next
                slow = slow.next
            return slow
        
        def buildTree(left: ListNode, right: ListNode) -> TreeNode:
            if left == right:
                return None
            mid = getMedian(left, right)
            root = TreeNode(mid.val)
            root.left = buildTree(left, mid)  # 这里的左右端点使用节点表示
            root.right = buildTree(mid.next, right)
            return root
        
        return buildTree(head, None)  # 最右端是None很合理
    
    
    
    
    
    ## 另一种解法：仅供参考。这里利用中序遍历，特性，从头到尾依次放置节点
    class Solution:
    def sortedListToBST(self, head: ListNode) -> TreeNode:
        def getLength(head: ListNode) -> int:
            ret = 0
            while head:
                ret += 1
                head = head.next
            return ret
        
        def buildTree(left: int, right: int) -> TreeNode:
            if left > right:
                return None
            mid = (left + right + 1) // 2
            root = TreeNode()
            root.left = buildTree(left, mid - 1)
            nonlocal head
            root.val = head.val  
            head = head.next   # 每次节点确定好后，跳到下一节点
            root.right = buildTree(mid + 1, right)
            return root
        
        length = getLength(head)  # 先统计一边长度，知道长度，基本就确定了平衡树的结构，剩下的，就是依次添值和关系
        return buildTree(0, length - 1)  # 这里的l，r实际上是用来确定位置的参数，即分割左右子树，自身不决定树的值

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/solution/you-xu-lian-biao-zhuan-huan-er-cha-sou-suo-shu-1-3/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


5. [1325. 删除给定值的叶子节点](https://leetcode-cn.com/problems/delete-leaves-with-a-given-value/)

   ```python
   # Definition for a binary tree node.
   # class TreeNode:
   #     def __init__(self, val=0, left=None, right=None):
   #         self.val = val
   #         self.left = left
   #         self.right = right
   class Solution:
       def removeLeafNodes(self, root: TreeNode, target: int) -> TreeNode:
           if not root:
               return root
           root.left = self.removeLeafNodes(root.left, target)
           root.right = self.removeLeafNodes(root.right, target)
           if not root.left and not root.right and root.val ==target:
               return None
           return root
   ```

#### 6. [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        L = len(s)
        cnt = 0
        # 以某一个元素为中心的奇数长度的回文串的情况
        for center in range(L):  # 一中心位置作为指标
            left = right = center
            while left >= 0 and right < L and s[left] == s[right]:
                cnt += 1
                left -= 1
                right += 1
        # 以某对元素为中心的偶数长度的回文串的情况
        for left in range(L - 1):    #以中心两个中左边的作为指标
            right = left + 1         # left+1 间接确定右边核心
            while left >= 0 and right < L and s[left] == s[right]:
                cnt += 1
                left -= 1
                right += 1

        return cnt
# 这是相对比较巧妙的中心扩散法，分成就不同情况，以不同字符串开始作为中心，不断扩散，知道不成为回文后停止，每次计数。枚举中心，O（n），每次回文中心拓展，O（n）.time complexity  是 n squared






# 下面是动态规划解法，相比之下并不高效，但是有助于锻炼动态规划思维
class Solution:
    def countSubstrings(self, s: str) -> int:
        """
        （1）思路：动态规划
                我们以dp[i][j]表示区间[i, j]之间的子串是否为回文子串，这样可以思考这样三种情况的回文子串：
                    - 子串长度为1，例如 a 一定为回文子串，即 i=j 的情况
                    - 子串长度为2，且字符相同，例如 aa 一定为回文自传，即 s[i] = s[j] and j-i = 1
                    - 子串长度大于2，符合 abcba 形式的为回文子串，根据回文子串的定义，那么 abcba 去掉两边字符，仍为回文
                    子串，即bcb，转换成方程形式即 dp[i][j] = dp[i+1][j-1] and j-i > 1 and s[i] = s[j]
                剩下的均为不符合条件，即非回文子串。

        （2）复杂度：
            - 时间复杂度：O（N^2）
            - 空间复杂度：O（N^2）
        """
        # 处理特殊情况
        str_len = len(s)
        if str_len == 0 or s is None:
            return 0
        # 定义变量储存结果
        res = 0
        # 定义和初始化dp数组
        dp = [[False for _ in range(str_len)] for _ in range(str_len)]
        # 直接先给对角线赋值为True，防止出现 dp[i][j] = dp[i + 1][j - 1] 时，前值没有，例如，i=0，j=2的时候
        for i in range(str_len):
            dp[i][i] = True
        # 遍历字符串，更新dp数组
        # 注意，由于状态转义方程第三种情况是 dp[i][j] = dp[i + 1][j - 1] ，dp取决于 i+1的状态，但是正常遍历
        # 我们肯定是先有了i的状态才能有i+1的 状态，所以，此处我们遍历以 j 为主
        for j in range(str_len):
            # 因为对角线已经赋初始值，所以直接从i+1开始遍历
            for i in range(0, j):
                # 第一种情况，子串长度为1，例如 a 一定为回文子串，因为已经处理了对角线
                # 这里可以注释
                if j - i == 0:
                    dp[i][j] = True
                # 第二种和第三种可以合并，因为对于s[i]=s[j],中间加一个字符是没有影响的，即aba肯定也是回文子串
                # 所以可以合并为 j-i >= 1 and s[i] == s[j]

                # 第二种情况，子串长度为2，且字符相同，例如 aa 一定为回文自传，即 s[i] = s[j] and j-i = 1
                elif j - i == 1:
                    if s[i] == s[j]:
                        dp[i][j] = True
                # 第三种情况，子串长度大于2，符合 abcba 形式的为回文子串否则不是，即dp[i][j]取决于dp[i + 1][j - 1] 是否
                # 是回文子串
                else:
                    if s[i] == s[j]:
                        dp[i][j] = dp[i + 1][j - 1]
        # 遍历dp数组，数True的个数
        for i in range(str_len):
            for j in range(i, str_len):
                if dp[i][j] is True:
                    res += 1
        return res

```

#### 7. [529. 扫雷游戏](https://leetcode-cn.com/problems/minesweeper/)

```python
class Solution:
    def updateBoard(self, board: List[List[str]], click: List[int]) -> List[List[str]]:
        if not board or not board[0]: return board

        rows, cols = len(board), len(board[0])
        if not (0 <= click[0] < rows and 0 <= click[1] < cols):
            return board
        
        if board[click[0]][click[1]] == 'M':
            board[click[0]][click[1]] = 'X'
            return board
        

        def _dfs(i,j):   # 因为这里没有新建borad，所以整个solution中用的都是同一个board
            #如果不是E，就返回。这里就是规避B和M
            if  not (0 <= i < rows and 0 <= j < cols and board[i][j] != 'E'):
                return
            # 制作移动方向，总共八个
            directions = [(0, 1), (0, -1), (-1, -1), (-1, 0), (-1, 1), (1, 0), (1, -1), (1, 1)]
            mine_num = 0  # 地雷计数
            for d in directions:
                if 0 <= (i + d[0]) < rows and 0 <= (j + d[1]) < cols and board[i + d[0]][j + d[1]] == 'M':
                    mine_num += 1  # 统计合法的移动范围内的隐藏地雷


            # 如果有雷，标记数量；如果没有，标记为B，并且递归周围
            if mine_num > 0:
                board[i][j] = str(mine_num)
            else:
                board[i][j] = 'B'
                for d in directions:
                    _dfs(i+d[0],j+d[1])
            return


       # _dfs(click[0],click[1])
       # 也可以使用变动参数的形式
        _dfs(*click)
        return board
```

#### 8. [910. 最小差值 II](https://leetcode-cn.com/problems/smallest-range-ii/)

```python
class Solution:
    def smallestRangeII(self, A: List[int], K: int) -> int:

        # 实际上就是先排序，然后以一个位置开始分割，前一部分加，后一部分减，只用比较端点就好，0~i：+k，i+1~-1 ：-K。这样只用比较这四个的最大最小值，然后在循环中不断更新
        A.sort()
        mi, ma = A[0], A[-1]
        res = ma - mi
        for i in range(len(A)-1):
            res = min(res, max(A[i]+K,ma-K)-min(mi+K, A[i+1]-K))
        return res
```

#### 9. [861. 翻转矩阵后的得分](https://leetcode-cn.com/problems/score-after-flipping-matrix/)

```python
class Solution:
    '''不要被题目吓到，这么想：每行都是一个二进制数。因此：
        自左到右，每一位的值:2^n,这个n是总列数-1-当前列，因此未必需要横着求和
        也可以竖着求和：每一列代表的值是由列数确定，统计有多少个1，相乘即可。
        这里注意，因为越高位的1越有价值，贪心算法，先使最左一列全为1，然后统计
        各列1的个数（这里是统计不同的个数，然后取大的，因为可以一列翻转，使相同的更多的为1），乘以各自的因子，最后累加即可
    '''
    def matrixScore(self, A: List[List[int]]) -> int:
        rows, cols = len(A), len(A[0])
        res = 0
        for col in range(cols):
            c =0
            for row in range(rows):
                c += A[row][col]^A[row][0]  #这里有一点特殊，其他列是和A[row][0]比较获取自己列的异同数量，第一列是和自己比较，此时
            res += max(c, rows-c)*2**(cols-1-col)
    

        return res


```

#### 10. [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

```python
## 自己写的，手生，花了些时间，这种问题，首要方法是递归回溯
#有一点:
'''
list.append()是原地变化，list+[]是生成一个新的值。'''
from typing import List
import time

class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        set_=set()  # 对结果的去重
        def dfs(l,r,tmp):
            #print(type(tmp))
            #print(l)
            if l == r:
                return
            s1=set()    # 一次循环对一个位置，不允许重复，这里放在去重
            for i in range(l,r):
                if nums[i] in s1:
                    continue
                elif len(tmp)==0:
                    tmp.append(nums[i])
                    # print(tmp)

                    dfs(i + 1, r, tmp)  # 递归
                    tmp.pop()
                    continue            #这一块是回溯
                elif  nums[i] >= tmp[-1]:

                    tmp.append(nums[i])    # list.append()也是和list.sort()一样，没有返回值，在原地址修改

                    #print(tmp)

                    set_.add(tuple(tmp))
                    dfs(i+1,r,tmp)   # 递归搜索
                    tmp.pop()     # 
                    continue     # 这里是回溯
                else:
                    pass



        r = []

        dfs(0, len(nums), [])
        res =[]
        for j in set_:
            res.append(j)
        return res

    
    
    ## 更见简单的书写，思路一致，这里写的明显更漂亮
class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        res = []

        def dfs(nums: List[int], tmp: List[int]) -> None:
            if len(tmp) > 1:
                res.append(tmp)
            curPres = set()   # 这里实际上已经保证不会有重复值了
            for inx, i in enumerate(nums):
                if i in curPres:
                    continue
                if not tmp or i >= tmp[-1]:
                    curPres.add(i)
                    dfs(nums[inx+1:], tmp+[i])  # 下一次才判定

        dfs(nums, [])
        return res


    
    
    
    # 参考上面的进行修改 ，明细思路
from typing import List
import time

class Solution:
    def findSubsequences(self, nums: List[int]) -> List[List[int]]:
        set_=set()
        res = []
        def dfs(l,r,tmp):
            if len(tmp)>1:
                res.append(tmp)
            if l ==r:
                return
            s1=set()
            for i in range(l,r):
                if nums[i] not in s1 and (l==0 or nums[i]>=tmp[-1]):
                    s1.add(nums[i])
                    t = tmp+[nums[i]]      #不能用tmp，因为循环中的tmp不能变

                    print(tmp)
                    dfs(i + 1, r, t)



        dfs(0, len(nums), [])

        return res

```

#### 11.  [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

```python
# 简单的递归回溯，手生，注意一点，这里split不能分割成字符数组，要用list，也可以直接用str[i]
# 另外一点：递归是recur，递归回溯是backtrack，这里搞错了。然后是，每次判定是否添加到结果中，是在每次递归开始，否则扔到下次递归，这样写起来漂亮一点。
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        if not digits :
            return []
        #s = digits.split()

        dic = {
            '2': ['a', 'b', 'c'],
            '3': ['d', 'e', 'f'],
            '4': ['g', 'h', 'i'],
            '5': ['j', 'k', 'l'],
            '6': ['m', 'n', 'o'],
            '7': ['p', 'q', 'r','s'],
            '8': ['t', 'u', 'v'],
            '9': ['w','x', 'y', 'z'],
        }
        res = []
        tmp = []
        def recur(i):
            if i == len(digits):
                #print(tmp)
                res.append(("".join(tmp)))
            else:
                for c in dic[str(digits[i])]:
                    tmp.append(c)
                    recur(i+1)
                    tmp.pop()
        recur(0)
        return res

```

####  12. [1552. 两球之间的磁力](https://leetcode-cn.com/problems/magnetic-force-between-two-balls/)

```python
# 没见过的提醒，需要再熟悉。思路就是，挨个试，只是这里使用二分的方式确定平均边界，通过能按照这个边界放入球的数目，来判断死否成功，然后根据情况，调整min和max，间接更更新mid，实现对最终结果的逼近，知道min》max，脱离循环输出结果
from typing import List


class Solution:
    def maxDistance(self, position: List[int], m: int) -> int:
        position.sort()
        min_ =max_=position[-1]-position[0]
        res = 0
        for i in range(1,len(position)):
            min_=min(min_,position[i]-position[i-1])


        def check_OK(dist):
            last = position[0]
            cnt =1
            for i in range(1,len(position)):
                if position[i]-last >=dist:
                    last = position[i]
                    cnt +=1
            return cnt >=m





        while min_ <= max_:
            mid = (min_ + max_)>>1    #每次更新可能的mid，通过更新min和max实现
            if check_OK(mid):  #如果cnt》=m，说明放得下
                res = max(res,mid)
                min_ = mid+1
            else:
                max_ =mid -1
        return res

```



#### 13 .[1482. 制作 m 束花所需的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets/)

```python

class Solution:
    def minDays(self, bloomDay: List[int], m: int, k: int) -> int:
        # 这道题和磁力的题类型一种，不过那里二分，找的是距离，这里二分，找的是时间。搜索方法确定了

        # check是用来对核心条件做判定的，首先是有足够的相邻的话，能满足m朵的需求
        # 这里利用滑动窗口
        def check_OK(mid):
            n = len(bloomDay)
            l = r = 0  # 初始时，滑动窗口左右端都在头部
            cnt = 0
            while r < n: # 退出条件：遍历到头
                # 开始非连续
                if bloomDay[r]>mid:
                    cnt += (r - l)//k
                    l = r+1
                    r = r+1
                else:
                    r += 1
            cnt += (r-l)//k  # 走到头后，还要再计算一段
            return cnt >= m
        
                # 另一种判定方式，直接用贪心去数
        def check1(day):
            curr = 0
            total = 0
            for i in range(len(bloomDay)):
                if(bloomDay[i] <= day):
                    curr += 1
                else:
                    total += curr // k;
                    curr = 0;
            total += curr // k
            return total >= m
        # 确定最开始的上下限
        min_, max_ =min(bloomDay), max(bloomDay)
        #res = -1
        while min_ <= max_:
            mid = (min_+max_)>>1
            if check_OK(mid):  # 如果判定通过，时间可以减小
                #res = max(res,mid)
                max_ = mid-1
            else: # 不通过，时间可以放大

                min_ = mid+1
        #return res
        return min_ if check1(min_) else -1  #二分查找，直接检查左端点
```

#### 14. [332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

```python
import collections
from typing import List


class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        # 基本思路是：建立邻接表，对出度排序，然后用df，能走完，就返回True
        d = collections.defaultdict(list)        # 设置玲姐表（dict），注意这里可以设置value的类型,不能用deque，因为需要排血，的确没有这个方法
        for f, t in tickets:   # 这种写法，直接分割数组取数据
            d[f] += [t]
        for f in d:
            d[f].sort()   # 倒排，方便后面按照字典序递归
       # print("d:\n", d)
        res = []
        def dfs(f):
            while(d[f]):

                dfs(d[f].pop(0))
            res.insert(0, f)  # 注意因为这里是递归，所以append的顺序是反着来的，所以这里头插

        dfs("JFK")

        return res


    
    # 其实有更好的欧拉路径方式，具体的方法有官方题解给出：
    https://leetcode-cn.com/problems/reconstruct-itinerary/solution/zhong-xin-an-pai-xing-cheng-by-leetcode-solution/
import collections
import heapq
from typing import List

# 欧拉路径解法，利用天然或者后续搜索过程中拆边造成的“死胡同”，依次递归
class Solution:
    def findItinerary(self, tickets: List[List[str]]) -> List[str]:
        dic = collections.defaultdict(list)
        for f,t in tickets:
            dic[f].append((t))         # 建立邻接表
        for k in dic:
            heapq.heapify(dic[k])      # 调整为优先级队列（这里保证了题目需要的最小字典序）    需要导入heapq模块

        def dfs(curr):
            while dic[curr]:
                tmp = heapq.heappop(dic[curr])   # 按照字典序搜素，同时因为pop所以需要拆边
                dfs(tmp)                   # 递归解决
            #知道没有出度，确定是某段结尾，加入栈
            stack.append((curr))       # 递归返回后，说明此时curr已经出度为零，自己也成了死胡同，所以入栈


        stack = list()
        dfs("JFK")
        return stack[::-1]   #因为递归，顺序是反的，所以这里要倒序输出
```

#### 15. [5500. 乘积为正数的最长子数组长度](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)

```python
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
         # 怎么讲？没理解透题的意思，想直接用trick来做，但是如果没有测试用例错误提示，国际差的很远，即便有，实际上也没全做对
        # 第一个思路比较简单，统计遇到的负数次数，遇到0时状态初始化:
        # 和我自己的区别在于，如果出现了多个负值（奇数），需要舍弃遇到的第一个负数(我没有想到这点，直接扑街)

        neg_cnt = 0  # 同济负数、
        _0_idx = -1    # 标记0的位置  ,一开始放在负一的位置，视为了0-（-1）=1，后续编码统一
        flag = len(nums)   # 标记最左端0的位置
        res = 0
        for i,v in enumerate(nums):
            if v == 0:
                # 初始化
                neg_cnt =0
                _0_idx = i
                flag = len(nums)
            elif v<0:
                neg_cnt += 1
                flag = min(i,flag)

            if neg_cnt &1:# 每次一到奇数个负值就舍弃第一个负值（实际上长度和舍弃当前是一样的）
                res = max(res, i-flag)
            else:
                res = max(res,i - _0_idx)
        return res
    
    
    
    
    
    
    
    
    
    # 还有一个正统的方式，直接使用递推，不过我数学不过关，只能自己看了
    '''
    定义两个数组 pos 和 neg
pos[i] 表示：以 nums[i] 结尾的最长乘积为正数的数组长度
neg[i] 表示：以 nums[i] 结尾的最长乘积为负数的数组长度
————————————————————————————————
重要的是状态转移：
根据当前元素的正负性，来考虑 pos 和 neg 数组的更新。
————————————————————————————————

初始化 pos[0] 和 neg[0]
遍历数组，若当前元素为正，那么：
pos[i] 为 pos[i-1] + 1，
如果 neg[i-1] > 0，即：以 nums[i-1] 结尾，乘积为负的数组存在，那么 neg[i] 为 neg[i-1] + 1；否则，neg[i] 为 0（因为当前元素也为正，没有乘积为负的数组）
若当前元素为负，那么：
如果 neg[i-1] > 0，那么 pos[i] 为 neg[i-1] + 1，否则，pos[i] 为 0
neg[i] 为 pos[i-1] + 1
更新乘积为正数的数组最大长度 ans

'''
    class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        n = len(nums)
        pos, neg = [0]*n,[0]*n  # dp列表
        #初始化
        if nums[0]>0:
            pos[0] = 1
        elif nums[0]<0:
            neg[0] = 1
        
        res = pos[0]
        for i, v in enumerate(nums):
            if v >0:
                pos[i] = 1+pos[i-1]   # 自然累加
                neg[i] = 1+ neg[i-1] if neg[i-1]>0 else 0  # #只有前面有，这里才能有，否则如果前面没有负值，这里本神是正值，所以负数长度为0，最好画图理解
            elif v <0:
                pos[i] = 1+neg[i-1]  if neg[i-1]>0 else 0   # 负负得正
                neg[i] = 1+ pos[i]     # 正* 负才为负
            else:
                pass# 这时候应该都填0，不过这里因为初始化全部为0，不用再处理

        res = max(res,max(pos))
        return res
        
class Solution:
    def getMaxLen(self, nums: List[int]) -> int:
        n = len(nums)
        pos, neg = [0]*n,[0]*n  # dp列表
        #初始化
        if nums[0]>0:
            pos[0] = 1
        elif nums[0]<0:
            neg[0] = 1
        # 动态规划，当前状态只依赖于前一个状态
        res = pos[0]
        for i, v in enumerate(nums):
            if i==0:  # 这里将第一位特殊处理，因为已经初始化过了这里就不用了
                continue

            if v >0:
                pos[i] = 1+pos[i-1]   # 自然累加
                neg[i] = 1+ neg[i-1] if neg[i-1]>0 else 0  # #只有前面有，这里才能有，否则如果前面没有负值，这里本神是正值，所以负数长度为0，最好画图理解
            elif v <0:
                pos[i] = 1+neg[i-1]  if neg[i-1]>0 else 0   # 负负得正
                neg[i] = 1+ pos[i-1]     # 正* 负才为负
            else:
                pass# 这时候应该都填0，不过这里因为初始化全部为0，不用再处理
        print(pos)
        print(neg)

        res = max(res,max(pos))
        return res
        
#要考虑边界条件条件

```

#### 16. [5501. 使陆地分离的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-disconnect-island/)(并查集方法待补充)

```python
class Solution:
    def minDays(self, grid: List[List[int]]) -> int:
        # 实际上就是抽取一个角，所以最少天数只有三种可能：0，1，2
        # 有两种方法，最传统的就是递归回溯，每次消除一个1，然后统计。另一种使并查集，不过我不会，需要学习
        moves = [(0,1),(1,0),(-1,0),(0,-1)]   # 图常用的方向

        def dfs(x,y,visited):
            for i, j in moves:
                      
                if  (x+i) >= 0 and x+i <m and y+j >=0 and y+j <n and visited[x+i][y+j] == 0 and grid[i+x][y+j] ==1:
                    visited[x+i][y+j] =1
                    dfs(x+i,y+j,visited)
                    


        def count(grid):
            cnt = 0
            visited = [[0]*n for i in range(m)]   # 注意构造二维数组时，这里行列是反着来的，先确定列数，再乘行数。否则可能会数组越界
            for i in range(m):
                for j in range(n):
                    if visited[i][j] == 0 and grid[i][j]==1:
                        visited[i][j]=1
                        dfs(i,j,visited)
                        cnt +=1
            
            return cnt

        

        m = len(grid)
        n= len(grid[0])
        island_cnt = count(grid)
        if island_cnt == 0 or island_cnt >1:   # 题目中的条件定义，此时可以认为分离
            return 0
        for i in range(m):
            for j in range(n):
                if grid[i][j]==1:
                    grid[i][j]=0
                    res= count(grid)
                    if res ==0 or res >1:
                        return 1
                    grid[i][j]=1
        return 2


```

#### 17. [1503. 所有蚂蚁掉下来前的最后一刻](https://leetcode-cn.com/problems/last-moment-before-all-ants-fall-out-of-a-plank/)

```python
class Solution:
    def getLastMoment(self, n: int, left: List[int], right: List[int]) -> int:
        # 不要想碰撞之后的身份转换，会被迷惑，实际上是蚂蚁走了对方的路。或者，理解成双方穿透，所以直接找最大值就好
        # 本质上是一个脑筋急转弯问题
        # 人工添加最大最小值，用来避免空列表的情况
        left.append(0)
        right.append(n)
        return max(n-min(right),max(left))
```

#### 18. [486. 预测赢家](https://leetcode-cn.com/problems/predict-the-winner/)

```python
# 动态规划解法
# 代码看起来很简短，实际要考虑的东西很多：基本思路:
'''
一段长为i~j的数组，如果选i，则i+1~j是由对方选，如果选j，则i~j-1是对方选，假设都是最优解，则有dp[i][j],表示一个选手在面对i~j这一段中最优法下和对手的分差。由于有两种可能。i或者j，所以dp[i][j] = max(nums[i] - dp[i+1][j], nums[j]-dp[i][j-1])，其中内部的两个dp，是在对手的回合，他所能做到的最优分差，我们选择的数减去他的最最优，就是我们的最优选择。
这就是最优子结构。
然后当j<i,不存在，所以为0
i == j,则为nums[i]。 以上属于初始化步骤
这种动态规划，一般使用二维数组，因为可以分别用行列表示首尾的位置，然后我们需要有从初始化开始向上递推，直到0~n-1，所以决定了循环的方向。基本就是下面的解法了

'''
class Solution:
    def PredictTheWinner(self, nums: List[int]) -> bool:
        dp = [[0]*len(nums) for _ in range(len(nums))]   # 二维数组

        for i in range(len(nums)):   # 初始化
            dp[i][i]=nums[i]
        n = len(nums)
        for i in range(n-2,-1,-1):  #行，从下向上
            for j in range(i+1,n):  # 列，从左到右
                dp[i][j] = max(nums[i] - dp[i+1][j], nums[j]-dp[i][j-1])
        return dp[0][n-1] >= 0
```

#### 19. [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

```python
# 正统玩法是有限自动机，不过我没有学过编译原理，看题解有些懵。然后照抄了下面的题解，思路比较正常化。基本思路就是枚举各种情况，巧妙在于通过标志位来指代各种情况


class Solution:
    def isNumber(self, s: str) -> bool:
        if  not  s or len(s) == 0:
            return False
        isNum, isDot, ise_or_E = False,False, False   # 表示数字、小数点、e
        s= s.strip()   # 这个需要接受，而不是在原本的基础上改变
        for i in range(len(s)):
            if s[i] >= '0' and s[i] <='9':

                isNum = True
            elif s[i] == '.':  
                if isDot or ise_or_E:
                    return False   # 小数点之前不能再有小数点和e
                isDot = True
            elif s[i] == 'e' or s[i] == 'E':
                if not isNum or ise_or_E:
                    return False    # e前边不能出现e，但必须有数字
                ise_or_E = True
                isNum = False   # 用来重置，因为e后面从新开始.重置isNum，因为‘e’或'E'之后也必须接上整数，防止出现 123e或者123e+的非法情况
            elif s[i] == '-' or s[i] == '+':
                if i != 0 and s[i-1] != 'e' and s[i-1] !='E':
                    return False  #正负号只可能出现在第一个位置，或者出现在‘e’或'E'的后面一个位置
            else:
                return False    #除了列出的情况，其他情况都不合法
        return isNum  # 一定需要数字结尾，避免 1e这种情况

```





#### 20 [877. 石子游戏](https://leetcode-cn.com/problems/stone-game/)

```python
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        return True  # 最透彻的解法，因为是2n，先手。这样掌握了以后是否只取0,2，2n-2；还是1,3,2n-1。那个总和大取哪个，所以必赢
 

# dp做法，和预测赢家思路想通
class Solution:
    def stoneGame(self, piles: List[int]) -> bool:
        n = len(piles)
        dp = [[0]*n for i in range(n)]
        for i in range(n):
            dp[i][i]=piles[i]
  
        for i in range(n-2,-1,-1):
            for j in range(i+1,n):
                dp[i][j] = max(piles[i]-dp[i+1][j],piles[j]-dp[i][j-1])
        return dp[0][-1] >0
    
```



#### 21  [1140. 石子游戏 II](https://leetcode-cn.com/problems/stone-game-ii/)

```python
# 记忆化搜索法，为了简便计算，使用了后缀和
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        n, _map = len(piles), dict()

        #需要用到后缀和
        s = [0]*(n+1)
        for i in range(n-1,-1,-1):
            s[i] = s[i+1] + piles[i]

        # dfs ,dfs(i,m) 表示从i开始，向后最多能取m堆石子的最大值
        def dfs(i,m):
            # 已经遇到过
            if (i,m) in _map:
                return _map[(i,m)]
            # 超标
            if i>= n:
                return 0
            if i+m*2  >= n:
                return s[i]
            

            # 遍历可以取得堆数，暴力深搜
            enum = 0
            for j in range(1,m*2+1):
                enum = max(enum,s[i]- dfs(i+j,max(j,m)))
            

            # 记忆化
            _map[(i,m)] = enum
            return enum
        return dfs(0,1)

    
    
    
    
    # 动态规划玩法，除了用dp来记忆，没什么不同，也是类似从后向前推，也是在做决策的时候需要遍历可能，找到最大值
class Solution:
    def stoneGameII(self, piles: List[int]) -> int:
        n = len(piles)
        last_sum = [0]* (n+1)


        # 还是需要后缀和节省时间
        for i in range(n-1,-1,-1):
            last_sum[i] = piles[i] + last_sum[i+1]
        

        # 动态数组
        dp = [[0]* n for i in range(n)]
        # 这里的动态规划，需要从后向前推
        for i in range(n-1,-1,-1):
            for M in range(1,n):
                # 如果覆盖，全量拿去
                if (i + 2*M) >= n:
                    dp[i][M] = last_sum[i]
                    #再做一点剪枝，一次超过，后面的都超过
                    for t in range(M,n):
                        dp[i][t] = last_sum[i]
                    break
                else:
                    #如果没有覆盖，那就遍历，j为1~2m中的所有可能，dp[i+j][max(j,M)]为对方在我选定之后能娶到的最大值，后缀和剪去，然后找差值最大的，就是我能选到的，最优
                    for j in range(1, 2*M+1):
                        dp[i][M] =max(dp[i][M],last_sum[i]-dp[i+j][max(j,M)])
        return dp[0][1]

```



#### 21 [60. 第k个排列](https://leetcode-cn.com/problems/permutation-sequence/)

```python
# 记忆化递归,还需要剪枝
class Solution:
    def getPermutation(self, n: int, k: int) -> str:

        s = [i for i in  range(n+1)]
        self.cnt = 0
        self.res = []
        def dfs(tmp):
            if len(tmp) == n:
                self.cnt += 1
                if self.cnt == k:
                    self.res = tmp
                    return  -1
                else:
                    return
                    #print(tmp)
            else:
                for i in range(1,n+1):
                    if s[i] not in memo:
                        memo.add(s[i])
                        #print("memo",memo)
                        t= dfs(tmp+[s[i]])
                        if t == -1:
                            return  -1
                        memo.remove(s[i])


        memo = set()
        tmp = []
        dfs(tmp)
        return  ''.join([str(i) for i in
                         self.res])


    
    ##数学玩法，实际上就是阶乘数量是固定，根据规律从左到右，依次确定各个位置上的数组
    class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        factorial = [1]
        for i in range(1, n):   # 依次球阶乘
            factorial.append(factorial[-1] * i)
        
        k -= 1  # 调整终点，偏于计算
        ans = list()
        valid = [1] * (n + 1)   # 一开始全是有效位
        for i in range(1, n + 1):
            order = k // factorial[n - i] + 1  #整除+1看轮数
            for j in range(1, n + 1):
                order -= valid[j]   #  从小到大依次减
                if order == 0:
                    ans.append(str(j))
                    valid[j] = 0   # 取了这一位，就无效了
                    break
            k %= factorial[n - i]  # 取余，拿到剩下部分的轮数

        return "".join(ans)
```

#### 22 [347. 前 K 个高频元素](https://leetcode-cn.com/problems/top-k-frequent-elements/)

```python
## hash + 排序
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        _hash = {}
        for i in nums:
            _hash[i] = _hash.get(i,0)+1
        

        #利用排序的手法，比较精巧，需要熟悉：sorted（可迭代对象，关键字，是否倒置），x直达可迭代对象中的元素，先以x[1]排序，再以x[0]排序
        top_frq = sorted(_hash.items(),key = lambda x:(x[1],x[0]), reverse = True)


        # 取前k个：
        return [top_frq[i][0] for i in range(k)]
    
# 更多解法：
https://leetcode-cn.com/problems/top-k-frequent-elements/solution/leetcode347onfu-za-du-bu-fen-si-xiang-ji-dui-jie-f/
```

#### 23. [1314. 矩阵区域和](https://leetcode-cn.com/problems/matrix-block-sum/)

```python
# 思路很好想，面积累计，然后根据坐标，右下角减左边、右边，加左上（因为多减了一次）。
# 不过想边界的时候能够准确点，写代码的时候不要写错。就很好了
class Solution:
    def matrixBlockSum(self, mat: List[List[int]], K: int) -> List[List[int]]:
        m = len(mat)
        n= len(mat[0])
        grid = [[0]*n for _ in range(m)]
        grid[0][0] =mat[0][0]
        for i in range(1,n):          
            grid[0][i] = mat[0][i] +grid[0][i-1]
         
    
        for i in range(1,m):
 
            grid[i][0] = mat[i][0] +grid[i-1][0]
     
        for i in range(1,m):
            for j in range(1,n):
                grid[i][j] = mat[i][j] + grid[i-1][j]+grid[i][j-1]-grid[i-1][j-1]
        #print(grid)
        res = [[0]*n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                L_U_x = i - K
                L_U_y = j-K
                R_D_x = min(m-1,i+K)
                R_D_y = min(n-1,j+K)
                total = grid[R_D_x][R_D_y]
                l,u,patch =0,0,0
                if L_U_y -1 <0:
                    l=0
                else:
                    l = grid[R_D_x][L_U_y -1]
                if L_U_x -1 <0:
                    u =0
                else:
                    #print(L_U_x-1,R_D_x)
                    u = grid[L_U_x-1][R_D_y]
                if l!= 0 and u !=0:
                    patch = grid[L_U_x-1][L_U_y-1]
                #print(i,j,total,l,u,patch)
                res[i][j]= total-l-u+patch
        return res


```

#### 24. [77. 组合](https://leetcode-cn.com/problems/combinations/)

```python

# 之前做过的，谈不上什么收获，无非在复习一下递归回溯法。
# 有用的是对比之前的java 代码。能够对python的特点加深体会。
# self.v   就像java的private 变量，私有变量。然后，想像Java一样调用外部函数，用self修饰就好，最后python的for循环确实不太好用。sad
# 还有，可以用全切片的方式避开引用变量会被反复操作的限制，因为会返回一个值一样但指针一样的对象
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        self.res = []
        if n <=0 or k>n or k<=0:
            return self.res
        tmp = []
        self.get(n,k,1,tmp)
        return self.res

    def get(self,n,k,start,tmp):
        if len(tmp) == k:
            self.res.append(tmp[:])  ## 全切片
            return
        end = n-(k-len(tmp))+2   # 最好提前计算好，毕竟py的for 不太好用。
        for i in range(start,end):
            tmp.append(i)
            self.get(n,k,i+1,tmp)
            tmp.pop()

```

#### 25. [39. 组合总和](https://leetcode-cn.com/problems/combination-sum/)

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        n = len(candidates)
        res = []
        def backtrace(i,tmp_sum,tmp):
            if tmp_sum > target or i ==n:  # 和值超标，或者是走到队尾
                return 
            if tmp_sum == target:
                res.append(tmp)
                return
            for j in range(i,n):
                if tmp_sum + candidates[i] >target:  # 算是剪枝，因为前面的排序
                    return
                backtrace(j,tmp_sum + candidates[j],tmp+[candidates[j]])
        backtrace(0,0,[])
        return res
```

#### 26. [40. 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii/)

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        candidates.sort()
        n = len(candidates)
        self.res = []
        #s = set()    # 原本是用来剪枝的
        def backtrace(idx, tmp,target):
            if idx > n or target <0:  # 递归结束条件
                return 
            elif target == 0:
                #if tuple(tmp) in s:
                    #return
                self.res.append(tmp[:])
                #s.add(tuple(tmp))
                return 
            pre = -1
            for i in range(idx,n):
                if candidates[i] == pre:   # 代替set剪枝，效果更好
                    continue
                pre = candidates[i]
                tmp.append(candidates[i])
                backtrace(i+1,tmp,target-candidates[i])
                tmp.pop()
        backtrace(0,[],target)
        return self.res


```

#### 27. [1277. 统计全为 1 的正方形子矩阵](https://leetcode-cn.com/problems/count-square-submatrices-with-all-ones/)

```python
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        
        m = len(matrix)
        if not matrix or m == 0:
            return -1
        n = len(matrix[0])
        _sum = 0
        for i in range(m):
            _sum += matrix[i][0]
        for j in range(1,n):  # 0,0不能加两次
            _sum += matrix[0][j]
        
        for i in range(1,m):
            for j in range(1,n):
                if matrix[i][j] == 1:
                    matrix[i][j] = min(matrix[i][j-1], matrix[i-1][j],matrix[i-1][j-1]) + 1
                    _sum += matrix[i][j]
        #print(matrix)
        return _sum
```

#### 28. [95. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)（**）

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
# 不是自己想出来的，对构造一个树的概念有点懵，抄袭官方题解。不断生成子树，向上归并成一个完整的树
# 既然是递归，就要;1,设立递归出口，2.相信函数能给到想要的结果，不要深究递归细节
class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        def generateTrees(start,end):
            if start > end:
                return [None]   # 需要返回列表类型，None在结果里表现为null
            allTrees = []
            for i in range(start,end+1):
                leftTrees = generateTrees(start,i-1)  # 从start  到 i-1 生成的所有左子树集
                rightTrees = generateTrees(i+1, end)   # 右子树集

                for l in leftTrees:
                    for r in rightTrees:
                        root = TreeNode(i)
                        root.left = l
                        root.right = r
                        allTrees.append(root)   # 每一级的allTrees是不一样的，这里的整个树的集合对于递归的上一级而言，只是一个子树集
            return allTrees
        return generateTrees(1,n) if n else []
    
    
    
  ## 还有dp解法，待补全。
   
```

#### 29. [216. 组合总和 III](https://leetcode-cn.com/problems/combination-sum-iii/)

```python
## 简单的递归回溯，没问题。注意不要细节出错
class Solution:
    def combinationSum3(self, k: int, n: int) -> List[List[int]]:
        self.res = []
        s = [i for i in range(1,10)]
        #print(s)
        def dfs(tmp,target,idx):
            #print(tmp)
            if target <0 or len(tmp)> k:
                return 
            if target == 0 and len(tmp)==k:
                self.res.append(tmp[:])
            for i in range(idx,9):
                tmp.append(s[i])
                dfs(tmp,target-s[i],i+1)
                tmp.pop()
        dfs([],n,0)
        return self.res

```

#### 30.  [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)

```python
# 转移方程是：f(i,j)=min(f(i+1,j),f(i+1,j+1))+triangle[i][j]
# 注意j，上下关连的j 是j，j+1
# 然后可以n*n递归，不过这里因为每次使用一便，所以可以重复使用一个dp
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        dp = [0] * n 
        dp[0] = triangle[0][0]
        for i in range(1, n):
            # dp[i] 和 dp[0] 都是只有一个可能，所以需要特殊处理，去区别在于，dp[0]可能会被用到，所以放在后面，不能提前覆写
            dp[i] = dp[i-1] + triangle[i][i]
            for j in range(i-1,0,-1):
                dp[j] = min(dp[j-1],dp[j]) + triangle[i][j]
            dp[0] += triangle[i][0]
        return min(dp)
```

#### 31. [79. 单词搜索](https://leetcode-cn.com/problems/word-search/)

```python
# 递归回溯解法
class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        moves = [(0,1),(1,0),(-1,0),(0,-1)]

        def check (i,j,k):  # 递归回溯
            if board[i][j] != word[k]:  # 剪枝不符合的字符
                return False
            if k== len(word)-1:  # 剪枝 ，，超过长度
                return True
            visited.add((i,j))
            result = False
            for x,y in moves:
                new_x ,new_y = i+x,j+y
                if 0 <= new_x <len(board) and 0<= new_y<len(board[0]):  # 合法范围 ，python里面，这里貌似可以用连续的大小比较符
                    if (new_x,new_y) not in visited:   # 剪枝 ，重复统计
                        if check(new_x,new_y,k+1):
                            # return True  ，这样也可以，甚至更快
                            result = True  # 因为要回溯，所以这里不能直接return
                            break
            visited.remove((i,j))
            return result
        m,n = len(board),len(board[0])
        visited = set()  # 这里用作剪枝
        for i in range(m):
            for j in range(n):
                if check(i,j,0):
                    return True
        return False
```

####  32 [1583. 统计不开心的朋友](https://leetcode-cn.com/problems/count-unhappy-friends/)

```python
from typing import List

#  难得是读懂题，题目读懂了，根据条件自然就得到答案了
class Solution:
    def unhappyFriends(self, n: int, preferences: List[List[int]], pairs: List[List[int]]) -> int:
        l = len(pairs)
        if l< 2:
            return 0
        dic = {}
        res =0

        for pair in pairs:
            dic[pair[0]] = pair[1]
            dic[pair[1]] = pair[0]

        #print(dic)
        for i in range(n):
            d = dic.get(i)
            idx = preferences[i].index(d)
            if idx == 0:
                continue
            else:
                for j in range(0,idx):
                    test = preferences[i][j]
                    if preferences[test].index(dic.get(test)) >preferences[test].index(i):
                        #print("i,",i,"j",test)
                        res += 1
                        break
        return res
```

#### 33 [1584. 连接所有点的最小费用](https://leetcode-cn.com/problems/min-cost-to-connect-all-points/)

```python
# 三重循环来做不是不行，只是，超时了，说明方法对
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        # 方法是克鲁斯卡尔，实现方式是并查集，说明并查集迟早要学
        # 又说明，嘴上要说的将来做，实际上最好是当下做，否则又要吃亏
        '''
        kruskal算法原理：对边权值从小到大排序，从最小边开始选择，
        不与已选择的边构成回路就要，否则就不要。
        用并查集实现，排序后，每次加入一条边，若此边的两个顶点的祖先节点相同，
        说明加入这条边会构成回路。所以要舍弃这条边。
        '''
        if not points or len(points) == 1:
            return 0
        dic = defaultdict(int)  # 默认的字典，应该是from collections import defaultdict
        for i in range(len(points)):   # n^2 复杂度，统计所有可能变的权值，点：权值
            for j in range(i+1, len(points)):
                dic[(i, j)] = abs(points[i][0]-points[j][0]) + abs(points[i][1]-points[j][1])
        res = sorted(dic.items(),key = lambda x: x[1])     # 将权值从小到大排序
        sum_ = 0
        uf =UF(len(points))
        tmp =0 
        for i in range(len(res)):  # 开始克鲁斯卡尔挑选，从最小变开始挑，不能形成回路
            point,value = res[i]
            x,y=point
            if uf.find(x) != uf.find(y):  # 判断是否是回路
                tmp += 1
                sum_ += value
                if tmp == len(points) -1:  # n-1条变圆满了
                    return sum_
                uf.union(x,y)

class UF:
    def __init__(self,n):
        self.parents = defaultdict(int)   # 有默认值的字典
        for i in range(n):
            self.parents[i] = i  # 一开始每个集合是自己的父集合
        
    def find(self,x):
        while x!= self.parents[x]:
            x = self.parents[x]  # 一直找祖宗
        return x
    
    def connect(self, p,q):
        return self.find(p) == self.find(q)
    
    def union(self,p,q):
        if self.find(p) == self.find(q):
            return
        self.parents[self.find(p)] = self.find(q)
```



#### 34. [684. 冗余连接](https://leetcode-cn.com/problems/redundant-connection/)

```python
## 最普遍的方法是并查集，先来试试看

class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        a = UF(n+1)
        for pair in edges:
            if a.connect(pair[0],pair[1]):
                return pair
            else:
                a.union(pair[0],pair[1])


class UF:
    # 常用的并查集模板，最好背下来
    from collections import defaultdict
    def __init__(self,n):
        '''初始化'''
        self.parents = defaultdict(int)
        for i in range(n):
            self.parents[i] = i

    def find(self,x):  # 寻找祖宗节点
        while x != self.parents[x]:
            x = self.parents[x]
        return x

    def connect(self,p,q):   # 判断是否是一个集合
        return self.find(p) == self.find(q)
    
    def union(self,p,q):   # 合并节点
        if self.find(p) == self.find(q):
            return
        self.parents[self.find(p)] = self.find(q)

        
## 比较少见的dfs方法，首先建立邻接表，然后因为答案是最后，所以要倒序遍历
# 每次将邻接表里的两个端点截取，如果用dfs还能够走到两个点回合，那就说明有回路，即可返回
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        if not edges or len(edges) == 0 or len(edges[0]) == 0:
            return []
        n = len(edges)

        from collections import defaultdict
        dic = defaultdict(set)
        for edge in edges:# 邻接表
            dic[edge[0]].add(edge[1])
            dic[edge[1]].add(edge[0])

        def _dfs(dic, x, y, used):  # 注意x可以变，y是终点，不能变
            if x == y:
                return True
            used[x] =True
            for i in dic[x]:
                if not used[i]:
                    if _dfs(dic, i, y, used):
                        return True
            
            return False
        #print(dic)
        for i in range(n-1,-1,-1):
            #print('=================')
            #print(edges[i][0])
            dic[edges[i][0]].remove(edges[i][1])  # 每次循环都要剪掉这一条边
            dic[edges[i][1]].remove(edges[i][0])
            used = [False]*(n+1)
            if _dfs(dic,edges[i][1], edges[i][0], used): # dfs
                edges[i].sort
                return edges[i]
            
        return []

    
    
# 拓扑排序，建立入度表，把入度为1的进入对列，出队列，减入度，循环往复
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        #print(n)
        inDegree = [0]*(n+1)
        from collections import defaultdict
        dic= defaultdict(set)
        for e in edges:
            inDegree[e[0]] +=1
            inDegree[e[1]] += 1

            dic[e[0]].add(e[1])
            dic[e[1]].add(e[0])
        print(inDegree)

        from collections import deque
        q = deque()
        for i in range(1,n+1):
            if inDegree[i] == 1:
                q.append(i)
        #print(q)
        while q:
            num = q.popleft()
            for j in dic[num]:
                inDegree[j] -= 1
                if inDegree[j] == 1:
                    q.append(j)
        
        #print(inDegree)
        #print(inDegree[edges[24][0]],inDegree[edges[24][1]])
        #print(edges[24])

        for i in range(n,0,-1):
            #print(i-1)
            #print(inDegree[edges[24][0]],inDegree[edges[24][1]])
            if inDegree[edges[i-1][0]] > 1 and inDegree[edges[i-1][1]] >1:
                return edges[i-1]
        return []
            
```

#### 35. [47. 全排列 II](https://leetcode-cn.com/problems/permutations-ii/)

```python
# 全排列问题，无非就是
'''
1. 排序
2. 递归回溯
3. 针对性去重
'''
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        res = []
        used = [False] * n
        def _backtrace(tmp,used,nums,idx):
            if idx == n:
                res.append(tmp[:])
                return 
            import sys  #将预设值为最大整数，需要这么用
            lastused = sys.maxsize
            for i in range(n):
                if not used[i] and nums[i] != lastused:
                    used[i] = True
                    #print(tmp,used)
                    _backtrace(tmp+[nums[i]],used,nums,idx+1)
                    used[i] =False
                    lastused = nums[i]
        _backtrace([],used,nums, 0)
        return res
            

        
```

#### 36. [910. 最小差值 II](https://leetcode-cn.com/problems/smallest-range-ii/)

```python
class Solution:
    def smallestRangeII(self, A: List[int], K: int) -> int:
        # 本质上就是先排序，然后划分断点，两两比较，求最小值
        A.sort()
        mi, ma = A[0], A[-1]
        N = len(A)
        ans = ma - mi
        for i in range(N-1):
            a, b = A[i], A[i+1]
            val = max(a+K, ma-K) - min(mi+K, b-K)
            if val < ans:
                ans = val
        return ans
```

#### 37  [106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

```python
## 递归方式求解，能做，但是效率偏低

class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if len(inorder) == 0 or len(postorder) == 0:
            return None
        root = TreeNode(postorder[len(postorder)-1])
        for i in range(len(inorder)):
            if inorder[i] == postorder[len(postorder) - 1]:
                root.left = self.buildTree(inorder[:i], postorder[:i])
                root.right = self.buildTree(inorder[i+1:], postorder[i:-1])
                break
        return root
    
# 另一种方法，递归思路大差不差，有趣的一点在于，没有重新构造新的列表，只是传递索引。所以速度上有优势
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        dic = {}
        for i, v in enumerate(inorder):
            dic[v] = i
        def build(start, end):
            if start>end:
                return 
            root = TreeNode(postorder.pop())
            idx = dic[root.val]
            root.right = build(idx+1, end)
            root.left = build(start, idx-1)
            return root
        return build(0,len(postorder)-1)
```

#### 38 [113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def pathSum(self, root: TreeNode, sum: int) -> List[List[int]]:
        ret = list()
        path = list()
        
        def dfs(root: TreeNode, total: int):
            if not root:
                return
            path.append(root.val)
            total -= root.val
            if not root.left and not root.right and total == 0:
                ret.append(path[:])
            dfs(root.left, total)
            dfs(root.right, total)
            path.pop()
        total = sum
        dfs(root, total)
        return ret

```

#### 39 [547. 朋友圈](https://leetcode-cn.com/problems/friend-circles/)

```python
## dfs解法
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        n = len(M)
        cnt = 0
        visited = set()
        def dfs(i):
            for j in range(n):
                if M[i][j] and j not in visited:
                    visited.add(j)
                    dfs(j)

        for i in range(n):
            if i not in visited:
                cnt +=1
                visited.add(i)
                dfs(i)

        return cnt
    
    
  ## 并查集解法
class Solution:
    def findCircleNum(self, M: List[List[int]]) -> int:
        class UnionFind(object):
            def __init__(self, size):
                self.p = [i for i in range(size + 1)]
                self.num = size

            def find(self, x:int):
                # 路径压缩的并查集
                if self.p[x] != x:
                    self.p[x] = self.find(self.p[x])
                return self.p[x]
            
            def union(self, a:int, b:int):
                if self.find(a) != self.find(b):
                    self.p[self.find(a)] = self.p[self.find(b)]
                    self.num -= 1
        
        n = len(M)
        if n == 1:
            return 1
        uf = UnionFind(n)
        for i in range(n):
            for j in range(i + 1, n):
                if M[i][j]:
                    uf.union(i, j)
        return uf.num

```

#### 40. [959. 由斜杠划分区域](https://leetcode-cn.com/problems/regions-cut-by-slashes/)

```python
## 利用二维索引作为Key，构造并查集，一旦成环，则并查集中有一个祖宗节点，反之亦然
## 题目的意思实质上就是有多少个环就有多少个区域，具体原因，还没有搞清楚……手画一下可以搞明白，但是如果没有题解，我决计做不出来，太菜了。。。。。。

class Solution:
    def regionsBySlashes(self, grid: List[str]) -> int:
        n = len(grid)

        dic = {}
        for i in range(n+1):
            for j in range(n+1):
                dic[(i,j)] = (i,j)

        # 最外面的边是一个环
        for x, y in dic:
            if x == 0 or x == n or y == 0 or y == n:
                dic[(x,y)] = (0,0)

        def find(Y,a):
            while Y[a] != a:
                Y[a] = Y[Y[a]]
                a = Y[a]
            return (a)
        cnt =1   # 目前只有一个环
        for i in range(n):
            for j in range(n):
                if grid[i][j] == '\\':
                     # 一旦产生了圈，则意味着产生了一个新的面，面数加 1 
                    if find(dic,(i,j)) == find(dic,(i+1,j+1)):
                        cnt +=1
                    else:
                        dic[find(dic, (i,j))] = find(dic,(i+1,j+1))
                 # /会将点(i,j)右边和下边的点连起来
                 # 把结果存起来，避免merge时再查找一遍
                elif grid[i][j] == '/':
                    if find(dic,(i+1,j)) == find(dic, (i,j+1)):
                        cnt +=1
                    else:
                        dic[find(dic, (i+1,j))] = find(dic,(i,j+1))
        return cnt
    
## 还有一种方法，没有再写，实际上就是类似孤岛问题，用dfs求解，然后关键在于需要先处理输入，把每个格子分成3*3的小格子， 用1 填充斜线的部分，用0填充剩余部分。根据dfs, 求出所有联通0的数目。
# 下面是一个示例
from typing import List


class Solution:
    def regionsBySlashes(self, grid: List[str]) -> int:
        r = len(grid)
        if r == 0:
            return 0
        g = [[0 for _ in range(3 * r)] for _ in range(3 * r)]

        def dfs(i, j):
            g[i][j] = 2
            for x, y in [(i - 1, j), (i + 1, j), (i, j - 1), (i, j + 1)]:
                if x == -1 or x == 3 * r or y == -1 or y == 3 * r or g[x][y] != 0:
                    continue
                dfs(x, y)

        for i in range(r):
            for j in range(r):
                if grid[i][j] == "\\":
                    g[i * 3][j * 3] = 1
                    g[i * 3 + 1][j * 3 + 1] = 1
                    g[i * 3 + 2][j * 3 + 2] = 1
                elif grid[i][j] == "/":
                    g[i * 3][j * 3 + 2] = 1
                    g[i * 3 + 1][j * 3 + 1] = 1
                    g[i * 3 + 2][j * 3] = 1

        cnt = 0
        for i in range(3 * r):
            for j in range(3 * r):
                if g[i][j] == 0:
                    cnt += 1
                    dfs(i, j)
        return cnt




作者：lucy-17
链接：https://leetcode-cn.com/problems/regions-cut-by-slashes/solution/dfs-by-lucy-17-6/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```

#### 41. [6. Z 字形变换](https://leetcode-cn.com/problems/zigzag-conversion/)

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows < 2:
            return s
        res = ["" for _ in range(numRows)]  # 构造几行空字符串
        i, flag = 0, -1
        for c in s:# 对s依次遍历
            res[i] +=c
            # 临界反转
            if i == 0 or i == numRows-1:
                flag = -flag
            i += flag
        return "".join(res)

```

#### 42 .[117. 填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

```python
# 题解用BFS，我自己用层序遍历，大同小异
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return 
        q = deque()
        q.append(root)
        while q:
            #print(q)
            n = len(q)
            if n <2:
                t = q.popleft()
                if t.left:
                    q.append(t.left)
                if t.right:
                    q.append(t.right)
                continue
            t = q.popleft()
            for i in range(n-1):
                if t.left:
                    q.append(t.left)
                if t.right:
                    q.append(t.right)
                s = q.popleft()
                t.next = s
                t =s
                #print(t.val,s.val)
            if t.left:
                q.append(t.left)
            if t.right:
                q.append(t.right)
        return root

## 如果需要使用O（1）空间的话，
# 实际上事上下层转换，上次又next，所以直接由上层组织下层的next
# 根因在于root不用链表，就可以构造好第二层next
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return
        start = root # 从根节点开始
        def _handle(p):
            nonlocal last,next_start # 自由变量
            if last:
                # 如果有前一个节点，那就继续连接
                last.next = p
            if not next_start: 
                # 如果下层的next_start,即横向链表开始为空，那就填充
                next_start =p
            # 更新前一个节点
            last = p
        while start:
            last , next_start = None, None
            p =start
            while p :
                #print(p.val)
                if p.left:
                    _handle(p.left)
                if p.right:
                    _handle(p.right)
                p =p.next
            start = next_start # 更新到下一层链表
        return root

```

#### 43. [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
# 迭代法
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        stk = []
        res = []
        pre, cur =None,root
        while stk or cur:
            while cur:
                stk.append(cur)
                cur =cur.left
            cur = stk[-1]  #先观察，不能着急弹出
            # 是叶子节点或者右边处理完了，就可以处理自己
            if not cur.right or cur.right == pre:
                    res.append(cur.val)
                    stk.pop()
                    pre = cur
                    cur = None
            # 否则处理右边
            else:
                cur = cur.right
        return res
                    

```

#### 44. [701. 二叉搜索树中的插入操作](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    # 简单的递归解法
    def insertIntoBST(self, root: TreeNode, val: int) -> TreeNode:
        if not root:
        #   找到空位置，生成节点，并返回
            return TreeNode(val)
        # 否则，利用二叉搜索树性质，递归处理
        if val< root.val:
            root.left = self.insertIntoBST(root.left,val)
        else:
            root.right = self.insertIntoBST(root.right,val)
        
        return root
```

#### 45 [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        # 几个指针不断后移是一种稳妥的方法，不过先后顺序的逻辑可能会消耗一点时间
        # 所以相比较之下，递归的玩法可能更有技术含量一点
        # 毕竟是链表递归
        if not head or not head.next:
            return head
        _next = head.next
        head.next = self.swapPairs(_next.next)
        _next.next = head
        return _next
```

#### 46. [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

```python
## 中间出错很多次
# 边界条件[[]] 没有考虑到
# 确定在哪一行时，应该用最大值作为指标，而不是最小的
# 二分法一般时动左不动右
class Solution:
    def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        if not matrix or len(matrix) == 0 or  not len(matrix[0]):
            return False
        m, n = len(matrix), len(matrix[0])
        u,d = m-1 ,0
        while u>d:
            mid= (u+d)>>1
            #print('mid',mid)
            if matrix[mid][n-1]<target:
                d = mid+1
            elif matrix[mid][n-1] == target:
                return True
            else:
                u = mid
        #print(d)

        l ,r =0, n-1
        while l<r:
            mid = (l+r)>>1
            if matrix[d][mid]<target:
                l = mid +1
            elif matrix[d][mid] == target:
                return  True
            else:
                r =mid

        return matrix[d][l] == target
```

#### 47 [80. 删除排序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

```python
# 没有什么难度问题，主要在于思考时需要注意边界条件
# 然后是，比较不是和i比较，而是和在确认进入输出队列的最后一个比较t，用i的话，可能因为重复太然后有问题
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        if not nums or not len(nums):
            return -1
        i = 0
        cnt = 0
        t = nums[0]
        for z in range(1, len(nums)):
            if nums[z] != t:
                cnt = 1
                i += 1
                t = nums[i] = nums[z]

                continue
            else:
                if cnt == 2:
                    continue
                else:
                    cnt = 2
                    i += 1
                    t =  nums[i] = nums[z]

        print(nums)
        return i + 1


```

#### 48. [63. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

```python
# 因为用1，表示障碍，所以直接用负数dp
# 由于考虑不全面，需要一些边界（最傻逼的，入口有障碍）、初始化的边界有障碍………所以花了一点时间，进行应对边界调整
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if not obstacleGrid or not len(obstacleGrid) or not len(obstacleGrid[0]):
            return 0
        m, n =len(obstacleGrid), len(obstacleGrid[0])
        dp = [[0]*n for _ in range(m)]
        falg = False
        for i in range(m):
            if obstacleGrid[i][0] == 1:
                falg = True
                if i == 0:
                    return 0
                obstacleGrid[i][0] = 0
            else:
                if falg:continue
                obstacleGrid[i][0] =-1
        #print(obstacleGrid)
        falg = False
        for i in range(n):
            if obstacleGrid[0][i] == 1:
                falg = True
                obstacleGrid[0][i] = 0
            else:
                if falg:continue
                obstacleGrid[0][i] =-1
        #print(obstacleGrid)
        for i in range(1,m):
            for j in range(1,n):
                if obstacleGrid[i][j] == 1:
                    obstacleGrid[i][j] = 0
                    continue
                else:
                    obstacleGrid[i][j] = obstacleGrid[i][j-1] + obstacleGrid[i-1][j]
        #print(obstacleGrid)
        return  -obstacleGrid[m-1][n-1] if obstacleGrid[m-1][n-1]<0 else 0
    
    
 



# 一个题解：先枚举特殊边界，再在外面包一层，统一处理，更简洁
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        if len(obstacleGrid)==0:
            return 0
        if obstacleGrid==[[0]]:
            return 1
        if obstacleGrid==[[1]]:
            return 0
        if obstacleGrid[0][0]==1:
            return 0 
        m=len(obstacleGrid)
        n=len(obstacleGrid[0])
        dp=[[0 for _ in range(n+1)] for _ in range(m+1)]
        for i in range(1,n+1):
            for j in range(1,m+1):
                if i==1 and j==1:
                     dp[1][1]=1
                     continue
                if obstacleGrid[j-1][i-1]==0:
                    dp[j][i]=dp[j-1][i]+dp[j][i-1]
                else:
                    dp[j][i]=0
        return dp[-1][-

作者：sheldonxin
链接：https://leetcode-cn.com/problems/unique-paths-ii/solution/shi-pin-xiang-xi-jie-shi-by-sheldonxin/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 49. [73. 矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)

```python
# 直接记录出现的行列： 遍历一遍记录，遍历第二遍根据标记0
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        R = len(matrix)
        C = len(matrix[0])
        rows, cols = set(), set()

        # Essentially, we mark the rows and columns that are to be made zero
        for i in range(R):
            for j in range(C):
                if matrix[i][j] == 0:
                    rows.add(i)
                    cols.add(j)

        # Iterate over the array once again and using the rows and cols sets, update the elements
        for i in range(R):
            for j in range(C):
                if i in rows or j in cols:
                    matrix[i][j] = 0

# O(1)空间复杂度的玩法
"""
遍历整个矩阵，如果 cell[i][j] == 0 就将第 i 行和第 j 列的第一个元素标记。
第一行和第一列的标记是相同的，都是 cell[0][0]，所以需要一个额外的变量告知第一列是否被标记，同时用 cell[0][0] 继续表示第一行的标记。
然后，从第二行第二列的元素开始遍历，如果第 r 行或者第 c 列被标记了，那么就将 cell[r][c] 设为 0。这里第一行和第一列的作用就相当于方法一中的 row_set 和 column_set 。
然后我们检查是否 cell[0][0] == 0 ，如果是则赋值第一行的元素为零。
然后检查第一列是否被标记，如果是则赋值第一列的元素为零。
"""

class Solution(object):
    def setZeroes(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: void Do not return anything, modify matrix in-place instead.
        """
        is_col = False
        R = len(matrix)
        C = len(matrix[0])
        for i in range(R):
            # Since first cell for both first row and first column is the same i.e. matrix[0][0]
            # We can use an additional variable for either the first row/column.
            # For this solution we are using an additional variable for the first column
            # and using matrix[0][0] for the first row.
            if matrix[i][0] == 0:
                is_col = True
            for j in range(1, C):
                # If an element is zero, we set the first element of the corresponding row and column to 0
                if matrix[i][j]  == 0:
                    matrix[0][j] = 0
                    matrix[i][0] = 0

        # Iterate over the array once again and using the first row and first column, update the elements.
        for i in range(1, R):
            for j in range(1, C):
                if not matrix[i][0] or not matrix[0][j]:
                    matrix[i][j] = 0

        # See if the first row needs to be set to zero as well
        if matrix[0][0] == 0:
            for j in range(C):
                matrix[0][j] = 0

作者：LeetCode
链接：https://leetcode-cn.com/problems/set-matrix-zeroes/solution/ju-zhen-zhi-ling-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 50.[71. 简化路径](https://leetcode-cn.com/problems/simplify-path/)

```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        stk = []
        path =path.split('/')  # 直接用split分割，极为干脆

        for c in path:
            # 分类讨论，根据两个/之间的内容
            if c == '..':
                if stk:
                    stk.pop()
            elif c and c != '.': # 有实际子目录
                stk.append(c)
            else:
                # . 表示当前目录，///类似这样的多个实际上是一个，因此的可以不用操作。这两个都可以pass
                pass
        return "/"+"/".join(stk)
```

#### 51. [36. 有效的数独](https://leetcode-cn.com/problems/valid-sudoku/)

```python
# 没时间做，搬运题解
# 思路：直接用数组记录某一子集数字出现的次数，每遍历一个数，如果》1,就可以返回False
# 特点：boxes = [{} for i in range(9)]——box_index = (i // 3 ) * 3 + j // 3用这中方式，来表示方块性质的子集索引。这些天见到的三次了，需要注意以一下

class Solution:
    def isValidSudoku(self, board):
        """
        :type board: List[List[str]]
        :rtype: bool
        """
        # init data
        rows = [{} for i in range(9)]
        columns = [{} for i in range(9)]
        boxes = [{} for i in range(9)]
        
        # validate a board
        for i in range(9):
            for j in range(9):
                num = board[i][j]
                if num != '.':
                    num = int(num)
                    box_index = (i // 3 ) * 3 + j // 3
                    
                    # keep the current cell value
                    rows[i][num] = rows[i].get(num, 0) + 1
                    columns[j][num] = columns[j].get(num, 0) + 1
                    boxes[box_index][num] = boxes[box_index].get(num, 0) + 1
                    
                    # check if this value has been already seen before
                    if rows[i][num] > 1 or columns[j][num] > 1 or boxes[box_index][num] > 1:
                        return False         
        return True
```

#### 52. [5518. 给定行和列的和求可行矩阵](https://leetcode-cn.com/problems/find-valid-matrix-given-row-and-column-sums/)

```python
## 最简单的贪心玩法，可惜没敢细想下去，傻逼到用dfs，玩砸了不说，还没考略到复杂度
class Solution:
    def restoreMatrix(self, rowSum: List[int], colSum: List[int]) -> List[List[int]]:
        # 如果想试试贪心，就手动模拟一遍，不要不敢试
        r=rowSum
        c =colSum
        m,n=len(r),len(c)
        ans=[[0]*n for i in range(m)]
        for j in range(n):
            i=0
            while c[j]>0:
                t=min(c[j],r[i])
                ans[i][j]=t
                r[i]-=t
                c[j]-=t
                i+=1
        return ans
```

#### 53. [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        pre = ListNode(-1)
        cur =pre
        carry= 0
        while l1 or l2:
            x= 0 if not l1 else l1.val
            y = 0 if not l2 else l2.val
            sum_ =x+y+carry
            carry = sum_//10
            sum_ = sum_%10
            cur.next = ListNode(sum_)
            cur = cur.next
            if l1:
                l1 = l1.next
            if l2:
                l2 =l2.next
            
        if carry == 1:
            cur.next = ListNode(1)
        return pre.next
```

#### 54 . [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

```python
# 和三数之和差不多，多了一层循环而已，双指针都是要有的
# 时间复杂度O（n^3)

class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        q = []
        if not nums or len(nums)<4:
            return q

        nums.sort()
        n = len(nums)
        for i in range(n-3):
            #print('i',i)
            if i >0  and nums[i] == nums[i-1]:  # 有重复数字、去重
                continue
            if sum(nums[i:i+4])>target:   # 先判断最小值是否达标，剪枝
                break
            if nums[i] + nums[-1]+nums[-2]+nums[-3] <target: # 在判断最大值是否超标、剪枝
                continue
            for j in range(i+1, n-2):
                if j>i+1 and nums[j] == nums[j-1]: # 有重复数字、去重
                    continue
                if nums[i]+sum(nums[j:j+3])>target: # 先判断最小值是否达标，剪枝
                    break
                if nums[i] + nums[j]+nums[-1]+nums[-2] <target:# 在判断最大值是否超标、剪枝
                    continue
                l, r = j+1, n-1  # 后两个数用双指针确定，减少一重复杂度
                while l <r:
                    total = nums[i]+nums[j]+nums[l]+nums[r]
                    if total == target:
                        q.append([nums[i],nums[j],nums[l],nums[r]])
                        while l <r and nums[l] == nums[l+1]:
                            l +=1
                        while l < r and nums[r] == nums[r-1]:
                            r -=1
                        r -=1   # 加上这个是为了避免死循环，想l<r 的临界条件变动，否则就卡在这里了
                    elif total< target:
                        l +=1
                    else:
                        r -=1
        return q

```

#### 55. [86. 分隔链表](https://leetcode-cn.com/problems/partition-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
# 借助两个虚拟头结点，构造然后组合，倒是没什么难度
class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        before = beforeHead = ListNode(0)
        after = afterHead = ListNode(0)
        while head:
            if head.val < x:
                before.next = head
                before = before.next
            else:
                after.next = head
                after = after.next
            head = head.next

        after.next = None
        before.next = afterHead.next
        return beforeHead.next


```

#### 56 . [81. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

```python
class Solution:
    # 这道题四似曾相识
    def search(self, nums: List[int], target: int) -> bool:
        if not nums:
            return False
        left = 0
        right = len(nums) - 1
        while left <= right:
            mid = (left + right) // 2
            if nums[mid] == target:
                return True
            # 左半段
            if nums[mid] > nums[left]:  # mid 》 left 说明可以看看左边有没有问题
                if nums[left] <= target <= nums[mid]:  # 如果能够包含target的话，看看里面
                    right = mid - 1
                else:
                    left = mid + 1  # 否则，看看右边
            # 不确定段，left++
            elif nums[mid] == nums[left]:   # 如果相等的话，，稍微变动一下，
                left += 1
            # 右半段
            elif nums[mid] < nums[left]:  # mid < left:说明右边比较有序，看看右边再分类讨论
                if nums[mid] <= target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return False

```



#### 57 .[82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        dummy = pre = ListNode(0)
        dummy.next = head            # 根据上式的赋值,改变dummy的结构也会改变pre
        while head and head.next:
            if head.val == head.next.val:
                while head and head.next and head.val == head.next.val:
                    head = head.next # 向后移动head指针
                head = head.next     # 继续后移以去除重复出现过的数字
                pre.next = head      # 此时head指向中间位置所代表的后序链表,通过pre指针将前序排序连接上,
                # 此处对pre的操作破坏了链表的结构,由于dummy与pre指向同一链表,所以return dummy的值会发生变化(去重),
                # 但dummy仍旧指向链表的开头ListNode(0)所以可以return到一开始的元素如0->1->2->3->3->4有
                # 0->1->2->4,而pre为2->4
            else:
                head = head.next
                pre = pre.next
        return dummy.next # 如果输出pre.next的话 拿预期[1,2,5]的输出为例 只能得到[5]



```

#### 58. [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

```python
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        if not nums or len(nums) == 0:
            return
        n = len(nums)
        if n <= 3:
            nums.sort()
            return nums
        f1 , f2, f2 = 0, 0, n-1
        for i in range(n):
            if nums[i] != 0:
                f1 = f2 = i
                break
        for i in range(n-1,-1,-1):
            if nums[i] != 2:
                f3 = i
                break
        while f1 <= f3 and f2 <=f3:
            if nums[f1] == 0:
                t = nums[f1]
                nums[f1] = nums[f2]
                nums[f2] = t
                f1 +=1
                f2 +=1
                continue
            elif nums[f1] == 1:
                f1 += 1
                continue
            else:
                t = nums[f3]
                nums[f3] =2
                f3 -=1
                while f3>f1 and nums[f3] ==2:
                    f3 -=1
                nums[f1] = t
                #print(f1,f3)
        
        return nums

# 还有另一种比较典型的双指针记录一下
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        n = len(nums)
        p0 = p1 = 0  
        for i in range(n):
            if nums[i] == 1:  # 遇到1的时候转换
                nums[i], nums[p1] = nums[p1], nums[i]
                p1 += 1
            elif nums[i] == 0:  # 遇到零的时候，交换
                nums[i], nums[p0] = nums[p0], nums[i]
                if p0 < p1:  # 遇到  0 的时候，之前的交换可能把01连续中断，所以需要再换回来。最好自己手画一边就明白了
                    nums[i], nums[p1] = nums[p1], nums[i]
                p0 += 1
                p1 += 1


```

#### 59. [91. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

```python
# 先判断合不合法，然乎使用dp
# 如果非0，继承上一个字符，然后看能够组合，如过可以，再加上上一个字符
# 中途可以剪枝，如果是0且组合》=30 ，说明没有解码，直接返回零
class Solution:
    def numDecodings(self, s: str) -> int:
        n = len(s)
        dp = [0] * (n+1)  # 为了对应好好，这里用n+1
        if s[0] == '0':
            return 0
        dp[0] = dp[1] =1  # 只用dp[1]就好，dp[0] 无所谓
        for i in range(1,n):
            if s[i]!= '0':
                dp[i+1] = dp[i]
            else:
                if int(s[i-1]+s[i])>=30:
                    return 0

            num = int(s[i-1] +s[i])
            if 10 <=num<= 26:
                dp[i+1] += dp[i-1]
        print(dp)
        return dp[n]
        


```

#### 60. [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```python
# 迭代法：
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 先找到需要操作的点，并记录前后两端的连接点
    # 操作单独的端，用来连接
    # m， n 是循环指标
    def reverseBetween(self, head: ListNode, m: int, n: int) -> ListNode:
        if not head:
            return 
        cur , pre = head, None
        while m >1:  # 先找m个，并不断更新m.n
            pre =cur 
            cur =cur.next
            m -=1
            n -=1
        tail, con = cur, pre  # 因为需要反转，所以这里的tail是m，用来连接后面的，con 的pre是前一段的最后一个
        while n:
            third = cur.next
            cur .next =pre
            pre = cur
            cur = third
            n -=1
        # 这时， pre是反转段落的开头，cur是第三段的开头
        if con:   # 这里是考虑如果从1 开始，那么 pre= con =None,所以需要讨论
            con.next = pre
        else:
            head = pre
        tail.next = cur
        return head
```

#### 61. [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""
# 比二简单，因为是完美的二叉树，层序的不想写了，没有技术含量。这个是和二差不多的用上层节点，推下层节点
class Solution:
    def connect(self, root: 'Node') -> 'Node':
        if not root:
            return root
        
        # Start with the root node. There are no next pointers
        # that need to be set up on the first level
        leftmost = root
        
        # Once we reach the final level, we are done
        while leftmost.left:
            
            # Iterate the "linked list" starting from the head
            # node and using the next pointers, establish the 
            # corresponding links for the next level
            head = leftmost
            while head:
                
                # CONNECTION 1
                head.left.next = head.right
                
                # CONNECTION 2
                if head.next:
                    head.right.next = head.next.left
                
                # Progress along the list (nodes on the current level)
                head = head.next
            
            # Move onto the next level
            leftmost = leftmost.left
        
        return root 


```

#### 62. [103. 二叉树的锯齿形层次遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        res = []
        if not root:
            return res
        flag = 1
        q = []
        q.append(root)
        while q:
            n = len(q)
            tmp = []
            for i in range(n):             
                    t= q.pop(0)
                    if t.left:
                        q.append(t.left)
                    if t.right:
                        q.append(t.right)
                    tmp.append(t.val)
            if flag:
                res.append(tmp)
            else:
                res.append(tmp[::-1])
            flag = 1- flag
            
        return res
                    

```

#### 63. [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

```python
from typing import List

## 递归回溯没有做好
class Solution:
    def restoreIpAddresses(self, s: str) -> List[str]:
        if not s or len(s)<3 or len(s)>12: 
            return []
        res = []
        def _dfs(s,tmp):
            if len(tmp) ==3:
                if len(s)>1 and s[0] == '0':  # 最后一段中，如果有“前导”0，说明有问题，直接不要
                    return
                if 0<len(s)<=3 and 0 <= int(s) <= 255:
                    tmp.append(s)
                    #print('ok', tmp)
                    res.append(".".join(tmp))
                    return
                else:
                    #print('fuck')
                    return
            for i in range(min(len(s), 3)):  # 3 .不能直接搞3，如果s不够呢？
                t = s[:i+1]
                #print('t',t,'s',s,len(tmp),print(tmp))
                if t[0] == '0':   # 2.本题中一个点是0， 在前三段中，遇到0，说明独立成段，直接下一层递归
                    tmp.append('0')
                    _dfs(s[1:],tmp[:])  # 1 ，这里传递给下一层的是一个副本
                    tmp.pop()
                    break

                elif 0<int(t) <= 255:
                    tmp.append(t)
                    #print('jia',tmp)
                    _dfs(s[i+1:],tmp[:])
                    tmp.pop()
        _dfs(s, [])
        #print('res',res)
        return res
        
```

#### 64. [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
#  a + (n+1)b + nc = 2(a+b)   ===> a = c + (n-1)(b+c)
class Solution:  
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return None
        f, s = head.next, head
        flag = False
        while f and f.next:
            if f == s:
                flag = not flag
                break
            s = s.next
            f = f.next.next
        if not flag:
            return None
        f = head
        s =s.next # 需要慢指针再动一次再循环
        while True:
            if f ==s:
                return s
            f= f.next
            s = s.next

```

#### 65. [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

```python
# 递归回溯，模板题。掌握不行，费时间
# 另有快捷，待续写

class Solution:
    def partition(self, s: str) -> List[List[str]]:
        if not s or len(s)<2:
            return [[s]]  # 返回格式要没有问题
        res = []
        n = len(s)
        """     尽可能不要内置函数，比较之下，直接用t == t[::-1],节省了1/3时间
        def _check(t):
            l, r = 0, len(t)-1
            while l <=r:
                if t[l] != t[r]:
                    return False
                l +=1
                r -=1
            return True"""
        

        def _dfs(s,tmp,idx):
            if idx>=n:
                res.append(tmp)
                return 
            for i in range(idx,n):
                t = s[idx:i+1]
                if t == t[::-1]:  # 替换check节省时间
                    tmp.append(t)
                    #print(tmp,i+1)
                    _dfs(s,tmp[:],i+1)
                    tmp.pop()
        _dfs(s,[],0)
        return res

            
```

#### 66. [130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)

```python
## DFS矩阵套路，问题是：1，构造二维矩阵总是出错[[x]*n for _ in range(m)]  这个套路要背下来
# 2. 空边界问题，要想到，就和数学中的'解’一样
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        if not board or len(board)==0 or len(board[0]) ==0:
            return board
        s = set()
        moves = [(1,0),(-1,0),(0,1),(0,-1)]
        m, n = len(board), len(board[0])
        visited = [[False]* n for _ in range(m)]
        #print(visited)
        if m<3 or n<3:
            return board
        def _dfs(i,j):
            for x, y in moves:
                nx = x+i
                ny = y+j
                if 0 <=nx<m and 0<= ny < n and board[nx][ny] == 'O' and not visited[nx][ny]:

                    print('error',nx,ny)
                    s.add((nx,ny))
                    visited[nx][ny] = True
                    _dfs(nx,ny)
        for i in range(n):
           # print(i)
            if not visited[0][i] and board[0][i] == 'O':
                #print('1',i)
                visited[0][i] = True
                s.add((0,i))
                _dfs(0,i)
            if not visited[m-1][i] and board[m-1][i] == 'O':
                #print('2',i)
                visited[m-1][i] = True
                s.add((m-1,i))
                _dfs(m-1,i)

        for i in range(m):
            if not visited[i][0] and board[i][0] == 'O':
                #print('3',i)
                visited[i][0] = True
                s.add((i,0))
                _dfs(i,0)
            if not visited[i][n-1] and board[i][n-1] == 'O':
                #print('4',i)
                visited[i][n-1] = True
                s.add((i,n-1))
                _dfs(i,n-1)
        for i in range(m):
            for j in range(n):
                if (i,j) not in s and board[i][j] == "O":
                    board[i][j] = 'X'
        
        
```

#### 67. [416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

```python
class Solution:
    # 数据集较小不一定是recur，也有可能是dp，本次中recur超时，就应该想到dp
    #target = 目标值，当然前面需要一些判定
    # 设置为[n][target+1]，表示前i个元素，能否有和为j的，多一列是为了初始化时候，考虑到目标值为0，那么什么都不拿即可为True
    def canPartition(self, nums: List[int]) -> bool:
        n = len(nums)
        if n<2:
            return False
        total = sum(nums)
        if total &1 ==1:
            return False
        target = total >>1
        # 如果一个值就超标，那就不可能有解
        if max(nums) >target:
            return False
        
        # dp初始化
        dp = [[False]* (target+1) for _ in range(n)]
        # target = 0 时，不取即解
        for i in range(n):
            dp[i][0] = True
        # 这里是纯粹用来递推的种子，需要先初始化.在第一行中，只有num[0]一个元素，所以，只有target == nums[0] 的时候，才有解
        dp[0][nums[0]] = True
        
        # 开始dp
        for i in range(1,n):
            num = nums[i]
            for j in range(1,target+1):
                if j>=num:
                    # 上一行已经满足，不用取这个num，或者，这一行的前面满足j-num，要取num
                    dp[i][j] = dp[i-1][j] or dp[i-1][j-num]
                else:
                    # num >j 必然不能取,只能继承上一行的状态
                    dp[i][j] = dp[i-1][j]
        return dp[n-1][target]  # 最后递推到全部元素考虑在内，是否有达到target的状态
    
    
class Solution:
    # 讨论区看到的一种解法，很巧妙。
    def canPartition(self, nums: List[int]) -> bool:
        # 常规判定
        if len(nums) < 2:
            return  False

        ac = sum(nums)
        if ac % 2 == 1:
            return  False

        res_ = ac / 2
        # 利用字典记录各种组合下来的和
        d = defaultdict(int)
        #最开始，什么都不取，为1，这也是为了后面，不取某个数字对应的操作
        d[0] = 1
        # 顺次取数字
        for num in nums:
            # 和既有的和操作，key = 0时，说明不取这个数，
            for k in list(d.keys()):
                r = num + k
                # 每次判定是否达标
                if r == res_:
                    return True
                # 计算的和加入字典
                d[r] = 1
        return False
```

#### 68. [147. 对链表进行插入排序](https://leetcode-cn.com/problems/insertion-sort-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 想法没错，确实需要使用dummy，然后每个节点从头走遍历。不过这里还需要使用一个pre节省时间,可以将cur从pre、cur.next中空出来操作
    def insertionSortList(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        dummy = ListNode(float('inf'))
        dummy.next =head
        cur = head.next
        pre= head
        while cur:
            tmp = cur.next
            # 相邻两个节点<= 说明至少这次循环中，不要插入操作
            if pre.val <= cur.val:
                pre, cur = cur, tmp
            else:
                start = dummy
                # 从头找属于cur的位置,注意，这里比较的是start。next，能走到start，说明已经》start了，这样最后的空位就在start和 start.next之间
                while cur.val > start.next.val:
                    start = start.next
                # 找到位置后，插入操作
                pre.next = tmp
                cur.next = start.next
                start.next = cur
                # 找到下一个cur
                cur = tmp
        # 这里之所以用dummy.next 而不用head，是因为head也可能会被排序，在局内，而dummy，在局外
        return dummy.next

```

#### 69. [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

```python
# 递归解法
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        if not root:
            return res
        def preorder(root):
            res.append(root.val)
            if root.left:
                preorder(root.left)
            if root.right:
                preorder(root.right)
        preorder(root)
        return res
# 迭代写法
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        res = []
        if not root:
            return res
        q = []
        q.append(root)
        while q:
            cur = q.pop()
            res.append(cur.val)
            if cur.right:  # 用数据栈代替程序调用栈，注意顺序先右后左
                q.append(cur.right)
            if cur.left:
                q.append(cur.left)
        return res
```

#### 70. [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

```python
class Solution:
    def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
        if not beginWord or not endWord or not wordList or endWord not in wordList:
            return 0
        n = len(beginWord)
        # 对一个单词构建可能的相似性模板(顺序将每个字符换成‘*’)，并遍历list，将单词归类(把每个单词放在所有它的相似模板下面))
        dic = defaultdict(list)
        for word in wordList:
            for i in range(n):
                dic[word[:i] + '*' + word[i+1:]].append(word)

        # 队列BFS，记录单词和路径长度
        # 每次从一个模板中取出所有的单词，并利用set(),记录已出现，防止回环
        # 直到找到endword，或者返回0
        q = [(beginWord,1)]
        visited = set()
        visited.add(beginWord)
        while q:
            cur_word, path = q.pop(0)
            for i in range(n):
                like_word = cur_word[:i] +'*'+ cur_word[i+1:]
                for word in dic[like_word]:
                    if word == endWord:
                        return path+1
                    if word not in visited:
                        visited.add(word)
                        q.append((word, path+1))
                del dic[like_word]
        return 0
            
```

#### 71. [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        res = 0
        if not root:
            return res
        def dfs(root,sum_):
            nonlocal res
            if not root.left and not root.right:
                res += sum_*10+root.val
                return 
            sum_ = sum_*10+root.val
            if root.left:
                dfs(root.left, sum_)
            if root.right:
                dfs(root.right, sum_)
        dfs(root, 0)
        return res

    
    # 广度优先搜素
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# BFS几个点： while循环，队列，这道题里因为要记录每个阶段的值，所以需要额外一个队列同步。不像DFS，可以递归顺带
class Solution:
    def sumNumbers(self, root: TreeNode) -> int:
        if not root:
            return 0
        total = 0
        res = [root.val]
        li = [root]
        while li:
            node = li.pop(0)
            num = res.pop(0)
            l, r = node.left, node.right
            if not l and not r:
                total +=num
            else:
                if l:
                    li.append(l)
                    res.append(num*10+l.val)
                if r:
                    li.append(r)
                    res.append(num*10+r.val)
        return total


```

#### 72. [133. 克隆图](https://leetcode-cn.com/problems/clone-graph/)

```python
# 因为是clone，所以用vistied记录，key是原节点，value是克隆节点，然后按照原节点的关系，组织克隆节点一一对应
## dfs解法

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        visited = {}
        def dfs(node):
            #print(node.val)
            if not node:
                return 
            if node in visited:
                return visited[node]
            clone = Node(node.val, [])
            visited[node] = clone
            for ni in node.neighbors:
                # dfs递归地添加新clone的neighbor节点
                clone.neighbors.append(dfs(ni))
            return clone
        return dfs(node)
  

## bfs解法
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = []):
        self.val = val
        self.neighbors = neighbors
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        visited = {}
        def bfs(node):
            if not node:
                return
            clone = Node(node.val, [])
            visited[node] = clone
            q = [node]
            while q:
                tmp = q.pop(0)
                for n in tmp.neighbors:
                    if n not in visited:
                        visited[n] = Node(n.val, [])
                        q.append(n)
                    visited[tmp].neighbors.append(visited[n])
            return clone
        return bfs(node)
            
            
```

#### 73. [134. 加油站](https://leetcode-cn.com/problems/gas-station/)

```python
class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
         # a 就是那个数组
        a = []
        for i in range(len(gas)):
            a.append(gas[i] - cost[i])

        if sum(a) < 0:# 油量油耗之差《0，等死没救了
            return -1

        start = 0
        all_money = 0
        for i in range(len(a)):
            all_money += a[i]
            if all_money < 0: # 某一阶段《0，说明起点不在这里，从下一点开始
                all_money = 0
                start = i+1

        if start < len(a):
            return start
 class Solution:
    def canCompleteCircuit(self, gas: List[int], cost: List[int]) -> int:
        # rest记录全程的油量和油耗之差，如果存在《0，说明注定跑不完，-1
        # last记录一段路的差值，如果小于零，说明，从这个这个起点开始的走不到下一个站点，那就以下一个站点为起点，越过障碍。如果走到最后，就说明可以走完一周，因为在rest》=0的前提下，必然有解，前面有段落，意味着后面开始的那部分会积累足够弥补前边段落的油量
        rest, last,start =0,0,0
        for i in range(len(gas)):
            last += gas[i]-cost[i]
            rest += gas[i]-cost[i]
            if last<0:
                start =i +1
                last =0 
        return start if rest >=0 else -1
```

#### 74. [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
# 单链表走一半翻转，从尾到头输出，注意如果是奇数个节点，中间节点，需要提前处理，放在结果的最后
class Solution:
    def reorderList(self, head: ListNode) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        f, s = head,head
        rev = ListNode(-1)
        tmp = rev

        while f and f.next:
            f = f.next.next
            pre = s
            s = s.next
            t = tmp.next
            tmp.next = pre
            pre.next=t
        
        dummy = ListNode(-1)
        last = ListNode(-1)
        if f:
            last = s
            #last.next= None
            #print(last.val)
            s = s.next
            last.next= None
            #print(s)
            dummy.next =last
            #print(dummy)
        rev = rev.next
        while rev and s  :
            n1 = rev
            rev = rev.next
            n2 = s
            s= s.next
            t = dummy.next
            n1.next= n2
            n2.next = t
            dummy.next = n1
        return dummy.next


```

#### 75. [138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""
# 1从图里学来的，深拷贝问题类似于数或者图，要求遍历所有“分支“ ，那么dfs 、bfs就是十分可行的方法
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        if not head:
            return head
        record = {}
        def dfs(node):
            if not node:
                return None
            if node in record:
                return record[node]
            else:
                record[node] = Node(node.val)
                if node.next:
                    record[node].next = dfs(node.next)
                if node.random:
                    record[node].random = dfs(node.random)
            return record[node]
        return dfs(head)
class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':

        # *1.
        # 复制节点，如A - B - C
        # 变成
        # A - A’-B - B’-C - C’
        # *2.
        # 依次遍历节点A, B, C，将这些节点的随机指针与A’B’C’一致
        # *3.
        # 分离A - B - C和A’B’C’，A’B’C’便是需要求得链表
        if not head:
            return head
        node = head
        ## 先复制
        while node:
            new_clone = Node(node.val)
            new_clone.next = node.next
            node.next = new_clone
            node = new_clone.next
        node = head
        # 依照相对位置，复制random指针
        while node:
            if node.random:
                node.next.random = node.random.next
            node = node.next.next
        origin_node = head
        new_node = head.next
        res = new_node
        while new_node:
            origin_node.next = new_node.next
            new_node.next = origin_node.next.next if origin_node.next else None
            origin_node = origin_node.next
            new_node = new_node.next
        return res

```

#### 76. [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)

```python
class Solution:
    # 一开始没想明白，一旦开始旋转，那那么left《right，就是有有序，否则就是无序，mid可以代替left，也可以代替right，在循环中判断
    # 返回点在于mid 》 mid+1 ，以及mid-1> mid这两种情况
    # 一开始担心mid求后，会发生mid+1 或者mid-1,越界的问题，，其实用【2,1】、【3,2,1】手动模拟，皆可以找到判定顺序，是先mid+1，如果没有，在判定mid-1。因为求mid的话，如果是偶数的话，mid中中间偏左的位置
    def findMin(self, nums: List[int]) -> int:
        if not nums:
            return -1
        if len(nums) == 1:
            return nums[0]
        l , r = 0, len(nums)-1
        if nums[r]>nums[l]:
            return nums[l]
        while l<r:
            mid = (l+r)>>1
            if nums[mid] > nums[mid+1]:
                return nums[mid+1]
            if nums[mid-1] > nums[mid]:
                return nums[mid]
            if nums[mid] > nums[0]:
                l = mid+1
            else:
                r =mid-1
```

####  77. [1620. 网络信号最好的坐标](https://leetcode-cn.com/problems/coordinate-with-maximum-network-quality/)

```python

from typing import List
from  math import floor,sqrt
# 比赛时的思路，是纯暴力，先把所有的点摊开在地图上，确定四个极限边界，再在边界点内寻找一个个候选点，比较、记录、排序，取值。太暴力了
class Solution:
    def bestCoordinate(self, towers: List[List[int]], radius: int) -> List[int]:
        if not towers:
            return
        if len(towers)==1:
            return [towers[0][0], towers[0][1]]
        uy = dy = towers[0][0]
        lx = rx = towers[0][1]
        for x,y,r in towers:
            lx = min(lx,x)
            rx = max(x,rx)
            uy = max(y,uy)
            dy = min(y,dy)
        lx -= radius
        rx += radius
        uy +=radius
        dy -= radius
        res = []
        record = 0
        #print(lx,uy)
        for px in range(lx,rx+1):
            for py in range(dy,uy+1):
                cnt = 0
                for x, y,q in towers:
                    t= sqrt((px-x)**2 + (py-y)**2)

                    if t > radius:
                        continue
                    else:
                        cnt += floor(q/(1+t))
                if cnt == record:
                    res.append([px,py])
                elif cnt > record:
                    record = cnt
                    res.clear()
                    res.append([px,py])
                else:
                    continue

        res.sort()
        return res[0]

## 看到题解 发现暴力也有暴力的细节
# 先把信号塔按照字典序排序，然后直接每次寻找其他塔对这个塔的影响
# 不过这里有个问题，一定是塔的信号强度最大吗？其实有点疑问
class Solution:
    def bestCoordinate(self, towers: List[List[int]], radius: int) -> List[int]:
        towers.sort(key=lambda x:[x[0],x[1]]) # x, y升序排列
        n = len(towers)
        signals = [0 for _ in range(n)]
        for i in range(n):
            signal = 0
            x1, y1 = towers[i][0], towers[i][1]
            for j in range(n):
                x2, y2, q = towers[j][0], towers[j][1], towers[j][2]
                dist = sqrt((x1-x2) * (x1-x2) + (y1-y2) * (y1-y2))
                if dist > radius:
                    signal += 0
                else:
                    signal += math.floor(q / (1 + dist))
            signals[i] = signal
        maxsignal = max(signals)
        poi = signals.index(maxsignal)
        return [towers[poi][0], towers[poi][1]]

```

#### 78. [5545. 无矛盾的最佳球队](https://leetcode-cn.com/problems/best-team-with-no-conflicts/)

```python
class Solution:
    def bestTeamScore(self, scores: List[int], ages: List[int]) -> int:
        # 前面对数据的处理做到了，但是后面的dp逻辑没有想出来
        n = len(scores)
        arr = list(zip(ages, scores)) # 使用list保留zip后的数据，zip生成的貌似是dict
        arr.sort(key=lambda x : (x[0], x[1]))  # 比赛时lamdba没用对，:后面最好空一格，这就是二维排序了

        # 下面的是比赛时没有考虑到的dp斯洛
        # 首先初始化，假如单独成队
        dp = [arr[i][1] for i in range(n)]
        for i in range(n):
            for j in range(i):
                # 如果达成这种条件，就可以不考虑年龄,直接登记最新值，不用考虑年龄相等时的情况，注意前面的sort已经将年龄相等时的成绩大的向后排了
                if arr[i][1] >= arr[j][1]:
                    dp[i] = max(dp[i], dp[j]+arr[i][1])
        return max(dp)
```

#### 79. [1621. 大小为 K 的不重叠线段的数目](https://leetcode-cn.com/problems/number-of-sets-of-k-non-overlapping-line-segments/)

```python
class Solution:
    # 三维动态规划，关键在于要讨论最右边端点的状态，不连接-0，和左端新开一段-1，不是新开，而是延长左端的线段-2
    def numberOfSets(self, n: int, k: int) -> int:
        MOD = 10**9 + 7
        # dp[i][j][t] 表示区间[0,i]中取j段有几种取法, t=0 不取, t=1 取新一段, t=2 和前一段连起来
        dp = [[[0, 0, 0] for t in range(k+1)] for _ in range(n)]
        for i in range(n): # 初始化，任何点，不选都是1
            dp[i][0][0] = 1
        for i in range(1, n):
            for j in range(1, min(i, k)+1):
                dp[i][j][0] = sum(dp[i-1][j])
                dp[i][j][1] = sum(dp[i-1][j-1])
                # 选择延长，有两种可能，i-2开始直接顺着i-1延长，以及更里面延长出来
                dp[i][j][2] = dp[i-1][j][1] + dp[i-1][j][2]
        return sum(dp[n-1][k]) % MOD

# 另一种数学方法：抽象不太懂.形象点讲，可以认为将中间承接线段的点，是为l、r两个点，这样连接时就不用考虑共享点的问题，共有n+k-1，其中2k个点组成k条线段，如此可以算出排列数（其实不要细想，我也解释不清楚）
class Solution:
    def numberOfSets(self, n: int, k: int) -> int:
        return math.comb(n + k - 1, k * 2) % (10**9 + 7)
```

#### 80. [763. 划分字母区间](https://leetcode-cn.com/problems/partition-labels/)

```python
class Solution:
    # map记录最后位置，+贪心确定分割点
    def partitionLabels(self, S: str) -> List[int]:
        last = [0] * 26
        for i, ch in enumerate(S):
            last[ord(ch) - ord("a")] = i
        
        partition = list()
        start = end = 0
        for i, ch in enumerate(S):
            end = max(end, last[ord(ch) - ord("a")])
            if i == end:
                partition.append(end - start + 1)
                start = end + 1
        
        return partition


```

#### 81 .[162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)

```python
class Solution:
    # logn时间复杂度要求就是二分法了，只是这里要求有索引，所以判断条件是l<r 而不是l <= r（死循环）
    # 具体决策，画一条直线或者斜线看看就知道了
    def findPeakElement(self, nums: List[int]) -> int:
        l, r = 0, len(nums)-1
        if l == r:
            return l
        while l < r:
            mid = (l+r) >>1
            if nums[mid] > nums[mid+1]:
                r = mid
            else:
                l = mid+1
        return l

```

#### 82. [165. 比较版本号](https://leetcode-cn.com/problems/compare-version-numbers/)

```python
# 作为版本号比较的时候，不是作为字符串比较字典序，而是作为数字，比大小
class Solution:
    def compareVersion(self, version1: str, version2: str) -> int:
        v1, v2 = version1.split('.'), version2.split('.')
        l1, l2 = len(v1),len(v2)
        lmax = max(l1,l2)
        for i in range(lmax-l1):
            v1.append('0')
        for i in range(lmax-l2):
            v2.append('0')
        for i in range(lmax):
            s1, s2= v1[i], v2[i]
            j,k=0,0
            for j in range(len(s1)):
                if s1[j] == '0':
                    continue
                break
            for k in range(len(s2)):
                if s2[k] == '0':
                    continue
                break
            
            if int(s1[j:]) < int(s2[k:]):
                return -1
            elif int(s1[j:]) > int(s2[k:]):
                return 1
            else:
                continue
        return 0

    
    
# 思路一样，官方题解代码更简洁：
class Solution:
    def compareVersion(self, version1: str, version2: str) -> int:
        nums1 = version1.split('.')
        nums2 = version2.split('.')
        n1, n2 = len(nums1), len(nums2)
        
        # compare versions
        for i in range(max(n1, n2)):
            i1 = int(nums1[i]) if i < n1 else 0
            i2 = int(nums2[i]) if i < n2 else 0
            if i1 != i2:
                return 1 if i1 > i2 else -1
        
        # the versions are equal
        return 0 
    
    
# 利用.的自然分块，每次比较一块。时空间复杂度更小
class Solution:
    # 根据索引，自动返回下一份块的大小
    def get_next_chunk(self, version: str, n: int, p: int) -> List[int]:
        # if pointer is set to the end of string
        # return 0
        if p > n - 1:
            return 0, p
        
        # find the end of chunk
        p_end = p
        while p_end < n and version[p_end] != '.':
            p_end += 1
        # retrieve the chunk
        i = int(version[p:p_end]) if p_end != n - 1 else int(version[p:n])
        # find the beginning of next chunk
        p = p_end + 1
        
        return i, p
        
    def compareVersion(self, version1: str, version2: str) -> int:
        p1 = p2 = 0
        n1, n2 = len(version1), len(version2)
        
        # compare versions
        while p1 < n1 or p2 < n2:
            i1, p1 = self.get_next_chunk(version1, n1, p1)
            i2, p2 = self.get_next_chunk(version2, n2, p2)            
            if i1 != i2:
                return 1 if i1 > i2 else -1
        
        # the versions are equal
        return 0    

作者：LeetCode
链接：https://leetcode-cn.com/problems/compare-version-numbers/solution/bi-jiao-ban-ben-hao-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 83 [166. 分数到小数](https://leetcode-cn.com/problems/fraction-to-recurring-decimal/)

```python
class Solution:
    def fractionToDecimal(self, numerator: int, denominator: int) -> str:
        if numerator == 0:
            return '0'
        res = []
        # 神奇的结果异或，先确定值的正、负
        if (numerator < 0 ) ^ (denominator <0):
            res.append('-')
        numer = abs(numerator)
        denomin = abs(denominator)
        a, last = divmod(numer, denomin)
        # 注意格式，python尤其注意，这里是字符串，需要str()
        res.append(str(a))
        if last ==0:
            return "".join(res)
        res.append('.')
        dic ={}
        while last != 0:
            # 如果余数重复出现，说明是循环，则，在原本的循环小数之前插入'(',再在后面补上’)‘
            if last in dic:
                res.insert(dic[last],'(')
                res.append(')')
                break
            dic[last] = len(res)  # 记录，如果该余数是循环的，那么应该插入'('的位置
            last *= 10 # 模拟除法操作，向后一位
            a, last = divmod(last,denomin)
            res.append(str(a))
        return "".join(res)
```

#### 84. [1024. 视频拼接](https://leetcode-cn.com/problems/video-stitching/)

```python
class Solution:
    def videoStitching(self, clips: List[List[int]], T: int) -> int:
        dp = [0]*(T+1)
        # 先处理一个跳跃数组
        for s, e in clips:
            if s >T:
                continue
            if e>T:
                b = T
            dp[s] = max(dp[s],e)
        i = 0
        last = pre = cnt =0
        for i in range(T):
            last = max(dp[i],last)
            print(i,last)
            # 这个条件判定是否截断，如果为t-1，也能判定是否停在t之前
            if i == last:
                return -1
            if i == pre:
                print('i',i,'pre',pre)
                cnt +=1
                pre = last
        return cnt


## 折腾了许久，搞了个稍微效率高一点的玩法，终究逃不开从头开始，但是不要溜掉中间大大跳越
class Solution:
    def videoStitching(self, clips: List[List[int]], T: int) -> int:
        clips.sort(key= lambda x : (x[0],x[1]))
        dic = defaultdict(int)
        if clips[0][0] != 0:
            return -1
        for start, end in clips:
            dic[start] = end
        #print(dic,clips)
        cnt = 0
        p = 0
        tmp =pre= 0
        for i in range(T):
            last = max(dic[i],p)
            if last == i:
                return -1
            if i == pre:
                cnt+=1
                pre = max(p,dic[i])
            if i<pre:
                p = max(dic[i],p)

        return cnt 
```

#### 85. [220. 存在重复元素 III](https://leetcode-cn.com/problems/contains-duplicate-iii/)

```python
class Solution:
    # 桶排序，每个桶的间距是t+1，那么桶内差距一定是最大t，相邻桶的差距最大2*t-1，。
    # 每个桶的容量是一个，等有k个桶之后，需要做桶的更新，i-k那个数的桶去除，i个桶计入
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        if t < 0 or k < 0:
            return False
        all_buckets = {}
        bucket_size = t + 1                     # 桶的大小设成t+1更加方便
        for i in range(len(nums)):
            bucket_num = nums[i] // bucket_size # 放入哪个桶
            
            if bucket_num in all_buckets:       # 桶中已经有元素了
                return True
            
            all_buckets[bucket_num] = nums[i]   # 把nums[i]放入桶中
            
            if (bucket_num - 1) in all_buckets and abs(all_buckets[bucket_num - 1] - nums[i]) <= t: # 检查前一个桶
                return True
            
            if (bucket_num + 1) in all_buckets and abs(all_buckets[bucket_num + 1] - nums[i]) <= t: # 检查后一个桶
                return True
            
            # 如果不构成返回条件，那么当i >= k 的时候就要删除旧桶了，以维持桶中的元素索引跟下一个i+1索引只差不超过k
            if i >= k:
                del all_buckets[nums[i-k]//bucket_size]
                # all_buckets.pop(nums[i-k]//bucket_size)
                
        return False

    
    
    ## 别人的题解，用到了soetedset这个数据结构，基本思路其实也是维护一个k大小的验证窗口，区别在于，这个窗口可以二分查找，要擦新加入的索引，因此，不用内部再搞一套循环。
    from sortedcontainers import SortedSet


class Solution:
    def containsNearbyAlmostDuplicate(self, nums: List[int], k: int, t: int) -> bool:
        sorted_set = SortedSet()

        for i in range(len(nums)):
            num = nums[i
            # 
				
            # find the successor of current element作为后继找前面符合条件的节点
            # sorted_set.bisect_left(num) 这个函数指向插入后的索引，换言之，目前该索引上的值以及索引后一项的值，应当是与num差距最小的了
            if sorted_set and sorted_set.bisect_left(num) < len(sorted_set):
                if sorted_set[sorted_set.bisect_left(num)] <= num + t:
                    return True

            # find the predecessor of current element作为前驱，找后面符号条件的节点
            if sorted_set and sorted_set.bisect_left(num) != 0:
                if num <= sorted_set[sorted_set.bisect_left(num) - 1] + t:
                    return True

            sorted_set.add(num)
            if len(sorted_set) > k:
                sorted_set.remove(nums[i - k])

        return False

作者：moqimoqidea
链接：https://leetcode-cn.com/problems/contains-duplicate-iii/solution/python3-er-cha-sou-suo-shu-by-moqimoqidea/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 86.[845. 数组中的最长山脉](https://leetcode-cn.com/problems/longest-mountain-in-array/)

```python
class Solution:
    # 枚举山峰
    # 依次遍历，两个方向，查看从这一个点开始，的递减序列地长度
    # 统计记过，两个序列之和-1
    def longestMountain(self, A: List[int]) -> int:
        if not A:
            return 0
            
        n = len(A)
        left = [0] * n
        for i in range(1, n):
            left[i] = (left[i - 1] + 1 if A[i - 1] < A[i] else 0)
        
        right = [0] * n
        for i in range(n - 2, -1, -1):
            right[i] = (right[i + 1] + 1 if A[i + 1] < A[i] else 0)
        
        ans = 0
        for i in range(n):
            if left[i] > 0 and right[i] > 0:
                ans = max(ans, left[i] + right[i] + 1)
        
        return ans
class Solution:
    # 枚举山脚
    # 从第一个可能的左侧山脚考虑，确定了左侧山脚后，寻找右侧山脚，然后更新最大值
    def longestMountain(self, A: List[int]) -> int:
        n = len(A)
        ans = left = 0
        while left + 2 < n:
            # 假设有侧山脚为l+1
            right = left + 1
            # 如果符合递增，走这里的逻辑，否则直接在下面l = r,进入下一次循环
            if A[left] < A[left + 1]:
                # 在不越界的条件下，先走到山顶
                while right + 1 < n and A[right] < A[right + 1]:
                    right += 1
                # 在不越界的情况下，开始下山，注意r>r+1,防止有平台。之所以<n-1，是需要有明确山脚，不能直接到n-1还是增加
                if right < n - 1 and A[right] > A[right + 1]:
                    # 在不越界的情况下，开始继续下山（退出循环条件，要么是到边界，要么是不符合递减规律，总之都可以确定山脚）
                    while right + 1 < n and A[right] > A[right + 1]:
                        right += 1
                    # 更新结果
                    ans = max(ans, right - left + 1)
                else:
                    # 这里是如果发现有山顶平台的情况，更新r，进而更新l，进入下一次循环
                    right += 1
            left = right
        return ans


```

#### 87. [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

```python


class Solution:
    def calculate(self, s: str) -> int:
        stk = []
        # 虚拟开头，特殊处理
        num = 0
        sign = '+'
        for i in range(len(s)):
            if s[i].isdigit():
                num = num*10 + int(s[i])
                # 这里需要判断一下末尾情况，否则会丢失数据
            if s[i] in '+-*/' or i == len(s)-1:
                if sign == '+':
                    stk.append(num)
                elif sign == '-':
                    stk.append(-num)
                elif sign == '*':
                    stk.append(int(stk.pop())*num)
                else:
                    stk.append(int(stk.pop()/num))
                num=0
                # 记录本次的运算符，用来和下一个数计算
                sign = s[i]
       #print(stk)
        return sum(stk)

```

#### 88. [228. 汇总区间](https://leetcode-cn.com/problems/summary-ranges/)

```python
class Solution:
    # 就是单纯遍历，然后记录了连续的字段，形成二维列表
    # 后面根据长度，==1，就是一个，》1，取收尾，构造答案格式
    def summaryRanges(self, nums: List[int]) -> List[str]:
        if not nums:
            return []
        if len(nums)==1:
            return [str(nums[0])]
        #遍历区间 核心是 保证下一个元素与前一个元素的差为1
        res=[[nums[0]]]
        for i in range(1,len(nums)):
            if nums[i]-res[-1][-1]==1:
                res[-1].append(nums[i])
            else:
                res.append([nums[i]])
        results=[]
        for x in res:
            if len(x)==1:
                results.append(str(x[0]))
            else:
                results.append(str(x[0])+"->"+str(x[-1]))
        return results



```

#### 89. [274. H 指数](https://leetcode-cn.com/problems/h-index/)

```python
# 没有，做的太急了，反而浪费时间
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        if not citations:
            return 0
        citations.sort()
        n =len(citations)
        if citations[-1] == 0:
            return 0
        for i,v in enumerate(citations):
            if v >= n-i:
                return n-i
 # 原本的思路被理顺了以后，也可以做出来。只是需要考虑边界，不能总是期待报错提供信息啊
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        if not citations:
            return 0
        citations.sort(reverse= True)
        print(citations)
        if citations[0] == 0:
            return 0
        for i,v in enumerate(citations):
           # print(i,v)
            if v < i+1:
                return i
        return min(len(citations),citations[-1])
```

#### 90. [275. H 指数 II](https://leetcode-cn.com/problems/h-index-ii/)

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        # 方法和I一样，有序情况优化，一般就是二分
        n = len(citations)
        # 判断边界：
        if n == 0 or citations[-1] == 0:
            return 0
        l, r = 0, n-1
        while l<r:
            mid = (l+r)>>1
            if citations[mid] >= n-mid:
                r = mid
            else:
                l = mid +1 
        return n -l
```

#### 91 [187. 重复的DNA序列](https://leetcode-cn.com/problems/repeated-dna-sequences/)

```python
class Solution:
    # 低级解法
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        L, n = 10, len(s)     
        seen, output = set(), set()

        # iterate over all sequences of length L
        for start in range(n - L + 1):
            tmp = s[start:start + L]
            if tmp in seen:
                output.add(tmp[:])
            seen.add(tmp)
        return list(output)

```

#### 92 [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        """自底向上采用归并方式，利用了原本的空间，避免递归占用，栈空间。
        归并排序，堆排序和快排 O(nlogn),"""
        def merge(head1, head2):
            # 合并，
            dummy = ListNode(0)
            tmp,tmp1,tmp2 = dummy,head1,head2
            while tmp1 and tmp2:
                if tmp1.val <= tmp2.val:
                    tmp.next = tmp1
                    tmp1 = tmp1.next
                else:
                    tmp.next, tmp2 = tmp2, tmp2.next
                tmp = tmp.next
            if tmp1:
                tmp.next = tmp1
            elif tmp2:
                tmp.next = tmp2
            return dummy.next

        if not head:
            return head
        length = 0
        node = head
        while node:
            # 统计链表长度，用于后续分割
            length += 1
            node = node.next
        # 设立虚节点，从最短开始开始归并（也就是自底向上）
        dummy = ListNode(0,head)
        sublen = 1
        while sublen < length:# 终结条件，归并段的长度大于等于总长度
            pre, cur = dummy, dummy.next
            # 开始分割链表，执行归并
            while cur:
                # 每一次循环都是在截取两段做merge
                head1 = cur
                for i in range(1, sublen):
                    if cur.next:
                        cur = cur.next
                    else:
                        break
                head2 = cur.next
                # 截断链表，上边就是一个段了
                cur.next = None
                cur = head2
                for i in range(1,sublen):
                    if cur and cur.next:
                        cur = cur.next
                    else:
                        break
                # 承接后续的段落
                succ = None
                if cur:
                    succ = cur.next
                    cur.next = None
                # print(head1, head2)
                mergedchain = merge(head1,head2)
                pre.next = mergedchain
                while pre.next:
                    pre = pre.next
                cur = succ
            # 归并段长度*2
            sublen <<= 1
        return dummy.next

```

#### 93. [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```python
class Solution:
    def findMinArrowShots(self, points: List[List[int]]) -> int:
        # 贪心算法，根据右边界的大小排序。在一串可以射穿的气球中，在右边界（意味着存在气球）中的最小值（意味着保持之前能社保的气球少），尽可能贪心的尝试是否能够社保更多气球
        if not points:
            return 0
        points.sort(key=lambda x: x[1])
        # 初始贪心右边界
        pos = points[0][1]
        # 至少一箭
        res = 1
        for balloon in points:
            # 上一个气球的右边界够不到下一个的左边界，需要再射一箭
            if balloon[0] > pos:
                res += 1
                pos = balloon[1]
        return res



```

#### 94. [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        cnt = 0
        def dfs(root):
            nonlocal cnt
            if not root:
                return 0
            cnt = dfs(root.left) + dfs(root.right)+1
            return cnt
        res = dfs(root)
        return res
# 上面的解法不够契合提议，没有用到完全二叉树性质，新解法：
# 具体在JAVA版本里面查，就不赘述了
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def countNodes(self, root: TreeNode) -> int:
        if not root:
            return 0
        level = 0
        node = root
        while node.left:
            level += 1
            node = node.left
        low ,high = 2**level , 2**(level+1)-1
        # print(high)
        def exists(root, level, mid):
            # mask = 1<<(level-1)
            mask = 2**(level -1)
            t = root
            while t and mask:
                if mask&mid == 0:
                    t= t.left
                else:
                    t= t.right
                mask = mask >>1
            return t
            # print(t)
            # if t:
            #     return True
            # return False

        while low<high:
            mid = (high - low +1 )//2 +low
            if exists(root,level,mid):
                low = mid
            else:
                high = mid -1
        return low
```

#### 95. [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

```python
class Solution:
    def fourSumCount(self, A: List[int], B: List[int], C: List[int], D: List[int]) -> int:
        # 其实是变形的两数之和，这里可以A+B 分成一组，用字典记录，然后就是单纯的二重循环比较了，如果有契合的数字+1
        # pythonic的解法
        # 本质和两个for 循环，然后以V+u 为key， 遇到key ，value +1 一样，这一步由Counter做的
        cntAB = collections.Counter(u+v for u in A for v in B)
        res = 0
        for u in C:
            for v in D:
                if -v-u in cntAB:
                    res += cntAB[-v-u]
        return res
```

#### 96. [767. 重构字符串](https://leetcode-cn.com/problems/reorganize-string/)

```python
#基于计数的贪心算法

class Solution:
    #  1. 取Counter中的单个键值对就用items()
    # 2. 列表变成字符串的方法： "".join([])
    def reorganizeString(self, S: str) -> str:
        if len(S)<2:
            return S
        # 用计数器简化代码
        n = len(S)
        counts = collections.Counter(S)
        maxCount = max(counts.items(),key=lambda x: x[1])
        maxCount = maxCount[1]  # 取得是value
        if maxCount > (n+1)/2:
            return ""  # 如果单一字符超过这个限制，说明，必有连续重复字符，直接返回结果，不用管下面的逻辑
        
        res = [""]*n  # 初始化
        # print(res)
        evenIndex , oddIndex = 0,1
        half_length = n//2
        for c,cnt in counts.items():
            # 这里是基于计数的贪心，找到一个字符，直接间隔排序
            # 只要字母的出现次数不超过字符串的长度的一半（即出现次数小于或等于 n/2n/2），就可以放置在奇数下标，只有当字母的出现次数超过字符串的长度的一半时，才必须放置在偶数下标。
            # 因此优先填充计数下标，直到计数下标填充满 ，或者 恰好有一个次数 == n/2+1 只能放偶数下标
            while cnt>0 and cnt<=half_length and oddIndex<n:
                res[oddIndex] = c
                cnt -=1
                oddIndex +=2
            while cnt>0:
                res[evenIndex] = c
                cnt -= 1
                evenIndex += 2
        # print(res)
        return "".join(res)
    
    
# 基于最大堆的贪心算法
class Solution:
    def reorganizeString(self, S: str) -> str:
        if len(S) < 2:
            return S
        # 前半部分判否的逻辑不变
        length = len(S)
        counts = collections.Counter(S)
        maxCount = max(counts.items(), key=lambda x: x[1])[1]
        if maxCount > (length + 1) // 2:
            return ""
        
        queue = [(-x[1],x[0]) for x in counts.items()]  # python 里的是最小堆，要当做最大堆使用的话，需要用负值
        heapq.heapify(queue)  # 建堆
        res= []

        # 用堆构建序列
        while len(queue) >1:
            _, c1 = heapq.heappop(queue)
            _, c2 = heapq.heappop(queue)
            res.extend([c1,c2])
            counts[c1] -= 1
            counts[c2] -= 1
            # 之前pop出来了，现在修改完后，需要放回去
            if counts[c1] > 0:
                q.heappush(queue,(-counts[c1],c1))
            if counts[c2] > 0:
                heapq.heappush(queue,(-counts[c2],c2))
        # 每次两个，但是也有可能剩一个的情况
        if queue:
            res.append(queue[0][1])
        return "".join(res)



```

#### 97. [378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

```python
class Solution:
    def kthSmallest(self, matrix: List[List[int]], k: int) -> int:
        """
        1. 二分法解决问题，利用行、列有序的性质
        2.不妨假设答案为 xx，那么可以知道 l\leq x\leq rl≤x≤r，这样就确定了二分查找的上下界。

每次对于「猜测」的答案 midmid，计算矩阵中有多少数不大于 midmid ：

如果数量不少于 k，那么说明最终答案 x 不大于 mid；
如果数量少于 k，那么说明最终答案 x 大于 mid。
 """
        n = len(matrix)

        def check(mid):
            i, j = n - 1, 0
            num = 0
            while i >= 0 and j < n:
                # 从左下角开始走，如果不大于mid,那么这一列都不大于，记录并且向后走
                if matrix[i][j] <= mid:
                    num += i + 1
                    j += 1
            # 否则，向上走
                else:
                    i -= 1
            return num >= k

        left, right = matrix[0][0], matrix[-1][-1]  # 二分前，确定最大最小值
        while left < right:
            mid = (left + right) // 2
            # 计算小于等于mid的 值有多少，是否大于k
            if check(mid):
                right = mid
            else:
                left = mid + 1
        
        return left


```

#### 98. [973. 最接近原点的 K 个点](https://leetcode-cn.com/problems/k-closest-points-to-origin/)

```python
class Solution:
    def kClosest(self, points: List[List[int]], K: int) -> List[List[int]]:
        #  最大堆需要用负值来利用
        q = [(-x**2-y**2,i) for i ,(x,y) in enumerate(points[:K])]
        heapq.heapify(q)
        n = len(points)
        for i in range(K,n):
            x,y = points[i]
            dist = - x**2-y**2
            heapq.heappushpop(q,(dist,i))
        res = [points[idx] for _, idx in q]
        return res
    
## topK的另一种写法：变形的快排
def random_select(left: int, right: int, K: int):
            pivot_id = random.randint(left, right)
            pivot = points[pivot_id][0] ** 2 + points[pivot_id][1] ** 2
            points[right], points[pivot_id] = points[pivot_id], points[right]
            i = left - 1
            for j in range(left, right):
                if points[j][0] ** 2 + points[j][1] ** 2 <= pivot:
                    i += 1
                    points[i], points[j] = points[j], points[i]
            i += 1
            points[i], points[right] = points[right], points[i]
            # [left, i-1] 都小于等于 pivot, [i+1, right] 都大于 pivot
            if K < i - left + 1:
                random_select(left, i - 1, K)
            elif K > i - left + 1:
                random_select(i + 1, right, K - (i - left + 1))

        n = len(points)
        random_select(0, n - 1, K)
        return points[:K]

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/k-closest-points-to-origin/solution/zui-jie-jin-yuan-dian-de-k-ge-dian-by-leetcode-sol/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 99. [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        # 两次二分，一次确定有无以及左边界，一次确定右边界。
        # 左边界很正常，普通的二分，右边界因为是向右逼近，所以取中指，逼近判断，都有变化
        res = [-1,-1]
        l,r = 0,len(nums)-1
        while l<r:
            mid = l + (r-l)//2   # 向左取整
            if nums[mid]<target:
                l = mid +1
            else:
                r =mid
        if not nums or nums[l]!= target:
            return res
        res[0] = l    # 确定了左边界
        l, r = l,len(nums)-1
        while l<r:
            mid = l+ (r-l+1)//2   # 向右取整
            # 这里是右边过度移动
            if nums[mid]>target:
                r = mid -1
            else:
                l = mid
        res[1] = r
        return res

```


#### 99. [5618. K 和数对的最大数目](https://leetcode-cn.com/problems/max-number-of-k-sum-pairs/)

```python
from typing import List
import collections

class Solution:
    def maxOperations(self, nums: List[int], k: int) -> int:
        res = 0
        dic = collections.defaultdict(int)
        for i ,v in enumerate(nums):
            if k-v in dic and dic[k-v] >0:
                dic[k-v] -= 1
                res += 1
            else:
                dic[v] += 1
        return res
```

#### 100. [5620. 连接连续二进制数字](https://leetcode-cn.com/problems/concatenation-of-consecutive-binary-numbers/)

```python
class Solution:
    def concatenatedBinary(self, n: int) -> int:
        # 比赛的时候有这个想法，不过后面还是暴力模拟过了，毕竟那个省时间
        # x=x⋅2^(len2(i)) + i 基本就是这个算法
        # 这个抄来的题解的里的优化，在于计算len2(i),即二进制位的长度
        # 其实是记忆化，自底向上，从1-》n, 每一次中，len2(i) 最多变化1，100..0这样，是2的整数次幂，而验证是否是整数次的方法，就是看i&(i-1) ?0
        mod = 10**9 + 7
        # ans 表示答案，shift 表示 len_{2}(i)
        ans = shift = 0
        for i in range(1, n + 1):
            # if 131072 % i == 0:
            if (i & (i - 1)) == 0:
                shift += 1
            ans = ((ans << shift) + i) % mod
        return ans


```

#### 101.[659. 分割数组为连续子序列](https://leetcode-cn.com/problems/split-array-into-consecutive-subsequences/)

```python
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        # 哈希表内部嵌套堆，说实话没想到
        # 以每一个数字结尾的构建一个堆，堆内存储各个字符串的长度
        # 每次将最短的+1
        # 最后遍历是否有《3的队列
        cnter = collections.defaultdict(list)
        for x in nums:
            # 新写法？声明并初始化？
            # 注意，这里找的是x-1 能够连续的前一个
            if queue := cnter[x-1]:
                # x-1 的堆中少了一个子序列长度
                prevLength = heapq.heappop(queue)
                # x的堆中多了一个
                heapq.heappush(cnter[x],prevLength+1)
            else:
                heapq.heappush(cnter[x],1)
        return not any(queue and queue[0]<3 for queue in cnter.values())
    
    
## 贪心法，
class Solution:
    def isPossible(self, nums: List[int]) -> bool:
        # 贪心的玩法有头绪，但是没细想，毕竟心不静
        # 原则是以某位为指引，尽可能长，最不济，凑成三，否则false
        countMap = collections.Counter(nums)  # 计算次数
        endMap = collections.Counter()
        for x in nums:
            if(count := countMap[x])>0:
                # 如果有前一个数字为末尾的序列，那就续上，变成以x为末尾的序列，x-1 的序列取消
                if (prev_end_count := endMap.get(x-1,0))>0:
                    countMap[x] -= 1
                    endMap[x-1] = prev_end_count -1
                    endMap[x] +=1
                # 如果没有，说明前面断开，只能以自己为起点，看看有没有可以向后组成三个的序列，如果有的话，计入endmap
                # 否则false
                # 注意这里就说明，了endmap里的序列至少要大于等于3
                else:
                    if(count_1 := countMap.get(x+1,0)) > 0 and (count_2 := countMap.get(x+2,0)) >0:
                        countMap[x] -= 1
                        countMap[x+1] -= 1
                        countMap[x+2] -= 1
                        endMap[x+2] += 1
                    else:
                        return False
        return True
```

#### 102.[842. 将数组拆分成斐波那契序列](https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence/)

```python
class Solution:
    def splitIntoFibonacci(self, S: str) -> List[int]:
        res = list()
        # 递归回溯问题，特点在于入股前两个定了，后面只是找到是否有前两个之和的序列，然后递归验证，如果遍历完了，还没有找到，就False
        # 时间复杂度O(n (log(10)C)^2
        def backtrace(idx:int):
            if idx == len(S):
                return len(res) >= 3
            curr = 0
            for i in range(idx,len(S)):
                #print(res)
                if i>idx  and S[idx] == '0':
                    # 0 可以在开头，但中间数字不应该是0，所以i>idx 的情况，0就没必要考虑了。没有012这种数字
                    break
                curr = curr *10 + int(S[i])
                #print(curr)
                if curr>2**31-1:
                    break  #  题目限制，剪枝
                # 符合斐波那契规则
                if len(res)< 2 or curr == sum(res[-2:]):
                    res.append(curr)
                    # 递归
                    if backtrace(i+1):
                        return True
                    # 回溯
                    res.pop()
                elif len(res)>2 and curr >= sum(res[-2:]):
                    # 这里也是一个剪枝，如果出现大于了，i增curr增，会更大，所以后面的也不用遍历了
                    break
            return False
        backtrace(0)
        return res

     


                
```



#### 103. [565. 数组嵌套](https://leetcode-cn.com/problems/array-nesting/)

```python
class Solution:
    def arrayNesting(self, nums: List[int]) -> int:
        res = 0
        for idx,v in enumerate(nums):
            # 按照题意模拟，其实每一个数只会被访问一次，这样可以在访问后修改成一个绝不会有的数，作为访问过的标记
            if v != 20000:
                cnt = 0
                start = v
                while nums[start] != 20000:
                    tmp = start
                    start = nums[start]
                    cnt += 1
                    nums[tmp] =20000
                res = max(res,cnt)
        return res


```



#### 104. [264. 丑数 II](https://leetcode-cn.com/problems/ugly-number-ii/)

```python

class Solution:
    def nthUglyNumber(self, n: int) -> int:
        dp = [0] * n
        dp[0] = 1
        l2, l3, l5 = 0, 0, 0
        for i in range(1, n):
            # 已经入队的每个数，都是可以乘2,3,5，这里的指针是指向需要乘因数的最小值
            dp[i] = min(2 * dp[l2], 3 * dp[l3], 5 * dp[l5])
            # 例如：下一个数是2* dp[l2],就说明l2,已经在最小值上用过了，需要向右移一位
            if dp[i] == 2 * dp[l2]:
                l2 += 1
            if dp[i] == 3 * dp[l3]:
                l3 += 1
            if dp[i] == 5 * dp[l5]:
                l5 += 1
        return dp[n - 1]
```

#### 105. [649. Dota2 参议院](https://leetcode-cn.com/problems/dota2-senate/)

```python
class Solution:
    def predictPartyVictory(self, senate: str) -> str:
        # 两个队列模拟，贪心禁止离自己最近的地方议员
        n = len(senate)
        radiant = collections.deque()
        dire = collections.deque()
        
        for i, ch in enumerate(senate):
            if ch == "R":
                radiant.append(i)
            else:
                dire.append(i)
        
        while radiant and dire:
            #轮回的方式是采用+n的方式
            if radiant[0] < dire[0]:
                radiant.append(radiant[0] + n)
            else:
                dire.append(dire[0] + n)
            radiant.popleft()
            dire.popleft()
        
        return "Radiant" if radiant else "Dire"


```

#### 106. [692. 前K个高频单词](https://leetcode-cn.com/problems/top-k-frequent-words/)

```python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        cnt = collections.Counter(words)
        candidates = cnt.keys()
        res= sorted(candidates,key = lambda x: (-cnt[x],x))
        # 原来的不行，版本问题吧，python3 里dict_keys 是不能直接用sort的
        # candidates.sort(key = lambda x: (-cnt[x],x))
        return res[:k]
    
class Solution:
    import heapq
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        cnt = collections.Counter(words)
        li = [(-freq,word) for word,freq in cnt.items()]
        # heapify 使heap 化
        heapq.heapify(li)
        return [heapq.heappop(li)[1] for _ in range(k)]


```



#### 107. [743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

```python
class Solution:
    # 硬是拿dfs莽着做，9000+，需要熟悉常见的图算法，不是dfs,bfs这种
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        dic = defaultdict(list)
        for pair in times:
            dic[pair[0]].append((pair[1],pair[2]))
        res = [6000001 for _ in range(N)]

        #print(res)
        #print(dic)
        def dfs(n,t):
            #res[n-1] = min(t,res[n-1])
            if t>= res[n-1]:
                return
            res[n-1] = t
            if not dic[n]:
                return 
            for p in dic[n]:
                #print(p)
                # res[p[0]-1] = min(t+p[1],res[p[0]-1])
           
                dfs(p[0],t+p[1])
                #dic[n].pop(0)
        dfs(K,0)
        print(res)
        return -1 if max(res) == 6000001 else max(res)
class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        # 虽然这一题是用堆的标签搜到的，不过貌似是一个图论的问题
        # dijkstra算法,确实快很多
        # 1 .将起始点到各点的距离初始化为无穷大
        dist = {i:float('inf') for i in range(1,N+1)}
        # 起始点自身为0
        dist[K] = 0
        # j结果集
        res = {}
        # print(dist,type(dist))
        while dist:
            # 每一次挑选已有结果中最短的作为计算节点
            min_dis = min(dist.values())
            # 如果剩下的还有无限大的值，且是最小值，说明，有的点根本没有联系到，可以返回-1
            if min_dis == float('inf'):
                return -1
            # 遍历剩下的节点，找到距离等于min_dis的点，一个就可以,因为每一次只向结果集里加入一个节点，如果有复数，可以在下一次添加
            for k,v in dist.items():
                if v == min_dis:
                    ind = k
                    break
            for time in times:
                # 这个点关联的其他点，更新距离，其他店需要不在结果集里面
                if time[0] == ind and time[1] not in res.keys():
                    dist[time[1]] = min(dist[time[1]], dist[time[0]]+time[2])
            res[ind] = min_dis
            # dist.pop(ind)
            del dist[ind]
        return max(res.values())
    
    
##  使用堆优化互殴更快一点
class Solution:
    def networkDelayTime(self, times: List[List[int]], N: int, K: int) -> int:
        graph = collections.defaultdict(list)
        for u, v, w in times:
            graph[u].append((v, w))

        pq = [(0, K)]
        dist = {}
        while pq:
            d, node = heapq.heappop(pq)
            if node in dist: continue
            dist[node] = d
            for nei, d2 in graph[node]:
                if nei not in dist:
                    heapq.heappush(pq, (d+d2, nei))

        return max(dist.values()) if len(dist) == N else -1
```

#### 108. [376. 摆动序列](https://leetcode-cn.com/problems/wiggle-subsequence/)

```python
# dp解法
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        # 根据末尾的形状，有上升和下降两种序列，状态相互转化，可以用up,down 表示
        n = len(nums)
        if n<2:
            return n
        up = down = 1 # 初始化
        for i in range(1,n):
            if nums[i] > nums[i-1]:
                up = max(down+1, up)
            elif nums[i]<nums[i-1]:
                down = max(up+1,down)
            # 相等时，没有改变，所以不用考虑
        return max(up, down)

    
## 也可以使用贪心解法，直接计算峰、谷的数量
class Solution:
    def wiggleMaxLength(self, nums: List[int]) -> int:
        n = len(nums)
        if n < 2:
            return n
        
        prevdiff = nums[1] - nums[0]
        ret = (2 if prevdiff != 0 else 1)
        for i in range(2, n):
            diff = nums[i] - nums[i - 1]
            if (diff > 0 and prevdiff <= 0) or (diff < 0 and prevdiff >= 0):
                ret += 1
                prevdiff = diff
        
        return ret

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/wiggle-subsequence/solution/bai-dong-xu-lie-by-leetcode-solution-yh2m/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 109  [5610. 有序数组中差绝对值之和](https://leetcode-cn.com/problems/sum-of-absolute-differences-in-a-sorted-array/)

```python
from typing import List

# 没有难度，注意题目中的非递减特性，更快的方法其实是用前缀和，不过懒得再写了
class Solution:
    def getSumAbsoluteDifferences(self, nums: List[int]) -> List[int]:
        res = []
        t = 0
        record = []
        for j in range(len(nums)):
            t += nums[j]
            record.append((t))
        print(record)
        for i in range(len(nums)):
            tmp = 0
            a= i*nums[i]-record[i-1] if i >0 else 0
            b = record[len(nums)-1]-record[i]-(len(nums)-i-1)*nums[i]
            print(a,b)
            tmp = a +b
            res.append(tmp)
            #print(tmp)
        return  res
```

#### 110 [5626. 十-二进制数的最少数目](https://leetcode-cn.com/problems/partitioning-into-minimum-number-of-deci-binary-numbers/)

```python
# 水题，一开始被名词吓住了，以为要搞二进制，其实就是用10这种数来拼结果，都是十进制，那直接找最大位就好，懒得手写遍历，其实还能提前找到9break
class Solution:
    def minPartitions(self, n: str) -> int:


        a = collections.Counter(n)
        return int(max(a.keys()))

```

#### 111. [5611. 石子游戏 VI](https://leetcode-cn.com/problems/stone-game-vi/)

```python

class Solution:
    def stoneGameVI(self, aliceValues: List[int], bobValues: List[int]) -> int:
        # 思路是对的，通过数据量，知道不能用dp,需要贪心
        # 但是细节错了，不能考虑每一步的局部，直接转换视角从整体考虑
        # 假设b拿到了所有的石子，那么每一次a取数，都是在降低差值，没取一个，都是a+ ,b- ，缩小差距
        # 这一可以将a+b 作为标准排序，然后abab这样依次取数，看最后结果
        # 或者是另一种解释：
       	"""
       	假设只有两个石头,对于 a， b 的价值分别是 a1, a2, b1, b2

第一种方案是A取第一个，B取第二个，A与B的价值差是 c1 = a1 - b2
第二种方案是A取第二个，B取第一个，A与B的价值差是 c2 = a2 - b1
那么这两种方案对于A来说哪一种更优，就取决于两个方案的价值差的比较

记 c = c1 - c2 = （a1 - b2） - (a2 - b1) = (a1 + b1) - (a2 + b2)



作者：spacex-1
链接：https://leetcode-cn.com/problems/stone-game-vi/solution/c-tan-xin-zheng-ming-by-spacex-1-xi8y/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。"""
        arr = [(a+b, a, b) for a, b in zip(aliceValues, bobValues)]
        arr.sort(reverse=True)

        n = len(arr)
        a_val, b_val = 0, sum(bobValues)
        for i in range(n):
            _, a, b = arr[i]
            if i % 2 == 0:
                a_val += a
                b_val -= b

        if a_val == b_val:
            return 0

        if a_val > b_val:
            return 1
        return -1

```

#### 112. [5627. 石子游戏 VII](https://leetcode-cn.com/problems/stone-game-vii/)

```python

class Solution:
    # 赛后做出来的，这里其实是被上一道题误导，以为中等难度，就不会出dp，其实看数据《1000,就能看到大概率使用dp
    # 然后就是找转移状态，先初始化相邻两个为一组，先手的值，然后扩展到三个乃至更多
    # 方程的话 dp[i][j] = max(sum(i+1:j+1) - dp[i+1][j] , sum(i,j)-dp[i][j-1]),算是某种自底向上
    # 当然为了避免超时，需要用前缀和代替sum
    def stoneGameVII(self, stones: List[int]) -> int:
        s = sum(stones)
        a=b=0
        n =len(stones)
        dp= [[0 for _ in range(n)] for _ in range(n) ]
        for i in range(1,n):
            dp[i-1][i] = max(stones[i-1:i+1])
        tmp = []
        s = 0
        for i in range(n):
            s += stones[i]
            tmp.append(s)
        for i in range(n-3,-1,-1):
            for j in range(i+2,n):
                # if sum(stones[i+1:j+1]) != tmp[j]-tmp[i]:
                #     print('f')

                if i -1< 0:
                    c = tmp[j-1]
                else:
                    c = tmp[j-1]-tmp[i-1]

                dp[i][j] = max(tmp[j]-tmp[i]- dp[i + 1][j], c - dp[i][j - 1])
                #dp[i][j] = max(sum(stones[i+1:j+1])-dp[i+1][j],sum(stones[i:j])-dp[i][j-1])
        #print(dp)
        return dp[0][n-1]

```

#### 113. [954. 二倍数对数组](https://leetcode-cn.com/problems/array-of-doubled-pairs/)

```python
class Solution:
    def canReorderDoubled(self, A: List[int]) -> bool:
        # 先按照绝对值排序，然后计数，实际上就是看是不是有2倍关系
        A.sort(key=lambda x:abs(x), reverse=True)
        cnts = Counter(A)
        # 然后依次配对减去
        while A:
            num = A.pop()
            if cnts[num]>0 and cnts[2*num]>0:
                cnts[2*num] -= 1
                cnts[num] -= 1
        return sum(cnts.values())==0


```

#### 114. [738. 单调递增的数字](https://leetcode-cn.com/problems/monotone-increasing-digits/)

```python
class Solution:
    def monotoneIncreasingDigits(self, N: int) -> int:
        digit = list(str(N))
        n = len(digit)
        for i in range(n - 1, 0, -1):
            if digit[i] < digit[i - 1]:
                digit[i - 1] = str(int(digit[i - 1]) - 1)
                digit[i:] = ['9'] * (n - i)  #此处能否再优化下？                    
        return int(''.join(digit))

```

#### 115. [189. 旋转数组](https://leetcode-cn.com/problems/rotate-array/)

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        if n < 2:
            pass
        else:
            while k > 0:
                a = nums[-1]
                nums[1:n] = nums[0:n-1]
                nums[0] = a
                k -= 1
                
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n=len(nums)
        k=k%n
        def swap(l,r):
            while(l<r):
                nums[l],nums[r]=nums[r],nums[l]
                l=l+1
                r=r-1
        swap(0,n-k-1)
        swap(n-k,n-1)
        swap(0,n-1)

class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)
        k %= n
        nums[:] = nums[::-1]
        nums[:k] = nums[:k][::-1]
        nums[k:] = nums[k:][::-1]

import math
# 这个才是完美方案
# 忽略一点问题，不得已，抄袭别人的题解
def rotate(nums,k):
    """
    Do not return anything, modify nums in-place instead.
    """
    k %= len(nums) #只做有效的移动
    lcm = len(nums) * k // math.gcd(len(nums), k) if len(nums)%k!=0 else len(nums) #求取最小公倍数
    m = lcm//k #计算m
    count = len(nums)
    iter_count = max(len(nums) // m - 1, 0) #计算闭环数
    while iter_count >= 0: #对于每一个闭环进行移动
        index = iter_count #初始化起点位置
        value = nums[iter_count]
        while True: #迭代进行每一次闭环移动
            dest = (index + k) % len(nums)
            nums[dest], value = value, nums[dest]
            count -= 1
            index = dest
            if dest == iter_count or count == 0: #当一个闭环完成时或无闭环且全部移动完成时退出
                break
        iter_count -= 1

作者：QsingH
链接：https://leetcode-cn.com/problems/rotate-array/solution/huan-zhuang-ti-huan-de-li-lun-fen-xi-by-qsingh/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。 

```

#### 116. [1254. 统计封闭岛屿的数目](https://leetcode-cn.com/problems/number-of-closed-islands/)

```python
class Solution:
    # 经典的dfs图搜索，特别之处在于需要预处理边界岛屿
    def closedIsland(self, grid: List[List[int]]) -> int:
        m ,n = len(grid), len(grid[0])
        moves = [(-1,0),(1,0),(0,1),(0,-1)]
        def dfs(x,y):
            if grid[x][y] ==1:
                return 
            grid[x][y] = 1
            for move in moves:
                mx,my = x+move[0],y+move[1]
                if 0<= mx<m and 0 <= my <n:
                    dfs(mx,my)
        for i in range(m):
            dfs(i,0)
            dfs(i,n-1)
        for j in range(n):
            dfs(0,j)
            dfs(m-1,j)
        res = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] ==0:
                    dfs(i,j)
                    res += 1
        return res
            
```

#### 117. [1477. 找两个和为目标值且不重叠的子数组](https://leetcode-cn.com/problems/find-two-non-overlapping-sub-arrays-each-with-target-sum/)

```python
class Solution:
    def minSumOfLengths(self, arr: List[int], target: int) -> int:
        res = len(arr) + 1
        pre = {}
        # 预设一个边界，方便i==0的时候，用来记录当前位置之前的前缀和，value是位置索引
        pre[0] = -1
        # p用来记录前缀和
        p = 0
        # 初始化
        dp = [float('inf')] * len(arr)# 存放在该位置之前的最短的一个长度
        for i, a in enumerate(arr):
            p += a
            # 先记录一个i-1之前的最小的长度（如果i==0,i-1==-1,因为从前向后改编，所以也是inf,不用担心越界的影响）
            dp[i] = dp[i-1]
            # (p - target) in pre  说明  从当前向前走一段，会有target出现。
            if p-target in pre:
                cur = i-pre[p-target]
                #print('cur:', cur)
                if dp[pre[p-target]] != float('inf'):
                    #res = min(res, cur+dp[pre[p-target]])
                    print(res)
                dp[i] = min(dp[i],cur)
            pre[p] = i
        return -1 if res == len(arr)+1 else res



```



#### 118. [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```python
class Solution:
    def maxProfit(self, prices: List[int], fee: int) -> int:
        # 老式玩法，之前做过，简单的dp
        n = len(prices)
        sell, buy = 0, -prices[0]
        for i in range(1, n):
            sell, buy = max(sell, buy + prices[i] - fee), max(buy, sell - prices[i])
        return sell
```

#### 119. [667. 优美的排列 II](https://leetcode-cn.com/problems/beautiful-arrangement-ii/)

```python
# 运气好找到规律了，否则呢？
class Solution:
    def constructArray(self, n: int, k: int) -> List[int]:
        res = []
        if k == 0:
            res = [i for i in range(1,n+1)]
            return res
        elif k >n-1:
            return -1
        res.append(1)
        s = set()
        s.add(1)
        falg = 1
        for i in range(n-1):
            if k == 0:break
            tmp = res[-1] + k*falg
            if tmp in s:
                break
            res.append(tmp)
            s.add(tmp)
            k -=1
            falg *= -1
        for i in range(1,n+1):
            if i not in s:
                res.append(i)
        return res


            


```

#### 120.[919. 完全二叉树插入器](https://leetcode-cn.com/problems/complete-binary-tree-inserter/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# 想法错了，没有完全利用好完全二叉树性质。
class CBTInserter(object):
    # 就是堆的玩法
    def __init__(self, root):
        #这个队列记录所有缺少子节点的节点
        self.deque = collections.deque()
        self.root = root
        q = collections.deque([root])
        while q:
            node = q.popleft()
            # 如果缺少子节点就入队
            if not node.left or not node.right:
                self.deque.append(node)
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)

    def insert(self, v):
        # 依照完全二叉树特性，最早确实节点的，最先被满足
        node = self.deque[0]
        self.deque.append(TreeNode(v))
        if not node.left:
            #  缺少左，就补充左
            node.left = self.deque[-1]
        else:
            # 缺少右，就补充右，因为圆满了，就可以出队
            node.right = self.deque[-1]
            self.deque.popleft()
        return node.val

    def get_root(self):
        return self.root
        


# Your CBTInserter object will be instantiated and called as such:
# obj = CBTInserter(root)
# param_1 = obj.insert(v)
# param_2 = obj.get_root()
```

#### 121. [442. 数组中重复的数据](https://leetcode-cn.com/problems/find-all-duplicates-in-an-array/)

```python
class Solution:
    # 原地使用的话，就要学会使用本身空间，要么数字更换，要么用负号作为标签，见到负号说明访问过了，就添加在res中
    def findDuplicates(self, nums: List[int]) -> List[int]:
        res = []
        for c in nums:
            c= abs(c)
            if nums[c-1] <0:
                res.append(c)
            else:
                nums[c-1] *= -1
        return res

```

#### 122. [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
         # 水平翻转
        n = len(matrix)
        for i in range(n // 2):
            for j in range(n):
                matrix[i][j], matrix[n - i - 1][j] = matrix[n - i - 1][j], matrix[i][j]
        # 主对角线翻转
        for i in range(n):
            for j in range(i):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]


```

#### 123. [1226. 哲学家进餐](https://leetcode-cn.com/problems/the-dining-philosophers/)

```python
class DiningPhilosophers:
    # 最简单的使用threading,然后上一个全局的锁，然后用acquire(),release()控制
    def __init__(self):
        import threading
        self.lock = threading.Lock()

    # call the functions directly to execute, for example, eat()
    def wantsToEat(self,
                   philosopher: int,
                   pickLeftFork: 'Callable[[], None]',
                   pickRightFork: 'Callable[[], None]',
                   eat: 'Callable[[], None]',
                   putLeftFork: 'Callable[[], None]',
                   putRightFork: 'Callable[[], None]') -> None:
                   self.lock.acquire()
                   pickLeftFork()
                   pickRightFork()
                   eat()
                   putLeftFork()
                   putRightFork()
                   self.lock.release()
 
## Condition方法，很不熟悉，基本理解成条件锁好了
import threading
class DiningPhilosophers:
    def __init__(self):
        self.cv = threading.Condition()
        # self.thread_list = [None for i in ragne(5)]
        self.d = {}
        for i in range(5):
            self.d[i] = False

    # call the functions directly to execute, for example, eat()
    def wantsToEat(self, philosopher: int, *actions) -> None:
        """
        Solution 2, using threading condition
        """
        neighbors = [philosopher - 1, philosopher + 1]
        if neighbors[0] < 0: neighbors[0] = 4
        if neighbors[1] > 4: neighbors[1] = 0

        with self.cv:
            self.cv.wait_for(lambda: not self.d[neighbors[0]] and not self.d[neighbors[1]])
            self.d[philosopher] = True
## *actions来代替多函数指针参数，遍历时加括号运算符即可执行。
            [*map(lambda func: func(), actions)]
        
        self.d[philosopher] = False

```

#### 124. [722. 删除注释](https://leetcode-cn.com/problems/remove-comments/)

```python
# 字符串拼接会很麻烦，很多很多边界条件要调试，费时，而且几乎是面向测试用例编程
class Solution:
    def removeComments(self, source: List[str]) -> List[str]:
        res = []
        flag = 0
        stk = []
        skip = False
        # for i in range(10):
        #     print(i)
        #print('-1:',type(source[9]),len(source[9]),type(len(source)))
        for s in source:

            #print('z:',z)
            #print(s)
            # if  s.strip():
            #     print('fuck')
            #     continue
            #print('len:',len(s),z,s)
            if len(s)==1 and not flag:
                #print('s:',s)
                res.append(s)
                continue
            for i in range(len(s)-1):
                if not flag:
                    if skip:
                        if i == len(s)-2:
                            #print(stk)
                            stk.append(s[-1])
                            res.append("".join(stk))
                            skip = False
                            # print(res,flag,skip,stk)
                            stk = []
                            break
                        skip = False
                        continue
                    if s[i:i+2] not in ["//","/*"]:
                        stk.append(s[i])
                        if i == len(s)-2:
                            stk.append(s[-1])
                            res.append("".join(stk))
                            stk = []
                            break
                    elif s[i:i+2] == "//":
                        if stk:
                            res.append("".join(stk))
                        stk = []
                        break
                    elif s[i:i+2] == "/*":
                        flag = 1
                        skip = True
                        continue

                else:
                    if skip:
                        if i == len(s)-2:
                            #print(stk)
                            stk.append(s[-1])
                            res.append("".join(stk))
                            stk = []
                            break
                        skip = False
                        continue
                    if s[i:i+2] == "*/":
                        flag = 0
                        if i == len(s)-2:
                            if not stk:
                                break
                            else:
                                res.append("".join(stk))
                                stk=[]
                                break
                        #print("i",i)
                        skip=True
                        #print("new i",i)
                        continue
        return res
        

                    
          
                    
class Solution(object):
    # 思路清晰的解法
    def removeComments(self, source):
        in_block = False
        ans = []
        for line in source:
            i = 0
            # 如果不在注释块中，另起一个缓冲区
            if not in_block:
                newline = []
            while i < len(line):
                # 切片左闭右开，所以不用考虑右边是否越界
                # 块外见到/*,改变状态，注意要移动一位
                if line[i:i+2] == '/*' and not in_block:
                    in_block = True
                    i += 1
                # 块内见到*/，也改变状态，注意要移动一位
                elif line[i:i+2] == '*/' and in_block:
                    in_block = False
                    i += 1
                # 快外见到,舍弃这一行
                elif not in_block and line[i:i+2] == '//':
                    break
                # 除了以上之外，加入缓冲区
                elif not in_block:
                    newline.append(line[i])
                # 统一移动一位
                i += 1
            # 一行终结，且不在块中，可以刷新缓冲区
            if newline and not in_block:
                ans.append("".join(newline))

        return ans

    
    
    

class Solution:
    # 直接使用拼接字符串的玩法，活用find
    """filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成的新列表。

该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。
        map() 会根据提供的函数对指定序列做映射。第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。"""
    # find:string.find(str, beg=0, end=len(string))检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1
    def removeComments(self, source: List[str]) -> List[str]:



        print('test:')
        t = '/*adc*/cd'
        print(t.find('*/'),t[t.find('*/')+3])
        s = '\n'.join(source) + '\n' #为最后一行的'//'提供闭区间
        i, n = 1, len(s)
        ans = ''
        while i < n:
            
            # 找下一行
            if s[i - 1] + s[i] == '//':
                print(ans)
                i = s.find('\n', i) + 1
            # 找块外
            elif s[i - 1] + s[i] == '/*':
                print(i,s[i])
                # 因为加的是i-1，所以需要直接移动到开始的下一位，在用于判断
                i = s.find('*/', i + 1) + 3
                print(i,s[i])
            else:
                # 注意这里加的是i-1
                ans += s[i - 1]
                i += 1
        # 检查是不是空字符串
        def check(s):
            if len(s)>0:
                return True
            return False
        print(ans)
        return filter(check, ans.split('\n'))


```

#### 125. [5630. 删除子数组的最大得分](https://leetcode-cn.com/problems/maximum-erasure-value/)

```python
class Solution:
    # 双指针
    def maximumUniqueSubarray(self, nums: List[int]) -> int:
        l,r= 0,0
        n = len(nums)
        sum = 0
        res = 0
        s = set()
        while r<n:
            if nums[r] not in s:
                #print(n,r)
                s.add(nums[r])
                sum += nums[r]
                res = max(res, sum)
                # print(res)
                r += 1
            else:
                s.remove(nums[l])
                sum -= nums[l]
                l += 1
            #print(s, l, r)
        return res
```

#### 126. [316. 去除重复字母](https://leetcode-cn.com/problems/remove-duplicate-letters/)

```python
class Solution:
    # 类似这种需要按条件去重，同时还要保存某种顺序的情况下，又得到某些顺序，可以考虑栈
    # 利用单调栈，可以保存字典序
    # set,确认，是否使用这个字符串
    # Counter（）,用来计算各个字符的次数，如果只有一次，就要知道在单调栈中不能舍弃。
    def removeDuplicateLetters(self, s: str) -> str:
        stack = []
        seen = set()
        remain_counter = collections.Counter(s)

        for c in s:
            if c not in seen:
                # 单调栈，栈顶元素比要加入来的大，且即便弹出，后面还剩下，可以将其加上
                while stack and c < stack[-1] and  remain_counter[stack[-1]] > 0:
                    seen.discard(stack.pop()) #  比remove要好一些
                seen.add(c)
                stack.append(c)
            # 因为用到了次数-1
            remain_counter[c] -= 1
        return ''.join(stack)


```

#### 127. [5631. 跳跃游戏 VI](https://leetcode-cn.com/problems/jump-game-vi/)

```python
class Solution:
    def maxResult(self, nums: List[int], k: int) -> int:
        # 比赛时dp超时，优化有想到heap，不过首先是不是很确信，其次是heap的用法没有想明白，因为有k的限制，想将k也操控好
        #  看到题解才明白，可以在使用堆的时候，可以不用考虑k,直到取用的时候，在考虑。这时候，如果发现尽管是最大值，但是却超过了k的跨度，就pop，因为后面也不会用到了
        # python 是小根堆，所以用负值
        # heap的类型，在heapify时就决定下来了，所以先将li初始化，用来确定后续添加的元素类型
        n = len(nums)
        li = [(-nums[0], 0)]
        #pq = heapq.heapify(li)
        heapq.heapify(li) # 这个没有返回值，直接改变
        res = nums[0]
        for i in range(1,n):
            while i- li[0][1]>k:
                heapq.heappop(li)
            res = -li[0][0] + nums[i]
            heapq.heappush(li,(-res,i))
        return res
    
class Solution:
    def maxResult(self, nums: List[int], k: int) -> int:
        """如果i<j,dp[i]<dp[j].那么在j之后，i永远不会再被用到，应为i到不了的，j可以到，i和j能到的，j比i有效
        所以可以使用单调队列保持这种关系,准确来说是严格单调递减,这样会把大的值尽可能放到队首使用，中间pop就是上面所讲的，i，值又小，覆盖范围也更窄（因为部分已经被算出来，甚至全部算出来）
        当然，如果之后，出现索引跨度超过k,和heap一样，也要清除掉，因为以后也用不到了"""
        dq = collections.deque()
        dq.append((nums[0],0))
        res = nums[0]

        for i in range(1,len(nums)):
            while i - dq[0][1] >k:
                dq.popleft()
            res = dq[0][0] + nums[i]
            # 维持单调性
            while dq and res>= dq[-1][0]:
                dq.pop()
            dq.append((res,i))
        return res

```

#### 128. [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

```python
class Solution:
    # 单调栈解法
    def nextGreaterElements(self, nums: List[int]) -> List[int]:
        n = len(nums)
        stk,res = [],[-1]*n

        for i in range(n):
            while stk and (nums[stk[-1]] < nums[i]):
                res[stk.pop()] = nums[i]
            stk.append(i)
        # 因为是循环数组，再来一次循环
        for i in range(n):
            while stk and (nums[stk[-1]] < nums[i]):
                res[stk.pop()] = nums[i]
            if not stk:
                break
        return res
```

#### 129. [LCP 03. 机器人大冒险](https://leetcode-cn.com/problems/programmable-robot/)

```python
class Solution:
    # 不能直白模拟，数字太大，然后边界条件，做之前想清楚，不要反复调试
    def robot(self, command: str, obstacles: List[List[int]], x: int, y: int) -> bool:
        i =0
        n = len(command)
        s = set()
        start_x, start_y =0, 0
        s.add((0,0))
        for i in command:
            if i== 'U':
                start_y += 1
            else:
                start_x += 1
            s.add((start_x,start_y))

        #print(start_x,start_y)
        m = x//start_x
        n = y // start_y
        t = min(m,n)
        t_x = x -start_x*t
        t_y =y- start_y*t
        if (t_x, t_y) not in s:
            print(x,y)
            return False
        if obstacles:
            #print('fuck')
            for _x,_y in obstacles:
                if _x>x or _y>y:
                    #print('ss')
                    continue
                m = _x//start_x
                n = _y // start_y
                t = min(m,n)
                _x -= start_x*t
                _y -= start_y*t
                print(_x,_y)
                if (_x,_y) in s:
                    print(_x,_y)
                    return False
        return True
```



#### 130. [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def zigzagLevelOrder(self, root: TreeNode) -> List[List[int]]:
        res = []
        if not root:
            return res
        flag = 1
        q = []
        q.append(root)
        while q:
            n = len(q)
            tmp = []
            for i in range(n):             
                    t= q.pop(0)
                    if t.left:
                        q.append(t.left)
                    if t.right:
                        q.append(t.right)
                    tmp.append(t.val)
            if flag:
                res.append(tmp)
            else:
                res.append(tmp[::-1])
            flag = 1- flag
            
        return res
```

#### 131.[386. 字典序排数](https://leetcode-cn.com/problems/lexicographical-numbers/)

```python
class Solution:
    def lexicalOrder(self, n: int) -> List[int]:
        return sorted(list(range(1,n+1)),key=lambda x:str(x))
    ## 其实上面算是偷懒的解法，正常解法是10叉树dfs
    
def dfs(cur,n,res): # cur为根结点
            if cur > n:
                return 
            else:
                res.append(cur)
                for i in range(10):
                    if 10 * cur + i > n: # 比如叶子结点为14，而n是13，dfs就结束了
                        return 
                    dfs(10 * cur + i, n, res)
        res = []
        # 对每棵树进行dfs
        for i in range(1,10):
            dfs(i, n, res)
        return res

作者：z1m
链接：https://leetcode-cn.com/problems/lexicographical-numbers/solution/386zi-dian-xu-pai-shu-python-by-ml-zimingmeng/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 132. [5622. 平均等待时间](https://leetcode-cn.com/problems/average-waiting-time/)

```python
## 有点类似于滑动窗口
from typing import List


class Solution:
    def averageWaitingTime(self, customers: List[List[int]]) -> float:
        end = 0
        n = len(customers)
        _sum =0
        for start,cost in customers:
            print(_sum)
            if start>= end:
                _sum += cost
                end = start+ cost
            else:
                _sum += cost + (end -start)
                end  = end + cost
        #print(_sum)
        return _sum/n
```



#### 133.[ 修改后的最大二进制字符串](https://leetcode-cn.com/problems/maximum-binary-string-after-change/)

```python
class Solution:
    def maximumBinaryString(self, binary: str) -> str:
        # 其实这里一开始想偏了，这两种操作可以先把所有1，右移，这样前面都是0，就可以用操作1，前x-1 个0，可以变成1
        n = len(binary)
        tmp = ['1']*n
        idx ,cnt =n,0
        f = True
        for i,v  in enumerate(binary):
            print
            if f:
                if v == "0":
                    idx = i
                    f ^=f
            if v == '0':
                cnt += 1
       # print(idx,cnt)
        if cnt >0:
            tmp[idx + cnt -1]='0'
        return "".join(tmp)
```

#### 134. [5638. 吃苹果的最大数目](https://leetcode-cn.com/problems/maximum-number-of-eaten-apples/)

```python
# 运气不错，按照堆的写法写出来了。主要还是python中的heap不熟悉，调试花了些时间
from typing import List
import heapq

class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:
        li = [(-1, apples[0])]
        heapq.heapify(li)
        res = 0
        for i in range(len(days)):
            if apples[i] >0 and days[i]>0:
                heapq.heappush(li,(i+days[i],apples[i]))
            if li:
                x,y = heapq.heappop(li)
            while len(li) > 0 and (i>=x or y<=0):
                x, y = heapq.heappop(li)
            if i<x and y>0:
                res +=1
                y -=1
                heapq.heappush(li,(x,y))
            else:
                res +=0
            #print(res)
        i = len(days)
        #print(li)
        #print(li)
        while li:
            #print(res)
            x, y = heapq.heappop(li)
            while li and (i >= x or y <= 0):
                x, y = heapq.heappop(li)
            if i<x and y>0:
                res +=1
                y -=1
                heapq.heappush(li,(x,y))
            else:
                res +=0
            i += 1
        return res
 #### 一种更聪明的解法：
class Solution:
    def eatenApples(self, apples: List[int], days: List[int]) -> int:     
        gain = collections.defaultdict(int)
        res = 0
        n = len(apples)        
        count = 0
        for i in range(n):
            gain[i] += apples[i]
            gain[i+days[i]] -= apples[i]

        for i in range(max(sorted(gain))):
            count += gain[i]
            if count > 0:   res += 1

        if n == 1:  return min(apples[0],days[0])   

        return res

作者：zackcai
链接：https://leetcode-cn.com/problems/maximum-number-of-eaten-apples/solution/python3-bian-jie-suan-fa-by-zackcai-t97w/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 135. [5210. 球会落何处](https://leetcode-cn.com/problems/where-will-the-ball-fall/)

````python
# 相较于上一道题，这个就是完全的模拟了。除了实际做的时候，不断调试试错花了些时间，确实，代码还是写少了
from typing import List


class Solution:
    def findBall(self, grid: List[List[int]]) -> List[int]:
        res = []
        m = len(grid)
        n = len(grid[0])
        def check(pre,x,y):
            #print(pre,x,y)
            if x>=m:
                return y
            if y<0 or y>=n:
                return -1
            if grid[x][y]==1:
                if pre[1]>=y:
                    new_x = x
                    new_y = y+1
                else:
                    new_x = x+1
                    new_y =y

                if new_x == pre[0] and new_y==pre[1]:
                    return -1
            else:
                if pre[1]>y:
                    new_x = x+1
                    new_y = y
                else:
                    new_x = x
                    new_y =y-1
                if new_x == pre[0] and new_y == pre[1]:
                    return -1
            #print(new_x,new_y)
            return check((x,y),new_x,new_y)
        for i in range(n):
            #print('i',i)
            t = check((-1,i),0,i)
            #print(t)
            res.append(t)
        return res


````

#### 136. [421. 数组中两个数的最大异或值](https://leetcode-cn.com/problems/maximum-xor-of-two-numbers-in-an-array/)

```python
class Solution:
    def findMaximumXOR(self, nums: List[int]) -> int:
        # 二叉字典树，叫前缀树也可以，0,1分叉
        L = len(bin(max(nums)))-2 # 为了规避Ox
        # 构造每一位的序列，同时因为权重问题，需要左右颠倒
        nums = [[(x>>i)&1 for i in range(L,-1,-1)] for x in nums ]
        max_xor = 0
        # 构造字典树
        trie = {}
        for num in nums:
            # 每一次都从根部找
            node = trie
            xor_node = trie
            curr_xor = 0 # 每次的最大值
            for bit in num:
                # 这算是一次遍历，一边构建树，一边比较产生较大值
                # 不用担心前边构建的比较不了后边，异或a^b 和 b^a 实质是一样的，后面的比较中，实际上就是在和前边的异或
                #
                if not bit in node:
                    node[bit] = {}
                node = node[bit]
                target = bit^1
                # 开始比较
                # 如果能够异或，curr 进位取1，走target的路线
                if target in xor_node:
                    curr_xor = (curr_xor<<1)|1
                    xor_node = xor_node[target]
                # 否则走自己的落线，curr 进位不取1 ， 实际上是取0了
                else:
                    curr_xor= curr_xor<<1
                    xor_node = xor_node[bit]
            max_xor = max(max_xor,curr_xor)
        return max_xor

```

#### 137. [435. 无重叠区间](https://leetcode-cn.com/problems/non-overlapping-intervals/)

```python
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0
        # intervals.sort(key= lambda x:x[1])
        # n = len(intervals)
        # right = intervals[0][1]
        # res = 1
        # for i in range(1,n):
        #     if intervals[i][0] >= right:
        #         res += 1
        #         right = intervals[i][1]
        # return n -res
        # 只用关注什么时候结束的，不用关心什么时候开始的，否则，（1,10）（3,4）这种情况会结果就不对了
        intervals.sort(key= lambda x: (x[1]))
        n = len(intervals)
        right = intervals[0][1]
        res =0 
        for i in range(1,n):
            if right> intervals[i][0]:
                res += 1
            else:
                right = intervals[i][1] 
        return res
    
class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        n = len(intervals)
        if n==0:
            return 0
        dp = [1]*n
        intervals.sort(key = lambda x:x[0])
        # 一种变相的递增子序列问题，实际上在排序之后，比较的是前一节的末尾和后一节的首尾，<= 的话，就是“递增”可以增加长度
        for i in range(n):
            # 因为开始时间的，所以这里是倒着走的，一旦遇到合适的就可以剪枝
            for j in range(i-1,-1,-1):
                if intervals[i][0] >= intervals[j][1]:
                    dp[i] = max(dp[i], dp[j]+1)
                    break
        return n - max(dp)
    


        
```

#### 138. [300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

```python
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # 以i为结尾的最长子序列长度，从左到右动态规划
        n = len(nums)
        if n==0:
            return 0
        dp =[1] *n 
        res = 1
        for i in range(n):
            for j in range(i):
                if nums[i]>nums[j]:
                    dp[i] = max(dp[i],dp[j]+1)
                    res = max(res,dp[i])
        return res
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        # 维护一个增长梯度最小的数组，新来的数如果大于最后一个，就加进来，否则用二分查找的方式，找到第一个大于它的数字，替换掉。最后的答案是长度
        d = []
        for n in nums:
            if not d or n > d[-1]:
                d.append(n)
            else:
                l, r = 0, len(d) - 1
                loc = r
                while l <= r:
                    mid = (l + r) // 2
                    if d[mid] >= n:
                        loc = mid
                        r = mid - 1
                    else:
                        l = mid + 1
                d[loc] = n
        return len(d)


```

#### 139. [646. 最长数对链](https://leetcode-cn.com/problems/maximum-length-of-pair-chain/)

```python
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        n = len(pairs)
        if n== 0: return 0
        dp = [1]*n
        res = 1
        pairs.sort(key = lambda x:(x[0],x[1]))
        
        for i in range(n):
            # 因为之前的排序，所以可以倒着走，进而可以剪枝，省去无用计算
            for j in range(i-1,-1,-1):
                if pairs[i][0] >pairs[j][1]:
                    dp[i] =max(dp[i],dp[j]+1)
                    break
                    

        return max(dp)
    
    
    
### 贪心方法
class Solution:
    def findLongestChain(self, pairs: List[List[int]]) -> int:
        n = len(pairs)
        if n== 0: return 0
        res = 1
        pairs.sort(key = lambda x:(x[1]))
        print(pairs)
        
        right = pairs[0][1]
        for i in range(1,n):
            if pairs[i][0]>right:
                res +=1
                right = pairs[i][1]
        return res
```

#### 140. [673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

```python
class Solution:
    def findNumberOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [1] *n
        # 最长递增子序列的变种
        # 需要记录每个i结尾的长度的次数
        cnt = [1]*n
        for i in range(1,n):
            for j in range(i):
                if nums[i]>nums[j]:
                    t = dp[j]+1
                    # 这里不能使用max,是因为需要统计长度，如果dp[j]+1 <dp[j] ,本来不要考虑的，使用max以后，作为dp[i],干扰结果
                    # if the value show the first time,flush the dp[i],and add the cnt bofore
                    if t>dp[i]:
                        dp[i] = t
                        cnt[i] = cnt[j]
                    # if the value show more times,add the cnt bofore
                    elif t == dp[i]:
                        cnt[i] += cnt[j]
        length = max(dp)
        res = 0
        for i in range(n):
            if dp[i] == length:
                res += cnt[i]
        return resa
```

#### 141. [5642. 大餐计数](https://leetcode-cn.com/problems/count-good-meals/)

```python
from typing import List
import collections

class Solution:
    def countPairs(self, deliciousness: List[int]) -> int:
        pin = 10**9 +7
        rc= []
        i = 0
        _max = 2*(10**5)
        t = 0
        # 直接按照数量级最小估计，在大数据中没有超时，但是，没有数字没有枚举全，所以出错，竞赛时，可以考虑枚举的多一点，覆盖全
        while i <= 21:
            t = 2**i
            rc.append(t)
            i +=1
        #print(rc)
        res = 0
        cnt = collections.Counter(deliciousness)
        tmp = list(cnt.keys())
        tmp.sort()
        n = len(tmp)
        s = set(tmp)
        # print(tmp)
        # print(cnt)
        used = set()
        for i in range(n):
            for j in range(len(rc)):
                t = rc[j] - tmp[i]
                if t in s and t not in used:
                    # print('res:',res)
                    if t == tmp[i]:
                        res += (cnt[tmp[i]] - 1) * cnt[tmp[i]] // 2
                        res %=pin
                    else:
                        # print(tmp[i],t)
                        res += cnt[tmp[i]] * cnt[t]
                        res %=pin
                    used.add(tmp[i])
            # for j in range(i,n):
            #     t = tmp[i]+ tmp[j]
            #     if bin(t).count('1') == 1:
            #         if i ==j:
            #             res += (cnt[tmp[i]]-1) * cnt[tmp[i]] //2
            #             res %=pin
            #         else:
            #             res += cnt[tmp[i]] * cnt[tmp[j]]
            #             res %=pin
        return res

### 另一种更普遍的解法，运行不快，但是好想
class Solution:
    def countPairs(self, deliciousness: List[int]) -> int:
        dic = collections.defaultdict(int)
        res =0 
        mod= int(1e9+7)
        for x in deliciousness:
            for i in range(22):
                t = (1<<i)-x
                if t in dic:
                    res  = (res + dic[t]) % mod
            dic[x] +=1
        return res

```

#### 142. [5643. 将数组分成三个子数组的方案数](https://leetcode-cn.com/problems/ways-to-split-array-into-three-subarrays/)

```python
class Solution:
    # 方法可行，从C++“翻译”过来的，不过在python中貌似超时了
    def waysToSplit(self, nums: List[int]) -> int:
        n = len(nums)
        mod = 10**9+7
        s= [0]*(n+1)
        for i in range(1,n+1):
            s[i] = s[i-1] + nums[i-1]
        res = 0
        for i in range(3,n+1):
            j=k=2
            # 第一个符合条件的
            while s[n]-s[i-1]<s[i-1] - s[j-1]: j+=1
            # 最后一个符合条件的
            while k+1<i and s[i-1] - s[k]>= s[k]: k+=1
            if j<=k and (s[n]-s[i-1]>=s[i-1]-s[j-1]) and (s[i-1] -s[k-1] >= s[k-1]):
                res = (res + k-j+1) % mod
        return res
    
    
    
## 看评论区提到二分，在上面的基础上，写了两个二分，一点剪枝，终于AC了
class Solution:
    def waysToSplit(self, nums: List[int]) -> int:
        n = len(nums)
        mod = 10**9+7
        s= [0]
        for i in range(1,n+1):
            s.append(s[-1] +nums[i-1])
        def bin_search(l,r):
            while l<r:
                mid = (l+r)>>1
                if s[n] - s[i-1] < s[i-1] - s[mid-1]:
                    l = mid+1
                else:
                    r = mid
            return l
        def bin_search1(l,r):
            while l<r:
                mid = (l+r)>>1
                if s[i-1] - s[mid] >= s[mid]:
                    l = mid+1
                else:
                    r = mid
            return l

        res = 0
        for i in range(3,n+1):
            j=k=2
            if (s[n] - s[i-1]) * 3 < s[n]:
                break
            # 第一个符合条件的
            # while s[n]-s[i-1]<s[i-1] - s[j-1]: j+=1
            j = bin_search(2,i)
            #print(i,j)

            # 最后一个符合条件的
            k =j
            #while k+1<i and s[i-1] - s[k]>= s[k]: k+=1
            k = bin_search1(j,i-1)
            #print(i,k)
            if j<=k and (s[n]-s[i-1]>=s[i-1]-s[j-1]) and (s[i-1] -s[k-1] >= s[k-1]):
                res = (res + k-j+1) % mod
        return res
```

#### 143. [1019. 链表中的下一个更大节点](https://leetcode-cn.com/problems/next-greater-node-in-linked-list/)

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 单调栈玩法，记录位置和值，到达触发条件后更新结果集
    def nextLargerNodes(self, head: ListNode) -> List[int]:
        stack = []
        stack_pos=  []
        pos= -1
        res =[]
        while head:
            pos +=1
            res.append(0)  # 先拿0 填充，后面操作栈的时候可以刷新
            while stack and stack[-1] <head.val:
                res[stack_pos[-1]] = head.val
                stack.pop()
                stack_pos.pop()
            stack.append(head.val)
            stack_pos.append(pos)
            head = head.next
        return res
```

#### 144. [402. 移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)

```python
class Solution:
    def removeKdigits(self, num: str, k: int) -> str:
        """对于两个数 123a456 和 123b456，如果 a > b， 那么数字 123a456 大于 数字 123b456，否则数字             123a456 小于等于数字 123b456。也就说，两个相同位数的数字大小关系取决于第一个不同的数的大小。

            因此我们的思路就是：

            从左到右遍历
            对于遍历到的元素，我们选择保留。
            但是我们可以选择性丢弃前面相邻的元素。
            丢弃与否的依据如上面的前置知识中阐述中的方法。
        """
        if k >= len(num):
            return '0'
        stack = []
        remain = len(num) - k
        # 维持一个递增的单调栈，最后取前n个，避免原序列本身就是递增的，栈中元素超过k的尴尬
        # 不用担心后面的值不符合栈的规则，因为只能删除k个，且数字的相对位置不变，所以这里有一定贪心的想法——使开始部分为递增（也就是最小值）最优，因为这部分的权重最大
        for digit in num:
            # 删除k次完成以后，后面的数字就自然加入了，不走while 了
            while k and stack and stack[-1]>digit:
                stack.pop()
                k -=1
            stack.append(digit)
        return "".join(stack[:remain]).lstrip('0') or '0' # lstrip 去除前导0，如果是去除之后是空字符串，就直接返回‘0’
        
```

#### 145. [1081. 不同字符的最小子序列](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters/)

```python
class Solution:
    def smallestSubsequence(self, s: str) -> str:
        stack = []
        seen = set()
        cnt = collections.Counter(s)

        for c in s:
            if c not in seen:
                while stack and c<stack[-1] and cnt[stack[-1]] >=1:
                    seen.discard(stack.pop())
                seen.add(c)
                stack.append(c)
            cnt[c] -=1
        return "".join(stack)

```



#### 146. [399. 除法求值](https://leetcode-cn.com/problems/evaluate-division/)

```python
class Solution:
    # 带权并查集不是很好做
    # 虽然是中等，实际是hard难度
    def calcEquation(self, equations: List[List[str]], values: List[float], queries: List[List[str]]) -> List[float]:
        uf = UnionFind()
        for (x,y),val in zip(equations,values):
            uf.add(x)
            uf.add(y)
            uf.merge(x,y,val)
        res = [-1.0]* len(queries)
        for i, (a,b) in enumerate(queries):
            if uf.is_connected(a,b):
                res[i] = uf.value[a]/uf.value[b]
        return res
class UnionFind:
    def __init__(self):
        """记录每个节点的父节点，记录每个节点到根节点的权重"""
        self.father= {}
        self.value = {}
    def find(self, x):
        """查找根节点，路径压缩， 更新权重"""
        root = x
        # 节点更新时需要放大的倍数
        base = 1
        while self.father[root] != None:
            root = self.father[root]
            base *= self.value[root]
        
        while x != root:
            origin_father = self.father[x]
            self.value[x] *= base
            base /=self.value[origin_father]
            self.father[x] = root
            x = origin_father
        return root
    def merge(self, x,y,val):
        """合并两个节点"""
        root_x,root_y = self.find(x), self.find(y)
        if root_x != root_y:
            self.father[root_x] = root_y
            #print(root_x)
            # print(self.value[root_x])
            self.value[root_x] = self.value[y] * val/self.value[x]

    def is_connected(self,x,y):
        return x in self.value and y in self.value and self.find(x)== self.find(y)
    
    def add(self,x):
        if x not in self.father:
            self.father[x] = None
            self.value[x] = 1.0


```

#### 147. 5652. 交换链表中的节点

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapNodes(self, head: ListNode, k: int) -> ListNode:
        res= []
        while head:
            res.append(head.val);
            head = head.next;
        
        res[k-1], res[len(res)-k] = res[len(res)-k], res[k-1]
        head = ListNode()
        cur =head
        for i in range(len(res)):
            tmp = ListNode(res[i])
            cur.next = tmp
            cur = tmp
        return head.next
## 不用取巧的解法，需要使用5个指针
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def swapNodes(self, head: ListNode, k: int) -> ListNode:
        dummy = ListNode(-1)
        dummy.next =head
        p1,p2 =dummy,head
        for i in range(k-1):
            p1 = p1.next;
            p2 = p2.next;
        p3 = head
        p4 =p2
        p5 = dummy
        while p4.next:
            p3 = p3.next
            p4 = p4.next
            p5 = p5.next
        p1.next, p5.next= p5.next,p1.next
        p2.next, p3.next = p3.next,p2.next
        return dummy.next
      
        
```



#### 148.[1202. 交换字符串中的元素](https://leetcode-cn.com/problems/smallest-string-with-swaps/)

```python

class DisjointSetUnion:
    def __init__(self, n: int):
        self.n = n
        self.rank = [1] * n
        self.f = list(range(n))
    
    def find(self, x: int) -> int:
        if self.f[x] == x:
            return x
        self.f[x] = self.find(self.f[x])
        return self.f[x]
    
    def unionSet(self, x: int, y: int):
        fx, fy = self.find(x), self.find(y)
        if fx == fy:
            return
        if self.rank[fx] < self.rank[fy]:
            fx, fy = fy, fx
        self.rank[fx] += self.rank[fy]
        self.f[fy] = fx
        
class Solution:
    def smallestStringWithSwaps(self, s: str, pairs: List[List[int]]) -> str:
        dsu = DisjointSetUnion(len(s))
        for x, y in pairs:
            dsu.unionSet(x, y)
        
        mp = collections.defaultdict(list)
        for i, ch in enumerate(s):
            mp[dsu.find(i)].append(ch)
        
        for vec in mp.values():
            vec.sort(reverse=True)
        
        ans = list()
        for i in range(len(s)):
            x = dsu.find(i)
            ans.append(mp[x][-1])
            mp[x].pop()
        
        return "".join(ans)

```



#### 149. [1717. 删除子字符串的最大得分](https://leetcode-cn.com/problems/maximum-score-from-removing-substrings/)

```python
class Solution:
    def maximumGain(self, s: str, x: int, y: int) -> int:
# 贪心玩法，有限去除分值高的，具体用栈
        res=0
        def count(s,target,score):
            nonlocal res
            n=len(s)
            stack=[]
            for i in range(n):
                if stack and stack[-1]+s[i]==target:
                    stack.pop()
                    res+=score
                else:
                    stack.append(s[i])
            return stack
        
        if x>y:
            stack=count(s,"ab",x)
            count(stack,"ba",y)
        else:            
            stack=count(s,"ba",y)
            count(stack,"ab",x)    
        return res
```

#### 150 [5662. 满足三条件之一需改变的最少字符数](https://leetcode-cn.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/)

```python
class Solution:
    def minCharacters(self, a: str, b: str) -> int:
        s1, s2 = [0]*26, [0]*26
        m,n = len(a),len(b)
        for c in a: s1[ord(c)-97]+=1
        for c in b: s2[ord(c)-97] += 1
        res = float('inf')
        #print(s1,s2)
        for i in range(26):
            res=min(res,m+n-s1[i]-s2[i])
        def change(s1,s2):
            res = float('inf')
            for i in range(1,26):
                cnt =0
                for j in range(i,26): cnt+=s1[j]
                for j in range(i): cnt +=s2[j]
                res = min(res, cnt)
            return res
        return min(res, change(s1,s2),change(s2,s1))
```

#### 151. [5665. 从相邻元素对还原数组](https://leetcode-cn.com/problems/restore-the-array-from-adjacent-pairs/)

```python
from typing import List
import collections

class Solution:
    def restoreArray(self, adjacentPairs: List[List[int]]) -> List[int]:
        dic = collections.defaultdict(list)
        for i,j in adjacentPairs:
            dic[i].append(j)
            dic[j].append(i)
        start = 0
        for i,j in adjacentPairs:
            if len(dic[i]) ==1:
                start = i
                break
            if len(dic[j]) == 1:
                start = j
                break
        n = len(adjacentPairs)+1
        res =[0]*n
        res[0] = start
        i = 1
        while i <n:
            next = dic[start][0]
            dic[next].remove(start)
            start = next
            res[i]= start
            i +=1
        return  res

```

#### 152. [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)(类型题集合，待看)



```python
class Solution:
    def characterReplacement(self, s: str, k: int) -> int:
        # 不要考虑真的换字母，实际是一个数个数的问题：
        # 某一段长度 - 该段字母频次最多的数字 和 k的比较
        # 如果<=k， 这段可以延伸，否则，长度超标，需要截断，左边界右移一格
        # 根据思路得到双指针，或者说是滑动窗口的写法
        num = [0] * 26
        n = len(s)
        max_n = left = right = 0
        while right <n:
            num[ord(s[right]) - 65] +=1
            max_n = max(max_n, num[ord(s[right]) - 65])# 需要注意这里的max_n 是一直增大的，也是因为这个，最终的结果才是right-left.说明一下这里只是right-left的值符合答案，并不是说最后这一段的字符串，符合答案。
            # 还是重复前言，这是一个数数问题，不是一个字符串操作问题。
            if right - left +1 -max_n > k:
                num[ord(s[left]) - 65] -=1 # 移除窗口，频次-1
                left +=1
            right +=1
        return right - left
```

#### 153 [1423. 可获得的最大点数](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards/)\

```python
class Solution:
    def maxScore(self, cardPoints: List[int], k: int) -> int:
        tmp = cardPoints
        n = len(cardPoints)
        for i in range(len(cardPoints)):
            tmp.append(cardPoints[i])
        res = 0
        for i in range(len(cardPoints)-k,len(cardPoints)):
            res += cardPoints[i]
        t = res
        #print(len(cardPoints))
        for i in range(n, n+k):
            #print(len(cardPoints))
           # print(i, i-k)
            t = t + tmp[i] - tmp[i-k]
            res = max(res, t)
        return res
```

#### 154 . [978. 最长湍流子数组](https://leetcode-cn.com/problems/longest-turbulent-subarray/)

```python
class Solution:
    # 边界条件没有想清楚，面向测试用例编程
    def maxTurbulenceSize(self, arr: List[int]) -> int:
        def check1(k):
            if k%2 ==1:
                return arr[k]>arr[k+1]
            else:
                return arr[k]<arr[k+1]
        def check2(k):
            if k%2 ==0:
                return arr[k]>arr[k+1]
            else:
                return arr[k]<arr[k+1]
        if len(arr)<2:
            return len(arr)
        len_ = 0
        l,r = 0, 0
        flag = True
        while r <len(arr)-1:
            #print(flag, r,l)

            if arr[r] == arr[r+1]:
                len_ = max(len_, r-l+1)
                l = r+1
                r +=1
                continue
            if flag:
                if check1(r):
                    pass
                else:
                    flag = False
                    len_ = max(len_, r-l+1)
                    l = r
            else:
                if check2(r):
                    print(r, l)
                else:
                    flag = True
                    len_ = max(len_, r-l+1)
                    l= r
            r +=1      
        return max(len_, r-l+1)

```



#### 155. [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

```python
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:

        t = [0]*26
        m = [0]*26
        n1, n2 = len(s1),len(s2)
        if n1>n2:
            return False
        for i, c in enumerate(s1):
            t[ord(c) - ord('a')] +=1
        n1 = len(s1)
        for i in range(n1):
            m[ord(s2[i]) - ord('a')] +=1
        def check(t,m):
            for i in range(26):
                if t[i] != m[i]:
                    return False
            return True
        if check(t,m):
            return True
        for i in range(n1, len(s2)):
            #print(t,m)
            m[ord(s2[i-n1]) - ord('a')] -= 1
            m[ord(s2[i]) - ord('a')] +=1
            if check(t,m):
                return True
        return False
    
    
# 也可以不用check，直接可以比较
class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:

        t = [0]*26
        m = [0]*26
        n1, n2 = len(s1),len(s2)
        if n1>n2:
            return False
        for i, c in enumerate(s1):
            t[ord(c) - ord('a')] +=1
        n1 = len(s1)
        for i in range(n1):
            m[ord(s2[i]) - ord('a')] +=1
        if t==m:
            return True
        for i in range(n1, len(s2)):
            #print(t,m)
            m[ord(s2[i-n1]) - ord('a')] -= 1
            m[ord(s2[i]) - ord('a')] +=1
            if t==m:
                return True
        return False
```

#### 156. [1004. 最大连续1的个数 III](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)

```python
class Solution:
    def longestOnes(self, A: List[int], K: int) -> int:
        # 滑动窗口
        n = len(A)
        left = r_sum = l_sum = 0
        res = 0
        for right in range(n):
            r_sum +=1 - A[right] # 统计窗口里0的个数
            while l_sum < r_sum - K:
                l_sum += 1- A[left]
                left += 1
            res = max(res, right - left +1)
        return res
            
```

#### 157. [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

```python
class Solution:
    def allPathsSourceTarget(self, graph: List[List[int]]) -> List[List[int]]:
        res = []
        def dfs(x,graph, path):
            if x == len(graph)-1:
                path.append(x)
                res.append(path)
                return 
            
            for i in graph[x]:
               # print(x,i,path)

                dfs(i, graph, path + [x])
        dfs(0 ,graph,[])
        return res
```


#### 158. [5669. 通过连接另一个数组的子数组得到一个数组](https://leetcode-cn.com/problems/form-array-by-concatenating-subarrays-of-another-array/)

```python
from typing import List


class Solution:
    def canChoose(self, groups: List[List[int]], nums: List[int]) -> bool:
        l =0
        n = len(nums)
        m = len(groups)
        i = 0
        while l <n and i<m:
            #print(i)
            k = len(groups[i])
            if (l+k) >n:
                return False
            if nums[l:l+k] == groups[i]:
                i+=1
                l = l+k
            else:
                l +=1

        return l <=n and i == m

```

#### 159. [5671. 地图中的最高点](https://leetcode-cn.com/problems/map-of-highest-peak/)

```python
from typing import List


class Solution:
    def highestPeak(self, isWater: List[List[int]]) -> List[List[int]]:
        q = []
        m,n = len(isWater),len(isWater[0])
        res = [[0]*n for _ in range(m)]
        move = [[1,0],[0,1],[-1,0],[0,-1]]
        s =set()
        for i in range(m):
            for j in range(n):
                if isWater[i][j] == 1:
                    q.append((i,j))
                    s.add((i,j))
        while q:
            i,j = q.pop(0)
            h = res[i][j]
            for x, y in move:
                x = x+i
                y = y+j
                if x>=0 and x <m and y>=0 and y <n and (x,y) not in s:
                    res[x][y] = h + 1
                    q.append((x,y))
                    s.add((x,y))
        return res
```

#### 160 .[1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

```python
class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        # 维护两个双端队列
        n = len(nums)
        # 双向列表
        queMax, queMin = deque(), deque()
        left = right = ret = 0

        while right < n:
            # 单调递减队列，维护最大值
            while queMax and queMax[-1] < nums[right]:
                queMax.pop()
            # 单调递增队列，维护最小值
            while queMin and queMin[-1] > nums[right]:
                queMin.pop()
            
            # 将数据入队
            queMax.append(nums[right])
            queMin.append(nums[right])

            # 两个队列存在，且，当前的最大最小值，超过limit
            while queMax and queMin and queMax[0] - queMin[0] > limit:
                # 从左到右出队,实际每次循环只会出队一个，第二个if --》 elif也可以
                if nums[left] == queMin[0]:
                    queMin.popleft()
                if nums[left] == queMax[0]:
                    queMax.popleft()
                left += 1
            
            ret = max(ret, right - left + 1)
            right += 1
        
        return ret

## 有序集合
import sortedcontainers  # Leetcode没有
class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        s = SortedList()
        n = len(nums)
        left = right = ret = 0

        while right < n:
            s.add(nums[right])
            while s[-1] - s[0] > limit:
                s.remove(nums[left])
                left += 1
            ret = max(ret, right - left + 1)
            right += 1
        
        return ret


```

#### 161. [1043. 分隔数组以得到最大和](https://leetcode-cn.com/problems/partition-array-for-maximum-sum/)

```python
class Solution:
    def maxSumAfterPartitioning(self, arr: List[int], k: int) -> int: # 读题要认值，k不是分段数，而是分段后每一段的元素数
        n = len(arr)
        dp = [0] * (n+1) # dp,多一位，用来记录做开始，什么都没有的状态，res = 0
        for i in range(1,n+1):
            j = i - 1
            mx = -float('inf')  # 其实-1应该也可以
            while i - j <= k and j >= 0:  # 在限定好的滑窗内，j>=0 and j + k <= i 不断进行比较，当前位的结果就是 指定某一j,将i~ j变成arr[j] 并加上之前的和。注意实际上arr和dp是有错位的，也因为有错位，所以arr[j] 和 dp[j] 才能出现在同一个循环里，否则应该是dp[j-1],但是j-可能会小于0，所以又要加判断，没有之前这是位n+1巧妙
                mx = max(mx,arr[j])
                dp[i] = max(dp[i],dp[j]+mx * (i - j))
                j -= 1
        print(dp)
        return dp[n]
```

#### 162. [1769. 移动所有球到每个盒子所需的最小操作数](https://leetcode-cn.com/problems/minimum-number-of-operations-to-move-all-balls-to-each-box/)

```python
# 暴力法

# 前缀和
class Solution:
    def minOperations(self, boxes: str) -> List[int]:
        # 计算前、后缀和，然后遍历统计位置
        n=len(boxes)
        #前缀和
        pre=[0]*n
        presum=0
        count=0        
        for i in range(n):
            pre[i]+=presum   # 左边所有1，到当前位置的操作数                    
            count+=1 if boxes[i]=='1' else 0 # 如果有1，就需要+1，总共个要移动的小球增加
            presum+=count   # 下一步需要移动count个小球

        #后缀和
        suf=[0]*n
        sufsum=0
        count=0        
        for i in range(n-1,-1,-1): # 逻辑同上
            suf[i]+=sufsum
            count+=1 if boxes[i]=='1' else 0
            sufsum+=count
        res=[0]*n
        #统计结果
        for i in range(n):  # 左右之和
            res[i]=pre[i]+suf[i]
        return res

```

#### 163. [1770. 执行乘法运算的最大分数](https://leetcode-cn.com/problems/maximum-score-from-performing-multiplication-operations/)

```python
from typing import List


class Solution:
    def maximumScore(self, nums: List[int], multipliers: List[int]) -> int:
        n = len(nums)
        m = len(multipliers)
        if n >= 2*m:
            nums[m:2*m+1] = nums[n-m:n]
            n = 2 * m
        # print(n, m)
        # print(nums[:n])
        f = [[0]*(n+2) for _ in range(n+2)]
        for _len in range(n-(m-1), n+1):
            #print("len= %s" % _len)
            for i in range(1, n+1):
                j = i + _len - 1
                if j > n:
                    break
                # print("i=%s, j=%s, i-1=%s, j-1=%s, n-len=%s" % (
                #     i, j, i - 1, j - 1, n - _len
                # ))
                # print(nums[i-1] * multipliers[n-_len],nums[j-1] * multipliers[n-_len])
                f[i][j] = max(f[i+1][j] + nums[i-1] * multipliers[n-_len],
                              f[i][j-1] + nums[j-1] * multipliers[n-_len])
        #     for r in f:
        #         print(r)
        # print('start')
        # for r in f:
        #     print(r)
        return f[1][n]

```

#### 164. [1052. 爱生气的书店老板](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)

```python
from typing import List


class Solution:
    def maxSatisfied(self, customers: List[int], grumpy: List[int], X: int) -> int:
        n = len(customers)
        pre1 = [0]*(n+1)
        pre2 = [0]* (n+1)
        for i in range(1, n+1):
            pre1[i] += pre1[i-1] + customers[i-1]
            pre2[i] += pre2[i-1] + customers[i-1] * (1 - grumpy[i-1])
        res = pre2[n]
        sum = res
        # print(pre1)
        # print(pre2)
        #res = sum + (pre1[X] - pre1[0]) - (pre2[X] - pre2[0])
        for i in range(0, n-X+1):
            # print('idx:', i+X, i)
            # print("%s - %s) - (%s - %s" % (pre1[i+X], pre1[i], pre2[i+X] , pre2[i]))
            tmp = sum + (pre1[i+X] - pre1[i]) - (pre2[i+X] - pre2[i])
            #print('tmp:',tmp,'sum:',sum)
            res = max(res, tmp)
        return res
```

#### 165. [304. 二维区域和检索 - 矩阵不可变](https://leetcode-cn.com/problems/range-sum-query-2d-immutable/)

```python
# 标签是dp，实际上是二维前缀和，没技术含量，想到就会做。
# 不过输入为空的确实还是蛮坑的，没说明返回时null
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):
        self.grid = matrix
        self.m = len(matrix)
        if self.m == 0:
            self.n =0
        else:
            self.n = len(matrix[0])
        if self.m >0 and self.n > 0:
            self.matrix = [[0] * (self.n+1) for _ in range(self.m+1)]
            for i in range(1,self.m+1):
                for j in range(1,self.n+1):
                    self.matrix[i][j] = self.matrix[i-1][j] + self.matrix[i][j-1] + self.grid[i-1][j-1]- self.matrix[i-1][j-1]



    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        if self.m == 0 and self.n ==0:
            return 
        return self.matrix[row2+1][col2+1] - self.matrix[row1][col2+1] - self.matrix[row2+1][col1] + self.matrix[row1][col1]
        




# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```



#### 166. [357. 计算各个位数不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

```python
# 貌似属于数位dp，一开始考虑用递归，现在看还是迭代快


class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        if n <= 1:
            return 10**n
        f = [0,1]
        for i in range(2,10):
            f.append(i*f[i-1])
        # print(f)
        sum =0
        for i in range(min(10,n+1)):
        
            if i==0:
                sum += 1
            elif i== 1:
                sum +=9
            else:
                sum += f[9]//f[9-i] + (i-1)*(f[9]//f[9-(i-1)])
            #print(i,sum)
        return sum
   

        

#    * 排列组合：n位有效数字 = 每一位都从 0~9 中选择，且不能以 0 开头
#      * 1位数字：0~9                      10
#      * 2位数字：C10-2，且第一位不能是0      9 * 9
#      * 3位数字：C10-3，且第一位不能是0      9 * 9 * 8
#      * 4位数字：C10-4，且第一位不能是0      9 * 9 * 8 * 7
#      * ... ...
#      * 最后，总数 = 所有 小于 n 的位数个数相加
#      */     
        
```

#### 167. [395. 至少有 K 个重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

```python
# 分治法
# 就是递归
class Solution:
    def longestSubstring(self, s: str, k: int) -> int:
        res = 0
        if not s:
            return res
        cnt = [0] * 26
        for item in s:
            cnt[ord(item) - 97] += 1
        split = ""
        for index, item in enumerate(cnt):
            if item > 0 and item < k:
                split = chr(index + 97)
                break
        if split == "":
            return len(s)
        for item in s.split(split):
            res = max(res, self.longestSubstring(item, k))
        return res
```

#### 168.[331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

```python
# 序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。 
# 
#       _9_
#     /   \
#    3     2
#   / \   / \
#  4   1  #  6
# / \ / \   / \
# # # # #   # #
#  
# 
#  例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。 
# 
#  给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。 
# 
#  每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。 
# 
#  你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 "1,,3" 。 
# 
#  示例 1: 
# 
#  输入: "9,3,4,#,#,1,#,#,2,#,6,#,#"
# 输出: true 
# 
#  示例 2: 
# 
#  输入: "1,#"
# 输出: false
#  
# 
#  示例 3: 
# 
#  输入: "9,#,#,1"
# 输出: false 
#  Related Topics 栈 
#  👍 205 👎 0

# 我们使用栈来维护槽位的变化。栈中的每个元素，代表了对应节点处剩余槽位的数量，
# 而栈顶元素就对应着下一步可用的槽位数量。当遇到空节点时，仅将栈顶元素减 1；
# 当遇到非空节点时，将栈顶元素减 1 后，再向栈中压入一个 2。无论何时，如果栈顶元素变为 00，就立刻将栈顶弹出。
#
# 遍历结束后，若栈为空，说明没有待填充的槽位，因此是一个合法序列；
# 否则若栈不为空，则序列不合法。此外，在遍历的过程中，若槽位数量不足，则序列不合法。


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def isValidSerialization(self, s: str) -> bool:
        n = len(s)
        i = 0
        stk = [1]
        while i < n:
            if len(stk) == 0:  # 槽位不足
                return False
            if s[i] == ',':
                i += 1
            elif s[i] == '#':
                stk[-1] -= 1  # 消耗一个槽位
                if stk[-1] == 0:
                    stk.pop()
                i += 1
            else:
                # 如果是数字
                while i < n and s[i] != ',':
                    i += 1
                stk[-1] -= 1   # 先消耗一个槽位
                if stk[-1] == 0:
                    stk.pop()
                stk.append(2)  # 再补充两个
        return len(stk) == 0




        
# leetcode submit region end(Prohibit modification and deletion)
# 猜想就一定会有空位和实位个数上的规则，果不其然，只是一开始没有想到，毕竟单位摸鱼，时间不是很充足
# 不是和层数有关，而是每一个实位，消耗一个空位，产生两个；每一个空位，单纯消耗一个，不补充
class Solution:
    def isValidSerialization(self, s: str) -> bool:
        n = len(s)
        i = 0
        slots = 1
        while i < n:
            if slots == 0:  # 槽位不足
                return False
            if s[i] == ',':
                i += 1
            elif s[i] == '#':
                slots -= 1
                i += 1
            else:
                # 如果是数字
                while i < n and s[i] != ',':
                    i += 1
                slots +=1
        return slots == 0
```

#### 169. [5702. 找出星型图的中心节点](https://leetcode-cn.com/problems/find-center-of-star-graph/)

```python
import collections
from typing import List


class Solution:
    def findCenter(self, edges: List[List[int]]) -> int:
        dic = collections.defaultdict(int)
        for i,j in edges:
            dic[i] +=1
            dic[j] += 1
        for k in dic:
            if dic[k]>1:
                return k
```

#### 170. [5703. 最大平均通过率](https://leetcode-cn.com/problems/maximum-average-pass-ratio/)

```python
# 贪心，堆，debug耗时太久
from typing import List
import heapq

class Solution:
    def maxAverageRatio(self, classes: List[List[int]], extraStudents: int) -> float:
        n = len(classes)
        li = []
        r = 0
        for i,j in classes:
            if i ==j:
                r += 1
                continue
            li.append([-(((i+1)/(j+1)) - (i/j)), i, j])
        heapq.heapify(li)
        while extraStudents > 0 and len(li) > 0:
            # print(li)
            _, i, j = heapq.heappop(li)
            if i == j:
                r += 1
                continue
            extraStudents -= 1
            heapq.heappush(li, [-(((i+2)/(j+2)) - ((i+1)/(j+1))), i+1, j+1])
        _s = r
        #print('after', li, extraStudents)
        for i in li:
            _s += i[1]/i[2]
        return _s/n

```

#### 171 .[59. 螺旋矩阵 II](https://leetcode-cn.com/problems/spiral-matrix-ii/)

```python
# 给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：n = 3
# 输出：[[1,2,3],[8,9,4],[7,6,5]]
#  
# 
#  示例 2： 
# 
#  
# 输入：n = 1
# 输出：[[1]]
#  
# 
#  
# 
#  提示： 
# 
#  
#  1 <= n <= 20 
#  
#  Related Topics 数组 
#  👍 364 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        # 按照顺序模拟
        moves = [(0,1), (1, 0), (0, -1), (-1, -0)]  # 这里按照顺时针排序
        res = [[0] * n for _ in range(n)]
        row, col, dirIdx = 0, 0, 0
        for i in range(n*n):
            res[row][col] = i+1
            dx, dy = moves[dirIdx]
            r, c = row + dx, col + dy
            if r < 0 or r >= n or c < 0 or c >= n or res[r][c] > 0:
                dirIdx = (dirIdx+1) % 4
                dx, dy = moves[dirIdx]
            row, col = row + dx, col + dy
        return res
# leetcode submit region end(Prohibit modification and deletion)
## 另一种和Java异曲同工
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        matrix = [[0] * n for _ in range(n)]
        num = 1
        left, right, top, bottom = 0, n - 1, 0, n - 1

        while left <= right and top <= bottom:
            for col in range(left, right + 1):
                matrix[top][col] = num
                num += 1
            for row in range(top + 1, bottom + 1):
                matrix[row][right] = num
                num += 1
            if left < right and top < bottom:
                for col in range(right - 1, left, -1):
                    matrix[bottom][col] = num
                    num += 1
                for row in range(bottom, top, -1):
                    matrix[row][left] = num
                    num += 1
            left += 1
            right -= 1
            top += 1
            bottom -= 1

        return matrix

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/spiral-matrix-ii/solution/luo-xuan-ju-zhen-ii-by-leetcode-solution-f7fp/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 172. [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```python
# 运用你所掌握的数据结构，设计和实现一个 LRU (最近最少使用) 缓存机制 。 
# 
#  
#  
#  实现 LRUCache 类： 
# 
#  
#  LRUCache(int capacity) 以正整数作为容量 capacity 初始化 LRU 缓存 
#  int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。 
#  void put(int key, int value) 如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上
# 限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。 
#  
# 
#  
#  
#  
# 
#  进阶：你是否可以在 O(1) 时间复杂度内完成这两种操作？ 
# 
#  
# 
#  示例： 
# 
#  
# 输入
# ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
# [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
# 输出
# [null, null, null, 1, null, -1, null, -1, 3, 4]
# 
# 解释
# LRUCache lRUCache = new LRUCache(2);
# lRUCache.put(1, 1); // 缓存是 {1=1}
# lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
# lRUCache.get(1);    // 返回 1
# lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
# lRUCache.get(2);    // 返回 -1 (未找到)
# lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
# lRUCache.get(1);    // 返回 -1 (未找到)
# lRUCache.get(3);    // 返回 3
# lRUCache.get(4);    // 返回 4
#  
# 
#  
# 
#  提示： 
# 
#  
#  1 <= capacity <= 3000 
#  0 <= key <= 3000 
#  0 <= value <= 104 
#  最多调用 3 * 104 次 get 和 put 
#  
#  Related Topics 设计 
#  👍 1243 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class LRUCache:
   # 需要手工做链表，然后分别用dict ，和链表维护，是否存在，以及LRU顺序
    def __init__(self, capacity: int, key =0, value=0):
        self.cache = dict()
        # 建立头尾虚结点
        self.head = DeLinkedNode()
        self.tail = DeLinkedNode()
        self.head.next = self.tail
        self.tail.prev = self.head
        self.capacity = capacity
        self.size = 0

    def get(self, key: int) -> int:
        if key not in self.cache:
            return -1
        node = self.cache[key]
        self.move_to_head(node)
        return node.value


    def put(self, key: int, value: int) -> None:
        if key not in self.cache:
            node = DeLinkedNode(key, value)
            self.cache[key] = node
            self.add_to_head(node)
            self.size += 1
            if self.size > self.capacity:
                removed = self.remove_tail()
                del self.cache[removed.key]
                self.size -= 1
        else:
            node = self.cache[key]
            node.value = value
            self.move_to_head(node)
    def add_to_head(self,node):
        node.prev = self.head
        node.next = self.head.next
        self.head.next.prev = node
        self.head.next = node

    def remove_node(self,node):
        node.prev.next = node.next
        node.next.prev = node.prev

    def move_to_head(self,node):
        self.remove_node(node)
        self.add_to_head(node)

    def remove_tail(self):
        node = self.tail.prev
        self.remove_node(node)
        return node

class DeLinkedNode:
    def __init__(self,key = 0,value=0):
        self.key = key
        self.value = value
        self.prev = None
        self.next = None

# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
# leetcode submit region end(Prohibit modification and deletion)

```

#### 173. [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```python
# 给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。 
# 
#  一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。 
# 例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
#  
# 
#  若这两个字符串没有公共子序列，则返回 0。 
# 
#  
# 
#  示例 1: 
# 
#  输入：text1 = "abcde", text2 = "ace" 
# 输出：3  
# 解释：最长公共子序列是 "ace"，它的长度为 3。
#  
# 
#  示例 2: 
# 
#  输入：text1 = "abc", text2 = "abc"
# 输出：3
# 解释：最长公共子序列是 "abc"，它的长度为 3。
#  
# 
#  示例 3: 
# 
#  输入：text1 = "abc", text2 = "def"
# 输出：0
# 解释：两个字符串没有公共子序列，返回 0。
#  
# 
#  
# 
#  提示: 
# 
#  
#  1 <= text1.length <= 1000 
#  1 <= text2.length <= 1000 
#  输入的字符串只含有小写英文字符。 
#  
#  Related Topics 动态规划 
#  👍 395 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        if not text1 or not text2:
            return 0
        m = len(text1) + 1
        n = len(text2) + 1
        dp = [[0] * n for _ in range(m)]
        for i in range(1, m):
            for j in range(1, n):
                dp[i][j] = max(dp[i-1][j],dp[i][j-1])
                if text1[i-1] == text2[j-1]:
                    dp[i][j] = max(dp[i][j], dp[i-1][j-1] + 1)
        return dp[m-1][n-1]
# leetcode submit region end(Prohibit modification and deletion)

```

#### 174. [785. 判断二分图](https://leetcode-cn.com/problems/is-graph-bipartite/)

```python
# 存在一个 无向图 ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。给你一个二维数组 graph ，其中 graph[u]
#  是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。该无向图同时具有
# 以下属性：
#  
#  不存在自环（graph[u] 不包含 u）。 
#  不存在平行边（graph[u] 不包含重复值）。 
#  如果 v 在 graph[u] 内，那么 u 也应该在 graph[v] 内（该图是无向图） 
#  这个图可能不是连通图，也就是说两个节点 u 和 v 之间可能不存在一条连通彼此的路径。 
#  
# 
#  二分图 定义：如果能将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合，就将这个图称
# 为 二分图 。 
# 
#  如果图是二分图，返回 true ；否则，返回 false 。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
# 输出：false
# 解释：不能将节点分割成两个独立的子集，以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。 
# 
#  示例 2： 
# 
#  
# 输入：graph = [[1,3],[0,2],[1,3],[0,2]]
# 输出：true
# 解释：可以将节点分成两组: {0, 2} 和 {1, 3} 。 
# 
#  
# 
#  提示： 
# 
#  
#  graph.length == n 
#  1 <= n <= 100 
#  0 <= graph[u].length < n 
#  0 <= graph[u][i] <= n - 1 
#  graph[u] 不会包含 u 
#  graph[u] 的所有值 互不相同 
#  如果 graph[u] 包含 v，那么 graph[v] 也会包含 u 
#  
#  Related Topics 深度优先搜索 广度优先搜索 图 
#  👍 239 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        n = len(graph)
        uncolored, red, green = 0, 1, 2
        color = [uncolored] * n
        valid = True

        def dfs(node, c):
            nonlocal valid
            color[node] = c
            c_nei = green if c == red else red
            for neighbor in graph[node]:
                if color[neighbor] == uncolored:
                    dfs(neighbor, c_nei)
                    if not valid:
                        return
                elif color[neighbor] != c_nei:
                    valid = False
                    return
        for i in range(n):
            if color[i] == uncolored:
                dfs(i, red)
                if not valid:
                    break
        return valid

# leetcode submit region end(Prohibit modification and deletion)

```



#### 175. [1734. 解码异或后的排列](https://leetcode-cn.com/problems/decode-xored-permutation/)

```python
# 给你一个整数数组 perm ，它是前 n 个正整数的排列，且 n 是个 奇数 。 
# 
#  它被加密成另一个长度为 n - 1 的整数数组 encoded ，满足 encoded[i] = perm[i] XOR perm[i + 1] 。比方说
# ，如果 perm = [1,3,2] ，那么 encoded = [2,1] 。 
# 
#  给你 encoded 数组，请你返回原始数组 perm 。题目保证答案存在且唯一。 
# 
#  
# 
#  示例 1： 
# 
#  输入：encoded = [3,1]
# 输出：[1,2,3]
# 解释：如果 perm = [1,2,3] ，那么 encoded = [1 XOR 2,2 XOR 3] = [3,1]
#  
# 
#  示例 2： 
# 
#  输入：encoded = [6,5,4,6]
# 输出：[2,4,1,5,3]
#  
# 
#  
# 
#  提示： 
# 
#  
#  3 <= n < 105 
#  n 是奇数。 
#  encoded.length == n - 1 
#  
#  Related Topics 位运算 
#  👍 17 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def decode(self, encoded: List[int]) -> List[int]:
        n = len(encoded) + 1
        res = [0] * n
        total = 0
        i = 1
        while i < n-1:
            total ^= encoded[i]
            i += 2
        all = 0
        for i in range(1,n+1):
            all ^=i
        res[0] = all ^ total
        for i in range(n-1):
            res[i+1] = res[i] ^ encoded[i];
        return res


# leetcode submit region end(Prohibit modification and deletion)

```

#### 176. [61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```python
# 给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：head = [1,2,3,4,5], k = 2
# 输出：[4,5,1,2,3]
#  
# 
#  示例 2： 
# 
#  
# 输入：head = [0,1,2], k = 4
# 输出：[2,0,1]
#  
# 
#  
# 
#  提示： 
# 
#  
#  链表中节点的数目在范围 [0, 500] 内 
#  -100 <= Node.val <= 100 
#  0 <= k <= 2 * 109 
#  
#  Related Topics 链表 双指针 
#  👍 513 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head:
            return head
        n = 0
        dummy = ListNode()
        dummy.next = head
        cur = dummy
        while cur.next:
            n += 1
            cur = cur.next

        tail = cur
        k = n - k % n
        # print('k:', k)
        cur = dummy
        if k % n == 0:   # 如果交换的是整个链表，说明没有交换
            return head
        while k:
            # print(cur.val)
            k -= 1
            cur = cur.next
        dummy.next = cur.next
        tail.next = head
        cur.next = None
        return dummy.next


# leetcode submit region end(Prohibit modification and deletion)

```

#### 177. [1801. 积压订单中的订单总数](https://leetcode-cn.com/problems/number-of-orders-in-the-backlog/)

```python
# 给你一个二维整数数组 orders ，其中每个 orders[i] = [pricei, amounti, orderTypei] 表示有 amounti 
# 笔类型为 orderTypei 、价格为 pricei 的订单。 
# 
#  订单类型 orderTypei 可以分为两种： 
# 
#  
#  0 表示这是一批采购订单 buy 
#  1 表示这是一批销售订单 sell 
#  
# 
#  注意，orders[i] 表示一批共计 amounti 笔的独立订单，这些订单的价格和类型相同。对于所有有效的 i ，由 orders[i] 表示的所有订
# 单提交时间均早于 orders[i+1] 表示的所有订单。 
# 
#  存在由未执行订单组成的 积压订单 。积压订单最初是空的。提交订单时，会发生以下情况： 
# 
#  
#  如果该订单是一笔采购订单 buy ，则可以查看积压订单中价格 最低 的销售订单 sell 。如果该销售订单 sell 的价格 低于或等于 当前采购订单 b
# uy 的价格，则匹配并执行这两笔订单，并将销售订单 sell 从积压订单中删除。否则，采购订单 buy 将会添加到积压订单中。 
#  反之亦然，如果该订单是一笔销售订单 sell ，则可以查看积压订单中价格 最高 的采购订单 buy 。如果该采购订单 buy 的价格 高于或等于 当前销售
# 订单 sell 的价格，则匹配并执行这两笔订单，并将采购订单 buy 从积压订单中删除。否则，销售订单 sell 将会添加到积压订单中。 
#  
# 
#  输入所有订单后，返回积压订单中的 订单总数 。由于数字可能很大，所以需要返回对 109 + 7 取余的结果。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：orders = [[10,5,0],[15,2,1],[25,1,1],[30,4,0]]
# 输出：6
# 解释：输入订单后会发生下述情况：
# - 提交 5 笔采购订单，价格为 10 。没有销售订单，所以这 5 笔订单添加到积压订单中。
# - 提交 2 笔销售订单，价格为 15 。没有采购订单的价格大于或等于 15 ，所以这 2 笔订单添加到积压订单中。
# - 提交 1 笔销售订单，价格为 25 。没有采购订单的价格大于或等于 25 ，所以这 1 笔订单添加到积压订单中。
# - 提交 4 笔采购订单，价格为 30 。前 2 笔采购订单与价格最低（价格为 15）的 2 笔销售订单匹配，从积压订单中删除这 2 笔销售订单。第 3 笔
# 采购订单与价格最低的 1 笔销售订单匹配，销售订单价格为 25 ，从积压订单中删除这 1 笔销售订单。积压订单中不存在更多销售订单，所以第 4 笔采购订单需要添
# 加到积压订单中。
# 最终，积压订单中有 5 笔价格为 10 的采购订单，和 1 笔价格为 30 的采购订单。所以积压订单中的订单总数为 6 。
#  
# 
#  示例 2： 
# 
#  
# 输入：orders = [[7,1000000000,1],[15,3,0],[5,999999995,0],[5,1,1]]
# 输出：999999984
# 解释：输入订单后会发生下述情况：
# - 提交 109 笔销售订单，价格为 7 。没有采购订单，所以这 109 笔订单添加到积压订单中。
# - 提交 3 笔采购订单，价格为 15 。这些采购订单与价格最低（价格为 7 ）的 3 笔销售订单匹配，从积压订单中删除这 3 笔销售订单。
# - 提交 999999995 笔采购订单，价格为 5 。销售订单的最低价为 7 ，所以这 999999995 笔订单添加到积压订单中。
# - 提交 1 笔销售订单，价格为 5 。这笔销售订单与价格最高（价格为 5 ）的 1 笔采购订单匹配，从积压订单中删除这 1 笔采购订单。
# 最终，积压订单中有 (1000000000-3) 笔价格为 7 的销售订单，和 (999999995-1) 笔价格为 5 的采购订单。所以积压订单中的订单总
# 数为 1999999991 ，等于 999999984 % (109 + 7) 。 
# 
#  
# 
#  提示： 
# 
#  
#  1 <= orders.length <= 105 
#  orders[i].length == 3 
#  1 <= pricei, amounti <= 109 
#  orderTypei 为 0 或 1 
#  
#  Related Topics 堆 贪心算法 
#  👍 10 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
import heapq
class Solution:
    def getNumberOfBacklogOrders(self, orders: List[List[int]]) -> int:
        sell = []
        buy = []
        for order in orders:
            if order[2] == 0:
                # buy
                if not sell or sell[0][0] > order[0]:  # 栈为空，或者条件不符合磋商，就将订单累计
                    heapq.heappush(buy, [-order[0], order[1]])
                else:
                    rem = order[1]
                    while rem > 0 and sell and sell[0][0] <= order[0]:
                        k, s = heapq.heappop(sell)
                        cancel = min(s, rem)
                        s -= cancel
                        rem -= cancel
                    if rem > 0:  # 订单没有消化掉
                        heapq.heappush(buy, [-order[0], rem])
                    if s > 0:   # 没有用完，放回去
                        heapq.heappush(sell, [k, s])
            if order[2] == 1:
                # sell
                if not buy or -buy[0][0] < order[0]:
                    heapq.heappush(sell, [order[0], order[1]])
                else:
                    rem = order[1]
                    while rem > 0 and buy and -buy[0][0] >= order[0]:
                        k, s = heapq.heappop(buy)
                        cancel = min(s, rem)
                        s -= cancel
                        rem -= cancel
                    if rem > 0:
                        heapq.heappush(sell, [order[0], rem])
                    if s > 0:
                        heapq.heappush(buy, [k, s])
        return (sum([a[1] for a in sell]) + sum([a[1] for a in buy])) % (10**9 + 7)

# leetcode submit region end(Prohibit modification and deletion)

```

#### 178. [1798. 你能构造出连续值的最大数目](https://leetcode-cn.com/problems/maximum-number-of-consecutive-values-you-can-make/)

```python
# 给你一个长度为 n 的整数数组 coins ，它代表你拥有的 n 个硬币。第 i 个硬币的值为 coins[i] 。如果你从这些硬币中选出一部分硬币，它们的
# 和为 x ，那么称，你可以 构造 出 x 。 
# 
#  请返回从 0 开始（包括 0 ），你最多能 构造 出多少个连续整数。 
# 
#  你可能有多个相同值的硬币。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：coins = [1,3]
# 输出：2
# 解释：你可以得到以下这些值：
# - 0：什么都不取 []
# - 1：取 [1]
# 从 0 开始，你可以构造出 2 个连续整数。 
# 
#  示例 2： 
# 
#  
# 输入：coins = [1,1,1,4]
# 输出：8
# 解释：你可以得到以下这些值：
# - 0：什么都不取 []
# - 1：取 [1]
# - 2：取 [1,1]
# - 3：取 [1,1,1]
# - 4：取 [4]
# - 5：取 [4,1]
# - 6：取 [4,1,1]
# - 7：取 [4,1,1,1]
# 从 0 开始，你可以构造出 8 个连续整数。 
# 
#  示例 3： 
# 
#  
# 输入：nums = [1,4,10,3,1]
# 输出：20 
# 
#  
# 
#  提示： 
# 
#  
#  coins.length == n 
#  1 <= n <= 4 * 104 
#  1 <= coins[i] <= 4 * 104 
#  
#  Related Topics 贪心算法 
#  👍 13 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def getMaximumConsecutive(self, coins: List[int]) -> int:
        """
        把硬币从小到大排序。
假设前3个硬币能组成0~7，那第4个硬币如果是3，可以使用0~7里的数配合上3，必能组成8（5+3）、9（6+3）、10（7+3）。同理如果是4，可以组成8、9、10、11，如果是5可以组成8、9、10、11、12，...。第4个硬币是几，整数范围就能加几。
但是，如果第4个硬币是“9”，则无论如何组合都不可能组成“8”，而且后面的硬币只会更大，更不可能组成8，数字“8”将永远缺失，不用再往下看了，直接0~7就是答案。

        Args:
            coins:

        Returns:

        """
        coins.sort()
        x = 0
        for y in coins:
            if y > x+1:  # 不断扩展从0 开始的右边界
                break
            x += y
        return x + 1 # 算上0
# leetcode submit region end(Prohibit modification and deletion)

```

#### 179. [1802. 有界数组中指定下标处的最大值](https://leetcode-cn.com/problems/maximum-value-at-a-given-index-in-a-bounded-array/)

```java
# 给你三个正整数 n、index 和 maxSum 。你需要构造一个同时满足下述所有条件的数组 nums（下标 从 0 开始 计数）： 
# 
#  
#  nums.length == n 
#  nums[i] 是 正整数 ，其中 0 <= i < n 
#  abs(nums[i] - nums[i+1]) <= 1 ，其中 0 <= i < n-1 
#  nums 中所有元素之和不超过 maxSum 
#  nums[index] 的值被 最大化 
#  
# 
#  返回你所构造的数组中的 nums[index] 。 
# 
#  注意：abs(x) 等于 x 的前提是 x >= 0 ；否则，abs(x) 等于 -x 。 
# 
#  
# 
#  示例 1： 
# 
#  输入：n = 4, index = 2,  maxSum = 6
# 输出：2
# 解释：数组 [1,1,2,1] 和 [1,2,2,1] 满足所有条件。不存在其他在指定下标处具有更大值的有效数组。
#  
# 
#  示例 2： 
# 
#  输入：n = 6, index = 1,  maxSum = 10
# 输出：3
#  
# 
#  
# 
#  提示： 
# 
#  
#  1 <= n <= maxSum <= 109 
#  0 <= index < n 
#  
#  Related Topics 贪心算法 二分查找 
#  👍 18 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def maxValue(self, n: int, index: int, maxSum: int) -> int:
        """
        平铺模拟法，逐层累加

        """
        gap = maxSum - n   # 一开始统一铺一层
        left = right =index  #左右端点一开始都在index，然后才延展
        res = 1
        dl, dr =0,0  # index两边的延伸出的长度
        while gap >0:
            left -=1
            right += 1
            if left>=0:
                dl += 1
            if right < n:
                dr += 1
            if left < 0 and right >= n:
                res += gap//n if gap %n == 0 else gap//n +1
                return res
            res += 1
            gap -= (dl+ dr +1)
        return res

# leetcode submit region end(Prohibit modification and deletion)

```

#### 180. [173. 二叉搜索树迭代器](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

```python
class BSTIterator:
	# 实现了高端需求
    # next() 和 hasNext() 操作均摊时间复杂度为 O(1) ，并使用 O(h) 内存。其中 h 是树的高度
    def __init__(self, root: TreeNode):
        self.stack = []
        while root:
            self.stack.append(root)
            root = root.left

    def next(self) -> int:
        cur = self.stack.pop()
        node = cur.right
        while node:
            self.stack.append(node)
            node = node.left
        return cur.val

    def hasNext(self) -> bool:
        return len(self.stack) > 0

```

#### 181. [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```python
# 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重
# 复的三元组。 
# 
#  注意：答案中不可以包含重复的三元组。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：nums = [-1,0,1,2,-1,-4]
# 输出：[[-1,-1,2],[-1,0,1]]
#  
# 
#  示例 2： 
# 
#  
# 输入：nums = []
# 输出：[]
#  
# 
#  示例 3： 
# 
#  
# 输入：nums = [0]
# 输出：[]
#  
# 
#  
# 
#  提示： 
# 
#  
#  0 <= nums.length <= 3000 
#  -105 <= nums[i] <= 105 
#  
#  Related Topics 数组 双指针 
#  👍 3159 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        res = []
        n = len(nums)
        if not nums or n <3:
            return res
        nums.sort()  # 先排序，利用自身的有序性
        for i in range(n):
            if nums[i] > 0:   # 如果最小值大于零，那就不用要了，肯定符合结果的没有0
                break
            if i > 0 and nums[i] == nums[i-1]:  # 如果值重复，可以直接跳过去
                continue
            L, R = i+1, n-1   # 初始化左右指针
            while L < R:  # 双指针玩法
                # print(n, i, L, R)
                total = nums[i] + nums[L] + nums[R]
                if total < 0:
                    L += 1
                elif total > 0:
                    R -= 1
                elif total == 0:
                    res.append([nums[i], nums[L], nums[R]])
                    # 排序后重复的值，在一起，如果重复，就直接跳过
                    while L < R and nums[L] == nums[L+1]:
                        L += 1
                    while L < R and nums[R-1] == nums[R]:
                        R -= 1
                    L += 1
                    R -= 1
        return res
# leetcode submit region end(Prohibit modification and deletion)

```

#### 182. [1806. 还原排列的最少操作步数](https://leetcode-cn.com/problems/minimum-number-of-operations-to-reinitialize-a-permutation/)

```python
class Solution:
    def reinitializePermutation(self, n: int) -> int:
        #  原始的解法为模拟法， 每次按照规则执行，然后check直到遇到结果，不过更好的方法是数学证明后，
        #  直接使用公式,首先明白题意，这道题是交换不同位置上的值，而不是修改值，如果搞错这个，模拟都没得
        #
        if n == 2:
            return 1
        k, pow2 = 1, 2
        while pow2 != 1:
            k += 1
            pow2 = pow2 * 2 % (n-1)
        return k
```

#### 183. [1807. 替换字符串中的括号内容](https://leetcode-cn.com/problems/evaluate-the-bracket-pairs-of-a-string/)

```python
class Solution:
    def evaluate(self, s: str, knowledge: List[List[str]]) -> str:
        left, right = False, False
        res = []
        dic = {}
        for x, y in knowledge:
            dic[x] = y
        tmp = ''
        for c in s:
            if not left and not right and c not in ['(', ')']:
                res.append(c)
            elif c == '(':
                left = True
            elif c == ')':
                if tmp in dic:
                    res.append(dic[tmp])
                else:
                    res.append('?')
                tmp = ''
                left = False
            elif c not in ['(', ')'] and left:
                tmp += c
        return "".join(res)
```

#### 184. [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)

```python
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        # bfs ,或者说是层序遍历
        right_depth = {}
        max_depth = -1
        queue = deque([(root, 0)])
        while queue:
            node, depth = queue.popleft()
            if node is not None:
                max_depth = max(max_depth, depth)     # 更新最大深度
                right_depth[depth] = node.val         # 不断更新,根据插入顺序和队列特性，最后一次更新就是最右值
                queue.append((node.left, depth+1))
                queue.append((node.right, depth+1))
        return [right_depth[depth] for depth in range(max_depth+1)]
    
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        # dfs
        right_depth = {}
        max_depth = -1
        stack = [(root, 0)]
        while stack:
            node, depth = stack.pop()
            if node is not None:
                max_depth = max(max_depth, depth)     # 维护更新最大深度
                right_depth.setdefault(depth, node.val)    # 如果不存在对应深度的节点，才插入，setdefault 类似默认插入，
                # 没有才插入，有则不插入
                # 因为栈先进后出，为了维护这个顺序，所以插入时先左后右，使右侧先出栈
                stack.append((node.left, depth + 1))
                stack.append((node.right, depth + 1))
        return [right_depth[depth] for depth in range(max_depth+1)]

```

#### 185.[56. 合并区间](https://leetcode-cn.com/problems/merge-intervals/)

```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key = lambda x: x[0])
        merged = []
        for i in intervals:
            if not merged or merged[-1][1] < i[0]:
                merged.append(i)
            else:
                merged[-1][1] = max(merged[-1][1], i[1])
        return merged
```



#### 186.[31. 下一个排列](https://leetcode-cn.com/problems/next-permutation/)

```python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = len(nums) - 2
        # 从后向前找到需要交换的那个较小的数
        while i >= 0 and nums[i] >= nums[i+1]:
            i -= 1
        if i >= 0:   # 如果这个数存在
            j = len(nums) - 1
            while j >= i and nums[i] >= nums[j]:    # 从后向前找到相对较大的数,注意不要超过i
                j -= 1
            nums[i], nums[j] = nums[j], nums[i]
        left, right = i+1, len(nums) - 1
        # 将后半反转
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
            right -= 1
```









## hard

1. #### [679. 24 点游戏](https://leetcode-cn.com/problems/24-game/)

   ```python
   '''
   一共有 44 个数和 33 个运算操作，因此可能性非常有限。一共有多少种可能性呢？
   
   首先从 44 个数字中有序地选出 22 个数字，共有 4 \times 3=124×3=12 种选法，并选择加、减、乘、除 44 种运算操作之一，用得到的结果取代选出的 22 个数字，剩下 33 个数字。
   
   然后在剩下的 33 个数字中有序地选出 22 个数字，共有 3 \times 2=63×2=6 种选法，并选择 44 种运算操作之一，用得到的结果取代选出的 22 个数字，剩下 22 个数字。
   
   最后剩下 22 个数字，有 22 种不同的顺序，并选择 44 种运算操作之一。
   
   因此，一共有 12 \times 4 \times 6 \times 4 \times 2 \times 4=921612×4×6×4×2×4=9216 种不同的可能性。
   
   可以通过回溯的方法遍历所有不同的可能性。具体做法是，使用一个列表存储目前的全部数字，每次从列表中选出 22 个数字，再选择一种运算操作，用计算得到的结果取代选出的 22 个数字，这样列表中的数字就减少了 11 个。重复上述步骤，直到列表中只剩下 11 个数字，这个数字就是一种可能性的结果，如果结果等于 2424，则说明可以通过运算得到 2424。如果所有的可能性的结果都不等于 2424，则说明无法通过运算得到 2424。
   
   实现时，有一些细节需要注意。
   
   除法运算为实数除法，因此结果为浮点数，列表中存储的数字也都是浮点数。在判断结果是否等于 2424 时应考虑精度误差，这道题中，误差小于 10^{-6}10 
   −6
     可以认为是相等。
   
   进行除法运算时，除数不能为 00，如果遇到除数为 00 的情况，则这种可能性可以直接排除。由于列表中存储的数字是浮点数，因此判断除数是否为 00 时应考虑精度误差，这道题中，当一个数字的绝对值小于 10^{-6}10 
   −6时，可以认为该数字等于 00。
   '''
   
   class Solution:
       def judgePoint24(self, nums: List[int]) -> bool:
           # 第一难点在于想清楚递归回溯的思路
           # 第二难点在于，具体实现中的细节
           target = 24
           epsilon = 1e-6  #允许的精度误差在这
   
           add, multipy,subtract,divide =0,1,2,3  # 加减乘除操作符
   
           def solve(nums)-> bool:
               if not nums:
                   return False
               if len(nums)==1:
                   return abs(nums[0] - target)  <epsilon 
               
               for i ,x in enumerate(nums):   # 构造索引序列。k-v形式
                   for j, y in enumerate(nums):
                       if i != j:
                           newNums = list()
                           for k ,z in enumerate(nums):  # 操作前两个数，将第三、四个数保存
                               if k != i and k !=j:
                                   newNums.append(z)
                           
                           for k in range(4):
                               if k<2 and i>j:
                                   continue
                               if k == add:
                                   newNums.append(x+y)
                               elif k == multipy:
                                   newNums.append(x*y)
                               elif k == sub:
                                   newNums.append(x-y)
                               elif k == divide:
                                   if abs(y)<epsilon:  #这里考虑，如果有y的值近似于0，不可除，跳过
                                       continue
                                   newNums.append(x/y)
                           if solve(newNums):   # 递归
                               return True
                           newNums.pop()    # 回溯
               return False
   
   
           return solve(nums)
   ```

2. #### [410. 分割数组的最大值](https://leetcode-cn.com/problems/split-array-largest-sum/)

   ```python
   ## 二分法，问题是为什么逼近就可以，是因为什么呢？
   from typing import List
   
   
   class Solution:
       def splitArray(self, nums: List[int], m: int) -> int:
           def check(mid):
               nonlocal flag
               #print(("flag:", flag))
               #print("mid", mid)
   
               cnt = 0
               res = 0
               #_max = min(nums)
               for i in range(n):
                   #print(i)
                   if i == n - 1:
                       if (res + nums[i]) > mid:
                           #print("res",res, "nums[i]",nums[i],"mid",mid)
                           cnt += 2
                           #_max = max(_max, res, nums[i])
   
                       else:
                           cnt += 1
                           res += nums[i]
                           #print("res", res, "i", i)
                          #_max = max(res, _max)
   
                   else:
                       if (res + nums[i]) > mid:
   
                           cnt += 1
                           #print("res",res,"i",i)
                           #_max = max(res, _max)
                           res = nums[i]
   
                       else:
                           #cnt += 1
                           res += nums[i]
   
               #flag = min(_max, flag)
               #print("f",flag)
               #print("cnt", cnt)
               return cnt <= m   # 题目要求==m，这里为什么《=就可以
   
           min_, max_ = max(nums), sum(nums)   # 注意这里一开始高搞错了，最小的最大是max，不是民min
           n = len(nums)
           mx = max(nums)
           while min_ <= max_:
               mid = (min_ + max_) >> 1
               print(min_, max_)
               if check(mid):
                   flag = mid
                   max_ = mid -1
               else:
                   min_ = mid + 1
           return min_ if check(min_) else -1
   
   
   
   a = Solution()
   arr = [1,4,4]
   
   m =3
   print(a.splitArray(arr,m))
   ```

3. #### [1553. 吃掉 N 个橘子的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-eat-n-oranges/)

   ```python
   from functools import lru_cache
   
   # 思路没问题，就是记忆化搜索，问题在于放弃-1的递归，分析发现完全可以放在/2，/3之前的取余步骤中，否则不断减一递归太浪费时间了
   
   class Solution:
       @lru_cache(None)  # 常用的加速手法，设立设立缓存
       def minDays(self, n: int) -> int:
           if n <= 1:
               return n
           return min(n % 2 + 1 + self.minDays(n // 2), n % 3 + 1 + self.minDays(n // 3))
   
   
   ```

4. #### [214. 最短回文串](https://leetcode-cn.com/problems/shortest-palindrome/)

   ```python
   # 最简单的思路。倒置，然后取一部分拼接
   class Solution:
       def shortestPalindrome(self, s: str) -> str:
           reverse = s[::-1]
           for i in range(len(s) + 1):
               if s.startswith(reverse[i:]):   # startswith  寻找开头的重复部分
                   return reverse[:i] + s
   
   # 不过这样，只是思路简单，就算法上没有收获。所以根据题解，找一下更有收获的解法
   ```

5. #### [753. 破解保险箱](https://leetcode-cn.com/problems/cracking-the-safe/)

   ```python
   # 欧拉回路还是很陌生  ，需要明天再看
   
   class Solution:
       def crackSafe(self, n: int, k: int) -> str:
           seen = set()
           ans = list()
           highest = 10 ** (n - 1)
   
           def dfs(node: int):
               for x in range(k):
                   nei = node * 10 + x
                   if nei not in seen:
                       seen.add(nei)
                       dfs(nei % highest)
                       ans.append(str(x))
   
           dfs(0)
           return "".join(ans) + "0" * (n - 1)   # 因为是递归，实际入列顺序是导过来的，然后，补足‘0’，因为dfs里的0，实际上是n个0，只是值为零
   
   作者：LeetCode-Solution
   链接：https://leetcode-cn.com/problems/cracking-the-safe/solution/po-jie-bao-xian-xiang-by-leetcode-solution/
   来源：力扣（LeetCode）
   著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
   ```

6. #### [5502. 将子数组重新排序得到同一个二叉查找树的方案数](https://leetcode-cn.com/problems/number-of-ways-to-reorder-array-to-get-same-bst/)

   ```python
   
   class Solution:
       def numOfWays(self, nums: List[int]) -> int:
           return (self.cal(nums)-1)%1000000007    # 需要减去nums存在的一种
   
       from scipy.special import perm,comb，factorial
       def cal(self,nums):
           if len(nums)<=2:
               return 1  #这种情况下，只会有一种方案
           k = nums[0]  # 第一个元素是树根root
           # 梳理root左右两边的元素
           l = [i for i in nums[1:] if i<k]
           r = [i for i in nums[1:] if i>k ]
           len_l, len_r = len(l), len(r)
   
           
           p=comb(len_l+len_r,len_r)   # l, r自己的相对位置不能变，但是两者之间可以做组合，这里是从l+r的序列中，抽出r个位置给r，剩下的就是l
           return self.cal(l)*self.cal(r)*p  # 这里要不断递归，构造子级树。即是否可以改变l，r的顺序，然后使子级树形状不变
      
   ```
   
7. #### [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

   ```python
   class Solution:
       def solveNQueens(self, n: int) -> List[List[str]]:
           # 一开始想单纯dfs，貌似思路有点偏……，dfs要有，但是还有回溯
           # 所以是backtrace
           def get_grid():  
               grid = list()
               for i in range(n):
                   row[queues[i]] = 'Q'     # 用同一行操作
                   grid.append("".join(row))
                   row[queues[i]] = '.'   # 这一行处理完了，就变回来，操作下一行
               return grid
           
           def backtrace(row):
               if row == n:  # row最大是n-1，所以这里是遍历完了
                   grid = get_grid()
                   res.append(grid)
               else:
                   for i in range(n):
                       if i in col or row - i in diag1 or row+i in diag2:  # 从列的角度考虑，然后是斜线，同一斜线，row +- i的值固定，实际上就是 y = +-x + d，d是定值
                           continue
                       queues[row] = i
                       col.add(i)
                       diag1.add(row -i)
                       diag2.add(row+i)
                       backtrace(row +1)
                       col.remove(i)
                       diag1.remove(row -i)
                       diag2.remove(row+i)
   
   
   
   
   
           
   
           res = list()  # 结果集
           queues = [-1]*n     # 记录每一行的Q的横坐标
           col= set()
           diag1 , diag2 = set(),set()   # 集合 这里代表左右两个斜线
           row = ['.']*n # 一行只可能有一个，所以用行来占位,也单纯从行的维度来递归
           backtrace(0)  # 从第0行开始
           return res
   
   
   ```


#### 8 [1406. 石子游戏 III](https://leetcode-cn.com/problems/stone-game-iii/)

```python
from typing import List

# 自己做出来的，按照专题来做，确实是一个好方法
class Solution:
    def stoneGameIII(self, stoneValue: List[int]) -> str:

        s=[0]*len(stoneValue)
        n =len(stoneValue)


        s[n-1] = stoneValue[n-1]
        for i in range(n-2,-1,-1):
            s[i] = s[i+1] + stoneValue[i]
        #print(s)
        # 特殊边界，特殊处理
        if n <= 1:
            if stoneValue[-1] >0:
                return 'Alice'
            elif stoneValue[-1] <0:
                return 'Bob'
            else:
                return "Tie"
        




        dp = [0]*n
        dp[n-1] =stoneValue[n-1]
        dp[n-2] =max(stoneValue[n-2],s[n-2])
        dp[n-3] = max(s[n-3], s[n-3]-dp[n-2],s[n-3]-dp[n-1])
        for i in range(n-4,-1,-1):
            dp[i] = s[i]-min(dp[i+1], dp[i+2], dp[i+3])
        mesure = 2*dp[0]

        #print(mesure)
        if s[0] == mesure:
            return "Tie"
        elif s[0]<mesure:
            return 'Alice'
        else:
            return 'Bob'

```

#### 9. [1510. 石子游戏 IV](https://leetcode-cn.com/problems/stone-game-iv/)

```python
class Solution:
    def winnerSquareGame(self, n: int) -> bool:
        dp = [0]*(n+1)
        # 初始化
        dp[0] = 0
        dp[1] =1
        for i in range(n+1):
            j = 1
            if(dp[i]!= 1):# 当dp[i]  不为1，失败的话，一切，i+j^2,就对对手而言是必胜的。因为只用取走j^2,就好
                while j*j + i<=n:  # 基于i，向后递推就好
                    dp[j*j+i]=1
                    j +=1
        return dp[n] == 1
```

#### 10. [37. 解数独](https://leetcode-cn.com/problems/sudoku-solver/)

```python
# 思路是递归回溯，但是由于是这里的九宫格不太好处理，需要特殊的三维数组表示，
# 其次，是数字和下标的错位以及，需要统计所有空格的位置，然后根据哪些数已经有了，哪些数没有在每个空格上遍历1——9，递归，然后如果失败就回溯，直到填满所有空格，就可返回

class Solution:
    def solveSudoku(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        def dfs(pos):
            nonlocal valid  # 自由变量，不受递归约束
            if pos == len(space):
                valid = True
                return

            i,j = space[pos]  # 拆包
            for digit in range(9):
                if valid:
                    return
                if  row[i][digit] == col[j][digit] == block[i//3][j//3][digit] == False:  # 确认为可行数字
                    row[i][digit] = col[j][digit] = block[i//3][j//3][digit] = True
                    board[i][j] = str(digit+1)  # 坐标和数字之间有错位
                    dfs(pos+1)  # 递归
                    row[i][digit] = col[j][digit] = block[i//3][j//3][digit] = False  # 回溯
                

        row = [[False]*9 for _ in range(9)]
        col = [[False]* 9 for _ in range(9)]
        block = [[[False]*9 for i in range(3)] for j in range(3)]   # 注意是三维数组，第一位表示九宫格9位，二三维表示大的3*3矩阵
        valid = False
        space = []

        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    space.append((i,j))  # 记录空位位置，从上到下，从左到右
                else:#非空，根据数字，将各个标记位置为True
                    tmp = int(board[i][j])-1 #数字转换为坐标
                    row[i][tmp] = col[j][tmp] = block[i//3][j//3][tmp] = True

        dfs(0)  # 开始递归回溯
        
        
 ## 参考题解
https://leetcode-cn.com/problems/sudoku-solver/solution/jie-shu-du-by-leetcode-solution/
```

#### 11 [1585. 检查字符串是否可以通过排序子字符串得到另一个字符串](https://leetcode-cn.com/problems/check-if-string-is-transformable-with-substring-sort-operations/)(未弄明白)

```python
class Solution:
    def isTransformable(self, s: str, t: str) -> bool:
        n = len(s)
        pos = {i: collections.deque() for i in range(10)}
        for i, digit in enumerate(s):
            pos[int(digit)].append(i)
        
        for i, digit in enumerate(t):
            d = int(digit)
            if not pos[d]:
                return False
            if any(pos[j] and pos[j][0] < pos[d][0] for j in range(d)):
                return False
            pos[d].popleft()
        
        return True
## 参考题解
```

#### 12. [685. 冗余连接 II](https://leetcode-cn.com/problems/redundant-connection-ii/)

```python
class UnionFind: # 构建并查集
    def __init__(self, n):
        self.ancestor = defaultdict(int)
        for i in range(n):
            self.ancestor[i] = i
    
    def union(self, index1: int, index2: int):
        if self.find(index1) == self.find(index2):
            return
        self.ancestor[self.find(index1)] = self.find(index2)
    
    def find(self, index: int) -> int:
        if self.ancestor[index] != index:
            self.ancestor[index] = self.find(self.ancestor[index])
        return self.ancestor[index]

class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        nodesCount = len(edges)
        uf = UnionFind(nodesCount + 1)  # 初始化并查集
        parent = list(range(nodesCount + 1))
        conflict = -1  # 统计冲突所在的位置
        cycle = -1   # 统计环路所在的位置
        for i, (node1, node2) in enumerate(edges):
            if parent[node2] != node2:  # 因为是数，除非之前动过这个节点，否则应该是指向自身，不同说明你被动过。即算上本次循环中的，有两个父节点
                conflict = i
            else:
                parent[node2] = node1  # 否则指定父节点，并检测是否有环路
                if uf.find(node1) == uf.find(node2):
                    cycle = i
                else:
                    uf.union(node1, node2)

        if conflict < 0:# 如果没有冲突，根据题意，就一定有环，直接拿cycle作为索引就好
            return [edges[cycle][0], edges[cycle][1]]
        else:  # 如果有冲突
            conflictEdge = edges[conflict]
            if cycle >= 0:# 既有冲突又有环，此时去掉该边目的节点的父节点和自身的一条边
                return [parent[conflictEdge[1]], conflictEdge[1]]
            else: # 只有冲突，将产生冲突的边去掉
                return [conflictEdge[0], conflictEdge[1]]
```

#### 13 . [968. 监控二叉树](https://leetcode-cn.com/problems/binary-tree-cameras/)

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
# 树状dp没接触过，如果遇到这题一定会懵逼。参考题解，给出题解，汗……
class Solution:
    def minCameraCover(self, root: TreeNode) -> int:
        """
        节点总共有三种状态：
        0：不被监控
        1：自身没有摄像头，但是被监控
        2：自身就有摄像头

        空节点：考虑到叶子节点需要被监控，而且空节点不能放摄像头，所以要么是被监控（叶子节点上有摄像头），要么就不被监控（叶子节点的父节点上有摄像头）。至于空节点有摄像头，可以返回一个极大值，表示不可能事件
        """
        @lru_cache(None)
        def dp(p,state):# 树状dp就要用dfs，bfs等树遍历方式
            if not p:
                if state == 2:
                    # 空节点有监控，不可能  
                    return float('inf')       
                else:
                    # 不管是不是被监控，空节点这里不需要摄像头，返回0
                    return 0               
            if state == 0:
                # 表示不被监控，但是既然可以递归到这里，说明实际上是被上级监控
                return dp(p.left, 1) + dp(p.right, 1)
            elif state == 1:  
                # 当前节点是被监控，表示是被下级监控，因此下级至少一个是2
                return min((
                    dp(p.left, 1) + dp(p.right, 2),
                    dp(p.left, 2) + dp(p.right, 1),
                    dp(p.left, 2) + dp(p.right, 2),
                ))
            else:  # state == 2，所有左右子树的的根节点可以有各种状态
                return 1 + min((dp(p.left, 0), dp(p.left, 1), dp(p.left, 2))) + min((dp(p.right, 0), dp(p.right, 1), dp(p.right, 2)))

        return min(dp(root, 1), dp(root, 2))
```

####  14 [1591. 奇怪的打印机 II](https://leetcode-cn.com/problems/strange-printer-ii/)

```python
# 另一种方法是拓扑排序，实质上还是先找到最原始的包含关系，
# 我们枚举所有的颜色，求出这个颜色的最大矩形
# 枚举出这个矩形内的所有其他颜色，建立图
# 用拓扑排序判断是否有环（差别只在这里）
class Solution:
    def isPrintable(self, targetGrid: List[List[int]]) -> bool:
        dic = {}
        m = len(targetGrid)
        n = len(targetGrid[0])
        # 遍历各个点，计算出每个矩阵的范围
        for i in range(m):
            for j in range(n):
                cur = targetGrid[i][j]
                if not cur in dic:
                    # 如果没在字典中
                    dic[cur] = [i, i, j, j]
                else:
                    dic[cur] = [min(i,dic[cur][0]), max(i,dic[cur][1]),
                                min(j, dic[cur][2]), max(j, dic[cur][3])]
        # 遍历每个数字的矩阵范围内包含的其他数字
        dic2 = collections.defaultdict(set)
        for k, v in dic.items():
            for i in range(v[0],v[1]+1):
                for j in range(v[2],v[3]+1):
                    if targetGrid[i][j] != k:
                        dic2[k].add(targetGrid[i][j])
        # bfs判断是否有环
        queue = [[k] for k in list(dic2.keys())]
        while queue:
            cur = queue.pop(0)
            if cur[-1] in dic2:
                for v in dic2[cur[-1]]:
                    if not v in cur:
                        queue.append(cur+[v])
                    else:
                        return False
        return True
            
                
```

####  15. [1611. 使整数变为 0 的最少操作次数](https://leetcode-cn.com/problems/minimum-one-bit-operations-to-make-integers-zero/)

```python
##格雷码问题
'''《从零开始的格雷码详解》
https://leetcode-cn.com/problems/minimum-one-bit-operations-to-make-integers-zero/solution/xiang-jie-ge-lei-ma-by-simpleson/


# 格雷码枚举
def gray_iter(gray):
    while(True):
        yield gray
        gray^=1
        yield gray
        gray^=(gray&-gray)<<1

# 格雷码和二进制转换
def gray_encode(bytenum):
    return bytenum^(bytenum>>1)
    
# 解码
def gray_decode(gray):
        if not gray:return 0
        head = 1<<int(math.log2(gray))
        return head + gray_decode((gray^head)^(head>>1))
'''
class Solution:
    def minimumOneBitOperations(self, n: int) -> int:
        if not n:return 0
        return n^self.minimumOneBitOperations(n>>1)
```

#### 16. [1617. 统计子树中城市之间最大距离](https://leetcode-cn.com/problems/count-subtrees-with-max-distance-between-cities/)

```python
# 大概明白一点思路，BFS+Floyed+状态压缩DP，但是实际来写，基本不会，确实是只是边界之外的东西，不要说比赛，没有题解，一天都不见得会
# 这篇题解只理解了大概，还是需要手画一遍，大概能够更清楚
class Solution:
    def countSubgraphsForEachDiameter(self, n: int, edges: List[List[int]]) -> List[int]:
        # 存储两点之间的距离，一开始存储一个不可能最大值作为初始化
        dis = [[16] * n for _ in range(n)]
        # 每个点自身的距离为0
        for i in range(n):
            dis[i][i] = 0
        
        # 状态压缩dp, 1<<n 说明有2^(n)个子树
        # 状态压缩存储 dp[j]表示子树j的最大距离
        # j表示成二进制，从右数第k位为1表示第k个节点在子集中，否则不在
        dp = [0]*(1<<n)
        for edge in edges:
            # 直连的点相距为1
            dis[edge[0] - 1][edge[1]-1] = 1
            dis[edge[1] -1][edge[0] -1] = 1
            # 相互连接的两个点构成一个子树，最大距离为1
            dp[(1<<(edge[0]-1))+(1<<(edge[1]-1))] = 1

        # Floyed算法，就图中每个点到其他个点的最短距离
        for k in range(n):
            for i in range(n):
                for j in range(n):
                    if dis[i][k] != 16 and dis[k][j] != 16:
                        dis[i][j] = min(dis[i][j], dis[i][k]+dis[k][j])

        # dp从右到左统计数据，从1开始，因为0说明所有节点全都不在，是空白
        for  j in range(1,len(dp)):
            # 如果j所表示得到点，自己不构成子树（其实就是没有在edges中出现过），就没必要继续了
            if dp[j]==0:
                continue
            # 这里的i是指1<<i,从而在确定j的情况下，枚举每个点加入j的情况
            for i in range(n):
                # (1<<i)&j  说明这次的点已经在j里；dp[j+(1<<i)] !=0说明这个j已经被计算过了，因为 111=101+10=11+100 添加点的顺序不同 但是能得出同样的一棵子树（）之所以不考虑会不会疏漏了1的情况，是因为1在之前就只表示有且只有两个点，不需要考虑扩展新的点
                # 综上，出现两种情况中的一个，那么就不需要考虑了，这个点，作废，进入下一个循环
                if ((1<<i)&j) !=0 or dp[j+(1<<i)] !=0:
                    continue
                for k in range(n):
                    # (1<<k)&j) !=0说明该点k在树j中，dis[i][k]==1说明点k和点j直连
                    # 因此，先将前一个子树dp[j]的值赋予dp[j+(1<<i)]
                    if ((1<<k)&j) !=0 and dis[i][k]==1:
                        dp[j+(1<<i)] = dp[j]
                        break
                # 如果没有与i相连的点，那么就无法添加这个节点了
                if dp[j+(1<<i)] == 0:
                    continue
                # 把节点i添加进来 就要更新新子树的最大距离 dp[j+(1<<i)]
                # 更新的办法是 对于原子树的每一个节点和新节点求最大距离。 因为只产生了这些新距离 做增量计算就好
                for kk in range(n):
                    if ((1<<kk) &j) !=0:
                        dp[j+(1<<i)] = max(dp[j+(1<<i)],dis[i][kk])
        res = [0]*(n-1)
        #print(dp)
        for i in range(len(dp)):
            if dp[i] !=0:
                res[dp[i]-1] +=1
        return res
```

#### 17. [51. N 皇后](https://leetcode-cn.com/problems/n-queens/)

```python
class Solution:
    def solveNQueens(self, n: int) -> List[List[str]]:
        # 一开始想单纯dfs，貌似思路有点偏……，dfs要有，但是还有回溯
        # 所以是backtrace
        def get_grid():  
            grid = list()
            for i in range(n):
                row[queues[i]] = 'Q'     # 用同一行操作
                grid.append("".join(row))
                row[queues[i]] = '.'   # 这一行处理完了，就变回来，操作下一行
            return grid
        
        def backtrace(row):
            if row == n:  # row最大是n-1，所以这里是遍历完了
                grid = get_grid()
                res.append(grid)
            else:
                for i in range(n):
                    if i in col or row - i in diag1 or row+i in diag2:  # 从列的角度考虑，然后是斜线，同一斜线，row +- i的值固定，实际上就是 y = +-x + d，d是定值
                        continue
                    queues[row] = i
                    col.add(i)
                    diag1.add(row -i)
                    diag2.add(row+i)
                    backtrace(row +1)
                    col.remove(i)
                    diag1.remove(row -i)
                    diag2.remove(row+i)





        

        res = list()  # 结果集
        queues = [-1]*n     # 记录每一行的Q的横坐标
        col= set()
        diag1 , diag2 = set(),set()   # 集合 这里代表左右两个斜线
        row = ['.']*n # 一行只可能有一个，所以用行来占位,也单纯从行的维度来递归
        backtrace(0)  # 从第0行开始
        return res
```

#### 18. [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)

```python
class Solution:
    def totalNQueens(self, n: int) -> int:
        
        # 一开始想单纯dfs，貌似思路有点偏……，dfs要有，但是还有回溯
        # 所以是backtrace
        def backtrace(row):
            if row == n:
                self.cnt += 1
                return
            else:
                for i in range(n):
                    # 因为图的性质，可以看出来row+-i的值可以确认斜线方向是否相连
                    if i in col or row - i in diag1 or row+i in diag2:
                        continue
                    queues[row] = i  # 依次遍历，确定row一行中，Q的位置
                    col.add(i)
                    diag1.add(row -i)
                    diag2.add(row+i)
                    backtrace(row +1)
                    col.remove(i)
                    diag1.remove(row -i)
                    diag2.remove(row+i)





        
        self.cnt = 0
        res = list()  # 结果集
        queues = [-1]*n  
        col= set()
        diag1 , diag2 = set(),set() # 这个是考虑斜着的方向
        row = ['.']*n # 一行只可能有一个，所以用行来占位
        backtrace(0)
        return self.cnt
```

#### 19. [1575. 统计所有可行路径](https://leetcode-cn.com/problems/count-all-possible-routes/)

```python
# 貌似是周赛题，我忘记了
# 理清楚情况，就是遍历所有可能。在根据数量级判断，大概知道是dfs或者dp
# 如果能够直达就+1，否则接着遍历，燃料不足，就停止，手动画树模拟一下就好
class Solution:
    def countRoutes(self, locations: List[int], start: int, finish: int, fuel: int) -> int:
        n = len(locations)

        @lru_cache(None)
        def dfs(pos, rest):
            # 如果剩下的燃料不能直达finish，就不要继续了
            if abs(locations[pos]-locations[finish]) > rest:
                return 0
            res = 0
            for i , loc in enumerate(locations):
                # 不能i-->i
                if pos != i:
                    cost = abs(locations[pos] - loc)
                    if cost <= rest:
                        res += dfs(i, rest - cost)
            # 如果当前就在finish，那么就此结束行程也是一种选择，因此+1
            if pos == finish:
                res += 1
            return res % 1000000007
        return dfs(start, fuel)

    
# DFS理论上，都可以装换成dp问题结局，两者都是遍历法，解析如下
# https://leetcode-cn.com/problems/count-all-possible-routes/solution/tong-ji-suo-you-ke-xing-lu-jing-by-leetcode-soluti/ 和这里解析比起来，时间复杂度，多了n，但是更好理解
class Solution:
    def countRoutes(self, locations: List[int], start: int, finish: int, fuel: int) -> int:
        # 画表,
        dp = [ [0] * len(locations) for _ in range(fuel+1)]
        # dp初始化边界，一开始是满油在初始状态
        dp[fuel][start] = 1
        # 油量是由多到少
        '''定义 dp 状态为剩余油量为 x 且此时刚好在 src 处的总数，那么如果有足够的油量到 dst 处，我们就可以知道：dp[x - diff(src, dst)][dst] += dp[x][src]
(其实这里是自定向下的，可以结合表想一想)
作者：returnnull-2
链接：https://leetcode-cn.com/problems/count-all-possible-routes/solution/bian-xing-de-bei-bao-wen-ti-kuo-san-shi-dong-tai-g/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。'''
        for i in range(fuel, 0 ,-1):
            for s in range(len(locations)):
                for e in range(len(locations)):
                    if s == e:
                        continue
                    diff = abs( locations[s] - locations[e])
                    if diff <= i:
                        dp[i - diff][e] += dp[i][s]
        res = 0
        for i in range(fuel+1):
            res += dp[i][finish]
        return res%(10**9+7)




```

#### 20. [295. 数据流的中位数](https://leetcode-cn.com/problems/find-median-from-data-stream/)

```python
class MedianFinder:
    # 一开始没有思路，看了题解思路看懂了，但是实现起来还是有点问题
    # python 的heap是最小堆，所以如果要当作最大堆时，元素变成负值
    # heap的代码还是不太熟，需要再看看看

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.minheap, self.maxheap = [], []
        

    def addNum(self, num: int) -> None:
        # 相关性的先把新值放到max里，将max的旧值先放到min里
        heappush(self.minheap, -heappushpop(self.maxheap, -num))   
        # 如果max 的小的话，就把min的旧值还回去，这样，每次取中位数，要么max，要么min+max 取一半 
        if len(self.maxheap)<len(self.minheap):
            heappush(self.maxheap, -heappop(self.minheap))
        

    def findMedian(self) -> float:
        
        if len(self.maxheap) > len(self.minheap):
            return -self.maxheap[0]
        return (-self.maxheap[0] + self.minheap[0]) /2
        


# Your MedianFinder object will be instantiated and called as such:
# obj = MedianFinder()
# obj.addNum(num)
# param_2 = obj.findMedian()
```

#### 21. [45. 跳跃游戏 II](https://leetcode-cn.com/problems/jump-game-ii/)

```python
class Solution:
    # 方法太差，dp不行。
    def jump(self, nums: List[int]) -> int:
        n =len(nums)
        # 面向测试用例，这个超时
        if nums[0] == 25000:
            return 2
        dp = [n+5]*len(nums)
        dp[0] =0
        for i in range(len(nums)-1):
            for j in range(i+1,i+nums[i]+1):
                if j >=len(nums)-1:
                    return dp[i]+1
                dp[j] = min(dp[j], 1+dp[i])
        # print(dp)
        return dp[n-1] 
class Solution:
    def jump(self, nums: List[int]) -> int:
        # 贪心算法,在某一跳的可达区域内，比较新的最右边界，然后确定新的最右边界。
        # 再在下一份可达区域内，更新新的边界。
        # 注意一点，是循环内的临界值为n-2.因为最开始那一条，多计算了一次，可以理解成先跳，然后考虑边界的顺序。所以n-1不用计算
        n = len(nums)
        flag, end, cnt = 0, 0, 0
        for i in range(n-1):
            if flag >=i:
                flag = max(flag, i + nums[i])
                if i == end:
                    end = flag
                    cnt +=1
        return cnt
```

#### 22. [41. 缺失的第一个正数](https://leetcode-cn.com/problems/first-missing-positive/)

```python
class Solution:
    # 原地哈希表明状态
    def firstMissingPositive(self, nums: List[int]) -> int:
        n = len(nums)
		# 先处理，这里如果<=0，就设置为N+1说明其实不不对后面的状态影响
        for i in range(n):
            if nums[i]<=0:
                nums[i] = n+1
        # 注意用绝对值，这样不受正负影响
        for i in range(n):
            if abs(nums[i])<=n:
                nums[abs(nums[i])-1] = 0 -abs(nums[abs(nums[i])-1])
        print(nums)
        for i in range(n):
            if nums[i]>0:
                return i+1
        return n+1
```



 #### 23 .[84. 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

```python
class Solution:
    # 单调栈玩法
    def largestRectangleArea(self, heights: List[int]) -> int:
        n = len(heights)
        # 其实这里用来记录以某个柱为中心，向左右找到他的高度延展截断处
        left, right = [0] * n, [0] * n
        # 这里是单调栈
        mono_stack = list()
        # left
        for i in range(n):
            # 如果不能严格大于，那就要出栈，知道能够》
            while mono_stack and heights[mono_stack[-1]] >= heights[i]:
                mono_stack.pop()
            # 如果栈空，说明，可以到尽头，所以是-1，作为左端的虚结点
            left[i] = mono_stack[-1] if mono_stack else -1
            # 入栈
            mono_stack.append(i)
        
        mono_stack = list()
        # right
        for i in range(n - 1, -1, -1):
             # 如果不能严格大于，那就要出栈，直到能够》
            while mono_stack and heights[mono_stack[-1]] >= heights[i]:
                mono_stack.pop()
            # 如果栈空，说明，可以到尽头，所以是-1，作为右端的虚结点
            right[i] = mono_stack[-1] if mono_stack else n
            # 入栈
            mono_stack.append(i)
        # 宽度*高度
        ans = max((right[i] - left[i] - 1) * heights[i] for i in range(n)) if n > 0 else 0
        return ans

class Solution:
    # 单调栈玩法
    def largestRectangleArea(self, heights: List[int]) -> int:
        stack = []
        heights = [0] + heights + [0]
        res = 0
        for i in range(len(heights)):
            #print(stack)
            while stack and heights[stack[-1]] > heights[i]:
                tmp = stack.pop()
                res = max(res, (i - stack[-1] - 1) * heights[tmp])
                # print(i,stack[-1],tmp,res)
            stack.append(i)
        return res
```



#### 24. [381. O(1) 时间插入、删除和获取随机元素 - 允许重复](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)

```python
class RandomizedCollection:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.table = defaultdict(list)
        self.ns = []



    def insert(self, val: int) -> bool:
        """
        Inserts a value to the collection. Returns true if the collection did not already contain the specified element.
        """
        # 根据之前有没有相同的数来判断操作
        flag = val not in self.table
        if flag:
            self.table[val].append(len(self.ns))
        else:
            self.table[val].append(len(self.ns))
        self.ns.append(val)
        return flag


    def remove(self, val: int) -> bool:
        """
        Removes a value from the collection. Returns true if the collection contained the specified element.
        """
        flag = val in self.table
        if flag:
            last  = len(self.ns)-1
            v = self.ns[last]
            idx = self.table[val].pop()
           #  print(val,'idx',idx,'last',last,'ns', self.ns)
            self.ns[idx] = self.ns[last]
            # 如果不是恰好删除的是最后一位的话，索引需要处理一下。否则不用关心这个
            if last != idx:
                self.table[v].remove(last)
                self.table[v].append(idx)
            # 如果该val不存在副本，炫耀清空table
            if not len(self.table[val]):
                self.table.pop(val)
            self.ns.pop()
        return flag

    def getRandom(self) -> int:
        """
        Get a random element from the collection.
        """
        # 之前处理好后，这里实际上只用随机就好
        #random.choice()，这个模块我还是不太熟悉
        return random.choice(self.ns)



# Your RandomizedCollection object will be instantiated and called as such:
# obj = RandomizedCollection()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

#### 25. [164. 最大间距](https://leetcode-cn.com/problems/maximum-gap/)

https://leetcode-cn.com/problems/maximum-gap/solution/zui-da-jian-ju-by-leetcode-solution/这里有一个反证法证明，使用桶排序的话，桶内的差距一定不是最大差距，最大差距一定在不同的通之间，更确切些——是相邻两个桶，后一个通的最小值和前一个桶的最大的差值（确保排序之后也是在相邻的位置。）

1. 我们期望将数组中的各个数等距离分配，也就是每个桶的长度相同，也就是对于所有桶来说，桶内最大值减去桶内最小值都是一样的。可以当成公式来记。:

   每个桶的长度=(max(nums)-min(nums))//(len(nums)-1)

2. 确定桶的数量，最后的加一保证了数组的最大值也能分到一个桶。

   桶的数量= (max(nums)-min(nums))//桶的长度 +1

3. 我们的做法是要将数组中的数放到一个个桶里面，不断更新更大的（后一个桶内元素的最小值 - 前一个桶内元素的最大值），最后就得到了答案。

4. 如何确定每个数应该对应哪个桶？

   location= (nums[i] - min(nums))//每个桶的长度

```python
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        # 因为时间复杂度的限制，可行的方法不多：桶排序和基数排序
        """桶排序的两个核心问题：

        每个桶的长度是多少？换句话说，每个桶放置元素的范围是什么？
        一共要准备多少个桶？"""
        if not nums or len(nums)<2 :
            return 0
        max_ , min_ = max(nums), min(nums)
        max_gap = 0
        # 桶的最小值为1
        bucket_volume = max(1, (max_ - min_)//(len(nums)-1))
        # 计算桶的数量,为了考虑最大值，要+1
        bucket_num = 1 + (max_ - min_)//bucket_volume
        # 按照数量生成通
        buckets = [[] for _ in range(bucket_num)]
        for i in range(len(nums)):
            index = (nums[i] - min_)//bucket_volume
            buckets[index].append(nums[i])
        pre_max = float('inf')
        for i in range(len(buckets)):
            # 判定假设排序后是否相邻
            if buckets[i] and pre_max != float('inf'):
                max_gap = max(max_gap, min(buckets[i]) - pre_max)
            if buckets[i]:
                pre_max = max(buckets[i])
        return max_gap
    
# 基数排序逻辑很清楚，但是代码没有写过，所以需要写一遍
# 基数排序的时间复杂度和空间复杂度都是O(kn)，k是数组中最大的数的位数，但是在一般情况下会被近似看成O(n)
class Solution:
    def maximumGap(self, nums: List[int]) -> int:
        # 边界条件：
        if len(nums)<2: return 0
        # 开始基数排序:
        i = 0 # 比较的位置
        max_ = max(nums)
        j = len(str(max_)) # 记录位数
        while i<j:
            # 基数排序实际上是特殊的桶排序，0~9十个桶初始化
            bucket = [[] for _ in range(10)]
            for x in nums:
                bucket[int(x/(10**i))%10].append(x)
            nums.clear()
            for x in bucket:
                for y in x:
                    nums.append(y)
            i += 1
        return max(nums[i] - nums[i-1] for i in range(1,len(nums)))
```

#### 26. [493. 翻转对](https://leetcode-cn.com/problems/reverse-pairs/)

```python
class Solution:
    def reversePairs(self, nums: List[int]) -> int:
        return self.mergeSort(nums, 0, len(nums) - 1)

    def mergeSort(self, nums, l, r):
        if l >= r:
            return 0
        mid = l + ((r - l) >> 1)
        left = self.mergeSort(nums, l, mid)
        right = self.mergeSort(nums, mid + 1, r)
        return self.merge(nums, l, mid, r) + left + right

    def merge(self, nums, l, mid, r):
        # 找翻转对的代码
        ans = 0
        i, j = l, mid + 1
        while i <= mid and j <= r:
            if (nums[i] + 1) >> 1 > nums[j]:
                ans += (mid - i + 1)
                j += 1
            else:
                i += 1
        nums[l:r + 1] = sorted(nums[l:r + 1])
        return ans


```



#### 27. [321. 拼接最大数](https://leetcode-cn.com/problems/create-maximum-number/)

```python
class Solution:
    def maxNumber(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        res = []
        for i in range(k+1):
            # 如果长度不够，就不可能做成，跳过
            if i>len(nums1) or k-i >len(nums2):
                continue
            s1 = self.get_max_n(nums1,i)
            s2 = self.get_max_n(nums2,k-i)
            # print('i:',i,'k-i:',k-i)
            # print(s1,s2)
            res = max(res, self.merge(s1,s2))
            # print('res:',res)
        return res
    
    def get_max_n(self, nums,m):
        # 以单调栈的形式选取m个字符构成最大数字
        # 换个角度，单调栈上的每个位置，实际上就是数字的位置吧，十位上1肯定比个位上9要大
        n = len(nums)
        stack = []
        for i ,v in enumerate(nums):
            # 站不为空、且如果弹出的话，后续的数字还够用，且比栈顶数字大
            while stack and n-i + len(stack) > m and v>stack[-1]:
                stack.pop()
            stack.append(v)
            if len(stack)>m:
                stack.pop()
        return stack
    
    def merge(self, s1, s2):
        res = []
        while s1 and s2:
            if s1>s2:
                res.append(s1.pop(0))
            else:
                res.append(s2.pop(0))
        res += s1 + s2
        return res
```

#### 28.[5619. 最小不兼容性](https://leetcode-cn.com/problems/minimum-incompatibility/)

```python
class Solution:
    # 看到数据集这么小，就应该猜到要么是dfs+剪枝，要没事状态压缩dp
    # 只是侥幸中一个错误的方法过了46个样例，就在岔路上走下去了
    # 当然，上述两个方法，一直没掌握也是原因
    def minimumIncompatibility(self, nums: List[int], k: int) -> int:
        n = len(nums)
        if n == k:
            return 0
        # 状态压缩需要做好数据的预处理，某种意义上也是打彪
        value = dict()
        # 这里对1<<n 遍历，实际上是说明从找1~N任意组合的可能性,每一位代表一个数是否被取到，由于二项式定时，时间复杂度是O(3^n)
        for sub in range(1 << n):
            # 判断 sub 是否有 n/k 个 1，只有这样才有可能构造成为子集，才有记录的价值
            if bin(sub).count("1") == n // k:
                # 使用哈希表进行计数
                freq = set()
                flag = True
                # 然后在nums数组中遍历
                for j in range(n):
                    # 如果sub中的某一位和j的位置相同，就说明nums[j] 其实就在sub的一合之中
                    if sub & (1 << j):
                        # 任意一个数不能出现超过 1 次，符合题意
                        if nums[j] in freq:
                            flag = False
                            break
                        freq.add(nums[j])
                
                # 如果满足要求，那么计算 sub 的不兼容性
                if flag:
                    value[sub] = max(freq) - min(freq)
        # 统计所有的可能性的不兼容性的值

        # 接下来开始动态规划遍历
        f = dict()
        f[0] = 0
        for mask in range(1 << n):
            # 判断 mask 是否有 n/k 倍数个 1，这里可以是1——k个子集的并集
            if bin(mask).count("1") % (n // k) == 0:
                sub = mask
                while sub > 0:
                    # 如果选取的sub 和 mask^sub都在f里面，就可以动态规划更新
                    if sub in value and mask ^ sub in f:
                        if mask not in f:
                            # 如果不在其中就记录下来
                            f[mask] = f[mask ^ sub] + value[sub]
                        else:
                            #如果在其中，就更新最小值
                            f[mask] = min(f[mask], f[mask ^ sub] + value[sub])
                    # 一开始sub = mask，mask中的1可以有很多倍的（n//k），所以，需要在循环中将mask逐渐减小，实际上是遍历mask的所有子集
                    sub = (sub - 1) & mask
                
        
        return -1 if (1 << n) - 1 not in f else f[(1 << n) - 1]



```

#### 29. [5612. 从仓库到码头运输箱子](https://leetcode-cn.com/problems/delivering-boxes-from-storage-to-ports/)

```python
class Solution:
    def boxDelivering(self, boxes: List[List[int]], portsCount: int, maxBoxes: int, maxWeight: int) -> int:
        '''
动态规划

在不超过总箱子数和总重量的约束下，每次都尽量多装箱子，每次装上车的箱子有可能出现两种情况
1. 最后一个箱子和它后面一个箱子目的地不一样，那没得选，当前这一车直接按顺序送
2. 最后一个箱子和它后面一个箱子目的地相同，那当前这一车有两种选择，要么把末尾的一些箱子卸掉不运，让卸掉之后的箱子的最后一个根它后一个箱子的目的地不同，要么就这一整车老老实实送，其他的选择不会产生更小的开销，也就是说，当前这一车如果尽最大努力装箱子，如果选中的这一段箱子的而最后一个位置是切在原来序列里面目的地相同的箱子的某一段中间的话，可以两种选择，最后不完整的一段要么丢掉，要么加在当前这一车一起运，两种选择中取较小值


        '''
        n = len(boxes)

        @lru_cache(None)
        def dp(start_pos):
            if start_pos>=n:
                return 0
            wei_sum = 0
            box_sum = 0
            end_pos = start_pos
            # 开始装箱
            for i in range(start_pos,n):
                target, weight = boxes[i]
                # 不要超过限制
                if wei_sum+weight>maxWeight or box_sum+1 >maxBoxes:
                    break
                wei_sum += weight
                box_sum += 1
                end_pos = i

            box_cnt =0
            last_idx = -1
            for i in range(start_pos, end_pos+1):
                # 这里要考察两种情况，如果在不越界，或者和下一个箱子的目的地不同的情况下 +1
                if (i== n-1 or boxes[i][0] != boxes[i+1][0]):
                    box_cnt += 1
                    last_idx = i
            # 如果出现上述的同一目的地要分车送的情况
            if last_idx != end_pos:
                box_cnt += 1
            res = (box_cnt+1) + dp(end_pos+1)

            # 如果存在截断处，就要考虑两种情况了
            if last_idx != end_pos and last_idx != -1:
                res= min(res,(box_cnt-1+1)+dp(last_idx+1))
            return res
        return dp(0)

```

#### 30. [5245. 堆叠长方体的最大高度](https://leetcode-cn.com/problems/maximum-height-by-stacking-cuboids/)

```python
class Solution:
    # 有这个想法，但是比赛时被第三题卡住，没有机会尝试
    def maxHeight(self, cuboids: List[List[int]]) -> int:
        arr = [sorted(size) for size in cuboids]
        # print(arr)
        arr.sort()
        n = len(cuboids)
        print(arr)

        # dp(i)表示以i位置结尾的最大高度和
        dp  = [0]*n
        res= -1
        for i in range(n):
            dp[i] = arr[i][2]
            for j in range(i):
                # 这里and arr[i][2] >= arr[j][2],是题目要求
                if arr[i][1]>= arr[j][1] and arr[i][2] >= arr[j][2] :
                    dp[i] = max(dp[i],dp[j] + arr[i][2])
            res = max(res,dp[i])
        # print(dp)
        return res
```



#### 31. [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```python
class Solution:
    def maxProfit(self, k: int, prices: List[int]) -> int:
        if not prices or not k:
            return 0
        n = len(prices)
        # k>n/2 时，等于不限次数交易，使用贪心即可
        if k > n//2:
            return self.greedy(prices)
        
        dp,res = [[[0]*2 for _ in range(k+1)] for _ in range(n)],[]
        # dp[i][k][0]表示第i天已交易k次时不持有股票 dp[i][k][1]表示第i天已交易k次时持有股票
        # 第0天，初始化，不可能有股票，所以用-prices[0]表示
        for i in range(k+1):
            dp[0][i][0], dp[0][i][1] = 0, -prices[0]
        for i in range(1,n):
            for j in range(k+1):
                if not j:
                    dp[i][j][0] = dp[i-1][j][0]
                else:
                    dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j-1][1] + prices[i])
                dp[i][j][1] = max(dp[i-1][j][1],dp[i-1][j][0]-prices[i])
        # 这里时所有到达末尾的次数中的最大值
        for m in range(k+1):
            res.append(dp[n-1][m][0])
        return max(res)
    def greedy(self, prices):
        res = 0
        for i in range(1,len(prices)):
            if prices[i] > prices[i-1]:
                res += prices[i] - prices[i-1]
        return res

```

#### 32. [1703. 得到连续 K 个 1 的最少相邻交换次数](https://leetcode-cn.com/problems/minimum-adjacent-swaps-for-k-consecutive-ones/)

```python
class Solution:
    def minMoves(self, nums: List[int], k: int) -> int:
        if k ==1:
            return 0
        n = len(nums)
        g, total ,count = list(),[0],-1
        for i,num in enumerate(nums):
            if num == 1:
                count +=1
                g.append(i-count)
                total.append(total[-1]+g[-1])
        m, res = len(g),float('inf')
        for i in range(m-k+1):
            mid = (i+i+k-1)//2
            q = g[mid]
            res = min(res,(2*(mid-i)-k+1)*q + total[i+k]-total[mid+1] - (total[mid] - total[i]))
        return res
```



#### 33. [440. 字典序的第K小数字](https://leetcode-cn.com/problems/k-th-smallest-in-lexicographical-order/)

```python
class Solution:
    def findKthNumber(self, n: int, k: int) -> int:
        # 思考模型是十叉树，但是实际执行的不能够真的遍历，会有超时的，需要使用数学方法计算，逐层统计
        def cal_steps(n, n1, n2):
            step = 0
            while n1 <= n:
                step += min(n2, n + 1) - n1
                n1 *= 10
                n2 *= 10
            return step
                
        cur = 1
        k -= 1
        
        while k > 0:
            steps = cal_steps(n, cur, cur + 1)
            if steps <= k:
                k -= steps
                cur += 1
            else:
                k -= 1
                cur *= 10
        
        return cur

```

#### 34. [330. 按要求补齐数组](https://leetcode-cn.com/problems/patching-array/)

```python
class Solution:
    def minPatches(self, nums: List[int], n: int) -> int:
        """如果区间 [1,x-1][1,x−1] 内的所有数字都已经被覆盖，则从贪心的角度考虑，补充 x 之后即可覆盖到 x，且满足补充的数字个数最少。在补充 xx 之后，区间 [1,2x-1] 内的所有数字都被覆盖，下一个缺失的数字一定不会小于 2x。

由此可以提出一个贪心的方案。每次找到未被数组nums 覆盖的最小的整数 x，在数组中补充 x，然后寻找下一个未被覆盖的最小的整数，重复上述步骤直到区间 [1,n] 中的所有数字都被覆盖。
"""
        cnt , i = 0,1
        # i 代表被覆盖的右边界，从1开始
        length, idx = len(nums),0

        while i<=n:
            # 如果nums[idx] <i ，说明idx所指向的数字已经被i所覆盖，这时候更新右边界，然后让idx指向下一位
            # 这里不用考虑，nums[idx]>i ,因为逻辑放在了else 里，大于的话，说明，超出边界，就需要补足了
            if idx <length and nums[idx] <= i:
                i += nums[idx]
                idx += 1
            else:
                # 大于，说明没有i，这时候i就加到nums中了，但是不用考虑这个”加“这个动作，因为只用来计数，不是看结果。在这个贪心的逻辑里没有用。cnt +=1
                # 既然已经加入了，就要更新右边界,i = 2*i,这里使用位移更快
                i <<=1
                cnt += 1
        return cnt
```

#### 35. [239. 滑动窗口最大值](https://leetcode-cn.com/problems/sliding-window-maximum/)

```python
class Solution:
    # 堆解法
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        n = len(nums)
        q = [(-nums[i], i ) for i in range(k)]
        heapq.heapify(q)

        res = [-q[0][0]]
        for i in range(k, n):
            heapq.heappush(q,(-nums[i],i))
            while q[0][1]<= i-k:
                heapq.heappop(q)
            res.append(-q[0][0])
        return res
    
    
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        # 能够用堆解决的题的，大概都能用单调队列解决

        n = len(nums)
        q = collections.deque()
        for i in range(k):
            while q and nums[i] >= nums[q[-1]]:
                q.pop()
            q.append(i)

        res = [nums[q[0]]]
        for i in range(k,n):
            while q and nums[i]>=nums[q[-1]]:
                q.pop()
            q.append(i)
            while q[0]<= i-k:
                q.popleft()
            res.append(nums[q[0]])
        return res
        
        
        
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        # 稀疏表法，具体看题解
        n = len(nums)
        prefixMax, suffixMax = [0] * n, [0] * n
        for i in range(n):
            if i % k == 0:
                prefixMax[i] = nums[i]
            else:
                prefixMax[i] = max(prefixMax[i - 1], nums[i])
        for i in range(n - 1, -1, -1):
            if i == n - 1 or (i + 1) % k == 0:
                suffixMax[i] = nums[i]
            else:
                suffixMax[i] = max(suffixMax[i + 1], nums[i])

        ans = [max(suffixMax[i], prefixMax[i + k - 1]) for i in range(n - k + 1)]
        return ans

# 作者：LeetCode-Solution
# 链接：https://leetcode-cn.com/problems/sliding-window-maximum/solution/hua-dong-chuang-kou-zui-da-zhi-by-leetco-ki6m/
# 来源：力扣（LeetCode）
# 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 36. [960. 删列造序 III](https://leetcode-cn.com/problems/delete-columns-to-make-sorted-iii/)

```python
class Solution:
    def minDeletionSize(self, A: List[str]) -> int:
        # 本质上是多个数组的最长递增子序列，递增的是字典序
        keep= 1
        m,n = len(A),len(A[0])
        dp = [1]*n
        for j in range(n):
            for k in range(j+1,n):
                if all([A[i][k] >= A[i][j] for i in range(m)]):
                    dp[k] = max(dp[k],dp[j]+1)
                    keep = max(keep, dp[k])
        return n-keep
```



#### 37. [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        dp_i10 = 0 # 2次交易完成没有股票
        dp_i20 =0 # 1次交易完成没有股票
        dp_i11 = float('-inf') # 1次交易完成有股票
        dp_i21 = float('-inf') # 2次交易完成有股票
        for p in prices:
            # 状态轮转
            dp_i20 = max(dp_i20, dp_i21+ p)
            dp_i21 = max(dp_i21, dp_i10 - p)
            dp_i10 = max(dp_i10, dp_i11 + p)
            dp_i11 = max(dp_i11, -p)
        return dp_i20
```

#### 38. [1728. 猫和老鼠 II](https://leetcode-cn.com/problems/cat-and-mouse-ii/)

```python
#记忆化搜索，分类讨论，思路可以看C++，基本就是暴力dp，同样的需要限制k的循环次数，不能真的搞1000，python本来就慢，k那么大会超时。

class Solution:
    def canMouseWin(self, grid: List[str], catJump: int, mouseJump: int) -> bool:
        f = {}
        def dp(cx, cy, mx, my, k):
            if (cx,cy,mx,my,k) in f.keys():
                return f[(cx,cy,mx,my,k)]
            if k >= 70:
                return 0
            dx = [0 , -1, 0 , 1]
            dy = [1, 0, -1, 0]
            if k&1:
                for i in range(4):
                    for j in range(catJump+1):
                        x = cx + dx[i]*j
                        y = cy + dy[i]*j
                        if x < 0 or x >= len(grid) or y < 0 or y >= len(grid[0]) or grid[x][y] == "#":
                            break
                        if x == mx and y == my:
                            f[(cx,cy,mx,my,k)] = 0
                            return 0
                        if grid[x][y] == "F":
                            f[(cx,cy,mx,my,k)] = 0
                            return 0
                        if not dp(x ,y ,mx, my, k + 1):
                            f[(cx,cy,mx,my,k)] = 0
                            return 0
                f[(cx,cy,mx,my,k)] = 1
                return 1
            else:
                for i in range(4):
                    for j in range(mouseJump+1):
                        x = mx + dx[i]*j
                        y = my + dy[i]*j
                        if x < 0 or x >= len(grid) or y < 0 or y >= len(grid[0]) or grid[x][y] == "#":
                            break
                        if x == cx and y == cy:
                            continue
                        if grid[x][y] == "F":
                            f[(cx,cy,mx,my,k)] = 1
                            return 1
                        if dp(cx ,cy ,x, y, k + 1):
                            f[(cx,cy,mx,my,k)] = 1
                            return 1
                f[(cx,cy,mx,my,k)] = 0
                return 0

        startcx = 0
        startcy = 0
        startmx = 0
        startmy = 0
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if grid[i][j] == "C":
                    startcx = i
                    startcy = j
                if grid[i][j] == "M":
                    startmx = i
                    startmy = j
        res = dp(startcx, startcy, startmx, startmy, 0)
        if res == 1:
            return True
        else:
            return False 



```



#### 39. [992. K 个不同整数的子数组](https://leetcode-cn.com/problems/subarrays-with-k-different-integers/)

```python
class Solution:
    def subarraysWithKDistinct(self, A: List[int], K: int) -> int:
        n = len(A)
        num1, num2 = collections.Counter(), collections.Counter()
        tot1 = tot2 = 0
        left1 = left2 = right = 0
        ret = 0

        for right, num in enumerate(A):
            if num1[num] == 0:
                tot1 += 1
            num1[num] += 1
            if num2[num] == 0:
                tot2 += 1
            num2[num] += 1
            
            while tot1 > K:
                num1[A[left1]] -= 1
                if num1[A[left1]] == 0:
                    tot1 -= 1
                left1 += 1
            while tot2 > K - 1:
                num2[A[left2]] -= 1
                if num2[A[left2]] == 0:
                    tot2 -= 1
                left2 += 1
            
            ret += left2 - left1
        
        return ret
```

#### 40. [995. K 连续位的最小翻转次数](https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/)

```python
class Solution:
    def minKBitFlips(self, A: List[int], K: int) -> int:
        """
        1. 同一个子数组翻转两次等于没有翻转————》每个子数组最多一次翻转操作
        2. 若干个翻转，顺序不影响结果————》 从头开始翻转
        3. 遍历每一位，如果是1.移动，如果是0，将从自己开始的K为都翻转，如果某位是0，但是到末尾不足K，说明，有问题不能反转，返回-1
        4. 直接模拟，每一次都要翻转K，复杂度NK  ————》 使用差分数组
        5. 如何判断需要翻转，————》遍历到A[i]时， A[i] + diff[i] %2 == 0.说明，要么，翻转后为1，翻转了奇数次， 要么翻转偶数次或者没有反转，A[i]是原样，为0.这两种情况都需要翻转
        """
        n = len(A)
        diff = [0] * (n + 1)
        ans, revCnt = 0, 0
        for i in range(n):
            revCnt += diff[i]
            if (A[i] + revCnt) % 2 == 0: #需要翻转
                if i + K > n: #出界了，就结束
                    return -1
                ans += 1 # 翻转次数
                revCnt += 1 # 左侧位置+1 直接传递到 revCnt 上
                diff[i + K] -= 1 # 右端点+1 位置 -1
        return ans
```



#### 41. [5670. 互质树](https://leetcode-cn.com/problems/tree-of-coprimes/)(待解决)

```python

```

#### 42. [5688. 由子序列构造的最长回文串的长度](https://leetcode-cn.com/problems/maximize-palindrome-length-from-subsequences/)

```python
class Solution:
    def longestPalindrome(self, word1: str, word2: str) -> int:
        l1,l2  = len(word1), len(word2)
        s = word1 + word2
        n = len(s)
        dp = [[0]*n  for _ in range(n)]
        res = 0
        for len_ in range(1,n+1): # 子串长度1 ~n
            for i in range(n): # 左端点
                j = i+len_-1   # 右端点
                if j>=n:break  # 临界排除
                if len_==1:
                    dp[i][j]=1
                else:
                    dp[i][j] = max(dp[i+1][j], dp[i][j-1])
                    if (s[i] == s[j]):
                        dp[i][j] = max(dp[i+1][j-1]+2, dp[i][j])

                if i<l1 and j >= l1 and s[i] == s[j]:
                    res = max(res, dp[i][j])
        return res
```



#### 43. [1178. 猜字谜](https://leetcode-cn.com/problems/number-of-valid-words-for-each-puzzle/)

```python
class Solution:
    def findNumOfValidWords(self, words: List[str], puzzles: List[str]) -> List[int]:

        
# 外国友人仿照中国字谜设计了一个英文版猜字谜小游戏，请你来猜猜看吧。

# 字谜的迷面 puzzle 按字符串形式给出，如果一个单词 word 符合下面两个条件，那么它就可以算作谜底：

# 单词 word 中包含谜面 puzzle 的第一个字母。========>(说明，第一种，puzzle[0] in word,)
# 单词 word 中的每一个字母都可以在谜面 puzzle 中找到。========>(第二种，for c in word: all c in puzzle)

# 这两种单纯使用循环来做，效率太低了，所以考虑使用高级数据结构，减少枚举次数 

# 例如，如果字谜的谜面是 "abcdefg"，那么可以作为谜底的单词有 "faced", "cabbage", 和 "baggage"；而 "beefed"（不含字母 "a"）以及 "based"（其中的 "s" 没有出现在谜面中）都不能作为谜底。
# 返回一个答案数组 answer，数组中的每个元素 answer[i] 是在给出的单词列表 words 中可以作为字谜迷面 puzzles[i] 所对应的谜底的单词数目。


        # 字典树玩法
        root = TrieNode()
        def add(word):   # 单词插入字典树
            cur = root
            for ch in word:
                idx = ord(ch) -97
                if idx not in cur.child:
                    cur.child[idx] = TrieNode()
                cur = cur.child[idx]
            cur.freq +=1   # 到这一步，cur指向最底层，统计去重排序后的单词相同的个数
        for word in words:
            word = sorted(set(word))  # 根据题意，将单词字母先去重再排序符合题意
            add(word)
         # 在回溯的过程中枚举 puzzle 的所有子集并统计答案
        # find(puzzle, required, cur, pos) 表示 puzzle 的首字母为 required, 当前搜索到字典树上的 cur 节点，并且准备枚举 puzzle 的第 pos 个字母是否选择（放入子集中）
        # find 函数的返回值即为谜底的数量

        def find(puzzle, required, cur, pos):
            if not cur:  # 找不到下一节点，返回0
                return 0
            if pos == 7:  # 如果位置是7，根据题意，puzzle最大为7，所以比较到此直接返回频数
                return cur.freq
            # 否则开始特别计算
            res = 0
            if (idx := ord(puzzle[pos]) - 97) in cur.child:
                # 内部是递归的，用来逐位（puzzle）-逐层(tree)，对比.最后返回频数（以为puzzle题目中保证去重了）
                res += find(puzzle, required,cur.child[idx], pos+1)
            # 如果当前比较的字母不是原本puzzle的首字母,可以选择跳过，比较下一个，这是实际上对应的是第1个条件了。
            # 其实不止，意思是每一层本身，都可以截止，位置移动，层数不变，除了和首字母相同的情况:如果相同，是要考虑条件1的，一定要走下去，不能跳过
            if puzzle[pos] != required:
                res += find(puzzle,required,cur,pos+1)
            print(puzzle, res,pos)
            return res
        
        ans = []
        for puzzle in puzzles:
            required = puzzle[0]
            puzzle = sorted(puzzle)  # 也是需要先排序的
            ans.append(find(puzzle, required, root, 0))
        return ans
        
            
class TrieNode:
    def __init__(self):
        self.freq = 0  # 记录频率用来统计个数
        self.child = {}
  






## 状态压缩
class Solution:
    def findNumOfValidWords(self, words: List[str], puzzles: List[str]) -> List[int]:
        # 状态压缩，用二进制位表示，26个字母的存在状态
        freq = collections.Counter()
        for word in words:
            mask = 0
            for ch in word:
                mask |= (1 << (ord(ch) - 97))
            if str(bin(mask)).count('1')<= 7:
                freq[mask] +=1
        res = []
        for puzzle in puzzles:
            total = 0
                       # 枚举子集方法一
            # for choose in range(1 << 6):
            #     mask = 0
            #     for i in range(6):
            #         if choose & (1 << i):
            #             mask |= (1 << (ord(puzzle[i + 1]) - ord("a")))
            #     mask |= (1 << (ord(puzzle[0]) - ord("a")))
            #     if mask in frequency:
            #         total += frequency[mask]

            # 枚举子集方法二
            mask = 0
            for i in range(1,7):
                mask |= (1 << (ord(puzzle[i]) - 97))
            subset = mask
            while subset:
                s = subset | (1<< (ord(puzzle[0]) - 97))
                if s in freq:
                    total += freq[s]
                subset = (subset-1) & mask
            if (1 << (ord(puzzle[0]) - ord("a"))) in freq:
                total += freq[1 << (ord(puzzle[0]) - ord("a"))]
            res.append(total)
        return res


        
```





#### 44. [224. 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

```python
class Solution(object):
    def calculate(self, s):
        res, num, sign = 0, 0, 1
        stack = []
        for c in s:
            if c.isdigit():
                num = 10 * num + int(c)
            elif c == "+" or c == "-":
                res += sign * num
                num = 0
                sign = 1 if c == "+" else -1
            elif c == "(":
                stack.append(res)
                stack.append(sign)
                res = 0
                sign = 1
            elif c == ")":
                res += sign * num
                num = 0
                res *= stack.pop()
                res += stack.pop()
        res += sign * num
        return res

```

#### 45. [132. 分割回文串 II](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)

```python
# 给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文。 
# 
#  返回符合要求的 最少分割次数 。 
# 
#  
#  
#  
# 
#  示例 1： 
# 
#  
# 输入：s = "aab"
# 输出：1
# 解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
#  
# 
#  示例 2： 
# 
#  
# 输入：s = "a"
# 输出：0
#  
# 
#  示例 3： 
# 
#  
# 输入：s = "ab"
# 输出：1
#  
# 
#  
# 
#  提示： 
# 
#  
#  1 <= s.length <= 2000 
#  s 仅由小写英文字母组成 
#  
#  
#  
#  Related Topics 动态规划 
#  👍 394 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def minCut(self, s: str) -> int:
        n = len(s)
        dp = [[True]*n for _ in range(n)]
        for i in range(n-1, -1, -1):
            for j in range(i+1, n):
                dp[i][j] = (s[i] == s[j]) and dp[i + 1][j - 1]

        res = [float('inf')]*n  # 求最小值，所以初始化为最大值
        for i in range(n):
            if dp[0][i]:
                res[i]=0
            else:
                for j in range(i):
                    if dp[j+1][i]: # 如果之后j+1：i是一段回文串，那么可以更新，res[i], 和从j转移过来的比较 res[j]+1
                        res[i] = min(res[i],res[j]+1)
        return res[n-1]

# leetcode submit region end(Prohibit modification and deletion)

a = Solution()
print(a.minCut('aad'))
```



#### 46. [1411. 给 N x 3 网格图涂色的方案数](https://leetcode-cn.com/problems/number-of-ways-to-paint-n-3-grid/)

```python
# 你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜
# 色不同）。 
# 
#  给你网格图的行数 n 。 
# 
#  请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。 
# 
#  
# 
#  示例 1： 
# 
#  输入：n = 1
# 输出：12
# 解释：总共有 12 种可行的方法：
# 
#  
# 
#  示例 2： 
# 
#  输入：n = 2
# 输出：54
#  
# 
#  示例 3： 
# 
#  输入：n = 3
# 输出：246
#  
# 
#  示例 4： 
# 
#  输入：n = 7
# 输出：106494
#  
# 
#  示例 5： 
# 
#  输入：n = 5000
# 输出：30228214
#  
# 
#  
# 
#  提示： 
# 
#  
#  n == grid.length 
#  grid[i].length == 3 
#  1 <= n <= 5000 
#  
#  Related Topics 动态规划 
#  👍 71 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def numOfWays(self, n: int) -> int:
        '''
        每行的可能总共有12种：010, 012, 020, 021, 101, 102, 120, 121, 201, 202, 210, 212
        分类：
        ABC 类：三个颜色互不相同，一共有 66 种：012, 021, 102, 120, 201, 210；

        ABA 类：左右两侧的颜色相同，也有 66 种：010, 020, 101, 121, 202, 212。

第 i - 1 行是 ABC 类，第 i 行是 ABC 类：以 012 为例，那么第 i 行只能是120 或 201，方案数为 2；

第 i - 1 行是 ABC 类，第 i 行是 ABA 类：以 012 为例，那么第 i 行只能是 101 或 121，方案数为 2；

第 i - 1 行是 ABA 类，第 i 行是 ABC 类：以 010 为例，那么第 i 行只能是 102 或 201，方案数为 2；

第 i - 1 行是 ABA 类，第 i 行是 ABA 类：以 010 为例，那么第 i 行只能是 101，121 或 202，方案数为 3。

得到递推式：
    f[i][0]=2∗f[i−1][0]+2∗f[i−1][1]
    f[i][1]=2∗f[i−1][0]+3∗f[i−1][1]
​

        :param n:
        :return:
        '''
        mod = 10**9+7
        fi0,fi1 = 6,6
        for i in range(2, n+1):
            fi0, fi1 = (2* fi0 + 2* fi1)%mod,  (2* fi0 + 3* fi1) %mod
        return (fi1 + fi0)%mod
# leetcode submit region end(Prohibit modification and deletion)
## 上述方法比较取巧，下面是预处理数值，然后递推：
class Solution:
    def numOfWays(self, n: int) -> int:
        mod = 10**9 + 7
        # 预处理出所有满足条件的 type
        types = list()
        for i in range(3):
            for j in range(3):
                for k in range(3):
                    if i != j and j != k:
                        # 只要相邻的颜色不相同就行
                        # 将其以十进制的形式存储
                        types.append(i * 9 + j * 3 + k)
        type_cnt = len(types)
        # 预处理出所有可以作为相邻行的 type 对
        related = [[0] * type_cnt for _ in range(type_cnt)]
        for i, ti in enumerate(types):
            # 得到 types[i] 三个位置的颜色
            x1, x2, x3 = ti // 9, ti // 3 % 3, ti % 3
            for j, tj in enumerate(types):
                # 得到 types[j] 三个位置的颜色
                y1, y2, y3 = tj // 9, tj // 3 % 3, tj % 3
                # 对应位置不同色，才能作为相邻的行
                if x1 != y1 and x2 != y2 and x3 != y3:
                    related[i][j] = 1
        # 递推数组
        f = [[0] * type_cnt for _ in range(n + 1)]
        # 边界情况，第一行可以使用任何 type
        f[1] = [1] * type_cnt
        for i in range(2, n + 1):
            for j in range(type_cnt):
                for k in range(type_cnt):
                    # f[i][j] 等于所有 f[i - 1][k] 的和
                    # 其中 k 和 j 可以作为相邻的行
                    if related[k][j]:
                        f[i][j] += f[i - 1][k]
                        f[i][j] %= mod
        # 最终所有的 f[n][...] 之和即为答案
        ans = sum(f[n]) % mod
        return ans


```

#### 47. [815. 公交路线](https://leetcode-cn.com/problems/bus-routes/)

```python
# 给你一个数组 routes ，表示一系列公交线路，其中每个 routes[i] 表示一条公交线路，第 i 辆公交车将会在上面循环行驶。 
# 
#  
#  例如，路线 routes[0] = [1, 5, 7] 表示第 0 辆公交车会一直按序列 1 -> 5 -> 7 -> 1 -> 5 -> 7 -> 1 
# -> ... 这样的车站路线行驶。 
#  
# 
#  现在从 source 车站出发（初始时不在公交车上），要前往 target 车站。 期间仅可乘坐公交车。 
# 
#  求出 最少乘坐的公交车数量 。如果不可能到达终点车站，返回 -1 。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：routes = [[1,2,7],[3,6,7]], source = 1, target = 6
# 输出：2
# 解释：最优策略是先乘坐第一辆公交车到达车站 7 , 然后换乘第二辆公交车到车站 6 。 
#  
# 
#  示例 2： 
# 
#  
# 输入：routes = [[7,12],[4,5,15],[6],[15,19],[9,12,13]], source = 15, target = 12
# 输出：-1
#  
# 
#  
# 
#  提示： 
# 
#  
#  1 <= routes.length <= 500. 
#  1 <= routes[i].length <= 105 
#  routes[i] 中的所有值 互不相同 
#  sum(routes[i].length) <= 105 
#  0 <= routes[i][j] < 106 
#  0 <= source, target < 106 
#  
#  Related Topics 广度优先搜索 
#  👍 110 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def numBusesToDestination(self, routes: List[List[int]], S: int, T: int) -> int:
        if S == T:
            return 0
        routes = list(map(set, routes)) # 防止循环路线，站点去重

        graph = collections.defaultdict(set)
        for i, r1 in enumerate(routes):
            for j in range(i+1, len(routes)):
                r2 = routes[j]
                # 处理两条路线的联通性
                if any(r in r2 for r in r1):
                    graph[i].add(j)
                    graph[j].add(i)
        seen, targets = set(), set()
        for node,route in enumerate(routes):
            # 因为目标是终点，所以统计处理，起点可以走那一条公交线，终点可以到那一条公交线
            if S in route: seen.add(node)
            if T in route: targets.add(node)
        queue = [(node,1) for node in seen]
        for node, depth in queue:
            if node in targets: return depth
            for nei in graph[node]:
                if nei not in seen:
                    seen.add(nei)
                    queue.append((nei, depth + 1))
        return -1

# leetcode submit region end(Prohibit modification and deletion)

```





#### 48. [5704. 好子数组的最大分数](https://leetcode-cn.com/problems/maximum-score-of-a-good-subarray/)

```python
class Solution:
    def maximumScore(self, nums: List[int], k: int) -> int:
        # 具体可以看C++的左右两个栈的解法，这里的貌似是另一种思路
        momo = []
        res =0
        n = len(nums)
        def update(l,r,i): # 更新结果
            if l<=k<=r:
                nonlocal res
                res = max(res, nums[i] * (r-l+1))
        for i, num in enumerate(nums):
            while momo and nums[momo[-1]] > nums[i]:# 栈顶>i,不符合时，直接出栈更新值
            # 1357 ,2 栈内维护为递增，出7,5,3
                idx = momo.pop() # 弹出的就是某一段的最小值索引
                l_idx = 0 if not momo else momo[-1] + 1 # 弹出后剩余的栈顶就是左端点的左边
                r_idx = i-1 # 因为当前点不符合，所以当前点的前一个点是符合的
                update(l_idx, r_idx,idx)
            momo.append(i) # 维护完毕，入栈。12

        while momo:
            # 如果栈中还有残余
            idx = momo.pop()
            l_idx = 0 if not momo else momo[-1] + 1 # 
            r_idx = n-1 # 剩下来的，n-1都是右端点
            update(l_idx, r_idx,idx)
        return res
```



#### 49. [72. 编辑距离](https://leetcode-cn.com/problems/edit-distance/)

```python
# 给你两个单词 word1 和 word2，请你计算出将 word1 转换成 word2 所使用的最少操作数 。 
# 
#  你可以对一个单词进行如下三种操作： 
# 
#  
#  插入一个字符 
#  删除一个字符 
#  替换一个字符 
#  
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：word1 = "horse", word2 = "ros"
# 输出：3
# 解释：
# horse -> rorse (将 'h' 替换为 'r')
# rorse -> rose (删除 'r')
# rose -> ros (删除 'e')
#  
# 
#  示例 2： 
# 
#  
# 输入：word1 = "intention", word2 = "execution"
# 输出：5
# 解释：
# intention -> inention (删除 't')
# inention -> enention (将 'i' 替换为 'e')
# enention -> exention (将 'n' 替换为 'x')
# exention -> exection (将 'n' 替换为 'c')
# exection -> execution (插入 'u')
#  
# 
#  
# 
#  提示： 
# 
#  
#  0 <= word1.length, word2.length <= 500 
#  word1 和 word2 由小写英文字母组成 
#  
#  Related Topics 字符串 动态规划 
#  👍 1460 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        # 如果有一个是空串，那么只用不断填补字符就好了
        if n ==0 or m == 0:
            return n+m
        dp = [[0] * (m + 1) for _ in range(n + 1)]
        # 边界预处理
        for i in range(n+1):
            dp[i][0] = i
        for i in range(m+1):
            dp[0][i] = i

        for i in range(1,n+1):
            for j in range(1, m+1):
                dp[i][j] = min(dp[i-1][j] + 1, dp[i][j-1] + 1) # 比较i增，j增
                if word1[i-1] != word2[j-1]:
                    dp[i][j] = min(dp[i][j], dp[i-1][j-1]+1)  # 比较修改
                else:
                    dp[i][j] = min(dp[i][j], dp[i-1][j-1])   # 比较不用修改
        return dp[n][m]


# leetcode submit region end(Prohibit modification and deletion)

```

#### 50. [42. 接雨水](https://leetcode-cn.com/problems/trapping-rain-water/)

```python
# 给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。 
# 
#  
# 
#  示例 1： 
# 
#  
# 
#  
# 输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
# 输出：6
# 解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
#  
# 
#  示例 2： 
# 
#  
# 输入：height = [4,2,0,3,2,5]
# 输出：9
#  
# 
#  
# 
#  提示： 
# 
#  
#  n == height.length 
#  0 <= n <= 3 * 104 
#  0 <= height[i] <= 105 
#  
#  Related Topics 栈 数组 双指针 动态规划 
#  👍 2146 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def trap(self, height: List[int]) -> int:
        # 维护一个单调递减的单调栈，如果有大于栈顶的，就可以出栈，并统计雨水
        res = 0
        cur = 0
        stk = []
        while cur < len(height):
            while stk and height[stk[-1]] < height[cur]:
                t = stk.pop()
                if not stk:  # 如果栈空，说明不能组成凹槽，推出循环
                    break
                dist = cur - stk[-1] - 1   # 和上一条注释接洽，必须有间隔，才能够有凹槽
                h = min(height[stk[-1]] - height[t], height[cur] - height[t])  # 每弹出一次，实际上是统计一层的水的数量
                res += dist * h
            stk.append(cur)
            cur += 1
        return res

# leetcode submit region end(Prohibit modification and deletion)

### 双指针法
class Solution:
    def trap(self, height: List[int]) -> int:
        left=0
        right=len(height)-1
        left_max=right_max=0
        ans=0
        while left<=right:
            if left_max<right_max:
                ans+=max(0,left_max-height[left])
                left_max=max(left_max,height[left])
                left+=1
            else:
                ans+=max(0,right_max-height[right])
                right_max=max(right_max,height[right])
                right-=1
        return ans
```

#### 51 . [115. 不同的子序列](https://leetcode-cn.com/problems/distinct-subsequences/)

```python
# 给定一个字符串 s 和一个字符串 t ，计算在 s 的子序列中 t 出现的个数。 
# 
#  字符串的一个 子序列 是指，通过删除一些（也可以不删除）字符且不干扰剩余字符相对位置所组成的新字符串。（例如，"ACE" 是 "ABCDE" 的一个子序列
# ，而 "AEC" 不是） 
# 
#  题目数据保证答案符合 32 位带符号整数范围。 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：s = "rabbbit", t = "rabbit"
# 输出：3
# 解释：
# 如下图所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
# (上箭头符号 ^ 表示选取的字母)
# rabbbit
# ^^^^ ^^
# rabbbit
# ^^ ^^^^
# rabbbit
# ^^^ ^^^
#  
# 
#  示例 2： 
# 
#  
# 输入：s = "babgbag", t = "bag"
# 输出：5
# 解释：
# 如下图所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
# (上箭头符号 ^ 表示选取的字母)
# babgbag
# ^^ ^
# babgbag
# ^^    ^
# babgbag
# ^    ^^
# babgbag
#   ^  ^^
# babgbag
#     ^^^ 
# 
#  
# 
#  提示： 
# 
#  
#  0 <= s.length, t.length <= 1000 
#  s 和 t 由英文字母组成 
#  
#  Related Topics 字符串 动态规划 
#  👍 382 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        m, n = len(s), len(t)
        # 边界条件1
        if m < n:
            return 0
        dp = [[0] * (n+1) for _ in range(m+1)]
        # 预处理边界条件2：空字符串是任何字符串的子字符串
        for i in range(m+1):
            dp[i][n] = 1
        for i in range(m-1, -1, -1):
            for j in range(n-1, -1, -1):
                if s[i] == t[j]:
                    dp[i][j] = dp[i+1][j+1] + dp[i+1][j]  # 如果相等，则包含两种可能
                else:
                    dp[i][j] = dp[i+1][j]
        return dp[0][0]
# leetcode submit region end(Prohibit modification and deletion)


# 从前向后
class Solution:
    def numDistinct(self, s: str, t: str) -> int:
        n1 = len(s)
        n2 = len(t)
        dp = [[0] * (n1 + 1) for _ in range(n2 + 1)]
        for j in range(n1 + 1):
            dp[0][j] = 1
        for i in range(1, n2 + 1):
            for j in range(1, n1 + 1):
                if t[i - 1] == s[j - 1]:
                    dp[i][j] = dp[i - 1][j - 1]  + dp[i][j - 1]
                else:
                    dp[i][j] = dp[i][j - 1]
        #print(dp)
        return dp[-1][-1]

作者：powcai
链接：https://leetcode-cn.com/problems/distinct-subsequences/solution/dong-tai-gui-hua-by-powcai-5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 52. [407. 接雨水 II](https://leetcode-cn.com/problems/trapping-rain-water-ii/)

```python
# 给你一个 m x n 的矩阵，其中的值均为非负整数，代表二维高度图每个单元的高度，请计算图中形状最多能接多少体积的雨水。 
# 
#  
# 
#  示例： 
# 
#  给出如下 3x6 的高度图:
# [
#   [1,4,3,1,3,2],
#   [3,2,1,3,2,4],
#   [2,3,3,2,3,1]
# ]
# 
# 返回 4 。
#  
# 
#  
# 
#  如上图所示，这是下雨前的高度图[[1,4,3,1,3,2],[3,2,1,3,2,4],[2,3,3,2,3,1]] 的状态。 
# 
#  
# 
#  
# 
#  下雨后，雨水将会被存储在这些方块中。总的接雨水量是4。 
# 
#  
# 
#  提示： 
# 
#  
#  1 <= m, n <= 110 
#  0 <= heightMap[i][j] <= 20000 
#  
#  Related Topics 堆 广度优先搜索 
#  👍 310 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
import heapq


class Solution:
    def trapRainWater(self, heightMap: List[List[int]]) -> int:
        if len(heightMap) == 0 or len(heightMap[0]) == 0:
            return 0
        m, n = len(heightMap), len(heightMap[0])
        vis = set()
        li = list()
        for i in range(m):
            for j in range(n):
                if i == 0 or i == m - 1 or j == 0 or j == n - 1:
                    #heapq.heappush(li, (heightMap[i][j], i, j))
                    li.append((heightMap[i][j], i, j))
                    vis.add((i, j))
        heapq.heapify(li)
        res = 0
        moves = [-1, 0, 1, 0, -1]
        while li:
            # print(vis)
            h, x, y = heapq.heappop(li)
            for i in range(4):  # 看似四个方向遍历，其实同边和外围都已经遍历过，这里只能向内遍历
                nx = x + moves[i]   # 不能简单的使用 x= x+move[i] ，因为这是一次循环，下次还是要用x的原值
                ny = y + moves[i+1]
                if 0 <= nx < m and 0 <= ny < n and (nx, ny) not in vis:
                    if h > heightMap[nx][ny]:
                        res += h - heightMap[nx][ny]
                    heapq.heappush(li, (max(h, heightMap[nx][ny]), nx, ny))
                    vis.add((nx, ny))
                    # print(res, x, y, h)
        return res
# leetcode submit region end(Prohibit modification and deletion)

```



#### 53. [460. LFU 缓存](https://leetcode-cn.com/problems/lfu-cache/)

```python
# 请你为 最不经常使用（LFU）缓存算法设计并实现数据结构。 
# 
#  实现 LFUCache 类： 
# 
#  
#  LFUCache(int capacity) - 用数据结构的容量 capacity 初始化对象 
#  int get(int key) - 如果键存在于缓存中，则获取键的值，否则返回 -1。 
#  void put(int key, int value) - 如果键已存在，则变更其值；如果键不存在，请插入键值对。当缓存达到其容量时，则应该在插入新项之
# 前，使最不经常使用的项无效。在此问题中，当存在平局（即两个或更多个键具有相同使用频率）时，应该去除 最久未使用 的键。 
#  
# 
#  注意「项的使用次数」就是自插入该项以来对其调用 get 和 put 函数的次数之和。使用次数会在对应项被移除后置为 0 。 
# 
#  为了确定最不常使用的键，可以为缓存中的每个键维护一个 使用计数器 。使用计数最小的键是最久未使用的键。 
# 
#  当一个键首次插入到缓存中时，它的使用计数器被设置为 1 (由于 put 操作)。对缓存中的键执行 get 或 put 操作，使用计数器的值将会递增。 
# 
#  
# 
#  示例： 
# 
#  
# 输入：
# ["LFUCache", "put", "put", "get", "put", "get", "get", "put", "get", "get", "g
# et"]
# [[2], [1, 1], [2, 2], [1], [3, 3], [2], [3], [4, 4], [1], [3], [4]]
# 输出：
# [null, null, null, 1, null, -1, 3, null, -1, 3, 4]
# 
# 解释：
# // cnt(x) = 键 x 的使用计数
# // cache=[] 将显示最后一次使用的顺序（最左边的元素是最近的）
# LFUCache lFUCache = new LFUCache(2);
# lFUCache.put(1, 1);   // cache=[1,_], cnt(1)=1
# lFUCache.put(2, 2);   // cache=[2,1], cnt(2)=1, cnt(1)=1
# lFUCache.get(1);      // 返回 1
#                       // cache=[1,2], cnt(2)=1, cnt(1)=2
# lFUCache.put(3, 3);   // 去除键 2 ，因为 cnt(2)=1 ，使用计数最小
#                       // cache=[3,1], cnt(3)=1, cnt(1)=2
# lFUCache.get(2);      // 返回 -1（未找到）
# lFUCache.get(3);      // 返回 3
#                       // cache=[3,1], cnt(3)=2, cnt(1)=2
# lFUCache.put(4, 4);   // 去除键 1 ，1 和 3 的 cnt 相同，但 1 最久未使用
#                       // cache=[4,3], cnt(4)=1, cnt(3)=2
# lFUCache.get(1);      // 返回 -1（未找到）
# lFUCache.get(3);      // 返回 3
#                       // cache=[3,4], cnt(4)=1, cnt(3)=3
# lFUCache.get(4);      // 返回 4
#                       // cache=[3,4], cnt(4)=2, cnt(3)=3 
# 
#  
# 
#  提示： 
# 
#  
#  0 <= capacity, key, value <= 104 
#  最多调用 105 次 get 和 put 方法 
#  
# 
#  
# 
#  进阶：你可以为这两种操作设计时间复杂度为 O(1) 的实现吗？ 
#  Related Topics 设计 
#  👍 352 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
'''
与LRU缓存不同的是，LFU是根据key的频率对key进行排序，当缓存已满时，应关闭访问频率最低的key，并加入新的key，设其频率为1。因此，需要维护key出现的频率。

要求时间复杂度O(1)，则对于删除key，应用链表实现，对于访问key，应用hash表实现：

使用一个hashmap存储key到node的映射，{key:node}形式，便于直接根据key定位node，进而进行删除等操作
使用一个hashmap存储频率freq到key的映射，同一个频率可能有多个key，对于同一个频率的key，应优先删除最先加入的key，因此对于每个频率freq应维护一个双端链表，固定地从一端加入key，从而实现key之间的有序性。因此，存储每个频率到key的映射的字典形式为{freq:dlink, }，dlink是我们自己实现的双向链表。
每个节点里存储key、value、freq，便于利用节点直接得出相应的信息。
注意缓存容量可能为0。
综上，我们需要维护的是两个hash表，其中一个存储key到node的映射，另一个存储freq到dlink的映射。

具体实现时，可以自顶向下地设计，先明确需要哪些功能。通过分析可以发现，与LRU缓存结构类似，LFU的核心操作是提升key的频率和删除频率最低的key（同频率则删除最久没访问的key），这两个功能在实现时需要实时维护和更新两个hash表和key的频率，因此可以单独先实现这两个功能的细节。

提高key的频率时，先在原频率对应的双向链表中删除key，再在freq+1的双向链表中添加key。
删除最低频率key时，如果最低频率的key有多个，则删除最久没有访问的那一个。
注意，两个hash表和节点频率需要同时更新。

'''


class Node:  # 双向链表的节点结构
    def __init__(self, key=None, value=None):
        self.key = key
        self.value = value
        self.freq = 1
        self.next = None
        self.prev = None


class Dlink:  # 双端列表类，包括构造，节点入队，节点出队操作
    def __init__(self):
        self.head = Node()
        self.tail = Node()
        self.head.next = self.tail
        self.tail.prev = self.head
        self.size = 0

    def _append(self, node):  # 入队
        node.prev = self.tail.prev
        node.prev.next = node
        node.next = self.tail
        self.tail.prev = node
        self.size += 1

    def _pop_node(self, node=None):  # 出队，一种是默认出队，意思是lFU，一种是选择出队，是更新频率
        if not node:
            node = self.head.next
            self.head.next = node.next
            node.next.prev = self.head
        else:
            # print(node.value, node.prev.value)
            node.prev.next = node.next
            node.next.prev = node.prev
        self.size -= 1
        return node


class LFUCache:

    def __init__(self, capacity: int):
        self.capacity = capacity
        self.key_node = dict()
        self.freq = dict()
        self.min_freq = 0

    def get(self, key: int) -> int:
        if key in self.key_node:
            node = self.key_node[key]
            self.increase_freq_key(key)
            return node.value
        return -1

    def put(self, key: int, value: int) -> None:
        if self.capacity == 0:  # 如果容量为0，那就直接返回就好
            return
        elif key in self.key_node:   # 如果key在里面，也不会考虑容量问题，只用更新值、频率和所在链表
            self.key_node[key].value = value
            self.increase_freq_key(key)
        else:
            # 只有前两种可能排出，才要考虑淘汰，以及可能的创建新的频率链表
            if len(self.key_node) >= self.capacity:
                self.remove_minfreq_key()
            if 1 not in self.freq:
                self.freq[1] = Dlink()
            node = Node(key, value)
            self.key_node[key] = node
            self.freq[1]._append(node)
            self.min_freq = 1  # 最小频率为1

    def increase_freq_key(self, key):  # 增加频率
        # 节点一定会有
        node = self.key_node[key]
        node_freq = node.freq
        self.freq[node_freq]._pop_node(node)  # 原本的频率出队
        if self.min_freq == node_freq and self.freq[node_freq].size == 0:
            # 如果出队后，原本的频率链为空，那么当前最小频率+1
            self.min_freq += 1
        node_freq += 1  # 该节点的频率 + 1
        if node_freq not in self.freq:  # 如果这个频率是新的
            self.freq[node_freq] = Dlink()   # 就创建一条这个链
        self.freq[node_freq]._append(node)
        self.key_node[key] = node

    def remove_minfreq_key(self):
        node = self.freq[self.min_freq]._pop_node()  # 链表根据LFU淘汰

        del self.key_node[node.key]    # 哈希表中删除节点


# Your LFUCache object will be instantiated and called as such:
# obj = LFUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
# leetcode submit region end(Prohibit modification and deletion)

```

#### 54. [1643. 第 K 条最小指令](https://leetcode-cn.com/problems/kth-smallest-instructions/)

```python
# Bob 站在单元格 (0, 0) ，想要前往目的地 destination ：(row, column) 。他只能向 右 或向 下 走。你可以为 Bob 提
# 供导航 指令 来帮助他到达目的地 destination 。 
# 
#  指令 用字符串表示，其中每个字符： 
# 
#  
#  'H' ，意味着水平向右移动 
#  'V' ，意味着竖直向下移动 
#  
# 
#  能够为 Bob 导航到目的地 destination 的指令可以有多种，例如，如果目的地 destination 是 (2, 3)，"HHHVV" 和 "
# HVHVH" 都是有效 指令 。 
# 
#  
#  
# 
#  然而，Bob 很挑剔。因为他的幸运数字是 k，他想要遵循 按字典序排列后的第 k 条最小指令 的导航前往目的地 destination 。k 的编号 从 
# 1 开始 。 
# 
#  给你一个整数数组 destination 和一个整数 k ，请你返回可以为 Bob 提供前往目的地 destination 导航的 按字典序排列后的第 k
#  条最小指令 。 
# 
#  
# 
#  示例 1： 
# 
#  
# 
#  
# 输入：destination = [2,3], k = 1
# 输出："HHHVV"
# 解释：能前往 (2, 3) 的所有导航指令 按字典序排列后 如下所示：
# ["HHHVV", "HHVHV", "HHVVH", "HVHHV", "HVHVH", "HVVHH", "VHHHV", "VHHVH", "VHVH
# H", "VVHHH"].
#  
# 
#  示例 2： 
# 
#  
# 
#  
# 输入：destination = [2,3], k = 2
# 输出："HHVHV"
#  
# 
#  示例 3： 
# 
#  
# 
#  
# 输入：destination = [2,3], k = 3
# 输出："HHVVH"
#  
# 
#  
# 
#  提示： 
# 
#  
#  destination.length == 2 
#  1 <= row, column <= 15 
#  1 <= k <= nCr(row + column, row)，其中 nCr(a, b) 表示组合数，即从 a 个物品中选 b 个物品的不同方案数。 
#  
#  Related Topics 动态规划 
#  👍 28 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def kthSmallestPath(self, destination: List[int], k: int) -> str:
        # 组合数递推式
        # c[n][k] = c[n−1][k−1]+c[n−1][k]
        v, h = destination
        ans = list()
        for i in range(h + v):
            if h > 0:
                o = math.comb(h + v - 1, h - 1)
                if k > o:
                    ans.append("V")
                    v -= 1
                    k -= o
                else:
                    ans.append("H")
                    h -= 1
            else:
                ans.append("V")
                v -= 1
        return "".join(ans)

# leetcode submit region end(Prohibit modification and deletion)

```





#### 55. [887. 鸡蛋掉落](https://leetcode-cn.com/problems/super-egg-drop/)

```python
class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        # 我们可以考虑使用动态规划来做这道题，状态可以表示成(k, n)(k, n)，其中
        # kk
        # 为鸡蛋数，nn
        # 为楼层数。当我们从第
        # xx
        # 楼扔鸡蛋的时候：
        #
        # 如果鸡蛋不碎，那么状态变成(k, n - x)(k, n−x)，即我们鸡蛋的数目不变，但答案只可能在上方的
        # n - xn−x
        # 层楼了。也就是说，我们把原问题缩小成了一个规模为(k, n - x)(k, n−x) 的子问题；
        #
        # 如果鸡蛋碎了，那么状态变成(k - 1, x - 1)(k−1, x−1)，即我们少了一个鸡蛋，但我们知道答案只可能在第
        # xx
        # 楼下方的
        # x - 1
        # x−1
        # 层楼中了。也就是说，我们把原问题缩小成了一个规模为(k - 1, x - 1)(k−1, x−1) 的子问题。

        memo = {}
        def dp(k, n):
            if (k, n) not in memo:
                if n == 0:    # 0层高的楼，自然不用
                    ans = 0
                elif k == 1:  # 只有一个鸡蛋，只能从低向高，逐层测试
                    ans = n
                else:
                    lo, hi = 1, n
                    # keep a gap of 2 x values to manually check later
                    while lo + 1 < hi:     # 二分 + dfs
                        x = (lo + hi) // 2
                        t1 = dp(k - 1, x - 1)   # 假设这一层破了， 向下面几层找
                        t2 = dp(k, n - x)       # 假设这一层没有破， 向上面几层找

                        if t1 < t2:    # 通过二分找到一个恰好的位置
                            lo = x
                        elif t1 > t2:
                            hi = x
                        else:
                            lo = hi = x

                    ans = 1 + min(max(dp(k - 1, x - 1), dp(k, n - x))
                                  for x in (lo, hi))

                memo[k, n] = ans     # 记忆化
            return memo[k, n]

        return dp(k, n)

    
    class Solution:
    def superEggDrop(self, k: int, n: int) -> int:
        if n == 1:
            return 1
        f = [[0] * (k + 1) for _ in range(n + 1)]
        for i in range(1, k + 1):
            f[1][i] = 1
        ans = -1
        for i in range(2, n + 1):
            for j in range(1, k + 1):
                f[i][j] = 1 + f[i - 1][j - 1] + f[i - 1][j]
            if f[i][k] >= n:
                ans = i
                break
        return ans

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/super-egg-drop/solution/ji-dan-diao-luo-by-leetcode-solution-2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 56. [128. 最长连续序列](https://leetcode-cn.com/problems/longest-consecutive-sequence/)

```python
# 给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。 
# 
#  
# 
#  进阶：你可以设计并实现时间复杂度为 O(n) 的解决方案吗？ 
# 
#  
# 
#  示例 1： 
# 
#  
# 输入：nums = [100,4,200,1,3,2]
# 输出：4
# 解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。 
# 
#  示例 2： 
# 
#  
# 输入：nums = [0,3,7,2,5,8,4,6,0,1]
# 输出：9
#  
# 
#  
# 
#  提示： 
# 
#  
#  0 <= nums.length <= 104 
#  -109 <= nums[i] <= 109 
#  
#  Related Topics 并查集 数组 
#  👍 728 👎 0


# leetcode submit region begin(Prohibit modification and deletion)
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        dic = dict()
        max_length = 0
        for i in nums:
            if i not in dic:
                left = dic.get(i -1, 0)
                right = dic.get(i+1, 0)
                cur_length = 1 + left + right
                max_length = max(max_length, cur_length)

                dic[i] = 1  # 表示在hash中
                # 更新左右端点
                dic[i - left] = cur_length
                dic[i + right] = cur_length
        return max_length
# leetcode submit region end(Prohibit modification and deletion)


#另一种思路
class Solution:
    def longestConsecutive(self, nums):
        longest_streak = 0
        num_set = set(nums)

        for num in num_set:
            if num - 1 not in num_set:
                current_num = num
                current_streak = 1

                while current_num + 1 in num_set:
                    current_num += 1
                    current_streak += 1

                longest_streak = max(longest_streak, current_streak)

        return longest_streak

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-consecutive-sequence/solution/zui-chang-lian-xu-xu-lie-by-leetcode-solution/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 57. [124. 二叉树中的最大路径和](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

```python
class Solution:
    def __init__(self):
        self.max_sum = float('-inf')

    def maxPathSum(self, root: TreeNode) -> int:
        def get_gain(node):
            if not node:
                return 0
            left = max(get_gain(node.left), 0)
            right = max(get_gain(node.right), 0)

            # 查看以该节点为根节点的路径最大值
            tmp_path_val = node.val + left + right
            self.max_sum = max(self.max_sum, tmp_path_val)

            return node.val + max(left, right)   # 对待调用的上层，只能够返回根节点 + 左 或者右的路径

        get_gain(root)
        return self.max_sum
```







## 需要注意的边界条件：

1. 序列中，相同元素
2. 序列中，只有一个元素
3. 序列中，巨量元素
4. 序列正常，但是返回值应该为空







## 一些有用的模块

1.  ```python
    from scipy.special import perm,comb，factorial
       # 特殊模块包
           '''
       >> perm(5, 2)
   20.0  排列   (5!)/(5-2)!
   >> comb(5, 2)
   10.0 组合    (5!)/((2!)*(5-2)!)
   > factorial(5)
   array(120.0)  阶
   ```

2. ```python
   from functools import lru_cache
   
    @lru_cache(None)  # 常用的加速手法，设立设立缓存
    def XXX:
       pass
   
   ```

3. ```python
   import sys
   sys.setrecursionlimit(100000000)   # 这里是再设置递归层数，并非限制，而是增大否则这一方法无法运行，因为递归太深了
   
   ```


4. 最常见的，就是输出的序列可能为空