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
---
