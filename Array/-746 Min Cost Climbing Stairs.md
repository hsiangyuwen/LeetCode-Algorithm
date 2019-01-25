
## [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)
> :black_nib: Ray
### 題目解釋
    可以一次爬兩階或是一階，找出最低 cost 總和。
### 審題注意
    Greedy + DP (可以拆成小事件且可求出局部最優解)
### 解法
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
---
