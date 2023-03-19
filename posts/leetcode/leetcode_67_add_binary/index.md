# LeetCode 67. Add Binary 解題紀錄


## [題目](https://leetcode.com/problems/add-binary/)

{{< admonition type=quote title="Problem">}}

Given two binary strings a and b, return their sum as a binary string.

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

**Constraints:**

-   1 $\leq$ `a.length`, `b.length` $\leq$ $10^4$
-   `a` and `b` consist only of '0' or '1' characters.
    Each string does not contain leading zeros except for the zero itself.

{{< /admonition >}}

## 想法

由 `a` 和 `b` 兩字串的尾端讀取，假如該位置合法 (>=0)，則確認是否有較小位值的進位 (holder)，再依據二進位的進位法則進行計算。

-   [看這邊](https://eyzim.github.io/posts/cpp/cpp_int_to_string/)或是下面的小抄

    -   string to int: `a[i] - '0'`
    -   int to string: `to_string(intA)`

    ps: 這個問題好像被我 google 了 n 遍了 :rofl:

## 解法

```cpp
string addBinary(string a, string b) {

    string ans = "";

    // 由最右邊/最小的位置開始計算
    int i=a.size()-1, j=b.size()-1, holder=0;

    while(i>=0 || j>=0 || holder>0)
    {
        if(i>=0)
        {
            holder += (a[i]-'0');
            i--;
        }

        if(j>=0)
        {
            holder += (b[j]-'0');
            j--;
        }

        // 檢查是否有進位後剩下的數值
        ans = to_string(holder%2) + ans;

        // 已經進位
        holder /= 2;
    }

    return ans;
}
```

### 縮寫

```cpp
string addBinary(string a, string b) {

    string ans = "";

    // 由最右邊/最小的位置開始計算
    int i=a.size()-1, j=b.size()-1, holder=0;

    while(i>=0 || j>=0 || holder>0)
    {
        holder += i>=0 ? (a[i--]-'0') : 0;
        holder += j>=0 ? (b[j--]-'0') : 0;

        // 檢查是否有進位後剩下的數值
        ans = to_string(holder&1) + ans;

        // 已經進位
        holder >>= 1;
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

下面這種縮寫寫法並不會快到多少，面試時用一下炫技可以，平時寫專案我不大敢用，以我的看 code 速度，`n /= 2` 絕對比 `n >>= 1` 容易懂。

個人其實還蠻喜歡這題考的型別轉換加上 edge case 思考。

## Reference

1. https://stackoverflow.com/questions/11310898/how-do-i-get-the-type-of-a-variable

