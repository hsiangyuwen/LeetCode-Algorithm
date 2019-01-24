## [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)
> :black_nib: Ray
### 題目解釋
    判斷 Array 中是否有重複的數字
### 審題注意
    1. 去重複立刻想到 Set
    2. 也可以用 Map 概念(key-value)去記錄每個數字出現的次數 (發現重複就立刻回傳 true)
### 解法
#### Javascript (1 line, set)
##### tags: `1 line`, `set`
```javascript
const containsDuplicate = nums => new Set(nums).size < nums.length
```
- 解釋： `Set` 的使用 ([文檔](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set))
    1. 首先是 `Set` 的 constructor (如下)，如果不了解 iteralbe 的涵義，可以把 `[iterable]` 想像成 `Array`。
    ```javascript
    new Set([iterable])
    ```
    2. 特別注意，計算 `Set` 中的元素個數，有別於 `Array` 使用 `length`，**`Set` 使用 `size`**。
    3. 因為 `Set` 中每個元素是 unique 的，所以如果 `Set` 的 `size` 小於原本 `nums` 的長度，代表 `nums` 中有重複的元素。
- 缺點
    1. 把 `Array` 存成 `Set` 理論上需要的時間是 O(nlgn)。
    2. 沒有必要全部跑完，可以在發現重複的時候就停止。

#### Javascript (without set)
```javascript
var containsDuplicate = function(nums) {
    let count = {}
    for(let i = 0; i < nums.length; ++i)
        if(count[nums[i]])
            return true
        else
            count[nums[i]] = true
    return false
}
```
#### Python3 (1 line, set)
##### tags: `1 line`, `set`
```python
def containsDuplicate(self, nums):
    return len(set(nums)) != len(nums)
```
#### Go
```go
func containsDuplicate(nums []int) bool {
	hashTable := map[int]bool{}
	for _, val := range nums {
		if _, ok := hashTable[val]; ok == true {
			return true
		}
		hashTable[val] = true
	}
	return false
}
```
#### Go
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
---
