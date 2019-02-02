## [747. Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，裡面只有一個最大的數。
    如果這個最大的數都是其他的數的兩倍(含)以上，就回傳該數的index。
    如果不符合以上條件，就回傳-1。
### 審題注意
    這題和2nd-largest題相似。
    
    先記下最大數的index（O(n)），再維護一個max_heap得到第二大的數（O(n)）做比較。
### 解法
#### Python3 (heap)
##### tags: `heap`
```python
def dominantIndex(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    if len(nums) == 1:
        return 0
    
    H = nums[:2]  # First 2 element.
    heapq.heapify(H)
    
    # Maintain a size == 2 heap.
    # H[0] is 2nd_largest
    # H[1] is largest
    for i in range(2, len(nums)):
        if nums[i] >= H[0]:
            heapq.heappushpop(H, nums[i])
    
    return nums.index(H[1]) if H[1] >= 2 * H[0] else -1
```

#### JavaScript
```javascript
var dominantIndex = function(nums) {
    let max1 = -1, max2 = -1, maxIndex = -1
    for(let i = 0; i < nums.length; ++i) {
        if(nums[i] > max1) {
            [max1, max2] = [nums[i], max1]
            maxIndex = i
        } else if (nums[i] > max2)
            max2 = nums[i]
    }
    return max1 >= max2 * 2 ? maxIndex : -1
}
```
---
