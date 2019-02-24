## [941. Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array/)
> :black_nib: Ray
### 題目解釋
    給一組數列，前段必須是嚴格遞增，後段必須是嚴格遞減，可以想像成爬山(只有一個山頂)。
### 審題注意
    從左側爬到山頂(最大值)，並記錄位置；再從右側爬到山頂，判斷兩個山頂位置是否相同。
### 解法
#### JavaScript
```javascript
var validMountainArray = function(A) {
    let fromLeft = 0, fromRight = A.length - 1
    while(A[fromLeft] < A[fromLeft + 1] && fromLeft < A.length - 1)
        fromLeft++
    while(A[fromRight] < A[fromRight - 1] && fromRight > 0)
        fromRight--
    return fromLeft > 0 && fromLeft === fromRight && fromRight < A.length - 1
}
```
- 解釋：非常直觀的解法，以 `fromLeft` 為例，判斷下一個是否比它大並且不超出陣列範圍，否則就停在最高點(可能是 local maximum)或邊界，同理 `fromRight`。在最後判斷 `fromLeft` 或 `fromRight` 是否在起點(例如：`[1, 2]` 或 `[3, 3, 4]` 這種特殊例子)，並且是否在高點相遇(如果有多個 local maximum，它們會分別卡在不同的高點)。

#### Python3
```python
def validMountainArray(self, A: List[int]) -> bool:
    if len(A) <= 2:
        return False
    
    from_left = 0
    from_right = len(A) - 1
    while from_left < len(A) - 1 and A[from_left] < A[from_left + 1]:
        from_left += 1
    while from_right > 0 and A[from_right] < A[from_right - 1]:
        from_right -= 1
    return from_left > 0 and from_left == from_right and from_right < (len(A) - 1)
```
- Tips: `while from_left < len(A) - 1 and A[from_left] < A[from_left + 1]:` 條件先後順序不能寫錯，不然會在判斷符不符合條件時有list index out of range的問題。

#### Python3 (One Pass)
```python
def validMountainArray(self, A: List[int]) -> bool:
    i = 0

    # walk up
    while i < len(A) - 1 and A[i] < A[i + 1]:
        i += 1

    # peak can't be first or last
    if i == 0 or i == len(A) - 1:
        return False

    # walk down
    while i < len(A) - 1 and A[i] > A[i + 1]:
        i += 1

    return i == len(A) - 1
```
- 解釋：這是官方solution。詳細去算步數會發現我們自己使用的解法和官方解法，步數都是array的長度。
---
