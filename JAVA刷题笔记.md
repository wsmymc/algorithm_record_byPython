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



### 2. [321. 拼接最大数](https://leetcode-cn.com/problems/create-maximum-number/)

```python
class Solution {
    public int[] maxNumber(int[] nums1, int[] nums2, int k) {
        int[] res = new int[0];
        // 从 nums1 中选出长 i 的子序列
        for (int i = 0; i <= k && i <= nums1.length; i++) {
            // 从 nums2 中选出长 k - i 的子序列
            if (k - i >= 0 && k - i <= nums2.length) {
                // 合并
                int[] tmp = merge(subMaxNumber(nums1, i), subMaxNumber(nums2, k - i));
                // 取最大值
                if (compare(tmp, 0, res, 0)) {
                    res = tmp;
                }
            }
        }
        return res;
    }

    // 类似于单调递减栈
    public int[] subMaxNumber(int[] nums, int len) {
        int[] subNums = new int[len];
        int cur = 0, rem = nums.length - len; // rem 表示还可以删去多少字符
        for (int i = 0; i < nums.length; i++) {
            while (cur > 0 && subNums[cur - 1] < nums[i] && rem > 0) {
                cur--;
                rem--;
            }
            if (cur < len) {
                subNums[cur++] = nums[i];
            } else {
                rem--; // 避免超过边界而少删字符
            }
        }
        return subNums;
    }

    public int[] merge(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length + nums2.length];
        int cur = 0, p1 = 0, p2 = 0;
        while (cur < nums1.length + nums2.length) {
            // python 可以字典序比较大小，这里需要专门写一个方法，内部使用递归
            if (compare(nums1, p1, nums2, p2)) { // 不能只比较当前值，如果当前值相等还需要比较后续哪个大
                res[cur++] = nums1[p1++];
            } else {
                res[cur++] = nums2[p2++];
            }
        }
        return res;
    }

    public boolean compare(int[] nums1, int p1, int[] nums2, int p2) {
        if (p2 >= nums2.length) return true;
        if (p1 >= nums1.length) return false;
        if (nums1[p1] > nums2[p2]) return true;
        if (nums1[p1] < nums2[p2]) return false;
        return compare(nums1, p1 + 1, nums2, p2 + 1);
    }
}



```



### 3. [992. K 个不同整数的子数组](https://leetcode-cn.com/problems/subarrays-with-k-different-integers/)

```java
class Solution {
    public int subarraysWithKDistinct(int[] A, int K) {
        int n = A.length;
        int[] num1 = new int[n + 1];
        int[] num2 = new int[n + 1];
        int tot1 = 0, tot2 = 0;
        int left1 = 0, left2 = 0, right = 0;
        int ret = 0;
        while (right < n) {
            if (num1[A[right]] == 0) {
                tot1++;
            }
            num1[A[right]]++;
            if (num2[A[right]] == 0) {
                tot2++;
            }
            num2[A[right]]++;
            while (tot1 > K) {
                num1[A[left1]]--;
                if (num1[A[left1]] == 0) {
                    tot1--;
                }
                left1++;
            }
            while (tot2 > K - 1) {
                num2[A[left2]]--;
                if (num2[A[left2]] == 0) {
                    tot2--;
                }
                left2++;
            }
            ret += left2 - left1;
            right++;
        }
        return ret;
    }
}

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/subarrays-with-k-different-integers/solution/k-ge-bu-tong-zheng-shu-de-zi-shu-zu-by-l-9ylo/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 4. [995. K 连续位的最小翻转次数](https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/)

```java
class Solution {
    public int minKBitFlips(int[] A, int K) {
        int n = A.length;
        int[] diff = new int[n+1];// 使用差分数组，多1位
        int res =0, revCnt=0;
        for(int i=0; i<n;i++){
            revCnt += diff[i];
            if ((A[i] + revCnt)%2 == 0 ){
                if (i+K-1 >=n){ // 注意这里要么是： i+K-1 >=n（i最大为n-1）， 要么转换为： i+K >n
                    return -1;
                }
                res++;
                revCnt++;
                diff[i+K]--;
            }
        }
        return res;


    }
}
```

### 5. [5688. 由子序列构造的最长回文串的长度](https://leetcode-cn.com/problems/maximize-palindrome-length-from-subsequences/)

```java
class Solution {
    public int longestPalindrome(String word1, String word2) {
        // 我就说这里应该可以直接求最长回文子串，不用向C++版本的高两趟
        int n1 = word1.length();
        int n2 = word2.length();
        int n = n1 + n2;
        String s = word1 + word2;
        int max = 0;
        // 在子串 s[i..j] 中，最长回文子序列的长度为 dp[i][j]
        int[][] dp = new int[n][n];
        for (int len = 1; len <= n; len++) {//区间长度
            for (int i = 0; i + len - 1 < n; i++) {//左端点
                int j = i + len - 1;//右端点
                if (len == 1) {
                    dp[i][j] = 1;
                }
                else {
                    if (s.charAt(i) == s.charAt(j)){
                        dp[i][j] = dp[i+1][j-1] + 2;
                    } else {
                        dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                    }
                }
                //需要保证非空,且si==sj时才满足条件 ，相较于正常的最长回文子串，区别在于这里
                // i ,j 分别在各自段落内，而且对应字符相等，只有这样才符合题意,也能过滤干扰项：
                /*
                例如：aaa    bb 两个， 算虽然aab子串有回文长度是2，可以登记在dp中，但是不符合筛选条件，所以不会记录在res中*/
                if (i < n1 && j >= n1 && s.charAt(i) == s.charAt(j)) {
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        return max;
    }
}

```

### 6. [224. 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

```java
//给你一个字符串表达式 s ，请你实现一个基本计算器来计算并返回它的值。 
//
// 
//
// 示例 1： 
//
// 
//输入：s = "1 + 1"
//输出：2
// 
//
// 示例 2： 
//
// 
//输入：s = " 2-1 + 2 "
//输出：3
// 
//
// 示例 3： 
//
// 
//输入：s = "(1+(4+5+2)-3)+(6+8)"
//输出：23
// 
//
// 
//
// 提示： 
//
// 
// 1 <= s.length <= 3 * 105 
// s 由数字、'+'、'-'、'('、')'、和 ' ' 组成 
// s 表示一个有效的表达式 
// 
// Related Topics 栈 数学 
// 👍 495 👎 0


import java.util.Deque;
import java.util.LinkedList;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public int calculate(String s) {
        int res =0, num=0, sign =1;
        // 这里统计的num都是前一个计算符的第二个对象，所以遇到下一个计算符，需要更新
        Deque<Integer> stack = new LinkedList<>();
        for (int i=0; i<s.length();i++){
            char c = s.charAt(i);
            if (Character.isDigit(c)){
                num = num*10 + s.charAt(i) - '0';
            } else if (c == '+' || c == '-') {
                res += sign * num; // 遇到下一个计算符，前面的计算，累计起来
                num = 0;// num待更新
                sign =(c=='+')?1:-1;
            }else if(c =='('){
                stack.addLast(res);
                stack.addLast(sign);
                res = 0;
                sign =1;
            }else if (c==')'){ // 最后不能用else，因为s中可能有空格，遇到空格不讨论，如果用else，可可能
                //报空指针异常，因为可能栈空
                res += sign*num;
                num =0 ;
                res *= stack.pollLast();
                res += stack.pollLast();
            }
        }
        res += sign * num;  //别忘记最后一个数字，上面的逻辑是遇到非数字时处理res，没有考虑到最后一个
        return res;

    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 7. [132. 分割回文串 II](https://leetcode-cn.com/problems/palindrome-partitioning-ii/)

```java
//给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文。 
//
// 返回符合要求的 最少分割次数 。 
//
// 
// 
// 
//
// 示例 1： 
//
// 
//输入：s = "aab"
//输出：1
//解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。
// 
//
// 示例 2： 
//
// 
//输入：s = "a"
//输出：0
// 
//
// 示例 3： 
//
// 
//输入：s = "ab"
//输出：1
// 
//
// 
//
// 提示： 
//
// 
// 1 <= s.length <= 2000 
// s 仅由小写英文字母组成 
// 
// 
// 
// Related Topics 动态规划 
// 👍 394 👎 0


import java.util.Arrays;

//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public int minCut(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        for(int i=0;i<n;i++){
            Arrays.fill(dp[i], true);
            /*
            * public static void fill(long[] a, long val) {
            *    for (int i = 0, len = a.length; i < len; i++)
                      a[i] = val;
                 }
            * */
        }
        for(int i=n-1;i>-1;i--){
            for (int j=i+1;j<n;j++){
                dp[i][j] = (s.charAt(i) == s.charAt(j)) && dp[i+1][j-1];
            }
        }
        int[] res = new int[n];
        Arrays.fill(res, Integer.MAX_VALUE);
        for (int i=0;i<n;i++){
            if (dp[0][i]){
                res[i]=0;
            }
            else{
                for (int j=0;j<i;j++){
                    if (dp[j+1][i]) {
                        res[i] = Math.min(res[i], res[j] + 1);
                    }
                }
            }
        }
//        for (boolean[] row:dp)
//        System.out.println(Arrays.toString(row));
        return res[n-1];

    }
}
//leetcode submit region end(Prohibit modification and deletion)

```

### 8. [1411. 给 N x 3 网格图涂色的方案数](https://leetcode-cn.com/problems/number-of-ways-to-paint-n-3-grid/)

```java
//你有一个 n x 3 的网格图 grid ，你需要用 红，黄，绿 三种颜色之一给每一个格子上色，且确保相邻格子颜色不同（也就是有相同水平边或者垂直边的格子颜
//色不同）。 
//
// 给你网格图的行数 n 。 
//
// 请你返回给 grid 涂色的方案数。由于答案可能会非常大，请你返回答案对 10^9 + 7 取余的结果。 
//
// 
//
// 示例 1： 
//
// 输入：n = 1
//输出：12
//解释：总共有 12 种可行的方法：
//
// 
//
// 示例 2： 
//
// 输入：n = 2
//输出：54
// 
//
// 示例 3： 
//
// 输入：n = 3
//输出：246
// 
//
// 示例 4： 
//
// 输入：n = 7
//输出：106494
// 
//
// 示例 5： 
//
// 输入：n = 5000
//输出：30228214
// 
//
// 
//
// 提示： 
//
// 
// n == grid.length 
// grid[i].length == 3 
// 1 <= n <= 5000 
// 
// Related Topics 动态规划 
// 👍 71 👎 0


//leetcode submit region begin(Prohibit modification and deletion)
class Solution {
    public int numOfWays(int n) {
        int mod = 1000000007;
        long fi0=6,fi1 = 6;
        for (int i=2;i<=n;i++){
            long tmp_0 = (2 * fi0 + 2* fi1) %mod;
            long tmp_1 = (2 * fi0 + 3* fi1) %mod;
            fi0 = tmp_0;
            fi1 = tmp_1;
        }
        return (int)((fi0 + fi1) % mod);

    }
}
//leetcode submit region end(Prohibit modification and deletion)
// 递推式：
class Solution {
    public int numOfWays(int n) {
        int mod = 1000000007;
        List<Integer> types = new ArrayList<>();
        for (int i=0;i<3;i++){
            for (int j=0; j<3;j++){
                for (int k=0;k<3;k++){
                    // 预处理出所有满足条件的 type
                    // 只要相邻的颜色不相同就行
                    // 将其以十进制的形式存储
                    if (i != j && j!=k) {
                        types.add(i * 100 + j * 10 + k);
                    }
                }
            }
        }
        int typeCnt = types.size();
        // 预处理出所有可以作为相邻行的 type 对,i,j可以相邻值为1
        int[][] match = new  int[typeCnt][typeCnt];
        for(int i=0;i<typeCnt;i++){
            // 得到 types[i] 三个位置的颜色
            int x1 = types.get(i)/100, x2 = (types.get(i)%100)/10, x3 = types.get(i)%10;
            for(int j=0; j<typeCnt;j++){
                int y1 = types.get(j)/100, y2 = (types.get(j))%100/10, y3 = types.get(j)%10;
                if(x1!=y1 && x2 != y2 && x3 != y3){
                    match[i][j]=1;
                }
            }
        }
        // 递推数组
        int[][] f = new int[n + 1][typeCnt];
        // 边界情况，第一行可以使用任何 type
        for(int i=0;i<typeCnt;i++){
            f[1][i] = 1;
        }
        for(int i=2;i<=n ;i++){
            for(int j=0;j<typeCnt;j++){
                for(int k=0;k<typeCnt;k++){
                    // f[i][j] 等于所有 f[i - 1][k] 的和
                    // 其中 k 和 j 可以作为相邻的行
                    if(match[j][k] ==1){
                        f[i][j] += f[i-1][k];
                        f[i][j] %=mod;
                    }
                }
            }
        }
        int res =0;
        for (int i=0;i<typeCnt;i++){
            res += f[n][i];
            res %=mod;
        }

        return res;
    }
}
```

### 9. [5704. 好子数组的最大分数](https://leetcode-cn.com/problems/maximum-score-of-a-good-subarray/)

```Java
class Solution {
    public int maximumScore(int[] heights, int k) {
  		int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];

        Stack<Integer> mono_stack = new Stack<Integer>(); // 栈容器
        for (int i = 0; i < n; ++i) {
            while (!mono_stack.isEmpty() && heights[mono_stack.peek()] >= heights[i]) {
                mono_stack.pop();
            }
            left[i] = (mono_stack.isEmpty() ? -1 : mono_stack.peek());
            mono_stack.push(i);
        }

        mono_stack.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!mono_stack.isEmpty() && heights[mono_stack.peek()] >= heights[i]) {
                mono_stack.pop();
            }
            right[i] = (mono_stack.isEmpty() ? n : mono_stack.peek());
            mono_stack.push(i);
        }

        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if(right[i]>k&& left[i]<k)ans = Math.max(ans, (right[i] - left[i] - 1) * heights[i]);
        }
        return ans;
    }
}

作者：chen-wen-liang
链接：https://leetcode-cn.com/problems/maximum-score-of-a-good-subarray/solution/84-zhu-zhuang-tu-zhong-zui-da-de-ju-xing-42xm/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 10 .149. 直线上最多的点数](https://leetcode-cn.com/problems/max-points-on-a-line/)

```java
class Solution {
    public int maxPoints(int[][] points) {
        int n = points.length;
        // 极小边界
        if(n<=2){
            return n;
        }
        int res = 0;
        for(int i=0; i<n;i++){
            // 当前最大的值 >= 剩下未统计的的点（因为最多剩下的点共线），或者大于一半的点（原因相同），直接返回
            if(res >= n- i || res >n /2){
                break;
            }
            Map<Integer,Integer> map = new HashMap<>();
            for(int j = i+1;j<n;j++){
                // 固定一点，遍历之后的所有点
                int x = points[i][0] - points[j][0];
                int y = points[i][1] - points[j][1];
                // 极端斜率
                if(x==0){
                    y = 1;
                }
                else if(y ==0){
                    x = 1;
                }
                else{
                    // 规定y非负
                    if (y<0){
                        x = -x;
                        y = -y;
                    }
                    // 分子式最简化
                    int gcdXY = gcd(Math.abs(x), Math.abs(y));
                    x /= gcdXY;
                    y /=gcdXY;
                }
                // 以点i为基点， 与j构造成相同斜率的次数统计在map中，其实就是共线了。这里的100000，是为了避免可能的重合
                int key = y + x * 100000;
                map.put(key, map.getOrDefault(key, 0) +1);
            }
            int max = 0;
            //
            for(Map.Entry<Integer, Integer> pair : map.entrySet()){
                int num = pair.getValue();
                max = Math.max(num + 1, max);
            }
            res = Math.max(max, res);
        }
        return res;

    }
    // 辗转相除求最大公约数
    public int gcd(int a, int b){
        return b == 0 ? a: gcd(b, a%b);
    }
}
```

### 11.[5783. 设计电影租借系统](https://leetcode-cn.com/problems/design-movie-rental-system/)

```java
class MovieRentingSystem {
    private int n;
    List<Set<Integer>> movieSetList; // 每个商店未借出电影
    List<Map<Integer, Integer>> moviePriceList; // 每个商店电影价格；
    Map<Integer, TreeSet<Movie>> searchTreeSetMap; // 每个商店的电影堆；
    TreeSet<Movie> reportTreeSet; // 总的已经借出电影（shop, movie ,proce）

    

    public MovieRentingSystem(int n, int[][] entries) {
        this.n = n;
        movieSetList = new ArrayList<>();
        moviePriceList = new ArrayList<>();
        searchTreeSetMap = new HashMap<>();
        reportTreeSet = new TreeSet<Movie>((o1,o2) -> {
            if(o1.price != o2.price) return o1.price -o2.price;
            if(o1.shop != o2.shop) return o1.shop -o2.shop;
            return o1.movie - o2.movie;
        });
        for(int i=0;i<n;i++){
            movieSetList.add(new HashSet<>());
            moviePriceList.add(new HashMap<>());
        }
        for(int i=0;i<entries.length;i++){
            int shop = entries[i][0];
            int movie = entries[i][1];
            int price = entries[i][2]; 
            movieSetList.get(shop).add(movie);
            moviePriceList.get(shop).put(movie, price);
            // 针对每个电影，存入所有的拷贝信息,如果该电影不存在，需要初始化
            if(!searchTreeSetMap.containsKey(movie)){
                searchTreeSetMap.put(movie, new TreeSet<Movie>((o1,o2) ->{
                    return o1.price == o2.price ? o1.shop -o2.shop : o1.price - o2.price;
                }));
            }
            searchTreeSetMap.get(movie).add(new Movie(shop, movie, price));

        }


    }
    
    public List<Integer> search(int movie) {
        if(!searchTreeSetMap.containsKey(movie)) return new ArrayList<>();
        List<Integer> res = new ArrayList<>();
        TreeSet<Movie> set = searchTreeSetMap.get(movie);
        int i =0 ;
        for(Movie m : set){
            if(i>=5) break;
            res.add(m.shop);
            i++;
        }
        return res;

    }
    
    public void rent(int shop, int movie) {
        // 借出，需要更新相关集合
        int price = moviePriceList.get(shop).get(movie);
        movieSetList.get(shop).remove(movie);
        reportTreeSet.add(new Movie(shop, movie, price));
        if(searchTreeSetMap.containsKey(movie)){
            searchTreeSetMap.get(movie).remove(new Movie(shop, movie, price));
        }

    }
    
    public void drop(int shop, int movie) {
        // 返回，逆向更新
 int price = moviePriceList.get(shop).get(movie);
        movieSetList.get(shop).add(movie);
        reportTreeSet.remove(new Movie(shop, movie, price));
        if(searchTreeSetMap.containsKey(movie)){
            searchTreeSetMap.get(movie).add(new Movie(shop, movie, price));
        }

    }
    
    public List<List<Integer>> report() {
        List<List<Integer>> res = new ArrayList<>();
        int i=0;
        for(Movie m: reportTreeSet){
            if(i >=5) break;
            List<Integer> tmp = Arrays.asList(m.shop, m.movie);
            res.add(tmp);
            i++;
        }
        return res;

    }


    class Movie {
        int shop;
        int movie;
        int price;

        Movie(int a, int b,int c){
            this.shop =a;
            this.movie = b;
            this.price = c;
        }
        @Override
        public boolean equals(Object o){
            if(this == o) {
                return true;
            }
            if(o == null || getClass() != o.getClass()) return false;
            Movie m = (Movie) o;
            return m.shop == shop && m.movie == movie && m.price == price;
        }

        @Override
        public int hashCode(){
            return Objects.hash(shop, movie, price);
        }
    }
}

/**
 * Your MovieRentingSystem object will be instantiated and called as such:
 * MovieRentingSystem obj = new MovieRentingSystem(n, entries);
 * List<Integer> param_1 = obj.search(movie);
 * obj.rent(shop,movie);
 * obj.drop(shop,movie);
 * List<List<Integer>> param_4 = obj.report();
 */

//优先队列写法
class MovieRentingSystem {
    Map<Integer,PriorityQueue<int[]>>movies=new HashMap<>();
    Set<int[]>set=new HashSet<>();
    PriorityQueue<int[]>jie=new PriorityQueue<>(new Comparator<int[]>() {
        @Override
        public int compare(int[] o1, int[] o2) {
            if(o1[2]!=o2[2])return o1[2]-o2[2];
            else if(o1[1]!=o2[1])return o1[1]-o2[1];
            else return o1[0]-o2[0];
        }
    });
    Map<Integer,int[]>[]shops;
    public MovieRentingSystem(int n, int[][] entries) {
        shops=new Map[n];
        for(int i=0;i<n;i++)
            shops[i]=new HashMap();
        for(int i=0;i<entries.length;i++)
        {
            int[]tem=entries[i];
            if(movies.get(tem[1])==null)movies.put(tem[1],new PriorityQueue<>(new Comparator<int[]>() {
                @Override
                public int compare(int[] o1, int[] o2) {
                    if(o1[2]!=o2[2])
                        return o1[2]-o2[2];
                    else return o1[0]-o2[0];
                }
            }));
            movies.get(tem[1]).add(tem);
            shops[tem[0]].put(tem[1],tem);
        }
    }
    public List<Integer> search(int movie) {
        int num=5;
        List<Integer>list=new ArrayList<>();
        if(movies.get(movie)==null)return list;
        PriorityQueue<int[]>tem=(movies.get(movie));
        Stack<int[]>stack=new Stack<>();
        while (!tem.isEmpty()&&num>0)
        {
            int[]arr=tem.poll();
            stack.add(arr);
            if(!set.contains(arr)) {
                list.add(arr[0]);
                num--;
            }
        }

        while (!stack.isEmpty())
            tem.add(stack.pop());
        return list;
    }

    public void rent(int shop, int movie) {
        int[]tem=shops[shop].get(movie);
        set.add(tem);
        jie.add(tem);
    }

    public void drop(int shop, int movie) {
        int[]tem=shops[shop].get(movie);
        set.remove(tem);
        jie.remove(tem);
    }

    public List<List<Integer>> report() {
        int num=5;
        PriorityQueue<int[]>tem=jie;
        List<List<Integer>>list=new ArrayList<>();
        Stack<int[]>stack=new Stack<>();
        while (!tem.isEmpty()&&num>0)
        {
            int[]arr=tem.poll();
            stack.add(arr);
                List<Integer> ll = new ArrayList<>();
                ll.add(arr[0]);
                ll.add(arr[1]);
                list.add(ll);
                num--;
        }
        while (!stack.isEmpty())
            jie.add(stack.pop());
        return list;
    }
}

作者：xiaoshuaila
链接：https://leetcode-cn.com/problems/design-movie-rental-system/solution/javadai-ma-you-xian-dui-lie-by-xiaoshuai-u79l/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 12. [1872. 石子游戏 VIII](https://leetcode-cn.com/problems/stone-game-viii/)

```java
class Solution {
    public int stoneGameVIII(int[] stones) {
        int n = stones.length;
        int[] dp = new int[n+1];
        int[] sum = new int[n+1];
        // 从任何一个地方取值，值不会变，所以先搞前缀和
        for(int i =1;i<=n;i++){
            sum[i] = sum[i-1] + stones[i-1];
        }
        dp[n] = sum[n]; // 这里相当于最后全部都拿到，因为是倒序，所以要有、
        // 题目说的两种情况，其实都是在说，自己减去对方最大。所以是从这里取全部的前缀的和和对方取之后获得的最大值的插值
        for(int i= n-1;i>1;i--){
            dp[i] = Math.max(dp[i+1], sum[i] -dp[i+1]);
        }
        return dp[2];

    }
}
```

### 13.[剑指 Offer 37. 序列化二叉树](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

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
public class Codec {
    public String serialize(TreeNode root) {
        if(root == null) return "[]";
        StringBuilder res = new StringBuilder("[");
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(node != null) {
                res.append(node.val + ",");
                queue.add(node.left); // 如果为空会返回null，加入，否则为值，逐层加入
                queue.add(node.right);
            }
            else res.append("null,");
        }
        res.deleteCharAt(res.length() - 1);
        res.append("]");
        return res.toString();
    }

    public TreeNode deserialize(String data) {
        if(data.equals("[]")) return null;
        String[] vals = data.substring(1, data.length() - 1).split(",");
        TreeNode root = new TreeNode(Integer.parseInt(vals[0]));
        Queue<TreeNode> queue = new LinkedList<>() {{ add(root); }};
        int i = 1;
        while(!queue.isEmpty()) {
            TreeNode node = queue.poll();
            if(!vals[i].equals("null")) {
                node.left = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.left);
            }
            i++;
            if(!vals[i].equals("null")) {
                node.right = new TreeNode(Integer.parseInt(vals[i]));
                queue.add(node.right);
            }
            i++;
        }
        return root;
    }
}


// Codec codec = new Codec();
// codec.deserialize(codec.serialize(root));
```



### 14. [726. 原子的数量](https://leetcode-cn.com/problems/number-of-atoms/)

```java
class Solution {
    // 全局变量复用
    int i, n;
    String formula;

    public String countOfAtoms(String formula) {
        this.i = 0;
        this.n = formula.length();
        this.formula = formula;
        // 字符串存储每一层的原子和个数
        Deque<Map<String, Integer>> stack = new LinkedList<Map<String, Integer>>();
        // 先初始一个最外层，作为计算统计
        stack.push(new HashMap<String, Integer>());
        while (i < n) {
            char ch = formula.charAt(i);
            // 左括号，新的一个哈希表
            if (ch == '(') {
                i++;
                stack.push(new HashMap<String, Integer>()); // 将一个空的哈希表压入栈中，准备统计括号内的原子数量
                // 右括号，解析内部，同时考虑后缀数字
            } else if (ch == ')') {
                i++;
                int num = parseNum(); // 括号右侧数字
                Map<String, Integer> popMap = stack.pop(); // 弹出括号内的原子数量
                Map<String, Integer> topMap = stack.peek();
                for (Map.Entry<String, Integer> entry : popMap.entrySet()) {
                    String atom = entry.getKey();
                    int v = entry.getValue();
                    // 将本层的结果放到次外层中
                    topMap.put(atom, topMap.getOrDefault(atom, 0) + v * num); // 将括号内的原子数量乘上 num，加到上一层的原子数量中
                }
            } else {
                // 平时对原子和个数的计算
                String atom = parseAtom();
                int num = parseNum();
                // 将结果放在当前层中
                Map<String, Integer> topMap = stack.peek();
                topMap.put(atom, topMap.getOrDefault(atom, 0) + num); // 统计原子数量
            }
        }

        // 循环完毕，这里只由最外层用于计算的一层
        Map<String, Integer> map = stack.pop();
        // 使用treemap，可以保证是按照字母是字典序的
        TreeMap<String, Integer> treeMap = new TreeMap<String, Integer>(map);

        StringBuffer sb = new StringBuffer();
        for (Map.Entry<String, Integer> entry : treeMap.entrySet()) {
            String atom = entry.getKey();
            int count = entry.getValue();
            sb.append(atom);
            if (count > 1) {
                sb.append(count);
            }
        }
        return sb.toString();
    }

    public String parseAtom() {
        StringBuffer sb = new StringBuffer();
        sb.append(formula.charAt(i++)); // 扫描首字母
        // 判断是否为小写，如果是，继续加入
        while (i < n && Character.isLowerCase(formula.charAt(i))) {
            sb.append(formula.charAt(i++)); // 扫描首字母后的小写字母
        }
        return sb.toString();
    }

    public int parseNum() {
        // Character 判断是否为数字的方法isDigit
        if (i == n || !Character.isDigit(formula.charAt(i))) {
            return 1; // 不是数字，视作 1
        }
        int num = 0;
        // 这是考虑了不止一位数字
        while (i < n && Character.isDigit(formula.charAt(i))) {
            num = num * 10 + formula.charAt(i++) - '0'; // 扫描数字
        }
        return num;
    }
}

```

### 15. [1923. 最长公共子路径](https://leetcode-cn.com/problems/longest-common-subpath/)

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

