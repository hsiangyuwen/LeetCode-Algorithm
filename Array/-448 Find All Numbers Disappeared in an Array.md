## [448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
> :black_nib: Cropse
### 題目解釋
給一個長度n的數列，數列中有1~n的數字，列出裡面沒有的數字
### 審題注意
題目很單純，沒特別需要注意什麼
### 解法
S(n)解決即可，注意結果可以拿原本的cache切割再回傳
#### Golang
```go
func findDisappearedNumbers(nums []int) []int {
    cache := make([]int, len(nums))
    
    for _, val := range(nums){
        cache[val-1] = 1
    }
    count := 0
    for i, val := range(cache){
        if val==0{
            cache[count] = i+1
            count++
        }
    }
    return cache[:count]
}
```

#### Python3 (Set, 1-line)
```python
def findDisappearedNumbers(self, nums: 'List[int]') -> 'List[int]':
    return list(set(range(1, len(nums) + 1)) - set(nums))
```
---
