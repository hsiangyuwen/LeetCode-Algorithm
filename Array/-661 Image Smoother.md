## [661. Image Smoother](https://leetcode.com/problems/image-smoother/)
> :black_nib: Cropse
### 題目解釋
二維陣列，數值表示灰度，每個節點是透過九宮格內的平均值捨去小數點
### 審題注意
注意邊界即可，基本上就是暴力解。
### 解法
最好寫一個helper function或是closure function把部分邏輯切割出來。
#### Golang
```go
func imageSmoother(M [][]int) [][]int {

	getVal := getValGen(M)
	result := make([][]int, len(M))
	for i := range result {
		result[i] = make([]int, len(M[0]))
	}
	divide := 0

	for x, y := 0, 0; x < len(M[0]) && y < len(M); {
		for i := -1; i <= 1; i++ {
			for j := -1; j <= 1; j++ {
				preResult, preDivide := getVal(y+i, x+j)
				result[y][x] += preResult
				divide += preDivide
			}
		}
		result[y][x] = result[y][x] / divide
        divide = 0
		x++
		if x == len(M[0]) {
			x = 0
			y++
		}
	}
	return result
}

func getValGen(M [][]int) func(y int, x int) (int, int) {
	return func(y int, x int) (int, int) {
		if x < 0 || y < 0 || y >= len(M) || x >= len(M[0]) {
			return 0, 0
		} else {
			return M[y][x], 1
		}
	}
}
```
- Tips
寫一個function把邊界計算的部分給獨立出來，邏輯會相對清楚。
- 解釋
golang的closure需要回傳function，可以改寫成傳址的方法，效率應該會更好一點。
- 優/缺點
沒有

#### Python
```python
def imageSmoother(self, M: 'List[List[int]]') -> 'List[List[int]]':
	ret = [[0] * len(M[0]) for _ in range(len(M))]  # Create an empty 2D-array
	row_last = len(M) - 1
	col_last = len(M[0]) - 1
	
	for row in range(len(M)):
		for col in range(len(M[0])):
			# Sum up all the 8 surrounding cells and itself(5).
			# Position:
			# 1 2 3
			# 4 5 6
			# 7 8 9

			ret[row][col] += M[row][col]  # 5
			cell_count = 1
			if row != 0:
				ret[row][col] += M[row-1][col]  # 2
				cell_count += 1
				if col != 0:
					ret[row][col] += M[row-1][col-1]  # 1
					cell_count += 1
				if col != col_last:
					ret[row][col] += M[row-1][col+1]  # 3
					cell_count += 1
			if row != row_last:
				ret[row][col] += M[row+1][col]  # 8
				cell_count += 1
				if col != 0:
					ret[row][col] += M[row+1][col-1]  # 7
					cell_count += 1
				if col != col_last:
					ret[row][col] += M[row+1][col+1]  # 9
					cell_count += 1
			if col != 0:
				ret[row][col] += M[row][col-1]  # 4
				cell_count += 1
			if col != col_last:
				ret[row][col] += M[row][col+1]  # 6
				cell_count += 1

			# Divide the count of used cells.
			ret[row][col] //= cell_count
	
	return ret
```
---
