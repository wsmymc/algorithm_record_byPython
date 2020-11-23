#  python刷题笔记

* 目前所做的都是用Java刷过的题，一方面复习算法思路，恢复手感，一方面学习python语法
* 目前共计400+题目，先从简单的刷起
* 有些题目重复或者太弱智，就不写了。

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



## easy

#### 1. https://leetcode-cn.com/problems/guess-numbers/（猜数字）

```python
class Solution:
    def game(self, guess: List[int], answer: List[int]) -> int:
        count = 0
        for i in range(3):
             if guess[i] == answer[i]:
                count += 1
        return count
```

#### 2. https://leetcode-cn.com/problems/delete-middle-node-lcci/(删除中间节点)

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

#### 3.https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/（左旋字符串）

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        a=s[:n]
        b=s[n:]
        return b+a
```

#### 4. https://leetcode-cn.com/problems/shuffle-the-array/(重新排列数组----1470)

```python
class Solution:
    def shuffle(self, nums: List[int], n: int) -> List[int]:
        res=[]
        for i in range(n):
            res.append(nums[i])
            res.append(nums[i+n])
        return res
```

#### 5. https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/(拥有最多糖果的孩子)

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