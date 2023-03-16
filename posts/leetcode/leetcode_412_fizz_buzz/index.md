# LeetCode 412. Fizz Buzz 解題紀錄



## [題目](https://leetcode.com/problems/fizz-buzz/)


{{< admonition type=quote title="Problem">}}

Given an integer $n$, return a string array `answer` *(1-indexed)* where:

- `answer[i] == "FizzBuzz"` if $i$ is divisible by 3 and 5.
- `answer[i] == "Fizz"` if i is divisible by 3.
- `answer[i] == "Buzz"` if i is divisible by 5.
- `answer[i] == i` (as a string) if none of the above conditions are true.
 

**Example 1:**
```
Input: n = 3
Output: ["1","2","Fizz"]
```
**Example 2:**
```
Input: n = 5
Output: ["1","2","Fizz","4","Buzz"]
```
**Example 3:**
```
Input: n = 15
Output: ["1","2","Fizz","4","Buzz","Fizz","7","8","Fizz","Buzz","11","Fizz","13","14","FizzBuzz"]
```

**Constraints:**

1 $\leq$ n $\leq 10^4$

{{< /admonition >}}


## 想法

依序插入每一元素的值，如果遇到 3、5、15 的倍數，換成插入 "Fizz"、"Buzz"、"FizzBuzz"

{{< admonition type=note title="to_string()">}}
想要將數字強轉成 `string` 時，使用 `std::to_string()`，卻一直出現 **'to_string': 找不到識別項**，在前方記得要先加上 include

```cpp
#include <string>
```
{{< /admonition >}}



## 解法

### 解法一

```cpp
vector<string> fizzBuzz(int n) {

    vector<string> ans(n);

    for(auto i=1; i<=n; i++)
    {
        // 15 的倍數為 "FizzBuzz"
        if(i%15 == 0)
        {
            ans.push_back("FizzBuzz");
        }
        // 5 的倍數為 "Buzz"
        else if(i%5 == 0)
        {
            ans.push_back("Buzz");
        }
        // 3 的倍數為 "Fizz"
        else if(i%5 == 0)
        {
            ans.push_back("Fizz");
        }
        // 依序填入值
        else
        {
            ans.push_back(to_string(i));
        }
    }

    return ans;
}
```
- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.

### 解法二

```cpp
vector<string> fizzBuzz(int n) {

    vector<string> ans(n);

    // 依序填入值
    for(auto i=1; i<=n; i++)
    {
        ans[i-1] = to_string(i);
    }

    // 3 的倍數為 "Fizz"
    for(auto i=2; i<n; i+=3)
    {
        ans[i] = "Fizz";
    }

    // 5 的倍數為 "Buzz"
    for(auto i=4; i<n; i+=5)
    {
        ans[i] = "Buzz";
    }

    // 15 的倍數為 "FizzBuzz"
    for(auto i=14; i<n; i+=15)
    {
        ans[i] = "FizzBuzz";
    }

    return ans;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.


{{< admonition type=question title="兩種做法的時間、空間複雜度都是 $O(n)$，哪個比較好呢？">}}

做 **除法/mod** 時，對 CPU 相對來說是相當耗時的。

反而是多次地在 memory 做寫入還比較好。

> *Doing __extra writes__ is better than __expensive arithmetic__.*

{{< /admonition >}}

## Reference
1. https://www.cplusplus.com/reference/string/to_string/
