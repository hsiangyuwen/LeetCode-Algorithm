## [119. Pascal's Triangle II](https://leetcode.com/problems/pascals-triangle-ii/submissions/)
> :black_nib: Cropse
### 題目解釋
一樣是pascal's Triangle，但是只需要最後一階即可。
要注意第一層為`[1, 1]`、第二層為`[1, 2, 1]`、第三層為`[1, 3, 3, 1]`，以此類推。
### 審題注意
題目要求可以到33，如果去求一整個三角形再拿最後一層來回傳，會有
### 解法
因為只需要最後一次的結果，所以透過`T[i][j] = T[i-1][j] + T[i-1][j-1]`，可以用原本的list由後至前，把前一個元素往後加，就可以算出所有總額。
（`加完的值 = 前一次同位置的值 + 前一次前一個位置的值`）
比較難望文生義，我們直接以一個例子去看list中元素數值每一步的變化。
以第五層為`[1,5,10,10,5,1]`來舉例：

    第五層會有6個元素，我們先創出六個元素皆為1的list（假設變數名為nums）：
    index:  0 | 1 | 2 | 3 | 4 | 5
    value:  1 | 1 | 1 | 1 | 1 | 1

    由 index=1 開始，到 index=4 結束，跑一個迴圈，把 nums[index - 1] 加到 nums[index] 上，再把index減1，直到index為1時做完後結束迴圈。
    也就是說會有10個迴圈：
    0: nums[1] += nums[0]  =>  1 | 2 | 1 | 1 | 1 | 1

    1: nums[2] += nums[1]  =>  1 | 2 | 3 | 1 | 1 | 1
    2: nums[1] += nums[0]  =>  1 | 3 | 3 | 1 | 1 | 1

    3: nums[3] += nums[2]  =>  1 | 3 | 3 | 4 | 1 | 1
    4: nums[2] += nums[1]  =>  1 | 2 | 6 | 4 | 1 | 1
    5: nums[1] += nums[0]  =>  1 | 4 | 6 | 4 | 1 | 1

    6: nums[4] += nums[3]  =>  1 | 4 | 6 | 4 | 5 | 1
    7: nums[3] += nums[2]  =>  1 | 4 | 6 | 10 | 5 | 1
    8: nums[2] += nums[1]  =>  1 | 4 | 10 | 10 | 5 | 1
    9: nums[1] += nums[0]  =>  1 | 5 | 10 | 10 | 5 | 1    

#### golang
```go
func getRow(rowIndex int) []int {
    res := make([]int, rowIndex+1)
    for i:=0; i< rowIndex+1; i++{
        res[i] = 1
    }

    for i:=1; i<rowIndex; i++{
        for j:= i; j>0; j--{
            res[j] += res[j-1]
        }
    }
    return res
}
```

#### JavaScript
```javascript
var getRow = function(rowIndex) {
    let result = new Array(rowIndex + 1).fill(1)
    
    for(let i = 1; i < rowIndex; ++i)
        for(let j = i; j > 0; --j)
            result[j] += result[j - 1]
    
    return result
}
```

#### Python3
```python
def getRow(self, rowIndex: int) -> List[int]:
    ret = [1]*(rowIndex+1)
    for i in range(1, rowIndex):
        for j in range(i, 0, -1):
            ret[j] += ret[j-1]
            
    return ret
```
---
