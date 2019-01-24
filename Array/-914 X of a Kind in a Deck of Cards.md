## [914. X of a Kind in a Deck of Cards](https://leetcode.com/problems/x-of-a-kind-in-a-deck-of-cards/)
> :black_nib: Ray
### 題目解釋
    給一組牌，判斷是否能將這組牌分成若干疊，每疊張數相同且至少兩張一樣的數字。
### 審題注意
    1. 統計每個數字的牌數
    2. 找出牌數的最大公因數 (GCD)
    3. 如果 GCD 大於等於 2 回傳 true
### 解法
#### JavaScript (GCD)
##### tags: `GCD`
```javascript
const gcd = (a, b) => !b ? a : gcd(b, a % b)

var hasGroupsSizeX = function(deck) {
    let dic = deck.reduce((acc, cur) => {
        acc[cur] = (acc[cur] || 0) + 1
        return acc
    }, {})
    return Object.values(dic).reduce((acc, cur) =>
        gcd(acc, cur), 0) >= 2
}
```
- 解釋
    1. 最大公因數(GCD)：這個是輾轉相除法的精髓，不在這贅述。
    2. 第一個 reduce：統計所有數字的牌數。比較需要注意的是中間是從下方精簡而來的
    ```javascript=
    if(acc[cur])
        acc[cur]++
    else
        acc[cur] = 1
    ```
    3. 第二個 reduce：遍歷 `dic` 的 `values` (每個數字的牌數)，找出所有牌數的 GCD，這邊用的技巧是兩兩找出 GCD，最後會得到的是整組牌數的 GCD。
---
