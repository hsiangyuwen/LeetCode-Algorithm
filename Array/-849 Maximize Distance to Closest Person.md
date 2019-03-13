## [849. Maximize Distance to Closest Person](https://leetcode.com/problems/maximize-distance-to-closest-person/)
> :black_nib: Sammy
### 題目解釋
	給定一個名為seats的陣列表示一排座位，裡面只有 `0` 和 `1` 兩種element。
	`1` 代表位子已經有人坐了，`0` 代表位子是空的。
	今天有一個人要入座，求最遠可以離旁邊已入座的人多遠。

	seats測資保證至少一個0與一個1，因此保證可以入座。
### 審題注意
	假設seats為 [0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0]，即：

	索引： 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10| 11
	座位： 0   0   1   0   0   0   1   0   1   0   0   0

	可以將seats分割成三部分去思考：

	1. 入座左半邊（選擇座位0或座位1）
	2. 入座中間部分，即左右都有人的狀況（選擇座位3, 4, 5, 7）
	3. 入座左半邊（選擇座位9, 10, 11）

### 解法
	遍歷一次seats，分成上述的三部分去處理：
	1：最遠距離為（最左邊已入座的人的座位index - 0）
	2：最遠距離為（左右兩側的人的距離）除以 2
	3：最遠距離為（最右側座位index - 最右邊已入座的人的座位index）

	計算第二部份時會發現該部分距離即為 int((right_index - left_index) / 2) ，即無條件捨去小數點。

	時間複雜度 O(n)，空間複雜度 O(1)
#### Python3
```python
def maxDistToClosest(self, seats):
    """
    :type seats: List[int]
    :rtype: int
    """
    
    # Handle the left bound
    for i in range(len(seats)):
        if seats[i] == 1:
            largest_distance = i
            seated_index = i
            start = i + 1
            break
    
    # Handle the middle part
    for i in range(start, len(seats)):
        if seats[i] == 1:
            largest_distance = max(largest_distance, (i - seated_index)//2)
            seated_index = i
    
    # Handle the right bound
    largest_distance = max(largest_distance, len(seats) - 1 - seated_index)
    
    return largest_distance

```
#### Golang
```go
func maxDistToClosest(seats []int) int {
    max := 0
    sideMax := math.MinInt64
    distance := 0
    for i := range seats{
        if seats[i] == 1{
            if max <= distance{
                max = distance
                if sideMax == math.MinInt64{
                    sideMax = max
                }
            }
            distance = 0
        }else{
            distance ++
        }
    }
    if sideMax < distance{
        sideMax = distance
    }
    return maxInt((max+1)/2, sideMax)
}
func maxInt(a,b int) int{
    if a > b{
        return a
    }
    return b
}
```
---
