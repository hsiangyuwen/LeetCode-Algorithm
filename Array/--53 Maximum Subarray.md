## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
> :black_nib: Cropse
### 題目解釋
    從列表中找出相加集合的最大值
### 審題注意
    注意負值也有可能是最大值
### 解法
1. 從頭開始不斷加總，但是如果值是負值就拋棄掉(避免值再加總後更小)  
2. Divide-and-conquer algorithm(分治法)  
#### Go
##### tags: `O(n)`
```go
func maxSubArray(nums []int) int {
    max := nums[0]
    val := 0
    for _, i := range nums{
        val += i
        if val > max{
            max = val
        }
        if val < 0{
            val = 0
        }
    }
    return max
}
```
---
