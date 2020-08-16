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

7. 





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

21. #### 733. 图像渲染](https://leetcode-cn.com/problems/flood-fill/)

    （周一填补）

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

   