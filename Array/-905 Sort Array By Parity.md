## [905 Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/)
> :black_nib: Ray
### 題目解釋
    偶數排前面，奇數排後面
### 審題注意
    1. 須知 Built-in 函數及不能使用該函數的作法
    2. 奇數和偶數的判斷
    3. 有沒有不使用額外記憶體的方法
    4. Swap 的概念
### 解法
#### JavaScript (1 line solution, built-in function, 80 ms)
```javascript
const sortArrayByParity = A => A.sort((a, b) => (a % 2) - (b % 2))
```
##### tags: `1 line`, `sort` 
- Tips
    1. ES6 (Arrow Function)
    2. Array (sort)
    3. `%2` 判斷偶數也可以用 `&1`
- 解釋：Array.prototype.sort() ([文檔](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Array/sort))：
  1. `compareFunction(a, b)` 回傳小於 0，則 `a` 排在 `b` **前面**。以這題為例：如果 `a` 是偶數，`b` 是奇數，則 `a % 2` 是 0，`b % 2` 是 1，`compareFunction` 將回傳 -1，因此 a (偶數) 會排在 b (奇數) 前面。
  2. `compareFunction(a, b)` 回傳等於 0，不會改變彼此的排序。
  3. `compareFunction(a, b)` 回傳大於 0，則 `a` 排在 `b` **後面**。
  4. 我的記法：要實現小到大的排序 `arr.sort((a, b) => a - b)`，這個是 sort 中最簡單的範例，可以把這個背起來，之後再想例子，例如：輸入`[3, 1, 2, 4]`，輸出`[1, 2, 3, 4]`，所以 `1 - 2` 小於 0，因此 1 排在 2 的前面，反之 `2 - 1` 大於 0，所以 2 排在 1 的後面，這樣記比較不容易忘記。
#### JavaScript (readable solution, swap, 76 ms)
```javascript
const sortArrayByParity = A => {
    for(let i = 0, j = 0; j < A.length; ++j)
        if(A[j] % 2 === 0) {
            [A[i], A[j]] = [A[j], A[i]]
            i++
        }
    return A
}
```
##### tags: `swap`
- 解釋
    1. `i` 是紀錄偶數可以交換的 flag，`j` 則是跑過整個 array 的 flag，有點抽象，直接看例子吧。
    2. 例子：
    ```
    Example 1
          3 | 1 | 2 | 4      3 | 1 | 2 | 4      3 | 1 | 2 | 4
       i  v              =>  v              =>  v              =(swap)=>
       j  v                      v                      v
      -------------------------------------------------------------------   
          2 | 1 | 3 | 4      2 | 1 | 3 | 4            2 | 4 | 3 | 1
       i      v          =>      v          =(swap)=>         v     (end)
       j          v                      v                        v

    Example 2
          2 | 1 | 3 | 4             2 | 1 | 3 | 4      2 | 1 | 3 | 4
       i  v              =(swap)=>      v          =>      v         =>
       j  v                             v                      v
       -------------------------------------------------------------------
          2 | 1 | 3 | 4             2 | 4 | 3 | 1
       i      v          =(swap)=>      v          (end)
       j              v                         v
    ```
- Swap
    * 簡單做法
    ```javascript
        let tmp = a
        a = b
        b = tmp
    ```
    * ES6
    ```javascript
        [a, b] = [b, a]
    ```
    * Another ES6 (advanced)
    ```javascript
        b = [a, a = b][0]
    ```
- 優點
    a. 時間是 O(n)，快
    b. **不需要**額外的存儲空間

#### Python3 (sort)
##### tags: `sort`
```python
def sortArrayByParity(self, A: List[int]) -> List[int]:
    A.sort(key = lambda x: x % 2)
    return A
```
- 解釋：參數key代表我們根據什麼樣的值對list進行排序。比方說list中每個元素都是長度為2的tuple，則我們可用 `key = lambda x: x[1]` 去根據tuple中第二個元素對list做排序。 `key = lambda x: x % 2` 則代表把每個元素根據奇偶性化成0與1再做由小到大的排序（偶數便會在前、奇數便會在後）。
- 優/缺點：時間複雜度O(nlogn)、空間複雜度O(n)（因為Python sort的內部實作）。

#### Python3 (list comprehension, 1 line)
##### tags: `list comprehension`, `1 line`
```python
def sortArrayByParity(self, A: List[int]) -> List[int]:
    return ([x for x in A if x % 2 == 0] + [x for x in A if x % 2 == 1])
```
- 優/缺點：只有Python使用這方式，程式碼會寫得清楚明瞭。時間複雜度O(n)、空間複雜度O(n)

#### Python3 (swap)
##### tags: `swap`
```python
def sortArrayByParity(self, A: List[int]) -> List[int]:
    i = 0
    for j in range(len(A)):
        if A[j] % 2 == 0:
            A[i], A[j] = A[j], A[i]
            i += 1
    
    return A
```
- 解釋：改寫自quick sort的子問題處理方式，利用兩個指標去不斷看需不需要交換，並持續更新指標。
---

