
## [989. Add to Array-Form of Integer](https://leetcode.com/problems/add-to-array-form-of-integer/)
> :black_nib: Ray
### 題目解釋
    A 陣列中的元素代表位元，因此 A 是陣列型式的一個整數，將此數加上 K 並用陣列型式表示。
### 審題注意
1. 小心進位的處理。
2. 可以把 `K` 當作是進位，從低位數(右邊)開始，每位數加進位(`K`)後求對 10 的餘數，最後除以 10 當作是下個進位。
3. 不建議 `reverse`，因為要做兩次，並且不會讓操作簡化。
4. 不行將 `A` 轉成 Integer，因為 `A` 長度最長可以到 10000。
### 解法
#### JavaScript
```js
var addToArrayForm = function(A, K) {
    for(let i = A.length - 1; i >= 0; --i) {
        A[i] += K
        K = Math.floor(A[i] / 10)
        A[i] %= 10
    }
    while(K) {
        A.unshift(K % 10)
        K = Math.floor(K / 10)
    }
    return A
}
```
> 也可以將取進位合併
```js
var addToArrayForm = function(A, K) {
    let flag = A.length - 1
    while(K) {
        if(flag < 0) {
            A.unshift(K % 10)
        } else {
            K += A[flag]
            A[flag--] = K % 10
        }
        K = Math.floor(K / 10)
    }
    return A
}
```
- 解釋：用 `flag` 指向每個位數 (從低位數到高位數)，如果 `flag < 0` 則代表已經超過原本 `A` 的位數了，只需要將剩下的 `K` 補到高位數即可。
---
