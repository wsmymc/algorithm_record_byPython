##  kickstart

## python 输入输出样例

* python

  ```python
  import sys
  try: 
  	while True:
  		line = sys.stdin.readline().strip()
  		if line == '':
  			break 
  		lines = line.split()
  		print int(lines[0]) + int(lines[1])
  except: 
  	pass
  ```

* python3

  ```python
  import sys 
  for line in sys.stdin:
      a = line.split()
      print(int(a[0]) + int(a[1]))
  ```

  



## 2020 D

* A题

  ```
  Problem
  Isyana is given the number of visitors at her local theme park on N consecutive days. The number of visitors on the i-th day is Vi. A day is record breaking if it satisfies both of the following conditions:
  The number of visitors on the day is strictly larger than the number of visitors on each of the previous days.
  Either it is the last day, or the number of visitors on the day is strictly larger than the number of visitors on the following day.
  Note that the very first day could be a record breaking day!
  
  Please help Isyana find out the number of record breaking days.
  
  Input
  The first line of the input gives the number of test cases, T. T test cases follow. Each test case begins with a line containing the integer N. The second line contains N integers. The i-th integer is Vi.
  
  Output
  For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the number of record breaking days.
  
  Limits
  Time limit: 20 seconds per test set.
  Memory limit: 1GB.
  1 ≤ T ≤ 100.
  0 ≤ Vi ≤ 2 × 105.
  
  Test set 1
  1 ≤ N ≤ 1000.
  
  Test set 2
  1 ≤ N ≤ 2 × 105 for at most 10 test cases.
  For the remaining cases, 1 ≤ N ≤ 1000.
  
  Sample
  
  Input
   	
  Output
   
  4
  8
  1 2 0 7 2 0 2 0
  6
  4 8 15 16 23 42
  9
  3 1 4 1 5 9 2 6 5
  6
  9 9 9 9 9 9
  
    
  Case #1: 2
  Case #2: 1
  Case #3: 3
  Case #4: 0
  
    
  In Sample Case #1, the bold and underlined numbers in the following represent the record breaking days: 1 2 0 7 2 0 2 0.
  
  In Sample Case #2, only the last day is a record breaking day.
  ```

  * anylysis

    ```c++
    We can check whether the current element is the last element and, if not, if it is greater than the next element in constant time. To check whether i is a record breaking day, we also need to check whether the number of visitors of day i is greater than number of visitors from all the previous days.
    
    Test Set 1
    For each element j such that (1 <= j < i), check that the number of visitors on day j are less than number of visitors on day i. Hence, for each day we would compare it with all the previous days and it would take O(N) time. Therefore, for N days, the time complexity of this solution would be O(N^2).
    
    Test Set 2
    However that wouldn't be fast enough for Test Set 2, so we need a faster approach. Instead of comparing the number of visitors of day i against all the previous days one by one, we can compare the number of visitors of day i against the greatest number of visitors from all previous days. That reduces our processing time for each day from O(N) to O(1). Therefore, for N days, the time complexity of this solution would be O(N), which is sufficiently fast for both Test Set 1 and Test Set 2.
    
    Sample Code (C++)
    
    int countRecordBreakingDays(vector<int> visitors) {
      int recordBreaksCount = 0;
      int previousRecord = 0;
      for(int i = 0; i < checkpoints.size(); i++) {
         bool greaterThanPreviousDays = i == 0 || visitors[i] > previousRecord;
         bool greaterThanFollowingDay = i == checkpoints.size()-1 || visitors[i] > visitors[i+1];
         if(greaterThanPreviousDays && greaterThanFollowingDay) {
            recordBreaksCount++;
         }
         previousRecord = max(previousRecord, visitors[i]);
      }
      return recordBreaksCount;
    ```

  * answer

    ```python
    import sys
    
    '''
    try:
        while True:
            case_num = int(sys.stdin.readline().strip())
            for i in range(case_num):
                n = int(sys.stdin.readline().strip())
                row = int(sys.stdin.readline().strip().split())
                print(n, "\n", row, "\n")
    
                print("第", i+1, "轮")
    
    except:
        print("ss")
    '''
    # 最简单的思路，比较法统计
    a = int(input())
    
    for i in range(a):
        res = 0
        n = int(input())
        line = [int(x) for x in input().split()]
        ma = -1
        for j in range(len(line)):
            if line[j]>ma:
                if(j == len(line)-1 or line[j]>line[j+1]):  # 如果是最后一个，直接短路，+1。这里注意：没有关注第一个是因为第一个一定>0 ,but the original ma == -1,so,if the first > 2nd,就是一个破纪录，所以不需要特殊对待
                    res +=1
                ma = max(ma, line[j])
        print("Case #{0}: {1}".format(int(i+1), int(res)))
    
    
    
    ```
    
    