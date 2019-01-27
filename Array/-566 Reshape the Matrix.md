## [566. Reshape the Matrix](https://leetcode.com/problems/reshape-the-matrix/)
> :black_nib: Sammy
### 題目解釋
    給定一個二維陣列nums，轉換成r * c的二維陣列。
    如果無法轉換，則回傳原本的nums
### 審題注意
    這一題非常單純的考驗解題者是否能寫出座標等價的演算法。

    對於新二維陣列的第x列第y行，可以看做第(x * c + y)個數。
    所以假設原本給的nums是r' * c'的二維陣列，
    則 (x * c + y) ≡（x' * c' + y'）。
### 解法
#### Python3
```python
def matrixReshape(self, nums, r, c):
    """
    :type nums: List[List[int]]
    :type r: int
    :type c: int
    :rtype: List[List[int]]
    """
    prev_r = len(nums)
    prev_c = len(nums[0])
    if prev_r * prev_c != r * c:
        return nums
    
    matrix = [[0 for j in range(c)] for i in range(r)]  # initialize empty matrix
    for flat_idx in range(r * c):
        matrix[flat_idx // c][flat_idx % c] = nums[flat_idx // prev_c][flat_idx % prev_c]
    
    return matrix
```
---
