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







## medium









## hard



