## [985. Sum of Even Numbers After Queries](https://leetcode.com/problems/sum-of-even-numbers-after-queries/)
> :black_nib: Ray
### 題目解釋
    算出陣列中偶數元素的總和，並根據每次 Query 修改及重新計算總和。
### 審題注意
1. 不要每次修改後全部重新計算，時間複雜度會是 o(n + mn) (假設 Query m 次)。
2. 如果每次根據修改前的值及修改後的值去更新偶數總和，那時間複雜度會是 o(n + m)。
### 解法
#### JavaScript
```js
var sumEvenAfterQueries = (A, queries) => {
    let evenSum = A.reduce((acc, cur) => acc + (cur % 2 ? 0 : cur), 0)
    return queries.map(([val, idx]) => {
        if(A[idx] % 2 === 0) evenSum -= A[idx]
        A[idx] += val
        if(A[idx] % 2 === 0) evenSum += A[idx]
        return evenSum
    })
}
```

1. 請熟悉 `reduce` 及 `map` 的使用。
2. 在 JavaScript 中請小心**負數**的**求餘** ([Ref](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#%E6%B1%82%E4%BD%99_()))，判斷求餘是不是 0 比是不是 1 還要保險。
  ```js
    -1 % 2 // -1
    -4 % 2 // -0
    -0 === 0 // true
    -1 !== 1 // true
  ```
3. 事實上可以根據每種情況去對 `evenSum` 作出修改。
  ```js
        if(A[idx] % 2 === 0) {
            if(val % 2 === 0)
                evenSum += val
            else
                evenSum -= A[idx]
        } else if(val % 2 !== 0)
            evenSum += A[idx] + val
        A[idx] += val
  ```
4. 也可以將 `if(condition) statement` 改成 `(condition) && (statement)`。
  ```js
        (A[idx] % 2 === 0) && (evenSum -= A[idx])
        A[idx] += val;
        (A[idx] % 2 === 0) && (evenSum += A[idx])
  ```

---
