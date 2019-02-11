## [832. Flipping an Image](https://leetcode.com/problems/flipping-an-image/)
> :black_nib: Cropse
### 題目解釋
一個二維陣列表示圖片，翻轉圖片
### 審題注意
注意左右是交換然後把數字反向，上下(row)之間則沒有直接關聯
### 解法
#### Golang(S(N))
```go
func flipAndInvertImage(A [][]int) [][]int {
    cache := make([]int, len(A[0]))
    for i, row := range A{
        for j, _ := range row{
            cache[j] = 1-row[len(A[0])-j-1]
        }
        copy(A[i], cache)
    }
    return A
}
```
- Tips
直觀解，直接複製一份轉好的copy回去就可以了
- 解釋
題目沒特殊解法，直接O(M*N)解即可，用S(N)可以少跑一遍
- 優/缺點
題目可以用S(1)解，如下解
- 
#### Golang(S(1))
```go
func flipAndInvertImage(A [][]int) [][]int {
    for _, row := range A{
        for l, r := 0, len(A[0])-1;l<=r; l,r=l+1,r-1{
            row[l], row[r] = 1-row[r], 1-row[l]
        }
    }
    return A
}
```
- Tips
左右對調跟數字反轉其實可以用交換的，所以不需要S(N)。
- 優/缺點
又快又小，真的很棒。
---
