## [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
> :black_nib: Sammy
### 題目解釋
    給定一個由小到大排列好的數列nums（pass by reference），要使得：
    1. nums裏面不能有重覆的元素
    2. 回傳值為整理過後的nums（已無重覆元素）的長度
### 審題注意
    請特別注意Example所敘述的內容：

Given nums = [0,0,1,1,1,2,2,3,3,4],  
Your function should return length = 5,  
with the **first five elements of nums being modified** to 0, 1, 2, 3, and 4 respectively.  
It **doesn't matter what values are set beyond the returned length**.  

    也就是很明白的告訴你，假如說整理好的nums長度應為5，只要把nums前面5位整理成正確就好了。
    1. 實際上nums這個instance的長度不用去動。
    2. 第6位（含）以後的值都不用理會。
### 解法
    遍歷一次，遇到不一樣的就往前填就好了。舉例如下：
    `nums = [5,5,7,7]`：
    0. 先設定「現在紀錄著的值，它的位置(cur_index)」：0。（num[0] == 5）
    1. 第1位是5，和num[cur_index]相同，不用把他往前填。
    2. 第2位是7，遇到不一樣了，nums[cur_index + 1] == 5, cur_index = 1。
    3. 第3位是7，不用把他往前填。
    4. 遍歷完了。return長度為cur_index + 1 => 2。
#### Python (inplace)
##### tags: `inplace`
```python
def removeDuplicates(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    if len(nums) <= 1:
        return len(nums)
    
    cur_index = 0
    for i in range(1, len(nums)):
        if nums[i] != nums[cur_index]:
            nums[cur_index + 1] = nums[i]
            cur_index += 1
    
    return cur_index + 1
                
```
- 解釋：長度為0或1則不可能有duplicate的情況，直接回傳長度即可。
- 優/缺點：複雜度為O(n)
---
