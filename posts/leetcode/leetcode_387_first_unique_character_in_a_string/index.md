# LeetCode 387. First Unique Character in a String 解題紀錄



## [題目](https://leetcode.com/problems/first-unique-character-in-a-string/)


{{< admonition type=quote title="Problem">}}

Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.

 

Example 1:

```
Input: s = "leetcode"
Output: 0
```

Example 2:

```
Input: s = "loveleetcode"
Output: 2
```

Example 3:

```
Input: s = "aabb"
Output: -1
```

Constraints:

- 1 $\leq$ s.length $\leq$ 105
- s consists of only lowercase English letters.

{{< /admonition >}}


## 解法

沿著字串一一計算每個字母出現的次數，並記錄在 `mp` 中，再依照 `s` 中出現的時間遍歷 `mp` 檢查是否有只出現過一次的字母。

### 解法一

```cpp
int firstUniqChar(string s)
{
    unordered_map<char, int> mp;
    
    // 計算每個 char 出現的次數
    for(auto i:s)
    {
        mp[i]++;
    }
    
    // 每一個 char 遍歷尋找只出現一次的 char
    for(auto i=0; i<s.size(); i++)
    {
        if(mp[s[i]]==1)
            return i;
    }
    
    return -1;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.

### 解法二

一樣沿著 `s` 檢查每個字母是否有出現過，若有出現過的話，在 `mp[i]` 的位置將次數記錄直接改成 `INT_MAX`；反之則紀錄這個字母出現過的位置。

再搜尋 `mp` 中最小的值，即為第一個遇到的 unique number。

```cpp

int firstUniqChar(string s) {
    
    unordered_map<char, int> mp;
    int ans = INT_MAX;
    
    for(auto i=0; i<s.size(); i++)
    {
        // 沒看過這個字母，紀錄出現的位置 (index)
        if(mp.find(s[i]) == mp.end())
        {
            mp[s[i]] = i;
        }
        // 有看過這個字母了，出現位置改成 INT_MAX
        else
        {
            mp[s[i]] = INT_MAX;
        }
    }
    
    // 尋找最小的 index 值
    for(auto i:mp)
    {
        ans = min(i.second, ans);
    }
    
    // 若最小 index 為 INT_MAX，表示每個 char 都重複出現過
    return (ans==INT_MAX) ? -1:ans;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.

