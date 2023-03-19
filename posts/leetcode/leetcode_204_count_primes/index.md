# LeetCode 204. Count Primes 解題紀錄


## [題目](https://leetcode.com/problems/count-primes/)

{{< admonition type=quote title="Problem">}}

Given an integer n, return the number of prime numbers that are strictly less than n.

**Example 1:**

```
Input: n = 10
Output: 4
```

Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.

**Example 2:**

```
Input: n = 0
Output: 0
```

**Example 3:**

```
Input: n = 1
Output: 0
```

**Constraints:**

0 $\leq$ n $\leq$ 5 $\times 10^6$

{{< /admonition >}}

## 想法

{{< admonition type=info title="質數">}}

「質數」有兩個定義：

1. 必為大於 1 的**整數**
2. 只有**兩個因數**，一個是 1、一個是自己

100 以內的質數：
2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97， 共 25 個。

{{< /admonition >}}

根據以上定義，我們可以簡單寫出一個 isPrime function 來判斷這個數字是否為質數。

給出一個數字 n，從 2 到 n-1 依序除除看，若能整除，代表這個數字有除了 1 和 n 以外的因數，也就意味著 n 並非質數。

```cpp
bool isPrime(int n)
{
    if(n < 2)   return 0;

    // 從 2 到 n-1 依序除除看
    for(auto i=2; i<n; i++)
    {
        if(n%i == 0)
        {
            return 0;
        }
    }

    return 1;
}
```

-   1.215 _sec_.

現在執行效率有點差，有哪些數字是一看就知道不是質數的數字呢？

4, 6, 8, 10, 12, 14... 那些偶數，一看就知道它們能被 2 整除，可以排除計算，節省一點時間。

希望能在檢查到偶數時直接回答 false，但是 2 又是質數，返回 n 是否為 2 即可。

因為事先已經檢查過偶數了，接下來的迴圈檢查就可以跳過它們囉，節省一點時間！

```cpp
bool isPrime(int n)
{
    if(n < 2)   return 0;

    // 若 n=2 則為質數，若為 2 的倍數則否
    if(n%2 == 0)    return n==2;

    // 從 3 到 n-1 依序除除看
    for(auto i=3; i<n; i++)
    {
        if(n%i == 0)
        {
            return 0;
        }
    }

    return 1;
}
```

-   1.215 _sec_.

好像...沒什麼差別，上面的方法還是依序檢查了 3 到 n-1 ，應該直接在迴圈中跳過 2 才對吧 XD

```cpp
bool isPrime(int n)
{
    if(n < 2)   return 0;

    for(auto i=3; i<n; i+=2)
    {
        if(n%i == 0)
        {
            return 0;
        }
    }

    return 1;
}
```

-   0.628 _sec_.

現在的方法搜尋了 $\frac{n}{2}$ 次，效能也提升了將近一倍 :kissing_smiling_eyes:

小結一下，到目前為止的優化，時間複雜度是

`isPrime` $\mathcal{O}(n)$ $\times$`countPrimes`$ \mathcal{O}(n^2)=$ $\mathcal{O}(n^2)$

又以 36 為例，

$36$

$= 1 \times 36 = 2 \times 18 = 3 \times 12$

$= 4 \times 9 = 6 \times 6 = 9 \times 4$

$= 12 \times 3 = 18 \times 2 = 36 \times 1$

仔細想想，我們在遇到 6 之後，所有的因數組合全部都和前面相同：最後的 $36 \times 1$ 和第一段 $1 \times 36$ 是重複的，那麼我們只要計算到 $i \leq \sqrt{n}$ 或是 $i \times i \leq n$ 就能停止了，後半部都是一模一樣嘛！

```cpp
bool isPrime(int n)
{
    if(n < 2)   return 0;

    for(auto i=3; i*i<=n; i++)
    {
        if(n%i == 0)
        {
            return 0;
        }
    }

    return 1;
}
```

-   0.007 _sec_.

目前的複雜度是 $\mathcal{O}(n\sqrt{n})$

雖然思考了一段時間，但是還是 Time Limit Exceeded :cry:

{{< admonition type=info title="break & continue">}}

### break

在迴圈中執行時，只要 if 條件符合使用 break 的時機，立刻會離開 for/while 迴圈。

```cpp
#include <iostream>

using namespace std;

int main()
{
    for(auto i=0; i<6; i++)
    {
        if(i==3)
        {
            break;
        }

        cout << i << ", ";
    }

    cout << "end";

    return 0;
}
```

```
0, 1, 2, end
```

### continue

在迴圈中執行時，只要 if 條件符合使用 continue 的時機，將會結束本次迴圈，進入下一次 for/while 的條件判斷 (ex: i++)

```cpp
#include <iostream>

using namespace std;

int main()
{
    for(auto i=0; i<6; i++)
    {
        if(i==3)
        {
            continue;
        }

        cout << i << ", ";
    }

    cout << "end";

    return 0;
}
```

```
0, 1, 2, 4, 5, end
```

{{< /admonition >}}

## 解法

### 解法一：Sieve of Eratosthenes 法

1. 預設 1 ~ n-1 所有數字都是質數。

2. 第一輪找到 2，2 是質數，但 2 的倍數則不是質數，所以我們可以剔除 4, 6, 8, 10, 12,...

3. 第二輪找到 3，3 是質數，但 3 的倍數則不是質數，所以我們可以剔除 ~~6~~, 9, ~~12~~,...，由於在第一輪 2 的倍數已經先移除 6 和 12 了，所以節省了相當多的時間。

4. 最後我們再一一清點 `isPrime` 中有多少質數。

{{< image src="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif"  height="200" src_s="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif" src_l="https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif"
caption="Sieve of Eratosthenes" linked="false">}}

```cpp
int countPrimes(int n) {

    if(n<2) return 0;

    // 預設 n 個數為質數
    vector<bool> isPrime(n, 1);

    // 依序檢查是否為 n 的因數
    for(auto i=2; i*i<n; i++)
    {
        // 如果這個數字確定是質數
        if(isPrime[i])
        {
            // 將 i 的倍數全部標示為「非質數」
            for(auto j=i*i; j<n; j+=i)
            {
                isPrime[j] = 0;
            }
        }
    }

    // 計算總共有幾個質數
    auto ans = 0;

    for(auto i=2; i<n; i++)
    {
        if(isPrime[i])  ans++;
    }

    return ans;
}
```

-   0.003 _sec_.

-   Time complexity: $\mathcal{O}(n log (log(n)))$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法二

1. 預設 1 ~ n 所有數字都是質數。
2. 第一個數字看到 3，3 是質數，但 3 的倍數不是質數，為了減少計算偶數的計算，3 的奇數倍為 $3 \times 3$, $3 \times 5$, $3 \times 7$, $3 \times 9$, $3 \times (2n-1)$：9, 15, 21, 27,...
3. 剩下作法同解法一

```cpp
int countPrimes(int n) {

    if(n<2)
    {
        return 0;
    }

    // 預設 n 個數是質數
    vector<bool> prime(n, 1);

    // 2 本身就是一個質數，因為這個 function 沒有計算偶數的部分，故當作特例
    auto ans = 1;

    // 等同於 i*i < n
    int upper = sqrt(n);

    // 依序掃描所有的「奇數」
    for(auto i=3; i<n; i+=2)
    {
        // 若此數已經不是質數，則逕行檢查下一個數字
        if(!prime[i])
        {
            continue;
        }

        ans++;

        // 防止 i^2 溢位
        if(i > upper)
        {
            continue;
        }

        // 每次增加 2*i，讓 j 永遠都是奇數
        for(auto j=i*i; j<n; j+=2*i)
        {
            prime[j] = 0;
        }
    }

    return ans;
}
```

-   0.003 _sec_.

-   Time complexity: $\mathcal{O}(n log (log(n)))$.
-   Space complexity: $\mathcal{O}(n)$.

## Reference

1. https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes
2. https://www.geeksforgeeks.org/sieve-of-eratosthenes/

