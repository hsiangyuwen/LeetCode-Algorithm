## [888. Fair Candy Swap](https://leetcode.com/problems/fair-candy-swap/)
> :black_nib: Sammy
### 題目解釋
	Alice和Bob兩人各有一些不同大小的candy bar，兩者擁有的candy bar大小分別記錄在A、B兩個陣列中。
	兩人可分別選擇一個candy bar做交換。
	
	求若要使得交換後兩者擁有的candy bar總大小相同，
	Alice和Bob分別要拿出什麼大小的candy bar。

	保證答案存在；若答案有多組，回傳任何一組皆可。
### 審題注意
	以 A = [1,2,5], B = [2,4] 為例，總大小分別為8和6（A比B 大 2 ），
	所以要使得交換後變成7和7（A總和-1，B總和+1）。
	解為A拿出5，B拿出4（5比4 大 1）做交換。

	觀察多組就會發現皆符合以下兩個規則：
	1. 總和差距一定是偶數，不然不可能有解
	2. 拿出來做交換的大小差距 = (總和差距) / 2
### 解法
	找符合條件的candy bar大小，因為A、B陣列是沒有排序的，故直觀解法是O(n^2)。
	如果利用set，可以簡化成O(n)。
#### Python3 (set)
##### tags: `set`
```python
def fairCandySwap(self, A, B):
    # It is guaranteed an answer exists, so it is sure that (sum_A - sum_B) is even.
    target_diff = (sum(A) - sum(B)) // 2
    
    # Need to exchange the candy bars which (size_from_A - size_from_B) == target_diff
    set_A = set(A)
    for size in B:
        # (size_from_A - size_from_B) == target_diff  =>  size_from_A == size_from_B + target_diff
        if (size + target_diff) in set_A:
            return [size + target_diff, size]

```

#### JavaScript (set)
```javascript
var fairCandySwap = function(A, B) {
    let sumA = A.reduce((a, c) => a + c, 0)
    let sumB = B.reduce((a, c) => a + c, 0)
    let diff = (sumB - sumA) / 2
    let setB = new Set(B)
    for(let a of A)
        if(setB.has(a + diff))
            return [a, a + diff]
}
```
---
