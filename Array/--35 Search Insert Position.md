## [35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)
> :black_nib: Sammy
### 題目解釋
    給定一個由小到大排列好的數列nums，以及一個數字target。
    求：target在nums裏面，則回傳其index；如果不在，則回傳為維持順序，target該插在哪個index。
### 審題注意
    直觀上會分別處理兩種case，但其實兩種case的含意是相同的：
        把target放在第一個比它大的數前面。
    
    也就是說，遍歷一次，遇到第一個第一個大於等於的數，這個index就是「他該放的/他在的」位置。
### 解法
#### Python3
```python
def searchInsert(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: int
    """
    for i in range(len(nums)):
        if nums[i] >= target:
            return i
    return len(nums)

```
- Tips：Submit測資中沒有nums為空的狀況，故沒有做檢查。
- 解釋：如果都找不到大於等於target的值，就放最後面（回傳值剛好就是nums長度）。

#### JavaScript
```javascript
var searchInsert = function(nums, target) {
    for(let i = 0; i < nums.length; ++i)
        if(target <= nums[i])
            return i

    return nums.length
}
```
---
#### Golang
```go
func searchInsert(nums []int, target int) int {
    for i, val := range nums{
        if target <= val{
            return i
        }
    }
    return len(nums)
}
```
