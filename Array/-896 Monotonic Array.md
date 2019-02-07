
## [896. Monotonic Array](https://leetcode.com/problems/monotonic-array/)
> :black_nib: Sammy
### 題目解釋
	給定一個數列A，求這個數列是否為遞增/遞減數列。
### 審題注意
	用直觀的解法求解即可。
	我們可以設定兩個flag： `is_increase` 和 `is_decrease`。
	但是要釐清如何使用這兩個flag，才能夠表示這個數列是否為遞增/遞減數列。

	*（以下跟官方solution有點不同）
	先將 `is_increase` 和 `is_decrease` 都設為False，
	遍歷一次A，同一個迴圈內做以下兩步：
	1. 如果 A[i] < A[i+1] ，則 `is_increase` 設為True
	2. 如果 A[i] > A[i+1] ，則 `is_decrease` 設為True
	（我們可以發現，遇到相等的情況時不做任何事情）
	最後回傳 !(is_increase && is_decrease)。

	檢查以下四種需要被考慮的情況：
	* [1,2,2,3] => !(true && false) = true
	* [6,5,5,4] => !(false && true) = true
	* [1,1,1,1] => !(false && false) = true
	* [1,3,2,4] => !(true && true) = false

	也就是說，有遇到增又有遇到減的狀況時，會不符合遞增/遞減數列。
### 解法
> 官方Solution Approach 3: One Pass (Simple Variant)

		和「審題說明」敘述的作法相同。

#### Python3 (One Pass (Simple Variant))
##### tags: `flag`
```python
def isMonotonic(self, A: 'List[int]') -> 'bool':
    is_inc = False
    is_dec = False
    
    for i in range(len(A)-1):
        if A[i] < A[i+1]:
            is_inc = True
        if A[i] > A[i+1]:
            is_dec = True
    
    return not (is_inc and is_dec)

```
- 缺點：兩個if branch，在len(A)較長的時候會略減慢速度。

> 官方Solution Approach 2: One Pass

		利用了compare function的特性（比大小後以1, 0, -1代表結果），將所有的比較結果存進一個set。
		遞增數列會使得這個set是{0, -1}的子集；
		遞減數列會使得這個set是{0, 1}的子集。

#### Python3 (One Pass)
##### tags: `set`, `cmp`
```python
def isMonotonic(self, A: 'List[int]') -> 'bool':
    store = {(i>j)-(i<j) for i, j in zip(A, A[1:])}
    return store.issubset({0, 1}) or store.issubset({0, -1})
```
- Tips：`zip([1, 2, 3], [2, 3]) => [(1, 2), (2, 3)]` （zip function是回傳一個迭代器，如果要印出list要用list function包住。）
- 解釋：Python3把compare function拿掉了，而Python世界中，True == 1、False == 0，因此我們可以用 `(i>j)-(i<j)` 表示compare function的運算。
(Ref: https://stackoverflow.com/questions/22490366/how-to-use-cmp-in-python-3)
---

