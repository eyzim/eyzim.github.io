# LeetCode 1502. Can Make Arithmetic Progression From Sequence 解題紀錄


## [題目](https://leetcode.com/problems/can-make-arithmetic-progression-from-sequence/)

{{< admonition type=quote title="Problem">}}

A sequence of numbers is called an **arithmetic progression** if the difference between any two consecutive elements is the same.

Given an array of numbers `arr`, return `true` _if the array can be rearranged to form an_ **_arithmetic progression_**. _Otherwise, return_ `false`.

**Example 1:**

```
Input: arr = [3,5,1]
Output: true
```

Explanation: We can reorder the elements as `[1,3,5]` or `[5,3,1]` with differences 2 and -2 respectively, between each consecutive elements.

**Example 2:**

```
Input: arr = [1,2,4]
Output: false
```

Explanation: There is no way to reorder the elements to obtain an arithmetic progression.

**Constraints:**

-   2 $\leq$ `arr.length` $\leq$ 1000
-   $-10^6 \leq$ `arr[i]` $\leq 10^6$

{{< /admonition >}}

## 想法

整組 vector 依照大小排列，兩兩檢查中間的間隙是否等大。

## 解法

```cpp
bool canMakeArithmeticProgression(vector<int>& arr) {

    sort(arr.begin(), arr.end());

    auto t = arr[1] - arr[0];

    for(auto i=arr.size()-1; i>1; i--)
    {
        if(arr[i] - arr[i-1] != t)
        {
            return 0;
        }
    }

    return 1;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

