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
    2. 第一個 reduce：統計所有數字的牌數。比較需要注意的是中間是從下方精簡而來的：

        if(acc[cur])
            acc[cur]++
        else
            acc[cur] = 1

    3. 第二個 reduce：遍歷 `dic` 的 `values` (每個數字的牌數)，找出所有牌數的 GCD，這邊用的技巧是兩兩找出 GCD，最後會得到的是整組牌數的 GCD。

#### Python3 (GCD, import libraries)
```python
def hasGroupsSizeX(self, deck: List[int]) -> bool:
    from collections import Counter
    from functools import reduce
    from math import gcd
    return reduce(gcd, collections.Counter(deck).values()) >= 2
```
- 解釋：
    >>> Counter([1,2,3,4,4,3,2,1])
    Counter({1: 2, 2: 2, 3: 2, 4: 2})
    >>> Counter([1,2,3,4,4,3,2,1]).values()
    dict_values([2, 2, 2, 2])

    `reduce(gcd, 可迭代對象)` 會將可迭代對象中的第一和第二個元素作gcd，結果值再對第三個元素作gcd，以此類推做到迭代結束。

#### Python3 (GCD)
```python
def hasGroupsSizeX(self, deck: List[int]) -> bool:
    # implement gcd function by using Euclid Algorithm
    def gcd(x, y):
        while y != 0:
            x, y = y, x % y
        return x
    
    # Counter
    counter = {}
    for card in deck:
        if card in counter:
            counter[card] += 1
        else:
            counter[card] = 1
    
    # reduce(gcd, iterable)
    vals = list(counter.values())
    if len(vals) == 1:
        return vals[0] >= 2
    
    ret = gcd(vals[0], vals[1])
    for i in range(2, len(vals)):
        ret = gcd(ret, vals[i])
    
    return ret >= 2
```
- 解釋：如果自己實作全部的東西，就會長這樣子。
---
#### Golang
```golang
func gcd (x, y int) int{
    for y != 0{
        x, y = y, x % y
    }
    return x
}

func hasGroupsSizeX(deck []int) bool {

    counter := make(map[int]int)
    for _, val := range(deck){
        counter[val] ++
    }
    ret := 0
    for _, val := range(counter){
        ret = gcd(ret, val)
    }
    return ret >= 2
}
```
