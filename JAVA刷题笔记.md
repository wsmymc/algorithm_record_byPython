# JAVA刷题笔记



## 语法点

1. https://leetcode-cn.com/circle/article/dnbYTt/
2. LinkedList遍历方法：[13,Java LinkedList遍历的7种方法 - 简书 (jianshu.com)](https://www.jianshu.com/p/46155da1e2cf)

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

