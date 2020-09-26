##  kickstart

* 参考题解：
* https://cp-wiki.vercel.app/tutorial/kick-start/2020E/#problem-b-high-buildings

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

### A题

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
  









* B

* ```
  Problem
  An alien has just landed on Earth, and really likes our music. Lucky for us.
  
  The alien would like to bring home its favorite human songs, but it only has a very strange instrument to do it with: a piano with just 4 keys of different pitches.
  
  The alien converts a song by writing it down as a series of keys on the alien piano. Obviously, this piano will not be able to convert our songs completely, as our songs tend to have many more than 4 pitches.
  
  The alien will settle for converting our songs with the following rules instead:
  The first note in our song can be converted to any key on the alien piano.
  For every note after,
  if its pitch is higher than the previous note, it should be converted into a higher-pitched key than the previous note's conversion;
  if lower, it should be converted into a lower-pitched key than the previous note's conversion;
  if exactly identical, it should be converted into the same key as the previous note's conversion.
  Note: two notes with the same pitch do not need to be converted into the same key if they are not adjacent.
  
  What the alien wants to know is: how often will it have to break its rules when converting a particular song?
  
  To elaborate, let us describe one of our songs as having K notes. The first note we describe as "note 1", the second note "note 2", and the last note "note K."
  So note 2 comes immediately after note 1.
  Now if note 2 is lower than note 1 in our version of the song, yet converted to an equally-pitched or lower-pitched key (relative to note 2's conversion) in the alien's version of the song, then we consider that a single rule break.
  For each test case, return the minimum amount of times the alien must necessarily break one of its rules in converting that song.
  
  Input
  The first line of the input gives the number of test cases, T. T test cases follow.
  Each test case consists of two lines.
  The first line consists of a single integer, K.
  The second line consists of K space-separated integers, A1, A2 ... AK, where Ai refers to the pitch of the i-th note for this test case.
  
  Output
  For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the minimum number of times that particular test case will require the alien to break its own rules during the conversion process.
  
  Limits
  Memory limit: 1GB.
  1 ≤ T ≤ 100.
  1 ≤ Ai ≤ 106.
  
  Test set 1
  Time limit: 20 seconds.
  1 ≤ K ≤ 10.
  
  Test set 2
  Time limit: 40 seconds.
  1 ≤ K ≤ 104.
  
  Sample
  
  Input
   	
  Output
   
  2
  5
  1 5 100 500 1
  8
  2 3 4 5 6 7 8 9
  
    
  Case #1: 0
  Case #2: 1
  
    
  We will use the notation A, B, C, D for the alien piano keys where A is the lowest note, and D is the highest. In sample case #1, the alien can simply map our song into the following sequence: A B C D C and this correctly reflects all the following:
  our first note with pitch 1 maps to A,
  our second note with pitch 5 maps to its key B. 5 > 1, and B is a higher key than A,
  our third note with pitch 100 maps to its key C. 100 > 5, and C is a higher key than B,
  our fourth note with pitch 500 maps to its key D. 500 > 100, and D is a higher key than C,
  our fifth note with pitch 1 maps to its key C. 1 < 500, and C is a lower key than D.
  So none of the rules are broken. Note: A B C D C is not the only way of conversion. A B C D A or A B C D B are also eligible conversions.
  
  In sample case #2, the only conversion sequence that provides the minimal result of 1 rule broken is: A B C D A B C D. Notably, the rule break comes from the fact that our 4th note with pitch 4 is lower than our 5th note with pitch 5, but A is a lower key than D.
  
  
  ```

* #### Analysis

  

  ### Test Set 1

  For the small test set, one can generate all possible conversions recursively and select the one with the smallest number of rule violations as the answer. Since each note can be converted to four possible notes in the alien scale, this results into 4^**K** combinations to be tested and O(4^**K**) time complexity.

  ### Test Set 2

  #### Dynamic Programming Solution

  Let DP[i, j] be the minimum number of rule violations required to convert the first i notes **A1**, **A2**, ..., **Ai** such that the i-th note **Ai** is converted to note j of the alien piano (1 ≤ j ≤ 4). Then the answer is the minimum DP[**K**, j] over all j, 1 ≤ j ≤ 4.

  The table DP[i, j] can be computed using dynamic programming as follows.
  (1) DP[1, j] = 0 for all j.
  (2) For i > 1, DP[i, j] = min{ DP[i - 1, j'] + P(i, j', j) | 1 ≤ j' ≤ 4 }. Here P(i, j', j) is a binary penalty term accounting for a rule violation between the notes **Ai-1** and **Ai**. For example, if **Ai-1** > **Ai**, then P(i, j', j) = 1 whenever j' ≤ j and P(i, j', j) = 0 otherwise.

  Since each entry of the table is calculated using a constant number of operations, the overall time complexity of the algorithm is O(**K**), which is okay to pass the large test set.

  #### Greedy Solution

  Let us say that a sequence of pitches is *playable* if it can be converted to the alien piano notes without violating any rules. Our goal here is to split the given sequence of pitches into as few playable fragments as possible.

  We can take the longest playable prefix of the sequence as the first fragment of the split. In this way, the remainder of the sequence is as short as possible, and therefore requires potentially fewer rule violations.

  Now let us characterise the playable sequences. Note that repeated notes of the same pitch do not affect the playability of the sequence, therefore, without loss of generality, suppose that any two consecutive notes are at a different pitch. Let us say that two consecutive notes form an *upward step* if the second note has a higher pitch than the first. Otherwise, we call it a *downward step*.

  Clearly, a sequence of notes is not playable if it contains more than three consecutive upward or downward steps, as we would run out of the alien scale. Otherwise, the sequence is playable and we can convert it using this simple strategy (assuming the note names A, B, C, and D of the alien piano from the lowest to the highest note):
  (1) If the first step is upward, convert the first note to A.
  (2) If the first step is downward, convert the first note to D.
  (3) If three consecutive notes form an upward step followed by a downward step, convert the second note to D.
  (4) If three consecutive notes form a downward step followed by an upward step, convert the second note to A.
  (5) In all other cases, convert a note one step higher or lower than the note before depending on whether they form an upward or downward step.
  Since any maximal sequence of upward steps starts at A and has no more than three steps (and similarly for any maximal sequence of downward steps), we will never leave the alien scale.

  The following diagram illustrates the process of splitting the sequence (1, 8, 9, 7, 6, 5, 4, 3, 2, 1, 3, 2, 1, 3, 5, 7) into two playable fragments. Since the subsequence (9, 7, 6, 5, 4) consists of four downward steps, the sequence needs to be split between notes 5 and 4.

  ![img](https://codejam.googleapis.com/dashboard/get_file/AQj_6U3H98Zj8WOikpEByfbS5Yxi7oOTQRes3itHzkWY709Us2HIl7__O_x0Z4iPBLST8Q/song.png)

  According to the analysis above, a single pass through the given sequence of notes maintaining the current number of consecutive upward and downward steps results in an O(**K**) solution, where **K** is the number of notes in the sequence. As soon as the number of upward or downward steps exceeds 3, we have to split the sequence by violating the rules and start a new fragment. A Python code snippet is included below for clarity.

  ```
  def solve():
    k = input()
    a = map(int, raw_input().split())
    # Filter out repeated notes.
    a = [a[i] for i in xrange(0, k) if i == 0 or a[i - 1] != a[i]]
    upCount = 0
    downCount = 0
    violations = 0
    for i in xrange(1, len(a)):
      if a[i] > a[i - 1]:
        upCount += 1
        downCount = 0
      else:
        downCount += 1
        upCount = 0
      if upCount > 3 or downCount > 3:
        violations += 1
        upCount = downCount = 0
    return violations
  ```

* 

```
4
4 1 3 1
4 4 4 3
5 3 3 2
6 3 4 4
1
```







## 2020 E

### A题（赛时已解决）

```python
# 没什么好说的，就是干。只是当时读题不仔细，以为是绝对值，浪费了时间
t= int(input())

for j in range(t):
    n=int(input())
    l=[int(x) for x in input().split()]
    #print("case",i)
    gap =(l[0]-l[1])
    flag =True
    res =0
    start=0
    for i in range(n-1):
        if (l[i]-l[i+1]) !=gap:
            #print("start", start, "end", i)
            res = max(res,(i-start+1))
            start =i
            gap= (l[i]-l[i+1])
        else:
            if i == n-2:

                #print("-1:  start",start,"end",i)
                res=max(res,(n-1-start+1))
            else:
                pass
    print("Case #{0}: {1}".format(j+1,res))
```

### B题

```python
Problem
In an unspecified country, Google has an office campus consisting of N office buildings in a line, numbered from 1 to N from left to right. When represented in meters, the height of each building is an integer between 1 to N, inclusive.

Andre and Sule are two Google employees working in this campus. On their lunch break, they wanted to see the skyline of the campus they are working in. Therefore, Andre went to the leftmost point of the campus (to the left of building 1), looking towards the rightmost point of the campus (to the right of building N). Similarly, Sule went to the rightmost point of the campus, looking towards the leftmost point of the campus.

To Andre, a building x is visible if and only if there is no building to the left of building x that is strictly higher than building x. Similarly, to Sule, a building x is visible if and only if there is no building to the right of building x that is strictly higher than building x.

Andre learned that there are A buildings that are visible to him, while Sule learned that there are B buildings that are visible to him. After they regrouped and exchanged information, they also learned that there are C buildings that are visible to both of them.

They are wondering about the height of each building. They are giving you the value of N, A, B, and C for your information. As their friend, you would like to construct a possible height for each building such that the information learned on the previous paragraph is correct, or indicate that there is no possible height construction that matches the information learned (thus at least one of them must have been mistaken).

Input
The first line of the input gives the number of test cases, T. T test cases follow. Each consists of a single line with four integers N, A, B, and C: the information given by Andre and Sule.

Output
For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is IMPOSSIBLE if there is no possible height for each building according to the above information, or N space-separated integers otherwise. The i-th integer in y must be the height of the i-th building (in meters) between 1 to N.

Limits
Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
1 ≤ C ≤ N.
C ≤ A ≤ N.
C ≤ B ≤ N.

Test Set 1
1 ≤ N ≤ 5.

Test Set 2
1 ≤ N ≤ 100.

Sample

Input
 	
Output
 
3
4 1 3 1
4 4 4 3
5 3 3 2

  
Case #1: 4 1 3 2
Case #2: IMPOSSIBLE
Case #3: 2 1 5 5 3

  
In Sample Case #1, the sample output sets the height of each building such that only the first building is visible to Andre, while the first, third, and fourth buildings are visible to Sule. Therefore, only the first building is visible to both Andre and Sule. Note that there exist other correct solutions, such as 4 3 1 2.

In Sample Case #2, all N = 4 buildings are visible to Andre and Sule. Therefore, it is impossible to have C ≠ N in this case.

In Sample Case #3, the sample output sets the height of each building such that the first, third, and fourth buildings are visible to Andre, while the third, fourth, and fifth buildings are visible to Sule. Therefore, the third and fourth buildings are visible to both Andre and Sule. Note that there exist other correct solutions.
```

```python
# 比赛时没有考虑全面，应该将公共部分分成两份，中间夹杂AB都看不到的部分
# 同时，应该考虐AB的大小关系，如果A>B那么输出结果的时候，需要倒着来，因为默认的方案是按照A<B考虑的
# 说白了还是素质不够，经验少
t = int(input())
for case_num in range(1, t + 1):
    n, a, b, c = map(int, input().split())
    if a + b - c > n or (a == b and a == c and c < min(n, 2)):
        print("Case #{}: IMPOSSIBLE".format(case_num))
        continue
    rev = False
    if a > b:
        tmp = a
        a = b
        b = tmp
        rev = True
    arrangement = [n - 1] * (a - c) + [n] + [1] * (n - (a + b - c)) + \
        [n] * (c - 1) + [n - 1] * (b - c)
    if rev:
        arrangement = arrangement[::-1]
    ans = ' '.join(map(str, arrangement))
    print("Case #{}: {}".format(case_num, ans))
```



