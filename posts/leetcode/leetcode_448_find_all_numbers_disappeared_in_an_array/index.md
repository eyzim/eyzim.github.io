# LeetCode 448. Find All Numbers Disappeared in an Array 解題紀錄


## [題目](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/)

{{< admonition type=quote title="Problem">}}

Given an array nums of n integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in nums.

**Example 1:**

```
Input: nums = [4,3,2,7,8,2,3,1]
Output: [5,6]
```

**Example 2:**

```
Input: nums = [1,1]
Output: [2]
```

**Constraints:**

-   n == `nums.length`
-   1 $\leq$ n $\leq$ $10^5$
-   1 $\leq$ `nums[i]` $\leq$ n

**Follow up:**

Could you do it without extra space and in $O(n)$ runtime? You may assume the returned list does not count as extra space.

{{< /admonition >}}

## 想法

輸入 vector 有 n 個 elements 就代表 elements 範圍由 `[1, n]`；

例如：input vector 有三個 elements 表示其範圍是 `[1, 3]`。

需要知道目前 vector 的 size，再尋找各個 elements 中是否有依序出現由 1 到 n 的數字。

## 解法

### 解法一

最直覺的方法是將 input vector 的元素全掃過一輪，填入 unordered map，最後再檢查是否範圍內的各數字都有出現，若未出現，則插入 `ans` vector。

```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {

    vector<int> ans;

    // 建立檢查數字是否存在的 unordered_map
    unordered_map<int, bool> mp;

    // 一一掃過所有的元素，並在對應數值設為 1
    for(auto i:nums)
    {
        mp[i] = 1;
    }

    // 檢查 mp 中，若該數字不存在則 push_back
    for(auto i=1; i<=nums.size(); i++)
    {
        if(mp[i] == 0)
        {
            ans.push_back(i);
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

### 解法二

若該位置的數字已經出現過，將該位置的值改為負數。

當全部的元素接已經完成前一步驟，該位置若 $>0$，就代表這個數字沒出現過。

例如：最後發現 nums[2] 到結束時仍為 10，表示 2+1=3 這個數字沒有出現過。

下有完整舉例

0. input array：

```
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4    3    2    7    8    2    3    1
```

1. nums[0] = 4 ➙ 把第四個元素 nums[3] 的數值變成負數

```
🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4    3    2   -7    8    2    3    1
               :triangular_flag_on_post:
```

2. nums[1] = 3 ➙ 把第三個元素 nums[2] 的數值變成負數

```
     🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4    3   -2   -7    8    2    3    1
          :triangular_flag_on_post:
```

3. nums[2] = -2，我們要看的是 abs(-2) = 2 ➙ 把第二個元素 nums[1] 的數值變成負數

```
          🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4   -3   -2   -7    8    2    3    1
     :triangular_flag_on_post:
```

4. nums[3] = -7，我們要看的是 abs(-7) = 7 ➙ 把第七個元素 nums[6] 的數值變成負數

```
               🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4   -3   -2   -7    8    2   -3    1
                              :triangular_flag_on_post:
```

5. nums[4] = 8 ➙ 把第八個元素 nums[7] 的數值變成負數

```
                    🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4   -3   -2   -7    8    2   -3   -1
                                   :triangular_flag_on_post:
```

6. nums[5] = 2 ➙ 把第三個元素 nums[1] 的數值變成負數

```
                         🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4   -3   -2   -7    8    2   -3   -1
     :triangular_flag_on_post:
```

7. nums[6] = -3，我們要看的是 abs(-3) = 3 ➙ 把第七個元素 nums[2] 的數值變成負數

```
                              🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
 4   -3   -2   -7    8    2   -3   -1
          :triangular_flag_on_post:
```

8. nums[7] = -1，我們要看的是 abs(-1) = 1 ➙ 把第七個元素 nums[0] 的數值維持負數

```
                                   🏷️
[0]  [1]  [2]  [3]  [4]  [5]  [6]  [7]
-4   -3   -2   -7    8    2   -3   -1
:triangular_flag_on_post:
```

最後發現 nums[4] 和 nums[5] 兩個元素的值仍為正數，表示 4+1=5 和 5+1=6 兩數字都沒有出現過。

```cpp
vector<int> findDisappearedNumbers(vector<int>& nums) {

    vector<int> ans;

    for(auto i=0; i<nums.size(); i++)
    {
        int n = abs(nums[i])-1;

        if(nums[n] > 0)
        {
            nums[n] *= -1;
        }
    }

    for(auto i=0; i<nums.size(); i++)
    {
        if(nums[i]>0)
        {
            ans.push_back(i+1);
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

這個方法好猛！！！

## Reference

1. https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/discuss/92958/c%2B%2B-solution-O(1)-space

