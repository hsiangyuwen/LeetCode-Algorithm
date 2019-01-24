## 624. Maximum Distance in Arrays (Need Premium Permission)
> :black_nib: Sammy
### 原題
    Given m arrays, and each array is sorted in ascending order.
    Now you can pick up two integers from two different arrays (each array picks one) and calculate the distance. 
    We define the distance between two integers a and b to be their absolute difference |a-b|. 
    Your task is to find the maximum distance.

    Example 1:
        Input: 
        [[1,2,3],
         [4,5],
         [1,2,3]]
        Output: 4

    Explanation: 
    One way to reach the maximum distance 4 is to： 
    pick 1 in the first or third array 
    and 
    pick 5 in the second array.

    Note:
    Each given array will have at least 1 number. 
    There will be at least two non-empty arrays.
    The total number of the integers in all the m arrays will be in the range of [2, 10000].
    The integers in the m arrays will be in the range of [-10000, 10000].

### 題目解釋
    input 是變數名為 arrays 的陣列，裏面有多個元素由小到大排序好的陣列。
    求從兩個不同的陣列各取一個數時，|num1-num2|的最大值。
### 審題注意
    重點在「從兩個不同的陣列各取一個數」。直觀上來說會遵循以下步驟：
    1. 記錄每個陣列最小值最大值
    2. 個別去比對，記錄住從哪個陣列來，以及算出來的距離值
    3. 拿從兩個不同的陣列各取一個數時，|num1-num2|的最大值。
    複雜度為O(n^2)
    
    實際上有將這樣的步驟簡化成O(n)並且一樣能求得最大值的方式：
    1. 記錄第0個陣列的最小值最大值
    2. 用第1個陣列的最小值最大值和第0個陣列的最小值最大值算距離(|0大-1小|與|1大-0小|)
    3. 更新距離最大值
    4. 更新紀錄起來的最小值最大值
    5. 重覆步驟2,3,4（第n個陣列和前一次更新完記錄起來的最大值最小值比對去算距離）
    這樣一定不會遇到去取同一列最大值和最小值計算出距離的情況。 
### 解法
#### Python3
```python
def maxDistance(self, arrays):
    m = len(arrays)
    distance = 0

    prev_max = arrays[0][-1]
    prev_min = arrays[0][0]
    for i in range(1, m):
        cur_max = arrays[i][-1]
        cur_min = arrays[i][0]

        distance = max(distance, abs(prev_max - cur_min), abs(cur_max - prev_min))

        prev_max = max(prev_max, cur_max)
        prev_min = min(prev_min, cur_min)

    return distance
```
---
