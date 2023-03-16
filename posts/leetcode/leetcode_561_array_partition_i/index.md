# LeetCode 561. Array Partition I 解題紀錄


## [題目](https://leetcode.com/problems/array-partition-i/)


{{< admonition type=quote title="Problem">}}

Given an integer array nums of 2$n$ integers, group these integers into $n$ pairs $(a_1, b_1), (a_2, b_2), ..., (a_n, b_n)$ such that the sum of $min(a_i, b_i)$ for all $i$ is maximized. Return the maximized sum.

 

**Example 1:**
```
Input: nums = [1,4,3,2]
Output: 4
```
Explanation: All possible pairings (ignoring the ordering of elements) are:
1. $(1, 4), (2, 3)$ -> $min(1, 4) + min(2, 3)$ = 1 + 2 = 3
2. $(1, 3), (2, 4)$ -> $min(1, 3) + min(2, 4)$ = 1 + 2 = 3
3. $(1, 2), (3, 4)$ -> $min(1, 2) + min(3, 4)$ = 1 + 3 = 4

So the maximum possible sum is 4.

**Example 2:**
```
Input: nums = [6,2,6,5,1,2]
Output: 9
```
Explanation: 

The optimal pairing is $(2, 1), (2, 5), (6, 6)$. 

$min(2, 1) + min(2, 5) + min(6, 6)$ = 1 + 2 + 6 = 9.
 

**Constraints:**

- 1 $\leq$ n $\leq$ 104

- `nums.length` = 2 $\times$ n

- $-10^4 \leq$ `nums[i]` $\leq 10^4$

{{< /admonition >}}


## 想法

希望從 input array 中得到兩兩 $min$ 並且想加為最大值的情況：
```
[1, 4, 3, 2]
```

應該在排序後的 array 中，第一項和第二項做 $min$，再依序相加：
```
[1, 2, 3, 4]
```
已知現在 array 為由小至大的排序，

又發現第一項和第二項做 $min$ 前，已經可以預知第一項必為 $min$ 值。

故以此推論，只要把排序過後的第一項、第三項等相加，即可獲得所求。

```
[1, 2, 3, 4]
 ^     ^
```


## 解法
```cpp
int arrayPairSum(vector<int>& nums) {

    // 將 input array 由小排到大
    sort(nums.begin(), nums.end());

    // 預設 max 為 0
    // ps: 當 nums.size()==0 時，return 也是 0
    auto ans = 0;

    // 只要將每組數字的前項相加即可獲得 max
    for(auto i=0; i<nums.size(); i+=2)
    {
        ans += nums[i];
    }

    return ans;
}

```

{{< admonition type=info title="Time complexity">}}
C++ sort 函數的 complexity 為 $O(nlog(n))$
{{< /admonition >}}

- Time complexity:  $\mathcal{O}(nlog(n))$.
- Space complexity:  $\mathcal{O}(n)$.



## Reference
1. https://en.wikipedia.org/wiki/Sort_(C%2B%2B)
