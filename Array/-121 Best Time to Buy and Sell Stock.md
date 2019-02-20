## [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
> :black_nib: Ray
### 題目解釋
    只能一次買賣，直覺上的思考是：找到價格最低點，並且在最低點之後找到最高點，可得一次買賣最高利潤。
### 審題注意
- 根據上述想法：
    1. 暴力解就是找到價格最低點，並記下 `index` 往右搜尋最高點。這個方法最壞情況是要把整個 Array go through 兩次，有沒有可能 go through 一次就好？
    2. 有的。造訪每個 `price` 時，不停更新 `minPrice` 及 `maxProfit`，跑到結尾前都是 local minimum 及 local maximum(有點抽象，下方有實際例子及解法)。
- Maximum Subarray Problem (Kadane Algorithm)
### 解法
#### JavaScript (直觀解)
```javascript
var maxProfit = function(prices) {
    let maxProfit = 0
    for(let minPrice = prices[0], i = 1; i < prices.length; ++i)
        if(prices[i] < minPrice)
            minPrice = prices[i]
        else if(prices[i] - minPrice > maxProfit)
            maxProfit = prices[i] - minPrice
    return maxProfit
}
```
- 解釋
    ```
                Example 1.         Example 2.       Example 3.
                7 1 5 3 6 4        7 6 4 3 1        100 105  4   3   9
    minPrice    7 1 1 1 1 1        7 6 4 3 1        100 100  4   3   3
    maxProfit   0 0 4 4 5 5        0 0 0 0 0         0   5   5   5   6
    ```
    可以理解成：用當前最低價去計算當前最高利潤。如果有新的最低價，那之後便是根據新的最低價去判斷是否要更新最大利潤，可以看 `Example 3.` 稍微極端的例子。解法中間可以換成比較簡潔的寫法：
    ```javascript=
    for(let minPrice = prices[0], i = 1; i < prices.length; ++i) {
        minPrice = Math.min(minPrice, prices[i])
        maxProfit = Math.max(maxProfit, prices[i] - minPrice)
    }
    ```

#### Python3 (直觀解)
```python
def maxProfit(self, prices: 'List[int]') -> 'int':
    if not len(prices):
        return 0
    
    min_price = prices[0]
    max_profit = 0
    
    for i in range(1, len(prices)):
        max_profit = max(max_profit, (prices[i] - min_price))
        min_price = min(min_price, prices[i])
    
    return max_profit
```

#### JavaScript (Kadane Algorithm)
```javascript
var maxProfit = function(prices) {
    let maxCur = 0, maxSoFar = 0
    for(let i = 1; i < prices.length; ++i) {
        maxCur = Math.max(0, maxCur + prices[i] - prices[i - 1])
        maxSoFar = Math.max(maxSoFar, maxCur)
    }
    return maxSoFar
}
```
- 解釋：
    ```
                Example 1.         Example 3.
                7 1 5 3 6 4        100 105  4   3   9
    maxCur      0 0 4 2 5 3         0   5   0   0   6
    maxSoFar    0 0 4 4 5 5         0   5   5   5   6
    ```
    - Kadane 算法過程：
        1. 遍歷 `prices` ，累加價差 (`maxCur`)，當累加的價差小於或等於 0 時，從下個元素開始重新累加。
        2. 累加的過程中，用 `maxSoFar` 紀錄所獲得過的最大值。
        3. 經過一次遍歷，`maxSoFar` 存的即是片段最大價差
        4. [延伸-最大子數列問題](https://zh.wikipedia.org/wiki/%E6%9C%80%E5%A4%A7%E5%AD%90%E6%95%B0%E5%88%97%E9%97%AE%E9%A2%98)
    - 事實上，上述這兩種方法是異曲同工，Kadane 也是透過當前 `minPrice` 去更新 `maxProfit`，因為每當遇到更低的 price，便意味著要重新累加，重新累加就是根據最新的低價格去做後續的累加，在累加的過程中，去紀錄出現過的最大利潤。(可以多觀察例子中的兩種更新過程)
    - 意即，`max_list[i]` 與 `max_list[i-1]` 和 `prices[i] - prices[i-1]` 的關係如下：
        `max_list[i] = max(0, max_list[i-1] + (prices[i] - prices[i-1]))`

#### Python3 (Kadane's Algorithm)
```python
def maxProfit(self, prices: 'List[int]') -> 'int':
    max_ending_here = max_so_far = 0
    for i in range(1, len(prices)):
        max_ending_here = max(0, max_ending_here + (prices[i] - prices[i-1]))
        max_so_far = max(max_so_far, max_ending_here)
    
    return max_so_far
```
#### Golang(直觀解)
```go
func maxProfit(prices []int) int {
    if len(prices)<=1{
        return 0
    }
    max := 0
    min := prices[0]
    for i:=1; i<len(prices); i++{
        if prices[i] < min{
            min = prices[i]
        }
        if prices[i]-min > max{
            max = prices[i]-min
        }
    }
    return max
}
```
#### Golang(kadane's Algorithm)
```go
func maxProfit(prices []int) int {
    maxCur := 0
    max := 0
    for i:= 1; i<len(prices); i++{
        maxCur += prices[i] - prices[i-1]
        if maxCur < 0{
            maxCur = 0
        }else if max < maxCur{
            max = maxCur
        }
    }
    return max
}
```
---
