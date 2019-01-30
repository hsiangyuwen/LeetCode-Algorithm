
## [27. Remove Element](https://leetcode.com/problems/remove-element/submissions/)
> :black_nib: Cropse
### 題目解釋
    只能使用S(1)且掃一遍的狀況下，列出移除指定數字的列表。
### 審題注意
    輸入數字是長度，但是題目本身會用輸入列表reference，所以一定要改動列表。
### 解法
#### Go (直觀解)
```go
func removeElement(nums []int, val int) int {
    count := 0
    for i:=0; i<len(nums); i++{
        if nums[i]!=val{
            count ++
        }else{
            nums = append(nums[:i],nums[i+1:]...)
            i--
        }
    }
    return count
}
```
- 解釋：一邊紀錄非指定數字，然後切割list，再調整index位置
- 優/缺點：直觀，但是分割list其實沒有必要
#### Go (inplace)
```go
func removeElement(nums []int, val int) int {
    index := 0
    for i:=0; i<len(nums); i++{
        if nums[i]!=val{
            nums[index] = nums[i]
            index ++
        }
    }
    return index
}
```
- 解釋：因為是要移除數值，所以迴圈中的i≥index，所以只要把值往前複製就可以了，最後輸出時index位置後面的列表其實不重要。
#### Python (inplace)
```python
def removeElement(self, nums, val):
    """
    :type nums: List[int]
    :type val: int
    :rtype: int
    """
    length = 0
    for i, num in enumerate(nums):
        if num != val:
            nums[length] = num
            length += 1
    return length
```

#### JavaScript (inplace)
```javascript
var removeElement = function(nums, val) {
    let curIndex = 0
    for(let i = 0; i < nums.length; ++i)
        if(nums[i] !== val)
            nums[curIndex++] = nums[i]

    return curIndex
}
```
---
