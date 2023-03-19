# LeetCode 1. Two Sum 解題紀錄


## [題目](https://leetcode.com/problems/two-sum/)

{{< admonition type=quote title="Problem">}}

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have **exactly one solution**, and you may not use the same element twice.

You can return the answer in any order.

**Example 1:**

```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
```

Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

**Example 3:**

```
Input: nums = [3,3], target = 6
Output: [0,1]
```

**Constraints:**

-   2 $\leq$ `nums.length` $\leq 10^4$
-   $-10^9 \leq$ `nums[i]` $\leq 10^9$
-   $-10^9 \leq$ `target` $\leq 10^9$
-   **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than $\mathcal{O}(n^2)$ time complexity?

{{< /admonition >}}

## 想法

天字第一題，可以先跳過(誤)

如果是第一次寫 LeetCode，建議可以複製以下內容用 [Online compiler](https://www.onlinegdb.com/online_c++_compiler) 寫寫看，完成後再把 `twoSum` funcion 的部分貼到 [LeetCode](https://leetcode.com/problems/two-sum/) 上。

```cpp
#include <iostream>
#include <vector>

using namespace std;

vector<int> twoSum(vector<int>& nums, int target)
{

    return {};
}

int main()
{
    // input
    vector<int> input = { 2, 7, 11, 15 };

    // get the output
    vector<int> output = twoSum(input, 9);

    // print the output
    cout << "output: ";
    for(auto i:output)
    {
        cout << i << " ";
    }

    return 0;
}
```

---

在 hash table 內尋找想要的 9(target) -2 (目前數字) 為欲尋找的數字 7，若有找到的話，將這個數字傳回 vector 作答；若沒有找到，則將目前數字 2 加入 hash 中，供後人搜尋。

## 解法

### 解法一：暴力法

全部的數字全部找過一輪 ❌(TLE)

Time complexity: $\mathcal{O}(n^2)$.

Space complexity: $\mathcal{O}(1)$.

### 解法二：`unordered_map`

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {

        unordered_map<int, int> mp;

        for(auto i=0; i<nums.size(); i++)
        {
            if(mp.find(target-nums[i]) != mp.end())
            {
                return {i, mp[target-nums[i]]};
            }
            else
            {
                mp[nums[i]] = i;
            }
        }

        return {};
    }
};
```

Time complexity: $\mathcal{O}(n)$.

Space complexity: $\mathcal{O}(1)$.

---

### 完整 C++ 實驗

附上可以直接在 [Online compiler](https://www.onlinegdb.com/online_c++_compiler) 跑的 code，可以先試試看在 [LeetCode](https://leetcode.com/problems/two-sum/) 介面的使用：

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

vector<int> twoSum(vector<int>& nums, int target)
{

    unordered_map<int, int> mp;

    if(!nums.size())
    {
        return {};
    }

    for(auto i=0; i<nums.size(); i++)
    {
        if(mp.find(target - nums[i]) != mp.end())
        {
            return {i, mp[target - nums[i]]};
        }
        else
        {
            mp[nums[i]] = i;
        }
    }

    return {};
}

int main()
{
    vector<int> input = { 2, 7, 11, 15 };
    vector<int> output = twoSum(input, 9);

    // print the output
    cout << "output: ";
    for(auto i:output)
    {
        cout << i << " ";
    }

    return 0;
}
```

