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







## hard



