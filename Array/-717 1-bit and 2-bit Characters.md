## [717. 1-bit and 2-bit Characters](https://leetcode.com/problems/1-bit-and-2-bit-characters/)
> :black_nib: Ray
### 題目解釋
    第一種字元是一位元的 0，第二種是兩位元的 11 或 10，判斷一字串最末字元為一位元的 0
### 審題注意
    1. 冷靜找規律
    2. 有多種解法：直觀解、遞迴及正規表示式判斷
    3. 有沒有可能不用遍歷整個字串就做出判斷？
### 解法
#### JavaScript (1 line, regex)
##### tags: `1 line`, `regex`
```javascript
var isOneBitCharacter = bits => bits
    .join('')
    .replace(/(11|10)/g, 'z')
    .slice(-1) != 'z'
```
- Tips
    1. regex
    2. Array(slice)
- 解釋
    1. 先看例子：
    ```
    Example 1. 010 -> 0z -> false
    Example 2. 110 -> z0 -> true
    Example 3. 1110 -> zz -> false
    Example 4. 10100010 -> zz00z -> false
    ```
    2. 先把 Array 轉成字串，再把字串中 11 或 10 換成 z，最後判斷最後一個字元是否為 z 或 0。
    3. **注意**：`Array.prototype.slice`，如果使用**負數**索引值，表示由陣列**最後**開始提取。
#### JavaScript (直觀解)
```javascript
var isOneBitCharacter = function(bits) {
    let flag = 0
    for(let i = 0; i < bits.length; i++)
        if(bits[i] === 1) {
            flag = 0
            i++
        } else {
            flag = 1
        }
    return flag
}
```
- 解釋：可以得出一個規律：如果判斷某 bit 為 1，就可以把它和後一位元一起判斷為一個兩位元的字元，所以最後判斷 `flag` 是否為 0 就知道是否為一位元的字元。
> 是否可以不用遍歷整個字串?
#### JavaScript (優化)
```javascript
var isOneBitCharacter = function(bits) {
    let oneCount = 0
    for(let i = bits.length - 2; i >= 0 && bits[i] === 1; --i)
        ++oneCount
    return !(oneCount % 2)
}
```
- 解釋：計算從倒數第二位開始 1 的個數直到遇到 0 為止，如果 1 是奇數個，那最末字元必是兩位元的 10；反之，1 是偶數個，那末字元是一位元的 0，看以下的例子：
    ```
    Example 1. 1101110 -> 奇數個 1 ，所以末字元是 10 -> 回傳 false
                 |321
    Example 2. 11110 -> 偶數個 1，末字元是 0 -> 回傳 true
              |4321
    ```
#### Python3 (regex)
##### tag: 'regex'
```python
def isOneBitCharacter(self, bits):

    # To perform a substitution using a regular expression, use re.sub()
    import re
    s = re.sub(r'(11|10)', 'z', ''.join([str(bit) for bit in bits]))

    # s[-1] == 'z' -> last character can be 2-bit character
    # s[-1] == '0' -> last character must be 1-bit character
    return s[-1] == '0'
```
- 解釋：Python的regular expression要另外import

> 是否可以不用遍歷整個字串?
#### Python3 (優化)
```python
def isOneBitCharacter(self, bits):
    one_count = 0
    index = len(bits) - 2
    while(index >= 0):
        if bits[index] == 0:
            break
        one_count += 1
        index -= 1

    return one_count % 2 == 0
```

#### Go (regex)
##### tag: 'regex'
```go
import (
	"fmt"
	"regexp"
	"strings"
)

func isOneBitCharacter(bits []int) bool {
	bitsStrings := strings.Trim(strings.Join(strings.Split(fmt.Sprint(bits), " "), ""), "[]")
	re := regexp.MustCompile("(11|10)")
	bitsStrings = re.ReplaceAllString(bitsStrings, "z")
	return bitsStrings[len(bitsStrings)-1] == '0'
}
```

> 是否可以不用遍歷整個字串?
#### Go (優化)
```go
func isOneBitCharacter(bits []int) bool {
	count := 0
	for i := len(bits) - 2; i >= 0; i-- {
		if bits[i] == 1 {
			count++
		} else {
			break
		}
	}
	return count%2 == 1

```
---
