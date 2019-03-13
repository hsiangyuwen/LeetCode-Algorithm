## [189. Rotate Array](https://leetcode.com/problems/rotate-array/)
> :black_nib: Cropse
### 題目解釋
給一定值K,把list向右平移K次
### 審題注意
題目是inplace，沒有給你回傳值的意思。
另外提到至少有三種做法，所以都做一做吧。

另外要注意的是，K可能會大於nums的長度，所以如果做法是用切的，需要先把k取成除以len(nums)的餘數。
### 解法
1.Brute Force
2.S(N) Extra Array
3.Cyclic Replacements
#### Golang (Brute Force)
```go
func rotate(nums []int, k int)  {
    for i:=0; i<k; i++{
        for j:=len(nums)-1; j>0;j--{
            nums[j], nums[j-1] = nums[j-1], nums[j]
        }
    }
}
```
- 解釋
  按照題目向右平移，類似氣泡的做法。
- 優/缺點
  O(n*k)
  很爛，會爆掉。 
#### Golang (Extra Array)
```go
func rotate(nums []int, k int)  {
    temp := make([]int, len(nums))
    k = k%len(nums)
    for i:=0;i<len(nums);i++{
        temp[i] = nums[(len(nums)-k+i)%len(nums)]
    }
    for i:=0;i<len(nums);i++{
        nums[i] = temp[i]
    }
}
```
- 解釋
  Extra array先擺好，後面再存進去
- 優/缺點
  O(n) S(n)

#### Golang (Cyclic Replacements)
```go
func rotate(nums []int, k int) {
	l := len(nums)
	k = k % l
	count := 0
	for i := 0; count < l; i++ {
		j := i
		cur := nums[i]
		for {
			j = (j + k) % l
			nums[j], cur = cur, nums[j]
			count++
			if j == i {
				break
			}
		}
	}
}
```
- 優/缺點
  O(n) S(1)

#### Python3 (Extra Array)
```python
def rotate(self, nums: 'List[int]', k: 'int') -> 'None':
    """
    Do not return anything, modify nums in-place instead.
    """
    k = k%len(nums)
    temp = nums[-k:] + nums[:-k]
    for i in range(len(nums)):
        nums[i] = temp[i]
```

#### Python3 (Cyclic Replacements)
```python
def rotate(self, nums: 'List[int]', k: 'int') -> 'None':
    """
    Do not return anything, modify nums in-place instead.
    """ 
    k = k%len(nums)
    start = 0
    swap_count = 0
    
    def _get_next_index(cur):
        return (cur + k) % len(nums)
    
    while swap_count < len(nums):
        cur = start
        to_be_swap = nums[start]
        while True:
            _next = _get_next_index(cur)
            to_be_swap, nums[_next] = nums[_next], to_be_swap
            cur = _next
            swap_count += 1
            
            if start == cur:
                start += 1
                break
```

#### JavaScript (3 Reverse)
```javascript
const reverse = (nums, start, end) => {
    while(start < end)
        [nums[start++], nums[end--]] = [nums[end], nums[start]]       
}

var rotate = (nums, k) => {
    k %= nums.length
    reverse(nums, 0, nums.length - 1)
    reverse(nums, 0, k - 1)
    reverse(nums, k, nums.length - 1)
}
```

#### JavaScript (Cyclic Replacements)
```javascript
var rotate = (nums, k) => {
    k %= nums.length
    const getNextIndex = i => (i + k) % nums.length
    let start = swapCount = 0
    while(k && swapCount < nums.length) {
        cur = start
        toBeSwap = nums[start]
        while(true) {
            let _next = getNextIndex(cur)
            toBeSwap = [nums[_next], nums[_next] = toBeSwap][0]
            cur = _next
            swapCount++
            if(cur === start) {
                start++
                break
            }
        }
    }
}
```

---
