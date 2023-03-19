# LeetCode 342. Power of Four 解題紀錄


在看這題題目之前，可以先試試看相似題 [Power of three](https://eyzim.github.io/posts/leetcode/leetcode_326_power_of_three/) 哦！

## [題目](https://leetcode.com/problems/power-of-four/)

{{< admonition type=quote title="Problem">}}

Given an integer `n`, return *`true` if it is a power of four. Otherwise, return `false`*.

An integer `n` is a power of four, if there exists an integer `x` such that `n == $4^x$`.

**Example 1:**

```
Input: n = 16
Output: true
```

**Example 2:**

```
Input: n = 5
Output: false
```

**Example 3:**

```
Input: n = 1
Output: true
```

**Constraints:**

-   $-2^{31} \leq x \leq 2^{31}-1$

**Follow up:**

Could you solve it without loops/recursion?

{{< /admonition >}}

### 解法一：

1. 使用[以前](https://eyzim.github.io/posts/leetcode/leetcode_326_power_of_three/)學過的

    $log{_b}{a}$

    $ \\ = log*{10}{a} \div log*{10}{b}$

    $ \\ = \frac{log{a}}{log{b}}$，

    舉一反三，

    $log_{4}{n}$

    $ \\ = log*{10}{n} \div log*{10}{4}$

    $ \\ = log{n} \div log4 $

    $ \\ = \frac{log{n}}{log{4}}$

2. 檢查做完 $log$ 之後是否為整數，若是整數，則 `n` 為 4 的次方；反之，則否。

```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {

        if(n < 1)   return 0;

        double check = log(n)/log(4);

				// 驗證是否為整數
        if(check - (int)check == 0)
            return 1;

        return 0;
    }
};
```

Time complexity: $\mathcal{O}(\log N)$.

Space complexity: $\mathcal{O}(1)$.

### 解法二：

遞迴檢查是否為 4 的次方及是否可被 4 整除

```cpp
bool isPowerOfFour(int n) {

        if(n < 1)
        {
            return 0;
        }

        if(n == 1)
        {
            return 1;
        }

        // 若可被 4 除盡 && 是 4 的次方
        return !(n%4) && isPowerOfFour(n/4);
    }
```

Time complexity: $\mathcal{O}(\log N)$.

Space complexity: $\mathcal{O}(1)$.

## Reference

1. [stackoverflow - Calculate the log base n](https://stackoverflow.com/questions/18874255/calculate-the-log-base-n-with-shift-left-or-shift-right)

2. [stackoverflow - Checking if float is an integer](https://stackoverflow.com/questions/5796983/checking-if-float-is-an-integer)

