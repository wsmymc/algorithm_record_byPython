# go_leetcode记录

## easy

### 1. [961. 重复 N 次的元素](https://leetcode-cn.com/problems/n-repeated-element-in-size-2n-array/)

```go
func repeatedNTimes(A []int) int {
    arr := make(map[int]int)
    var tmp int
    for _, v := range A{
        arr[v]++
        if arr[v] ==2{
            tmp = v
            return v
        }
    }
    return tmp

}
```





### 2. [1037. 有效的回旋镖](https://leetcode-cn.com/problems/valid-boomerang/)

```go
func isBoomerang(points [][]int) bool {
    var res = true
    if points[0][0] == points[1][0] && points[0][0] == points[2][0] || 
            points[0][0] == points[1][0] && points[0][1] == points[1][1] || 
            points[0][0] == points[2][0] && points[0][1] == points[2][1] || 
            points[1][0] == points[2][0] && points[1][1] == points[2][1]{
        res = false
        return res
    }
    if float64(points[0][1]-points[1][1])/float64(points[0][0]-points[1][0]) == float64(points[0][1]-points[2][1])/float64(points[0][0]-points[2][0]){
        res = false
    }
    return res
}


```







## hard

### 1. [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```go
func maxProfit(k int, prices []int) int {
    n := len(prices)
    if n==0{
        return 0
    }
    k = min(k,n/2)
    buy := make([][]int,n)
    sell := make([][]int,n)
    for i:= range buy{
        buy[i] = make([]int , k+1)
        sell[i] = make([]int ,k+1)
    }

    buy[0][0] = -prices[0]
    for i:= 1;i<=k;i++{
        buy[0][i] = math.MinInt64 / 2
        //sell[0][i] = 0;
    }

    for i:=1;i<n ;i++{
        for j:=0;j<=k;j++{
            if j==0{
                sell[i][j]=0
            }
            if j>0{
                sell[i][j] = max(buy[i-1][j-1]+prices[i], sell[i-1][j])
            }
            buy[i][j] = max(buy[i-1][j],sell[i-1][j]-prices[i])
        }
    }
    // for i := 1; i <= k; i++ {
    //     buy[0][i] = math.MinInt64 / 2
    //     sell[0][i] = math.MinInt64 / 2
    // }

    // for i := 1; i < n; i++ {
    //     buy[i][0] = max(buy[i-1][0], sell[i-1][0]-prices[i])
    //     for j := 1; j <= k; j++ {
    //         buy[i][j] = max(buy[i-1][j], sell[i-1][j]-prices[i])
    //         sell[i][j] = max(sell[i-1][j], buy[i-1][j-1]+prices[i])
    //     }
    // }
    return max(sell[n-1]...)
}
func min(a,b int) int {
    if a<b{
        return a
    }
    return b
}
func max(a ... int) int {
    res := a[0]
    for _, v := range a[1:]{
        if v>res{
            res =v
        }
    }
    return res

}
```

