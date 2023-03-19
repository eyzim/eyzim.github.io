# LeetCode 1790. Check if One String Swap Can Make Strings Equal 解題紀錄


## [題目](https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal/)

{{< admonition type=quote title="Problem">}}

You are given two strings `s1` and `s2` of equal length. A **string swap** is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return `true` \*if it is possible to make both strings equal by performing **_at most one string swap_** on **_exactly one_** of the strings\*. Otherwise, return `false`.

**Example 1:**

```
Input: s1 = "bank", s2 = "kanb"
Output: true
```

Explanation: For example, swap the first character with the last character of s2 to make "bank".

**Example 2:**

```
Input: s1 = "attack", s2 = "defend"
Output: false
```

Explanation: It is impossible to make them equal with one string swap.

**Example 3:**

```
Input: s1 = "kelb", s2 = "kelb"
Output: true
```

Explanation: The two strings are already equal, so no string swap operation is required.

**Constraints:**

-   1 $\leq$ `s1.length`, `s2.length` $\leq$ 100
-   `s1.length` == `s2.length`
-   `s1` and `s2` consist of only lowercase English letters.

{{< /admonition >}}

## 想法

題目希望兩個 string 相同，給出的條件是：最多做一次字元交換。

如果兩個 string 一開始就相同，那恭喜，直接就保送為 `true`。

若兩個 string 有一個位置不同，無法單靠 swap 字元得到相同的 string。

若有 `s1` 和 `s2` 中兩個位置不同呢？很好，檢查一下做一次交換字元後兩個 string 是否相同即可！

## 解法

```cpp
bool areAlmostEqual(string s1, string s2) {

    // 兩個 string 長度不一致
    if(s1.size() != s2.size())  return 0;

    // 紀錄兩個 string 不同之處
    vector<int> mp;

    for(auto i=0; i<s1.size(); i++)
    {
        if(s1[i] != s2[i])
        {
            mp.push_back(i);
        }
    }

    // 兩個 string 完全相同
    if(!mp.size())  return 1;

    // 兩個 string 共有兩處不同
    if(mp.size() == 2)
    {
        // 確定互換後可以達到 s1 == s2
        if( s1[mp[0]] == s2[mp[1]] && s1[mp[1]] == s2[mp[0]])
        {
            return 1;
        }
    }

    return 0;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

