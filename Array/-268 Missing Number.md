## [268. Missing Number](https://leetcode.com/problems/missing-number/)
> :black_nib: Ray
### 題目解釋
    有 n 個數字從 0, 1, 2, ..., n 選出，找出消失的那個數字
### 審題注意
    1. 總和公式解
    2. 直觀解
    3. XOR 解
    4. 如果他是排序好的，可以用二元搜尋
### 解法
#### JavaScript (公式解, 1 line solution)
##### tags: `1 line`
```javascript
const missingNumber = nums =>
    (nums.length * (nums.length + 1)) / 2 - nums.reduce((acc, cur) => acc + cur)
```
- 解釋
    1. 0 加到 n 的公式解是：

        <a  href="https://www.codecogs.com/eqnedit.php?latex=\frac{n&plus;1}{2}\cdot&space;(0&plus;n)"  target="_blank"><img  src="https://latex.codecogs.com/gif.latex?\frac{n&plus;1}{2}\cdot&space;(0&plus;n)"  title="\frac{n+1}{2}\cdot (0+n)"  /></a>

    2. 0 加到 n 再減去 `nums` 的總和即可得到消失的那個數字。
#### JavaScript (直觀解)
```javascript
var missingNumber = function(nums) {
    let sum = nums.length
    for(let i = 0; i < nums.length; ++i)
        sum += i - nums[i]
    return sum
}
```
- 解釋：把 0 加到 n 及減去 `nums` 的總和合併成一行。(`for` 迴圈中只加到 `nums.length - 1`，因此將 `sum` 的初始值設為 `nums.length`)
#### JavaScript
##### tags: `xor`, `bit operation`
```javascript
var missingNumber = function(nums) {
    let xor = nums.length
    for(let i = 0; i < nums.length; ++i)
        xor ^= i ^ nums[i]
    return xor
}
```
- 解釋：將抵銷的概念用 `xor` 去實踐，`xor` 兩個一樣的數會抵銷(`a ^ b ^ b = a`)，所以上述的作法，如果有出現在 `nums` 陣列中，會被 `xor` 抵銷，最後剩下的就是消失的。
> 1 line solution?
#### JavaScript (1 line)
##### tags: `1 line`, `xor`, `bit operation`
```javascript
const missingNumber = nums =>
    nums.reduce((acc, cur, index) => acc ^ cur ^ index, nums.length)
```

#### Python3 (公式解)
```python
def missingNumber(self, nums: 'List[int]') -> 'int':
    return len(nums) * (len(nums) + 1) // 2 - sum(nums)
```

#### Python3 (直觀解)
```python
def missingNumber(self, nums: 'List[int]') -> 'int':
    ret = len(nums)
    for i, num in enumerate(nums):
        ret += (i - num)
    return ret
```

#### Python3 (XOR)
```python
def missingNumber(self, nums: 'List[int]') -> 'int':
    return (set(range(0, len(nums)+1)) - set(nums)).pop()
```
#### Golang(公式解)
```go
func missingNumber(nums []int) int {
    sum := (1+len(nums))*len(nums)/2
    for _, val := range nums{
        sum -= val
    }
    return sum
}
```
#### Golang(直觀解)
```go
func missingNumber(nums []int) int {
    sum := len(nums)
    for i, val := range nums{
        sum += i - val
    }
    return sum
}
```
#### Golang(XOR)
```go
func missingNumber(nums []int) int {
    sum := len(nums)
    for i, val := range nums{
        sum ^= i ^ val
    }
    return sum
}
```
---
