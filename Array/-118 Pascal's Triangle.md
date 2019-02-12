## [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/submissions/)
> :black_nib: Cropse
### 題目解釋
    Pascal三角，數字是由左上跟上方的數字合併出來，頭尾則都是1
### 審題注意
    這題相當簡單，主要是對 Array 矩陣的操作，只要注意頭尾跟中間的狀況不同就可以了。
### 解法
#### Go
```go
func generate(numRows int) [][]int {
    result := [][]int{}
    for i:=0; i<numRows; i++{
        subResult := []int{}
        for j:=0; j<=i; j++{
            if j == 0 || j == i{
                subResult = append(subResult,1)    
            }else{
                subResult = append(subResult,result[i-1][j-1]+result[i-1][j])
            }
        }
        result = append(result, subResult)
    }
    return result
}
```

#### Python
```python
def generate(self, numRows: 'int') -> 'List[List[int]]':
    ret = []
    for i in range(numRows):
        row = [1]
        for j in range(1, i+1):
            a = ret[i-1][j-1] if j > 0 else 0
            b = ret[i-1][j] if j < i else 0
            row.append(a+b)
        ret.append(row)
    
    return ret
```

#### Python (map function)
##### tags: `map` , `lambda`
```python
def generate(self, numRows: 'int') -> 'List[List[int]]':
    ret = [[1]] if numRows else []
    for i in range(1, numRows):
        ret.append(list(map(lambda x, y: x + y, ([0] + ret[-1]), (ret[-1] + [0]))))
    return ret
```
- 解釋：
```
    1 3 3 1 0 
 +  0 1 3 3 1
 =  1 4 6 4 1
```
- Ref: [map function](http://www.runoob.com/python/python-func-map.html)

#### JavaScript
```javascript
var generate = function(numRows) {
    let result = []
    for(let i = 0; i < numRows; ++i) {
        let row = [1]
        for(let j = 1; j <= i; ++j)
            row.push((res[i - 1][j - 1] || 0) + (res[i - 1][j] || 0))
        
        result.push(row)
    }
    return result
}
```
---
