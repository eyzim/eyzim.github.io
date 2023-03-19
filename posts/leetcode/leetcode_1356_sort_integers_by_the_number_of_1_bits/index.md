# LeetCode 1356. Sort Integers by The Number of 1 Bits 解題紀錄


## [題目](https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/)

{{< admonition type=quote title="Problem">}}

You are given an integer array arr. Sort the integers in the array in ascending order by the number of 1's in their binary representation and in case of two or more integers have the same number of 1's you have to sort them in ascending order.

Return the array after sorting it.

**Example 1:**

```
Input: arr = [0,1,2,3,4,5,6,7,8]
Output: [0,1,2,4,8,3,5,6,7]
```

Explantion: `[0]` is the only integer with 0 bits.
`[1,2,4,8]` all have 1 bit.
`[3,5,6]` have 2 bits.
`[7]` has 3 bits.
The sorted array by bits is `[0,1,2,4,8,3,5,6,7]`

**Example 2:**

```
Input: arr = [1024,512,256,128,64,32,16,8,4,2,1]
Output: [1,2,4,8,16,32,64,128,256,512,1024]
```

Explantion: All integers have 1 bit in the binary representation, you should just sort them in ascending order.

**Constraints:**

-   1 $\leq$ `arr.length` $\leq$ 500
-   0 $\leq$ arr[i] $\leq$ 104

{{< /admonition >}}

## 想法

首先，[Count the number of 1 Bits](https://eyzim.github.io/posts/leetcode/leetcode_191_number_of_1_bits/) 在前面已經實作過了，只是這一次題目沒有要求實際手刻計算方法，那就從善如流地使用 [`__builtin_popcount`](https://www.geeksforgeeks.org/builtin-functions-gcc-compiler/) 這個 function 吧！

接下來要討論如何進行 bits 數分類，若有許多數字有相同的 1 bits 再次進行數字排序。

## 解法

### 解法一

這是非常典型的 bucket sort，在 [Top K Frequent Elements] 這題曾經寫過變形題：先開一個二維 vector ，每個 row 當作 1 個 bucket，因為最多的情況是有 31 bits，共有 32 種可能性，所以我們總共需要 32 個 buckets。

從 `arr` 中一一讀取出來，依照 1 bits 數放到對應的 row/bucket 中。

這樣 bucket sort 就完成了，只要從第一個 row 開始一一查訪，就能得到依照 1 bits 數量排序的數列。

但是題目還有要求要每個 bucket 內部也要依照數字由小排到大呀！

所以在輸出之前，只好先將每個 bucket 內部排序一下。

```cpp
// 191. Number of 1 Bits
int countBits(int n)
{
    auto count = 0;

    while(n)
    {
        n &= (n-1);
        count++;
    }

    return count;
}

vector<int> sortByBits(vector<int>& arr) {

    // 已經知道最多就 31 個 bits
    vector<vector<int>> bucket(32);

    // 先將所有的數字依據 1 bit 的數量放進對應的 bucket 中
    for(auto i:arr)
    {
        bucket[countBits(i)].push_back(i);
    }

    // 待會要將數字一一輸出到這裡
    vector<int> ans;

    // 從 1 bit 數量為 0 的 bucket 開始輸出
    for(auto i=0; i<bucket.size(); i++)
    {
        // 若有多個數字有相同的 1 bit 數量，則須依照數字順序輸出
        sort(bucket[i].begin(), bucket[i].end());

        // 輸出相同 1 bit 數量的 bucket 成員們
        for(auto j=0; j<bucket[i].size(); j++)
        {
            ans.push_back(bucket[i][j]);
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$. (其中 worst case 是 $\mathcal{O}(nlog(n))$.)
-   Space complexity: $\mathcal{O}(n)$.

### 解法二

孤陋寡聞，這 sort 方法還真是第一次用......。

先舉個例子：

```cpp
class Aclass
{
public:
    ...
    bool operator<(Aclass const& other)
    {
    ...
    }
};

int main()
{
    Foo a, b;
    a < b;
}
```

前面這個執行的是 `a.operator<(b)`

後面這個執行的是 `operator<(a, b)`，後面經過 overload 後這個不須要引入 member。

```cpp
class Aclass
{
    ...
};

bool operator<(Aclass const& c1, Aclass const& c2)
{
    ...
}

int main()
{
    Aclass a, b;
    a < b;
}
```

應該稍微可以感受的到差異了吧

---

```cpp
static bool compare(const int &a, const int &b)
{
    int c1 = __builtin_popcount(a);
    int c2 = __builtin_popcount(b);

    // 當 bits 總數相同時，內部排序
    if(c1 == c2)
    {
        return a < b;
    }

    // 排法為由小至大
    return c1 < c2;
}

vector<int> sortByBits(vector<int>& arr) {

    sort(arr.begin(), arr.end(), compare);

    return arr;
}
```

-   Time complexity: $\mathcal{O}(nlog(n))$.
-   Space complexity: $\mathcal{O}(1)$.

## Reference

1. https://www.geeksforgeeks.org/builtin-functions-gcc-compiler/
2. https://leetcode.com/problems/sort-integers-by-the-number-of-1-bits/discuss/534631/C%2B%2B-beats-100-with-__builtin_popcount
3. https://stackoverflow.com/questions/26131112/why-stdsort-requires-static-compare-function

