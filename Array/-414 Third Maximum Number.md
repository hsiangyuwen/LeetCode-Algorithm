## [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)
> :black_nib: Ray
### 題目解釋
    給定一個不為空的數列nums，求第三大的數；若沒有第三大的數，則回傳最大的數。
    （重覆的數不算進排序中。例如[2,2,3,1]回傳1。）
### 審題注意
    由於題目要求時間複雜度必須是 O(N)，不使用 sort；因此遍歷陣列並用 3 個變數來存前三大的數。
    k-th maximum則請想到min-heap：保持heap大小為k，若再加會超過，就pop掉最小的（最頂端的）再加。
### 解法
#### JavaScript (local variable)
```javascript
var thirdMax = function(nums) {
    let max1 = max2 = max3 = Number.NEGATIVE_INFINITY
    nums.forEach(e => {
        if(e != max1 && e != max2 && e != max3) {
            if(e > max1)
                [max3, max2, max1] = [max2, max1, e]
            else if(e > max2)
                [max3, max2] = [max2, e]
            else if (e > max3)
                max3 = e
        }
    })
    return max3 === Number.NEGATIVE_INFINITY ? max1 : max3
}
```
- 解釋：`line 4` 去重複，`line 5 - 10` 檢查是否為前三大的數，`line 13` 如果沒有第三大的值則回傳最大值。

#### Python3 (heap)
##### tags: `heap`
```python
def thirdMax(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    import heapq
    H = []
    k = 3  # k-th largest
    for num in nums:
        if num not in H:
            if len(H) == k:
                heapq.heappushpop(H, num)
            else:
                heapq.heappush(H, num)
        
    return max(H) if len(H) < 3 else min(H)

```
- Tips：heapq這個lib裡面也已經實作了 nsmallest / nlargest 這種function。
- 解釋：heapq的function原始碼：https://github.com/python/cpython/blob/3.7/Lib/heapq.py
- Reference：https://www.youtube.com/watch?v=NheWPxGpoxQ
---
