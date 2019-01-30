## [1. Two Sum](https://leetcode.com/problems/two-sum/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，nums裡有兩個數字總和會是target，請求出這兩個數字的index。
    題目已假定恰一組解。
### 審題注意
    這一題需要關注的點是所使用的方法的時間/空間複雜度。

    * Brute-Force：遍歷一次nums，針對每個數再去遍歷一次nums找看看有沒有數字能使得兩數相加等於target。
      * 時間複雜度：O(n^2)，因為使用雙層for迴圈。
      * 空間複雜度：O(1)，因為沒有再額外開空間。
    ~~解法區不提供暴力解。~~
    
    * Two-pointers：先排序，再利用2個pointer（一開始指在左右邊界）做夾擠。  
      （如果查詢的總和大於 `target`，那右邊界就往左走1 index，反之，左邊界就往右走1 index。）
      * 時間複雜度：O(nlogn)，因為排序。
      * 空間複雜度：O(1)，因為沒有再額外開空間。
    
    * Two-pass hashtable：先遍歷一次nums建好「key為值、value為這個值在nums的哪個index上」的hashtable，再遍歷一次nums，然後看hashtable有沒有(target - nums[i])這個key，并得到index。
      * 時間複雜度：O(n)，因為遍歷一次nums是O(n)，hashtable查找是O(1)。
      * 空間複雜度：O(n)，因為多開一個hashtable。

    * One-pass hashtable：邊建「key為值、value為這個值在nums的哪個index上」的hashtable，邊看hashtable有沒有(target - nums[i])這個key，并得到index。
      * 時間複雜度：O(n)，因為遍歷一次nums是O(n)，hashtable查找是O(1)。
      * 空間複雜度：O(n)，因為多開一個hashtable。但先找到就先return了，所以在某些馬上就找到兩個數字的index的情況下，跑很快就結束。

### 解法
#### Python (Two Pointers)
##### tags: `two-pointers`
```python
def twoSum(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    sorted_nums = nums[:]
    sorted_nums.sort()
    l = 0
    r = len(sorted_nums) - 1
    while(l<r):
        if sorted_nums[l] + sorted_nums[r] > target:
            r -= 1
        elif sorted_nums[l] + sorted_nums[r] < target:
            l += 1
        else:
            index_1 = nums.index(sorted_nums[l])
            for x in range(len(nums)):
                if nums[x] == sorted_nums[r] and x != index_1:
                    index_2 = x
            return [index_1, index_2]
```
- Tips：要保留著nums，所以要用slice先複製一份，再去sort，才不會影響到等等要參照原本的nums。
- 解釋：查詢的總和大於 `target`，那右邊界就往左走1 index，反之，左邊界就往右走1 index。
- 優/缺點：這題給定的nums是沒有sort過，卻又還要回傳兩數在nums中的index。不但要保留原數列做查找，還要排除掉兩數相等時，查找會找到同一個index的情況，非常麻煩。

#### Python (Two-pass hashtable)
##### tags: `hashtable`
```python
def twoSum(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    hashmap = {}

    for i in range(len(nums)):
        hashmap[nums[i]] = i

    for i in range(len(nums)):
        index = hashmap.get(target - nums[i], None)
        if index is not None and index != i:
            return [i, index]

```
- Tips：In Python, the Dictionary data types represent the implementation of hash tables.
- 解釋：這個實作方式下，i一定小於index，所以是return [i, index]。

```python
def twoSum(self, nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: List[int]
    """
    hashmap = {}
        
    for i in range(len(nums)):
        index = hashmap.get(target - nums[i], None)
        if index is not None:
            return [index, i]
        
        hashmap[nums[i]] = i
```
- Tips：In Python, the Dictionary data types represent the implementation of hash tables.
- 解釋：這個實作方式下，index一定小於i（因為是找之前存進hashtable的），所以是return [index, i]。

#### JavaScript (Two Pointers)
```javascript
var twoSum = function(nums, target) {
    let sortedNums = new Array(...nums).sort((a, b) => a - b)
    // es6
    // let sortedNums = [...nums].sort((a, b) => a - b)
    let l = 0, r = nums.length - 1
    while(l < r) {
        let sum = sortedNums[l] + sortedNums[r]
        if(sum > target)
            r--
        else if(sum < target)
            l++
        else 
            return [nums.indexOf(sortedNums[l]), nums.lastIndexOf(sortedNums[r])]
    }
}
```
#### JavaScript (One-pass hashtable)
```javascript
var twoSum = function(nums, target) {
    let hashMap = {}
    for(let i = 0; i < nums.length; ++i)
        if(hashMap[target - nums[i]] !== undefined)
            return [hashMap[target - nums[i]], i]
        else hashMap[nums[i]] = i
}
```
---
