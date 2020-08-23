# lc周赛&双周赛

## 周赛33

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
       []*n   这种是共同索引
       
   ```

   