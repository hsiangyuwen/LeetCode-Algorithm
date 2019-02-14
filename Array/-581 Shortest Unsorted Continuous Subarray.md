
## [581 Shortest Unsorted Continuous Subarray](https://leetcode.com/problems/shortest-unsorted-continuous-subarray/)
> :black_nib: Ray
### 題目解釋
	找到需重新排序的最短 subarray 長度，使整個 array 符合升冪排序 (ascending order)。
### 審題注意
    1. 先 sort 好，再比對與原陣列的不同。(Time: O(nlgn), Space: O(n))
    2. 是否有 two or one pass solution?(Time: O(n), Space: O(1))
### 解法
#### JavaScript (One-pass)
```javascript
var findUnsortedSubarray = function(nums) {
    let len = nums.length,
        start = -1,
        end = -2,
        min = nums[len - 1],
        max = nums[0]
    for(let i = 0; i < len; ++i) {
        min = Math.min(min, nums[len - i - 1])
        max = Math.max(max, nums[i])
        if(nums[len - i - 1] > min) start = len - i - 1
        if(nums[i] < max) end = i
    }
    return end - start + 1
}
```
- 解釋：
 1. 舉例：`nums` = [2, 6, 4, 8, 10, 9, 15]	
	```
		i | min  max start end
		0 | 15    2   -1   -2
		1 |  9    6   -1   -2
		2 |  9    6    4    2
		3 |  8    8    4    2
		4 |  4   10    4    2
		5 |  4   10    1    5
		6 |  2   15    1    5
	```
 
 2. 由於是 one-pass solution，可以想像成雙向查找，此外，重要的大原則是：一個升冪排序的陣列，末端是最大值，開端是最小值。從末端查找，索引上的值必是末端到該索引的最小值，如果不是則紀錄在 `start`，最後代表 `[start...len - 1]` 中，在`start`索引上的值，不是此陣列的最小值；從起始端查找，索引上得值必是起始端到該索引的最大值，不是則紀錄在 `end` ，最終 `[0...end]`中在 `end` 索引上的值不是此陣列的最大值。
 
 3. **找到 `start` 和 `end` 的意義何在？** `start` 可以想成需要調整的開端，`end` 則是結束，因為 `start` 索引上的值一定要調整才能符合升冪排序，`end` 亦是。
 
 4. **為什麼 `start` 初始值是 `-1`，`end` 是 `-2`？** 這是為整個陣列已經是升冪排序時設的特例，在該情況下，`start` 及 `end` 不會被更新，在最後 `end - start + 1` 會是 `0`，代表 unsorted subarray 長度為 0。

#### Python3 (One-pass)
```python
def findUnsortedSubarray(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    start = end = None
    minimum = nums[-1]
    maximum = nums[0]
    
    for i in range(len(nums)):
        minimum = min(minimum, nums[-1 - i])
        maximum = max(maximum, nums[i])
        if nums[-1 - i] > minimum:  # minimum is modified
            start = len(nums) - 1 - i
        if nums[i] < maximum:  # maximum is modified
            end = i
    
    if (start and end) is None:  # original list is already sorted
        return 0

    return end - start + 1
```
---
