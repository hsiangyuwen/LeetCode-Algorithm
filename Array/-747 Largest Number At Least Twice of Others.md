## [747. Largest Number At Least Twice of Others](https://leetcode.com/problems/largest-number-at-least-twice-of-others/)
> :black_nib: Sammy
### 題目解釋
    給定一個數列nums，裡面只有一個最大的數。
    如果這個最大的數都是其他的數的兩倍(含)以上，就回傳該數的index。
    如果不符合以上條件，就回傳-1。
### 審題注意
    這題和2nd-largest題相似。
    
    先記下最大數的index（O(n)），再維護一個max_heap得到第二大的數（O(n)）做比較。
### 解法
#### Python3 (heap)
##### tags: `heap`
```python
def dominantIndex(self, nums):
    """
    :type nums: List[int]
    :rtype: int
    """
    if len(nums) == 1:
        return 0
    
    H = nums[:2]  # First 2 element.
    heapq.heapify(H)
    
    # Maintain a size == 2 heap.
    # H[0] is 2nd_largest
    # H[1] is largest
    for i in range(2, len(nums)):
        if nums[i] >= H[0]:
            heapq.heappushpop(H, nums[i])
    
    return nums.index(H[1]) if H[1] >= 2 * H[0] else -1
```

#### JavaScript
```javascript
var dominantIndex = function(nums) {
    let max1 = -1, max2 = -1, maxIndex = -1
    for(let i = 0; i < nums.length; ++i) {
        if(nums[i] > max1) {
            [max1, max2] = [nums[i], max1]
            maxIndex = i
        } else if (nums[i] > max2)
            max2 = nums[i]
    }
    return max1 >= max2 * 2 ? maxIndex : -1
}
```
#### Golang
```go
func dominantIndex(nums []int) int {
    m, max, max2 := 0, 0, 0
    for i := range nums{
        if max < nums[i]*2{
            max2 = max
            max = nums[i]*2
            m = i
        }else if max2 < nums[i]*2{
            max2 = nums[i]*2
        }
    }
    if nums[m] < max2{
        return -1
    }
    return m
}
```
#### Golang(heap)
```go
import (
	"container/heap"
	"fmt"
)

type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x interface{}) {
	// Push and Pop use pointer receivers because they modify the slice's length,
	// not just its contents.
	*h = append(*h, x.(int))
}

func (h *IntHeap) Pop() interface{} {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func dominantIndex(nums []int) int {
    if len(nums)<2{
        return 0
    }
	h := &IntHeap{nums[0], nums[1]}
	heap.Init(h)
    
	for i := 2; i < len(nums); i++ {
		if nums[i] >= (*h)[0] {
			heap.Remove(h, 0)
			heap.Push(h, nums[i])
		}
	}

	if (*h)[1] >= (*h)[0]*2{
        for i, val := range nums{
            if (*h)[1] == val{
                return i
            }
        }
	}
	return -1
}
```
---
