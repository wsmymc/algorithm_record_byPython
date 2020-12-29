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

### 3. [LCP 01. 猜数字](https://leetcode-cn.com/problems/guess-numbers/)

```go
func game(guess []int, answer []int) int {
    cnt := 0
    for i,v := range guess{
        if v == answer[i]{
            cnt++;
        }
    }
    return cnt

}
```

### 4. [1431. 拥有最多糖果的孩子](https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/)

```go
func kidsWithCandies(candies []int, extraCandies int) []bool {
    n := len(candies)
    maxCandies := 0
    for _,v := range candies{
        maxCandies = max(maxCandies,v)
    }
    res := make([]bool,n)
    for i:=0;i<n;i++{
        res[i] = (candies[i] + extraCandies)>= maxCandies
    }
    //fmt.Print(maxCandies)
    return res



}
func max(a,b int) int{
    if a<b{
        return b
    }
    return a
}
```



### 5. [1470. 重新排列数组](https://leetcode-cn.com/problems/shuffle-the-array/)

```go
func shuffle(nums []int, n int) []int {
    // go 里面切片需要这么构造make([]type,len,cap)
    res := make([]int,0,2*n)
    n = len(nums)/2
    for i:=0; i<n;i++{
        // append()需要切片
        res = append(res,nums[i])
        res = append(res,nums[i+n])

    }
    return res

}
```



### 6. [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

```go
func reverseLeftWords(s string, n int) string {
    // go中字符 字符串是有变化的，需要用string()转型
    length := len(s)
    res :=""
    for i:= n;i<length;i++{
        res += string(s[i])

    }
    for i:=0;i<n;i++{
        res +=string(s[i])
    }
    return res

}


// 和python一样的切片玩法也可以
func reverseLeftWords(s string, n int) string {
    return s[n:] + s[:n]
}
```

### 7.[面试题 02.03. 删除中间节点](https://leetcode-cn.com/problems/delete-middle-node-lcci/)

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
// 不确定是不是导出的原因，都是大写
func deleteNode(node *ListNode) {
    node.Val = node.Next.Val
    node.Next = node.Next.Next
    
}
```







## medium

### 1. [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)

```go
func canFinish(numCourses int, prerequisites [][]int) bool {
    var(
        edges = make([][]int,numCourses)
        visited = make([]int ,numCourses)
        result []int
        valid = true
        dfs func(u int)
    )

    dfs = func(u int){
        visited[u] = 1
        for _,v:= range edges[u]{
            if visited[v] == 0{
                dfs(v)
                if !valid{
                    return 
                }
            }else if visited[v] ==1{
                valid = false
                return 
            }
        }
        visited[u] =2
        result = append(result,u)
    }

    for _,info := range prerequisites{
        edges[info[1]] = append(edges[info[1]],info[0])
    }
    for i:= 0;i<numCourses && valid;i++{
        if visited[i] ==0{
            dfs(i)
        }
    }
    return valid
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

