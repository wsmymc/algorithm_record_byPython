# C++_algorithm

## 前言:

​	一个小小的尝试，不知道能坚持或者付出多少，希望有一点点收获

## 零碎语法点

1. 内置算法：swap

2. to_string：

   ```c++
   string to_string(int val);
   string to_string(long val);
   string to_string(long long val);
   string to_string(unsigned val);
   string to_string(unsigned long val);
   string to_string(unsigned long long val);
   string to_string(float val);
   string to_string(double val);
   string to_string(long double val);
   ```

3. stoi()   string-->int

4. for (auto &i : numStr) 和for (auto i : numStr) 一致。区别在于前者可以改变numStr自身的内容，而后者只能够只读？

5. // 内置函数sort(),排序使用







## easy

### 1. [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        /*
        1. 第一个C++,刷题，一点小激动
        2. C++有内置算法，比如这里用到的swap，就很舒服
        3. if 中可以放数值1，这里其实和python有有点像了
        4. 当然，反复括号肯定是像Java
        */
        int n = nums.size(), left = 0, right =0;
        while(right <n){
            if(nums[right]){
                swap(nums[left], nums[right]);
                left++;
            }
            right++;
        }       
    }
};
```

### 2.  [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)

```c++
class Solution {
public:
    int addDigits(int num) {
        // 模拟法，就硬莽
        while(num/10 != 0){
            int sum = 0;
            while(num != 0){
                sum += num%10;
                num /= 10;
            }
            num=sum;
        }
        return num;
    }
};


class Solution {
public:
    int addDigits(int num) {
       //另一种解法,也是莽，价值在于int《--》String 类型互换
       /*
       1. to_string(int/long/ll/unsigned/f/d) 顾名思义转化成字符串
       2. stoi()   string-->int
       3. for (auto &i : numStr) 和for (auto i : numStr) 一致。区别在于前者可以改变numStr自身的内容，而后者只能够只读？
       */
       if (num ==0 ){
           return 0;
       }
       string numStr = to_string(num);
       if (numStr.size() == 1){
           return stoi(numStr);
       }
       int res = 0;
       for (auto &i : numStr){
           //字符转int
           res += i-'0';
       }
       return addDigits(res);
    }
};
```

### 3. [263. 丑数](https://leetcode-cn.com/problems/ugly-number/)

```C++
class Solution {
public:
    bool isUgly(int num) {
        if(num < 1)  return false;
        while(num%2 == 0){
            num /= 2;
        }
        while(num%3 == 0){
            num /= 3;
        }
        while(num % 5 ==0){
            num /=5;
        }
        if (num==1) return true;
        return false;

    }
};
```

### 4. [268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/)

```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
    int n = nums.size();
    
    for(int i=0;i<nums.size();i++){
        //cout<<i<<endl;
        n =n ^ i ^nums[i];
    }
    return n;

    }
};
```

### 5. [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.length() != t.length()){
            return false;
        }
        // 内置函数sort(),排序使用
        sort(s.begin(),s.end());
        sort(t.begin(),t.end());
        return s == t;

    }
};
```

### 6. [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

```C++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int max =0 ;
        int cnt =1;
        for(int i=0;i<nums.size();i++){
            if(nums[i] == 1){
                max = max>cnt?max:cnt;
                cnt ++;
            }else{
                cnt = 1;
            }
        }
        return max;

    }
};
```

### 7. [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

```++
class Solution {
public:
    int fib(int N) {
        // 手工计算
        if (N==0||N==1) return N;
        int a =0 ,b=1;
        for(int i=2;i<=N;i++){
            b = a+b;
            a = b-a;//就是原来的a
        }
        return b;

    }
};
```

### 8. [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

```c++
class Solution {
public:
/*
1. auto 指针自动分配类型
2. vector<vector<int>> new_Nums(r , vector<int>(c));
  vector 二维数组可以这么初始化
*/
    vector<vector<int>> matrixReshape(vector<vector<int>>& nums, int r, int c) {
        //先判断数组是不是可以重塑的
        auto r_Nums = nums.size();
        auto c_Nums = nums[0].size();

        //代表不能重塑返回原矩阵
        if(r_Nums*c_Nums != r*c)
            return nums;

        //可以重塑开始重塑
        vector<vector<int>> new_Nums(r , vector<int>(c));
        for(int i=0 ; i<r*c ; i++){
            new_Nums[i/c][i%c] = nums[i/c_Nums][i%c_Nums];

        }
        return new_Nums;




    }
};
```

### 9. [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int cnt = 0;
        flowerbed.insert(flowerbed.begin(),0);
        flowerbed.insert(flowerbed.end(),0);
        for (int i=1;i<flowerbed.size()-1;i++){
            if(flowerbed[i] == 0 && flowerbed[i-1] == 0 &&flowerbed[i+1]==0){
                cnt ++;
                flowerbed[i] =1;
            }
            if (n<= cnt){
                return true;
            }
        }
        return n <= cnt;

    }
};
```

### 10 .[643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)

```C++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        double sum_k = 0;
        for(int i=0;i<k;i++){
            sum_k += nums[i];
        }
        auto max_sum = sum_k;
        for(int i=k;i<nums.size();i++){
            sum_k = sum_k+nums[i]-nums[i-k];
            max_sum = max(max_sum,sum_k);
        }
        return max_sum/k;

    }
};
```

### 11 .[661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

```c++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        int R = M.size(),C = M[0].size();
        vector<vector<int>> res(R,vector<int>(C,0));
        //cout<<"R:"<<R<<endl;
        for(int r= 0;r<R;r++){
            //cout<<r<<endl;
            for(int c=0;c<C;c++){
                //cout<<"r;"<<r<<endl;
                int cnt =0;
                for(int nr= r-1;nr<=r+1;nr++){
                    for(int nc=c-1;nc<=c+1;nc++){
                        if(0<=nr &&nr<R&&nc>=0&&nc<C){

                            res[r][c] += M[nr][nc];
                            cnt ++;
                            //cout<<"res"<<res[r][c]<<endl;
                        }
                        // if (0<= nr &&nr<R&&0<=nc&&nc<C){
                        //     res[r][c] += M[nr][nc];
                        //     cnt ++;
                        // }
                    }
                }
                cout<<cnt;
                res[r][c] /= cnt;
            }
        }
        return res;

    }
};
```



### 12. [717. 1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

```C++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int i = bits.size()-2;
        while(i >=0 && bits[i]>0){
            i--;
        }
        return (bits.size()-1-(i-1))%2==0;

    }
};
```

### 13. [724. 寻找数组的中心索引](https://leetcode-cn.com/problems/find-pivot-index/)

```C++
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int sum = 0;
        for(int i=0;i<nums.size();i++){
            sum += nums[i];
        }
        int l_sum =0;
        for(int i=0;i<nums.size();i++){
            if (l_sum == sum-nums[i]-l_sum){
                return i;
            }
            l_sum += nums[i];
        }
        return -1;

    }
};
```



### 14.[697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

```c++
class Solution {
public:
    int findShortestSubArray(vector<int>& nums) {
        // 1. 利用 map 确定每个元素的度 (相同元素的所有索引)
        unordered_map<int, vector<int>> degree_map;
        
        // map 的 second 是 vector，所以用 push_back
        for (int i = 0; i < nums.size(); i++)
            degree_map[nums[i]].push_back(i);

        // 每个元素的度
        int n_degree = 0;

        // 数组的度
        int nums_degree = 0;

        // 与数组度数相同的最短子数组的长度
        int min_len = 0;

        int tmp_len = 0;

        // 2. 求跟原数组的度相同的最短子数组的长度
        for (auto n : degree_map) {
            // 3. 求每个元素 n 的度

            //这里的second 就是字典的value
            n_degree = n.second.size();
            
            if (n_degree >= nums_degree) {
                // 4. 求每个子数组的长度
                tmp_len = n.second[n_degree - 1] - n.second[0] + 1;

                if (n_degree == nums_degree) {
                    // 5. 找到度数相同子数组，每次取最小值
                    min_len = min(min_len, tmp_len);
                } else {
                    // 这是 degree > nums_degree 的情况
                    
                    // 求数组最大的度
                    nums_degree = n_degree;

                    // 更新子数组的长度
                    min_len = tmp_len;
                }
            }
        }

        return min_len;


    }
};
```

### 15. [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size()<=1){
            return nums.size();
        }
        int res =1,cnt =1;
        for(int i =0;i<nums.size()-1;i++){
            if(nums[i+1]>nums[i]){
                cnt ++;
            }else{
                res = max(res,cnt);
                cnt = 1;
            }
        }
        return max(res,cnt);
    }
};
```

### 16. [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

```c++
class Solution {
public:
    bool checkPossibility(vector<int>& nums) {
        if (nums.size()<3){
            return true;
        }
        int cnt = 0;
        for(int i=0;i<nums.size()-1;i++){
            if (nums[i]>nums[i+1]){
                cnt++;
                if (cnt>1){
                    return false;
                }
                // 核心判定，当i-1 比i+1 打的时候，i+1 升格
                if(i-1>=0&&nums[i-1]>nums[i+1]){
                    nums[i+1] = nums[i];

                }
                // 否则 i降格
                else{
                    nums[i] = nums[i+1];
                }
            }
        }
        return cnt<=1;
    }
};
```

### 从17 到26 省略，有空在填补



### 27. [1652. 拆炸弹](https://leetcode-cn.com/problems/defuse-the-bomb/)

```c++
class Solution {
public:
    vector<int> decrypt(vector<int> &code, int k) {
        vector<int> ans;
        int n = code.size();
        if (k < 0) {
            for (int i = 0; i < code.size(); ++i) {
                int num = 0;
                for (int j = 0, index = i; j < -k; ++j) {
                    num += code[(--index + n) % n];
                }
                ans.push_back(num);
            }
        } else {
            for (int i = 0; i < code.size(); ++i) {
                int num = 0;
                for (int j = 0, index = i; j < k; ++j) {
                    num += code[(++index) % n];
                }
                ans.push_back(num);
            }
        }
        return ans;
    }
};

```



### 28. [1656. 设计有序流](https://leetcode-cn.com/problems/design-an-ordered-stream/)

```c++
class OrderedStream {
public:
    string res[1005];
    int ptr;
    OrderedStream(int n) {
        this-> ptr = 1;
        for(int i =0;i<n;i++){
            res[i] = "";
        }

    }
    
    vector<string> insert(int id, string value) {
        res[id] = value;
        vector<string> ret;
        while(res[ptr].length()>0){
            ret.push_back(res[ptr++]);
        }
        return ret;
    }
};

/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream* obj = new OrderedStream(n);
 * vector<string> param_1 = obj->insert(id,value);
 */
```


### 29. [5617. 设计 Goal 解析器](https://leetcode-cn.com/problems/goal-parser-interpretation/)

```c++
class Solution {
public:
    string interpret(string command) {
        string res;//存储结果
        //遍历字符串
        for(int i=0;i<command.size();){
            if(command[i]=='G'){//若当前字符是G，则直接替换为G，并对索引增加1
                res+="G";
                ++i;
            }
            else if(command[i+1]==')'){//否则，下一个字符是右括号的话，则需要替换的一定是一对括号，替换为o，并对索引增2
                res+="o";
                i+=2;
            }
            else{//否则，一定是需要替换为 al 的字符串，替换，并对索引增加4
                res+="al";
                i+=4;
            }
        }
        return res;



    }

```

### 30.[1122. 数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)

```C++
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        vector<int> res,rest;
        unordered_map<int,int> hash;
        for(auto v:arr1){
            hash[v]++;
        }
        for(auto v:arr2){
            while(hash[v]) res.push_back(v),hash[v]--;
        }
        for(auto v:arr1){
            if(hash[v]){
                rest.push_back(v);
            }
        }
        sort(rest.begin(),rest.end());
        for(auto v:rest){
            res.push_back(v);
        }
        return res;

    }
};
```

### 31. [1413. 逐步求和得到正数的最小值](https://leetcode-cn.com/problems/minimum-value-to-get-positive-step-by-step-sum/)

```C++
class Solution {
public:
    int minStartValue(vector<int>& nums) {
        int sum =nums[0];
        int _min = nums[0];
        for(int i=1;i<nums.size();i++){
            sum += nums[i];
            _min = min(_min,sum);
        }
        if(_min>0) return 1;
        return 1-_min;

    }
    
};
```

### 31. [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)

```C++
class Solution {
public:
    int countPrimes(int n) {
        int count = 0;
        vector<bool> flags(n,true);
        for(int i=2;i<n;i++){
            if(flags[i]){
                count++;
                for(int j=2*i;j<n;j += i){
                    flags[j] = false;
                }
            }
        }
        return count;

    }
};
```

### 32. [374. 猜数字大小](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

```C++
class Solution {
public:
    int guessNumber(int n) {
        int l = 1,r = n;
        while(l<=r){
            int mid = l + (r-l)/2;
            //cout<<mid;
            //cout<<"fuck"<<guess(mid)<<endl;
            if (guess(mid) < 0) r = mid;
            else if(guess(mid)>0) l = mid+1 ;
            else 
                return mid;
        }
        return 0;  //需要多一个return 0
        
    }
};
```

### 33. [746. 使用最小花费爬楼梯](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)

```python
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        int n = cost.size();
        vector<int> dp(n+1);
        dp[0] = dp[1] =0;
        for (int i =2;i<=n;i++){

           dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
        }
        //cout<<dp;
        return dp[n];

    }
};
```



### 34. [LCP 11. 期望个数统计](https://leetcode-cn.com/problems/qi-wang-ge-shu-tong-ji/)

```C++
class Solution {
public:
    int expectNumber(vector<int>& scores) {
        unordered_set<int> set;
        for (int i : scores) set.insert(i);
        return set.size();

    }
};
```

### 35. [387. 字符串中的第一个唯一字符](https://leetcode-cn.com/problems/first-unique-character-in-a-string/)

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        unordered_map<int,int> freq;
        for(char &c : s){
            freq[c]++;
        }
        for(int i =0;i<s.size();i++){
            if (freq[s[i]] == 1){
                return i;
            }
        }
        return -1;

    }
};
```



### 36. [LCP 07. 传递信息](https://leetcode-cn.com/problems/chuan-di-xin-xi/)

```C++
class Solution {
public:
    // 逐轮dp解法
    int numWays(int n, vector<vector<int>>& relation, int k) {
        int dp[10][15];
        memset(dp,0,sizeof(dp));
        dp[0][0] = 1;
        for( int i =0;i<k;i++){
            for(auto &r:relation){
                dp[i+1][r[1]] += dp[i][r[0]];
            }
        }
        return dp[k][n-1];

    }
};
```

### 38. [LCP 02. 分式化简](https://leetcode-cn.com/problems/deep-dark-fraction/)

```C++
class Solution {
public:
    vector<int> fraction(vector<int>& cont) {
        int high = 1, low = cont.back();
        for(int i =cont.size()-2;i>=0;i--){
            high += low * cont[i];
            swap(low,high);
        }
        return vector<int>{low,high};

    }
};
```



## medium

### 1. [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```C++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        // 对于empty或只有一个区间, return;
        if (points.size() <= 1)
            return points.size();
        // 按照区间开始位置排序
        sort(points.begin(), points.end(),
        //[](){}匿名函数写法，注意传进去的两个参数，vector里面存储的类型，如int
            [](const vector<int> &a, const vector<int> b) {
                return a[0] == b[0] ? a[1] <= b[1] : a[0] < b[0];
            });

        int arrows = 1;
        // 初始化待发射区为[points[0][0], points[0][1]] ;
        vector<int> range = {points[0][0], points[0][1]};
        for (int i = 1; i < points.size(); i++) {
            auto curr = points[i];
            // 当前区间和待发射区间有交集, 更新交叉区间
            if (curr[0] <= range[1]) {
                range[0] = max(range[0], curr[0]);
                range[1] = min(range[1], curr[1]);
            } else {
                // 没有交集, 增加箭头数量, 将待发射区间设置为当前区间
                ++arrows;
                range[0] = curr[0];
                range[1] = curr[1];
            }
        }
        return arrows;



    }
};
```



### 2. [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```c++
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        vector<int> res(2,-1);
        if(nums.empty())  return res;
        int n =nums.size(),l=0 ,r= n-1;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]>=target) r=m;
            else l=m+1;
        }
        if(nums[l]!=target) return res;
        res[0]=l;
        r=n;
        while(l<r){
            int m=l+(r-l)/2;
            if(nums[m]<=target) l=m+1;
            else r=m;
        }
        res[1]=l-1;
        return res;

    }
};
```



### 3. [5618. K 和数对的最大数目](https://leetcode-cn.com/problems/max-number-of-k-sum-pairs/)

```C++
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        sort(nums.begin(),nums.end());
        int l=0,r=nums.size()-1;
        int ans=0;
        while(l<r){
            if(nums[l]+nums[r]<k){
                l++;
            }
            else if(nums[l]+nums[r]>k){
                r--;
            }
            else{
                l++;
                r--;
                ans++;
            }
        }
        return ans;
    }
};


```



### 4. [842. 将数组拆分成斐波那契序列](https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence/)

```C++
class Solution {
public:
    vector<int> splitIntoFibonacci(string S) {
        vector<int> list;
        backtrack(list, S, S.length(), 0, 0, 0);
        return list;
    }

    bool backtrack(vector<int>& list, string S, int length, int index, long long sum, int prev) {
        if (index == length) {
            return list.size() >= 3;
        }
        long long curr = 0;
        for (int i = index; i < length; i++) {
            if (i > index && S[index] == '0') {
                break;
            }
            curr = curr * 10 + S[i] - '0';
            if (curr > INT_MAX) {
                break;
            }
            if (list.size() >= 2) {
                if (curr < sum) {
                    continue;
                }
                else if (curr > sum) {
                    break;
                }
            }
            list.push_back(curr);
            if (backtrack(list, S, length, i + 1, prev + curr, curr)) {
                return true;
            }
            list.pop_back();
        }
        return false;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/split-array-into-fibonacci-sequence/solution/jiang-shu-zu-chai-fen-cheng-fei-bo-na-qi-ts6c/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 5. [714. 买卖股票的最佳时机含手续费](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int n = prices.size();
        vector<vector<int>> dp(n,vector<int>(2));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        for(int i=1;i<n;++i){
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]-fee);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);

        }
        return dp[n-1][0];

    }
};
```



### 6. [1696. 跳跃游戏 VI](https://leetcode-cn.com/problems/jump-game-vi/)

```python
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int  n = nums.size();

        priority_queue<pair<int, int>> q;
        q.emplace(nums[0],0);
        int res = nums[0];

        for(int i=1;i<n;i++){
            while(i -q.top().second >k){
                q.pop();
            }
            res = q.top().first + nums[i];
            q.emplace(res,i);
        }
        return res;


    }
};
class Solution {
public:
    int maxResult(vector<int>& nums, int k) {
        int  n = nums.size();

        deque<pair<int, int>> q;
        q.emplace_back(nums[0],0);
        int res = nums[0];

        for(int i=1;i<n;i++){
            while(i -q.front().second >k){
                q.pop_front();
            }
            res = q.front().first + nums[i];
            while(!q.empty() && res >= q.back().first){
                q.pop_back();
            }
            q.emplace_back(res,i);
        }
        return res;


    }
};
```

### 7. [503. 下一个更大元素 II](https://leetcode-cn.com/problems/next-greater-element-ii/)

```python
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n);
        stack<int> s;
        // 两次循环使用这个求余表示
        for (int i=2*n-1;i>=0;i--){
            while(!s.empty() && s.top()<= nums[i%n]){
                s.pop();
            }
            res[i%n] =  s.empty()?-1:s.top();
            s.push(nums[i%n]);
        }
        return res;

    }
};
```

### 8. [LCP 03. 机器人大冒险](https://leetcode-cn.com/problems/programmable-robot/)

```c++
class Solution {
    // 来不及写了，先记录一个调节，和python思路类似，主要熟悉C++语法
public:
    bool canReach(vector<vector<bool>> &g, int x, int y, int maxX, int maxY) {
        // 利用上述坐标推算方法进行推算
        int numOfLoop = 0;
        if (maxX != 0) {
            numOfLoop = x / maxX;
        }
        if (maxY != 0) {
            numOfLoop = min(y / maxY, numOfLoop);
        }
        x -= numOfLoop * maxX;
        y -= numOfLoop * maxY;
        if (x < 0 || x > maxX || y < 0 || y > maxY) {
            return false;
        }
        return g[x][y];
    }
    bool robot(string command, vector<vector<int>>& obstacles, int x, int y) {
        int n = command.size();        
        vector<vector<bool>> g = vector<vector<bool>>(n, vector<bool>(n, false));
        int maxX = 0;
        int maxY = 0;
        g[0][0] = true;
        for (char c : command) {
            if (c == 'U') {
                ++maxY;
            } else {
                ++maxX;
            }
            g[maxX][maxY] = true;
        }
        for (auto &o : obstacles) {
            if (o[0] > x || o[1] > y) {
                continue;
            }
            if (canReach(g, o[0], o[1], maxX, maxY)) {
                return false;
            }
        }
        return canReach(g, x, y, maxX, maxY);
    }
};

作者：potterxu
链接：https://leetcode-cn.com/problems/programmable-robot/solution/c-oobstaclessize-shi-jian-ocommandsize-commandsize/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 9. [103. 二叉树的锯齿形层序遍历](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    // 思路明白，但是C++对数，对队列的操作，还是需要熟悉
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if (!root) {
            return ans;
        }

        queue<TreeNode*> nodeQueue;
        nodeQueue.push(root);
        bool isOrderLeft = true;

        while (!nodeQueue.empty()) {
            deque<int> levelList;
            int size = nodeQueue.size();
            for (int i = 0; i < size; ++i) {
                auto node = nodeQueue.front();
                nodeQueue.pop();
                if (isOrderLeft) {
                    levelList.push_back(node->val);
                } else {
                    levelList.push_front(node->val);
                }
                if (node->left) {
                    nodeQueue.push(node->left);
                }
                if (node->right) {
                    nodeQueue.push(node->right);
                }
            }
            ans.emplace_back(vector<int>{levelList.begin(), levelList.end()});
            isOrderLeft = !isOrderLeft;
        }

        return ans;
    }
};

```



## hard



