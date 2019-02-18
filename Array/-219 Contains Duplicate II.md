## [219. Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，求下列兩項要求是否皆成立：
    1. 有重覆的數字
    2. 可以找到一個pair的重覆數字，他們的index距離小於等於(at most) k 。
### 審題注意
    題目解釋第2點，可以換句話說為：
        可以找到相鄰兩個重覆數字（[1,2,3,1]，index為0的1和index為3的1相鄰），他們的index距離小於等於(at most) k 。
    
    我們可以遍歷一次nums，並利用hashtable去記錄「前一次這個數字出現在哪」。
    有在hashtable裏面就代表有重覆，然後去比較紀錄，看距離能不能達成條件2。
### 解法
#### Python3 'hash'
##### tags: `hash`
```python
def containsNearbyDuplicate(self, nums, k):
    """
    :type nums: List[int]
    :type k: int
    :rtype: bool
    """
    if len(nums) == len(set(nums)):
        return False
    
    hashmap = {}
    for i in range(len(nums)):
        if nums[i] in hashmap:
            if i - hashmap[nums[i]] <= k:
                return True
        hashmap[nums[i]] = i
    
    return False
```
- 優/缺點：40ms, 100.00%

#### JavaScript (Hash)
##### tags: `hash`
```javascript
var containsNearbyDuplicate = function(nums, k) {
    let hashmap = {}
    for(let i = 0; i < nums.length; ++i)
        if(hashmap[nums[i]] >= 0 && i - hashmap[nums[i]] <= k)
            return true
        else
            hashmap[nums[i]] = i

    return false
}
```
---
#### Golang (Hash)
```go
func containsNearbyDuplicate(nums []int, k int) bool {
    temp := make(map[int]int)
    for i, val := range nums{
        if mapIndex, ok := temp[val]; i-mapIndex <= k && ok{
            return true
        }else{
            temp[val] = i
        }
    }
    return false
}
```
