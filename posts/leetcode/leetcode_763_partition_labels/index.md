# LeetCode 763. Partition Labels 解題紀錄


## [題目](https://leetcode.com/problems/partition-labels/)

{{< admonition type=quote title="Problem">}}

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return _a list of integers representing the size of these parts_.

**Example 1:**

```
Input: s = "ababcbacadefegdehijhklij"
Output: [9,7,8]
```

Explanation:
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

**Example 2:**

```
Input: s = "eccbbbbdec"
Output: [10]
```

**Constraints:**

-   1 $\leq$ `s.length` $\leq$ 500
-   s consists of lowercase English letters.

{{< /admonition >}}

## 想法

以 **Example 1** 為例子，`s[0]` 的字元 `a` 在字串中最後一次出現在 `s[8]`，只要從第一個到第八個字元最後一次出現都是在 `s[8]` 之前，我們就將這一字串分割為一組子字串。

|       pos        |    0    |  1  |  2  |  3  |  4  |  5  |  6  |  7  |    8    |  9  | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  | 21  | 22  | 23  |
| :--------------: | :-----: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-----: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|       字元       | **_a_** |  b  |  a  |  b  |  c  |  b  |  a  |  c  | **_a_** |  d  |  e  |  f  |  e  |  g  |  d  |  e  |  h  |  i  |  j  |  h  |  k  |  l  |  i  |  j  |
| 字元最後出現位置 |    8    |  5  |  8  |  5  |  7  |  5  |  8  |  7  |    8    | 14  | 15  | 11  | 15  | 13  | 14  | 15  | 19  | 22  | 23  | 19  | 20  | 21  | 22  | 23  |

從範例可以知道，在走到 `s[8]` 之前，`s[0]`~`s[7]` 的最後出現位置都*小於* 8，換言之，以 `s[8]` 最為分界點，將 `s[0]`~`s[8]` 分為一個子字串。

另外注意一點是，題目要求返回子字串**長度**。

## 解法

### 解法一

{{< admonition type=note title="string.find()">}}

平常常見的 `string.find()` 是正序搜尋，希望能倒序搜尋則使用 `string.rfind()`。

{{< /admonition >}}

```cpp
vector<int> partitionLabels(string s) {

    vector<int> ans;

    // cut 紀錄最大的子字串最後一次出現位置、pre 紀錄下一個字串的起始位置
    auto cut = INT_MIN, pre = 0;

    for(auto i=0; i<s.size(); i++)
    {
        // 檢查是否有更遠的字串最後一次出現位置，若有，則更新位置
        cut = max((int)s.rfind(s[i]), cut);

        // 如果現在位置就是最遠的字串最後一次出現位置 aka 切割點，則切成子字串
        // 並記錄下一字串的起始位置
        if(i == cut)
        {
            ans.push_back(cut-pre+1);
            pre = cut + 1;
        }
    }

    return ans;
}
```

`rfind()` 本身就是 $\mathcal{O}(n)$，每次都進行運算，使整個演算法的時間複雜度為 $\mathcal{O}(n^2)$.

-   Time complexity: $\mathcal{O}(n^2)$.
-   Space complexity: $\mathcal{O}(1)$.

### 解法二

```cpp
vector<int> partitionLabels(string s) {

    vector<int> ans;
    vector<int> alp(26, 0);

    // cut 紀錄最大的子字串最後一次出現位置、lastPos 紀錄下一個字串的起始位置
    int cut = -1, lastPos = 0;

    // 覆蓋存入每一個 char 的最後出現位置
    for(auto i=0; i<s.size(); i++)
    {
        alp[s[i]-'a'] = i;
    }

    for(auto i=0; i<s.size(); i++)
    {
        // 檢查是否有更遠的字串最後一次出現位置，若有，則更新位置
        cut = max(cut, alp[s[i]-'a']);

        // 如果現在位置就是最遠的字串最後一次出現位置 aka 切割點，則切成子字串
        // 並記錄下一字串的起始位置
        if(cut == i)
        {
            ans.push_back(cut - lastPos + 1);
            lastPos = cut + 1;
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

---

這題是多年前寫過，今天變成 LeetCode 每日選題，回去看以前的 code，發現那個答案是不知道哪裡查到直接抄的 XD

看著自己默默地也成長了一點點有點欣慰，已經買到回台灣的機票了，準備回去找工作了！

## Reference

1. https://www.cplusplus.com/reference/string/string/rfind/
2. https://vimsky.com/zh-tw/examples/usage/stdstringrfind-in-c-with-examples.html

