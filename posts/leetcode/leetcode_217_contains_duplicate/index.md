# LeetCode 217. Contains Duplicate 解題紀錄


## [題目](https://leetcode.com/problems/contains-duplicate/)

{{< admonition type=quote title="Problem">}}

Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.

**Example 1:**

```
Input: nums = [1,2,3,1]
Output: true
```

**Example 2:**

```
Input: nums = [1,2,3,4]
Output: false
```

**Example 3:**

```
Input: nums = [1,1,1,3,3,4,3,2,4,2]
Output: true
```

**Constraints:**

-   1 $\leq$ `nums.length` $\leq 10^5$
-   $-10^9 \leq$ `nums[i]` $\leq 10^9$

{{< /admonition >}}

## 想法

作法大致分為先排序及未排序兩種：排序後檢查是否有出現接連兩個相同數字，另一種則是開新的儲存空間一一記錄是否曾出現過。

## 解法

### 解法一：`unordered_set`

遍歷 `nums` 中所有元素，並儲存至 `mp` 中。

若 `mp` 的 size 和 `nums` 的 size 不同，表示有出現重複的元素。

```cpp
bool containsDuplicate(vector<int>& nums) {

    unordered_set<int> mp;

    for(auto i:nums)
    {
        mp.insert(i);
    }

    if(nums.size() != mp.size())
    {
        return 1;
    }

    return 0;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法二：`unordered_map`

遍歷 `nums` 中所有元素，以 `unordered_map` 紀錄是否曾出現過。

```cpp
bool containsDuplicate(vector<int>& nums) {

    unordered_map<int, bool> mp;

    for(auto i:nums)
    {
        /* 若 mp[i] 的值已經註記為出現過 */
        if(mp[i] == 1)
        {
            return 1;
        }

        /* 將 mp[i] 註記為出現過 */
        mp[i] = 1;
    }

    return 0;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法三：`sort`

將 `nums` 中元素排序，排序後檢查是否有相鄰的數字相等，若有相等的數字，即為出現重複的元素。

```cpp
bool containsDuplicate(vector<int>& nums) {

    sort(nums.begin(), nums.end());

    /* 檢查相鄰的數字是否相等 */
    for(auto i=0; i<nums.size()-1; i++)
    {
        if(nums[i] == nums[i+1])
        {
            return 1;
        }
    }

    return 0;
}
```

C++ 中 `sort` 的時間複雜度為 $\mathcal{O}(n log(n))$，加上一一檢查相鄰元素的時間 $\mathcal{O}(n)$ ==> $\mathcal{O}(n)$.

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法四：`bitset`

我們只是要檢驗其數字在數字空間中是否重疊，所以可以將所有數字化為正值：

先建立一個能裝下從負值到正值大小的 bitmap，再來是檢查是否已經在 bitmap 中存在，若有則返回 1，無則放進 bitmap。

```cpp
bool containsDuplicate(vector<int>& nums) {

    bitset<2000000> bset;

    for(auto i:nums)
    {
        i += 1000000;

        if(bset.test(i))
        {
            return 1;
            break;
        }

        bset.set(i);
    }

    return 0;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 各方法比較

記得曾經看過有人說，LeetCode 的時間跟你當下的網路順暢有關 :cry:

| 方法          | 時間  | 記憶體  |
| :------------ | :---- | :------ |
| Unordered set | 90 ms | 20.2 MB |
| Sort          | 54 ms | 15.6 MB |
| Bitmap        | 27 ms | 15.8 MB |

