## [66. Plus One](https://leetcode.com/problems/plus-one/)
> :black_nib: Cropse
### 題目解釋
    用列表表示數字，加上1
### 審題注意
    只要注意進位問題即可
### 解法
1. 加總數字進位O(n)  
2. 轉換數字再轉回列表(這方法很爛)  

#### Go
```go
func plusOne(digits []int) []int {
    flag := true
    for i:= len(digits)-1; i>=0; i--{
        if flag{
            digits[i]++
            flag = false
        }
        if digits[i] >= 10{
            digits[i] %= 10
            flag = true
        }
    }
    if flag{
        digits[0] %= 10
        digits = append([]int{1}, digits...)
    }
    return digits
}
```
#### JavaScript (直觀解)
```javascript
var plusOne = function(digits) {
    for(let i = digits.length - 1; i >= 0; --i)
        if(digits[i] === 9)
            digits[i] = 0
        else {
            digits[i]++
            return digits
        }

    return [1, ...digits]
}
```
#### JavaScript (精簡)
```javascript
var plusOne = function(digits) {
    for(let i = digits.length - 1; i >= 0; --i)
        if(++digits[i] > 9) digits[i] = 0
        else return digits

    return [1, ...digits]
}
```
---
