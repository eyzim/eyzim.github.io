# LeetCode 414. Third Maximum Number 解題紀錄


## [題目](https://leetcode.com/problems/third-maximum-number/)

{{< admonition type=quote title="Problem">}}
Given an integer array `nums`, return the **third distinct maximum** number in this array. If the third maximum does not exist, return the **maximum** number.

**Example 1:**

```
Input: nums = [3,2,1]
Output: 1
```

Explanation:

The first distinct maximum is 3.
The second distinct maximum is 2.
The third distinct maximum is 1.

**Example 2:**

```
Input: nums = [1,2]
Output: 2
```

Explanation:

The first distinct maximum is 2.
The second distinct maximum is 1.
The third distinct maximum does not exist, so the maximum (2) is returned instead.

**Example 3:**

```
Input: nums = [2,2,3,1]
Output: 1
```

Explanation:
The first distinct maximum is 3.
The second distinct maximum is 2 (both 2's are counted together since they have the same value).
The third distinct maximum is 1.

**Constraints:**

-   1 $\leq$ nums.length $\leq 10^4$
-   $-2^{31} \leq$ nums[i] $\leq 2^{31} - 1$

**Follow up:** Can you find an $O(n)$ solution?
{{< /admonition >}}

## 想法

將 `nums` 中所有元素進行排序，排序後剔除重複的元素。

若 nums.size$\geq$3 則取得倒數第三大的元素，反之則取最後一個元素(最大的)。

## 解法

### 解法一

用 vector 實作

```cpp
int thirdMax(vector<int>& nums) {

    // 排序所有元素
    sort(nums.begin(), nums.end());

    // ans 放置 nums 中不重複值的元素
    vector<int> ans;
    int former = INT_MAX;

    for(auto i:nums)
    {
        if(i != former)
        {
            ans.push_back(i);
        }
        former = i;
    }

    return ans.size()<3 ? ans[ans.size()-1] : ans[ans.size()-3];
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法二

既然要排序又希望 vector 中的元素唯一，那我們應該可以用 set 來實作吧?

```cpp
int thirdMax(vector<int>& nums) {

    set<int> top3;

    for(auto i:nums)
    {
        top3.insert(i);

        // 大於三個元素，捨棄最小的元素
        if(top3.size()>3)
        {
            top3.erase(top3.begin());
        }
    }

    // top3.begin(): 倒數第三大
    // top3.rbegin(): 倒數第一大 / 最大
    return top3.size()==3 ? *top3.begin() : *top3.rbegin();
}
```

-   Time complexity: $\mathcal{O}(nlog(3))$.
-   Space complexity: $\mathcal{O}(n)$.

## Reference

1. http://www.cplusplus.com/reference/set/set/

