# JAVA刷题笔记



## 语法点



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



## mediium

## hard

