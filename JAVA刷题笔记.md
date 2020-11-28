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

### 12. [717. 1比特与2比特字符](https://leetcode-cn.com/problems/1-bit-and-2-bit-characters/)

```java
class Solution {
    public boolean isOneBitCharacter(int[] bits) {
        int i = bits.length -2;
        // 统计从倒数第二位开始，到倒数第二个0开始，有多少个1，做差求出来。根据1的奇偶性，就可以确定合法与否
        while(i>=0 && bits[i]>0){
            i--;
        }
        return (bits.length - i)%i == 0;

    }
}
```

### 13. [724. 寻找数组的中心索引](https://leetcode-cn.com/problems/find-pivot-index/)

```java
class Solution {
    public int pivotIndex(int[] nums) {
        int sum = 0;
        for(int i =0 ;i<nums.length;i++){
            sum += nums[i];
        }
        int l_sum = 0;
        for(int i =0; i<nums.length;i++){
            if (l_sum == sum -nums[i]-l_sum){
                return i;
            }
            l_sum += nums[i];
        }
        return -1;

    }
}
```

### 14. [697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

```java
class Solution {
    // 泛型以及哈希表创建
    //getOrDeault
    //Collections.max
    public int findShortestSubArray(int[] nums) {
        Map<Integer,Integer>  left = new HashMap(),right = new HashMap(), cnt = new HashMap();
        for(int i=0;i<nums.length;i++){
            int x = nums[i];
            if (left.get(x) == null){
                left.put(x,i);
            }
            right.put(x,i);
            cnt.put(x,cnt.getOrDefault(x,0)+1);
        }
        int res = nums.length;
        int degree = Collections.max(cnt.values());
        for(int x:cnt.keySet()){
            if (cnt.get(x) == degree){
                res = Math.min(res,right.get(x) - left.get(x) + 1);
            }
        }
        return res;

    }
}
```

### 15. [674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

```java
class Solution {
    public int findLengthOfLCIS(int[] nums) {
        if(nums.length<=1){
            return nums.length;
        }
        int res = 1;
        int cnt = 1;
        for(int i=0;i<nums.length-1;i++){
            if(nums[i+1]> nums[i]){
                cnt++;
            }else{
                res = Math.max(res, cnt);
                cnt = 1;
            }
        }
        return Math.max(res, cnt);

    }
}
```

### 16. [665. 非递减数列](https://leetcode-cn.com/problems/non-decreasing-array/)

```java
class Solution {
    public boolean checkPossibility(int[] nums) {
        if (nums.length<3){
            return true;
        }
        int cnt = 0;
        for(int i=0;i<nums.length-1;i++){
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
        //System.out.println(cnt);
        return cnt<=1;

    }
}
```

### 17. [1370. 上升下降字符串](https://leetcode-cn.com/problems/increasing-decreasing-string/)

```java
class Solution {
    public String sortString(String s) {
        int[] num = new int[26];
        for(int i =0;i<s.length();i++){
            num[s.charAt(i)-'a']++;
        }
        StringBuffer sb = new StringBuffer();
        while(sb.length()<s.length()){
            for(int i=0;i<26;i++){
                if(num[i]>0){
                    num[i]--;
                    sb.append((char)(i+'a'));
                }
            }
            for(int i=25;i>=0;i--){
                if(num[i]>0){
                    num[i]--;
                    sb.append((char)(i+'a'));
                }
            }
        }
        return sb.toString();
    }
}
```

### 18. [747. 至少是其他数字两倍的最大数](https://leetcode-cn.com/problems/largest-number-at-least-twice-of-others/)

```java
class Solution {
    public int dominantIndex(int[] nums) {
        if(nums.length <2){
            return 0;
        }
        int[] flag = {0,1};
        if(nums[0]> nums[1]){
            flag[0] =1;
            flag[1] = 0;
        }
        for(int i =2;i<nums.length;i++){
            if(nums[i]<=nums[flag[0]]){
                continue;
            }
            else if(nums[i]>nums[flag[0]] && nums[i]<= nums[flag[1]]){
                flag[0] = i;
            }else{
                flag[0] = flag[1];
                flag[1] = i;
            }
            System.out.println(flag[1]);
        }
        if (nums[flag[1]] >= 1<<nums[flag[0]]){
            return flag[1];
        }
        return -1;

    }
}
```

### 19. [766. 托普利茨矩阵](https://leetcode-cn.com/problems/toeplitz-matrix/)

```java
class Solution {
    public boolean isToeplitzMatrix(int[][] matrix) {
        int c = matrix[0].length;
        for(int i =1 ;i<matrix.length;i++){
            for (int j = 1 ; j<c;j++){
                if(matrix[i][j] != matrix[i-1][j-1]){
                    return false;
                }
            }
        }
        return true;

        

    }
}
```

### 20. [830. 较大分组的位置](https://leetcode-cn.com/problems/positions-of-large-groups/)

```java
class Solution {
    public List<List<Integer>> largeGroupPositions(String s) {\
        // 二维列表定义确实比python 麻烦许多
        List<List<Integer>> res = new ArrayList();
        int i=0,n=s.length();
        for(int j =0 ;i<n;j++){
            if (j==n-1||s.charAt(j) != s.charAt(j+1)){
                if (j-i+1>=3){
                    // 临时生成的列表python中只用【】，java里需要先new Arraylist,填好值后再把Arraylist add 到res中
                    res.add(Arrays.asList(new Integer[]{i,j}));
                }
                i = j+1;
            }
        }
        return res;
    }
}
```

### 21. [867. 转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

```java
class Solution {
    public int[][] transpose(int[][] A) {
        int r= A.length,c = A[0].length;
        int[][] ans = new int[c][r];
        for(int i =0;i<r;i++){
            for(int j=0;j<c;j++){
                ans[j][i] = A[i][j];
            }
        }
        return ans;

    }
}
```

### 22. [888. 公平的糖果交换](https://leetcode-cn.com/problems/fair-candy-swap/)

```java
class Solution {
    public int[] fairCandySwap(int[] A, int[] B) {
        int sa =0 ,sb= 0;
        for(int x:A){
            sa += x;
        }
        for (int x:B){
            sb += x;
        }
        int gap = (sb-sa)/2;
        Set<Integer>  s = new HashSet();
        for (int x:B){
            s.add(x);
        }
        for (int x:A){
            if (s.contains(x + gap)){
                return new int[]{x,x+gap};
            }
        }
        throw null;

    }
}
```

### 23. [896. 单调数列](https://leetcode-cn.com/problems/monotonic-array/)

```java
class Solution {
    public boolean isMonotonic(int[] A) {
        if(A.length<2) return true;
        boolean b = A[0]>A[A.length-1];
        for (int i =1;i<A.length;i++){
            if (A[i] == A[i-1]){
                continue;
                //这里的两个否定实际上用的很巧妙
            }else if(A[i]>A[i-1] != b){
                continue;
            }else{
                return false;
            }
        }
        return true;

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

### 2. [222. 完全二叉树的节点个数](https://leetcode-cn.com/problems/count-complete-tree-nodes/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public int countNodes(TreeNode root) {
        //先判断层数
        if(root == null){
            return 0;
        }
        int level = 0;
        TreeNode node= root;
        while(node.left != null){
            level++;
            node = node.left;
        }
        // 知道了层数以后，可以根据完全二叉树的性质，估计出最大最小区间
        int low = 1<<level;
        int high = (1<<(level+1))-1;
        // 万物皆可二分？
        while(low<high){
            // 这种求中指的方法，应该可以防止溢出
            int mid = (high-low+1)/2 + low;
            // 判断中值点的有无
            if(exists(root,level,mid)){
                low = mid;
            }else{
                high = mid -1;
            }
        }
        return low;

    }
    public boolean exists(TreeNode root, int level,int k){
        // 这个掩码用来判断后面的节点该走的方向
        //其实这里有一个很巧妙的地方，如果给节点编号，比如5，6，7，8，那么二进制就是，101，110，111，1000，结合途中的顺序，其实从第二位开始，0代表，左下，1代表右下，本身包含了路径的顺序。
        // 又因为是从第二个节点开始的，所以掩码bits 是1 左移层数-1为，然后校验
        int bits = 1<<(level -1);
        TreeNode node = root;
        while(node != null && bits>0){
            // 1 & x =?,如果是0，说明x = 0,左下，反之右下。
            if ((bits & k) == 0){
                node = node.left;
            }else{
                node = node.right;
            }
            bits >>= 1;
        }
        //确定该节点是否存在
        return node != null;
    }
    // 时间复杂度，O(log^2n)
}
```

### 3. [454. 四数相加 II](https://leetcode-cn.com/problems/4sum-ii/)

```python
class Solution {
    public int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
        Map<Integer,Integer> cnt = new HashMap();
        for(int i:A){
            for(int j:B){
                cnt.put(i+j,cnt.getOrDefault(i+j,0)+1);
            }
        }
        int ans = 0;
        for(int i:C){
            for(int j:D){
                if(cnt.containsKey(-j-i)){
                    ans += cnt.get(-i-j);

                }
            }
        }
        return ans;

    }
}
```



## hard

### 1. [164. 最大间距](https://leetcode-cn.com/problems/maximum-gap/)

```java
class Solution {
    public int maximumGap(int[] nums) {
        // 桶排序
        int n = nums.length;
        if (n<2){
            return 0;
        }
        int min = nums[0];
        int max = nums[0];
        for (int i = 1;i<nums.length;i++){
            min = Math.min(min,nums[i]);
            max = Math.max(max, nums[i]);
        }
        // 如果最大最小相同，说明全组一个数，最大差值本身就是0
        if (max - min == 0){
            return 0;
        }

        int bucker_volumn = (int) Math.ceil((double)(max-min)/(n-1)); // 向上取整
        //每个箱子里数字的最小值和最大值
        //PS 这里其实是无脑了，不考虑实际的箱子的数量，直接搞最大可能
        int[] bucketMin = new int[n - 1];
        int[] bucketMax = new int[n - 1];
        
        //最小值初始为 Integer.MAX_VALUE
        Arrays.fill(bucketMin, Integer.MAX_VALUE);
        //最小值初始化为 -1，因为题目告诉我们所有数字是非负数
        Arrays.fill(bucketMax, -1);

        //考虑每个数字
        for (int i = 0; i < nums.length; i++) {
            //当前数字所在箱子编号
            int index = (nums[i] - min) / bucker_volumn;
            //最大数和最小数不需要考虑
            if(nums[i] == min || nums[i] == max) {
                continue;
            }
            //更新当前数字所在箱子的最小值和最大值
            bucketMin[index] = Math.min(nums[i], bucketMin[index]);
            bucketMax[index] = Math.max(nums[i], bucketMax[index]);
        }
        int maxGap = 0;
        //min 看做第 -1 个箱子的最大值
        int previousMax = min;
        //从第 0 个箱子开始计算
        for (int i = 0; i < n - 1; i++) {
            //最大值是 -1 说明箱子中没有数字，直接跳过
            if (bucketMax[i] == -1) {
                continue;
            }
            
            //当前箱子的最小值减去前一个箱子的最大值
            maxGap = Math.max(bucketMin[i] - previousMax, maxGap);
            previousMax = bucketMax[i];
        }
        //最大值可能处于边界，不在箱子中，需要单独考虑
        maxGap = Math.max(max - previousMax, maxGap);
        return maxGap;

    }
}



// java的基数排序貌似和python不太一样，估计是因为申请二维数组比较麻烦，尤其是其中的数组还是不定长的，所以使用了一些类似记录下标，从后往前，保留上一轮成果的技巧：
class Solution {
    public int maximumGap(int[] nums) {
        int n = nums.length;
        if (n < 2) {
            return 0;
        }
        long exp = 1;
        int[] buf = new int[n];
        int maxVal = Arrays.stream(nums).max().getAsInt();

        while (maxVal >= exp) {
            int[] cnt = new int[10];
            for (int i = 0; i < n; i++) {
                int digit = (nums[i] / (int) exp) % 10;
                cnt[digit]++;
            }
            for (int i = 1; i < 10; i++) {
                cnt[i] += cnt[i - 1];
            }
            //必须从后往前遍历，否则会破坏已排好的轮次
            //因为桶子里的值（即下标）是不断减小的
            //当某一位相同时，让上一位更大的（即上一轮排序结果中靠后的）获得更大的下标
            for (int i = n-1; i>=0 ; i--) {
                int digit = (nums[i] / (int) exp) % 10;
                buf[cnt[digit] - 1] = nums[i];
                cnt[digit]--;
            }
            System.arraycopy(buf, 0, nums, 0, n);
            exp *= 10;
        }

        int ret = 0;
        for (int i = 1; i < n; i++) {
            ret = Math.max(ret, nums[i] - nums[i - 1]);
        }
        return ret;
    }
}


```





