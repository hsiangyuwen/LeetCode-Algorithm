## [485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/)
> :black_nib: Sammy
### 題目解釋
    找出一個僅含0與1的數列nums中，最長的連續為1的長度。
### 審題注意
    與直觀解法相同。遍歷一次得出結果。
    複雜度為O(n)。
### 解法
#### Python3 (直觀解)
```python
def findMaxConsecutiveOnes(self, nums):
    ret = 0
    cur = 0
    for num in nums:
        cur = cur + 1 if num == 1 else 0
        ret = max(ret, cur)

    return ret
```
---
