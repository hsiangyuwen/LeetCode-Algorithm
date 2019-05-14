
## [605. Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)
> :black_nib: Ray
### 題目解釋
	給一串 1 跟 0 的陣列及 n，不能有連續的 1，判斷最多能將多少個 0 取代成 1 且至少比 n 多。
### 審題注意
    1. Greedy(有最佳子結構)。
    2. 小心邊界，邊界也可以放花，所以可以在首尾各放一個 0，就可以不用特別判斷邊界條件。
    3. 可以不用遍歷整個陣列，如果已經確定可以放 n 個就停止。
### 解法
#### JavaScript (直觀解)
```javascript
var canPlaceFlowers = function(flowerbed, n) {
    let count = 0
    for(let i = 0; i < flowerbed.length && count < n; ++i) {
        if(flowerbed[i] === 0) {
            let prev = (i === 0)? 0: flowerbed[i - 1]
            let next = (i === flowerbed.length - 1)? 0: flowerbed[i + 1]
            if(prev === 0 && next === 0)
                ++count, ++i
        }
    }
    return n === count
}
```
- 解釋：
	1. 求 `prev` 及 `next` 時，小心處理邊界情況。
	2. 如果前後都是 0，則確定可以放花 `count++` 並且跳過下一個，因為下一個必定不能放花。
#### JavaScript (精簡)
```javascript
var canPlaceFlowers = function(flowerbed, n) {
    flowerbed = [0, ...flowerbed, 0]
    for(let i = 1; i < flowerbed.length - 1 && n; ++i) 
        if(flowerbed[i - 1] + flowerbed[i] + flowerbed[i + 1] === 0)
            --n, ++i

    return n == 0
}
```
- 解釋：
	1. 在前後各插入一個 0，如此就不用判斷邊界條件。
	2. 因為陣列只有 0 跟 1，所以判斷多個是否為 0 時只需要把全部加起來。
	3. 發現可以放置的位置直接將 `n` 減一，可以想像成：事先給你 `n` 朵新花，沿著花圃種，種到手上沒花就結束；如果走完整個花圃手上還有花，則代表情況不符合。

#### Python3 (精簡)
```python
def canPlaceFlowers(self, flowerbed: 'List[int]', n: 'int') -> 'bool':
    length = len(flowerbed)
    flowerbed = [0] + flowerbed + [0]
    i = 1
    while n > 0 and i <= length:
        if (flowerbed[i - 1] + flowerbed[i] + flowerbed[i + 1]) == 0:
            n -= 1
            i += 1
        i += 1
    
    return n == 0
```

#### Python3 (One-pass)
```python
def canPlaceFlowers(self, flowerbed: 'List[int]', n: 'int') -> 'bool':
    flowerbed = [0] + flowerbed + [0, 1]
    prev_1_index = -1
    for i in range(len(flowerbed)):
        if flowerbed[i] == 1:
            zero_length = i - prev_1_index - 1
            n -= (zero_length - 1) // 2
            prev_1_index = i
    
    return n <= 0
```
- 解釋：0的長度要 (2k+1) 才可以放k朵花。將邊界改善完後，遍歷一次判斷各段連續的0的長度，求出各段能放多少朵花，看總和是否大於等於n。

#### Golang
```golang
func canPlaceFlowers(flowerbed []int, n int) bool {
    flowerbed = append([]int{0}, append(flowerbed, 0)...)
    for i:=1 ; i<len(flowerbed)-1; i++{
        if (flowerbed[i-1] | flowerbed[i] | flowerbed[i+1]) == 0{
            flowerbed[i] = 1
            n--
        }
        if n <= 0{
            return true
        }
    }
    return false
}
```
- 解釋：用位元or的方式寫效率會好一點點點。
---
