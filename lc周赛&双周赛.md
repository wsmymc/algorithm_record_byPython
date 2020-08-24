# lc周赛&双周赛

## 双周赛33

1. [千位分隔数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/thousand-separator/)

   ```python
    n = str(n)
           l = list()
           count =0
           for i in range(len(n)):
               l.append(n[-(i+1)])  #从后向前走
               count +=1
               if count%3 ==0 and (-i-1)!=-len(n):
                   l.append(".")
   
           l.reverse() #列表可以反转
           return "".join(l)  # 字符串化
   ```

2. [可以到达所有点的最少点数目](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-number-of-vertices-to-reach-all-nodes/)

   ```python
   class Solution:
       def findSmallestSetOfVertices(self, n: int, edges: List[List[int]]) -> List[int]:
           s1 = set()
           for e in edges: # 集合统计出度
               s1.add(e[1])
   
           l = list()
           for i in range(n):
               if not(i in s1):  # 不是出度——就是入度
                   l.append(i)
           return  l
       
       # 一开始想复杂了，其实就是集合统计出度，然后判断谁不是出度——就是入度
   ```



3. [得到目标数组的最少函数调用次数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-numbers-of-function-calls-to-make-target-array/)

   ```python
   class Solution:
       def minOperations(self, nums: List[int]) -> int:
           p_c, m_c=0,0  #分别表示pluscount ,mutlipy_count
           for i in nums:
               if i == 0:  # 如果为0，说明没变化，跳过
                   continue
               tmp=i  # 每次对当前数操作
               cnt=0  # 记录乘的次数
               while tmp>1:    
                   last = tmp%2   # 每次取余，有的话，说明是有+的操作
                   tmp= tmp>>1  
                   cnt+=1        #每次除，说明是*，因此局部+1
                   if last>0:   # 如果有加的操作，就全局加1
                       p_c+=1
               
               p_c +=1
               m_c=max(m_c,cnt)   # 统计每个数里，最大的乘数
           return p_c+m_c   #累计两种操作
       
       
      # 想复杂了，不用太关注规律，只要知道，找到，最大的数，每次它*2，其他的数都是顺带，然后观察每个数，要达到这个数，除了*2外，+1的次数，说白了，还是遍历。不过这时候就从二维变成一维，因为不用考虑乘了
   
   
   # 理解了这里有一个最简的，逻辑一样
   class Solution:
       def minOperations(self, nums: List[int]) -> int:
           c = 0
           for i in nums:
               c += bin(i).count("1")  # 因为如果是乘，1只会移动，不会增加，在二进制里，所以统计1的个数，实际就是在统计加的次数
           b = 2
           while b <= max(nums): # 直接找最大值作为标的，看多久能够达到，因为乘法最快，实际上统计乘法次数。这里b=2开始，是因为会有些3，5等数字，不是直接乘出来的，实际应该是乘到不能乘，再加，所以相当于提前乘了个2，不计入统计。否则后面还有if判断
               c += 1
               b *= 2
           return c
   ```



4. [二维网格图中探测环](https://leetcode-cn.com/contest/biweekly-contest-33/problems/detect-cycles-in-2d-grid/)

   ```python
   # 递归思路，只是在写的时候力不从心……ORZ
   
   import sys
   sys.setrecursionlimit(100000000)   # 这里是再设置递归层数，并非限制，而是增大否则这一方法无法运行，因为递归太深了
   class Solution:
       def containsCycle(self, grid: List[List[str]]) -> bool:
           self.flag=False
           self.visited={}    # 用vistited来表示是否访问过
           moves = [[1, 0], [-1, 0], [0, 1], [0, -1]]   # 移动方向
           def dfs(x,y,prex,prey):
               if (x,y) in self.visited:   # 回到原点，说明成环
                   self.flag=True
                   return
               self.visited[(x,y)] = True  # 记录访问
               for move in moves:
                   dx=move[0]
                   dy = move[1]
                   if not (x + dx == prex and y + dy == prey) and 0 <= x + dx < len(grid) and 0 <= y + dy < len(
                       grid[0]) and grid[x + dx][y + dy] == grid[x][y]:   # 需要限制方向，
                       dfs(x+dx,y+dy,x,y)
           for i in range(len(grid)):
               for j in range(len(grid[0])):
                   if (i,j) not in self.visited:
                       dfs(i,j,-1,-1)
           return self.flag
       
       
       # 错误原因，第一点： visited = [[True for i in range(m)] for i in range(n)]，二位矩阵要这么创建，否则：
      # [x]*n   这种是共同索引
       
   ```

   











# 周赛203

* #### [5495. 圆形赛道上经过次数最多的扇区](https://leetcode-cn.com/problems/most-visited-sector-in-a-circular-track/)

  ```python
  # 一开始被模拟法误导了，陷在里面浪费很多时间，事情的本质，是考察起点和终点，因为其他都是绕圈，只有不满圈的那一部分，会比其他部分多1次
  
  class Solution:
      def mostVisited(self, n: int, rounds: List[int]) -> List[int]:
          li =list()
          if len(rounds) == 1:
              return rounds[0]
          else:
              l = rounds[0]
              r = rounds[-1]
              if l < r:    # 正常情况是从左到右
                  # print("s")
                  li = [i for i in range(l, r + 1)]
                  return li
              elif l == r:
                  return [l]  # 相同的话，只用考虑这个
              elif l > r:   #非正常情况从右到左，这个时候，先把剩下的跑满，在考虑重新出发的部分
  
                  #print(lp)
                  for i in range(l, n + 1):  
                      li.append(i)
                  #print(lp)
                  for i in range(1, r + 1):
                      li.append(i)
                  #print(li)
  
                  li.sort() # 这一点差点搞死我，记住，这块sort后没有返回值，所以不能直接return，要return li
                  #print(li)
                  return  li
              
              
  ```

* #### [5496. 你可以获得的最大硬币数目](https://leetcode-cn.com/problems/maximum-number-of-coins-you-can-get/)

  ```python
  # 最简单的贪心，每一轮，我要拿第二，随意，必需和第一十分接近——排序，然后第三直接扔到最后一部分，实际上是排序后，间隔取数，取满n个
  class Solution:
      def maxCoins(self, piles: List[int]) -> int:
          piles.sort(reverse=True)
          #print((piles))
          n = len(piles)//3
          res = 0
          for i in range(n):
              #print(piles[2*i+1])
              res +=piles[2*i+1]
  
          return res
      
      #其实没什么好说的，比第一题用时还少，看题解时，有一个这个，可以看看
  class Solution:
      def maxCoins(self, piles: List[int]) -> int:
          # return sum(sorted(piles)[len(piles) // 3::2])
          print("sorrted:",sorted(piles))            #测试用的，实际只有最后一句话
          print(sorted(piles)[len(piles) // 3::2])  # 用来找原理的，很巧妙，因为第三有n个（0~n-1），直接从n开始，步长为2。
          # sorted（list）[start:end:step]这种用法值得记一下
          return sum(sorted(piles)[len(piles) // 3::2])   
  ```

* #### [5497. 查找大小为 M 的最新分组](https://leetcode-cn.com/problems/find-latest-group-of-size-m/)

  * 反思一个错误，注意是“最新”分组，我读题不仔细，当前没有的话，不代表下一次不会产生，我自己的代码是太早退出了……ORZ

  ```python
  import collections
  from typing import List
  
  '''
  来自题解的理解，我读懂了，偷懒复制：（实质还是暴力解，每次统计，然后有意思的是修改节点对应1的长度时，只修改端点的，相当于O(1)操作了。
  当然，总体是O（N））
  分析题目, 某个位置变成 1 之后, 最直接的影响就是其左右两边, 左右两边连续的 1 的长度都会变成新的总长度
  所以首先我们需要一个字典 iToLen, 键值对为{i:len}, 存储某个下标对应的连续 1 的长度, 用于动态更新
  其次我们还需要一个反向字典 lenToCnt, 键值对为{len:cnt}, 存储当前连续 1 长度对应的个数, 那么每次只要这个字典里 m 对应的 cnt 大于 0, 就说明仍有连续 1 长度为 m 的部分
  如果我们在每次把某个位置变成 1 之后都修改左右两边所有连续 1 的位置的 iToLen 字典, 这样时间复杂度就达到了 O(N^2), 大概率会超时
  但真的有必要修改所有下标吗? 答案是否定的, 其实我们只需要修改新的连续 1 的起点和终点的 iToLen 字典即可, 因为后面操作里新的 1 的左右两边绝不可能是当前连续 1 的中间部分, 只需要考虑两个边界就行, 这样就把这部分操作从 O(N)降到了 O(1)
  然后就是修改反向字典 lenToCnt 了, 这个也很简单, 就是拿到原来左右两侧连续 1 的长度 left 和 right, 将其对应的值各减去 left 和 right, 因为这些下标的长度都不再是原来的值了, 然后再把总长度 left+right+1 在 lenToCnt 字典中的值加上 left+right+1 即可
  下面代码对必要的步骤有详细的解释, 方便大家理解
  
  作者：suibianfahui
  链接：https://leetcode-cn.com/problems/find-latest-group-of-size-m/solution/di-203-chang-zhou-sai-ti-jie-by-suibianfahui-2/
  来源：力扣（LeetCode）
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  '''
  class Solution:
      def findLatestStep(self, arr: List[int], m: int) -> int:
          n = len(arr)
          lenToCnt = collections.defaultdict(int)   # 设置默认值的类型
          iToLen={}
          res =-1
          for index x in enumerate(arr):  #这两天常见，使生成索引序列，k-v格式
              i = x-1   #转换成以0为起点的操作,方便使用
              left = 0
              right = 0  ## 原来的左侧和右侧的连续1的长度
              start = end = i # 新的连续1的起点和终点下标, 初始化为当前下标
  
              if i-1 >=0 and i-1 in iToLen:  # 首先没有越界，其次，i-1是某段连续1的右端点，因为只
                  left =iToLen[i-1]          #有为1，才会在tTolen的字典里。取被填补缺口的左侧长度
                  start -=left               # 其实质:i-left,找到左端点的索引号
              if i+1 <n and i+1 in iToLen:    # 类似上面
                  right = iToLen[i+1]
                  end +=right
              newLen = left +right +1         # 左边长度+右边长度+自己（1）
              iToLen[start] = iToLen[end] = newLen  #这是更新新的左右端点的在字典里的对应长度
  
              lenToCnt[left] -= 1           # 原本的左边长度要减少
              lenToCnt[right] -= 1         #原本右边长度对应的计数也要减少
              lenToCnt[newLen] += 1
  
              if lenToCnt[m] > 0:   # 如果当前操作完成后，m长度对应的计数还有，那么就更新为当前操作，之所以+1，是因为enumerate生成的索引从0开始
                  res = index +1
          return res
  
  
  # 当然这里最聪明的办法是倒着做，什么时候第一次出现，就是什么时候最后更新。具体可以到后面实现  实现如下
  
  
  
  #bisect.bisect(a, x, lo=0, hi=len(a))
  #类似于 bisect_left()，但是返回的插入点是 a 中已存在元素 x 的右侧。
  #有点像Java里的TreeSet()
  #返回的插入点 i 可以将数组 a 分成两部分。左侧是 all(val <= x for val in a[lo:i])，右侧是 all(val > x for val in a[i:hi]) for the right side。
  '''
  bisect.bisect_left(a, x, lo=0, hi=len(a))
  在 a 中找到 x 合适的插入点以维持有序。参数 lo 和 hi 可以被用于确定需要考虑的子集；默认情况下整个列表都会被使用。如果 x 已经在 a 里存在，那么插入点会在已存在元素之前（也就是左边）。如果 a 是列表（list）的话，返回值是可以被放在 list.insert() 的第一个参数的
  '''
  
  class Solution:
      def findLatestStep(self, arr: List[int], m: int) -> int:
          if m == len(arr):
              return  m
          l = [0,len(arr)+1]   #两个端点？虚端点，用于计算方便
          for i, t in enumerate(arr[::-1]):   # 倒着走
              idx = bisect.bisect(l,t)   #查找插入后的索引 ,尝试手动加索引，超时。这个结构的好处是能够用二分缩短时间
              l.insert(idx,t)  # 在查找好索引后的指定位置插入数据
              if l[idx+1]-l[idx]-1 ==m or l[idx]- l[idx-1]-1 == m:#此即为这次更新的位置左右连续1的个数， 已经考察过的位置就不用再看了，必定不符合条件。这里运算是做差后-1，因为两端都是0，而不是1，仔细想想
                  return len(arr)-(i+1)  #  原因同上，i 是从0开始，操作是从1开始，需要吻合
          return -1
  
  
  
  
  
  
  
  ```

* #### [5498. 石子游戏 V](https://leetcode-cn.com/problems/stone-game-v/)

  ```python
  from typing import List
  
  # 看题解说是经典动态规划问题，问题是我没机会见到，见到了，也不会……
  '''
  时间复杂度：O(N^3) ,字典里最多有n^2个状态，每次遍历n个数
  元组这里最多会有n**2个，所以，空间复杂度是n**2（这里都需要内部连续，例如长为2，则只有n-1中科能，前n项和公式）
  
  
  ====
  一般对于这种博弈问题, 都可以先尝试用记忆化搜索的思路来解决, 这个题也不例外
  但这道题不需要双方做最优决策, 只需要 Alice 一个人来分, 所以不需要额外一个 flag 来判断当前是谁的回合
  直接模拟整个过程, 传入当前元组(转成元组的目的是可以作为 memo 字典的 key), 然后依次遍历当前元组, 动态求得左侧和右侧部分的和, 根据题目描述的情况继续递归, 最终求得最大值作为当前元组的最终结果, 加入 memo 字典中
  递归出口是元组长度为 1 的时候, 此时游戏结束, 直接返回 0 即可
  下面代码对必要的步骤有详细的解释, 方便大家理解
  
  作者：suibianfahui
  链接：https://leetcode-cn.com/problems/stone-game-v/solution/di-203-chang-zhou-sai-ti-jie-by-suibianfahui/
  来源：力扣（LeetCode）
  著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
  '''
  class Solution:
      def stoneGameV(self, stoneValue: List[int]) -> int:
          memo = {}    # 动态规划常有的记忆结构，其实动态规划本身就是一种记忆化搜索
          def getMax(t):
              if len(t) == 1:
                  return 0   # 只有1个石子，退出递归
              if t not in memo:     # 避免发生的重复，因为这里有递归，一旦有递归，就一定会有重复，可以减少计算
                  sm = sum(t)
                  leftsum =0
                  mx =0
                  for i in range(len(t)-1):
                      leftsum += t[i]
                      rightsum = sm - leftsum
                      if leftsum < rightsum:  # 分情况递归
                          mx = max(mx, leftsum+getMax(t[:i+1]))
                      elif leftsum == rightsum:
                          mx = max(mx ,leftsum+getMax(t[:i+1]), rightsum+getMax(t[i+1:]))
                      else:
                          mx = max(mx, rightsum+ getMax(t[i+1:]))
  
                  memo[t] = mx    # 记忆结果
              return memo[t]      # 返回
          return getMax(tuple(stoneValue))    # 注意这里用tuple(),将传入的数组元组化，这用与在memo最为key
  
  
      
      
    
      # 上面是记忆化搜索，下面是正规的动态规划
      import sys
  sys.setrecursionlimit(999999999)   #用来设置递归层数，防止栈深度不够
  
  from typing import List
  class Solution:
      def stoneGameV(self, stoneValue: List[int]) -> int:
          n = len(stoneValue)
          S = [val for val in stoneValue]
          for i in range(1, n):
              S[i] += S[i-1]
  
          # 区间i到j数值能够取到的最大值
          #from functools import lru_cache       用来设置缓存，提升速度，防止超时
          @lru_cache(typed=False, maxsize=1280000000)
          def dp(i, j):
              if i == j:
                  return 0
  
              start_val = 0 if i == 0 else S[i-1]
              total = S[j] - start_val
  
              ans = 0
              for mid in range(i, j):
                  part1 = S[mid] - start_val
                  part2 = total - part1
  
                  tmp = None
                  if part1 < part2:
                      tmp = part1 + dp(i, mid)
                  elif part1 > part2:
                      tmp = part2 + dp(mid+1, j)
                  else:
                      tmp = max( part1 + dp(i, mid), part2 + dp(mid+1, j) )
  
                  ans = max(ans, tmp)
  
              return ans
  
          return dp(0, n-1)
  
  
  ```
  
  