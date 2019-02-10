
## [840 Magic Squares In Grid](https://leetcode.com/problems/magic-squares-in-grid/)
> :black_nib: Ray
### 題目解釋
	找出所有 magic square 的數目。
### 審題注意
    1. Magic Square 的定義：
	    a. 3 x 3
	    b. 數字 1 ~ 9
	    c. 每行每列及對角和都相同
	2. 找規律
### 解法
#### JavaScript (One-pass)
```javascript
var numMagicSquaresInside = function(grid) {
    let count = 0
    for(let r = 1; r < grid.length - 1; ++r)
        for(let c = 1; c < grid[0].length - 1; ++c)
            count += (grid[r][c] === 5 && isMagic(grid, r, c)) ? 1 : 0
    return count
}

var isMagic = (grid, r, c) => {
    let set = new Set()
    for(let i = -1; i <= 1; ++i)
        for(let j = -1; j <= 1; ++j) {
            let num = grid[r + i][c + j]
            if(set.has(num) || num > 9 || num < 1)
                return false
            else set.add(num)
        }
    
    if(grid[r - 1][c - 1] + grid[r + 1][c + 1] !== 10 ||
       grid[r - 1][c + 1] + grid[r + 1][c - 1] !== 10)
        return false
    
    for(let i = -1; i <= 1; ++i)
        if(grid[r + i][c - 1] + grid[r + i][c] + grid[r + i][c + 1] !== 15 ||
           grid[r - 1][c + i] + grid[r][c + i] + grid[r + 1][c + i] !== 15)
            return false
    
    return true
}
```
- 解釋：
 1. 如果是 Magic Square，每行每列和對角的和一定是 15。證明如下：
		
    假設有一 Magic Square 為：
    
    ![](https://latex.codecogs.com/gif.latex?$%20%5Cbegin%7Bbmatrix%7D%20a_1%20&%20a_2%20&%20a_3%20%5C%5C%20a_4%20&%20a_5%20&%20a_6%20%5C%5C%20a_7%20&%20a_8%20&%20a_9%20%5Cend%7Bbmatrix%7D%20$)
		
    因為在 Magic Square 性質中，每個元素的值介於 1~9 之間且不重複，所以可得
    
    ![](https://latex.codecogs.com/gif.latex?%24%5Csum_%7B1%7D%5E%7B9%7D%20a_i%20%3D%2045%24)
		
    又每一列總和相同，故每一列的和為 15。
    求出每一列的和後可知每一行及對角的和亦為 15。
		
  2.  中心數字必為 5。證明如下：
		
        ![](https://latex.codecogs.com/gif.latex?%5C%5C%20a_1%20&plus;%20a_5%20&plus;%20a_9%20%3D%2015%5C%5C%20a_2%20&plus;%20a_5%20&plus;%20a_8%20%3D%2015%5C%5C%20a_3%20&plus;%20a_5%20&plus;%20a_7%20%3D%2015%5C%5C%20a_4%20&plus;%20a_5%20&plus;%20a_6%20%3D%2015)
        
		將 4 式相加得
		
        ![](https://latex.codecogs.com/gif.latex?%5C%5C%20%5Csum_%7B1%7D%5E%7B9%7D%20a_i%20&plus;%203a_5%20%3D%2060%5C%5C%203a_5%20%3D%2015%20%5C%5C%20a_5%20%3D%205%5C%5C)
        
  3. 在 `isMagic` 中先判斷是否介於 1~9 之間且不重複 (`set`)，再判斷對角的和及每行每列的和。
---
