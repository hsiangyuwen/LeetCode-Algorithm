## [448. Find All Numbers Disappeared in an Array](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)
> :black_nib: Cropse
### 題目解釋
給一個長度n的數列，數列中有1~n的數字，列出裡面沒有的數字
### 審題注意
題目很單純，沒特別需要注意什麼
### 解法
S(n)解決即可，注意結果可以拿原本的cache切割再回傳
#### Golang
```go
func findDisappearedNumbers(nums []int) []int {
    cache := make([]int, len(nums))
    
    for _, val := range(nums){
        cache[val-1] = 1
    }
    count := 0
    for i, val := range(cache){
        if val==0{
            cache[count] = i+1
            count++
        }
    }
    return cache[:count]
}
```

#### Python3 (Set, 1-line)
```python
def findDisappearedNumbers(self, nums: 'List[int]') -> 'List[int]':
    return list(set(range(1, len(nums) + 1)) - set(nums))
```

#### JavaScript
```javascript
var findDisappearedNumbers = function(nums) {
    for(let i = 0, pos; i < nums.length; ++i) {
        pos = Math.abs(nums[i]) - 1
        if(nums[pos] > 0)
            nums[pos] *= -1
    }
    
    return nums
        .map((num, idx) => num > 0 && idx + 1)
        .filter(num => num)
}
```
- 解釋：
    1. 先將陣列裡面的數字對應到的位置標記為負數，有點饒口，意思就是用陣列的**位置**紀錄陣列中元素是否有出現，整個跑完如果陣列中的數字還是正數，就代表該位置的**索引值**未曾出現過，下列有幾點要特別小心：
        1. 由於數字會出現 [1, n]，陣列位置只有 [0, n-1]，因此 `pos` 減一。
        2. 避免**負數**索引值，因此加了絕對值(`Math.abs()`)。
        3. 要多判斷該位置是否已經是負數，如果不是才乘以`-1`。
    2. 最後將陣列中僅存的正數替換為該**索引值 + 1**(`map`)，最後篩選出 `nums` 不為 0 的數(`filter`)。
---
