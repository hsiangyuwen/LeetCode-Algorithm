## [167. Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
> :black_nib: Ray
### 題目解釋
    給一排序好的陣列及目標，找出陣列中的兩個元素相加等於目標
### 審題注意
    1. Search
    2. 可以想到三種方法：直接搜尋、查表和二元搜尋
### 解法
#### JavaScript (directly search)
```javascript
var twoSum = function(numbers, target) {
    let left = 0, right = numbers.length - 1
    while(left < right)
        if(numbers[left] + numbers[right] === target)
            return [left + 1, right + 1]
        else
            numbers[left] + numbers[right] > target ? right-- : left++
    return null
}
```
- 解釋：最直觀的解法，如果查詢的總和大於 `target`，那右邊界就往左走一索引，反之，左邊界就往右走一索引。
  - (最壞情況 O(n))
#### JavaScript (dictionary)
```javascript
var twoSum = function(numbers, target) {
    let dic = {}
    for(let i = 0; i < numbers.length; ++i)
        if(dic[target - numbers[i]] >= 0)
            return [dic[target - numbers[i]] + 1, i + 1]
        else dic[numbers[i]] = i
    return null
}
```
- 解釋：先查詢 `dic` 中是否有 `target` - 查詢值，如果有就回傳索引值；反之，紀錄查詢值的索引，以備之後查詢。
  - (最壞情況 O(n))
#### JavaScript (binary search)
##### tags: `binary search`
```javascript
var twoSum = function(numbers, target) {
    for(let i = 0; i < numbers.length; ++i) {
        let left = i + 1, right = numbers.length - 1, mid
        while(left <= right) {
            mid = Math.floor((right + left) / 2)
            if(numbers[mid] + numbers[i] === target)
                return [i + 1, mid + 1]
            else
                numbers[mid] + numbers[i] < target
                    ? (left = mid + 1)
                    : (right = mid - 1) 
        }
    }
}
```
- 解釋：用二元搜尋去查找符合相加等於 `target` 的元素。
  - (最好情況 O(lgn)，最壞情況 O(nlgn))
#### Go (directly search)
```go
func twoSum(numbers []int, target int) []int {
	left, right := 0, len(numbers)-1
	for left < right {
		if numbers[left]+numbers[right] > target {
			right--
		} else if numbers[left]+numbers[right] < target {
			left++
		} else {
			return []int{left+1, right+1}
		}
	}
	return []int{}
}
```
#### Go (hash)
```go
func twoSum(numbers []int, target int) []int {
    dic := map[int]int{}
    for i, val := range(numbers){
        if index, ok := dic[val]; ok==true{
            return []int{index+1, i+1}
        }
        dic[target-val] = i
    }
    return []int{}
}
```

#### Python3 (directly search)
```python
def twoSum(self, numbers: 'List[int]', target: 'int') -> 'List[int]':
    left = 0
    right = len(numbers) - 1
    while(left < right):
        two_sum = numbers[left] + numbers[right]
        if two_sum == target:
            return[left + 1, right + 1]
        elif two_sum < target:
            left += 1
        else:
            right -= 1
```

#### Python3 (dictionary)
```python
def twoSum(self, numbers: 'List[int]', target: 'int') -> 'List[int]':
    dic = {}
    for i in range(len(numbers)):
        if (target - numbers[i]) in dic:
            return [dic[target - numbers[i]] + 1, i + 1]
        else:
            dic[numbers[i]] = i
```
---
