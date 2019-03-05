
## [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
> :black_nib: Ray
### 題目解釋
    可以一次爬兩階或是一階，找出最低 cost 總和。
### 審題注意
    Greedy + DP (可以拆成小事件且可求出局部最優解)
### 解法
官方hint為：
`f[i]: final cost to climb the staircase from some step i`
`f[i] = cost[i] + min(f[i+1], f[i+2])` 
意即：「我走到top之前要經過這步的話，要這麼多cost。」
若要用這個方法，就要從cost這個陣列的最右邊往回算。可以看作是Top-down DP。
最後， `min(f[0], f[1])` 即為解答。

討論區的解法偏向推薦Bottom-up DP，也就是先算好初始值，之後就由前面算好的值繼續往右算到結束。
`c[i]: "previous cost + cost of using step i" for climbing to some step i`
`c[i] = cost[i] + min(c[i-1], c[i-2])`
意即：「我走到step i，我之前最小用了多少cost，加上step i 這部要用的cost。」
最後， `min(c[len(cost) - 1], c[len(cost) - 2])` 即為解答。

上面兩個解法都可以簡化成只使用兩個變數去記錄之前算好的，使得空間複雜度為O(1)

#### JavaScript (Greedy, DP)
##### tags: `Greedy`, `DP`
```javascript
var minCostClimbingStairs = function(cost) {
    for(let i = 2; i < cost.length; ++i)
        cost[i] += Math.min(cost[i - 1], cost[i - 2])
    return Math.min(cost.pop(), cost.pop())
}
```
- 解釋：
    1. 貪心演算法(Greedy)：可以從局部最優解得到全局最優解。所以先從前三階開始想(局部最優解)，第三階一定是從第一階或是第二階相較低的 `cost` 而來，所以只要解決掉前三階的局部問題，第四階就是從第二階或第三階而來，直至最後就是我們要的答案。
    2. 動態規劃(Dynamic Programming)：這種局部最優解或最優子結構問題，就可以用 DP 去解決，事實上即是一個填表的過程。

        | COST | 1 | 100 | 1 | 1 | 1 | 100 | 1 | 1 | 100 | 1 |
        | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
        | DP  | 1 | 100 | 2 | 3 | 3 | 103 | 4 | 5 | 105 | 6 |

    3. 最後取值回傳可能會需要思考一下，取最後兩個相對小的值是因為：可以從倒數第二或倒數第一到達最頂層，不要忘記經過 DP 後的是到達那層所需花費 `cost` 的最低總合。

#### Python3 (Top-down DP)
##### tags: `DP`
```python
def minCostClimbingStairs(self, cost: 'List[int]') -> 'int':
    f_ip1 = f_ip2 = 0
    
    for c in reversed(cost):
        f_ip1, f_ip2 = c + min(f_ip1, f_ip2), f_ip1
    
    return min(f_ip1, f_ip2)
```

#### Python3 (Bottom-up DP)
##### tags: `DP`
```python
def minCostClimbingStairs(self, cost: 'List[int]') -> 'int':
    c_im1 = cost[1]
    c_im2 = cost[0]
    
    for i in range(2, len(cost)):
        c_im2, c_im1 = c_im1, cost[i] + min(c_im2, c_im1)  # c[i-2], c[i-1] = c[i-1], c[i]
    
    return min(c_im2, c_im1)
```
#### Goalgn(Bottom-up DP)
```go
func minCostClimbingStairs(cost []int) int {
    fm1 := cost[1]
    fm2 := cost[0]
    for i:=2; i<len(cost); i++{
        fm2, fm1 = fm1, cost[i] + min(fm1, fm2)
    }
    return min(fm2, fm1)
}
func min(a, b int)int{
    if a < b{
        return a
    }else{
        return b
    }
}
```
---
