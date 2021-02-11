# C++_algorithm

## 前言:

​	一个小小的尝试，不知道能坚持或者付出多少，希望有一点点收获

## 零碎语法点

#### 1.内置算法：swap

#### 2 .to_string：

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

#### 3.stoi()   string-->int

#### 4.  for (auto &i : numStr) 和for (auto i : numStr) 一致。区别在于前者可以改变numStr自身的内容，而后者只能够只读？

#### 5. // 内置函数sort(),排序使用

#### 6. 优先队列使用

```C++
  // 这是C++的预处理器，方便使用
    #define PII pair<int, int> 
 // 优先队列使用， 参数分别是数据类型， 容器类型，以及仿函数greater，less（数据类型）
priority_queue<PII, vector<PII>> // 默认大根堆
         priority_queue<PII, vector<PII>, greater<PII> > save; // 小根堆
```



####  7. stringstream

```C++
/*stringstream 是 C++ 提供的另一个字串型的串流(stream)物件，和之前学过的 iostream、fstream 有类似的操作方式。要使用 stringstream， 必須先加入這一行：

#include <sstream>
stringstream 主要是用在將一個字串分割，可以先用 clear( )以及 str( ) 將指定字串設定成一开始的內容，再用 >> 把个別的资料输出，例如：*/
#include <sstream>
string s;
stringstream ss;
int a, b, c;
getline(cin, s);
ss.clear();
ss.str(s);
ss >> a >> b >> c;
```

#### 8. toupper & tolower

```C++
 high+=toupper(word[i]);
 low+=tolower(word[i]);
// 文字转换大小写
```

<<<<<<< HEAD
#### 9. accumulate

```C++
accumulate带有三个形参：头两个形参指定要累加的元素范围，第三个形参则是累加的初值。
//sum the elements in vec starting the summation with the value 42
int sum = accumulate(vec.begin() , vec.end() , 42);
```

=======
#### 9. is_sorted()
>>>>>>> e76228a117d8cf203fe23c53f72d5d25281726da

is_sorted() 函数有 2 种语法格式，分别是：

```
//判断 [first, last) 区域内的数据是否符合 std::less<T> 排序规则，即是否为升序序列
bool is_sorted (ForwardIterator first, ForwardIterator last);
//判断 [first, last) 区域内的数据是否符合 comp 排序规则  
bool is_sorted (ForwardIterator first, ForwardIterator last, Compare comp);
```





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

### 34. [1441. 用栈操作构建数组](https://leetcode-cn.com/problems/build-an-array-with-stack-operations/)

```python
class Solution {
public:
    vector<string> buildArray(vector<int>& target, int n) {
        vector<string> res;
        int nextpush =1;
        for (int i=0;i<target.size();i++){
            while(nextpush<target[i]){
                res.push_back("Push");
                res.push_back("Pop");
                nextpush++;
            }
            res.push_back("Push");
            nextpush ++;
        }
        return res;
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

### 39.[1018. 可被 5 整除的二进制前缀](https://leetcode-cn.com/problems/binary-prefix-divisible-by-5/)

```C++
class Solution {
public:
    vector<bool> prefixesDivBy5(vector<int>& A) {
         int temp=0;
        // 依照二进制性质每次*2，就好，但是由于可能数字溢出，我们实际只关注各位数字，所以取模
        vector<bool> ans(A.size(),false);
        for(int i=0;i<A.size();i++){
            temp=(temp+A[i])%10;
            if(temp==0 || temp==5) ans[i]=true;
            temp*=2;
        }
        return ans;



    }
};
```

### 40. [455. 分发饼干](https://leetcode-cn.com/problems/assign-cookies/)

```C++
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        int m = g.size(),n= s.size();
        int i=0,j=0;
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int cnt =0;
        while (i<m && j <n){
            if(s[j]>= g[i]){
                cnt++;
                i++;
                j++;
            }else{
                j++;
            }
        }
        return cnt;

    }
};
```

### 41.[653. 两数之和 IV - 输入 BST](https://leetcode-cn.com/problems/two-sum-iv-input-is-a-bst/)

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
// set 中 ，s.count(key) == 0 or 1    s.find(key)  返回一个迭代器，根据是否是s.end() 。 确定是否找到
class Solution {
public:
    unordered_set<int> s;
    bool flag = false;
    int K;
    bool findTarget(TreeNode* root, int k) {
        K=k;
        inorder(root);
        return flag;

        
    }
    void inorder(TreeNode* root){
        if(root->left){
            inorder(root->left);
        }
        //cout<< to_string(s.find(K-root->val) == s.end())<<endl;
        if (s.find(K-root->val) != s.end()){
            flag = true;
            return;
        }
        s.insert(root->val);
        if(root->right){
            inorder(root->right);
        }
    }
};
```

### 42. [205. 同构字符串](https://leetcode-cn.com/problems/isomorphic-strings/)

```C++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char,char> s2t;
        unordered_map<char,char> t2s;
        int len= s.length();
        for (int i=0;i<len;i++){
            char x = s[i],y= t[i];
            // 首先已经在表里，其次不匹配，两者条件都满足之后，才能return false
            if((s2t.count(x)&& s2t[x] != y)|| (t2s.count(y) && t2s[y] != x)){
                return false;
            }
            s2t[x] = y;
            t2s[y] = x;
        }
        return true;

        
    }
};
```

### 43. [5637. 判断字符串的两半是否相似](https://leetcode-cn.com/problems/determine-if-string-halves-are-alike/)

```C++
class Solution {
public:
    bool halvesAreAlike(string s) {
       int  n = s.length();
       unordered_set<int> _s = {'a','e','i','o','u','A','E','I','O','U'};
       int cnt =0,mid = n/2;
       for(int i=0;i<n;i++){
           if (i<mid){
               if (_s.count(s[i])){
                   cnt++;
               }
           }else{
               if (_s.count(s[i])){
                   cnt--;
               }
           }
           //cout<<cnt;
       }
       return cnt ==0;


    }
};
```

### 44. [961. 重复 N 次的元素](https://leetcode-cn.com/problems/n-repeated-element-in-size-2n-array/)

```C++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {
        int n = A.size();
        unordered_set<int> hash;
        for (auto c :A){
            if(hash.count(c)){
                return c;
            }
            // 貌似对set类型的，不能用push_back
            hash.insert(c);
        }
        return 0;

    }
};
```

### 45. [1037. 有效的回旋镖](https://leetcode-cn.com/problems/valid-boomerang/)

```C++
class Solution {
public:
    bool isBoomerang(vector<vector<int>>& points) {
        // 这是我想的另一种方法，用三角形判定
        double a,b,c;
        a=sqrt((points[1][0]-points[0][0])*(points[1][0]-points[0][0])+(points[1][1]-points[0][1])*(points[1][1]-points[0][1]));
        b=sqrt((points[2][0]-points[0][0])*(points[2][0]-points[0][0])+(points[2][1]-points[0][1])*(points[2][1]-points[0][1]));
        c=sqrt((points[2][0]-points[1][0])*(points[2][0]-points[1][0])+(points[2][1]-points[1][1])*(points[2][1]-points[1][1]));
        return a+b>c&&a+c>b&&b+c>a;



    }
};
```

### 46. [1046. 最后一块石头的重量](https://leetcode-cn.com/problems/last-stone-weight/)

```C++
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> q;
        for (int s: stones) {
            q.push(s);
        }

        while (q.size() > 1) {
            int a = q.top();
            q.pop();
            int b = q.top();
            q.pop();
            if (a > b) {
                q.push(a - b);
            }
        }
        return q.empty() ? 0 : q.top();
    }
};
```

### 47.[1360. 日期之间隔几天](https://leetcode-cn.com/problems/number-of-days-between-two-dates/)

```c++
class Solution {
     bool leap_year(int n ){
        return ((n%4==0)  || ( n %4==0 && n%100 !=0));
    }
    int getdate(string date){
        int tab[]={-1,31,28,31,30,31,30,31,31,30,31,30,31};
        int y,m,d,r =0;
        sscanf(date.c_str(),"%d-%d-%d", &y,&m,&d);
        for(int i=1970;i<y;i++){
            if (leap_year(i)){
                r += 366;
            }
            else{
                r+=365;
            }
        }
        for (int i=0;i<m;i++){
            r += tab[i];
            if (leap_year(y)) r++;
        }
        r +=d;
        return r;
    }
public:
    int daysBetweenDates(string date1, string date2) {
        return abs(getdate(date1)-getdate(date2));

    }
   
};
```

### 48. [1518. 换酒问题](https://leetcode-cn.com/problems/water-bottles/)

```C++
class Solution {
public:
    /*第一步，首先我们一定可以喝到 bb 瓶酒，剩下 bb 个空瓶。

第二步，接下来我们来考虑空瓶换酒，换完再喝，喝完再换的过程——每次换到一瓶酒就意味着多一个空瓶，所以每次损失的瓶子的数量为 e - 1e−1，我们要知道这个过程能得到多少瓶酒，即希望知道第一个打破下面这个条件的 nn 是多少：

*/
    //b−n(e−1)≥e
    // ==>b−n(e−1)<e
    // n(min) = [(b-e)/(c-1) + 1]
    int numWaterBottles(int numBottles, int numExchange) {
        return numBottles >= numExchange ? (numBottles - numExchange) / (numExchange - 1) + 1 + numBottles : numBottles;
    }
};


```



### 49. [5641. 卡车上的最大单元数](https://leetcode-cn.com/problems/maximum-units-on-a-truck/)

```c++
class Solution {
public:
    int maximumUnits(vector<vector<int>>& b, int m) {
        // 匿名函数写法。可以再熟悉一下
        sort(b.begin(),b.end(),[](vector<int> a, vector<int>b){
            return a[1]>b[1];
        });
        int res =0;
        for (int i=0;i<b.size();i++){
            int cur = min(b[i][0], m);
            res += cur * b[i][1];
            m -= cur;
        }
        return res;

    }
};
```

### 50. [1337. 矩阵中战斗力最弱的 K 行](https://leetcode-cn.com/problems/the-k-weakest-rows-in-a-matrix/)

```c++
class Solution {
public:
    vector<int> kWeakestRows(vector<vector<int>>& mat, int k) {
        int n = mat.size();
        vector<pair<int, int>> power;
        for (int i = 0; i < n; ++i) {
            // 累计算法
            int sum = accumulate(mat[i].begin(), mat[i].end(), 0);
            power.emplace_back(sum, i);
        }

        sort(power.begin(), power.end());
        vector<int> ans;
        for (int i = 0; i < k; ++i) {
            ans.push_back(power[i].second);
        }
        return ans;
    }
};


```

### 51. [830. 较大分组的位置](https://leetcode-cn.com/problems/positions-of-large-groups/)

```C++
class Solution {
public:
    vector<vector<int>> largeGroupPositions(string s) {
        int pre= 0;
        vector<vector<int>> res ;
        int i;
        for ( i =1;i<s.size();i++)
        {
            if(s[i] == s[i-1]) continue;
            else
            {
                if (i-pre  >=3)
                {
                    res.push_back(vector<int>{pre,i-1});
                }
                pre = i;
            }
        }
        if(i-pre>=3) res.push_back(vector<int>{pre,i-1});
        return res;

    }
};
```

### 52. [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)

```c++
// The API isBadVersion is defined for you.
// bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        long long l=1, r= n;
        while (l<r)
        {
            long long mid = (l+r)>>1;
            if (isBadVersion(mid)) r =mid;
            else l =  mid+1;
        }
        return int(l);
        
    }
};
```

### 53. [367. 有效的完全平方数](https://leetcode-cn.com/problems/valid-perfect-square/)

```c++
class Solution {
public:
    bool isPerfectSquare(int num) {
        typedef long long LL;
        LL l = 1,r = num/2+1;
        while (l<r){
            LL mid = (l+r)>>1;
            if (mid*mid>=num) r = mid;
            else l =mid+1;
        }
        return l*l ==num;

    }
};
```

### 54.[345. 反转字符串中的元音字母](https://leetcode-cn.com/problems/reverse-vowels-of-a-string/)

```python
class Solution {
public:
    string reverseVowels(string s) {
        if (s.empty()) {
            return s;
        }
        unordered_set<char> vowelDict = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
        int N = s.size();
        int i = 0;
        int j = N - 1;
        while (i < j) {
            while (i < j && !vowelDict.count(s[i])) {
                i++; // find a vowel from left
            }
            while (j > i && !vowelDict.count(s[j])) {
                j--; // find a vowel from right; 
            }
            // 还是那句话，c++里的stirng当成vector，数组，不要当成其他语言里面不可变的字符串
            swap(s[i], s[j]);
            i++;
            j--;
        }

        return s;


    }
};
```



### 55. [342. 4的幂](https://leetcode-cn.com/problems/power-of-four/)

```C++
class Solution {
public:
    bool isPowerOfFour(int n) {
        // 位运算优先级很低，记得带括号
        // 4的幂首先是2的幂，所以用lowbit判断二进制中是否只有1格1
        // 4 进制是2的偶数次幂，2^(2k) = 4^(k) = (3+1)^k 
        // (3+1)^k mod 3 ==1
        return (n>0) && ((n&(n-1)) == 0) && (n %3==1);
        

    }
};
```



### 56. [228. 汇总区间](https://leetcode-cn.com/problems/summary-ranges/)

```c++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums) {
        vector<string> res;
        int i=0, n = nums.size();
        while(i <n){
            int low =i;
            i ++;
            while(i<n && nums[i] == nums[i-1] + 1){
                i++;
            }
            int high = i-1;
            string tmp = to_string(nums[low]);
            if(low<high){
                tmp.append("->");
                tmp.append(to_string(nums[high]));

            }
            res.push_back(tmp);
        }
        return res;
   



    }
};
```

### 57. 5649. 解码异或后的数组

```c++
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        vector<int> res;
        res.push_back(first);
        for (auto i : encoded){
            res.push_back(res.back()^i);
        }
        return res;

    }
};
```



### 58. [1716. 计算力扣银行的钱](https://leetcode-cn.com/problems/calculate-money-in-leetcode-bank/)

```C++
class Solution {
public:
    int totalMoney(int n) {
         int res = 0;
        for(int i=1, money=1; i<=n; i++){
            res += money++;
            if(i % 7 == 0) money = i/7 + 1;
        }
        return res;



    }
};
```



### 59. [383. 赎金信](https://leetcode-cn.com/problems/ransom-note/)

```C++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        //vector<int> a(26);
        int a[26] = {0};
        int b[26] = {0};
        //vector<int> b(26);
        for(auto c:ransomNote){
            //cout<<c-'a'<<endl;
            a[c-'a']++;
        }
        for(auto c: magazine) cout<<c-'a'<<endl,b[c-'a']++;
        for(int i=0;i<26;i++){
            //cout<<i<<"a[i]"<<a[i]<<"b[i] "<<b[i]<<endl;
            if (a[i]>b[i]){
                cout<<(char)(i+97)<<endl;
                return false;
            }
        }

        return true;
    }
};
```



### 60. [371. 两整数之和](https://leetcode-cn.com/problems/sum-of-two-integers/)

```c++
class Solution {
public:
    int getSum(int a, int b) {
        /*对于unsigned整型溢出，C的规范是有定义的——“溢出后的数会以2^(8*sizeof(type))作模运算”，也就是说，如果一个unsigned char（1字符，8bits）溢出了，会把溢出的值与256求模。
         对于signed整型的溢出，C的规范定义是“undefined behavior”，也就是说，编译器爱怎么实现就怎么实现*/
        while (b){
            //cout<<(( unsigned int)(a&b)<<1);
            auto carry =( unsigned int)(a&b)<<1;
            a ^=b;
            b = carry;
        }
        return a;

    }
};
```

### 61. [1668. 最大重复子字符串](https://leetcode-cn.com/problems/maximum-repeating-substring/)

```c++
class Solution {
public:
    int maxRepeating(string sequence, string word) {
          int count=0;
        string s(word);
        // C++的find方法返回的迭代器是末尾，而不是-1
        while(sequence.find(s)<sequence.size()){
            s+=word;
            count++;
        }
        return count;


    }
};
```

### 62. [551. 学生出勤记录 I](https://leetcode-cn.com/problems/student-attendance-record-i/)

```c++
class Solution {
public:
    bool checkRecord(string s) {
        string pre=" 1 ";
        int cnt_A = 0;
        for(auto c:s){
            if (c == 'A') cnt_A++;
            pre[2] = c;
            if ((pre) == "LLL") return false;
            if (cnt_A>1) return false;
            pre[0]=pre[1];
            pre[1]=pre[2];
        }
        return true;

    }
};
```

### 63. [575. 分糖果](https://leetcode-cn.com/problems/distribute-candies/)

```C++
class Solution {
public:
    int distributeCandies(vector<int>& candyType) {
        unordered_set<int> set;
        for(auto i:candyType){
            set.insert(i);
        }
        return candyType.size()/2<= set.size()? candyType.size()/2:set.size();


    }
};
```





### 64. [1507. 转变日期格式](https://leetcode-cn.com/problems/reformat-date/)

```C++
class Solution {
public:
    string reformatDate(string date) {
        unordered_map<string, string> s2month = {
            {"Jan", "01"},
            {"Feb", "02"},
            {"Mar", "03"},
            {"Apr", "04"},
            {"May", "05"},
            {"Jun", "06"},
            {"Jul", "07"},
            {"Aug", "08"},
            {"Sep", "09"},
            {"Oct", "10"},
            {"Nov", "11"},
            {"Dec", "12"}
        };

        stringstream ss(date); // 串式输入输出，用于分割字符串
        string year, month, day;
        ss >> day >> month >> year;
        month = s2month[month];
        day.pop_back();
        day.pop_back();
        if (day.size() == 1) {
            day = "0" + day;
        }
        return year + "-" + month + "-" + day;
    }
};


```

### 65. [1496. 判断路径是否相交](https://leetcode-cn.com/problems/path-crossing/)

```c++
class Solution {
public:
    bool isPathCrossing(string path) {
        int x=0,y=0;
        set<pair<int, int>> ms;
        ms.insert({x,y});
        for(auto c:path){
            if (c=='N') x++;
            if(c=='S') x--;
            if(c=='E') y++;
            if (c=='W') y--;
            if (ms.count({x,y})) return true;
            ms.insert({x,y});
            
        }
        return false;

    }
};
```



### 66 .[1078. Bigram 分词](https://leetcode-cn.com/problems/occurrences-after-bigram/)
>>>>>>> 062daddfe47d0d1a850786aea2a04df81f319803

```C++

    vector<string> findOcurrences(string text, string first, string second) {
        stringstream words(text); // 也是使用串流分割字符串，貌似是C++的常用工具需要熟悉
        string w;
        vector<string> tmp, ans;
        while (words >> w) tmp.push_back(w);
        for (int i = 0, j = 1; j < tmp.size() - 1; ++i, ++j) 
            if (tmp[i] == first && tmp[j] == second) ans.push_back(tmp[j + 1]);

        return ans;
    }
};


```

### 67. [1071. 字符串的最大公因子](https://leetcode-cn.com/problems/greatest-common-divisor-of-strings/)

```C++
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        /*如果能够相除，必然由X组成的str1， str2,因此先前后颠倒连接，比较是否是相等排出一部分必然不是的
        然后，求取最大公约数，__gcd是c++内置的最大公约数函数，从开头截取，str1和str2都可以*/
        if (str1 + str2 != str2 + str1) return "";
        return str2.substr(0, __gcd((int)str1.length(), (int)str2.length())); // __gcd() 为c++自带的求最大公约数的函数

    }
};
```





### 68. [594. 最长和谐子序列](https://leetcode-cn.com/problems/longest-harmonious-subsequence/)

```C++
class Solution 
{
    // map是有序哈希表，可以利用和这个特性
public:
    int findLHS(vector<int>& nums) 
    {
        if (nums.size()==0)  //数组为空直接返回0
        {
            return 0;
        }
        int max_length=0;  //最长河蟹子序列的长度
        map<int,int>temp;
        for (int i=0;i!=nums.size();i++)
        {
            temp[nums[i]]++;
        }
        auto next=temp.begin();  //代表map容器中的后一个位置，这是迭代器
        auto before=temp.begin();  //代表map容器中的前一个位置
        next++;  //next在before的下一个位置
        for (;next!=temp.end();next++)  //两两比较相邻的key
        {
            if (next->first-before->first==1)  //如果后一个位置的key比前一个位置的key大1
            {
                max_length=max(max_length,next->second+before->second);  //更新max_length
            }
            before++;
        }
        return max_length;
    }
};



// 也可以单纯使用哈希表
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int, int>mp;
		int maxx = 0;
		for (int i : nums) {
			mp[i]++;
		}
		for (auto i : mp) {
            // 这里取到的i是key-value
			if (mp.count(i.first + 1))
				maxx = max(maxx, i.second + mp[i.first + 1]);
		}
		return maxx;
    }
};


```

### 69. [1154. 一年中的第几天](https://leetcode-cn.com/problems/day-of-the-year/)

```C++
class Solution {
public:
    int dayOfYear(string date) {
         vector<int> Month{31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    vector<int> sum_month(13,0);
    for (int i = 1; i <= Month.size(); ++i)
    {
        sum_month[i] += Month[i-1] + sum_month[i-1];
    }
    int year=stoi(date.substr(0,4)); //// stoi()字符转数字， s.substr(0,4)，c++里的字符产截取
    int month=stoi(date.substr(5,2));
    int month=stoi(date.substr(5,2));
    int day=stoi(date.substr(8,2));
    return sum_month[month-1]+day+(year%4==0&&year!=1900&&month>=3);//当月之前的日期数+当月日期+闰年补偿


    }
};
```



### 70  [1275. 找出井字棋的获胜者](https://leetcode-cn.com/problems/find-winner-on-a-tic-tac-toe-game/)

```c++
class Solution {
public:
    // 比枚举的解法聪明很多，从最后一步入手，分析最后一步对应的那个的成功或者失败，再通过总的步数决定是胜败，还是平局，又或者是进行中
    string tictactoe(vector<vector<int>>& moves) {
         vector<int> count(8,0);
        int size = moves.size();
        for(int i = size-1; i >= 0; i-=2){
            //对井字棋的行做记录
            count[moves[i][0]]++;
            //对井字棋的列做记录
            count[moves[i][1]+3]++;
            //对井字棋的主对角线做记录
            if(moves[i][0] - moves[i][1] == 0)
                count[6]++;
            //对井字棋的副对角线做记录
            if(moves[i][0] + moves[i][1] == 2)
                count[7]++;
            //满三个棋子则获胜
            if(count[moves[i][0]]==3 || count[moves[i][1]+3]==3 || count[6]==3 || count[7]==3)
                  return (size%2==0) ? "B" : "A";

        }
        if(size < 9)
            return "Pending";
        else
            return "Draw";

```

### 71 . [888. 公平的糖果棒交换](https://leetcode-cn.com/problems/fair-candy-swap/)

```C++

class Solution {
public:
    vector<int> fairCandySwap(vector<int>& A, vector<int>& B) {
        // 取两者之间的差值作为变化量
        int sumA = accumulate(A.begin(), A.end(), 0);//  C++版本的sum
        int sumB = accumulate(B.begin(), B.end(), 0);
        int delta = (sumA - sumB) / 2;
        unordered_set<int> rec(A.begin(), A.end()); //== set(A) 
        vector<int> ans;
        for (auto& y : B) {
            int x = y + delta;
            if (rec.count(x)) {
                ans = vector<int>{x, y};
                break;
            }
        }
        return ans;
    }
};
```



### 71. [1748. 唯一元素的和](https://leetcode-cn.com/problems/sum-of-unique-elements/)

```c++
#include<numeric>
class Solution {
public:
    int sumOfUnique(vector<int>& nums) {
        unordered_set<int> s;
        unordered_set<int> rc;
        for(auto i:nums){
            if (!s.count(i) && !rc.count(i)){
                s.insert(i);
            }
            else if (s.count(i) && !rc.count(i)){
                s.erase(i);
                rc.insert(i);
            }
        }
        return accumulate(s.begin(), s.end(),0);

    }
};
```

### 72. [1752. 检查数组是否经排序和轮转得到](https://leetcode-cn.com/problems/check-if-array-is-sorted-and-rotated/)

```c++
class Solution {
public:
    bool check(vector<int>& nums) {
        if(is_sorted(nums.begin(), nums.end())){
            return true;
        }
        int n = nums.size(), cnt = 0;
        for(int i = 0; i <= n - 2; ++i){
            if(nums[i] > nums[ i + 1]){
                cnt++;
                if(cnt > 1) return false;
            }
        }
        return nums[n - 1] <= nums[0];
    }
};

```

### 73. [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

```C++
class Solution {
  public:
  int search(vector<int>& nums, int target) {
    int pivot, left = 0, right = nums.size() - 1;
    while (left <= right) {
      pivot = left + (right - left) / 2;
      if (nums[pivot] == target) return pivot;
      if (target < nums[pivot]) right = pivot - 1;
      else left = pivot + 1;
    }
    return -1;
  }
};


```

### 74. [693. 交替位二进制数](https://leetcode-cn.com/problems/binary-number-with-alternating-bits/)
```C++

      bool hasAlternatingBits(int n) {
        n = (n ^ (n >> 1));  // 貌似右移之后不用担心最左端的情况哈？  ^之后都为·1
        return (n & ((long)n + 1)) == 0;   //全是1的加1，后溢出，全是0.与运算为0
    }

};
```

### 75. [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

```C++
class MyHashMap {
    
private:
    
    struct node{
        
        int my_key;
        int my_val;
        node* next;
        
        node(int key, int val):my_key(key),my_val(val),next(NULL){}  
    };
    
    vector<node*> my_map;//用链表为节点的容器构造哈希表
    
    int size = 1000;
    
public:
    /** Initialize your data structure here. */
    MyHashMap() {
        
        my_map = vector<node*> (size, new node(-1,-1));//初始化表
    }
    
    /** value will always be non-negative. */
    void put(int key, int value) {
        
        int index = key % size;//通过哈希函数特定新数值对在容器中的相对位置
        
        node* temp = my_map[index];//指向这个相对位置的表头
        
        node* last_node;//特定当前表的表尾
        node* new_node = new node(key,value);
        node* tmp_next = temp -> next;
        temp->next = new_node;//头插法
        new_node->next = tmp_next;
        
        // while(temp!=NULL){
            
        //     if(temp->my_key == key){//如果发现已经存在对应的键，则更新它的值
                
        //         temp->my_val = value;
        //         return;
        //     }
            
        //     last_node = temp;
        //     temp = temp->next;
        // }
        
        // //将新数值对插入到表尾
        // node* new_node = new node(key,value);
        // last_node->next = new_node;
        
    }
    
    /** Returns the value to which the specified key is mapped, or -1 if this map contains no mapping for the key */
    int get(int key) {
        
        int index = key % size;
        
        node* temp = my_map[index];
        
        while(temp!=NULL){
            
            if(temp->my_key==key){
                
                return temp->my_val;
            }
            
            temp = temp->next;
        }
        
        return -1;

        
    }
    
    /** Removes the mapping of the specified value key if this map contains a mapping for the key */
    void remove(int key) {
        
        int index = key % size;
        
        node* temp = my_map[index];

        
        while(temp!=NULL){
            
            if(temp->my_key==key){
                
                temp->my_val = -1;
                return;
            }
            temp = temp->next;
        }
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */

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

### 10. [386. 字典序排数](https://leetcode-cn.com/problems/lexicographical-numbers/)

```python
class Solution {
    vector<int> ans;
public:
// 10 叉树DFS
    void dfs(int num, int& n) {
        if (num > n) return;
        ans.push_back(num);
        for (int i = 0; i <= 9; ++i) dfs(num * 10 + i, n);
    }

    vector<int> lexicalOrder(int n) {
        for (int i = 1; i <= 9; ++i) dfs(i, n);
        return ans;
    }
};
```

### 11. [5622. 平均等待时间](https://leetcode-cn.com/problems/average-waiting-time/)

```C++
class Solution {
public:
    double averageWaitingTime(vector<vector<int>>& customers) {
        // 思路和python一致，注意的是，这里res 用int会移溢出，考虑情况使用long , 或者unsigned long
        unsigned long res =0;
        int n = customers.size();
        for(int i =0,t=0;i<n;i++){
            t = max(customers[i][0],t);
            t += customers[i][1];
            res +=(t - customers[i][0]);
        }
        return double(res)/n;

    }
};
```

### 12. [5623. 修改后的最大二进制字符串](https://leetcode-cn.com/problems/maximum-binary-string-after-change/)

```C++
class Solution {
public:
    string maximumBinaryString(string binary) {
        int n = binary.size();
        // C++字符串初始化的一种方式
        // 第一个0 之前的1，不用动，之后的一，右移，这样中间的都是1，然后用操作1，使之出现一个0
        string res(n,'1');
        bool flag = false;
        int cnt = 0;
        int t = n;
        for(int i=0; i<n; i++){
            if(binary[i] == '0'){
                flag = true;
                t = min(t,i);
                cnt++;
            }
        }
        if (!flag){
            return res;
        }
        res[t+cnt-1]='0';
        return res;

        

    }
};
```

### 13. [5638. 吃苹果的最大数目](https://leetcode-cn.com/problems/maximum-number-of-eaten-apples/)

```c++
class Solution {
public:
    // 这是C++的预处理器，方便使用
    #define PII pair<int, int> 
    int eatenApples(vector<int>& apples, vector<int>& days) {
        int res = 0;
        // 优先队列使用， 参数分别是数据类型， 容器类型，以及仿函数greater，less（数据类型）
        priority_queue<PII, vector<PII>, greater<PII> > save; //按过期日期升序
        // 这里的循环判定条件是是两个，其中empty实际上是扩大边界，过了n继续数
        for (int i = 0; i < apples.size() || !save.empty(); i++) {
            //到了过期那一天就删掉
            while(!save.empty() 
            && save.top().first == i){ //到了过期的日子
                save.pop();
            }
            //加入今天产出的果子
            if(i<apples.size()&&apples[i]!=0)
                save.push(make_pair(i+days[i], apples[i]));
            //如果有果子的话，吃果子
            if(!save.empty()){
                PII tmp = save.top();
                save.pop();
                res++;
                tmp.second--;
                if(tmp.second>0){   //果子没吃完，放回去
                    save.push(tmp);
                }
            }
        }
        return res;
    }
};

```

### 14. [5210. 球会落何处](https://leetcode-cn.com/problems/where-will-the-ball-fall/)

```C++
class Solution {
public:
    vector<int> findBall(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<int> ans(n, -1);
        for(int j = 0; j < n; j++) 
        {
            int x = 0, y = j;
            cout<<j<<endl;
            while(x != m)
            {   cout<<x<<y<<endl;
                if(grid[x][y] == 1 && y == n-1)
                    break;//右边有墙
                else if(grid[x][y] == -1 && y == 0)
                    break;//左边有墙
                else if(grid[x][y] == 1 && grid[x][y+1] == -1)
                    break;//V字形的左边
                else if(grid[x][y] == -1 && grid[x][y-1] == 1)
                    break;//V字形的右边
                // 和我在python 里写的不同，这里在排出了上线的卡主情况以外，一定可以把球放到有右下。或者这么说，因为球在没有卡主的情况，也就晒前面几种情况以外，一定会向下，所以x++，
                // 然后根据情况，是1，-1，决定y的偏移方向
                else if(grid[x][y] == 1)
                    x++,y++;//向右下走
                else
                    x++,y--;//向左下走
            }
            if(x == m)
                ans[j] = y;
        }
        return ans;
    }
};


```

### 15. [5642. 大餐计数](https://leetcode-cn.com/problems/count-good-meals/)

```C++
class Solution {
public:
    int countPairs(vector<int>& d) {
        unordered_map<int,int> map;
        int res =0, mod= 1e9+7;
        for(auto x:d){
            for (int i=0;i<=21;i++){
                int t= (1<<i) - x;
                if(map.count(t))
                    res = (res + map[t]) % mod;

            }
            map[x] ++;
        }
        return res;

    }
};
```

### 16. [5643. 将数组分成三个子数组的方案数](https://leetcode-cn.com/problems/ways-to-split-array-into-three-subarrays/)

```c++
class Solution {
public:
    int waysToSplit(vector<int>& nums) {
        // 前缀和 + 双指针算法
        // 首先用前缀和方便后面计算
        int n = nums.size(), mod= 1e9+7;
        vector<int> s(n+1);
        for(int i=1;i<=n;i++) s[i] = s[i-1] + nums[i-1];
        int res =0;
        // 实际是两个双指针，i表示靠右的分界，从最小值3 不断右移.  j,k表示靠左分界的上下界限，每一次确定i以后，通过不断符合条件的判定，确定j,k的位置，进而确定在当前i的情况下，有几种可能
        for(int i=3,j=2,k=2;i<=n;i++){
            while (s[n]- s[i-1] < s[i-1]-s[j-1])  j++;
            while (k+1<i && s[i-1]-s[k] >= s[k]) k++;
            if (j<=k &&  (s[n]- s[i-1]) >= s[i-1]-s[j-1]  && s[i-1]-s[k-1] >= s[k-1])
                res = (res+ k-j+1) % mod;

        }
        return res;




    }
};
```

 ### 17. [1019. 链表中的下一个更大节点](https://leetcode-cn.com/problems/next-greater-node-in-linked-list/)

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> nextLargerNodes(ListNode *head)
    {
        int count = 0; //计数，作为下标
        vector<int> result;
        stack<pair<int, int>> tmp; //first为val，second为下标
        while (head)
        {
            result.push_back(0); //给result数组后面+0，1为保证长度，2是默认值（后无更大的值的话）为0
            while (!tmp.empty() &&
                   head->val > tmp.top().first) //栈不为空且head指针的val值大于栈顶的元素的值
            {
                result[tmp.top().second] = head->val; //result数组修改，满足题意要求的最大值，然后出栈，继续循环
                tmp.pop();
            }
            tmp.push(make_pair(head->val, count++)); //count++计数
            head = head->next; //下一个节点
        }
        return result;
    }
};


```



### 18. [402. 移掉K位数字](https://leetcode-cn.com/problems/remove-k-digits/)

```C++
class Solution {
public:
    string removeKdigits(string num, int k) {
        int  n = num.size();
        if (n <=k)
        {
            return "0";
        }
        vector<char> stk;
        for (auto &c:num)
        {
            while( k && !stk.empty() && stk.back() > c)
            {
                stk.pop_back();
                k--;
            }
            stk.push_back(c);
        }
        string res = "";
        //for (int i =0;i<stk.size();i++) cout<<stk[i]<<endl;
        //这里的k在上面循环中被改变了，所以需要用size来减
        int last = stk.size() -k;
        bool leadingZero = true;
        for(auto c : stk){
            //cout<<"last"<<last<<endl;
            if (leadingZero && c == '0'){
                last--;
                if (last == 0){
                    break;
                }
                continue;
            }
            leadingZero = false;
            res += c;
            last--;
            if (last == 0){
                    break;
                }
        }
        return res==""?"0":res;



    }
};
```



### 19. [1081. 不同字符的最小子序列](https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters/)

```C++
class Solution {
public:
    // C++的pop不返回东西，所以需要提前取
    // set的删除是erase
    string smallestSubsequence(string s) {
        unordered_map <char ,int> hash;
        for (auto c : s){
            hash[c]++;
        }
        set<char> used;
        vector<char> stk;
        for(auto c:s){
            if(used.count(c) == 0){
                while(!stk.empty() && stk.back()>c && hash[stk.back()]>=1){
                    char t= stk.back();
                    stk.pop_back();
                    used.erase(t);             
                }
                used.insert(c);
                stk.push_back(c);

            }
            hash[c]--;
        }
        string res ="";
        for (auto c:stk){
            res += c;
        }
        return res;
        


    }
};


// 集合可预测的情况，用vector可能比set更快
// C++的string类型一定程度上可以视为vector，empty(), back(),push_back/pop_back()都可以

class Solution {
public:
    string smallestSubsequence(string s) {
        vector<int> vis(26), num(26);
        for (char ch : s) {
            num[ch - 'a']++;
        }

        string stk;
        for (char ch : s) {
            if (!vis[ch - 'a']) {
                while (!stk.empty() && stk.back() > ch) {
                    if (num[stk.back() - 'a'] > 0) {
                        vis[stk.back() - 'a'] = 0;
                        stk.pop_back();
                    } else {
                        break;
                    }
                }
                vis[ch - 'a'] = 1;
                stk.push_back(ch);
            }
            num[ch - 'a'] -= 1;
        }
        return stk;



    }
};
```



### 20 5652. 交换链表中的节点

```C++
// 竞赛时候的做法，时间为王
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapNodes(ListNode* head, int k) {
        vector<int> a;
        for(auto p =head;p;p = p->next)  a.push_back(p->val);
        swap(a[k-1], a[a.size()-k]);
        head = new ListNode(a[0]);
        auto cur= head;
        for(int i=1;i<a.size();i++){
            cur = cur ->next = new ListNode(a[i]);
        }
        return head;

    }
};


// 也可以先推前一个指针，然后在得到结果后，下一位就是交换的目标
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapNodes(ListNode* head, int k) {
        auto dummy = new ListNode(-1);
        dummy->next = head;
        int n =0;
        for(auto p =dummy;p;p=p->next) n++;
        k ++;
        auto a= dummy;
        for(int i=0;i<k-2;i++) a= a->next;
        auto b = dummy;
        for(int i=0;i<n-k;i++) b = b->next;
        swap(a->next,b->next);
        swap(a->next->next,b->next->next);
        // auto p1= a->next, p2 = b->next;
        // //cout<<p1->val<<p2->val<<endl;
        // a->next = p2, b->next = p1;
        // auto tmp = p1->next;
        // p1->next = p2->next;
        // p2->next = tmp;
        return dummy->next;
        
        
        

    }
};
```



### 21. 5650. 执行交换操作后的最小汉明距离



```c++
class Solution {
public:
    vector<int> p;
    
    // 并查集find
    int find(int x){
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
    
    int minimumHammingDistance(vector<int>& a, vector<int>& b, vector<vector<int>>& as) {
        //需要找到一个特殊性质：可以把多个可交换相连的数对作为联通块，然后在一个连通块内部的数对，顺序可以随意变换。这里需要用到并查集
        int n = a.size();
        for(int i=0;i<n;i++) p.push_back(i);
        for(auto &t:as) p[find(t[0])] = find(t[1]);
        
        vector<unordered_multiset<int>> hash(n);
        for(int i=0;i<n;i++)
            hash[find(i)].insert(a[i]);
        int res =0;
        for(int i=0;i<n;i++){
            auto& h = hash[find(i)];
            //cout<<str(h.find(b[i])<<endl;
            if (h.count(b[i])) {
                // 这里的find不是并查集里的find，而是set里的，返回一个迭代器，需要加* 才能看到值
                h.erase(h.find(b[i]));
                
            }
            else res ++;
        }
        return res;
            

    }
};
```



### 22. [1202. 交换字符串中的元素](https://leetcode-cn.com/problems/smallest-string-with-swaps/)

```c++
class Solution {
public:
    vector<int> t;
    int find(int x){
        if (t[x] != x){
            t[x] = find(t[x]);
        }
        return t[x];
    }
    void union_(int x,int y){
        if (find(t[x]) == find(t[y])) return;
        else{
            t[find(t[y])] = find(t[x]);
        }
    }

    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n = s.size();
        for(int i=0;i<n;i++) t.push_back(i);
        for(auto& p:pairs){
            union_(p[0], p[1]);
        }
        unordered_map<int,vector<char>> hash;
        for(int i=0;i<n;i++){
            int j = find(i);
            if(hash.count(j)) hash[j].push_back(s[i]);
            else{
                vector<char> tmp;
                tmp.push_back(s[i]);
                hash[j]=tmp;

            }
        }
        for (auto& [x,vec] : hash){
            sort(vec.begin(), vec.end(),greater<int>());
        }
        //for(int i=0;i<n;i++) cout<<t[i]<<" "<<find(i)<<endl;
        for(int i =0;i<s.size();i++){
            int x = find(i);
            s[i] = hash[x].back();
            hash[x].pop_back();
        }
        return s;


    }
};
```

### 23. [1717. 删除子字符串的最大得分](https://leetcode-cn.com/problems/maximum-score-from-removing-substrings/)

```c++
class Solution {
public:
    int maximumGain(string a, int x, int y) {
        stack<char> s, t;
        int ret = 0;
        // 处理优化
        if(x > y) {
            swap(x, y);
            reverse(a.begin(), a.end());
        } 
        // 先处理 ba
        for(char c : a) {
            if(c != 'a') s.push(c);
            else {
                // 形成 ba 子字符串
                if(!s.empty() && s.top() == 'b') {
                    s.pop();
                    ret += y;
                } else {
                    s.push(c);
                }
            }
        }
        // 再处理 ab
        while(!s.empty()) {
            char c = s.top();
            s.pop();
            if(c != 'a') t.push(c);
            else {
                if(!t.empty() && t.top() == 'b') {
                    t.pop();
                    ret += x;
                } else {
                    t.push(c);
                }
            }
        }
        return ret;


    }
};
```

### 24. [5662. 满足三条件之一需改变的最少字符数](https://leetcode-cn.com/problems/change-minimum-characters-to-satisfy-one-of-three-conditions/)

```c++
class Solution {
    /*一开始想用最大最小值定界，反而想错了，这题本质上是暴力枚举，先计算出频次数组，然后遍历找到可能*/
public:
    int work(vector<int> s1, vector<int> s2){
        //让s1中所有字符小于s2中所有字符
        int res = INT_MAX;
        for(int i=1;i<26;i++){//只能从1开始，因为没有比a更小的字母
            int cnt=0;
            for(int j =i;j<26;j++) cnt +=s1[j];//计算s1中》=i的数，这是需要变的
            for(int j=0;j<i;j++) cnt+=s2[j];//count the number of aphpa which <i,which should also be changed
            res =  min(res,cnt);

        }
        return res;


    }


    int minCharacters(string a, string b) {
        vector<int> s1(26),s2(26);
        for(auto c:a) s1[c-'a']++;
        for (auto c:b) s2[c-'a']++;
        int res = INT_MAX;
        int m= a.size(),n= b.size();
        //条件3
        for(int i=0;i<26;i++) res = min(res,n+m-(s1[i]+s2[i]));

        return min(res,min(work(s1,s2), work(s2,s1)));
        

    }
};
```

### 25. [5665. 从相邻元素对还原数组](https://leetcode-cn.com/problems/restore-the-array-from-adjacent-pairs/)

```c++
class Solution {
public:
    unordered_map<int, vector<int>> g;
    vector<int> path;
    void dfs(int u,int father){
        path.push_back(u);
        for(int x:g[u]){
            if(x != father){
                dfs(x,u);
            }
        }
    }

    vector<int> restoreArray(vector<vector<int>>& ap) {
        unordered_map<int, int> cnt;
        for(auto& e:ap){
            int a = e[0], b= e[1];
            g[a].push_back(b), g[b].push_back(a);
            cnt[a]++, cnt[b]++;
        }
        int root =0 ;
        for(auto [k,v]: cnt){
            if (v==1){
                root= k;
                break;
            }
        }
        dfs(root, -1);
        return path;

    }
};
```

### 26. [5667. 你能在你最喜欢的那天吃到你最喜欢的糖果吗？](https://leetcode-cn.com/problems/can-you-eat-your-favorite-candy-on-your-favorite-day/)

```C++

typedef long long LL;

class Solution {
public:
    bool check(LL a, LL b, LL c, LL d){
        if(b<c|| d<a) return false;
        return true;
    }
    vector<bool> canEat(vector<int>& w, vector<vector<int>>& queries) {
        int n = w.size();
        vector<LL> s(n+1); // 数据会溢出，所以要转换
        for(int i =1;i<=n;i++){
            s[i] = s[i-1] + w[i-1];
        }
        vector<bool> res;
        for(auto& q: queries){
            int t =q[0], d= q[1], c = q[2];
           /// bool t = check(d+1, (LL)(d+1)*c, s[t]+1, s[t+1]);
            res.push_back(check(d+1, (LL)(d+1)*c, s[t]+1, s[t+1]));

        }
        return res;

    }
};
```

### 27. [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)

```c++
class Solution {
public:
    int characterReplacement(string s, int k) {
        vector<int> num(26);
        int n = s.length();
        int maxn = 0;
        int left = 0, right = 0;
        int res = 0;
        while (right < n) {
            num[s[right] - 'A']++;
            maxn = max(maxn, num[s[right] - 'A']);
            if (right - left + 1 - maxn > k) {
                num[s[left] - 'A']--;
                left++;
                res = max(res, right - left);
            }
            right++;
        }
        return max(right - left,res);
    }
};


```

### 28. [1423. 可获得的最大点数](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards/)

```C++
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        // python正向求解，两段拼接，C++反向求解，取出的最大值，说明剩下的连续段的最小值
        int n = cardPoints.size();
        // 滑动窗口大小为 n-k
        int windowSize = n - k;
        // 选前 n-k 个作为初始值
        int sum = accumulate(cardPoints.begin(), cardPoints.begin() + windowSize, 0);
        int minSum = sum;
        for (int i = windowSize; i < n; ++i) {
            // 滑动窗口每向右移动一格，增加从右侧进入窗口的元素值，并减少从左侧离开窗口的元素值
            sum += cardPoints[i] - cardPoints[i - windowSize];
            minSum = min(minSum, sum);
        }
        return accumulate(cardPoints.begin(), cardPoints.end(), 0) - minSum;
    }
};


```

### 29. [978. 最长湍流子数组](https://leetcode-cn.com/problems/longest-turbulent-subarray/)

```c++
class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        int n = arr.size();
        int ret = 1;
        int left =0, right=0;
        while(right<n-1){
            if(left == right){
                if (arr[left] == arr[left+1]){
                    left++;
                }
                right++;
            }
            else{
                // 其实不用考虑right的奇偶性，能走下来，就可以确定right-1和right 的大小关系， &&筛选后半部，符合的自然就是，否则，更新左边界
                if (arr[right -1] <arr[right] && arr[right] > arr[right+1]){
                    right++;
                }else if(arr[right -1] >arr[right] && arr[right] < arr[right+1]){
                    right++;
                }else{
                    left = right;
                }
            }
            ret = max(ret, right - left + 1);
        }
        return ret;

    }
};
```

### 30. [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

```C++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        vector<int> cnt1(26), cnt2(26);
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1[i] - 'a'];
            ++cnt2[s2[i] - 'a'];
        }
        if (cnt1 == cnt2) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2[i] - 'a'];
            --cnt2[s2[i - n] - 'a'];
            if (cnt1 == cnt2) { // C++貌似vector直接可以比较，不用0~25挨个比
                return true;
            }
        }
        return false;
    }
};

```





## hard

### 1. [188. 买卖股票的最佳时机 IV](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iv/)

```C++
class Solution {
public:
    // C++的报错信息，经常会出现溢出，这就要考虑好正负号
    int maxProfit(int k, vector<int>& prices) {
        if(prices.empty()){
            return 0;
        }

        int n = prices.size();
        k = min(k,n/2);
        // 相较于python中的三维维，这里把买卖拆开，分成了两个二维
        vector<vector<int>> buy(n,vector<int>(k+1));
        vector<vector<int>> sell(n,vector<int>(k+1));
        // 第0天，想要手上有股票，只能现买
        buy[0][0] = -prices[0];
        sell[0][0] = 0;
        for(int i=1;i<=k;i++){
            buy[0][i] = -prices[0];
            sell[0][i] = 0;
        }

        for(int i=1;i<n;++i){
            // 在j = 0 时，j-1== -1 ，不合法，所以特殊处理：令其为0 
            //buy没有这个问题，可以不处理
            for(int j=0;j<=k;++j){
                if(j ==0){
                    sell[i][0] = 0;
                }
                else{
                    //buy[i][j] = max(buy[i-1][j], sell[i-1][j] - prices[i]);
                    sell[i][j] = max(sell[i - 1][j], buy[i - 1][j - 1] + prices[i]); 
                }
                buy[i][j] = max(buy[i-1][j], sell[i-1][j] - prices[i]);
            }

        }
        //  for (int i = 1; i < n; ++i) {
        //     buy[i][0] = max(buy[i - 1][0], sell[i - 1][0] - prices[i]);
        //     for (int j = 1; j <= k; ++j) {
        //         buy[i][j] = max(buy[i - 1][j], sell[i - 1][j] - prices[i]);
        //         sell[i][j] = max(sell[i - 1][j], buy[i - 1][j - 1] + prices[i]);   
        //     }
        // }

        return *max_element(sell[n-1].begin(),sell[n-1].end());



    }
};
```



### 2. [330. 按要求补齐数组](https://leetcode-cn.com/problems/patching-array/)

```c++
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        int patches = 0;
        long long x = 1;
        int length = nums.size(),index =0;
        while(x<=n){
            if (index <length && nums[index]<=x){
                x +=nums[index];
                index++;
            }else{
                x<<=1;
                patches++;
            }
        }

        return patches;
    }
};
```

### 3. [440. 字典序的第K小数字](https://leetcode-cn.com/problems/k-th-smallest-in-lexicographical-order/)

```c++
class Solution {
public:
    #define  LL long long
    int findKthNumber(int n, int k) {
        int prefix = 1;
        k--;
        while (k>0){
            int cnt = getCnt(n,prefix);
            if (cnt<=k){ // 向右
                k -= cnt;
                prefix++;
            }else{      //向下
                k--;
                prefix *= 10;
            }
        }
        return prefix;
        


    }
    int getCnt(LL n,LL prefix){
        LL cnt =0;
        for(LL first = prefix,second= prefix+1;first<=n; first *= 10 , second*=10){
            cout << first<<endl;
            cnt += min(n+1,second) - first;
            cout<<"cnt:"<<cnt<<endl;
        }
        return cnt;
    }

};
```

### 4.[1703. 得到连续 K 个 1 的最少相邻交换次数](https://leetcode-cn.com/problems/minimum-adjacent-swaps-for-k-consecutive-ones/)

```c++
class Solution {
public:
    int minMoves(vector<int>& nums, int k) {
        // 实际上结果将连续的1放在一起，1的相对顺序不能变，如果变的话，1和1交换没有意义，就不是最优解
        vector<int> a;
        for(int i=0;i<nums.size();i++){
            if (nums[i]){
                // 这是实际上是在做映射，将计算把每个1贴合在一起的间距，之所以-size是因为等差的，依次放到相邻的下一格
                a.push_back(i-a.size());
            }
        }
        int n = a.size();
        for (int i =0 ;i<n;i++){
            cout<<a[i]<<endl;
        }
        // 为了后面计算方便，这里搞了一个前缀和，统计相对最开始0的偏移距离,可以理解为把1从左到右码齐，每码齐一个1，所需要的总的移动次数
        typedef long long LL;
        vector<LL> s(n+1);
        for(int i=1;i<=n;i++){
            s[i] = s[i-1] + a[i-1];
        }
        cout<<"=================="<<endl;
        for (int i=0;i<=n;i++){
            cout<<s[i]<<endl;
        }
        LL res = 1e18;
        // for(int i =k;i<=n;i++){
        //     int l = i-k+1,r=i;
        //     int mid = (l+r)/2;
        //     // s的坐标相对a的坐标偏移了1，所以这里要减1 
        //     LL x = a[mid-1];
        //     LL left = x* (mid-l) -(s[mid-1] - s[l-1]);
        //     LL right = (s[r] - s[mid]) - x*(r-mid);
        //     res =min(res,left+right);
        // }
        //调整一下偏移，更易懂一些
        for(int i =0;i+k<=n;i++){
            int l = i,r = i+k-1;
            int mid = (l+r)/2;
            
            LL x = a[mid];
            LL left = x* (mid-l) -(s[mid] - s[l]);
            LL right = (s[r+1] - s[mid+1]) - x*(r-mid);
            res =min(res,left+right);
        }
        return res;

    }
};class Solution {
public:
    int minMoves(vector<int>& nums, int k) {
        // 实际上结果将连续的1放在一起，1的相对顺序不能变，如果变的话，1和1交换没有意义，就不是最优解
        vector<int> a;
        for(int i=0;i<nums.size();i++){
            if (nums[i]){
                // 这是实际上是在做映射，将计算把每个1贴合在一起的间距，之所以-size是因为等差的，依次放到相邻的下一格
                a.push_back(i-a.size());
            }
        }
        int n = a.size();
        for (int i =0 ;i<n;i++){
            cout<<a[i]<<endl;
        }
        // 为了后面计算方便，这里搞了一个前缀和，统计相对最开始0的偏移距离,可以理解为把1从左到右码齐，每码齐一个1，所需要的总的移动次数
        typedef long long LL;
        vector<LL> s(n+1);
        for(int i=1;i<=n;i++){
            s[i] = s[i-1] + a[i-1];
        }
        for (int i=0;i<=n;i++){
            cout<<s[i]<<endl;
        }
        LL res = 1e18;
        for(int i =k;i<=n;i++){
            int l = i-k+1,r=i;
            int mid = (l+r)/2;
            // s的坐标相对a的坐标偏移了1，所以这里要减1 
            LL x = a[mid-1];
            LL left = x* (mid-l) -(s[mid-1] - s[l-1]);
            LL right = (s[r] - s[mid]) - x*(r-mid);
            res =min(res,left+right);
        }
        return res;

    }
};
```

### 5.[5644. 得到子序列的最少操作次数](https://leetcode-cn.com/problems/minimum-operations-to-make-a-subsequence/)

```C++
// 转化成最长公共子序列问题，貌似需要模板，稍后看看
class Solution {
public:
    int minOperations(vector<int>& target, vector<int>& arr) {
        unordered_map<int, int> pos;
        for (int i=0;i<target.size();i++){
            pos[target[i]] =i;
        }
        vector<int> a;
        for(auto x:arr){
            if (pos.count(x)){
                a.push_back(pos[x]);
            }
        }
        int len =0;
        vector<int> q(a.size()+1);
        for(int i=0;i<a.size();i++){
            int l=0,r = len;
            while(l<r){
                int mid = (l+r+1)>>1;
                if(q[mid]<a[i]) l =mid;
                else r =mid-1;
            }
            len = max(len,r+1);
            q[r+1] = a[i];
        }
        return target.size()-len;

    }
};
```

### 6. [123. 买卖股票的最佳时机 III](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/)

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy1= -prices[0], sell1=0;
        int buy2 = -prices[0], sell2 = 0;
        for(auto p:prices){
            buy1 = max(buy1,-p);
            sell1 = max(sell1, buy1+p);
            buy2 = max(buy2,sell1 - p);
            sell2 = max(sell2, buy2+p);
        }
        return sell2;

    }
};
```

### 7. 5639. 完成所有工作的最短时间

```c++
class Solution {
public:
    vector<int> jobs;
    vector<int> s;
    int res = 1e9;
    // 暴力搜索，考虑两种可能，一种将当前任务交给之前选中的人，dfs
    // 另一种，另开一个新的工人（假如还有工人没有任务），新开一个
    void dfs(int a,int b, int c){
        // 剪枝
        if(c>res) return;
        if(a==jobs.size()){
            res = c;
            return;
            
        }
        for (int i=0;i<b;i++){
            s[i]+=jobs[a];
            dfs(a+1, b,max(c,s[i]));
            s[i] -= jobs[a];
        }
        if(b<s.size()){
            s[b] = jobs[a];
            dfs(a+1, b+1, max(c,s[b]));
            s[b] = 0;
        }
    }
    int minimumTimeRequired(vector<int>& _jobs, int k) {
        jobs = _jobs;
        s.resize(k);
        dfs(0,0,0);
        return res;
        
        

    }
};
```

### 8. [5666. 回文串分割 IV](https://leetcode-cn.com/problems/palindrome-partitioning-iv/)

```C++
class Solution {
public:
    bool checkPartitioning(string s) {
        int n = s.size();
        vector<vector<bool>> f(n, vector<bool>(n));// 初始化二维数组
        // dp做法
        // 由于dp特性f[i+1][j-1]需要先被算出来，所以大到小枚举
        for(int i=n-1;i>=0;i--){
            for(int j=i;j<n;j++){
                if(i==j) f[i][j] = true; // 单个字符一定是
                else if(i +1 ==j) f[i][j] = s[i] ==s[j];//两个字母，判断是否相等
                else f[i][j] = s[i]==s[j] && f[i+1][j-1]; // 其他情况，首先考虑两端是否，相等，然后考虑内部的情况，由于之前遍历过，所以这里直接取值就好

            }
        }
        for(int i=0;i<n;i++){
            for(int j =i+1;j+1<n;j++) {// 要保证最后一段存在，所以这个j+1<n
                if(f[0][i] &&f[i+1][j] &&f[j+1][n-1])
                    return true;

            }
        }
        return false;


    }
};
```



### 9. [992. K 个不同整数的子数组](https://leetcode-cn.com/problems/subarrays-with-k-different-integers/)

```c++
class Solution {
public:
    int subarraysWithKDistinct(vector<int>& A, int K) {
        int n = A.size();
        vector<int> num1(n + 1), num2(n + 1);
        int tot1 = 0, tot2 = 0;
        int left1 = 0, left2 = 0, right = 0;
        int ret = 0;
        while (right < n) {
            if (!num1[A[right]]) {
                tot1++;
            }
            num1[A[right]]++;
            if (!num2[A[right]]) {
                tot2++;
            }
            num2[A[right]]++;
            while (tot1 > K) {
                num1[A[left1]]--;
                if (!num1[A[left1]]) {
                    tot1--;
                }
                left1++;
            }
            while (tot2 > K - 1) {
                num2[A[left2]]--;
                if (!num2[A[left2]]) {
                    tot2--;
                }
                left2++;
            }
            ret += left2 - left1;
            right++;
        }
        return ret;
    }
};

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/subarrays-with-k-different-integers/solution/k-ge-bu-tong-zheng-shu-de-zi-shu-zu-by-l-9ylo/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

