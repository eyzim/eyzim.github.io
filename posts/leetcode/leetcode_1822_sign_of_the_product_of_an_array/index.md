# LeetCode 1822. Sign of the Product of an Array 解題紀錄


## [題目](https://leetcode.com/problems/sign-of-the-product-of-an-array/)

{{< admonition type=quote title="Problem">}}

There is a function `signFunc(x)` that returns:

-   `1` if `x` is positive.
-   `-1` if `x` is negative.
-   `0` if `x` is equal to `0`.
    You are given an integer array `nums`. Let `product` be the product of all values in the array `nums`.

Return `signFunc(product)`.

**Example 1:**

```
Input: nums = [-1,-2,-3,-4,3,2,1]
Output: 1
```

Explanation: The product of all values in the array is 144, and signFunc(144) = 1

**Example 2:**

```
Input: nums = [1,5,0,2,-3]
Output: 0
```

Explanation: The product of all values in the array is 0, and signFunc(0) = 0

**Example 3:**

```
Input: nums = [-1,1,-1,1,-1]
Output: -1
```

Explanation: The product of all values in the array is -1, and signFunc(-1) = -1

**Constraints:**

-   1 $\leq$ `nums.length` $\leq$ 1000
-   -100 $\leq$ `nums[i]` $\leq$ 100

{{< /admonition >}}

## 想法

當 `nums` 中有任何元素為 0，乘積必為 0；會造成結果 `+1` 與 `-1` 擺盪，只與數字前的正負號有關，所以可以忽略數值的運算。

## 解法

```cpp
int arraySign(vector<int>& nums) {

    auto ans = 1;

    for(auto i:nums)
    {
        // 只有 nums 中有元素為 0，乘積必為 0
        if(i == 0)  return 0;

        // 若這個元素為負值，必定會造成結果乘以 (-1)
        if(i<0)
        {
            ans = (-1) * ans;
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

