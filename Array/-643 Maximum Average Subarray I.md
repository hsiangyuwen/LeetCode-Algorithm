## [643. Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，求連續k個元素組成的subarray，最大的可能平均值。
### 審題注意
    用prefix-sum的觀念去解題，可以將複雜度降為O(n)。
### 解法
#### Python (prefix-sum)
##### tags: `prefix-sum`
```python
def findMaxAverage(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: float
    """
    # 1. Build prefix-sum list
    prefix_sums = [nums[0]]
    for i in range(1, len(nums)):
        prefix_sums.append(prefix_sums[i-1] + nums[i])
    
    # 2. Build a list which contains subarrays' sum
    subarray_sums = [prefix_sums[k-1]]
    for i in range(k, len(nums)):
        subarray_sums.append(prefix_sums[i] - prefix_sums[i-k])

    return max(subarray_sums) / k
```
> Python有itertool和map可以分別拿來簡化步驟1和步驟2

#### Python (prefix-sum, use built-in lib)
##### tags: `built-in`
```python
def findMaxAverage(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: float
    """
    sums = [0] + list(itertools.accumulate(nums))
    return max(map(operator.sub, sums[k:], sums)) / k

```

#### JavaScript (直觀解, 2 liner)
```javascript
var findMaxAverage = function(nums, k) {
    for(let i = 1; i < nums.length; ++i)
        nums[i] += nums[i - 1]
    for(let i = nums.length - 1; i >= k; --i)
        nums[i] -= nums[i - k]
    return Math.max(...nums.slice(k - 1)) / k
}
```
- 解釋：觀察兩次 for loop 的結果。
    ```
    Example: [ 1, 12, -5, -6, 50, 3 ], k = 4
    After first for loop: [ 1, 13, 8, 2, 52, 55 ] (第 i 個元素是 0 ~ i 的總和)
    After seconde for loop: [ 1, 13, 8, 2, 51, 42 ] (第 i 個元素是 i-k ~ i 的總和)
    * 注意：經過兩次 for loop 第 0 ~ k-2 個元素不符合需求，所以不能拿它們比大小。
    ```
#### Golang(prefix-sum)
```go
func findMaxAverage(nums []int, k int) float64 {
    prefixSum := []int{0}
    for i:=0; i<k; i++{
        prefixSum[0] += nums[i]
    }
    for i:=1; i<len(nums)-k+1; i++{
        prefixSum = append(prefixSum, prefixSum[i-1] + nums[i+k-1] - nums[i-1])
    }
    max := math.MinInt64
    for _, val := range prefixSum{
        if max < val {
            max = val
        }
    }
    return float64(max)/float64(k)
}
```
- 解釋：做法稍微不同，`prefixSum`把第一次的總數先加起來，每一次平移都會使用扣除左邊加上右邊，就可以得到所有的最大值，最後拿出最大值除出即可，不用後續再處理一次。
---
