

## [674. Longest Continuous Increasing Subsequence](https://leetcode.com/problems/longest-continuous-increasing-subsequence/)
> :black_nib: Cropse
### 題目解釋
給一個數組，找出最長的遞增數列，回傳長度。
### 審題注意
中斷就要重新計算
### 解法
直接硬解，沒有特殊解法。
#### Golang
```go
func findLengthOfLCIS(nums []int) int {
    var pre, count, maximum int
    for _, val := range(nums){
        if val > pre{
            pre = val
            count++
            if maximum < count{
                maximum = count
            }
        }else{
            pre = val
            count = 1
        }
    }
    return maximum
}
```
- Tips
- 解釋
直觀解
- 優/缺點
簡單直覺
- 
```go
func findLengthOfLCIS(nums []int) int {
	if len(nums) <= 1{
		return len(nums)
	}
    count, maximum := 1, 1
	for i := 1; i < len(nums); i++ {
		if nums[i] > nums[0] {
			nums[0] = nums[i]
			count++
			if maximum < count {
				maximum = count
			}
		} else {
			nums[0] = nums[i]
			count = 1
		}
	}
	return maximum
}
```
- Tips
從1開始，借位置，也會快一點。
- 解釋
- 優/缺點
很難看
---
