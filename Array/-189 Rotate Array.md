## [189. Rotate Array](https://leetcode.com/problems/rotate-array/)
> :black_nib: Cropse
### 題目解釋
給一定值K,把list向右平移K次
### 審題注意
題目是inplace，沒有給你回傳值的意思。
另外提到至少有三種做法，所以都做一做吧。
### 解法
1.Brute Force
2.S(N) Extra Array
3.Cyclic Replacements
#### Golang
##### tags: `Brute Force`
```go
func rotate(nums []int, k int)  {
    for i:=0; i<k; i++{
        for j:=len(nums)-1; j>0;j--{
            nums[j], nums[j-1] = nums[j-1], nums[j]
        }
    }
}
```
- 解釋
  按照題目向右平移，類似氣泡的做法。
- 優/缺點
  O(n*k)
  很爛，會爆掉。 
#### Golang
##### tags: `Extra Array`
```go
func rotate(nums []int, k int)  {
    temp := make([]int, len(nums))
    k = k%len(nums)
    for i:=0;i<len(nums);i++{
        temp[i] = nums[(len(nums)-k+i)%len(nums)]
    }
    for i:=0;i<len(nums);i++{
        nums[i] = temp[i]
    }
}
```
- 解釋
  Extra array先擺好，後面再存進去
- 優/缺點
  O(n) S(n)

#### Golang
##### tags: `Cyclic Replacements`
```go
func rotate(nums []int, k int) {
	l := len(nums)
	k = k % l
	count := 0
	for i := 0; count < l; i++ {
		j := i
		cur := nums[i]
		for {
			j = (j + k) % l
			nums[j], cur = cur, nums[j]
			count++
			if j == i {
				break
			}
		}
	}
}
```
- 解釋
  還沒寫
- 優/缺點
  O(n) S(1)
