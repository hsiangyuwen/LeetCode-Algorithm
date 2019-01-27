
## [532. K-diff Pairs in an Array](https://leetcode.com/problems/k-diff-pairs-in-an-array/)
> :black_nib: Ray
### 題目解釋
    給一數字陣列及 k，找出所有差距為 k 的數對，例如 k = 2，(3,5) 和 (5,3)，被視為相同，只能算一組
### 審題注意
    1. 直覺想到暴力解：先計算每個數字出現的次數，每個數字逐一判斷是否存在加上 k 的數。
    2. 由於有去重複(數對)的需求，可以使用 set。
    3. 絕對值大於等於 0 (k >= 0)。
### 解法
#### JavaScript (直觀解)
```javascript
var findPairs = function(nums, k) {
    if(!nums || !nums.length || k < 0) return 0
    let counts = {}, pairCounts = 0
    for(let i = 0; i < nums.length; ++i)
        counts[nums[i]] = (counts[nums[i]] || 0) + 1

    Object.keys(counts).forEach(key => {
        if(k > 0) {
            if(counts[Number(key) + k])
                pairCounts++
        } else if(counts[key] >= 2)
            pairCounts++
    })
    return pairCounts
}
```
- 解釋：
	1. 先統計每個數字出現的次數(可以用 `reduce` 實現)。
	2. `k > 0` 時逐一判斷是否有 `key + k`，由於每個 `key` 的值在 `counts` 中是唯一的，所以 `key` 與 `key + k` 組成的數對也是唯一的，這一步可以完成去重複；`k == 0` 時，判斷是否出現兩次以上。
- 缺點：
	1. `if` 和 `else if` 可以簡化。
	2. 累加的部分可以使用 `reduce`。
	3. 只要知道有沒有出現過，不用知道出現的次數(用 `set` 實現)
#### JavaScript (reduce, 精簡)
```javascript
var findPairs = function(nums, k) {
    if(!nums || !nums.length || k < 0) return 0
    let counts = nums.reduce((acc, n) => {
        acc[n] = (acc[n] || 0) + 1
        return acc
    }, {})
    return Object.keys(counts).reduce((acc, key) =>
        ((k > 0 && counts[Number(key) + k]) || (k === 0 && counts[key] >= 2))
        ? acc + 1 : acc
    , 0)
}
```
- 解釋：用兩個 `reduce` 分別去統計次數及計算數對的數目，並將判斷精簡成一行。
#### JavaScript (set)
```javascript
var findPairs = function(nums, k) {
    if(!nums || !nums.length || k < 0) return 0
    let counts = new Set(), res = new Set()
    nums.forEach(n => {
        if(counts.has(n + k)) res.add(n + k)
        if(counts.has(n - k)) res.add(n)
        counts.add(n)
    })
    return res.size
}
```
- 解釋：先判斷 `counts` 中是否有 `n + k` 或 `n - k` 的存在，有的話則紀錄數對中大的值於 `res`，之後再將 `n` 存入 `counts` (如果先做這步驟， `k` 為 0 時會出錯)。
- 優點：徹底發揮 `set` 的特性。
---
