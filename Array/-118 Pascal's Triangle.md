## [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/submissions/)
> :black_nib: Cropse
### 題目解釋
    Pascal三角，數字是由左上跟上方的數字合併出來，頭尾則都是1
### 審題注意
    這題相當簡單，主要是對 Array 矩陣的操作，只要注意頭尾跟中間的狀況不同就可以了。
### 解法
#### Go
```go
func generate(numRows int) [][]int {
    result := [][]int{}
    for i:=0; i<numRows; i++{
        subResult := []int{}
        for j:=0; j<=i; j++{
            if j == 0 || j == i{
                subResult = append(subResult,1)    
            }else{
                subResult = append(subResult,result[i-1][j-1]+result[i-1][j])
            }
        }
        result = append(result, subResult)
    }
    return result
}
```
---
