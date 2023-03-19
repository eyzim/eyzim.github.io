# LeetCode 326. Power of Three 解題紀錄


## [題目](https://leetcode.com/problems/power-of-three/)

{{< admonition type=quote title="Problem">}}

Given an integer `n`, return `true` if it is a power of three. Otherwise, return `false`.

An integer `n` is a power of three, if there exists an integer `x` such that `n == 3x`.

**Example 1:**

```
Input: n = 27
Output: true
```

**Example 2:**

```
Input: n = 0
Output: false
```

**Example 3:**

```
Input: n = 9
Output: true
```

**Constraints:**

$-2^{31} \leq n \leq 2^{31} - 1$

Follow up: Could you solve it without loops/recursion?

{{< /admonition >}}

## 想法

最簡單的方法就是一直除以 3，若餘數也是 3 的倍數，且該數字 $n$ 已經除到 1 $(=3^0)$ 時，表示它是 3 的次方，反之則否。

## 解法

### 解法一

就是一直除以 3。

```cpp
bool isPowerOfThree(int n) {

    if(n<1) return 0;

    while(n%3 == 0)
    {
        n/=3;
    }

    return n==1;
}
```

-   Time complexity: $\mathcal{O}(log_3(n))$.
-   Space complexity: $\mathcal{O}(1)$.

### 解法二

題目要求不能做任何的 loop，這題最好想個 15 分鐘。

提示：要用到 $log$

不 :cry:

要 :cry:

偷 :cry:

看 :cry:

啦 :cry:

沒關係，我也忘光光了，附上小抄：

{{< admonition type=note title="Laws of Logarithms">}}

1. $log_{a} x + log_{a} y = log_{a}(x \times y)$

2. $log_{b} x - log_{b} y = \frac{log_{b} x}{log_{b} y} = log_{y} x$

3. $log_{a^m} x^n = \frac{n}{m} log_{a} x$

4. $log_{a}(1) =  0$

5. $log_{a}(a) = 1$

6. $log_{a}(a^n) = n$
   {{< /admonition >}}

假設 $n = 81 = 3^4$，$n$ 已知是 $3$ 的次方，

根據小抄第六點，$log_3 81 = log_3 {3^4} = 4$，只要以 3 為底取 $log$，檢查是否為整數就好。

事情沒那麼簡單，C++ 沒有以 3 為底的 $log$，我們必須用小抄第三點進行換底：

$log_3 81 = \frac{log_{10} 81}{log_{10} 3}$

再根據小抄第二點：

$\frac{log_{10} 81}{log_{10} 3} = log_{10} 81 - log_{10} 3$

最後，我們推得

只要 $log_{10} n - log_{10} 3$ 結果是整數，表示 $n$ 是 3 的次方。

```cpp
bool isPowerOfThree(int n) {

    if(n<1) return 0;

    auto ans = log10(n)/log10(3);

    return int(ans) == ans;
}
```

-   Time complexity: $\mathcal{O}(1)$.
-   Space complexity: $\mathcal{O}(1)$.

## Reference

[^1]: The eye [link](https://www.blueconemonochromacy.org/how-the-eye-functions/)

