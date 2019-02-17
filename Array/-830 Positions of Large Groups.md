## [830. Positions of Large Groups](https://leetcode.com/problems/positions-of-large-groups/)
> :black_nib: Ray
### 題目解釋
    回傳字串中至少連續且重複相同字元三次的起始及終點座標
### 審題注意
    用兩個 pointer
### 解法
#### JavaScript
```javascript
var largeGroupPositions = function(S) {
    let start = 0, end = 0, N = S.length, res = []
    while(start < N) {
        while(end < N && S[start] === S[end]) end++
        if(end - start >= 3) res.push([start, end - 1])
        start = end
    }
    return res
}
```
- 解釋
    1. 例子：
    ```
    Example.
            aabbbcc     aabbbcc     aabbbcc    
    start   0        => 0        => 0       =(end - start = 2)=>
    end     0            1            2 
    -------------------------------------------------------------
    res     []          []          []
    =============================================================
            aabbbcc    aabbbcc         aabbbcc
    start     2     =>   2     =>...=>   2      =(end - start = 3)=>record...
    end       2           3                 5
    -------------------------------------------------------------------------
    res     []         []              []                           [[2,4]]
    ```
    2. 唯一需要注意的地方就是：`line 4` 終止時，`end` 指向下一個不相同的字元，因此 `line 5` 的判斷式，可以想一個上方簡單的例子，起始索引跟結束索引至少要相差 **3** 以上才符合條件；另外，記錄起始和結束索引時，要將結束索引減一，原因如上(`end` 是指向下一個不相同字元)。

#### Python3
```python
def largeGroupPositions(self, S: 'str') -> 'List[List[int]]':
    ret = []
    start = end = 0
    while end < len(S):
        if S[end] == S[start]:
            end += 1
        else:
            if (end - start) >= 3:
                ret.append([start, end - 1])
            start = end

    if end - start >= 3:
        ret.append([start, end - 1])

    return ret
```

#### Go
```go
func largeGroupPositions(S string) [][]int {
	result := [][]int{}
	var index int
	for index < len(S) {
		s := index
		for index < len(S) && S[index] == S[s] {
			index++
		}
		if index-s >= 3 {
			result = append(result, []int{s, index - 1})
		}
	}
	return result
}
```
---
