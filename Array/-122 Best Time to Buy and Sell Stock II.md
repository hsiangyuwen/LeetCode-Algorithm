## [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
> :black_nib: Ray
### 題目解釋
    不限買賣次數，所以只要後項大於前項，就可以列入營利計算
### 審題注意
    這題相當簡單，考察對 Array 的基本操作，不要把題目想得太複雜
### 解法
#### Javascript (1 line)
##### tags: `1 line`, `reduce`
```javascript
const maxProfit = prices => 
    prices.reduce((acc, cur, index) =>
        index > 0 ?
        acc + Math.max(0, cur - prices[index-1]) :
        acc
    , 0)
```
- 解釋
    從 `index` 1 開始跟前項作比較，如果比前項還要大(代表有利可圖)，就會把順差累積到 `acc` 中，最終 `reduce()` 回傳所有的 profit。
#### Javascript (more readable)
```javascript
var maxProfit = function(prices) {
    let profit = 0
    for(let i = 1; i < prices.length; ++i)
        if(prices[i] > prices[i-1])
            profit += prices[i] - prices[i-1]
    return profit
}
```
如果要更精簡，可以把 line 4, 5 合併為一行。
```javascript
profit += Math.max(0, prices[i] - prices[i-1])
```
#### Python3 (ternary operator)
##### tags: `ternary operator`
```python
def maxProfit(self, prices):
    ret = 0
    for i in range(1, len(prices)):
        ret += (prices[i] - prices[i-1]) if prices[i] > prices[i-1] else 0

    return ret
```
#### Go (ternary operator)
##### tags: `ternary operator`
```go
func maxProfit(prices []int) int {
	profit := 0
	for i := 1; i < len(prices); i++ {
		if prices[i-1] < prices[i] {
			profit += prices[i] - prices[i-1]
		}
	}
	return profit
}
```
---
