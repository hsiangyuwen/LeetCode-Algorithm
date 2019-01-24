## [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
> :black_nib: Sammy
### 題目解釋
    Input為 nums1, m, nums2, n。
    nums1, nums2是由小到大排好的數列
    m是nums1裡面有數字的長度
    n是nums2裡面有數字的長度
    nums1開出來的空間一定大於等於 m+n (數字結束後剩下的空間都補0。例如：[1,2,3,0,0,0])
    nums2開出來的空間等於n
    
    寫一個函式把nums2合併進nums1，並且由小到大排好
    
### 審題注意
    1. 理論上這種題目不能再另外多開空間
    2. 直接召喚一般sort函式會排成[0,0,0,1,2,3]

    這題其實在考merge sort的最後一步(即"合併"步驟)。
    複雜度為O(n)。 (一般merge sort分成 log n 層，每層運算為O(n)，複雜度為O(nlogn))

補充：merge_sort source code
```c
#include <stdio.h>
#include <string.h>
#define ARRAY_SIZE 9

int arr[] = {37, 41, 19, 81, 41, 25, 56, 61, 49};
int temp[ARRAY_SIZE];

void mergesort(int a[], int r) {
    int m = r / 2;
    int ref1_len = m + 1;
    int ref2_len = r - m;
    int * ref1 = a;
    int * ref2 = a + (m + 1);

    if (r) {
        mergesort(ref1, ref1_len - 1);
        mergesort(ref2, ref2_len - 1);
    }

    memset(temp, 0, ARRAY_SIZE);
    
    int i = 0, j = 0, k = 0;
    for (; i < ref1_len && j < ref2_len;) {
        if (ref1[i] <= ref2[j])
            temp[k++] = ref1[i++];
        else
            temp[k++] = ref2[j++];
    }
    for (; i < ref1_len;)
        temp[k++] = ref1[i++];
    for (; j < ref2_len;)
        temp[k++] = ref2[j++];
    
    int x;
    for(x=0; x<=r; x++)
        a[x]=temp[x];
}

int main() {
    int x;

    mergesort(arr, ARRAY_SIZE - 1);
    for (x = 0; x < ARRAY_SIZE; x++)
        printf("%d: %d\n", x, arr[x]);
    return 0;
}
```
### 解法
#### Python3 (Merge Sort)
##### tags: `Merge Sort`
```python
def merge(self, nums1, m, nums2, n):
    if n == 0:
        return

    i = m - 1  # right-most of nums1
    j = n - 1  # right-most of nums2

    for index in range(m + n - 1, -1, -1):
        if i < 0:
            nums1[index] = nums2[j]
            j -= 1
        elif j < 0:
            return  # Because we merge nums2 to nums1
        else:
            if nums1[i] >= nums2[j]:
                nums1[index] = nums1[i]
                i -= 1
            else:
                nums1[index] = nums2[j]
                j -= 1
```
- Tips：假想nums1是一個填入的空間，nums1<sub>[0 - m-1]</sub> 和 nums2<sub>[0 - n-1]</sub> 是用來參照的兩個已排好的數列。從右邊開始由大到小填入，就不會有 nums1<sub>[0 - m-1]</sub> 這個參照區被覆蓋掉的狀況。
- 解釋：`for i in range(m + n - 1, -1, -1)`意同`for(i=(m+n-1); i>=0; i--)`。

#### JavaScript
```javascript
var merge = function(nums1, m, nums2, n) {
    for(let idx = m + n - 1, i = m - 1, j = n - 1; idx >= 0; --idx)
        if(nums1[i] >= nums2[j] || j < 0)
            nums1[idx] = nums1[i--]
        else
            nums1[idx] = nums2[j--]
}
```
---
