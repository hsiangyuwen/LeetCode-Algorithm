## [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
> :black_nib: Cropse, Sammy
### 題目解釋
    給定一個陣列nums，求元素相加最大的子陣列。回傳相加後的值
### 審題注意
    內含負值的子陣列也有可能是所求的子陣列。
    例如：[4, -1, 5, -2]，所求的子陣列為[4, -1, 5]。
### 解法
> 加總法

遍歷一次，不斷加總於一個變數，如果這個變數低於0的話，將它設為0，即重新加總。（正在計算的子陣列不該被列進所求的子陣列中。）另於同一個迴圈中維護一個變數max，如果加總變數大於max就更新max。

複雜度為O(n)

#### Go (Aggregate)
```go
func maxSubArray(nums []int) int {
    max := nums[0]
    val := 0
    for _, i := range nums{
        val += i
        if val > max{
            max = val
        }
        if val < 0{
            val = 0
        }
    }
    return max
}
```

#### Python3 (Aggregate)
```python
def maxSubArray(self, nums: 'List[int]') -> 'int':
    max_sum = nums[0]
    val = 0
    for num in nums:
        val += num
        if val > max_sum:
            max_sum = val
        if val < 0:
            val = 0
    
    return max_sum
```

#### JavaScript (Aggregate)
```javascript
var maxSubArray = function(nums) {
    let maxSum = nums[0], val = 0
    nums.forEach(num => {
        maxSum = Math.max(maxSum, val += num)
        val = Math.max(val, 0)
    })
    return maxSum
}
```

---
 > Kadane's algorithm

我們也可以把這個解法當作是Dynamic Programming解法。

想法是將一個陣列整理出disjoint sets，第0個set為「最後一個元素，index為0」的所有子陣列，第1個set為「最後一個元素，index為1」的所有子陣列，以此類推。（在這一題當中不考慮空陣列sum為0的狀況）
由這些disjoint sets可以整理出一個長度為原始陣列長度的list（以下稱它為sum_list），第i個元素代表第i個set裡面的所有子陣列中，能構出最大和值的子陣列的和值。

這個list可以怎麼簡單求出來呢？其實是有規律的。關鍵在 `sum_list[i]` 與 `sum_list[i-1]` 和 `nums[i]` 的關係：

    sum_list[i] = max(sum_list[i-1], sum_list[i-1] + nums[i])

這一題其實就是要求sum_list中的最大元素值。而且這個演算法經過改良可以做到記錄這個子陣列起終點在哪裡。
時間複雜度為O(n)。

Ref: [Wikipedia](https://en.wikipedia.org/wiki/Maximum_subarray_problem)

#### Python3 (Kadane's Algorithm)
##### tags: `Kadane's Algorithm`
```python
def maxSubArray(self, nums: 'List[int]') -> 'int':
    max_ending_here = max_so_far = nums[0]
    for i in range(1, len(nums)):
        max_ending_here = max(nums[i], max_ending_here + nums[i])
        max_so_far = max(max_so_far, max_ending_here)
    
    return max_so_far
```

#### JavaScript (Kadane's Algorithm)
##### tags: `Kadane's Algorithm`
```javascript
var maxSubArray = function(nums) {
    let maxSoFar = nums[0]
    for(let i = 1, maxEndHere = nums[0]; i < nums.length; ++i) {
        maxEndHere = Math.max(nums[i], maxEndHere + nums[i])
        maxSoFar = Math.max(maxSoFar, maxEndHere)
    }
    return maxSoFar
}
```

#### C++
##### tags: `Kadane's Algorithm`
```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int answer_max = -2147483647;
        int cul = 0;
        for(int i = 0; i < nums.size(); i++){
            cul = max(cul+nums[i], nums[i]);
            if(cul > answer_max) answer_max = cul;
        }
        return answer_max;
    }
};
```

---
> Divide and Conquer

Too Long; Don't Read -- 和其他作法比起來慢2倍，不要考慮此做法

假設 `nums = [8,-1,2,-9,3]` ，分問題的方式：
1. 左邊子問題：`[8,-1]`
2. 右邊子問題：`[-9,3]`
3. 後處理前需求出：`中間向左之子陣列最大和值 + 2(中間值) + 中間向右之子陣列最大和值`
4. 後處理：回傳 `max(1. , 2. , 3.)`

#### Python3 (Divide and Conquer)
##### tags: `Divide and Conquer`
```python
def maxSubArray(self, nums: 'List[int]') -> 'int':
    def max_sub(nums: 'List[int]', left: 'int', right: 'int') -> 'int':
        if left == right:
            return nums[left]
        if left > right:
            return -10000000000  # imitate INT_MIN since there is no minvalue in Python3
        
        mid = left + (right - left) // 2
        lmax = max_sub(nums, left, mid - 1)
        rmax = max_sub(nums, mid + 1, right)
        
        temp_total = left_extend_max_sum = 0
        for i in range(mid - 1, left - 1, -1):  # mid-1, mid-2, ... , left
            temp_total += nums[i]
            left_extend_max_sum = max(left_extend_max_sum, temp_total)
        
        temp_total = right_extend_max_sum = 0
        for i in range(mid + 1, right + 1):  # mid+1, mid+2, ... , right
            temp_total += nums[i]
            right_extend_max_sum = max(right_extend_max_sum, temp_total)
        
        return max((left_extend_max_sum + nums[mid] + right_extend_max_sum), lmax, rmax)
    
    return max_sub(nums, 0, len(nums) - 1)
```

#### JavaScript (Divide and Conquer)
##### tags: `Divide and Conquer`
```javascript
const maxSub = (nums, left, right) => {
    if(left === right)
        return nums[left]
    else if(left > right)
        return Number.NEGATIVE_INFINITY
    
    let mid = Math.floor((left + right) / 2)
    let lmax = maxSub(nums, left, mid - 1)
    let rmax = maxSub(nums, mid + 1, right)
    
    let leftExtendMaxSum = rightExtendMaxSum = 0
    for(let i = mid - 1, tempTotal = 0; i >= left; --i)
        leftExtendMaxSum = Math.max(tempTotal += nums[i], leftExtendMaxSum)

    for(let i = mid + 1, tempTotal = 0; i <= right; ++i)
        rightExtendMaxSum = Math.max(tempTotal += nums[i], rightExtendMaxSum)

    return Math.max(leftExtendMaxSum + nums[mid] + rightExtendMaxSum, lmax, rmax)
}

var maxSubArray = nums =>
    maxSub(nums, 0, nums.length - 1)
```

---


