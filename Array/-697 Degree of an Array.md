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

#### Python3
```python
def findShortestSubArray(self, nums: 'List[int]') -> 'int':
    dic = {}
    # key: value of the element
    # value: [count, leftest_index, rightest_index]

    for i, num in enumerate(nums):
        if num not in dic:
            dic[num] = [1, i, i]
        else:
            dic[num][0] += 1
            dic[num][2] = i
    
    # degree should be a min value in initial
    # return length should be a max value in initial
    degree = -1
    ret = 50000
    for t in dic.values():
        if t[0] > degree:
            degree = t[0]
            ret = t[2] - t[1] + 1
        elif t[0] == degree:
            ret = min(ret, (t[2] - t[1] + 1))
    
    return ret
```
- 解釋：官方解答直接用3個dictionary去紀錄，這個解法合併成一個，但解法觀念和官方解答相似：先記錄好各個數的出現次數、最左、最右，再去算最大degree和最小長度。
- 複雜度：遍歷兩次，時間複雜度為O(n)；空間複雜度為O(n)

---
