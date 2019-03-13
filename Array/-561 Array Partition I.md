## [561. Array Partition I](https://leetcode.com/problems/array-partition-i/)
> :black_nib: Sammy
### 題目解釋
    給定一數列nums，裡面有2n個整數。
    請試著把這2n個整數分成n個pair，使得每個pair的 min(num0, num1) 之總和最大，並回傳這個總和值。

    測資範圍：
    n: [1, 10000]
    元素的值: [-10000, 10000]
### 審題注意
    假設現在求出了每個pair的 avg(num0, num1) 的總和，
    我們可以發現把問題變成 min(num0, num1) 的時候，如果num0和num1相差越大，求出來的總和，比題目是 avg(num0, num1) 時的總和小越多。
    也就是說，在我們把這2n個整數分成n個pair的時候，要最小化每個pair中num0和num1的差距，才能使總和值最大。

    最直觀去實作這個想法的方式就是把nums由小到大排序，再由左至右兩個兩個組成pair。
    時間複雜度為O(nlogn)，空間複雜度為O(1)。

    若要求時間複雜度為O(n)，我們可以新開一個array，把每個數字的occurrence算出。
    再遍歷一次這個array（由小到大看每個數字的occurrence），組pair算總和值。
    這也就是有名的Counting Sort。
    假設元素的值範圍長度為m（比方說這題是 10000 - (-10000) + 1 = 20001），
    時間複雜度為O(m)，空間複雜度為O(m)。
### 解法
    這邊只示範新開array算occurrence的方法。
#### C++
```c++
class Solution {
public:
    int arrayPairSum(vector<int>& nums) {
        // n's range -10000 ~ 10000.
        // so we shift this range to 0 ~ 20000.
        // Implement Counting Sort, a O(n) sorting algorithm, to accelerate our program. 
        int bucket[20001] = {0};
        
        for(auto &i : nums)
            ++bucket[i+10000];
        
        int sum = 0;
        for(int i = 0, odd = 1, n = 0; i < 20001 && n < nums.size()/2; ++i){
            int num = bucket[i];
            while(num--){
                if(odd) sum += (i - 10000);
                n += odd;
                odd = !odd;
            }
        }
        return sum;
    } 
};
```
#### Python3
```python
def arrayPairSum(self, nums: List[int]) -> int:
    occur = [0] * (10000 - (-10000) + 1)
    flag = True
    
    # calculate occurrence
    for num in nums:
        occur[num + 10000] += 1
    
    # calculate sum
    ret = 0
    for i in range(len(occur)):
        while occur[i] > 0:
            if flag:
                ret += (i - 10000)
            flag = not flag
            occur[i] -= 1
    
    return ret
```
- 解釋：不使用collections.Counter，因為它並沒有保留key由小到大的順序性。
- 優/缺點：其實在每一次call這個function的時候都會開出20001個空間，並且確定都會走 len(nums) + 20001 步，測出來很容易不快。

#### JavaScript
```javascript
const arrayPairSum = nums => {
    const OFFSET = 1e4
    let occur = new Array(20001).fill(0)
    let min = 1e5, max = 0, ret = 0 - OFFSET * nums.length / 2
    nums.forEach(n => {
        n += OFFSET
        occur[n]++
        min = Math.min(min, n)
        max = Math.max(max, n)
    })
    
    for(let num = min, flag = 0; num <= max; ++num)
        while(occur[num]--)
            if(flag ^= 1)
                ret += num
    return ret
}
```
- 解釋：我事先減去 `OFFSET` 的值，可以避免最後計算答案時重複減去的操作，另外紀錄最小及最大值縮小最後遍歷的範圍，最後利用 XOR(`^`) 的特性實踐隔一個值紀錄的操作。
---
