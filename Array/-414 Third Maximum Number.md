## [414. Third Maximum Number](https://leetcode.com/problems/third-maximum-number/)
> :black_nib: Ray
### 題目解釋
    找到第三大的數，重複不算，如果沒有回傳最大的數。
### 審題注意
    由於題目要求時間複雜度必須是 O(N)，不使用 sort；因此遍歷陣列並用 3 個變數來存前三大的數。
### 解法
#### JavaScript
```javascript
var thirdMax = function(nums) {
    let max1 = max2 = max3 = Number.NEGATIVE_INFINITY
    nums.forEach(e => {
        if(e != max1 && e != max2 && e != max3) {
            if(e > max1)
                [max3, max2, max1] = [max2, max1, e]
            else if(e > max2)
                [max3, max2] = [max2, e]
            else if (e > max3)
                max3 = e
        }
    })
    return max3 === Number.NEGATIVE_INFINITY ? max1 : max3
}
```
- 解釋：`line 4` 去重複，`line 5 - 10` 檢查是否為前三大的數，`line 13` 如果沒有第三大的值則回傳最大值。
---
