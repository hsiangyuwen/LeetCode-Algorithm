## [922. Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)
> :black_nib: Sammy
### 題目解釋
    給訂一個數列A，並保證裡面有一半是奇數、一半是偶數（A的長度也保證是正偶數）。
    求任意一個將A排列成index是偶數時值為偶數、index是奇數時值為奇數的結果。
### 審題注意
    直觀上可以想到如果有一個奇數值在偶數index上，必會有一個偶數值在奇數index上。
    所以也可以馬上想到利用two-pointer swap的方式解題（找到兩個該swap的地方，然後做swap，再繼續遍歷）。
    當然，比我們在quick sort、merge sort所使用的swap簡單許多，因為swap的觸發條件很直覺。
### 解法
#### Python3
```python
def sortArrayByParityII(self, A: List[int]) -> List[int]:
    j = 1
    for i in range(0, len(A), 2):  # [0, 2, 4, ..., len(A)-3, len(A)-1]
        if A[i] % 2:  # violate
            while A[j] % 2:  # valid
                j += 2
            A[i], A[j] = A[j], A[i]
    
    return A
```
- 解釋：i這個index專注於偶數，找偶數index中違反規則的；j這個index專注於奇數，找奇數index中違反規則的。都違反時就swap，然後繼續遍歷下去。

#### C++
```c++
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
        int i = 0, j = 1;
        while(i < A.size() && j < A.size()){
            while(i < A.size() && A[i] % 2 == 0) i += 2;
            while(j < A.size() && A[j] % 2 == 1) j += 2;
            if(i < A.size()) swap(A[i], A[j]);
        }
        return A;
    }
};
```

#### JavaScript
```javascript
var sortArrayByParityII = function(A) {
    for(let i = 0, j = 1; i < A.length && j < A.length; i += 2, j += 2){
        while(A[i] % 2 === 0 && i < A.length) i += 2
        while(A[j] % 2 === 1 && j < A.length) j += 2
        if(i < A.length) [A[i], A[j]] = [A[j], A[i]]
    }
    return A
}
```

#### Goalang
```golang
func sortArrayByParityII(A []int) []int {
    j := 1
    for i:=0; i<len(A); i+=2{
        if A[i] % 2 != 0{
            for j<len(A){
                if A[j] % 2 == 0{
                    A[i], A[j] = A[j], A[i]
                    j += 2
                    break
                }
                j += 2
            }
        }
    }
    return A
}
```
---
