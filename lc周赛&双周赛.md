# lc周赛&双周赛



## 双周赛33

#### 1. [千位分隔数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/thousand-separator/)

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

#### 2. [可以到达所有点的最少点数目](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-number-of-vertices-to-reach-all-nodes/)

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



#### 3. [得到目标数组的最少函数调用次数](https://leetcode-cn.com/contest/biweekly-contest-33/problems/minimum-numbers-of-function-calls-to-make-target-array/)

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



#### 4. [二维网格图中探测环](https://leetcode-cn.com/contest/biweekly-contest-33/problems/detect-cycles-in-2d-grid/)

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







## 双周赛34

#### 1.[5491. 矩阵对角线元素的和](https://leetcode-cn.com/problems/matrix-diagonal-sum/)

```python
class Solution:
    def diagonalSum(self, mat: List[List[int]]) -> int:
        n = len(mat)
        sum = 0
        for i in range(n):
            sum += mat[i][i]
        for i in range(n):
            if i == n-i-1:
                continue
            sum += mat[i][n-i-1]
        return sum
```

#### 2. [5492. 分割字符串的方案数](https://leetcode-cn.com/problems/number-of-ways-to-split-a-string/)

```python
# 必死啊是没有思路，硬写出来的模拟，应该有更巧妙的方法
class Solution:
    from scipy.special import perm,comb  # 用来做排列数，或者组合数运算的
    def numWays(self, s: str) -> int:


        pin = 10**9 +7
        cnt = 0

        for i in range(len(s)):
            if s[i] == '1':
                cnt += 1
        if cnt == 0:
            return comb(len(s)-1,2)%pin
        if cnt % 3 != 0:
            return 0
        n = cnt //3
        l = r =-1
        sum = 0
        for i in range(len(s)):
            if sum == n:
                l = i
                break
            if s[i] == '1':
                sum +=1
                if sum == n:
                    l = i
                    break
        #print("l",l)
        sum =0
        for i in range(len(s)-1,-1,-1):
            if sum == n:
                r = i
                break

            if s[i] == '1':
                sum +=1
                if sum == n:
                    r = i
                    break
        #print("r", r)
        sum =0
        in_l=in_r =-1
        for i in range(l+1,len(s)):
            if s[i]== '1':
                in_l =i
                break
        for i in range(r-1,-1,-1):
            if s[i] == '1':
                in_r=i
                break
        #print(in_l, in_r)
        #print((in_l-l))
        #print((r-in_r))
        return (in_l-l)*(r-in_r)%pin
    
    
# 基本思路是一样的选定指标，然后数学计算，巧妙在于另起炉灶，直接统计1的坐标，然后以这个作为指标，计算三等分点，这样就可以略过0的影响。    
class Solution:
    def numWays(self, s: str) -> int:
        news = [i for i,num in enumerate(s) if num=='1']
        k = len(news)
        if k%3:return 0
        if not k:return (len(s)-1)*(len(s)-2)//2%1000000007        
        return (news[k//3]-news[k//3-1])*(news[k//3*2]-news[k//3*2-1])%1000000007

作者：sunrise-z
链接：https://leetcode-cn.com/problems/number-of-ways-to-split-a-string/solution/chun-shu-xue-jie-jin-o1jie-fa-by-sunrise-z/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

#### 3.[5493. 删除最短的子数组使剩余数组有序](https://leetcode-cn.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)

```python
# 这道题基本算是硬做的的，因为有些边界没想通，都是跑到一个用例，修改一个。基本就是从左和从右边找到不和谐的起点，然后根据端点的位置，向左向右挪动，找到最小值


from typing import List


class Solution:
    def findLengthOfShortestSubarray(self, arr: List[int]) -> int:
        l= r =-1
        if len(arr) <=1:
            return  0
        for i in range(len(arr)-1):
            if arr[i] >arr[i+1]:
                l = i
                break
        for i in range(len(arr)-1,0,-1):
            if arr[i]<arr[i-1]:
                r= i
                break
        #print(l,r)
        if r-l+1 == len(arr):
            #print("===")
            return  r-l if arr[0]>arr[-1] else r-l-1
        elif r == -1 and l == -1:
            return 0
        else:
            #print("ooo")
            flag =-1
            new_l = new_r =-1
            res = len(arr)
            for i in range(r,len(arr)):
                if arr[i]>= arr[l]:
                    flag =1
                    new_r = i
                    break
            if flag == 1:
                res = min(res,new_r -l -1)
            else:
                res = min(res,len(arr)-l-1)
            flag = -1
            for i in range(l,-1,-1):
                if arr[i] <= arr[r]:
                    flag =1
                    new_l = i
                    break
            if flag == 1:
                res = min(res,r-new_l-1)
            else:
                res = min(res,r)

            return res
```

#### 4. [5494. 统计所有可行路径](https://leetcode-cn.com/problems/count-all-possible-routes/)

```python
# 这道题才是重点，我也就没做出来这道题。一开始想用dfs，不过可能出问题，没有用好

from functools import lru_cache
from typing import List


class Solution:
    def countRoutes(self, locations: List[int], start: int, finish: int, fuel: int):
        n = len(locations)
        @lru_cache(None)
        def dp(s,f,c):
            if c < 0:
                return 0
            res = 0
            if f!=s and abs(locations[f]-locations[s]) <= c: # 关键点在这里，从这一点s到finis的花费小于
                # 当前燃料值，所以认为从此处到finis是一个可能，res+1，然后可以接着向下走
                res += 1
            for i in range(n):
                if i!=s:
                    res += dp(i,f,c-abs(locations[s]-locations[i]))  # 说是dfs也行，说是dp也可以
            return res

        res = dp(start,finish,fuel)
        if start == finish:
            res += 1
        return res%(10**9+7)
```













# 周赛203

####  1.[5495. 圆形赛道上经过次数最多的扇区](https://leetcode-cn.com/problems/most-visited-sector-in-a-circular-track/)

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

#### 2. [5496. 你可以获得的最大硬币数目](https://leetcode-cn.com/problems/maximum-number-of-coins-you-can-get/)

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

#### 3. [5497. 查找大小为 M 的最新分组](https://leetcode-cn.com/problems/find-latest-group-of-size-m/)

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

#### 4.[5498. 石子游戏 V](https://leetcode-cn.com/problems/stone-game-v/)

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





## 周赛205（未完）

#### 4.[5510. 保证图可完全遍历](https://leetcode-cn.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)

```python
# 看题解主流是并查集的玩法，我这种使用集合计算的反而在少数。enmmm，确实要学习了。不过今天还有别的事，暂先放下
```

##  2020力扣秋季个人赛

#### 1. 速算机器人（极简单，不计）

#### 2. 早餐组合

```python
from typing import List

# 一开始被超时折腾死，后面才想到保存上一次走过的指针，每次从上一循失败后做微调，即可压缩时间复杂度
class Solution:
    def breakfastNumber(self, staple: List[int], drinks: List[int], x: int) -> int:
        self.res = 0
        pin = 1000000007
        staple.sort()
        drinks.sort()
        r = len(staple)-1
        l = 0
        for i in range(r,-1,-1):
            #print("l",l)
            #print("i",i)
            for j in  range(l,len(drinks)):
                if staple[i]>=x:
                    #print('1')
                    break
                if drinks[j]>=x:
                    #print('2')
                    return self.res
                if staple[i]+drinks[j]>x:
                    #print('4')

                    l = j
                    break

                if j == len(drinks)-1:

                    if staple[i]+drinks[j] <=x:
                        self.res = (self.res +(j+1)%pin*(i+1)%pin)%pin
                        return self.res
```







## 207周赛

#### 1 [5519. 重新排列单词间的空格](https://leetcode-cn.com/problems/rearrange-spaces-between-words/)

```python
## 自己写的，很狗屎，逻辑一样，毕竟是简单题，但是实现起来步骤冗余，没有利用到python的简洁特性

class Solution:
    def reorderSpaces(self, text: str) -> str:
        cnt1 = 0
        cnt2 = 0
        res = []
        tmp = []
        for i in range(len(text)):
            if text[i] == ' ':
                cnt1 += 1
            else:
                if i == 0 or text[i-1] == ' ':
                    cnt2 +=1
        i = 0
        #print(cnt1,cnt
        if cnt1 == 0:
            return text
        while i<len(text):
            #print(tmp)
            if text[i] != ' ':
                tmp.append(text[i])
                if i == len(text)-1:
                    res.append(''.join(tmp))
            else:
                if tmp:
                    res.append(''.join(tmp))
                    tmp = []
                else:
                    pass
            i += 1
        cnt3 = len(res)-1
        if cnt3 == 0:
            return res[0]+' '*cnt1
        print(cnt3)
        skip = ' '*(cnt1//cnt3)
        t = res[0]
        #print(res)
        for i in range(1,len(res)):
            t += skip + res[i]
        t += ' '*(cnt1%cnt3)
        #print(t)
        return  t
# 更加python的写法
  class Solution:
    def reorderSpaces(self, text: str) -> str:
        cnt = text.count(' ')   # count直接统计字符串里的字符
        n = text.split()        # split在默认的情况下，以所有空字符分割，指空格、tab、换行
        if len(n) == 1:  # 只有一个单词的情况
            return text.strip() + ' '*cnt
        space, last = divmod(cnt,len(n)-1)  # 内建函数，直接获得数的商、余数
        return (' '*space).join(n)+' '*last  
    
```

#### 2. [5520. 拆分字符串使唯一子字符串的数目最大](https://leetcode-cn.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)

```python
## 很简单的递归回溯，问题在于，模板不熟练，再加上循环的边界有些不清晰，反复调试bug浪费了太多时间，这一题差不多花了45分钟，超标太多了
class Solution:
    def maxUniqueSplit(self, s: str) -> int:
        _res = 1
        n = len(s)
        _s = set()
        #print(_s)
        def _backtrace(_s,idx,res):
            nonlocal  _res
            #print(_s,'idx',idx)
            if n-idx+len(_s) <= _res:  # 新加入的剪枝，如果在当前条件下，即便将剩下的单个字符分割，结果仍旧比不上已有的值，就不用再递归下去了
                return
            if idx >= n:
                #print('jiesuan')
                _res = max(_res,len(_s))
            for i in range(idx+1,n+1): # 要成功取到最后一位，需要首尾都向后挪一位
                t = s[idx:i]
                #print(idx,i,t)
                #print('t',t,t not in _s)
                t= ''.join(t)

                if t not in _s:
                   # print(_s,t)
                    _s.add(t)
                    #print('记入', _s, t)
                    _backtrace(_s,i,res)
                    _s.remove(t)

        _backtrace(_s,0,1)
        return _res


```

#### 3. [5521. 矩阵的最大非负积](https://leetcode-cn.com/problems/maximum-non-negative-product-in-a-matrix/)

```python
# 做出来了，但是费时太久，等到比赛结束才调试成功。
# 1. 很多模板不熟练，题目稍微变一下，就知道怎么做了
# 2. 敲代码要仔细，既要看清自己到底敲对字符没有，还要想明白细节的逻辑到底对不对。

from typing import List


class Solution:
    def maxProductPath(self, grid: List[List[int]]) -> int:
        if not grid or grid[0] == 0:
            return -1
        row = len(grid)
        col = len(grid[0])

        dp = [[-1]*col for _ in range(2)]
        for i in range(1,col):
            grid[0][i] *= grid[0][i-1]
        for i in range(1,row):
            grid[i][0] *= grid[i-1][0]
        if row ==1 or col ==1:
            return grid[row-1][col-1] if grid[row-1][col-1]>=0 else -1
        #print(grid)
        for i in range(1,row):
            for j in range(1,col):
                #print('j', j)
                a,b,c,d =0,0,0,0
                if i == 1 :
                    a=b=grid[0][j]
                else:
                    a= dp[0][j]
                    #print('a',a)
                    b=dp[1][j]
                    #print('b',b)

                if j == 1:
                    c =d=grid[i][0]
                else:
                    c = dp[0][j-1]
                    d = dp[1][j-1]
                    #print(c,d)


                dp[0][j] =max(a*grid[i][j],b*grid[i][j],c*grid[i][j],d*grid[i][j])
                dp[1][j] =min(a*grid[i][j],b*grid[i][j],c*grid[i][j],d*grid[i][j])
                #print(dp,a,b,c,d)
        return dp[0][col-1]%(10**9+7) if dp[0][col-1]>=0 else -1

```

#### [1595. 连通两组点的最小成本](https://leetcode-cn.com/problems/minimum-cost-to-connect-two-groups-of-points/)（没搞懂！！）

```python
class Solution:
    # 状态压缩dp没有接触过的概念，本质上就是用二进制来代替集合，进而减少dp的维度
    def connectTwoGroups(self, cost: List[List[int]]) -> int:
        @lru_cache(None)
        def dp(i,state):
            nonlocal m 
            if i < 0:  # 到头
                if state == 0:  # 点全部连接完
                    return 0
                else:   # 没有连接完
                    return float('inf')
            
            res = float('inf')
            x = 1
            for j in range(m):
                if state & x > 0:  # 该点已经被连接了，那就要考保留该点的连接，跳过，还是不跳过
                    res = min(res, cost[i][j] + dp(i,state^x), cost[i][j] + dp(i-1, state^x))
                else:  # 该点没有被连接 ，那就要按照连接走
                    res = min(res, cost[i][j] + dp(i-1, state ))
                
                x <<= 1
            return res
        
        n = len(cost)
        m = len(cost[0])
        state = (1<<m)-1 # 这样二进制有m+1位，且除最高位外全是1
        
        return dp(n-1,state)  # 从后向前开始统计

```









## 35 双周赛

####1 

#### 2  [1589. 所有排列中的最大和](https://leetcode-cn.com/problems/maximum-sum-obtained-of-any-permutation/)

```python
class Solution:
    # 差分数组解法，又是没有接触过的知识点
    # 就是原来数组i位置上的元素和i-1位置上的元素作差，就是差分数组 i的值
    # 实质上记录的是变动值，然后从前向后累加，就是所有操作做完以后原数组实际的值
    def maxSumRangeQuery(self, nums: List[int], requests: List[List[int]]) -> int:
        n=len(nums)
        arr=[0]*(n+1)
        for l,r in requests:
            arr[l]+=1
            arr[r+1]-=1
        for i in range(1,n):
            arr[i]+=arr[i-1]
        arr.sort(reverse=True)
        nums.sort(reverse=True)
        return sum(map((lambda x,y:x*y),arr,nums))%(10**9+7)   # map的玩法还是有意思，要学习

```

#### 3 [1590. 使数组和能被 P 整除](https://leetcode-cn.com/problems/make-sum-divisible-by-p/)

```python
class Solution:
    # 同余定理即是如果两个数字a，b除以target的余数相等，则这两个数字的差(a - b)能被target整除。
    # 利用前缀和取余得到相对应的index。这里的“相对应”指的是余数差正好为数组和对p取得余数，这样当我们去掉这两个对应indices之间的子数组，我们将得到一个被p整除的数组。

    def minSubarray(self, nums: List[int], p: int) -> int:
        m = sum(nums)%p   # 先求余数
        if m == 0: return 0
        pre = 0
        dic = {0: -1}
        res = float('inf')  # 先初始化一个极大值
        for i, num in enumerate(nums):
            pre += num   # 前缀求和
            cur = pre%p  # 提前求余作为剪枝
            dic[cur] = i  # 存放字典待用
            if cur + p * int(cur < m) - m in dic:    # cur + p * int(cur < m) - m  指的是cur + p -m  或者cur -m  具体看cur、m的大小关系。这两个数 %p  都是 cur 而且是正的
                res = min(res, i - dic[cur + p * int(cur < m) - m])
        return res if res < len(nums) else -1

```

