## [283. Move Zeroes](https://leetcode.com/problems/move-zeroes/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，把0都移到後面，並且不改變不為0的element的順序。
### 審題注意
    這題也是希望解題者inplace改動。直觀上會想使用swap，但這樣element順序會不見。
    
    其實遍歷一次，把不為0的提到前面，剩下的再填0即可。
### 解法
#### Python
```python
def moveZeroes(self, nums):
    """
    :type nums: List[int]
    :rtype: void Do not return anything, modify nums in-place instead.
    """
    fillin_index = 0
    for i in range(len(nums)):
        if nums[i] != 0:
            nums[fillin_index] = nums[i]
            fillin_index += 1
    size = len(nums) - fillin_index
    nums[fillin_index:] = bytearray(size)
```
- Tips：利用slice方式，把新建出來的bytearray assign給原本list的某片段，是python實作memset效果的一種方式。

---
