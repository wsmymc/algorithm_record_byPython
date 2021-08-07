# JAVA刷题笔记



## 语法点

1. https://leetcode-cn.com/circle/article/dnbYTt/

2. LinkedList遍历方法：[13,Java LinkedList遍历的7种方法 - 简书 (jianshu.com)](https://www.jianshu.com/p/46155da1e2cf)

3. ```java
           char[] b = a.toCharArray();   // String --> char[]
         
   ```

4. ```java
    //左闭右开的截取字符串substring   
   ```

5. 多维数组排序

    ```java
    
    int [][]a = new int [5][2];
    
    //定义一个二维数组，其中所包含的一维数组具有两个元素
    
    对于一个已定义的二位数组a进行如下规则排序,首先按照每一个对应的一维数组第一个元素进行升序排序（即a[][0]）,若第一个元素相等,则按照第二个元素进行升序排序（a[][1]）。(特别注意,这里的a[][0]或者a[][1]在java中是不能这么定义的,这里只是想说明是对于某一个一维数组的第0或1个元素进行排序)
    
    Arrays.sort(a, new Comparator<int[]>() {
    @Override
    public int compare(int[] o1, int[] o2) {
    if (o1[0]==o2[0]) return o1[1]-o2[1];
    return o1[0]-o2[0];
    }
    });
    其中o1[1]-o2[1]表示对于第二个元素进行升序排序如果为o2[1]-o1[1]则表示为降序。
    
    // 也可以用lamdba
            Arrays.sort(mat, (o1,o2)-> (o2[0] - o1[0]));
            System.out.println(Arrays.deepToString(mat));
    ```

    

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

### 24. [976. 三角形的最大周长](https://leetcode-cn.com/problems/largest-perimeter-triangle/)

```java
class Solution:
    def largestPerimeter(self, A: List[int]) -> int:
        A.sort()
        n = len(A)
        for i in range(n-1,-1,-1):
            if A[i-1] + A[i-2] > A[i]:
                return sum(A[i-2:i+1])
        return 0
```

### 25. [922. 按奇偶排序数组 II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)

```java
class Solution {
    public int[] sortArrayByParityII(int[] A) {
        int n = A.length;
        int j = 1;
        for (int i = 0; i < n; i += 2) {
            //只考虑奇数就好剩下的自然就是偶数
            if (A[i] % 2 == 1) {
                while (A[j] % 2 == 1) {
                    j += 2;
                }
                swap(A, i, j);
            }
        }   
        return A;
    }

    public void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}

```

### 26.[922. 按奇偶排序数组 II](https://leetcode-cn.com/problems/sort-array-by-parity-ii/)

```JAVA
class Solution {
    // == 的优先级比&高，所以记得用的时候带括号
    public int[] sortArrayByParityII(int[] A) {
        int n = A.length;
        int j = 1;
        for (int i = 0; i < n; i += 2) {
            if (A[i] % 2 == 1) {
                while (A[j] % 2 == 1) {
                    j += 2;
                }
                swap(A, i, j);
            }
        }   
        return A;
    }

    public void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}

```

### 27. [1652. 拆炸弹](https://leetcode-cn.com/problems/defuse-the-bomb/)

```java
class Solution {
    public int[] decrypt(int[] code, int k) {
        int length = code.length;
        int[] result = new int[length];

        if (k == 0) {
            return result;
        } else {
            for (int i = 0; i < length; i++) {
                int sum = 0;
                for (int j = 0; j < Math.abs(k); j++) {
                    if (k > 0) {
                        sum += code[(i + j + 1) % length];
                    } else {
                        sum += code[(i - j - 1 + length) % length];
                    }
                }
                result[i] = sum;
            }
        }
        return result;



    }
}
```

### 28.[1656. 设计有序流](https://leetcode-cn.com/problems/design-an-ordered-stream/)

```java
class OrderedStream {
    String[] stream;
    int ptr = 0;
    public OrderedStream(int n) {
        // 根据长度创建String数组保存值
        stream = new String[n];
    }
    
    public List<String> insert(int id, String value) {
        // id从1起始，所以减1
        stream[id-1] = value;
        // 要返回的数组
        List<String> list = new ArrayList<>();
        // 从ptr开始，直到数组的末尾
        for (int i = ptr; i < stream.length; i++) {
            // 如果遇到流中的空值，跳出循环直接返回list
            if (stream[i] == null) {
                break;
            } else { // 如果该处不为空值，那么ptr就可以到这个地方，返回的list中也应包括这个值
                ptr++;
                list.add(stream[i]);
            }
        }
        return list;
    }
}


/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream obj = new OrderedStream(n);
 * List<String> param_1 = obj.insert(id,value);
 */
```

### 29. [1413. 逐步求和得到正数的最小值](https://leetcode-cn.com/problems/minimum-value-to-get-positive-step-by-step-sum/)

```java
class Solution {
    public int minStartValue(int[] nums) {
        int tmp= nums[0], _sum = nums[0];
        for(int i =1;i<nums.length;i++){
            _sum += nums[i];
            tmp =Math.min(_sum,tmp);
        }
        if(tmp>=0){
            return 1;
        }
        return 1-tmp;
    }
}
```

### 30. [1122. 数组的相对排序](https://leetcode-cn.com/problems/relative-sort-array/)

```python
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int upper = 0;
        for(int x :arr1){
            upper = Math.max(upper,x);
        }
        int[] freq = new int[upper+1];
        for(int x : arr1){
            freq[x]++;
        }
        int[] res= new int[arr1.length];
        int index= 0;
        for(int x:arr2){
            for(int i =0;i<freq[x];i++){
                res[index++] = x;
            }
            freq[x] = 0;
        }
        for(int x=0;x<=upper;x++){
            for(int i =0;i<freq[x];i++){
                res[index++] = x;
            }
        }
        return res;
    }
}
```

### 31. [204. 计数质数](https://leetcode-cn.com/problems/count-primes/)

```java
class Solution {
    public int countPrimes(int n) {
        List<Integer> list = new ArrayList<Integer>();
        int[] isprime = new int[n];
        Arrays.fill(isprime,1);
        for(int i=2;i<n;i++){
            if (isprime[i] == 1){
                list.add(i);
            }
            for( int j=0;j<list.size()&& i * list.get(j)<n;j++){
                isprime[i*list.get(j)] = 0;
                if(i%list.get(j)==0){
                    break;
                }
            }
        }
        return list.size();

    }
}
```

#### 32. [1226. 哲学家进餐](https://leetcode-cn.com/problems/the-dining-philosophers/)

```python
class DiningPhilosophers {
    ReentrantLock[] forks = new ReentrantLock[5];

    public DiningPhilosophers() {
        for (int i=0;i<5;i++){
            forks[i] = new ReentrantLock();
        }
        
    }

    // call the run() method of any runnable to execute its code
    public void wantsToEat(int philosopher,
                           Runnable pickLeftFork,
                           Runnable pickRightFork,
                           Runnable eat,
                           Runnable putLeftFork,
                           Runnable putRightFork) throws InterruptedException {
                                int fork1 = philosopher; 
                                int fork2 = (philosopher + 1) % 5;

                                forks[Math.min(fork1, fork2)].lock();
                                forks[Math.max(fork1, fork2)].lock();
                                pickLeftFork.run();
                                pickRightFork.run();
                                eat.run();
                                putLeftFork.run();
                                putRightFork.run();
                                forks[Math.min(fork1, fork2)].unlock();
                                forks[Math.max(fork1, fork2)].unlock();


        
    }
}
```





### 33. [1748. 唯一元素的和](https://leetcode-cn.com/problems/sum-of-unique-elements/)

```java
class Solution {
    public int sumOfUnique(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        for(int i =0; i<nums.length;i++){
            if(!map.containsKey(nums[i])){
                sum += nums[i];
                map.put(nums[i], 1);
            }else{
                if(map.get(nums[i])>0){
                    sum -= nums[i];
                    map.put(nums[i], 0);
                }
            }
        }
        return sum;

    }
}
```

### 34. [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)

```java

class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while(left<=right) {
            int mid = left + (right - left) / 2;
            if(nums[mid] == target) {
                return mid;
            } else if(nums[mid] > target) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return -1;
    }
}

```

### 35. [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

```Java
算法：

正如我们在上面讨论的，这里将采用 LinkedList 实现 HashSet 中的桶。

对于每个功能 add，remove，contains，我们首先生成桶的散列值，操作相对应的桶。

PythonJava

class MyHashSet {
  private Bucket[] bucketArray;
  private int keyRange;

  /** Initialize your data structure here. */
  public MyHashSet() {
    this.keyRange = 769;
    this.bucketArray = new Bucket[this.keyRange];
    for (int i = 0; i < this.keyRange; ++i)
      this.bucketArray[i] = new Bucket();
  }

  protected int _hash(int key) {
    return (key % this.keyRange);
  }

  public void add(int key) {
    int bucketIndex = this._hash(key);
    this.bucketArray[bucketIndex].insert(key);
  }

  public void remove(int key) {
    int bucketIndex = this._hash(key);
    this.bucketArray[bucketIndex].delete(key);
  }

  /** Returns true if this set contains the specified element */
  public boolean contains(int key) {
    int bucketIndex = this._hash(key);
    return this.bucketArray[bucketIndex].exists(key);
  }
}


class Bucket { // 桶下是链表
  private LinkedList<Integer> container;

  public Bucket() {
    container = new LinkedList<Integer>(); // 链表
  }

  public void insert(Integer key) {
    int index = this.container.indexOf(key); //链表的API
    if (index == -1) {
      this.container.addFirst(key);
    }
  }

  public void delete(Integer key) {
    this.container.remove(key);
  }

  public boolean exists(Integer key) {
    int index = this.container.indexOf(key);
    return (index != -1);
  }
}

作者：LeetCode
链接：https://leetcode-cn.com/problems/design-hashset/solution/she-ji-ha-xi-ji-he-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 36. [706. 设计哈希映射](https://leetcode-cn.com/problems/design-hashmap/)

```java

class Pair<U, V> {   //  类似C++中的pair
  public U first;
  public V second;

  public Pair(U first, V second) {
    this.first = first;
    this.second = second;
  }
}


class Bucket {
  private List<Pair<Integer, Integer>> bucket;  // 列表

  public Bucket() {
    this.bucket = new LinkedList<Pair<Integer, Integer>>();// 每个桶实际上就是一个数组
  }

  public Integer get(Integer key) {
    for (Pair<Integer, Integer> pair : this.bucket) {
      if (pair.first.equals(key))
        return pair.second;
    }
    return -1;
  }

  public void update(Integer key, Integer value) {
    boolean found = false;
    for (Pair<Integer, Integer> pair : this.bucket) {
      if (pair.first.equals(key)) {
        pair.second = value;
        found = true;
      }
    }
    if (!found)
      this.bucket.add(new Pair<Integer, Integer>(key, value));
  }

  public void remove(Integer key) {
    for (Pair<Integer, Integer> pair : this.bucket) {
      if (pair.first.equals(key)) {
        this.bucket.remove(pair);
        break;
      }
    }
  }
}

class MyHashMap {
  private int key_space;
  private List<Bucket> hash_table;

  /** Initialize your data structure here. */
  public MyHashMap() {
    this.key_space = 2069; //桶个数
    this.hash_table = new ArrayList<Bucket>();
    for (int i = 0; i < this.key_space; ++i) {
      this.hash_table.add(new Bucket());  // 添加桶
    }
  }

  /** value will always be non-negative. */
  public void put(int key, int value) {
    int hash_key = key % this.key_space;
    this.hash_table.get(hash_key).update(key, value);// list的方法中，不能随机取，需要有get
  }

  /**
   * Returns the value to which the specified key is mapped, or -1 if this map contains no mapping
   * for the key
   */
  public int get(int key) {
    int hash_key = key % this.key_space;
    return this.hash_table.get(hash_key).get(key);
  }

  /** Removes the mapping of the specified value key if this map contains a mapping for the key */
  public void remove(int key) {
    int hash_key = key % this.key_space;
    this.hash_table.get(hash_key).remove(key);
  }
}

/**
 * Your MyHashMap object will be instantiated and called as such: MyHashMap obj = new MyHashMap();
 * obj.put(key,value); int param_2 = obj.get(key); obj.remove(key);
 */

```

### 37. [703. 数据流中的第 K 大元素](https://leetcode-cn.com/problems/kth-largest-element-in-a-stream/)

```java
// java堆的API
class KthLargest {
    PriorityQueue<Integer> pq;
    int k;

    public KthLargest(int k, int[] nums) {
        this.k = k;
        pq = new PriorityQueue<Integer>();
        for(int x : nums){
            add(x);
        }

    }
    
    public int add(int val) {
        pq.offer(val);
        while (pq.size() > this.k){
            pq.poll();
        }
        return pq.peek();

    }
}

/**
 * Your KthLargest object will be instantiated and called as such:
 * KthLargest obj = new KthLargest(k, nums);
 * int param_1 = obj.add(val);
 */
```

### 38. [697. 数组的度](https://leetcode-cn.com/problems/degree-of-an-array/)

```java
class Solution {
    public int findShortestSubArray(int[] nums) {
        Map<Integer, int[]> map = new HashMap<Integer, int[]>();
        int n = nums.length;
        for (int i=0;i<n;i++){
            if(map.containsKey(nums[i])){
                map.get(nums[i])[0]++;
                map.get(nums[i])[2]=i;
            }else{
                map.put(nums[i], new int[]{1,i,i});
            }
        }
        int minLen = n, maxCnt = 0;
        for(Map.Entry<Integer, int[]> entry: map.entrySet()){
            int[] t = entry.getValue();
            if(maxCnt<t[0]){
                maxCnt = t[0];
                minLen = t[2]-t[1]+1;
            }else if(maxCnt == t[0]){
                if(minLen>t[2]-t[1]+1){
                    minLen=t[2]-t[1]+1;
                }
            }
        }
        return minLen;


    }
}
```

### 39. [5685. 交替合并字符串](https://leetcode-cn.com/problems/merge-strings-alternately/)

```java
class Solution {
    public String mergeAlternately(String s1, String s2) {
        int l1=s1.length(),l2 = s2.length();
        int minLen = Math.min(l1,l2);
        StringBuilder res = new StringBuilder();
        for(int i=0;i<minLen;i++){
            res.append(s1.charAt(i));
            res.append(s2.charAt(i));

        }
        if (l1>l2){
            res.append(s1, minLen, l1);
        }else {
            res.append(s2, minLen, l2);
        }
        return res.toString();

    }
}
```

### 40. [1022. 从根到叶的二进制数之和](https://leetcode-cn.com/problems/sum-of-root-to-leaf-binary-numbers/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int ans; // 类变量
    public int sumRootToLeaf(TreeNode root) {
        sumbinary(root, 0);
        return ans;
    }

    public void sumbinary(TreeNode root, int cur){
        if(root == null){
            return;
        }
        if(root.left == null && root.right == null){
            ans += cur * 2 + root.val;
            return;
        }
        sumbinary(root.left, cur * 2 + root.val);
        sumbinary(root.right, cur * 2 + root.val);
    }
}

```



### 41. [1030. 距离顺序排列矩阵单元格](https://leetcode-cn.com/problems/matrix-cells-in-distance-order/)

```java

class Solution {
    public int[][] allCellsDistOrder(int R, int C, int r0, int c0) {
        int maxDist = Math.max(r0, R - 1 - r0) + Math.max(c0, C - 1 - c0);// Math
        List<List<int[]>> bucket = new ArrayList<List<int[]>>();
        for (int i = 0; i <= maxDist; i++) {
            bucket.add(new ArrayList<int[]>());// 使用数组作为桶，理当如此，因为更有序
        }

        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                int d = dist(i, j, r0, c0);
                bucket.get(d).add(new int[]{i, j});
            }
        }
        int[][] ret = new int[R * C][];
        int index = 0;
        for (int i = 0; i <= maxDist; i++) {
            for (int[] it : bucket.get(i)) {
                ret[index++] = it;
            }
        }
        return ret;
    }

    public int dist(int r1, int c1, int r2, int c2) {
        return Math.abs(r1 - r2) + Math.abs(c1 - c2);
    }
}

```

### 42. [705. 设计哈希集合](https://leetcode-cn.com/problems/design-hashset/)

```java
//不使用任何内建的哈希表库设计一个哈希集合（HashSet）。 
//
// 实现 MyHashSet 类： 
//
// 
// void add(key) 向哈希集合中插入值 key 。 
// bool contains(key) 返回哈希集合中是否存在这个值 key 。 
// void remove(key) 将给定值 key 从哈希集合中删除。如果哈希集合中没有这个值，什么也不做。 
// 
// 
//
// 示例： 
//
// 
//输入：
//["MyHashSet", "add", "add", "contains", "contains", "add", "contains", "remove
//", "contains"]
//[[], [1], [2], [1], [3], [2], [2], [2], [2]]
//输出：
//[null, null, null, true, false, null, true, null, false]
//
//解释：
//MyHashSet myHashSet = new MyHashSet();
//myHashSet.add(1);      // set = [1]
//myHashSet.add(2);      // set = [1, 2]
//myHashSet.contains(1); // 返回 True
//myHashSet.contains(3); // 返回 False ，（未找到）
//myHashSet.add(2);      // set = [1, 2]
//myHashSet.contains(2); // 返回 True
//myHashSet.remove(2);   // set = [1]
//myHashSet.contains(2); // 返回 False ，（已移除） 
//
// 
//
// 提示： 
//
// 
// 0 <= key <= 106 
// 最多调用 104 次 add、remove 和 contains 。 
// 
//
// 
//
// 进阶：你可以不使用内建的哈希集合库解决此问题吗？ 
// Related Topics 设计 哈希表 
// 👍 136 👎 0


import java.util.Iterator;
import java.util.LinkedList;

//leetcode submit region begin(Prohibit modification and deletion)
class MyHashSet {
    private static final int BASE = 769;
            private LinkedList[] bucket; // 一个链表数组

    /** Initialize your data structure here. */
    public MyHashSet() {
        bucket = new LinkedList[BASE];
        for(int i=0;i<BASE; i++){
            bucket[i] = new LinkedList<Integer>();
        }

    }
    private static int hash(int key){
        return key % BASE;
    }
    public void add(int key) {
        int h = hash(key);
        Iterator<Integer> iterator = bucket[h].iterator();// 获取一个迭代器
        while(iterator.hasNext()){  // hasNext 判断是否有当前元素
            Integer e = iterator.next(); // next() 返回当前的值，并将游标后移一位
            if (e == key){
                return;
            }
        }
//        int h = hash(key);
//        Iterator<Integer> iterator = bucket[h].iterator();
//        while (iterator.hasNext()) {
//            Integer element = iterator.next();
//            if (element == key) {
//                return;
//            }
//        }
        bucket[h].offerLast(key);

    }
    
    public void remove(int key) {
//        int h = hash(key);
//        Iterator<Iterator> it = bucket[h].iterator(); // 获取一个迭代器
//        while(it.hasNext()){
//            Integer e = it.item();
//            if (e == key){
//                bucket[h].remove(e);
//            }
//        }
        int h = hash(key);
        Iterator<Integer> iterator = bucket[h].iterator();
        while (iterator.hasNext()) {
            Integer element = iterator.next();
            if (element == key) {
                bucket[h].remove(element);
                return;
            }
        }
    }
    
    /** Returns true if this set contains the specified element */
    public boolean contains(int key) {
        int h = hash(key);
        Iterator<Integer> iterator = bucket[h].iterator();
        while (iterator.hasNext()) {
            Integer element = iterator.next();
            if (element == key) {
                return true;
            }
        }
        return false;

    }
}

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet obj = new MyHashSet();
 * obj.add(key);
 * obj.remove(key);
 * boolean param_3 = obj.contains(key);
 */
//leetcode submit region end(Prohibit modification and deletion)

```



### 43. [1805. 字符串中不同整数的数目](https://leetcode-cn.com/problems/number-of-different-integers-in-a-string/)

```java
class Solution {
    public int numDifferentIntegers(String word) {
        Set<Long> s = new HashSet<>();
        for (int i=0; i<word.length(); i++){
            char c = word.charAt(i);
            if (Character.isDigit(c)){   // 判断是否是数字，需要用到Character类， 而不是字符自带的方法
                long num = c - '0';     // 不能用int(强制转型)，那样给到的ACSII，不是实际数字
                int j= i+1;
                while (j<word.length() && Character.isDigit(word.charAt(j))){
                    num = num *10 + word.charAt(j) - '0';
                    j ++;
                }
                i = j - 1;
                s.add(num);
            }
        }
//        for (long r:s){
//            System.out.println(r);
//        }
        return s.size();

    }
}
```



### 44 [231. 2 的幂](https://leetcode-cn.com/problems/power-of-two/)

```java
class Solution {
    private static final int big = 1<<30;
    public boolean isPowerOfTwo(int n) {
        return n> 0 && big %n ==0;

    }
}

// low bit 
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n&(n-1)) == 0;

    }
}
```



### 45 [5772. 检查某单词是否等于两单词之和](https://leetcode-cn.com/problems/check-if-word-equals-summation-of-two-words/)

```java
class Solution {
    public boolean isSumEqual(String firstWord, String secondWord, String targetWord) {

        return get(firstWord) + get(secondWord) == get(targetWord);
    }
    public int get(String a){
        int res  = 0;
        char[] b = a.toCharArray();   // String -- > char[]
        //System.out.println(b);
        for (char c: b){

            System.out.println(c -'a');
            res = res *10 + c -'a';
        }
       // System.out.println(res);
        return res;
    }
}
```

### 46 [5754. 长度为三且各字符不同的子字符串](https://leetcode-cn.com/problems/substrings-of-size-three-with-distinct-characters/)

```java
class Solution {
    // 用了三个变量轮转，还是有点复杂了，直接i，i+1， i+2.就OK的
    public int countGoodSubstrings(String s) {
        if (s.length() <3){
            return 0;
        }
        char x = s.charAt(0), y = s.charAt(1),z = s.charAt(2);
        int  i = 3;
        int res =check(x,y,z)? 1:0;
        for (;i<s.length();i++){
            x = y;
            y = z;
            z = s.charAt(i);
            if (check(x,y,z)){
                res +=1;
            }
        }
        return res;

    }
    public boolean check(char a, char b ,char c){
        return a != b && a !=c  && b !=c;
    }
}
```

### 47. [492. 构造矩形](https://leetcode-cn.com/problems/construct-the-rectangle/)

```java
class Solution {
    public int[] constructRectangle(int area) {
        int width = (int) Math.sqrt(area);
        int length;
        while (area % width != 0) {
            width--;
        }
        length = area / width;
        return new int[]{length, width};
    }
}
// 没啥意思，就是先开根号，然后先确定宽，再确定长
class Solution {
    public int[] constructRectangle(int area) {
        int width = (int) Math.sqrt(area);
        while( area % width != 0) width--;
        return new int[]{area / width, width};
    }
}




```

### 48. [453. 最小操作次数使数组元素相等](https://leetcode-cn.com/problems/minimum-moves-to-equal-array-elements/)

```java
public class Solution {
    public int minMoves(int[] nums) {
        // 先排序，然后确定每个元素之间的gap， 累计求和
        Arrays.sort(nums);
        int count = 0;
        for (int i = nums.length - 1; i > 0; i--) {
            count += nums[i] - nums[0];
        }
        return count;
    }
}

/*
该方法基于以下思路：将除了一个元素之外的全部元素+1，等价于将该元素-1，因为我们只对元素的相对大小感兴趣。因此，该问题简化为需要进行的减法次数。

显然，我们只需要将所有的数都减到最小的数即可。为了找到答案，我们不需要真的操作这些元素。只需要 moves=sum(nums) - min(nums) *length

本质上，理解成一个消消乐，超出min的都要消掉，每次操作，只能够消掉一个“1”， 所以求和就好*/
public class Solution {
    public int minMoves(int[] nums) {
        int moves = 0, min = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            moves += nums[i];
            min = Math.min(min, nums[i]);
        }
        return moves - min * nums.length;
    }
}

```

### 49 [1886. 判断矩阵经轮转后是否一致](https://leetcode-cn.com/problems/determine-whether-matrix-can-be-obtained-by-rotation/)

```java
// 简单题未必有巧妙解法，也可能仅仅只是数据量很小，所以解法很直
// 主要是看deepequal（）
class Solution {
    //顺时针旋转90°==先水平转矩阵，再按照对角线反转矩阵
    public boolean findRotation(int[][] mat, int[][] target) {
        if(Arrays.deepEquals(mat,target)) return true;  // 不旋转的情况是否相等
        // 顺时针旋转360°等于没旋转，所以最多旋转三次，如果不相等直接返回false
        int index = 3;
        while(index-->0){
            reverseMatrix(mat); 
            // deepEquals深比较
            if(Arrays.deepEquals(mat,target)) return true;
        }
        return false;
    }

    public void reverseMatrix(int[][] martrix){  // 旋转matrix90°
        int m = martrix.length,n = martrix[0].length;
        // 先水平转矩阵
        for(int i=0;i<m/2;i++){
            for(int j=0;j<n;j++){
                int temp = martrix[i][j];
                martrix[i][j] = martrix[m-i-1][j];
                martrix[m-i-1][j] = temp;
            }
        }
        // 按照对角线反转矩阵
        for(int i=0;i<m;i++){
            for(int j=i+1;j<n;j++){
                int temp = martrix[i][j];
                martrix[i][j] = martrix[j][i];
                martrix[j][i] = temp;
            }
        }
    }
}
```

### 50 [5780. 删除一个元素使数组严格递增](https://leetcode-cn.com/problems/remove-one-element-to-make-the-array-strictly-increasing/)

```java
class Solution {
        public boolean canBeIncreasing(int[] nums) {
        int n = nums.length;
        if (n <2){
            return true;
        }
        int cnt = 0;
        if(check(nums)){
            return true;
        }
        for(int i =0;i<n;i++){
            int[] tmp = new int[n-1];
            int k =0;
            for(int j=0;j<n;j++){
                if(i ==j){
                    continue;
                }else{
                    tmp[k++] = nums[j];
                }
            }
            if(check(tmp)){
                return true;
            }
        }
        return false;


    }
    private boolean check(int[] nums){
        for(int i=0;i<nums.length -1;i++){
            if (nums[i]>=nums[i+1]){
                return false;
            }
        }
        return true;
    }
}

///
class Solution {
    public boolean canBeIncreasing(int[] nums) {
        // 枚举比较，暴力解决
        for(int i= -1; i<nums.length;i++){ // -1开始，可以表示全部数据
            boolean flag = false;
            int last = 0;
            for(int j=0;j<nums.length;j++){
                // 自己相比，跳过
                if(j==i){
                    continue;
                }
                if(nums[j] <= last){
                    flag = true;
                }
                // 每次更新last，实际上就是两个窗口的移动
                last = nums[j];
            }
            // 任何一次ok，直接返回true
            if(!flag){
                return true;
            }
        }
        return false;

    }
}
```

### 51 [168. Excel表列名称](https://leetcode-cn.com/problems/excel-sheet-column-title/)

```java
class Solution {
    public String convertToTitle(int columnNumber) {
        StringBuffer sb = new StringBuffer();
        while (columnNumber != 0) {
            columnNumber--;
            sb.append((char)(columnNumber % 26 + 'A'));
            columnNumber /= 26;
        }
        return sb.reverse().toString();
    }
}


```

### 52.[1863. 找出所有子集的异或总和再求和](https://leetcode-cn.com/problems/sum-of-all-subset-xor-totals/)

```java
class Solution {
    int res = 0;
    public int subsetXORSum(int[] nums) {
        if(nums.length == 1)return nums[0];
        dfs(nums,0,0);
        return res;
    }
    //i：表示来到第i个位置
    public void dfs(int []nums,int i ,int xor_sum){
        if(i == nums.length){
            res+= xor_sum;
            return;
        }
        //当前位置要
        dfs(nums,i+1,xor_sum ^ nums[i]);
        //当前位置不要
        dfs(nums,i+1,xor_sum);
    }
}

```

### 52.[482. 密钥格式化](https://leetcode-cn.com/problems/license-key-formatting/)

```java
class Solution {
    public String licenseKeyFormatting(String s, int k) {
        String ns = s.replace("-","");
        int n = ns.length();
        int remain = n % k;
        int cnt = n /k;
        if (cnt == 0){
            return ns;
        }
        StringBuilder sb = new StringBuilder();
        int idx = n-1;
        int sum = 0;
        while(idx>=0){
            char tmp = ns.charAt(idx);
            sb.append(tmp);
            sum++;
            if(sum % k ==0 && idx !=0){
                sb.append("-");
            }
            idx--;
        }
        return sb.reverse().toString().toUpperCase();

    }
}
```

### 53. [495. 提莫攻击](https://leetcode-cn.com/problems/teemo-attacking/)

```java
class Solution {
    public int findPoisonedDuration(int[] timeSeries, int duration) {
        Arrays.sort(timeSeries);
        int res= 0;
        int last = 0;
        for(int i=0;i<timeSeries.length;i++){
            if(i == 0){
                last += duration -1 + timeSeries[0];
                res += duration;
            }else{
                if(timeSeries[i]>last){
                    last = timeSeries[i] + duration -1;
                    res += duration;
                }else{
                    int pre = last;
                    last = timeSeries[i] + duration -1;
                    res += last - pre;
                }
            }

        }
        return res;

    }
}
```

### 54. [1779. 找到最近的有相同 X 或 Y 坐标的点](https://leetcode-cn.com/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate/)

```java
class Solution {
    public int nearestValidPoint(int x, int y, int[][] points) {
        int min = 100000;
        int idx = -1;
        for(int i = 0;i<points.length;i++){
            if(points[i][0] == x || points[i][1] == y){
                int tmp = Math.abs(x  - points[i][0]) + Math.abs(y  - points[i][1]);
                if(tmp < min){
                    idx = i;
                    min = tmp;
                }

            }
        }
        return idx == -1? -1: idx;

    }
}
```

### 55. [1784. 检查二进制字符串字段](https://leetcode-cn.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones/)

```java
class Solution {
    public boolean checkOnesSegment(String s) {
        String tmp = s.replace("0", " ");
        String[] res = tmp.split(" ");
        int cnt = 0;
        for(String t : res){
            t =t.trim();
            if(t.length() >0){
                cnt++;
            }
            if (cnt >1){
                return false;
            }
        }
        return true;

    }
}


/// 不含前导0，所以一定是以1开头，绝不能存在01
class Solution {
    public boolean checkOnesSegment(String s) {
        return !s.contains("01");
    }
}

作者：jalonjia
链接：https://leetcode-cn.com/problems/check-if-binary-string-has-at-most-one-segment-of-ones/solution/ji-ran-da-jia-du-yi-xing-dai-ma-na-javay-jpoc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 56. [504. 七进制数](https://leetcode-cn.com/problems/base-7/)

```java
class Solution {
    public String convertToBase7(int num) {
        boolean f= false;

        if(num<0){
            num = -1 * num;
            f = !f;
        }
        StringBuilder sb = new StringBuilder();
        while(num>=0){
            int tmp = num%7;
            sb.append(tmp);
            num = (num - tmp)/7;
            if(num ==0){
                break;
            }
        }
        return f == true ? "-" + sb.reverse().toString(): sb.reverse().toString();

    }
}
```

### 58. [507. 完美数](https://leetcode-cn.com/problems/perfect-number/)

```java
class Solution {
    public boolean checkPerfectNumber(int num) {
        if(num == 1) {
            return false;
        }
        int sum = 1; // 正整数一定会有一个1，同时不用考虑自身，所以单独处理
        int i = 2;
        double sqrt = Math.sqrt(num);
        for(;i < sqrt;i++) {
            if(num % i == 0) {
                sum += i;
                sum += num / i;
            }
        }
        // 此处单独处理的原因在于只需要加1次i值，如果在循环中会加2次
        if(i * i == num) {
            sum += i;
        }
        return sum == num;
    }
}


```

### 59 .[506. 相对名次](https://leetcode-cn.com/problems/relative-ranks/)

```java
class Solution {
    public String[] findRelativeRanks(int[] score) {
        int n = score.length;

        Map<Integer, Integer> map = new HashMap();
        for(int i=0;i<score.length;i++){
            map.put(score[i], i);
            score[i] = -score[i];
        }
        Arrays.sort(score);
        // for(int i=0;i<score.length;i++){
        //     System.out.println(i +" "+ score[i] + " " +map.get(-score[i]));
        // }
        String[] res= new String[score.length];
        for(int i=0;i < score.length;i++){
            int idx = map.get(-score[i]);
            res[idx] = String.valueOf(i +1);
        }
        
        res[map.get(-score[0])] = "Gold Medal";
        if(n==1){
            return res;
        }
        res[map.get(-score[1])] = "Silver Medal";
        if(n==2){
            return res;
        }
        res[map.get(-score[2])] = "Bronze Medal";
        return res;


    }
}
```

### 60. [806. 写字符串需要的行数](https://leetcode-cn.com/problems/number-of-lines-to-write-string/)

```java
class Solution {
    public int[] numberOfLines(int[] widths, String s) {
        int row = 1;
        int preLong = 0;
        int n = s.length();
        int i =0;
        int len = 0;
        while(i<n){
            int idx = s.charAt(i) -'a';
            if(widths[idx] + len < 100){
                len += widths[idx];
            } else if(widths[idx] + len == 100){
                preLong = 100;
                len = 0;
                row++;
            }else{
                preLong = len;
                len = widths[idx];
                row++;
            }
            i++;

        }
        return new int[]{len==0?row-1:row, len==0?preLong:len};

    }
}
```

### 61.[812. 最大三角形面积](https://leetcode-cn.com/problems/largest-triangle-area/)

```java
class Solution {
    public double largestTriangleArea(int[][] points) {
        // …… 就硬是暴力，特点在于鞋带公式计算三角形面积
        int N = points.length;
        double ans = 0;
        for (int i = 0; i < N; ++i)
            for (int j = i+1; j < N; ++j)
                for (int k = j+1; k < N; ++k)
                    ans = Math.max(ans, area(points[i], points[j], points[k]));
        return ans;
    }

    public double area(int[] P, int[] Q, int[] R) {
        return 0.5 * Math.abs(P[0]*Q[1] + Q[0]*R[1] + R[0]*P[1]
                             -P[1]*Q[0] - Q[1]*R[0] - R[1]*P[0]);
    }
}


```

### 62.[824. 山羊拉丁文](https://leetcode-cn.com/problems/goat-latin/)

```java
class Solution {
    public String toGoatLatin(String S) {
        Set<Character> vowel = new HashSet();
        for (char c: new char[]{'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'})
            vowel.add(c);

        int t = 1;
        StringBuilder ans = new StringBuilder();
        for (String word: S.split(" ")) {
            char first = word.charAt(0);
            if (vowel.contains(first)) {
                ans.append(word);
            } else {
                ans.append(word.substring(1));
                ans.append(word.substring(0, 1));
            }
            ans.append("ma");
            for (int i = 0; i < t; i++)
                ans.append("a");
            t++;
            ans.append(" ");
        }

        ans.deleteCharAt(ans.length() - 1);
        return ans.toString();
    }
}

```

### 63.[868. 二进制间距](https://leetcode-cn.com/problems/binary-gap/)

```java
class Solution {
    public int binaryGap(int N) {
        int last = -1, ans = 0;
        for (int i = 0; i < 32; ++i)
            if (((N >> i) & 1) > 0) {
                if (last >= 0)
                    ans = Math.max(ans, i - last);
                last = i;
            }

        return ans;
    }
}


```

### 64.[1827. 最少操作使数组递增](https://leetcode-cn.com/problems/minimum-operations-to-make-the-array-increasing/)

```java
 public int minOperations(int[] nums) {
     /**
     一开始想法错了，这里的只要求严格递增，不要求一定大于一，所以路线错了
     */
        int ret = 0, max = nums[0];
        for (int i = 1; i < nums.length; i++) {
            max = nums[i] > max ? nums[i] : ++max;
            ret += max - nums[i];
        }
        return ret;
    }


```

### 65.[884. 两句话中的不常见单词](https://leetcode-cn.com/problems/uncommon-words-from-two-sentences/)

```java
class Solution {
    public String[] uncommonFromSentences(String A, String B) {
        Map<String, Integer> count = new HashMap();
        for (String word: A.split(" "))
            count.put(word, count.getOrDefault(word, 0) + 1);
        for (String word: B.split(" "))
            count.put(word, count.getOrDefault(word, 0) + 1);

        List<String> ans = new LinkedList();
        for (String word: count.keySet())
            if (count.get(word) == 1)
                ans.add(word);

        return ans.toArray(new String[ans.size()]);
    }
}

```

### 66.[874. 模拟行走机器人](https://leetcode-cn.com/problems/walking-robot-simulation/)

```java
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        // 不要想着很难，实际上就是最简单的暴力
        int[] dir_x = {0, 1, 0, -1};
        int[] dir_y = {1, 0, -1, 0};
        int x = 0;
        int y = 0;
        //0,1,2,3分别代表北、东、南、西方向，初始为正北方；
        int status = 0;
        int max_distance = 0;
        //判断障碍物：将障碍物的x和y坐标组合成一个字符串用set保存障碍物，查找的时候只要判断当前坐标组成的串是否在set里即可。
        HashSet<String> blockSet = new HashSet<>();
        for (int i = 0; i < obstacles.length; i++) {
            blockSet.add(obstacles[i][0] + "," + obstacles[i][1]);
        }
        for (int i = 0; i < commands.length; i++) {
            if (commands[i] == -1){
                status += 1;
            }
            if (commands[i] == -2){
                status += 3;
            }
            if (commands[i] > 0){
                for (int j = 0; j < commands[i]; j++) {
                    int next_x = x + dir_x[status % 4]; // 方向要取余的
                    int next_y = y + dir_y[status % 4];
                    if (blockSet.contains(next_x + "," + next_y)){
                        break;
                    }else {
                        x = next_x;
                        y = next_y;
                        max_distance = Math.max(max_distance, x*x+y*y);
                    }
                }
            }
        }
        return max_distance;
    }
}
```

### 67.[5792. 统计平方和三元组的数目](https://leetcode-cn.com/problems/count-square-sum-triples/)

```java
class Solution {
       public  int countTriples(int n) {
        int res = 0;
        System.out.println("l");
        for(int i=1;i<=n;i++) {
            for (int j = i ; j <= n; j++) {
                for (int k = j+1 ; k <= n; k++) {
//                    System.out.println("i:" + i + "j:" + j + " K:"+k);
                    if (i * i + j * j == k * k) {
                        if (i == j) {
                            res += 1;
                        } else {
                            res += 2;
                        }
                    }
                }

            }
        }
        return res;

    }
}
```

### 68.[1844. 将所有数字用字符替换](https://leetcode-cn.com/problems/replace-all-digits-with-characters/)

```java
class Solution {
    public String replaceDigits(String s) {
        char[] cs = s.toCharArray();
        StringBuilder sb  =new StringBuilder();
        for(int i=0;i<cs.length;i++){
            if(Character.isDigit(cs[i])){
                sb.append((char)(cs[i-1] + (cs[i] - '0')));
            }else{
                sb.append(cs[i]);
            }
        }
        return sb.toString();

    }
}
```

### 69.[1848. 到目标元素的最小距离](https://leetcode-cn.com/problems/minimum-distance-to-the-target-element/)

```java
class Solution {
    public int getMinDistance(int[] nums, int target, int start) {
        if (nums[start] == target) return 0;
        int min_value = 10001;
        for(int i=0;i<nums.length;i++){
            if(nums[i]==target) {min_value = Math.min(Math.abs(start-i),min_value);}
        }
        return min_value;
    }
}


```

### 70.[5817. 判断字符串是否可分解为值均等的子串](https://leetcode-cn.com/problems/check-if-a-string-is-decomposble-to-value-equal-substrings/)

```java
class Solution {
    public boolean isDecomposable(String s) {
        s += 'a';
        char[] ss = s.toCharArray();
        int count = 1, num = 0;
        for (int i = 0; i < ss.length - 1; i++) {
            if (ss[i] == ss[i + 1]) count++;
            else {
                if (count % 3 == 1) return false;
                else if (count % 3 == 2) num++;
                count = 1;
                if (num > 1) return false;
            }
        }
        return num == 1;
    }
}
```

### 71.[LCP 33. 蓄水](https://leetcode-cn.com/problems/o8SXZn/)

```java
class Solution {
    public int storeWater(int[] bucket, int[] vat) {
        /**
        真就是暴力 */
        int max = 0;
        for(int v:vat)
            if(max < v) max = v;
        if(max == 0) return 0;
        int n = bucket.length;
        int ans = Integer.MAX_VALUE;
        for(int i = 1; i <= 10000; i++) {//遍历倒水次数
            int per = 0;
            int cur = i;//倒水i次，所以操作次数+i
            for(int j = 0; j < n; j++) {//遍历每个水缸
                per = (vat[j] + i - 1) / i;// 水槽容量/倒水次数=每次倒水量
//+（i - 1）目的是为了向上取整(除完后如果有余数，加上i-1之后就一定会多商1，从而达到向上取整的功能)
//使用vat[j]%i==0 ? vat[j]/i : vat[j]/i+1 代替也行，但是更慢
                cur += Math.max(0, per - bucket[j]);// 每次倒水量-初始水量=需要升级次数
            }
            ans = Math.min(ans, cur);//所有倒水次数中，取最小的操作次数
        }
        return ans;
    }
}


```

### 72.[929. 独特的电子邮件地址](https://leetcode-cn.com/problems/unique-email-addresses/)

```java
class Solution {
    public int numUniqueEmails(String[] emails) {
        Set<String> res = new HashSet<>();
        for(String s:emails){
            int end = s.indexOf("@");
            String t = s.substring(0,end);
       t = t.replace(".", "");
            int idx = t.indexOf("+");
            String final_ = t + s.substring(end);
            if(idx != -1){
                final_ = t.substring(0, idx) + s.substring(end);
            }
            res.add(final_);
        }
        // System.out.println(res);
        return res.size();
    }

}
```

### 73 .[LCP 28. 采购方案](https://leetcode-cn.com/problems/4xy4Wx/)

```java
class Solution {
    public int purchasePlans(int[] nums, int target) {
        // 先左不动，右动， 再计算完毕后，左再动，是双指针，不过……
        int mod = 1_000_000_007;
        int ans = 0;
        // 首先对整体进行排序
        Arrays.sort(nums);
        // 双指针，left 从前往后找，right 从后往前
        int left = 0, right = nums.length - 1;
        while (left < right) {
            
            // 如果当前左右之和大于了目标值，说明偏大了，就把右指针往左移动
            if (nums[left] + nums[right] > target) right--;
            else {
                // 否则的话，说明找到了合适的，需要把两者中间的元素个数都累加起来
                ans += right - left;
                // 然后再向右移动左指针
                left++;
            }
            ans %= mod;
        }
        return ans % mod;
    }
}


```

### 74 .[1033. 移动石子直到连续](https://leetcode-cn.com/problems/moving-stones-until-consecutive/)

```java
class Solution {
    public int[] numMovesStones(int a, int b, int c) {
        int[] t = new int[]{a, b, c};
        Arrays.sort(t);
        int[] res = new int[2];
        if(t[0]+1==t[1] && t[1] + 1==t[2]){
            res[0] = 0;
            res[1] = 0;
        }else if(t[2]<=t[0]){
            return res;
        }else if(t[0]+1==t[1] || t[1] + 1==t[2] || t[0] +2==t[1] || t[1] + 2 ==t[2] ){
            res[0] = 1;
            res[1] = t[2] -t[0] -2;
        }else{
            res[0] = 2;
            res[1] =  t[2]-t[0]-2;
        }
        return res;

    }
}
```

### 75. [953. 验证外星语词典](https://leetcode-cn.com/problems/verifying-an-alien-dictionary/)

```java
class Solution {
    public boolean isAlienSorted(String[] words, String order) {
// 实际上就是两两比较，只是按照新的顺序来, 注意比较的是新的顺序
        int[] o = new int[26];
        // 除了数组，也可以使用map
        for(int i=0;i<order.length();i++){
            o[order.charAt(i) - 'a'] =i;
        }
        for(int i=0;i<words.length-1;i++){
            boolean flag = false;
            for(int j=-0; j < Math.min(words[i].length(), words[i+1].length()) ; j++){
                if(o[words[i].charAt(j) -'a']==  o[words[i+1].charAt(j)-'a']){
                    continue;
                }
                else if (o[words[i].charAt(j) -'a'] >  o[words[i+1].charAt(j)-'a']){
                    return false;
                }else{
                    flag = true;
                    break;
                }
            }
            // System.out.println(flag);
            if (!flag && words[i].length() > words[i+1].length()){
                return false;
            }
        }
        return true;
     
    }
}
```

### 76.[937. 重新排列日志文件](https://leetcode-cn.com/problems/reorder-data-in-log-files/)

```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        // 就是单纯排序
        Arrays.sort(logs, (log1, log2) -> {
            // 将字符串分成两份，一份标识，一份内容
            String[] split1 = log1.split(" ", 2);
            String[] split2 = log2.split(" ", 2);
            // 分割后，比较标识后的第一位
            boolean isDigit1 = Character.isDigit(split1[1].charAt(0));
            boolean isDigit2 = Character.isDigit(split2[1].charAt(0));
            // 如果都是字母日志，就是按照如果内容不同，直接排序，相同，按照标识排序
            if (!isDigit1 && !isDigit2) {
                int cmp = split1[1].compareTo(split2[1]);
                if (cmp != 0) return cmp;
                return split1[0].compareTo(split2[0]);
            }
            // 如果一个字母，一个数字，字母在前
            return isDigit1 ? (isDigit2 ? 0 : 1) : -1;
        });
        return logs;
    }
}

```

### 77 .[1005. K 次取反后最大化的数组和](https://leetcode-cn.com/problems/maximize-sum-of-array-after-k-negations/)

```java
class Solution {
     public int largestSumAfterKNegations(int[] A, int K) {
        int[] number = new int[201];//-100 <= A[i] <= 100,这个范围的大小是201
        for (int t : A) {
            number[t + 100]++;//将[-100,100]映射到[0,200]上
        }
        int i = 0;
        while (K > 0) {
            while (number[i] == 0)//找到A[]中最小的数字
                i++;
            number[i]--;//此数字个数-1
            number[200 - i]++;//其相反数个数+1
            if (i > 100) {//若原最小数索引>100,则新的最小数索引应为200-i.(索引即number[]数组的下标)
                i = 200 - i;
            }
            K--;
        }
        int sum = 0;
        for (int j = i; j <number.length ; j++) {//遍历number[]求和
            sum += (j-100)*number[j];//j-100是数字大小,number[j]是该数字出现次数.
        }
        return sum;
    }
}
```

### 78.[993. 二叉树的堂兄弟节点](https://leetcode-cn.com/problems/cousins-in-binary-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public boolean isCousins(TreeNode root, int x, int y) {
        int x_pos= 0,  y_pos = 0;
        TreeNode x_parent = null;
        TreeNode y_parent = null;
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        int level = 0;
        while(!q.isEmpty()){
            int n = q.size();
            level++;
            for(int i=0;i<n;i++){
                TreeNode t = q.poll();
                if(t.left != null){
                    if(t.left.val == x){
                        x_parent = t;
                        x_pos = level;
                    }
                    if(t.left.val == y){
                        y_parent = t;
                        y_pos = level;
                    }
                    q.offer(t.left);
                }

                 if(t.right != null){
                    if(t.right.val == x){
                        x_parent = t;
                        x_pos = level;
                    }
                    if(t.right.val == y){
                        y_parent = t;
                        y_pos = level;
                    }
                    q.offer(t.right);
                }
            }
        }
        return x_pos == y_pos && x_parent != y_parent;

    }
}
```

### 79 [1827. 最少操作使数组递增](https://leetcode-cn.com/problems/minimum-operations-to-make-the-array-increasing/)

```java
class Solution {
   public int minOperations(int[] nums) {
        int ret = 0, max = nums[0];
        for (int i = 1; i < nums.length; i++) {
            max = nums[i] > max ? nums[i] : ++max;
            ret += max - nums[i];
        }
        return ret;
    }
}
```

### 80 [LCP 29. 乐团站位](https://leetcode-cn.com/problems/SNJvJP/)

```java
class Solution {
    public int orchestraLayout(int num, int xPos, int yPos) {
        // 先采用剥洋葱的方式，将最外层剥开，然后临界层毕竟不完整，可以按照输顺序，走完、
        // 具体的数字，可以先统计个数，然后取余
        int level = Math.min(Math.min(xPos, yPos), Math.min(num-1 - xPos, num-1 - yPos));
        // System.out.println(num * num);
        // 个数统计外围，应该是a2 -b2， 但是tmd数据溢出，所以（a+b)(a-b) 这样可以提前取余
        // 傻逼数据溢出，需要进一步优化化简
        long s = (num -level) % 9 * (level%9) % 9 * 4;
        // System.out.println("up:"+s);
        int up = 0+level;
        int down = num-level -1;
        int left = 0+level;
        int right = down;
        // System.out.println("up:"+up + "down" + down);
        s = s %9;
        // System.out.println(s);
        if(xPos== up){
            s += yPos - level+1;
            System.out.println("up:"+s);
        }else if(yPos == right){
            s += (right - left) %9 + 1 + xPos - up;
            // System.out.println("right:"+s);
        }else if(xPos == down){

            s +=2 * (right -left)%9 + 1 + right - yPos;
            // System.out.println("up:"+s);
        }else{
            s += 3*(right -left)%9 + 1 + down - xPos;
            // System.out.println("up:"+s);
        }
        // System.out.println(s);
        return (int)s%9 == 0? 9:   (int)s %9;


    }
}
```

### 81 .[138. 复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)

```java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    /**
    使用哈希表加递归的方式，比较暴力，但是写起来简单
    */
    Map<Node, Node> map = new HashMap<>();
    public Node copyRandomList(Node head) {
        if(head == null){
            return head;
        }
        if(!map.containsKey(head)){
            Node headNew = new Node(head.val);
            map.put(head, headNew);
            headNew.next = copyRandomList(head.next);
            headNew.random = copyRandomList(head.random);
        }
        return map.get(head);
    }
}

// 节点拆分加迭代的方式，写起来比较多，但是方法更巧妙
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = new Node(node.val);
            nodeNew.next = node.next;
            node.next = nodeNew;
        }
        for (Node node = head; node != null; node = node.next.next) {
            Node nodeNew = node.next;
            nodeNew.random = (node.random != null) ? node.random.next : null;
        }
        Node headNew = head.next;
        for (Node node = head; node != null; node = node.next) {
            Node nodeNew = node.next;
            node.next = node.next.next;
            nodeNew.next = (nodeNew.next != null) ? nodeNew.next.next : null;
        }
        return headNew;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/copy-list-with-random-pointer/solution/fu-zhi-dai-sui-ji-zhi-zhen-de-lian-biao-rblsf/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 82 .[1736. 替换隐藏数字得到的最晚时间](https://leetcode-cn.com/problems/latest-time-by-replacing-hidden-digits/)

```java
class Solution {
    public String maximumTime(String time) {
        // 1. 状态机模拟清楚就ok 2. 有时候字符串可以切换成字符数组再操作
        char[] arr = time.toCharArray();
        if (arr[0] == '?') {
            arr[0] = ('4' <= arr[1] && arr[1] <= '9') ? '1' : '2';
        }
        if (arr[1] == '?') {
            arr[1] = (arr[0] == '2') ? '3' : '9';
        }
        if (arr[3] == '?') {
            arr[3] = '5';
        }
        if (arr[4] == '?') {
            arr[4] = '9';
        }
        return new String(arr);
    }
}


```

### 83 [5823. 字符串转化后的各位数字之和](https://leetcode-cn.com/problems/sum-of-digits-of-string-after-convert/)

```java
class Solution {
    public int getLucky(String s, int k) {
        String t = "";
        for(int i=0;i<s.length();i++){
            t += s.charAt(i) -'a' + 1;
        }
        int tmp = 0;
        for(int i=0;i<k;i++){
            // System.out.println(tmp + " " + t);
            tmp = 0;
            for(int j=0;j<t.length();j++){
                tmp += t.charAt(j) - '0';
            }
            t = ""+ tmp;

        }
        return tmp;

    }
}
```

### 84 [5804. 检查是否所有字符出现次数相同](https://leetcode-cn.com/problems/check-if-all-characters-have-equal-number-of-occurrences/)

```java
class Solution {
    public boolean areOccurrencesEqual(String s) {
        int[] cnt = new int[26];
        for(int i=0;i<s.length(); i++){
            cnt[s.charAt(i) - 'a']++;
        }
        int t = 0;
        for(int i:cnt){
            if(t == 0 && i!=0){
                t = i;
            }else{
                if(t != i && i!=0){
                    return false;
                }
            }
        }
        return true;

    }
}
```

### 85 [671. 二叉树中第二小的节点](https://leetcode-cn.com/problems/second-minimum-node-in-a-binary-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    int ans;
    int rootvalue;

    public int findSecondMinimumValue(TreeNode root) {
        ans = -1;
        rootvalue = root.val;
        dfs(root);
        return ans;
    }

    public void dfs(TreeNode node) {
        if (node == null) {
            return;
        }
        if (ans != -1 && node.val >= ans) {
            return;
        }
        if (node.val > rootvalue) {
            ans = node.val;
        }
        dfs(node.left);
        dfs(node.right);
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

### 4. [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int n = nums.length;
        int l=0, r=n-1;
        int[] res= {-1,-1};
        if(n ==0){
            return res;
        }
        while(l<r){
            int mid = l + (r-l)/2;
            if(nums[mid]<target){
                l = mid +1;
            }else{
                r =mid;
            }
        }
        if (nums[l] != target){
            return res;
        }
        res[0] = l;
        r =n -1;
        while(l<r){
            int mid = l + (r-l+1)/2;
            if(nums[mid]>target) r = mid -1;
            else l = mid;
        }
        res[1] = r;
        return res;

    }
}
```



### 5. [973. 最接近原点的 K 个点](https://leetcode-cn.com/problems/k-closest-points-to-origin/)

```Java
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        // 堆的用法里面，需要写一个比较器,实现compare 方法
        // 有限队列的offer ,poll ,peek方法
        PriorityQueue<int[]> pq = new PriorityQueue<int[]> (new Comparator<int[]>(){
            public int compare(int[] s1, int[] s2){
                return s2[0]-s1[0];
            }
        });
        for (int i =0 ;i<K;i++){
            pq.offer(new int[]{points[i][0]*points[i][0] + points[i][1]*points[i][1],i});
        }
        int n = points.length;
        for(int i= K;i<n;i++){
            int dist = points[i][0]*points[i][0] + points[i][1]*points[i][1];
            if(dist<pq.peek()[0]){
                pq.poll();
                pq.offer(new int[]{dist,i});
            }
        }
        int[][] res= new int[K][2];
        for(int i=0;i<K;i++){
            res[i] = points[pq.poll()[1]];
        }
        return res;

    }
}



// 快排
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        // 变形的快排
        return quickSelect(points, 0, points.length -1, K-1);
    }
    private int[][] quickSelect(int[][] points, int low, int high,int idx){
        // 快排切分，不过只要找到确定的位置为K-1 ，就可以返回，不用完全排序
        if (low>high){
            return new int[0][0];
        }
        int pos = partition(points,low,high); // 每一轮快排最终确定的位置
        if  (pos == idx){
            // Arrays 包里的方法，需要熟悉
            return Arrays.copyOf(points,idx+1);
        }
        // 如果不是，根据大小，向左向右照
        return pos<idx?quickSelect(points , pos+1 ,high,idx): quickSelect(points,low, pos-1,idx);
    }
    private int partition(int[][] points, int low, int high){
        int[] v = points[low];
        int dist = v[0]*v[0] + v[1]*v[1];
        // 快排起步，右端虚一位
        int i=low,j=high+1;
        while(true){
            // 双指针快排，都有点陌生了
            while(++i <= high && points[i][0]*points[i][0]+points[i][1]*points[i][1] <dist);
            while(--j <= high && points[j][0]*points[j][0]+points[j][1]*points[j][1] >dist);
            if( i >= j){
                break;
            }
            int[] tmp = points[i];
            points[i] = points[j];
            points[j] = tmp;

        }
        points[low] = points[j];
        points[j] = v;
        return j;
    }
}
```



### 6. [378. 有序矩阵中第K小的元素](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix/)

```java
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        // n重topK
        PriorityQueue<int[]> heap = new PriorityQueue<int[]>(new Comparator<int[]>(){
            public int compare(int[] a, int[]b){
                return a[0]-b[0];
            }
        });
        int n = matrix.length;
        for(int i=0; i<n;i++){
            heap.offer(new int[]{matrix[i][0],i,0});
        }
        for(int i=0; i<k-1;i++){
            int[] tmp = heap.poll();
            if(tmp[2] != n-1){
                heap.offer(new int[]{matrix[tmp[1]][tmp[2]+1], tmp[1], tmp[2]+1});
            }
        }
        return heap.poll()[0];

    }
}

class Solution {
    public int kthSmallest(int[][] matrix, int k) {

        // 二分法
        int n = matrix.length;
        int left = matrix[0][0], right = matrix[n-1][n-1];
        while(left<right){
            int mid = left + (right-left)/2;
            // 如果 <= mid的数量大于等于k，说明mid 太大，所以right 缩小。反之，left 增大
            if (check(matrix,mid,k,n)){
                right = mid;
            }
            else
            {
                left = mid+1;
            }
        }
        return left;

    }
    public boolean check(int[][] matrix, int mid ,int k,int n){
        int i = n-1;
        int j= 0;
        int num =0 ;
        while(i>= 0 && j <n){
            if (matrix[i][j]<= mid){
                num += i +1;
                j++;
            }
            else{
                i --;
            }
        }
        return num>= k;
    }
}
```



### 7. [767. 重构字符串](https://leetcode-cn.com/problems/reorganize-string/)

```java
class Solution {
    public String reorganizeString(String S) {
        // 思路是贪心，预设好方法，从次数最多的字符间隔排列
        // 实现是堆排序
        if(S.length()<2){
            return S;
        }
        int[] cnt = new int[26];
        int maxCnt = 0;
        int length = S.length();
        for(int i= 0 ;i<length;i++ ){
            char c = S.charAt(i);
            cnt[c-'a']++;
            maxCnt = Math.max(maxCnt,cnt[c-'a']);
        }
        if (maxCnt>(length+1)/2){
            return "";
        }
        PriorityQueue<Character> heap = new PriorityQueue<Character>(new Comparator<Character>(){
            public int compare(Character c1,Character c2){
                return cnt[c2-'a']-cnt[c1-'a'];
            }
        });
        for(char c= 'a';c<='z';c++){
            if(cnt[c-'a']>0){
                heap.offer(c);
            }
        }
        StringBuilder sb = new StringBuilder();
        while(heap.size()>1){
            char c1 =heap.poll();
            char c2= heap.poll();
            sb.append(c1);
            sb.append(c2);
            cnt[c1-'a']--;
            cnt[c2-'a']--;
            if(cnt[c1-'a']>0){
                heap.offer(c1);
            }
            if(cnt[c2-'a']>0){
                heap.offer(c2);
            }
        }
        if (heap.size()>0){
            sb.append(heap.poll());
        }
        
        return sb.toString();
    }
}
```



### 8. [978. 最长湍流子数组](https://leetcode-cn.com/problems/longest-turbulent-subarray/)

```java
class Solution {
    public int maxTurbulenceSize(int[] arr) {
        int n = arr.length;
        int res= 1;
        int left =0, right = 0;
        while(right<n-1){
            if (left == right){
                if (arr[left] == arr[left+1]){
                    left++;
                }
                right++;
            }else{
                if (arr[right-1]>arr[right] && arr[right]< arr[right+1]){
                    right++;
                }else if(arr[right-1]<arr[right] && arr[right]> arr[right+1]){
                    right++;
                }else{
                    left = right;
                }
            }
            res = Math.max(res, right - left +1);
            //System.out.println(right+""+left);
        }
        return res;
        

    }
}
```

### 9. [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1.charAt(i) - 'a'];
            ++cnt2[s2.charAt(i) - 'a'];
        }
        if (Arrays.equals(cnt1, cnt2)) { // JAVA里的比较久需要有equals方法了
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2.charAt(i) - 'a'];
            --cnt2[s2.charAt(i - n) - 'a'];
            if (Arrays.equals(cnt1, cnt2)) {
                return true;
            }
        }
        return false;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/permutation-in-string/solution/zi-fu-chuan-de-pai-lie-by-leetcode-solut-7k7u/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 10. [1004. 最大连续1的个数 III](https://leetcode-cn.com/problems/max-consecutive-ones-iii/)

```java
class Solution {
    public int longestOnes(int[] A, int K) {
        int n = A.length;
        int left = 0, lsum = 0, rsum = 0;
        int ans = 0;
        for (int right = 0; right < n; ++right) {
            rsum += 1 - A[right];
            while (lsum < rsum - K) {
                lsum += 1 - A[left];
                ++left;
            }
            ans = Math.max(ans, right - left + 1);
        }
        return ans;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/max-consecutive-ones-iii/solution/zui-da-lian-xu-1de-ge-shu-iii-by-leetcod-hw12/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 11. [797. 所有可能的路径](https://leetcode-cn.com/problems/all-paths-from-source-to-target/)

```java
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        return solve(graph, 0);
    }

    public List<List<Integer>> solve(int[][] graph, int node) {
        int N = graph.length;
        List<List<Integer>> ans = new ArrayList();
        if (node == N - 1) {
            List<Integer> path = new ArrayList();
            path.add(N-1);
            ans.add(path);
            return ans;
        }

        for (int nei: graph[node]) {
            for (List<Integer> path: solve(graph, nei)) {
                path.add(0, node);
                ans.add(path);
            }
        }
        return ans;
    }
}

作者：LeetCode
链接：https://leetcode-cn.com/problems/all-paths-from-source-to-target/solution/suo-you-ke-neng-de-lu-jing-by-leetcode/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


### 12. [1438. 绝对差不超过限制的最长连续子数组](https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/)

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        Deque<Integer> queMax = new LinkedList<Integer>();
        Deque<Integer> queMin = new LinkedList<Integer>();

        int n = nums.length;
        int left = 0, right = 0;
        int ret = 0;
        while (right < n) {
            while (!queMax.isEmpty() && queMax.peekLast() < nums[right]) {
                queMax.pollLast();
            }
            while (!queMin.isEmpty() && queMin.peekLast() > nums[right]) {
                queMin.pollLast();
            }
            queMax.offerLast(nums[right]);
            queMin.offerLast(nums[right]);
            while (!queMax.isEmpty() && !queMin.isEmpty() && queMax.peekFirst() - queMin.peekFirst() > limit) {
                if (nums[left] == queMin.peekFirst()) {
                    queMin.pollFirst();
                }
                if (nums[left] == queMax.peekFirst()) {
                    queMax.pollFirst();
                }
                left++;
            }
            ret = Math.max(ret, right - left + 1);
            right++;
        }
        return ret;

    }
}



// 学习有序集合
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        TreeMap<Integer, Integer> map = new TreeMap<Integer, Integer>();
        int n = nums.length;
        int left = 0, right = 0;
        int ret = 0;
        while (right < n) {
            map.put(nums[right], map.getOrDefault(nums[right], 0) + 1);
            while (map.lastKey() - map.firstKey() > limit) {
                map.put(nums[left], map.get(nums[left]) - 1);
                if (map.get(nums[left]) == 0) {
                    map.remove(nums[left]);
                }
                left++;
            }
            ret = Math.max(ret, right - left + 1);
            right++;
        }
        return ret;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/solution/jue-dui-chai-bu-chao-guo-xian-zhi-de-zui-5bki/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 13. [1043. 分隔数组以得到最大和](https://leetcode-cn.com/problems/partition-array-for-maximum-sum/)

```java
class Solution {
    public int maxSumAfterPartitioning(int[] arr, int k) {
        int n = arr.length;
        int[] dp = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            int max = -1;
            for (int j = i - 1; j >= Math.max(i - k, 0); j--) {
                max = Math.max(max, arr[j]);
                dp[i] = Math.max(dp[i], dp[j] + max * (i - j));
            }
        }
        return dp[n];
    }
}
```

### 14. [1770. 执行乘法运算的最大分数](https://leetcode-cn.com/problems/maximum-score-from-performing-multiplication-operations/)

```java
class Solution {
    public int maximumScore(int[] nums, int[] multipliers) {
         int n = nums.length, m = multipliers.length;
        if(n> 2*m){
            int x =m, y = n-m;
            while (y<n){
                nums[x++] = nums[y++];
            }
            n = x;
        }
        int[][] dp = new int[n+2][n+2];
        for(int len = n-m+1; len<=n;len ++){
            for(int i=1;i+ len-1<=n;i++){
                int j= i+len-1;
                dp[i][j] = Math.max(dp[i+1][j] + nums[i-1] * multipliers[n-len] , dp[i][j-1] + nums[j-1]*multipliers[n-len]);
            }
        }
        return dp[1][n];

    }
}
```

### 15. [1052. 爱生气的书店老板](https://leetcode-cn.com/problems/grumpy-bookstore-owner/)

```java
class Solution {
    public int maxSatisfied(int[] customers, int[] grumpy, int X) {
int n = customers.length;
        int[] pre1 = new int[n+1];
        int[] pre2 = new int[n+1];
        for(int i=1;i<=n; i++){
            pre1[i] = pre1[i-1] + customers[i-1];
            pre2[i] = pre2[i-1] + customers[i-1] * (1 - grumpy[i-1]);

        }
        int res = pre2[n];
        int sum = res;
        for(int i=0;i +X <=n;i++){
            int tmp = sum + (pre1[i+X] - pre1[i]) - (pre2[i+X] - pre2[i]);
            res = Math.max(tmp,res);
        }
        return res;
    }
}
```

### 16. [395. 至少有 K 个重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/)

```java

```

### 17. [227. 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

```java
package practice;

import java.util.*;

public class _5668_so {
    public static void main(String[] args) {
        Solution a = new Solution();
        String s = " 3/2";
        System.out.println(a.calculate(s));
        System.out.println(3/2);
    }
}
class Solution {
    public int calculate(String s) {
        Deque<Integer> stk = new LinkedList<>();
        int num=0;
        char sign = '+';
        for(int i=0;i<s.length();i++){
            if (Character.isDigit(s.charAt(i))){
                num = num*10 + s.charAt(i) - '0';
            }
            if(!Character.isDigit(s.charAt(i))  && s.charAt(i) != ' '|| i == s.length()-1){
                if( sign == '+'){
                    stk.add(num);
                }
                else if(sign == '-'){
                    stk.add(0-num);
                }
                else  if(sign == '*'){
                    stk.add(stk.pollLast() * num);
                }
                else{
                    stk.add(stk.pollLast()/num);
                }
                num = 0;
                sign = s.charAt(i);
            }
        }
        int res = 0;
        while (!stk.isEmpty()){
            int t = stk.pop();
            res += t;
        }
        return res;
    }
}

```

### 18.  [131. 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning/)

```java
//给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 回文串 。返回 s 所有可能的分割方案。 
//
// 回文串 是正着读和反着读都一样的字符串。 
//
// 
//
// 示例 1： 
//
// 
//输入：s = "aab"
//输出：[["a","a","b"],["aa","b"]]
// 
//
// 示例 2： 
//
// 
//输入：s = "a"
//输出：[["a"]]
// 
//
// 
//
// 提示： 
//
// 
// 1 <= s.length <= 16 
// s 仅由小写英文字母组成 
// 
// Related Topics 深度优先搜索 动态规划 回溯算法 
// 👍 638 👎 0


import java.util.ArrayList;
import java.util.List;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    List<List<String>> res = new ArrayList<List<String>>();
    public List<List<String>> partition(String s) {
        if (s == null || s.length() <2){
            List<String> tmp = new ArrayList<>();
            tmp.add(s);
            res.add(tmp);
            return res;
        }
        int n = s.length();
        dfs(s,new ArrayList<>(),0);
        return res;

    }

    public void dfs(String s, List<String> tmp, int idx){
        if (idx>= s.length()){
            res.add(tmp);
            return;
        }
        for (int i=idx;i<s.length();i++){
            String t = s.substring(idx,i+1);
            if( t.equals(new StringBuffer(t).reverse().toString())){
                // Java没有合适的字符串翻转函数，只能这么完：new StringBuffer(t).reverse().toString()
                // 列表复制： new ArrayList<String>(tmp)
                tmp.add(t);
                dfs(s,new ArrayList<String>(tmp), i+1);// 递归回溯，传值不传地址，否则加到res中的是同一个，回溯完毕都是空
                tmp.remove(tmp.size() -1);
            }
        }
    }
}
//leetcode submit region end(Prohibit modification and deletion)
// 不断递归回溯，有许多重复计算，可以考虑先一步计算结果，
class Solution {
    int[][] f;
    List<List<String>> res = new ArrayList<List<String>>();
    List<String> ans = new ArrayList<String>();
    int n;
    public List<List<String>> partition(String s) {
        n = s.length();
        f = new int[n][n];

        dfs(s, 0);
        System.out.println("================");
        for (int[] row:f
        ) {
            System.out.println(Arrays.toString(row));

        }
        return res;
    }


    public void dfs(String s, int i){
        if(i == n){
            res.add(new ArrayList<String>(ans));
            return;
        }
        for(int j=i;j<n;j++){
            if (isPalindrome(s,i,j) ==1){
                ans.add(s.substring(i,j+1));
                dfs(s,j+1);
                ans.remove(ans.size()-1);
            }
        }
    }



    public int isPalindrome(String s, int i, int j){
        System.out.printf("i:%d , j: %d \n",i,j);
        // 使用dp，并放在记忆化搜索里面，节省以后比较的时间
        if (f[i][j] != 0){  // 已经计算过，直接返回就好
            return f[i][j];
        }
        if(i>=j){  // 一般情况下是j >i ,如果i >=j 其实就是 i==j dp =1,之所以出现就 j<i 的情况在于下面这里 i+1,j-1
            f[i][j] = 1;
        }else if (s.charAt(i) == s.charAt(j)){
            f[i][j] = isPalindrome(s,i+1,j-1);  // 外部相等，判断内部
        }else{
            f[i][j] = -1;  //否则为-1
        }
        return f[i][j];
    }

}
```

### 19. [331. 验证二叉树的前序序列化](https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/)

```Java
//序列化二叉树的一种方法是使用前序遍历。当我们遇到一个非空节点时，我们可以记录下这个节点的值。如果它是一个空节点，我们可以使用一个标记值记录，例如 #。 
//
//      _9_
//    /   \
//   3     2
//  / \   / \
// 4   1  #  6
/// \ / \   / \
//# # # #   # #
// 
//
// 例如，上面的二叉树可以被序列化为字符串 "9,3,4,#,#,1,#,#,2,#,6,#,#"，其中 # 代表一个空节点。 
//
// 给定一串以逗号分隔的序列，验证它是否是正确的二叉树的前序序列化。编写一个在不重构树的条件下的可行算法。 
//
// 每个以逗号分隔的字符或为一个整数或为一个表示 null 指针的 '#' 。 
//
// 你可以认为输入格式总是有效的，例如它永远不会包含两个连续的逗号，比如 "1,,3" 。 
//
// 示例 1: 
//
// 输入: "9,3,4,#,#,1,#,#,2,#,6,#,#"
//输出: true 
//
// 示例 2: 
//
// 输入: "1,#"
//输出: false
// 
//
// 示例 3: 
//
// 输入: "9,#,#,1"
//输出: false 
// Related Topics 栈 
// 👍 213 👎 0


//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public boolean isValidSerialization(String preorder) {
        int n = preorder.length();
        int i = 0;
        Deque<Integer> stack = new LinkedList<Integer>();//当使用栈的时候，使用deque貌似会更好
        stack.push(1);
        while (i < n) {
            if (stack.isEmpty()) {
                return false;
            }
            if (preorder.charAt(i) == ',') {
                i++;
            } else if (preorder.charAt(i) == '#'){
                int top = stack.pop() - 1;
                if (top > 0) {
                    stack.push(top);
                }
                i++;
            } else {
                // 读一个数字
                while (i < n && preorder.charAt(i) != ',') {
                    i++;
                }
                int top = stack.pop() - 1;
                if (top > 0) {
                    stack.push(top);
                }
                stack.push(2);
            }
        }
        return stack.isEmpty();
    }
}

//leetcode submit region end(Prohibit modification and deletion)
class Solution {
    public boolean isValidSerialization(String preorder) {
        int n = preorder.length();
        int i = 0;
        int slots = 1;
        while (i < n) {
            if (slots == 0) {
                return false;
            }
            if (preorder.charAt(i) == ',') {
                i++;
            } else if (preorder.charAt(i) == '#'){
                slots--;
                i++;
            } else {
                // 读一个数字
                while (i < n && preorder.charAt(i) != ',') {
                    i++;
                }
                slots++; // slots = slots - 1 + 2
            }
        }
        return slots == 0;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/verify-preorder-serialization-of-a-binary-tree/solution/yan-zheng-er-cha-shu-de-qian-xu-xu-lie-h-jghn/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 20.  [146. LRU 缓存机制](https://leetcode-cn.com/problems/lru-cache/)

```Java
class LRUCache {
    private int cap;
    private Map<Integer,Integer> map=new LinkedHashMap<>();


    public LRUCache(int capacity) {
        this.cap=capacity;

    }
    
    public int get(int key) {
        if(map.keySet().contains(key)){
            int value=map.get(key);
            map.remove(key);  // 先取出，然后放在最后
            map.put(key,value);
            return value;
        }
        return -1;

       /* if (map.keySet().contains(key)) {
			int value = map.get(key);
			map.remove(key);
                       // 保证每次查询后，都在末尾
			map.put(key, value);
			return value;
		}
		return -1;*/
    }
    
    public void put(int key, int value) {
        if(map.keySet().contains(key)){ // 如果已经有值，先删除
            map.remove(key);
        }else if(map.size()==cap){  // 如果已经大道最大容量，先删除
            Iterator<Map.Entry<Integer,Integer>> iterator=map.entrySet().iterator();
            iterator.next(); // 先到真正的数据节点
            iterator.remove();
        }

        map.put(key,value); //放入值
       
    }
}


/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
```

### 21. [1143. 最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)

```Java
//给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。 
//
// 一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。 
//例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
// 
//
// 若这两个字符串没有公共子序列，则返回 0。 
//
// 
//
// 示例 1: 
//
// 输入：text1 = "abcde", text2 = "ace" 
//输出：3  
//解释：最长公共子序列是 "ace"，它的长度为 3。
// 
//
// 示例 2: 
//
// 输入：text1 = "abc", text2 = "abc"
//输出：3
//解释：最长公共子序列是 "abc"，它的长度为 3。
// 
//
// 示例 3: 
//
// 输入：text1 = "abc", text2 = "def"
//输出：0
//解释：两个字符串没有公共子序列，返回 0。
// 
//
// 
//
// 提示: 
//
// 
// 1 <= text1.length <= 1000 
// 1 <= text2.length <= 1000 
// 输入的字符串只含有小写英文字符。 
// 
// Related Topics 动态规划 
// 👍 395 👎 0


//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int n = text1.length();
        int m = text2.length();
        if (m==0||n==0) return 0;
        int[][] dp = new int[n+1][m+1];
        for (int i=1;i<=n;i++){
            for(int j=1; j<=m; j++){
                if (text1.charAt(i-1) == text2.charAt(j-1))  dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[n][m];

    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 22. [92. 反转链表 II](https://leetcode-cn.com/problems/reverse-linked-list-ii/)

```java
//给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链
//表节点，返回 反转后的链表 。
// 
//
// 示例 1： 
//
// 
//输入：head = [1,2,3,4,5], left = 2, right = 4
//输出：[1,4,3,2,5]
// 
//
// 示例 2： 
//
// 
//输入：head = [5], left = 1, right = 1
//输出：[5]
// 
//
// 
//
// 提示： 
//
// 
// 链表中节点数目为 n 
// 1 <= n <= 500 
// -500 <= Node.val <= 500 
// 1 <= left <= right <= n 
// 
//
// 
//
// 进阶： 你可以使用一趟扫描完成反转吗？ 
// Related Topics 链表 
// 👍 796 👎 0


//leetcode submit region begin(Prohibit modification and deletion)
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseBetween(ListNode head, int left, int right) {
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
        ListNode pre = dummy;
        for (int i=0;i<left - 1;i++){
            pre = pre.next;
        }
        ListNode cur = pre.next;
        ListNode next;
        for(int i=0;i<right - left; i++){
            next = cur.next;
            cur.next = next.next;
            next.next = pre.next;
            pre.next = next;
        }
        return dummy.next;

    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 23. [785. 判断二分图](https://leetcode-cn.com/problems/is-graph-bipartite/)

```java
//存在一个 无向图 ，图中有 n 个节点。其中每个节点都有一个介于 0 到 n - 1 之间的唯一编号。给你一个二维数组 graph ，其中 graph[u]
// 是一个节点数组，由节点 u 的邻接节点组成。形式上，对于 graph[u] 中的每个 v ，都存在一条位于节点 u 和节点 v 之间的无向边。该无向图同时具有
//以下属性：
// 
// 不存在自环（graph[u] 不包含 u）。 
// 不存在平行边（graph[u] 不包含重复值）。 
// 如果 v 在 graph[u] 内，那么 u 也应该在 graph[v] 内（该图是无向图） 
// 这个图可能不是连通图，也就是说两个节点 u 和 v 之间可能不存在一条连通彼此的路径。 
// 
//
// 二分图 定义：如果能将一个图的节点集合分割成两个独立的子集 A 和 B ，并使图中的每一条边的两个节点一个来自 A 集合，一个来自 B 集合，就将这个图称
//为 二分图 。 
//
// 如果图是二分图，返回 true ；否则，返回 false 。 
//
// 
//
// 示例 1： 
//
// 
//输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
//输出：false
//解释：不能将节点分割成两个独立的子集，以使每条边都连通一个子集中的一个节点与另一个子集中的一个节点。 
//
// 示例 2： 
//
// 
//输入：graph = [[1,3],[0,2],[1,3],[0,2]]
//输出：true
//解释：可以将节点分成两组: {0, 2} 和 {1, 3} 。 
//
// 
//
// 提示： 
//
// 
// graph.length == n 
// 1 <= n <= 100 
// 0 <= graph[u].length < n 
// 0 <= graph[u][i] <= n - 1 
// graph[u] 不会包含 u 
// graph[u] 的所有值 互不相同 
// 如果 graph[u] 包含 v，那么 graph[v] 也会包含 u 
// 
// Related Topics 深度优先搜索 广度优先搜索 图 
// 👍 239 👎 0


import com.sun.org.apache.xpath.internal.functions.FuncFalse;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public boolean isBipartite(int[][] graph) {
        // // 定义 visited 数组，初始值为 0 表示未被访问，赋值为 1 或者 -1 表示两种不同的颜色。
        int[] visited = new int[graph.length];
        Queue<Integer> queue = new LinkedList<>();
        for (int i=0;i<graph.length; i++){
            // 因为图中可能含有多个连通域，所以我们需要判断是否存在顶点未被访问，若存在则从它开始再进行一轮 bfs 染色。
            if (visited[i] != 0){
                continue;
            }
            // 每出队一个顶点，将其所有邻接点染成相反的颜色并入队。
            queue.offer(i);
            visited[i] = 1;
            while(!queue.isEmpty()){
                int v = queue.poll();
                for(int w: graph[v]){
                    if (visited[w] == visited[v]){
                        return false;
                    }
                    if (visited[w] == 0){
                        visited[w] = - visited[v];   // 去反值；
                        queue.offer(w);
                    }
                }
            }

        }
        return true;


    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 24. [1734. 解码异或后的排列](https://leetcode-cn.com/problems/decode-xored-permutation/)

```java
//给你一个整数数组 perm ，它是前 n 个正整数的排列，且 n 是个 奇数 。
//
// 它被加密成另一个长度为 n - 1 的整数数组 encoded ，满足 encoded[i] = perm[i] XOR perm[i + 1] 。比方说
//，如果 perm = [1,3,2] ，那么 encoded = [2,1] 。
//
// 给你 encoded 数组，请你返回原始数组 perm 。题目保证答案存在且唯一。
//
//
//
// 示例 1：
//
// 输入：encoded = [3,1]
//输出：[1,2,3]
//解释：如果 perm = [1,2,3] ，那么 encoded = [1 XOR 2,2 XOR 3] = [3,1]
//
//
// 示例 2：
//
// 输入：encoded = [6,5,4,6]
//输出：[2,4,1,5,3]
//
//
//
//
// 提示：
//
//
// 3 <= n < 105
// n 是奇数。
// encoded.length == n - 1
//
// Related Topics 位运算
// 👍 17 👎 0


//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public int[] decode(int[] encoded) {
        // 只要一个数字确定，那么所有都确定了
// 暴搜需要找n次，判断第一个数字是否合理，超时
// 那么可以提前算出一个数字, 具体的：

// (encoded共 n-1 个，a共n个)
// encoded所有[奇数索引]异或 encoded[1] ^ encoded[3] ..... ^ encoded[n-2]  (最后一个n-2是奇数)
// 就会得到a[1] ^ a[2] ^ a[3] ..... ^ a[n-2] ^ a[n-1] ，记为total

// 发现了没，正好缺少a[0]，接下来可以想办法把a[0]，也就是第一个给求出来

// 另 ALL = a[0] ^ a[1] ^ a[2] ^ a[3] ..... ^ a[n-2] ^ a[n-1] = 1 ^ 2.....^ n

// 则 ALL = a[0] ^ total
//    a[0] = ALL ^ total
        int n = encoded.length + 1;
        int[] res = new int[n];
        int total = 0;
        for(int i=1; i<n-1;i+=2){
            total ^= encoded[i];
        }
        int ALL = 0;
        for(int i=1;i<=n;i++){
            ALL ^=i;
        }
        res[0] = total ^ ALL;
        for(int i=0;i<n-1;i++){
            res[i+1] = res[i] ^ encoded[i];
        }
        return res;


    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 25.[61. 旋转链表](https://leetcode-cn.com/problems/rotate-list/)

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head == null || k ==0)  return head;
        ListNode t= head;
        int len = 0;
        while (t!=null){
            len++;
            t = t.next;
        }
        k = k % len;
        if (k ==0 )  return head;   // 如果是整个转换，就不用了，直接返回   
        t = head;
        while (k>0){
            k--;
            t =t.next;
        }
        ListNode fin = head;
        while (t.next != null){
            fin = fin.next;
            t = t.next;
        }
        ListNode res = fin.next;
        fin.next = null;
        t.next = head;
        return res;

    }
}

    
```

### 26. [173. 二叉搜索树迭代器](https://leetcode-cn.com/problems/binary-search-tree-iterator/)

```Java
class BSTIterator {
    private TreeNode cur;
    private Deque<TreeNode> stack;

    public BSTIterator(TreeNode root) {
        cur = root;
        stack = new LinkedList<TreeNode>();

    }
    
    public int next() {
        while (cur != null){  // 当前节点不为空，不断入栈，如果为空，就直接从栈中取一个
            stack.push(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        int res = cur.val;
        cur = cur.right;
        return res;

    }
    
    public boolean hasNext() {
        return cur != null || !stack.isEmpty();

    }
}
```



### 27. [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

```java
class Solution {//双指针解法
    public List<List<Integer>> threeSum(int[] nums) {

        List<List<Integer>> res=new ArrayList<>();
        int len=nums.length;
        if(nums==null||len<3)   return res;//不足三个的话，视为失败

        Arrays.sort(nums);//先排序

        for(int i=0;i<len;i++){
            if(nums[i]>0)  break;//最小值大于零，就不用要了

            if(i>0&&nums[i]==nums[i-1])  continue;//如果有重复的，直接跳到下一个
            

            int L=i+1;//左指针
            int R=len-1;//右指针

            while(L<R){
                int sum=nums[i]+nums[L]+nums[R];
                if(sum==0){
                    res.add(Arrays.asList(nums[i],nums[L],nums[R]));
                    while(L<R&&nums[L]==nums[L+1])   L++;//如果有重复的，直接跳到下一个
                    while(L<R&&nums[R]==nums[R-1])   R--;//如果有重复的，直接跳到下一个


                    L++;
                    R--;
                }
               else if (sum < 0) L++;//右移左指针
                else if (sum > 0) R--;//左移右指针
                


            }
        }
    



        return res;



    }
}
```

### 28. [74. 搜索二维矩阵](https://leetcode-cn.com/problems/search-a-2d-matrix/)

```java
class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        if (matrix == null || matrix.length ==0 || matrix[0].length == 0){
            return false;
        }
        int m = matrix.length, n = matrix[0].length;
        int u = 0, d = m - 1;
        while (u < d){
            int mid = (u + d) >>1;
            if (matrix[mid][n-1] < target){
                u++;
            }else{
                d = mid;
            }
        }

        int l=0, r = n - 1;
        while (l <r){
            int mid = (l+r)>>1;
            if (matrix[d][mid] < target){
                l ++;
            }else{
                r = mid;
            }
        }
        return matrix[d][l] == target;

    }
}
```

### 29. [1806. 还原排列的最少操作步数](https://leetcode-cn.com/problems/minimum-number-of-operations-to-reinitialize-a-permutation/)

```java
class Solution {
    public int reinitializePermutation(int n) {
        int pos = 1, res = 0;
        do{
            pos = (pos & 1) == 1 ? n/2 + (pos -1)/2: pos /2;
            res ++;
        }while(pos !=1);
        return res;

    }
}
```

### 30. [1807. 替换字符串中的括号内容](https://leetcode-cn.com/problems/evaluate-the-bracket-pairs-of-a-string/)

```java
import java.util.HashMap;
class Solution {
    public String evaluate(String s, List<List<String>> knowledge) {
        Map<String, String> map = new HashMap<>();
        for(List<String> l : knowledge){
            map.put(l.get(0), l.get(1));
        }

        StringBuilder res = new StringBuilder();
        StringBuilder tmp = new StringBuilder();
        int keyCount = 0;

        for (char c: s.toCharArray()){  //  转化成数组
            if (c == '(') {

                keyCount ++;
            }else if (c ==')'){
                keyCount --;
                res.append(map.getOrDefault(tmp.toString(), "?"));  // 将sb  -- 》 string
                tmp = new StringBuilder();
            }else if (keyCount > 0){
                tmp.append(c);
            }else{
                res.append(c);
            }
        }
        return res.toString();

    }
}
```

### 31. [90. 子集 II](https://leetcode-cn.com/problems/subsets-ii/)

```java
class Solution {
    List<Integer> t = new ArrayList<>();
    List<List<Integer>> res = new ArrayList<List<Integer>>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        Arrays.sort(nums);
        dfs(false, 0 ,nums);
//        for(List<Integer> l: res){
//            System.out.println(l);
//        }
        return res;
    }

    public void dfs(boolean choosePre, int cur, int[] nums){
        if (cur == nums.length){
            res.add(new ArrayList<Integer>(t));
            return;
        }
        dfs(false, cur + 1, nums);  // 当前结果不选，跳过
        if (!choosePre && cur > 0 && nums[cur - 1] == nums[cur]){    //  在递归时，若发现没有选择上一个数，且当前数字与上
                                                                    // 一个数相同，则可以跳过当前生成的子集（避免最终结果的重复）
            return;
        }
        t.add(nums[cur]);   // 当前结果选，并递归回溯
        dfs(true, cur + 1, nums);
        t.remove(t.size() -1);
    }
}
```

### 32. [48. 旋转图像](https://leetcode-cn.com/problems/rotate-image/)

```java
class Solution {
    public void rotate(int[][] matrix) {
        /*
        * 以旋转法来做题，首先要考虑到奇数、偶数边长，旋转的范围，由此确定循环边界条件
        * 然后模拟一个旋转过程，找到索引替换的公式*/
        int n = matrix.length; 
        for(int i=0; i< n/2; i++){    
            for (int j=0;i<(n+1)/2; j++){
                int tmp = matrix[i][j];  
                matrix[i][j] = matrix[n- j -1][i];
                matrix[n - j -1][i] = matrix[n - i -1][n- j -1];
                matrix[n- i-1][n-j-1] = matrix[j][n - i -1];
                matrix[j][n - i -1] = tmp;

            }
        }



    }
}
```

### 33. [5773. 插入后的最大值](https://leetcode-cn.com/problems/maximum-value-after-insertion/)

```java
// 方法想对了，但是返回是string 而不是int

// 左闭右开的截取字符串substring
class Solution {
    public String maxValue(String n, int x) {
        if(n.charAt(0) == '-'){
            int k = 0;
            while(k< n.length() &&(n.charAt(k)-'0')  <= x){
                k++;
            }
            return n.substring(0,k) + ""+x + n.substring(k,n.length());
            
        }else{
            int k = 0;
            while(k< n.length() && (n.charAt(k)-'0') >= x){
                k++;
            }
            return n.substring(0,k) + ""+x + n.substring(k,n.length());     
        }
    }
}
```

### 34. [5781. 删除一个字符串中所有出现的给定子字符串](https://leetcode-cn.com/problems/remove-all-occurrences-of-a-substring/)

```java
class Solution {
    public String removeOccurrences(String s, String part) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length(); i++) {
            sb.append(s.charAt(i));
        }
        int n = part.length();
        int cnt = 1;
        while (cnt > 0) {
            int tmp = cnt;
            for (int i = 0; i < sb.length() - n; i++) {
                if (part.equals(sb.substring(i, i + n))) {
                    sb.replace(i, i + n, "");
                    tmp++;
                    break;
                }
            }
            for (int i = sb.length(); i > n; i--) {
                if (part.equals(sb.substring(i - n, i))) {
                    sb.replace(i - n, i, "");
                    tmp++;
                    break;
                }
            }
            cnt = tmp - cnt;
        }
         if(sb.toString().equals(part)){
            return "";
        }
        return sb.toString();


    }
}
///
class Solution {
    public String removeOccurrences(String s, String part) {
        // 不要被左右干扰，可以直接单向消除
        // indexOf, substring() 方法要熟悉
        while(true){
            int idx = s.indexOf(part);
            if(idx == -1)   break;
            s = s.substring(0, idx) + s.substring(idx + part.length());
        }
        return s;

    }
}
 
```

### 35 [5782. 最大子序列交替和](https://leetcode-cn.com/problems/maximum-alternating-subsequence-sum/)

```java
必须在卖出上一股后才能买入下一股（可以当天卖出再当天买入）
public long maxAlternatingSum(int[] nums) {
        int n = nums.length;
        long ans = 0;
        int pre = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] > pre) {//价格比原先买入价格高，就卖出一股，并买入
                ans += nums[i] - pre;
                pre = nums[i];
            } else pre = nums[i];//如果遇到价格更低的，则换成该天买入
        }
        return ans;
    }

作者：mei-mi-xiao-gan-xing
链接：https://leetcode-cn.com/problems/maximum-alternating-subsequence-sum/solution/mo-ni-gu-piao-jiao-yi-by-mei-mi-xiao-gan-nxa3/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


///
class Solution {

    public long maxAlternatingSum(int[] nums) {
        int n = nums.length;
        // 理解成股票问题
        // 左边多出一个作为初始化的0，
        long[][] f = new long[n+1][2];
        f[0][0] = 0;
        for(int i = 1;i<=n;i++){
            // 0- 当前的截止，或者说之前最大的为偶数，所以新加的要减去
            f[i][0] = Math.max(f[i-1][0], f[i-1][1] -nums[i-1]);
            // 1- 当前的截止，最大的串作为奇数，所以最后的要加上，由于之前的算法可能为负数，所以这里需要内部max一下，如果负数，不如相当于之前全都不计算，为0
            f[i][1] = Math.max(f[i-1][1], Math.max(0L, f[i-1][0]) + nums[i-1]);
        }
        return Math.max(f[n][0], f[n][1]);     
    }
}
```

### 36. [1915. 最美子字符串的数目](https://leetcode-cn.com/problems/number-of-wonderful-substrings/)

```java
class Solution {
    public long wonderfulSubstrings(String word) {
        // 实际上是一个变相前缀和的问题，这里需要计算累计字母频率
        // 但是也要二进制表示一个奇偶状态，这一步我没由想全
        int[] cnt = new int[1024];
        cnt[0]++;
        int n = word.length();
        int[][] sum = new int[100020][10];
        long res = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 0;j < 10; j++) {
                if (word.charAt(i-1) - 'a' == j) {
                    sum[i][j] = sum[i - 1][j] + 1;
                } else {
                    sum[i][j] = sum[i - 1][j];
                }
            }
            int state = 0;
            for (int j = 0; j < 10; j++) {
                state = (state *2) + (sum[i][j] %2);
            }
            res += cnt[state];
            for (int j = 0; j < 10; j++) {
                res += cnt[state ^ (1 << j)];
            }
            cnt[state]++;
        }
        return res;

    }
}
```

### 37. [1871. 跳跃游戏 VII](https://leetcode-cn.com/problems/jump-game-vii/)

```java
    public boolean canReach(String v, int minJump, int maxJump) {
        /**
         * 是类似的遍历方式，只是其中可以用前缀和来做一点优化
         */
        int n = v.length();
        char[] str = v.toCharArray();
        int[] f = new int[n + 1];// dp
        int[] s = new int[n + 1];// 前缀和
        f[1] = 1;
        s[1] = 1;
        for(int i=2;i<=n;i++){
            if(str[i-1] == '0' &&  i- minJump >= 1){
                int l = Math.max(1, i- maxJump);
                int r =  i - minJump;
                if(s[r] > s[l - 1]){
                    f[i] =1;
                }
            }
            s[i] = s[i-1] + f[i];
        }
        return f[n] ==1;
    }
}
```

### 38. [451. 根据字符出现频率排序](https://leetcode-cn.com/problems/sort-characters-by-frequency/)

```java
class Solution {
    public String frequencySort(String s) {
        Map<Character, Integer> map = new HashMap<Character, Integer>();
        int length = s.length();
        for (int i = 0; i < length; i++) {
            char c = s.charAt(i);
            int frequency = map.getOrDefault(c, 0) + 1;
            map.put(c, frequency);
        }
        List<Character> list = new ArrayList<Character>(map.keySet());
        Collections.sort(list, (a, b) -> map.get(b) - map.get(a)); 
        StringBuffer sb = new StringBuffer();
        int size = list.size();
        for (int i = 0; i < size; i++) {
            char c = list.get(i);
            int frequency = map.get(c);
            for (int j = 0; j < frequency; j++) {
                sb.append(c);
            }
        }
        return sb.toString();
    }
}


```

### 39.[1922. 统计好数字的数目](https://leetcode-cn.com/problems/count-good-numbers/)

```java
class Solution {
   public int countGoodNumbers(long n) {
        int N=(int)Math.pow(10,9)+7;
       // 不能直接算，内部如果不取余的话，会有数据溢出，结果不对
        return (int)(myPow(5, (n+1)/2, N) * myPow(4, n/2, N) %N);
    }
    //快速幂 (记得要取余N,不只是结果取余,每次乘机也要取余)
    public long myPow(long x, long n,int N) {
        if(n==0){
            return 1;
        }
        /**
        快速幂方式：1.res为1  2. 指数不断除以2， 3. 结果不断乘以底数，且循环中底数平方， 如果当前指数为奇数，结果多乘以一个底数
        */
        long res = 1;
        while(n !=0){
            if((n&1) == 1)  res = res *x % N;
            x = x * x %N;
            n >>=1;
        }
        return res;
    }


}
```

### 40 .[886. 可能的二分法](https://leetcode-cn.com/problems/possible-bipartition/)

```java
class Solution {
    // 染色法，本质就是dfs
	ArrayList<Integer>[] graph;// 使用邻接表存储图
	Map<Integer,Integer> color;//记录上色结果
	public boolean possibleBipartition(int N, int[][] dislikes) {
		graph=new ArrayList[N+1];// 0位其实不用,使用的使1~N位
		//ArrayList实例化
		for (int i = 0; i !=N+1; i++) {
			graph[i]=new ArrayList<Integer>();
		}
		//图初始化
		for(int[] cp:dislikes) {
			graph[cp[0]].add(cp[1]);
			graph[cp[1]].add(cp[0]);	
		}
		color=new HashMap();
		for(int node=1;node!=N+1;node++) {// 对该组N人遍历
			if(!color.containsKey(node)) {// 还未上色
				boolean OK=dfs(node,0);//从node开始深度遍历
				if(!OK) return false; 
			}
		}
		return true;
	}
	private boolean dfs(int node, int c) {
		//从possibleBipartition调用时node是未上色的
		if(color.containsKey(node)) {// 若已经上色则看是否上色正确
			boolean OK=color.get(node)==c;
			return OK;
		}
		color.put(node,c);// 上色
		// 深度遍历
		for(int noFriend:graph[node]) {
			boolean OK=dfs(noFriend,c^1);
			if(!OK) return false;
		}
		return true;
	}
}


class Solution {
    public boolean possibleBipartition(int N, int[][] dislikes) {
        // 构造邻接表
        List<List<Integer>> diss = new ArrayList<>();
        for(int i = 0; i <= N; i++) {
            diss.add(new ArrayList<>());
        }

        for(int [] dislike : dislikes) {
            diss.get(dislike[0]).add(dislike[1]);
            diss.get(dislike[1]).add(dislike[0]);
        }

        // 初始化并查集
        UnionFind uf = new UnionFind(N+1);
        for(int i = 1; i <= N; i++) {
            List<Integer> dis = diss.get(i);
            if(dis.isEmpty()) continue;
            // 这里先判断是否相并
            int root1 = uf.find(i);
            int root2 = uf.find(dis.get(0));
            if(root1 == root2) return false;
            for(int j = 1; j < dis.size(); j++) {
                int root3 = uf.find(dis.get(j));
                if(root1 == root3) return false;
                // 归并，将不喜欢的并在一起
                uf.parents[root3] = root2;
            }
        }
        return true;
    }
}
// 并查集类
class UnionFind{

    int []parents;

    public UnionFind(int count) {
        // 构造函数初始化
        parents = new int[count+1];
        for(int i = 0; i <= count; i++) {
            parents[i] = i;
        }
    }

    int find(int x) {
        // 归并
        while(parents[x] != x) {
            parents[x] = parents[parents[x]];
            x = parents[x];
        }
        return x;
    }
}


```



### 41. [1923. 最长公共子路径](https://leetcode-cn.com/problems/longest-common-subpath/)

```java

class Solution {
    int N = 100010;
    int[][] paths; 
    long[] p, h;
    public int longestCommonSubpath(int n, int[][] paths) {
        this.paths = paths;
        p = new long[N];
        h = new long[N];
        int l = 0, r = N;
        for (int[] pa : paths) {
            r = Math.min(r, pa.length);
        }
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (check(mid)) l = mid;
            else r = mid - 1;
        }
        return r;
    }
    public boolean check(int mid) {
        HashSet<Long> set = new HashSet<>();
        p[0] = 1;  // 初始化
        int k = 0;
        for (int[] pa : paths) {
            int n = pa.length;
            // 字符串哈希的公式。本质是是和指数的哈希进制转换
            for (int i = 1; i <= n; ++i) {
                p[i] = p[i - 1] * 133331;
                h[i] = h[i - 1] * 133331 + pa[i - 1];
            }
            HashSet<Long> tmp = new HashSet<>();
            for (int i = mid; i <= n; ++i) {
                long t = get(i - mid + 1, i);
                if (k == 0) {
                    set.add(t);
                } else {
                    tmp.add(t);
                }
            }
            if (k != 0) set.retainAll(tmp); // 移除不再这里的元素
            if (set.size() == 0) return false; // 最终集合中需要有至少一个符合条件的哈希值
            k++;
        }
        return true;
    }
    // 这里是字符串哈希公式
    public long get(int l, int r) {
        return h[r] - h[l - 1] * p[r - l + 1];
    }
}


```

### 42 [1418. 点菜展示表](https://leetcode-cn.com/problems/display-table-of-food-orders-in-a-restaurant/)

```java
class Solution {
    public List<List<String>> displayTable(List<List<String>> orders) {
        TreeMap<Integer, TreeMap<String, Integer>> map = new TreeMap<>();
        Set<String> names= new HashSet<>();
        for(List<String> order: orders){
            TreeMap<String, Integer> table = map.getOrDefault(Integer.parseInt(order.get(1)), new TreeMap<>());
            String name = order.get(2);
            names.add(name);
            table.put(name, table.getOrDefault(name, 0)+1);
           map.put(Integer.parseInt(order.get(1)), table);
        }
        // System.out.println(map);
        List<List<String>> res = new ArrayList<>();
        List<String> row1 = new ArrayList<>();
        row1.add("Table");
        List<String> name_s = new ArrayList<>(names);
        Collections.sort(name_s);
        row1.addAll(name_s);
        res.add(row1);
        for(Map.Entry entry: map.entrySet()){
            List<String> row = new ArrayList<>();
            row.add( String.valueOf(entry.getKey()));
            // System.out.println(entry.getValue());
            for(String t: name_s){
                // System.out.println(t);
                    TreeMap<String, Integer> tmp = (TreeMap)entry.getValue();
                    if(!tmp.containsKey(t)){
                        row.add("0");
                    }else{
                        row.add(String.valueOf(tmp.get(t)));
                    }
                }
                res.add(row);
   
        }
        return res;

        

    }
}
```

### 43. [1711. 大餐计数](https://leetcode-cn.com/problems/count-good-meals/)

```java
class Solution {
    public int countPairs(int[] deliciousness) {
        // 解法上还是两数之和的思路。优化的点，在于先设置好上限，然后每次枚举数的2倍
        int max = deliciousness[0];
        int mod = (int)1e9+7;
        for(int i: deliciousness){
            max = Math.max(max,i);
        }
        int maxSum = max *2;
        int res = 0;
        Map<Integer, Integer> map = new HashMap<>();
        int n= deliciousness.length;
        // 从前到后，不重不漏，也不需要重新记录每个数的个数
        for(int i=0;i<n;i++){
            int val = deliciousness[i];
            for(int sum =1; sum <= maxSum; sum<<=1){
                int count = map.getOrDefault(sum -val, 0);
                res  = (res + count) %mod;
            }
            map.put(val, map.getOrDefault(val,0) +1);

        }
        return res;

    }
}
```



### 44 .[930. 和相同的二元子数组](https://leetcode-cn.com/problems/binary-subarrays-with-sum/)

```java
class Solution {
    public int numSubarraysWithSum(int[] nums, int goal) {
        int sum =0;
        Map<Integer, Integer> map = new HashMap<>();
        int res = 0;
        for(int num: nums){
            // 先更新这之前，sum相同的个数，实际上（两个间隔的1之间有几个0）
            map.put(sum, map.getOrDefault(sum, 0) +1);
            // 更新sum
            sum += num;
            // 计算之前有多少个值==sum -goal 的 0的位置
            res += map.getOrDefault(sum - goal, 0);

        }
        return res;

    }
}

// 双指针解法
class Solution {
    public int numSubarraysWithSum(int[] nums, int t) {
        int n = nums.length;
        int ans = 0;
        for (int r = 0, l1 = 0, l2 = 0, s1 = 0, s2 = 0; r < n; r++) {
            s1 += nums[r];
            s2 += nums[r];
            while (l1 <= r && s1 > t) s1 -= nums[l1++];
            while (l2 <= r && s2 >= t) s2 -= nums[l2++];
            ans += l2 - l1;
        }
        return ans;
    }
}

作者：AC_OIer
链接：https://leetcode-cn.com/problems/binary-subarrays-with-sum/solution/gong-shui-san-xie-yi-ti-shuang-jie-qian-hfoc0/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```





### 45.[981. 基于时间的键值存储](https://leetcode-cn.com/problems/time-based-key-value-store/)

```java
class TimeMap {
    class Node {
        String k, v; 
        int t;
        Node (String _k, String _v, int _t) {
            k = _k; v = _v; t = _t;
        }
    }
    
    Map<String, List<Node>> map = new HashMap<>();
    public void set(String k, String v, int t) {
        List<Node> list = map.getOrDefault(k, new ArrayList<>());
        list.add(new Node(k, v, t));
        map.put(k, list);
    }
    
    public String get(String k, int t) {
        List<Node> list = map.getOrDefault(k, new ArrayList<>());
        if (list.isEmpty()) return "";
        int n = list.size();
        int l = 0, r = n - 1;
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (list.get(mid).t <= t) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return list.get(r).t <= t ? list.get(r).v : "";
    }
}
```

### 46.[97. 交错字符串](https://leetcode-cn.com/problems/interleaving-string/)

```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int m = s1.length(), n = s2.length(), q = s3.length();
        if(m + n != q){
            return false;
        }
        boolean[][] f = new boolean[m+1][n+1];
        f[0][0] = true;
        for(int i=0;i<= m ;++i){
            for(int j=0;j<=n;++j){
                int pos = i+j -1;
                if(i>0){
                    f[i][j] = f[i][j] || (f[i-1][j] && s1.charAt(i-1) == s3.charAt(pos));
                }
                if(j>0){
                    f[i][j] = f[i][j] || (f[i][j-1] && s2.charAt(j-1) == s3.charAt(pos));
                }
            }
        }
        return f[m][n];
    }
}
```

### 47.[5793. 迷宫中离入口最近的出口](https://leetcode-cn.com/problems/nearest-exit-from-entrance-in-maze/)

```java
class Solution {
    public int nearestExit(char[][] maze, int[] entrance) {
        /**
        不要用dfs会超时，最短这种字眼，实际上用bfs是最好的 */
        int m = maze.length;
        int n = maze[0].length;
        int step = 0;
        Queue<int[]> q = new LinkedList<>();
        q.offer(entrance);
        int[] mvoes = new int[]{-1,0,1,0,-1};
        // 还是需要考虑去重的
        Set<String> set = new HashSet<>();
        set.add(entrance[0]+"-"+entrance[1]);
        while(!q.isEmpty()){
            int size = q.size();
            step++;
            for(int s=0;s<size;s++){
                int[] tmp = q.poll();
                int x =tmp[0],y = tmp[1];

                for(int i=0;i<4;i++){
                    int _x= x + mvoes[i];
                    int _y = y + mvoes[i+1];
                          
                    if(_x >=0 &&  _x <m&& _y>=0 && _y<n && maze[_x][_y] == '.' && !set.contains(_x+"-"+_y)){
                        // System.out.println(String.format("%d, %d, m;%d",_x,_y,m));
                        set.add(_x+"-"+_y);
                        if((_x==0 || _x==m-1 ||_y==0 || _y==n-1)   ){
                            return step;
                        }
                        q.offer(new int[]{_x, _y});
                    }
                }
            }
        }
        return -1;

    }
}
```

### 48. [5794. 求和游戏](https://leetcode-cn.com/problems/sum-game/)

```java
class Solution {
   public  boolean sumGame(String num) {
        /**
         需要分类讨论
         sum(左右和之差) ==0 时， cnt(左右问号之差)  == 0 ，必败， 剩下的，必胜，因为总可以使左右不等
         sum > 0使， cnt > 0， 必胜，  cnt == 0时，必胜， cnt <  0:
         cnt 为奇数：必胜   cnt 为偶数： 如果 - （cnt/2) *9 != sum 必胜， 否则必败（这里Bob可以控制cnt每一对之和为9，惟一可控的抓手）
         sum < 0 ,解法类似*/
        int n = num.length(), cnt = 0;
        int sum = 0;
        for (int i = 0; i < n / 2; i++) {
            if (num.charAt(i) == '?') cnt++;
            else {
                sum += num.charAt(i) - '0';
            }
        }
        for (int i = n / 2; i < n; i++) {
            if (num.charAt(i) == '?') cnt--;
            else {
                sum -= num.charAt(i) - '0';
            }
        }
        // System.out.println(String.format("%d, %d", sum, cnt));

        if (sum == 0) {
            return cnt != 0;
        }
        if (sum < 0) {
            sum *= -1;
            cnt *= -1;
        }// 这样左右两种情况统一处理；
        if (cnt >= 0) return true;
        if (cnt < 0) {
            if (cnt % 2 == 1) {
                return true;
            }
            if (-cnt / 2 * 9 == sum) {
                return false;
            }
        }
        return true;


    }
}
```

### 49 .[1818. 绝对差值和](https://leetcode-cn.com/problems/minimum-absolute-sum-difference/)

```java
// 想到了二分但是没有勇气，fuck
class Solution {
    public int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
        final int MOD = 1000000007;
        int n = nums1.length;
        int[] rec = new int[n];
        System.arraycopy(nums1, 0, rec, 0, n);
        Arrays.sort(rec);
        int sum = 0, maxn = 0;
        for (int i = 0; i < n; i++) {
            int diff = Math.abs(nums1[i] - nums2[i]);
            sum = (sum + diff) % MOD;
            int j = binarySearch(rec, nums2[i]);
            if (j < n) {
                maxn = Math.max(maxn, diff - (rec[j] - nums2[i]));
            }
            if (j > 0) {
                maxn = Math.max(maxn, diff - (nums2[i] - rec[j - 1]));
            }
        }
        return (sum - maxn + MOD) % MOD;
    }

    public int binarySearch(int[] rec, int target) {
        int low = 0, high = rec.length - 1;
        if (rec[high] < target) {
            return high + 1;
        }
        while (low < high) {
            int mid = (high - low) / 2 + low;
            if (rec[mid] < target) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minimum-absolute-sum-difference/solution/jue-dui-chai-zhi-he-by-leetcode-solution-gv78/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 50 .[1846. 减小和重新排列数组后的最大元素](https://leetcode-cn.com/problems/maximum-element-after-decreasing-and-rearranging/)

```java
class Solution {
    public int maximumElementAfterDecrementingAndRearranging(int[] arr) {
        int n = arr.length;
        Arrays.sort(arr);
        int res = 0;
        for(int i: arr){
            if(i > res){
                res++;
            }
        }
        return res;

    }
}
```

### 51.[5814. 新增的最少台阶数](https://leetcode-cn.com/problems/add-minimum-number-of-rungs/)

```java
class Solution {
    public int addRungs(int[] rungs, int dist) {
        int n = rungs.length;
        int idx = 0;
        int cur  = 0;
        int res = 0;
        while(idx < n){
            if(rungs[idx] - cur <= dist){
                cur= rungs[idx];
                idx ++;
            }
            else{
                // 计算有多少个，不要直接加
                int t = (rungs[idx] - cur-1)/ dist;
                if( t  > 0){
                    cur += t * dist;
                    res += t;
                }else{
                    cur =cur + dist;
                    res ++;
                }

            }
        }
        return res;

    }
}
```



### 52 [面试题 10.02. 变位词组](https://leetcode-cn.com/problems/group-anagrams-lcci/)

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        for (String str : strs) {
            char[] array = str.toCharArray();
            Arrays.sort(array);
            String key = new String(array);
            List<String> list = map.getOrDefault(key, new ArrayList<String>());
            list.add(str);
            map.put(key, list);
        }
        return new ArrayList<List<String>>(map.values());
    }
}

```

### 53 .[5815. 扣分后的最大得分](https://leetcode-cn.com/problems/maximum-number-of-points-with-cost/)

```java
// 自己的计算复杂度是m* n*n,看数据规模大概猜到需要mn的，可惜不会优化
class Solution {
    public long maxPoints(int[][] points) {
        int m = points.length,n = points[0].length;
        long[][] f = new long[m][n];
        long res =0;
        int y = -1;
        for(int i=0;i<n;i++){
            f[0][i] = (long)points[0][i];
            if( f[0][i] > res){
                res = f[0][i];
                y = i;
            }
        }
        
        for(int i=1;i<m;i++){
            for(int j=0;j<n;j++){
                for(int k = 0;k< n ; k++){
                    f[i][j] =Math.max(f[i][j],  points[i][j] + f[i-1][k] - Math.abs(j - k));
                    if(i == m-1){
                        res = Math.max(res, f[i][j]);
                    }
                }
            }
        }
        //  System.out.println(y);
        // for(int i=1;i<m;i++){
        //     long tmp  = 0;
        //     int tmp_y = -1;
        //     for(int j=0;j<n;j++){
        //         f[i][j] = (long)points[i][j]  - Math.abs(j - y);
        //         if(f[i][j] > tmp){
        //             tmp = f[i][j];
        //             tmp_y = j;
        //         }
        //     }
        //     System.out.println("tmp_y: " + tmp_y);
        //     res += tmp;
        //     y = tmp_y;
        // }
        return res;

    }
}


class Solution {

    public long maxPoints(int[][] points) {
        int m = points.length;
        int n = points[0].length;
        long[] dp = new long[n];
        for (int i = 0; i < m; i++) {
            long[] cur = new long[n + 1];
            long lmax = 0;
            // 优化点在这里，相当于把abs拆成两个数组，分别是左 - 右  右-左， 然后在循环内并列循环，
            // 时间复杂度降到了3mn， 以左减右为例： 针对每个j，统计它左侧最大值-偏移量，和当前正上方的大小，确定值
            // 左侧最大值会不断更新，就是这里的更新逻辑，比赛时没有想到
            for (int j = 0; j < n; j++) {
                lmax = Math.max(lmax - 1, dp[j]);
                cur[j] = lmax;
            }
            long rmax = 0;
            for (int j = n - 1; j >= 0; j--) {
                rmax = Math.max(rmax - 1, dp[j]);
                cur[j] = Math.max(cur[j], rmax);
            }
            for (int j = 0; j < n; j++) {
                dp[j] = cur[j] + points[i][j];
            }
        }
        long ans = 0;
        for (int j = 0; j < n; j++) {
            ans = Math.max(ans, dp[j]);
        }
        return ans;
    }
}
// 参考题解
https://leetcode-cn.com/problems/maximum-number-of-points-with-cost/solution/omndong-tai-gui-hua-by-seiei-5dm2/
```

### 54 .[1838. 最高频元素的频数](https://leetcode-cn.com/problems/frequency-of-the-most-frequent-element/)

```java
class Solution {
    public int maxFrequency(int[] nums, int k) {
        // 一开始思路搞错了，以为是每个数字都可以k次操作
        Arrays.sort(nums);
		int n = nums.length;
		int total = 0;
		int l = 0, res = 1;
		for (int r = 1; r < n; ++r) {
            //total是将窗口所有的值都转换成窗口最后的那个值所需要的操作次数
            //因为窗口1次滑动1个单位，所以每次判断当前最右值和前一个值的差值，然后乘以窗口中除去最后一个元素的前面的元素个数。
			total += (nums[r] - nums[r - 1]) * (r - l);
            //当total大于k时，需要将左边界右移，操作是：total减去最左值与最右值的差值
            //然后将左边界右移
			while (total > k) {
				total -= nums[r] - nums[l];
				++l;
			}
            //动态维护res的大小
			res = Math.max(res, r - l + 1);
		}
        //最后返回res即可
		return res;
    }
}

```

### 55 .[57. 插入区间](https://leetcode-cn.com/problems/insert-interval/)

```java
class Solution {
    public int[][] insert(int[][] intervals, int[] newInterval) {
        int left = newInterval[0];
        int right = newInterval[1];
        boolean placed = false;
        List<int[]> ansList = new ArrayList<int[]>();
    // 直接加就是了， 主要还是对java不熟悉
        for (int[] interval : intervals) {
            if (interval[0] > right) {
                // 在插入区间的右侧且无交集
                if (!placed) {
                    ansList.add(new int[]{left, right});
                    placed = true;                    
                }
                ansList.add(interval);
            } else if (interval[1] < left) {
                // 在插入区间的左侧且无交集
                ansList.add(interval);
            } else {
                // 与插入区间有交集，计算它们的并集
                left = Math.min(left, interval[0]);
                right = Math.max(right, interval[1]);
            }
        }
        if (!placed) {
            ansList.add(new int[]{left, right});
        }
        int[][] ans = new int[ansList.size()][2];
        for (int i = 0; i < ansList.size(); ++i) {
            ans[i] = ansList.get(i);
        }
        return ans;
    }
}


```

###  56 [1743. 从相邻元素对还原数组](https://leetcode-cn.com/problems/restore-the-array-from-adjacent-pairs/)

```java
class Solution {

    Map<Integer, List<Integer>> map = new HashMap<>(); 
    public int[] restoreArray(int[][] adjacentPairs) {
        for(int[] p: adjacentPairs){
            map.putIfAbsent(p[0], new ArrayList<Integer>());
            map.putIfAbsent(p[1], new ArrayList<Integer>());
            map.get(p[0]).add(p[1]);
            map.get(p[1]).add(p[0]);
        }
        int n = adjacentPairs.length + 1;
        int[] res = new int[n];
        // 找到一个端点
        for(Map.Entry<Integer, List<Integer>> entry: map.entrySet()){
            int e = entry.getKey();
            List<Integer> adj = entry.getValue();
            if(adj.size() == 1){
                res[0] = e;
                break;
            }
        }
        res[1] = map.get(res[0]).get(0);
        for(int i=2;i<n;i++){
            // 确定了方向以后，不断连接数据
            List<Integer> adj = map.get(res[i-1]);
            res[i] = res[i-2] == adj.get(0) ? adj.get(1): adj.get(0);
        }
        return res;
        }

    
}
```

### 57.[1946. 子字符串突变后可能得到的最大整数](https://leetcode-cn.com/problems/largest-number-after-mutating-substring/)

```java
class Solution {
    public String maximumNumber(String num, int[] change) {
        char[] s = num.toCharArray();
        for(int i=0; i<s.length;i++){
            
            if(s[i] - '0' < change[s[i] - '0']){
                while( i < s.length && (s[i] - '0') <= change[s[i] - '0']){
                    // System.out.println(i);
                    s[i] = (char)(change[s[i] - '0'] + '0');
                    i++;
                }
                break;
            }
        }
        return new String(s);

    }
}
```

### 58 .[1942. 最小未被占据椅子的编号](https://leetcode-cn.com/problems/the-number-of-the-smallest-unoccupied-chair/)

```java
class Solution {
    public int smallestChair(int[][] times, int targetFriend) {
        /**
        看到题就知道是堆的做法，只是细节需要确定 */
        int n = times.length;
        int[][] arrivals = new  int[n][2];
        int[][] leaves = new int[n][2];
        // 记录出入的时间以及对应的人
        for(int i=0; i<n;i++){
            arrivals[i][0] = times[i][0];
            arrivals[i][1] = i;
            leaves[i][0] = times[i][1];
            leaves[i][1]= i;
        }
        // 依据时间顺序从小到大排列
        Arrays.sort(arrivals, (a, b) -> (a[0] - b[0]));
        Arrays.sort(leaves, (a, b) -> (a[0] - b[0]));

        Map<Integer, Integer> map = new HashMap<>();
        // 优先级队列就是堆了
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        // 记录已有座位
        for(int i=0; i<n;i++){
            pq.offer(i);
        }
        int j=0;
        // 依据到达时间占座
        for(int[] a: arrivals){
            // 如果有离开时间小于当前到达的，将释放的作为放进堆里
            while(j < n && leaves[j][0] <= a[0]){
                pq.offer(map.get(leaves[j][1]));
                j++;
            }
            // 记录对应的人和座位
            map.put(a[1], pq.poll());
            // 如果有遇到目标人，就可以直接返回
            if(a[1] == targetFriend){
                return map.get(targetFriend);
            }
        }
        return -1;

    }
}
```

### 59.[1943. 描述绘画结果](https://leetcode-cn.com/problems/describe-the-painting/)

```java
class Solution {
    public List<List<Long>> splitPainting(int[][] segments) {
        // 差分数组
        List<List<Long>> res = new ArrayList<List<Long>>();
        long[][] change = new long[(int)1e5+1][2];
        for(int i=0;i<segments.length;i++){
            change[segments[i][0]][0] += segments[i][2];
            change[segments[i][1]][1] -= segments[i][2];
        }
        long sum = 0;
        long start = 0;
        for(int i=0;i<change.length;i++){
            // System.out.println(res);
            if(change[i][0] != 0 || change[i][1] != 0){
                if(sum ==0){
                    start = i;
                }
                if(sum != 0){
                    // System.out.println(res);
                    res.add(Arrays.asList(new Long[]{start, (long)i, sum}));
                    start = i;
                }
                sum += change[i][0] + change[i][1];
            }
        }
        return res;

    }
}
```

### 60. [1947. 最大兼容性评分和](https://leetcode-cn.com/problems/maximum-compatibility-score-sum/)

```java
class Solution 
{
    // 实际上就是全排列模板题，问题是模板记错了，导致超时
    int sum = 0;
    boolean[] f;
    public int maxCompatibilitySum(int[][] students, int[][] mentors) {
        f = new boolean[students.length];
        dfs(students, mentors, 0, 0);
        return sum;
    }
    void dfs(int[][] a, int[][] b, int l, int cur){
        if(l == a.length){
            sum = Math.max(sum, cur);
            return;
        }
        for(int i = 0; i < a.length; i++){
            if(!f[i]){
                f[i] = true;
                dfs(a, b, l + 1, cur + get(a, b, l, i));
                f[i] = false;
            }
        }
    }
    int get(int[][] a, int[][] b, int idx, int jdx){
        int sum = 0;
        for(int i = 0; i < a[0].length; i++){
            if(a[idx][i] == b[jdx][i])sum++;
        }
        return sum;
    }
}



```

### 61 .[284. 顶端迭代器](https://leetcode-cn.com/problems/peeking-iterator/)

```java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
    // 本质上是先走一步的迭代器
    private Iterator<Integer> it;

    Integer next;
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    it = iterator;
        next = it.hasNext()?it.next(): null;
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        return next;
        
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
        Integer tmp = next;
        next = it.hasNext()? it.next():null;
        return tmp;
	    
	}
	
	@Override
	public boolean hasNext() {
        return next != null;
	    
	}
}
```

### 62 .[1104. 二叉树寻路](https://leetcode-cn.com/problems/path-in-zigzag-labelled-binary-tree/)

```java
class Solution {
    public List<Integer> pathInZigZagTree(int label) {
        int row = 1, rowStart = 1;
        // 判断行号
        while(rowStart *2 <= label){
            row++;
            rowStart *=2;
        }
        // 奇偶性顺序不同,如果当前行在偶数行，需要将对应的编号切换成左序的
        // System.out.println(label);
        // System.out.println(row);
        if(row % 2== 0){
            label = getReveser(label, row);
        }
        // System.out.println(label);
        List<Integer> res = new ArrayList<>();
        while(row > 0){
            if(row % 2  ==0){
                // 因为之前统一左序话，如果是偶数行，需要重新转换成右序记录结果
                // System.out.println("even: " + getReveser(label, row));
                res.add(getReveser(label, row));
            }else{
                // System.out.println("odd: " + label);
                res.add(label);
            }
            row--;
            label >>= 1;  // 实际的二叉树序列应该改是从左到右的
        }
        Collections.reverse(res);
        return res;

    }

    // 针对偶数情况下的序号获取操作,也是根据编号获取满二叉树形式节点的位置
    public int getReveser(int label, int row){
        return (1<<row - 1) + (1<<row) -1 -label;
    }
}
```

### 63 .[375. 猜数字大小 II](https://leetcode-cn.com/problems/guess-number-higher-or-lower-ii/)

```java
class Solution {
    public int getMoneyAmount(int n) {
        /**
        重叠子问题，dp问题
        二维数组i，j标识i到j内的最坏情况下的最小情况，由于一次就准为 耗费为0 ，所以初始化就哈皮
        i为长度， j为起始，由于最坏是越大越好，因此从中带你后半段开始统计
        每一次统计当前位置选错的情况下，的耗费
        最后统一 */
        int[][] dp = new int[n+1][n+1];
        for(int len = 2; len<= n ;len++){
            for(int start = 1; start + len <= n + 1; start++){
                int minRes = Integer.MAX_VALUE;
                for(int idx = start + (len - 1)/2 ; idx < start + len -1; idx++){
                    // System.out.println(String.format("start: %d, idx: %d, len%d", start,idx,len));
                    int res =  idx + Math.max(dp[start][idx - 1], dp[idx+1][start + len -1]);
                    minRes = Math.min(res, minRes);
                }
                dp[start][start + len -1] = minRes;
            }
        }
        return dp[1][n];

    }
}
```





### 64 .[368. 最大整除子集](https://leetcode-cn.com/problems/largest-divisible-subset/)

```java
class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        int n = nums.length;
        Arrays.sort(nums);

        //有点类似最长递增数组， 首先确定最大长度，以及集合中的最大值
        int[] dp = new int[n];
        Arrays.fill(dp, 1);   // 初始时只有一个集合
        int maxSize = 1;
        int maxVal = dp[0]; // 记录集合中的最大值
        for(int i=1;i<n;i++){
            for(int j =0; j < i ; j++)
            {
                if(nums[i] % nums[j] == 0){
                    dp[i] = Math.max(dp[i], dp[j] +1);
                }
                // System.out.println(String.format("maxSize: %d, i %d, j: %d", maxSize, i, j));
            }

            if(dp[i] > maxSize){
                
                maxSize = dp[i];
                maxVal  = nums[i];
            }
            // System.out.println(String.format("maxSize: %d, i %d, j: %d", maxSize, i, j));
        }

        // 倒推获取最大子集：从后往前，依次找到符合最大长度，和最大值相除为0的值
        List<Integer> res = new ArrayList<>();
        if(maxSize == 1){
            res.add(nums[0]);
            return res;
        }
        for(int i = n -1;i >=0 && maxSize > 0;i--){
            if(dp[i] == maxSize && maxVal % nums[i] == 0){
                res.add(nums[i]);
                maxVal = nums[i];
                maxSize--;

            }
        }
        return res;

    }
}
```

### 65.[863. 二叉树中所有距离为 K 的结点](https://leetcode-cn.com/problems/all-nodes-distance-k-in-binary-tree/)

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
    Map<Integer, TreeNode> parents = new HashMap<Integer, TreeNode>();
    List<Integer> ans = new ArrayList<Integer>();

    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        // 从 root 出发 DFS，记录每个结点的父结点
        findParents(root);

        // 从 target 出发 DFS，寻找所有深度为 k 的结点
        findAns(target, null, 0, k);

        return ans;
    }

    public void findParents(TreeNode node) {
        if (node.left != null) {
            parents.put(node.left.val, node);
            findParents(node.left);
        }
        if (node.right != null) {
            parents.put(node.right.val, node);
            findParents(node.right);
        }
    }

    public void findAns(TreeNode node, TreeNode from, int depth, int k) {
        if (node == null) {
            return;
        }
        if (depth == k) {
            ans.add(node.val);
            return;
        }
        if (node.left != from) {
            findAns(node.left, node, depth + 1, k);
        }
        if (node.right != from) {
            findAns(node.right, node, depth + 1, k);
        }
        if (parents.get(node.val) != from) {
            findAns(parents.get(node.val), node, depth + 1, k);
        }
    }
}
```

### 66 .[324. 摆动排序 II](https://leetcode-cn.com/problems/wiggle-sort-ii/)

```java
class Solution {
    public  void wiggleSort(int[] nums) {
        int[]bucket=new int[5001];
        for(int num:nums)bucket[num]++;
        int len=nums.length;
        int small,big;//穿插数字时的上界
        //总长度为奇数时，“小 大 小 大 小”边界左右都为较小的数；
        //总长度为偶数时，“小 大 小 大”边界左为较小的数，边界右为较大的数
        if((len&1)==1){
            small=len-1;
            big=len-2;
        }else{
            small=len-2;
            big=len-1;
        }
        int j=5000; //从后往前，将桶中数字穿插到数组中，后界为j
        //桶中大的数字在后面，小的数字在前面，所以先取出较大的数字，再取出较小的数字
        //先将桶中的较大的数字穿插放在nums中
        for(int i=1;i<=big;i+=2){
            while (bucket[j]==0)j--;//找到不为0的桶
            nums[i]=j;
            bucket[j]--;
        }
        //再将桶中的较小的数字穿插放在nums中
        for(int i=0;i<=small;i+=2){
            while (bucket[j]==0)j--;//找到不为0的桶
            nums[i]=j;
            bucket[j]--;
        }
    }
}
```



### 67 .[99. 恢复二叉搜索树](https://leetcode-cn.com/problems/recover-binary-search-tree/)

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public void recoverTree(TreeNode root) {
        Deque<TreeNode> stack  = new ArrayDeque<TreeNode>();
        TreeNode x = null, y = null, pred= null;
        // 中序遍历迭代写法
        while(!stack.isEmpty() || root != null){
            while(root != null){
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            // 如果是逆序的，那么用y记录
            if(pred != null && root.val < pred.val){
                y = root;
                if(x == null){
                    x = pred;
                }else{
                    break;
                }
            }
            pred = root;
            root = root.right;
        }
        swap(x, y);
    }

    // 交换两个值
    public void swap(TreeNode x , TreeNode y){
        int tmp = x.val;
        x.val = y.val;
        y.val = tmp;
    }
}
```

### 68.[209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        int[] sum = new int[n+1];
        for(int i=0; i<n;i++){
            sum[i+1] = sum[i] + nums[i];
        }
        for(int i=1;i<=n;i++){
            for(int j=0; j + i <= n; j++){
                if(sum[j+i] - sum[j] >= target ){
                    return i;
                }
            }
        }
        return 0;

    }
}

// 上面的方法本质还是O(n^2)的，滑动窗口的O（N）明显更优秀
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int start = 0, end = 0;
        int sum = 0;
        while (end < n) {
            sum += nums[end];
            while (sum >= s) {
                ans = Math.min(ans, end - start + 1);
                sum -= nums[start];
                start++;
            }
            end++;
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum/solution/chang-du-zui-xiao-de-zi-shu-zu-by-leetcode-solutio/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

// O(nlogn)的前缀和+ 二分也可以，不过Arrays.binarySearch()不熟悉
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int ans = Integer.MAX_VALUE;
        int[] sums = new int[n + 1]; 
        // 为了方便计算，令 size = n + 1 
        // sums[0] = 0 意味着前 0 个元素的前缀和为 0
        // sums[1] = A[0] 前 1 个元素的前缀和为 A[0]
        // 以此类推
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 1; i <= n; i++) {
            int target = s + sums[i - 1];
            // 通过这个来查找时否有sum[j] - sum[i-1] == s
            int bound = Arrays.binarySearch(sums, target);
            // binarySearch()方法的返回值为：1、如果找到关键字，则返回值为关键字在数组中的位置索引，且索引从0开始2、如果没有找到关键字，返回值为负的插入点值，所谓插入点值就是第一个比关键字大的元素在数组中的位置索引，而且这个位置索引从1开始。
            // 所以返回值为负值的情况是能够间接确定应当插入的位置，返回为第几个，转换成索引要减一
            if (bound < 0) {
                System.out.println(bound);
                bound = -bound - 1;
            }
            // 如果在范围内部的话，确认合法，更新ans
            if (bound <= n) {
                ans = Math.min(ans, bound - (i - 1));
            }
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}

```

### 69 .[211. 添加与搜索单词 - 数据结构设计](https://leetcode-cn.com/problems/design-add-and-search-words-data-structure/)

```java
class WordDictionary {
    class Trie {
        Trie[] children;
        boolean isEnd;
        public Trie(){
            children = new Trie[26];
            isEnd = false;
        }
    }

    /** Initialize your data structure here. */
    Trie root;
    public WordDictionary() {
        root = new Trie();

    }
    
    public void addWord(String word) {
        Trie node = this.root;
        int n = word.length();
        for(int i=0; i< n;i++){
            if(node.children[word.charAt(i) - 'a'] == null){
                node.children[word.charAt(i) - 'a'] = new Trie();
            }
            node = node.children[word.charAt(i) - 'a'];
        }
        node.isEnd = true;

    }
    
    public boolean search(String word) {
        return searchWord(word, this.root);

    }

    public boolean searchWord(String word, Trie root){
        Trie node = root;
        int n = word.length();
        for(int i=-0; i<n;i++){
            char c = word.charAt(i);
            if(c == '.'){
                for(int j=0;j<26;j++){
                    if(node.children[j] != null){
                        if(searchWord(word.substring(i+1), node.children[j])){
                            return true;
                        }
                    }
                }
                return false;
            }
            else if (c != '.' && node.children[c -'a'] == null) return false;
            else{
                node = node.children[c - 'a'];
            }
            
            
        }
        return node.isEnd;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */

// 也有另一种方法，更符合我的直觉： 以长度作为key，然后对比时，对'.'跳过
class WordDictionary {

  private Map<Integer, List<String>> maps;
        /** Initialize your data structure here. */
        public WordDictionary() {
            maps = new HashMap<>();
        }

        public void addWord(String word) {
            int key = word.length();
            if(maps.containsKey(key)) {
                List<String> valueList = maps.get(key);
                valueList.add(word);
            }else {
                List<String> value = new ArrayList<>();
                value.add(word);
                maps.put(key,value);
            }
        }

        public boolean search(String word) {
            int key = word.length();
            if(!maps.containsKey(key)) return false;
            List<String> valueList = maps.get(key);
            boolean flag = true;
            for (String value : valueList) {
                flag = true;
                for (int i = 0; i < value.length(); i++) {
                    char w = word.charAt(i);
                    if(w == '.') {
                        continue;
                    }else {
                        char v = value.charAt(i);
                        if(w == v) {
                            continue;
                        }else {
                            flag = false;
                            break;
                        }
                    }
                }
                if(flag) {
                    return true;
                }
            }
            return false;
        }
}


```

### 70 [223. 矩形面积](https://leetcode-cn.com/problems/rectangle-area/)

```java
class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        // 单纯模拟想复杂了，直接计算了两个矩形总面积- 一次重叠面积
        int S1 = (C-A)*(D-B);
        int S2 = (G-E)*(H-F);
        //无重叠， 当最左最右，或者最上最下都不重叠的时候，就没有重复的面积，具体可以画图标识
        if(B >= H || C <= E || D <= F || A >= G){
            return S1+S2;
        }
        //有重叠
        else{
            //重叠部分,上边界y，下边界y，左边界x，右边界x， 上、右取最小， 下、左取最大
            int upY = Math.min(D,H);
            int downY = Math.max(B,F);
            int rightX = Math.min(C,G);
            int leftX = Math.max(A,E);
            return S1+S2-(upY-downY)*(rightX-leftX);
        }
    }
}


```

### 71.[743. 网络延迟时间](https://leetcode-cn.com/problems/network-delay-time/)

```java
// 堆优化的dijstra算法
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        final int INF = Integer.MAX_VALUE/2;
        List<int[]>[] g = new List[n];
        for(int i=0;i<n; i++){
            g[i] = new ArrayList<int[]>();
        }
        for(int[] t : times){
            int x = t[0]-1, y = t[1] -1;
            g[x].add(new int[]{y, t[2]});
        }

        int[] dist = new int[n];
        Arrays.fill(dist, INF);
        dist[k-1]  = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<int[]>((a,b) -> a[0] != b[0] ? a[0] - b[0] : a[1] - b[1]);
        pq.offer(new int[]{0, k-1});
        while(!pq.isEmpty()){
            int[] p = pq.poll();
            int time = p[0], x= p[1];
            if(dist[x] < time){
                continue;
            }
            for(int[] e:g[x]){
                int  y = e[0], d = dist[x] + e[1];
                if(d < dist[y]){
                    dist[y] = d;
                    pq.offer(new int[]{d, y});
                }
            }
        }
        int res = Arrays.stream(dist).max().getAsInt();
        return res == INF ? -1 : res;

    }
}


class Solution {
    // flord 算法
    int N = 110, M = 6010;
    int[][] w = new int[N][N];
    int INF = 0x3f3f3f3f;
    int n,k;
    public int networkDelayTime(int[][] times, int _n, int _k) {
        n = _n;
        k = _k;
        // 初始化
        for(int i=1;i<= n; i++){
            for(int j = 1; j<=n; j++){
                w[i][j] = w[j][i] = i==j ? 0 : INF;
            }
        }
        for(int[] t : times){
            int u = t[0], v = t[1], c = t[2];
            w[u][v] = c;
        }
        // 标准模板， k松弛点， i，j为固定点
        
        for(int k=1; k<=n; k++){
            for(int i = 1; i<=n; i++){
                for(int j=1; j <= n; j++){
                    w[i][j] = Math.min(w[i][j], w[i][k] + w[k][j]);
                }
            }
        }
        int res =0;
        for(int i=1; i<=n; i++){
            res = Math.max(res,w[k][i]);
        }
        return res == INF ? -1 : res;

    }
}
```

### 72 .[307. 区域和检索 - 数组可修改](https://leetcode-cn.com/problems/range-sum-query-mutable/)

```java
// 树状数组解法，说实话还是不熟悉， 需要加紧
class NumArray {
    int[] tree;
    int lowbit(int x){
        return x& -x;
    }

    int query(int x){
        int res =0;
        for(int i=x ; i>0; i-=lowbit(i)) res += tree[i];
        return res;
    }

    void add(int x, int u){
        for(int i=x; i <= n;i+= lowbit(i)){
            tree[i] += u;
        }
    }
    int[] nums;
    int n;

    public NumArray(int[] _nums) {
        nums = _nums;
        n = nums.length;
        tree = new int[n+1];
        for(int i=0;i <n ;i++){
            add(i+1, nums[i]);
        }

    }
    
    public void update(int i, int val) {
        add(i+1, val - nums[i]);
        nums[i] = val;

    }
    
    public int sumRange(int left, int right) {
        return query(right+1) - query(left);

    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * obj.update(index,val);
 * int param_2 = obj.sumRange(left,right);
 */
```

### 73.[299. 猜数字游戏](https://leetcode-cn.com/problems/bulls-and-cows/)

```java
class Solution {
    public String getHint(String secret, String guess) {
        int[] unmatched = new int[10]; //遇到从secret来的未被匹配的字符，则做++动作。遇到从guess来的未被匹配的字符，则做--动作
        int n = secret.length();
        int bullsCount = 0;
        int cowsCount = 0;
        for (int i = 0; i < n; i++){
            char c_secret = secret.charAt(i);
            char c_guess = guess.charAt(i);
            if (c_secret != c_guess){
                if (unmatched[c_secret - '0'] < 0){ //对于c_secret而言，之前遇到过来自guess的未被匹配字符，那么就找到一组奶牛
                    cowsCount++;
                }
                unmatched[c_secret - '0']++; //按照桶的操作规则更新桶
                if (unmatched[c_guess - '0'] > 0){ //对于c_guess而言，之前遇到过来自secret的未被匹配字符，那么就找到一组奶牛
                    cowsCount++;
                }
                unmatched[c_guess - '0']--; //按照桶的操作规则更新桶
            }
            else{
                bullsCount++; //secret和guess匹配上了，找到一组公牛
            }
        }
        return bullsCount + "A" + cowsCount + "B";
    }
}
```

### 74 .[306. 累加数](https://leetcode-cn.com/problems/additive-number/)

```java
class Solution {
    /**
    本质时是递归回溯，一开始被题目吓到了 */
    /**
     * 字符串
     */
    String s;
    /**
     * 字符串的长度
     */
    int n;

    public boolean isAdditiveNumber(String num) {
        this.s = num;
        this.n = num.length();
        return toFlashBack(0, 0, 0, 0);
    }

    /**
     * @param index    当前的下标
     * @param sum      前两个数的和
     * @param previous 前一个数的值
     * @param count    已生成几个数
     */
    public boolean toFlashBack(int index, long sum, long previous, int count) {
        // 如果已生成三个数及以上则返回 true
        if (index == n) {
            return count >= 3;
        }
        // 拼接数字的值，值可能越 int 界所以使用 long
        long value = 0;
        // 开始拼接数字
        for (int i = index; i < n; i++) {
            // 除 0 以外，其他数字第一位不能为 0
            if (i > index && s.charAt(index) == '0') {
                break;
            }
            // 计算数值
            value = value * 10 + s.charAt(i) - '0';
            // 判断数值是否合法，如果前面有两个以上的数，则判断前两个数的和是否等于这个数
            if (count >= 2) {
                if (value < sum) {
                    // 小的话继续向后继续拼接
                    continue;
                } else if (value > sum) {
                    // 大的话直接结束，再往后拼接无意义
                    break;
                }
            }
            // 使用该数，向下递归
            if (toFlashBack(i + 1, previous + value, value, count + 1)) {
                return true;
            }
            // 没有可恢复原样的变量
        }
        return false;
    }
}


```

### 75 .[581. 最短无序连续子数组](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        // 中间的一段乱序数组，符合的特性是当前值比左端最大值小，比右端最小值大，
        // 在循环中，最后符合条件的，确定了左右边界
        int n = nums.length;
        int maxn = Integer.MIN_VALUE, right = -1;
        int minn = Integer.MAX_VALUE, left = -1;
        for (int i = 0; i < n; i++) {
            if (maxn > nums[i]) {
                right = i;
            } else {
                maxn = nums[i];
            }
            if (minn < nums[n - i - 1]) {
                left = n - i - 1;
            } else {
                minn = nums[n - i - 1];
            }
        }
        // 这里要考虑全数组有序的边界情况
        return right == -1 ? 0 : right - left + 1;
    }
}

```

### 76. [473. 火柴拼正方形](https://leetcode-cn.com/problems/matchsticks-to-square/)

```java
class Solution {
    public boolean makesquare(int[] nums) {
        int total = 0;
        for(int a: nums){
            total +=a;
        }
        if(total == 0 || total % 4 != 0){
            return false;
        }
        Arrays.sort(nums);
        return backtrace(nums, nums.length - 1, total >> 2, new int[4]);

    }
    private boolean backtrace(int[] nums, int idx, int target, int[]size){
        if(idx == -1){
            return (size[0] == size[1] && size[1] == size[2] && size[2] == size[3]);
        }
        for(int i=0; i<size.length; i++){
            // 如果出现大于target， 或者重复的值
            if(size[i] + nums[idx] > target || (i>0 && size[i] == size[i-1]) || (idx == nums.length -1 && i != 0)){
                continue;
            }
            size[i] += nums[idx];
            if(backtrace(nums, idx - 1, target, size)){
                return true;
            }
            size[i] -=nums[idx];
        }
        return false;
    }
}
```

### 77 .[377. 组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)

```java
class Solution {
    public int combinationSum4(int[] nums, int target) {
        int[] dp = new int[target+1];
        dp[0] = 1;
        for(int i=1; i<= target; i++){
            for(int num : nums){
                if(num <= i){
                    dp[i] += dp[i-num];
                }
            }
        }
        return dp[target];

    }
}
```

