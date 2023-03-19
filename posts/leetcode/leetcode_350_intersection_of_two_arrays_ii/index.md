# LeetCode 350. Intersection of Two Arrays II 解題紀錄


## [題目](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

{{< admonition type=quote title="Problem">}}

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

**Example 1:**

```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]
```

**Example 2:**

```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]
```

Explanation: [9,4] is also accepted.

**Constraints:**

-   1 $\leq$ `nums1.length`, `nums2.length` $\leq$ 1000
-   0 $\leq$ `nums1[i]`, `nums2[i]` $\leq$ 1000

**Follow up:**

-   What if the given array is already sorted? How would you optimize your algorithm?
-   What if `nums1`'s size is small compared to `nums2`'s size? Which algorithm is better?
-   What if elements of `nums2` are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

{{< /admonition >}}

## 想法

1. 確認兩個 array 中是否有 size 為 0 的情況，若有，則直接 return 空。
2. 遍歷 array 中所有個數，紀錄到 `unordered_map` 中
3. 另一個 array 則檢查是否對應的 key 值不為 0，表示前一個 array 中也有這個元素，故印出它。

## 解法

```cpp
vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {

    // 檢查是否有 array 為空，若為空，則不可能有交集
    if(!nums1.size() || !nums2.size())  return {};

    vector<int> ans;

    // 紀錄每個數字出現的次數
    unordered_map<int, int> mp;

    for(auto i:nums1)
    {
        mp[i]++;
    }

    for(auto i:nums2)
    {
        // 若 mp[i] != 0 表示有交集
        if(mp[i])
        {
            ans.push_back(i);
            mp[i]--;
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

