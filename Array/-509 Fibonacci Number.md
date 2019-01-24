## [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)
> :black_nib: Sammy
### 題目解釋
    費波納契數列。對於N>1，第N個數為第N-1個數與第N-2個數的和。即：
    
        F(0) = 0,  F(1) = 1
        F(N) = F(N - 1) + F(N - 2), for N > 1
### 審題注意
~~1. 用for迴圈從小的F(N)算到大的F(N)~~  
~~2. 用遞迴的方式從大的F(N)往小的F(N)找~~  
~~3. 用黃金比例去求解~~  

    上述三種方法都很慢，刷題就是要用好方法！
    
    基於矩陣乘法，我們可以歸納出以下式子：
    
<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{bmatrix}&space;1&space;&&space;1\\&space;1&space;&&space;0&space;\end{bmatrix}^{N}&space;=&space;\begin{bmatrix}&space;F(N&plus;1)&space;&&space;F(N)\\&space;F(N)&space;&&space;F(N-1)&space;\end{bmatrix}&space;\forall&space;N&space;\in&space;\mathbb{N}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;1&space;&&space;1\\&space;1&space;&&space;0&space;\end{bmatrix}^{N}&space;=&space;\begin{bmatrix}&space;F(N&plus;1)&space;&&space;F(N)\\&space;F(N)&space;&&space;F(N-1)&space;\end{bmatrix}&space;\forall&space;N&space;\in&space;\mathbb{N}" title="\begin{bmatrix} 1 & 1\\ 1 & 0 \end{bmatrix}^{N} = \begin{bmatrix} F(N+1) & F(N)\\ F(N) & F(N-1) \end{bmatrix} \forall N \in \mathbb{N}" /></a>

    而我們可以歸納出以下算式：
    
<a href="https://www.codecogs.com/eqnedit.php?latex=F(2N&plus;1)=&space;F(N&plus;1)^{2}&plus;&space;F(N)^{2}&space;\\&space;F(2N)=&space;F(N)\cdot&space;(2F(N&plus;1)-&space;F(N))" target="_blank"><img src="https://latex.codecogs.com/gif.latex?F(2N&plus;1)=&space;F(N&plus;1)^{2}&plus;&space;F(N)^{2}&space;\\&space;F(2N)=&space;F(N)\cdot&space;(2F(N&plus;1)-&space;F(N))" title="F(2N+1)= F(N+1)^{2}+ F(N)^{2} \\ F(2N)= F(N)\cdot (2F(N+1)- F(N))" /></a>

    Asymptotic Complexity(平均複雜度) = Theta(log n)
    
[Reference of Matrix Exponentiation](https://brilliant.org/wiki/fast-fibonacci-transform/)

### 解法
#### Python3 (matrix exponentiation)
##### tags: `matrix exponentiation`
```python
def fib(self, N):
        
    def _fib(N):
        if N == 0:
            return (0, 1)
        else:
            f_n, f_np1 = _fib(N // 2)       # N // 2 means round(N / 2)
            f_2n = f_n * (2 * f_np1 - f_n)
            f_2np1 = f_n**2 + f_np1**2      # f_n**2 means (f_n)^2

            if N % 2 == 0:
                return (f_2n, f_2np1)
            else:
                return (f_2np1, (f_2n + f_2np1))

    return _fib(N)[0]
```
- 解釋：用遞迴的方式往小的N求解，因為一次是跳兩倍，所以比起普通的O(n)遞迴更快了一些。這一題的N限定在30以下所以很好求，可能不需要用到快速解法才能提高ranking，但對於更大的N就會很有優勢。

#### Go (DP)
```go
func fib(N int) int {
    newF, oldF := 1,0
    for i:=0;i<N;i++{
        newF, oldF = oldF, newF+oldF
    }
    return oldF
}
```

#### JavaScript (1 line solution, recursive)
##### tags: `1 line`, `recursive`
```javascript
const fib = N => N < 2? N: fib(N-1) + fib(N-2)
```
#### JavaScript (DP)
##### tags: `DP`
```javascript
var fib = function(N) {
    let table = [0, 1]
    for(let i = 2; i <= N; ++i)
        table[i] = table[i-1] + table[i-2]
    return table[N]
}
```
#### JavaScript (Another DP)
##### tags: `DP`
```javascript
var fib = function(N) {
    let _new = 1, old = 0
    for(let i = 2; i <= N; ++i)
        [old, _new] = [_new, _new + old]
    return N < 2 ? N: _new
}
```
#### JavaScript (Matrix Exponentiation)
```javascript
var fib = function(N) {
    if(N <= 2) return [0, 1, 1][N]
    if(N % 2)
        return Math.pow(fib(((N + 1) / 2)), 2) + Math.pow(fib((N - 1) / 2), 2)
    else 
        return fib(N / 2) * (2 * fib((N / 2) + 1) - fib(N / 2))
}
```
---
