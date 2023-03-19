# LeetCode 1051. Height Checker 解題紀錄


## [題目](https://leetcode.com/problems/height-checker/)

{{< admonition type=quote title="Problem">}}
A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in **non-decreasing order** by height. Let this ordering be represented by the integer array `expected` where `expected[i]` is the expected height of the $i^{th}$ student in line.

You are given an integer array `heights` representing the **current order** that the students are standing in. Each `heights[i]` is the height of the $i^{th}$ student in line (**0-indexed**).

Return the **_number of indices_** where `heights[i] != expected[i]`.

**Example 1:**

```
Input: heights = [1,1,4,2,1,3]
Output: 3
```

Explanation:

heights: [1,1,4,2,1,3]

expected: [1,1,1,2,3,4]

Indices 2, 4, and 5 do not match.

**Example 2:**

```
Input: heights = [5,1,2,3,4]
Output: 5
```

Explanation:

heights: [5,1,2,3,4]

expected: [1,2,3,4,5]

All indices do not match.

**Example 3:**

```
Input: heights = [1,2,3,4,5]
Output: 0
```

Explanation:

heights: [1,2,3,4,5]

expected: [1,2,3,4,5]

All indices match.

**Constraints:**

-   1 $\leq$ heights.length $\leq$ 100
-   1 $\leq$ heights[i] $\leq$ 100

{{< /admonition >}}

## 想法

1. 將現在 `heights` array 複製一份到 `sorted` array
2. 對 `sorted` array 進行從小到大的排序
3. 再一一比對每個元素是否相同

## 解法

```CPP
int heightChecker(vector<int>& heights) {

    // 複製一份到 sorted
    vector<int> sorted = heights;

    // 對 sorted 進行排序
    sort(sorted.begin(), sorted.end());

    // 統計同位置元素值不同的次數
    auto ans = 0;

    // 一一對照 sorted 和 heights 同位置之值
    for(auto i=0; i<sorted.size(); i++)
    {
        if(sorted[i]!=heights[i])
        {
            ans++;
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

