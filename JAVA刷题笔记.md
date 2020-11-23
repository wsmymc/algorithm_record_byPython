# JAVA刷题笔记



## 语法点

1. https://leetcode-cn.com/circle/article/dnbYTt/
2. 

## easy

### 1. [283. 移动零](https://leetcode-cn.com/problems/move-zeroes/)

```java
class Solution {
    //记录两个指标，将所有非零数往前移，后面都变为0
    public void moveZeroes(int[] nums) {
        if(nums==null)    return ;
        int j=0;
        for(int i=0;i<nums.length;i++){
            if(nums[i]!=0){
                nums[j++]=nums[i];
            }
        }
        for(int i=j;i<nums.length;i++){
            nums[i]=0;
        }
    }
}
```

### 2. [258. 各位相加](https://leetcode-cn.com/problems/add-digits/)

```java
//本质是使用了树根，如果自己推30之内，可以看到9循环
class Solution {
    public int addDigits(int num) {
        return (num-1)%9 +1;
    }
}
```

### 3. [263. 丑数](https://leetcode-cn.com/problems/ugly-number/)

```java
class Solution {
    //foreach 语法糖，分号，静态初始化数组，很多东西都忘掉了，需要重新捡起来
    public boolean isUgly(int num) {
        if(num == 0){
            return false;
        }
        int[] s = new int[]{2,3,5};
        for(int i:s) {
            while(num % i == 0){
                num = num/i;
                //System.out.println(num);
            }

        }
        return num == 1;

    }
}
```

### 4. [268. 丢失的数字](https://leetcode-cn.com/problems/missing-number/)

```java
class Solution {
    // 两次异或之后值归零，结果是什么就缺什么，因为没有两次异或
    public int missingNumber(int[] nums) {
        int n = nums.length;
        for (int i=0;i<nums.length;i++){
            n ^= i ^nums[i];
        }
        return n;

    }
}
```

### 5. [242. 有效的字母异位词](https://leetcode-cn.com/problems/valid-anagram/)

```java
class Solution {
    /*
    1. java语句要有分号
    2. python 中的字典是dict(),{}，defaultdict(),Java中需要Map<> = new HashMap<>();
    3. python 中使用[]可以赋值取值，Java需要get,getOrDefault(),put等方法
    4. 布尔值大小写
    5. 字符串的长度时length(),方法。获取字符串中字符的是charAt()
    */
    public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()){
            return false;
        }
        Map<Character, Integer> map =new HashMap<Character, Integer>();
        for(int i = 0; i<s.length();i++){
            char c = s.charAt(i);
            map.put(c, map.getOrDefault(c,0)+1);
        }
        for (int i = 0; i<t.length();i++){
            char c = t.charAt(i);
            map.put(c,map.getOrDefault(c,0)-1);
            if (map.get(c)<0){
                return false;
            }
        }
        return true;

    }
}
```

### 6. [485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

```Java
class Solution {
    /*
    1. 数组长度用.lenght，属性
    2. 变量声明时需要指定类型
    3. if 条件判断要在括号内部
    */
    public int findMaxConsecutiveOnes(int[] nums) {
        int cnt =0;
        int res = 0;
        for (int i =0 ;i<nums.length;i++){
            if(nums[i] == 1){
                cnt ++;
            }
            else{
                res = Math.max(res,cnt);
                cnt = 0;
            }
        }
        return Math.max(res,cnt);

    }
}
```

### 7. [509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

```java
class Solution {
    public int fib(int N) {
        if(N<=1){
            return N;
        }
        if (N==2){
            return 1;
        }
        int cur = 0;
        int pre1 = 1;
        int pre2 =1;
        for (int i=3;i<=N;i++){
            cur = pre1 + pre2;
            pre1 = pre2;
            pre2 = cur;
        }
        return cur;

    }
}
```

### 8. [566. 重塑矩阵](https://leetcode-cn.com/problems/reshape-the-matrix/)

```Java
class Solution {
    public int[][] matrixReshape(int[][] nums, int r, int c) {
        // 不用一位数组中间过渡，也可以。
        if (nums.length == 0|| r* c != nums.length * nums[0].length){
            return nums;
        }
        int[][] res = new int[r][c];
        int row=0,col=0;
        for(int i =0; i<nums.length;i++){
            for(int j =0; j<nums[0].length;j++){
                res[row][col] = nums[i][j];
                col ++;
                if(col == c){
                    row ++;
                    col =0;
                }

            }
        }
        return res;

    }
}
```

### 9. [605. 种花问题](https://leetcode-cn.com/problems/can-place-flowers/)

```java
class Solution {
    public boolean canPlaceFlowers(int[] flowerbed, int n) {
        int i=0,cnt=0;
        while(i<flowerbed.length){
            // 巧妙地地方在于两点：
            // 1. 利用短路特性，将，最左最右两种情况包含在内 2. 原地修改，记录情况
            if(flowerbed[i]==0 && (i==0 || flowerbed[i-1]==0) && ( i== flowerbed.length-1||flowerbed[i+1]==0)){
                flowerbed[i++] = 1;
                cnt++;
            }
            if(cnt>=n){
                return true;
            }
            i++;
        }
        return false;

    }
}
```

### 10. [643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)

```java
class Solution {/*
小数考虑用double声明
分号
Math.max
*/
    public double findMaxAverage(int[] nums, int k) {
        double _sum = 0;
        for(int i =0 ;i<k;i++){
            _sum += nums[i];
        }
        double res = _sum;
        for(int i =k;i<nums.length;i++){
            _sum += nums[i]-nums[i-k];
            res = Math.max(res,_sum);
        }
        return res/k;
    }
}
```

### 11. [661. 图片平滑器](https://leetcode-cn.com/problems/image-smoother/)

```java
class Solution {
    public int[][] imageSmoother(int[][] M) {
        int R= M.length,C= M[0].length;
        int[][] res =new int[R][C];

        for(int r= 0;r<R;r++){
            for(int c =0 ;c<C;c++){
                int cnt = 0;
                for(int nr=r-1;nr<=r+1;nr++){
                    for(int nc = c-1;nc<=c+1;nc++){
                        if (0<= nr &&nr<R&&0<=nc&&nc<C){
                            res[r][c] += M[nr][nc];
                            cnt ++;
                        }
                    }
                }
                res[r][c] /=cnt;
            }
        }
        return res;

    }
}
```



## mediium

### 1. [452. 用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

```Java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if (points.length == 0){
            return 0;
        }
        // 有选择的排序，以前一直记不住。
        // Comparator 是一个接口，这里需要实现一个匿名方法来作为参数，传进Arrays.sort()排序
        // 实现接口需要重写compare方法，这里x>y 返回1，是从小到大，反之是从大到小
        Arrays.sort(points, new Comparator<int[]>(){
            public int compare(int[] x, int[] y){
                if (x[1]>y[1]){
                    return 1;
                }else if(x[1]<y[1]){
                    return -1;
                }else{
                    return 0;
                }
            }
        });
        int pos = points[0][1];
        int res = 1;
        for(int[] balloon: points){
            if(balloon[0]>pos){
                res++;
                pos = balloon[1];
            }
        }
        return res;

    }
}
```



## hard

