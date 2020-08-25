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

    * | [`deque`](https://docs.python.org/zh-cn/3/library/collections.html#collections.deque) | 类似列表(list)的容器，实现了在两端快速添加(append)和弹出(pop) |
      | ------------------------------------------------------------ | ------------------------------------------------------------ |
      |                                                              | 相关用法点击链接                                             |

=======



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

46. #### [459. 重复的子字符串](https://leetcode-cn.com/problems/repeated-substring-pattern/)（待解决8.23）















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

   

