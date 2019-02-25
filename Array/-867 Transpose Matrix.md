## [867. Transpose Matrix](https://leetcode.com/problems/transpose-matrix/)
> :black_nib: Sammy
### 題目解釋
    給定一個m*n的矩陣，求其轉置矩陣（n*m）。
### 審題注意
    注意for迴圈的邊界即可。

    一般解法其實會使用到一個原矩陣大小的額外空間，空間複雜度為O(m*n)，
    可以使用cyclic-replacement的觀念去實作in-place transpose。
    時間複雜度並不會減少，且難寫難懂，故只附上補充連結。
    Ref: https://www.geeksforgeeks.org/inplace-m-x-n-size-matrix-transpose/
### 解法
#### Python3
```python
def transpose(self, A: List[List[int]]) -> List[List[int]]:
    ret = []
    for j in range(len(A[0])):
        row = []
        for i in range(len(A)):
            row.append(A[i][j])
        ret.append(row)
    
    return ret
```
#### Python3 (1-line)
```python
def transpose(self, A: List[List[int]]) -> List[List[int]]:
    return list(zip(*A))
```
- 解釋：`zip(*A)` 意同 `zip(A[0], A[1], ..., A[m])` ，也就會變成 `iter((A[0][0], A[1][0], ..., A[m][0]), ..., (A[0][n], A[1][n], ..., A[m][n]))` ，轉成list之後就變成轉置矩陣了。

---
