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



## easy

1. https://leetcode-cn.com/problems/guess-numbers/（猜数字）

   ```python
   class Solution:
       def game(self, guess: List[int], answer: List[int]) -> int:
           count = 0
           for i in range(3):
                if guess[i] == answer[i]:
                   count += 1
           return count
   ```

2. https://leetcode-cn.com/problems/delete-middle-node-lcci/(删除中间节点)

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

3. https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/（左旋字符串）

   ```python
   class Solution:
       def reverseLeftWords(self, s: str, n: int) -> str:
           a=s[:n]
           b=s[n:]
           return b+a
   ```

4. https://leetcode-cn.com/problems/shuffle-the-array/(重新排列数组----1470)

   ```python
   class Solution:
       def shuffle(self, nums: List[int], n: int) -> List[int]:
           res=[]
           for i in range(n):
               res.append(nums[i])
               res.append(nums[i+n])
           return res
   ```

5. https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/(拥有最多糖果的孩子)

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

6. #### [1313. 解压缩编码列表](https://leetcode-cn.com/problems/decompress-run-length-encoded-list/)

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

7. #### [1342. 将数字变成 0 的操作次数](https://leetcode-cn.com/problems/number-of-steps-to-reduce-a-number-to-zero/)

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

8. #### [1389. 按既定顺序创建目标数组](https://leetcode-cn.com/problems/create-target-array-in-the-given-order/)

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

9. #### [1365. 有多少小于当前数字的数字](https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number/)

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
   
10. #### [1295. 统计位数为偶数的数字](https://leetcode-cn.com/problems/find-numbers-with-even-number-of-digits/)

    ```python
    class Solution(object):
        def findNumbers(self, nums):
            """
            :type nums: List[int]
            :rtype: int
            """
            return sum([1 for num in nums if len(str(num)) % 2 == 0])
    ```

11. #### [面试题 04.02. 最小高度树](https://leetcode-cn.com/problems/minimum-height-tree-lcci/)

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

12. #### [1464. 数组中两元素的最大乘积](https://leetcode-cn.com/problems/maximum-product-of-two-elements-in-an-array/)

    ```python
    class Solution:
        def maxProduct(self, nums: List[int]) -> int:
            max_=max(nums)
            nums.remove(max_)
            max__=max(nums)
            return (max_-1)*(max__-1)
    ```

13. #### [1436. 旅行终点站](https://leetcode-cn.com/problems/destination-city/)

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

14. ####  [面试题 02.02. 返回倒数第 k 个节点](https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci/)

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

16. #### [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

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

17. #### [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

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

18. #### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

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

19. #### [1021. 删除最外层的括号](https://leetcode-cn.com/problems/remove-outermost-parentheses/)

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

20. #### [938. 二叉搜索树的范围和](https://leetcode-cn.com/problems/range-sum-of-bst/)

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

21. #### [696. 计数二进制子串](https://leetcode-cn.com/problems/count-binary-substrings/)

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

22. #### [617. 合并二叉树](https://leetcode-cn.com/problems/merge-two-binary-trees/)

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

23. #### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

    ```python
    class Solution:
        def replaceSpace(self, s: str) -> str: 
            res = []
            for c in s:
                if c == ' ': res.append("%20")
                else: res.append(c)
            return "".join(res)
    
    ```

24. #### [1351. 统计有序矩阵中的负数](https://leetcode-cn.com/problems/count-negative-numbers-in-a-sorted-matrix/)

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

25. #### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

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

26. #### [1252. 奇数值单元格的数目](https://leetcode-cn.com/problems/cells-with-odd-values-in-a-matrix/)

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

27. #### [709. 转换成小写字母](https://leetcode-cn.com/problems/to-lower-case/)

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

28. #### [804. 唯一摩尔斯密码词](https://leetcode-cn.com/problems/unique-morse-code-words/)

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

29. #### [832. 翻转图像](https://leetcode-cn.com/problems/flipping-an-image/)

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

30. #### [1323. 6 和 9 组成的最大数字](https://leetcode-cn.com/problems/maximum-69-number/)

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

31. #### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

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

32. #### [1460. 通过翻转子数组使两个数组相等](https://leetcode-cn.com/problems/make-two-arrays-equal-by-reversing-sub-arrays/)

    ```python
    class Solution:
        def canBeEqual(self, target: List[int], arr: List[int]) -> bool:
            return False if sorted(target) != sorted(arr) else True
        
      ##实际上只用考虑，两个数组元素是否一致就好，只要一直交换下去，总可以顺序一致，所以直接排序，作比较
    ##更值得注意的是，arr.sort和sorted（arr）的区别
    ```

33. #### [1309. 解码字母到整数映射](https://leetcode-cn.com/problems/decrypt-string-from-alphabet-to-integer-mapping/)

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

50. #### [1025. 除数博弈](https://leetcode-cn.com/problems/divisor-game/)

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

51. #### [1544. 整理字符串](https://leetcode-cn.com/problems/make-the-string-great/)

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

52. #### [1217. 玩筹码](https://leetcode-cn.com/problems/minimum-cost-to-move-chips-to-the-same-position/)

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

53. #### [1002. 查找常用字符](https://leetcode-cn.com/problems/find-common-characters/)

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















## medium

* 每日一题的任

  

1. #### [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

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

2. #### [43. 字符串相乘](https://leetcode-cn.com/problems/multiply-strings/)

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


3. #### [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

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

   


3. #### [剑指 Offer 64. 求1+2+…+n](https://leetcode-cn.com/problems/qiu-12n-lcof/)

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




4. [109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)

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


6. #### [647. 回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

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

7. #### [529. 扫雷游戏](https://leetcode-cn.com/problems/minesweeper/)

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

8. #### [910. 最小差值 II](https://leetcode-cn.com/problems/smallest-range-ii/)

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

9. #### [861. 翻转矩阵后的得分](https://leetcode-cn.com/problems/score-after-flipping-matrix/)

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

10. #### [491. 递增子序列](https://leetcode-cn.com/problems/increasing-subsequences/)

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

11. #### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

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

12. #### [1552. 两球之间的磁力](https://leetcode-cn.com/problems/magnetic-force-between-two-balls/)

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



13. #### [1482. 制作 m 束花所需的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-make-m-bouquets/)

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
    
14. [332. 重新安排行程](https://leetcode-cn.com/problems/reconstruct-itinerary/)

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


15. #### [5500. 乘积为正数的最长子数组长度](https://leetcode-cn.com/problems/maximum-length-of-subarray-with-positive-product/)

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

16. #### [5501. 使陆地分离的最少天数](https://leetcode-cn.com/problems/minimum-number-of-days-to-disconnect-island/)(并查集方法待补充)

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

17. #### [1503. 所有蚂蚁掉下来前的最后一刻](https://leetcode-cn.com/problems/last-moment-before-all-ants-fall-out-of-a-plank/)

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

18. #### [486. 预测赢家](https://leetcode-cn.com/problems/predict-the-winner/)

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

19. #### [剑指 Offer 20. 表示数值的字符串](https://leetcode-cn.com/problems/biao-shi-shu-zhi-de-zi-fu-chuan-lcof/)

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