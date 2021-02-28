# codility

## MinAbsSum

```python
# you can write to stdout for debugging purposes, e.g.
# print("this is a debug message")
# 一开始把题目想简单了，这是一个多重背包问题，因为1，-1可变，实际上就是视为正数，分成两队，差值最小
def solution(A):
    # write your code in Python 3.6
    has  = [0]*101   # 用来统计每个数可以使用的次数
    M = sum_ = 0
    n = len(A)
    for i in range(n):
        a = abs(A[i])
        has[a] +=1
        sum_ += a
        M = max(M, sum_)  # 通过求max,间接求target
    
    target = (sum_//2) + 1  # 这可以看作背包的容量

    dp = [-1] * target      # 记录背包价值，-1初始化
    can =0                 # 统计是背包价值超标
    dp[0] = 0               # 0 不存在，设置为0
    for i in range(1,M+1):
        if has[i]:
            for j in range(len(dp)):
                if dp[j]>=0:   # 之前已经初始化的话，就不是-1了，这时候背包价值
                    dp[j] = has[i]
                    can = max(can,j)
                else: # 初始计算
                    if j-i >=0 and dp[j-i]>0:  # 首先j-i 还有空间，其次，j-i对应的值已经初始化了
                        dp[j] = dp[j-i]-1
                        can = max(can,j)
        if can>=target:
            break
    return abs(sum_ -(can<<1))



```



