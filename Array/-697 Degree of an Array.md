## [697. Degree of an Array](https://leetcode.com/problems/degree-of-an-array/)
> :black_nib: Ray
### 題目解釋
    找到重複次數最多的元素，如果有多個，回傳最短的長度
### 審題注意
    1. 先統計出最多重複的次數，再找出能包含該重複次數的最短長度。
    2. 另一種做法是紀錄每個元素的起始索引值，如果出現最多重複次數的元素，就更新長度
### 解法
#### JavaScript
```javascript
var findShortestSubArray = function(nums) {
    let count = {}, degree = 0
    for(let i = 0; i < nums.length; ++i) {
        count[nums[i]] = (count[nums[i]] || 0) + 1
        degree = Math.max(degree, count[nums[i]])
    }
    let res = nums.length
    for(let i in count)
        if(count[i] === degree)
            res = Math.min(res, nums.lastIndexOf(Number(i)) - nums.indexOf(Number(i)) + 1)
    return res
}
```
- 解釋：先統計每個元素出現的次數，並紀錄最多出現的次數，最後找出該次數的第一元素跟最後元素的最短距離。(如果不能用 built-in function，可以另外用 `dictionary` 紀錄 `firstIndex` 跟 `lastIndex`)
#### JavaScript (better)
```javascript
var findShortestSubArray = function(nums) {
    let count = {}, first = {}, degree = 0, res = 1
    for(let i = 0; i < nums.length; ++i)
        if(!count[nums[i]])
            count[nums[i]] = 1, first[nums[i]] = i
        else if(++count[nums[i]] === degree)
            res = Math.min(i - first[nums[i]] + 1, res)
        else if(count[nums[i]] > degree)
            degree = count[nums[i]], res = i - first[nums[i]] + 1
    return res
}
```
- 解釋：邊統計出現次數，並紀錄最短距離。如果 `count` 中沒有該元素，就設值為 1，並記錄第一個該元素的索引值；如果該元素存在 `count`，且與當時最高 `degree` **相同**，則判斷是否比原本最短距離短；如果比當時 `degree` **高**，則更新 `degree` 與最短距離。
- 優點：
    1. 不須紀錄 `lastIndex`，省 memory。
    2. 最後不用再查詢 `firstIndex` 及 `lastIndex`，省時。

#### Go
```go
type m struct {
	left  int
	right int
	count int
}
func findShortestSubArray(nums []int) int {
	dmap := make(map[int]*m)
	for i, val := range nums {
		if dmap[val] == nil {
			dmap[val] = &m{}
			dmap[val].left = i
		}
		dmap[val].right = i
		dmap[val].count++
	}
	shortlestDegree := &m{}
	for _, val := range dmap {
		if shortlestDegree.count < val.count {
			shortlestDegree = val
		} else if shortlestDegree.count == val.count {
			if shortlestDegree.right-shortlestDegree.left > val.right-val.left {
				shortlestDegree = val
			}
		}
	}
	return 1 + shortlestDegree.right - shortlestDegree.left
}

```
---
