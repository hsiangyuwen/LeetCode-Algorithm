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
---
