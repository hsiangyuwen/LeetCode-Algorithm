## [766. Toeplitz Matrix](https://leetcode.com/problems/toeplitz-matrix/)
> :black_nib: Sammy
### 題目解釋
    矩陣的每一條對角線(左上-右下)，對角線上的數字都要相同。
### 審題注意
    題目的input是 List[List[int]]
    如果把元素位置標記成(列, 行)就會是：
    [
        [(0,0), (0,1)],
        [(1,0), (1,1)]
    ]

    就直觀的角度而言，會想要一條一條對角線做確認。但這樣在實作程式的時候會很麻煩。
    比較好的作法是：
        
        用雙層for迴圈，檢查每個元素是否都和它的左上角那個元素相同。
    
    複雜度為O(m*n)，更精確地來說是O((m-1)*(n-1))
### 解法
#### Python3 (for loop)
##### tags: `for loop`
```python
 def isToeplitzMatrix(self, matrix):
        
    # The description ensure that we don't need to care about empty matrix.
    row_count = len(matrix)
    col_count = len(matrix[0])

    for i in range(1, row_count):
        for j in range(1, col_count):
            if matrix[i][j] != matrix[i-1][j-1]:
                return False

    return True

```
- 解釋：一檢查到有不同，就回傳False。全部檢查完都沒有回傳False發生，就回傳True。

#### JavaScript (for loop)
##### tags: `for loop`
```javascript
var isToeplitzMatrix = function(matrix) {
    for(let i = 1; i < matrix.length; ++i)
        for(let j = 1; j < matrix[0].length; ++j)
            if(matrix[i][j] !== matrix[i - 1][j - 1])
                return false
    return true
}
```
#### Go (for loop)
##### tags: `for loop`
```go
func isToeplitzMatrix(matrix [][]int) bool {
    for i:=1; i<len(matrix); i++{
        for j:=1; j<len(matrix[0]); j++{
            if matrix[i][j] != matrix[i-1][j-1]{
                return false
            }
        }
    }
    return true
}
```
---
