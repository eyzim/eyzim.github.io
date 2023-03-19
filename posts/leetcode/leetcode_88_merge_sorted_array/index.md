# LeetCode 88. Merge Sorted Array 解題紀錄


## [題目](https://leetcode.com/problems/merge-sorted-array/)

{{< admonition type=quote title="Problem">}}

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
```

Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example 2:**

```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
```

Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].

**Example 3:**

```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
```

Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

Constraints:

-   `nums1.length` == m + n
-   `nums2.length` == n
-   0 $\leq$ m, n $\leq$ 200
-   1 $\leq$ m + n $\leq$ 200
-   $-10^9 \leq$ `nums1[i]`, `nums2[j]` $\leq 10^9$

**Follow up:** Can you come up with an algorithm that runs in $\mathcal{O}(m+n)$ time?

{{< /admonition >}}

## 想法

要把所有的值依照 sorted 塞回 `nums1` 裡，只要比較兩個 array 中最大值就可以知道應該要把誰放進去了。

如果 `nums1` 全部的值都比 `nums2` 大，則需要最後一個 while 將所有值放進 `nums1`。

## 解法

```cpp
void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {

    auto i = m-1, j = n-1, pos = m+n-1;

    while(i>=0 && j>=0)
    {
        if(nums1[i] < nums2[j])
        {
            nums1[pos--] = nums2[j--];
        }
        else
        {
            nums1[pos--] = nums1[i--];
        }
    }
    while(j>=0)
    {
        nums1[pos--] = nums2[j--];
    }
}
```

Time complexity: $\mathcal{O}(m+n)$.

Space complexity: $\mathcal{O}(1)$.

