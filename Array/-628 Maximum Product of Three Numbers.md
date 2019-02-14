## [628. Maximum Product of Three Numbers](https://leetcode.com/problems/maximum-product-of-three-numbers/)
> :black_nib: Ray
### 題目解釋
    找出三個數相乘最大
### 審題注意
    1. 考慮正負組合，只有可能是 - - + or + + +。我我
    2. 可以先排序，再找頭跟尾需要的值。(slow)
    3. 也可以只找需要的值。(fast)
### 解法
#### JavaScript (slow)
```javascript
var maximumProduct = function(nums) {
    nums.sort((a, b) => a - b)
    let max = nums.pop()
    return max * Math.max(nums[0] * nums[1], nums.pop() * nums.pop())
}
```
- 解釋：可以確定的是最大值一定會被選，剩下兩個數可以是負的且最小的兩個，或是剩下兩個正的且最大的；找負數且最小的只需要取最小的就好，因為即使最小值是正的，也會在比大小時被排除，因此判斷正負其實是多餘的。這個方法的缺點就是先整個排序，需要 O(nlogn)，但是其實真的會用到的數只有最大的三個跟最小的兩個，請看下方解法。
#### JavaScript (faster)
```javascript
var maximumProduct = function(nums) {
    let max1 = max2 = max3 = -1e4
    let min1 = min2 = 1e4
    nums.forEach(e => {
        if(e > max1)
            [max3, max2, max1] = [max2, max1, e]
        else if(e > max2)
            [max3, max2] = [max2, e]
        else if(e > max3)
            max3 = e
        
        if(e < min1)
            [min2, min1] = [min1, e]
        else if(e < min2)
            min2 = e
    })
    return max1 * Math.max(min1 * min2, max2 * max3)
}
```
- 解釋：只取最大的三個值跟最小的兩個值，整體時間只花 O(n)，而且代碼也很直觀。

#### Python3 (faster)
```python
def maximumProduct(self, nums: 'List[int]') -> 'int':
    max1 = max2 = max3 = -1000
    min1 = min2 = 1000
    for num in nums:
        if num > max1:
            max1, max2, max3 = num, max1, max2
        elif num > max2:
            max2, max3 = num, max2
        elif num > max3:
            max3 = num
        
        if num < min1:
            min1, min2 = num, min1
        elif num < min2:
            min2 = num
    
    return max1 * max(min1 * min2, max2 * max3)
```
---
