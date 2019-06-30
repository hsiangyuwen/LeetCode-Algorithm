## [66. Plus One](https://leetcode.com/problems/plus-one/)
> :black_nib: Cropse
### 題目解釋
    用列表表示數字，加上1
### 審題注意
    只要注意進位問題即可
### 解法
1. 加總數字進位O(n)  
2. 轉換數字再轉回列表(這方法很爛)  

#### Go
```go
func plusOne(digits []int) []int {
    for i:= len(digits)-1; i>=0; i--{
        if digits[i]==9{
            digits[i] = 0
        }else{
            digits[i]++
            return digits
        }
    }
        return append([]int{1}, digits...)
}
```
#### Python3
```python
def plusOne(self, digits: 'List[int]') -> 'List[int]':
    for i in range(len(digits) - 1, -1, -1):  # len-1, len-2, ... , 0
        digits[i] += 1
        if digits[i] == 10:
            digits[i] = 0
        else:
            return digits
    return [1] + digits
```
#### JavaScript (直觀解)
```javascript
var plusOne = function(digits) {
    for(let i = digits.length - 1; i >= 0; --i)
        if(digits[i] === 9)
            digits[i] = 0
        else {
            digits[i]++
            return digits
        }

    return [1, ...digits]
}
```
#### JavaScript (精簡)
```javascript
var plusOne = function(digits) {
    for(let i = digits.length - 1; i >= 0; --i)
        if(++digits[i] > 9) digits[i] = 0
        else return digits

    return [1, ...digits]
}
```

#### C++
```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i = digits.size()-1; i >= 0; i--)
        {
            /*
                Since only plus one, the carry always be 1 
                and just need to check whether digit is 9 or not
            */
            if(digits[i] != 9) // no carry
            {
                digits[i] += 1;
                return digits;
            }
            else // need carry
            {
                digits[i] = (digits[i] + 1) - 10;
            } 
        }
        // if there is a need of increasing one more digit, since there always has carry for each digit ( e.g. 99, 999 )  
        digits.insert(digits.begin(), 1);
        return digits;
    }
};
```
---
