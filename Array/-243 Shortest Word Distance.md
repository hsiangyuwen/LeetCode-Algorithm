## [243. Shortest Word Distance](https://leetcode.com/problems/shortest-word-distance/)
> :black_nib: Sammy
### 題目解釋
    給定一個word list以及兩個word。找出兩個word在這個word list當中的最短距離。
    題目假設兩個word不同，且兩個word一定都在這個word list當中。
### 審題注意
    直觀上會走這兩步：
    1. 求出這兩個word在這個word list的所有位置，形成兩個數列。
    2. 再把這兩個數列的元素互相比較，找出最小的相減絕對值(即距離)。
    
    如果把上述兩步合併成一個步驟，就會是一個好的程式寫法：
    
        走訪一次這個word list，
        和word1相同就記錄下這個位置為index1，和word2相同就記錄下這個位置為index2。
        只要有做了記錄的動作(index1或index2有更新)，就去看
            1. index1 - index2 的距離
            2. 之前求的距離值(如果是第一次求距離值，那就想像前一次距離值是個很大的值)
        取兩者中較小的。
        
    仔細想就會發現這樣走訪完之後，其實就把第二步順便做掉了。
    
    複雜度為O(n)。
        
### 解法
#### Python3 (1-layer for loop)
##### tags: `for loop`
```python
def shortestDistance(self, words, word1, word2):
    """
    :type words: List[str]
    :type word1: str
    :type word2: str
    :rtype: int
    """

    # Assume that word1 does not equal to word2, and word1 and word2 are both in the list.
    # It's obvious that the return value won't be bigger than the length of word list.
    ret = len(words)
    index1 = -1
    index2 = -1
    for i in range(0, len(words)):
        if words[i] == word1: index1 = i
        if words[i] == word2: index2 = i

        if index1 != -1 and index2 != -1:  # Update value when both index1 and index2 are assigned.
            ret = min(ret, abs(index1 - index2))

    return ret

```
- Tips：
    1. 把index1和index2都先設成-1。都不是-1(都被assign了)時才去更新距離值。
    2. 可以用 `for i, word in enumerate(words)` 去做，但enumerate會吃效能。
- 解釋：這份code沒有實作「任一index有更新」才去更新距離值。不管有沒有做，複雜度都是O(n)。

---
