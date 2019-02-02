## [724. Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，
    找到一個index，使得這個index左邊元素的和等於這個index右邊元素的和。
    如果沒有這種index，則回傳-1。
    如果很多符合條件的index，則回傳最小發生的index。
### 審題注意
    1. 如果nums是空陣列，回傳-1。
    2. 如果左邊或右邊沒有元素，元素和視為0。即 [1,0] 回傳的是 0。

    直觀上也是會想要先求個prefix-sum再比較兩邊的和，但還有更簡潔的方式：

    維護left, right兩個變數，遍歷一次nums然後看left的值和right的值是否相同。
### 解法
#### Python (local-var)
```python
def pivotIndex(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    left = 0
    right = sum(nums)
    
    for i in range(len(nums)):
        right -= nums[i]
        if left == right:
            return i
        left += nums[i]
    
    return -1
```

#### JavaScript
```javascript
var pivotIndex = function(nums) {
    let left = 0, right = nums.reduce((acc, cur) => acc + cur, 0)
    for(let i = 0; i < nums.length; ++i) {
        right -= nums[i]
        if(left === right) return i
        left += nums[i]
    }
    return -1
}
```
---
