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

#### JavaScript (直觀解)
``` javascript
var matrixReshape = function(nums, r, c) {
    let preRow = nums.length, preCol = nums[0].length
    if(preRow * preCol !== r * c) return nums
    let matrix = []
    for(let flatIndex = 0; flatIndex < r * c; ++flatIndex) {
        let rowIndex = Math.floor(flatIndex / c)
        if(!matrix[rowIndex]) matrix.push([])
        matrix[rowIndex].push(nums[Math.floor(flatIndex / preCol)][flatIndex % preCol])
    }
    return matrix
}
```

#### JavaScript (精簡)
```javascript
var matrixReshape = function(nums, r, c) {
    if(nums.length * nums[0].length !== r * c) return nums
    let matrix = []
    for(let i = 0, list = []; i < nums.length; ++i) {
        list.push(...nums[i])
        while(list.length >= c)
            matrix.push(list.splice(0, c))
    }
    return matrix
}
```

#### JavaScript (2 liner)
```javascript
var matrixReshape = function(nums, r, c) {
    let flatMatrix = nums.reduce((acc, cur) => acc.concat(cur), [])
    return flatMatrix.length === r * c ?
        new Array(r).fill(0).map((row, index) => flatMatrix.slice(c * index, c * index + c)) :
        nums
}
```
- 解釋：
    1. 把 matrix 扁平化有 es6 的寫法：
        ```javascript
        let flatMatrix = nums.reduce((acc, cur) => [...acc, ...cur], [])
        ```
    2. 拆 flat matrix 也有另一種寫法：
        ```javascript
        new Array(r).fill(0).map((row, index) => flatMatrix.splice(0, c))
        ```
    3. `slice` vs `splice`：後者會真的動到陣列，前者只是把要的部份複製出來，原陣列保持不變。


---
