# LeetCode 976. Largest Perimeter Triangle 解題紀錄



## [題目](https://leetcode.com/problems/largest-perimeter-triangle/)


{{< admonition type=quote title="Problem">}}

Given an integer array `nums`, return the *largest perimeter of a triangle with a non-zero area, formed from three of these lengths*. If it is impossible to form any triangle of a non-zero area, return `0`.

**Example 1:**
```
Input: nums = [2,1,2]
Output: 5
```
**Example 2:**
```
Input: nums = [1,2,1]
Output: 0
```

**Constraints:**

- 3 $\leq$ `nums.length` $\leq$ 104
- 1 $\leq$ `nums[i]` $\leq$ 106

{{< /admonition >}}


## 想法

題目希望找出組成「最大面積」的三角形邊常，理論上只要三條邊長是前三大即可。

由於要組成三角形，還需要依照三角不等式檢查是否符合「三角形任兩邊相加必大於第三邊」。

## 解法

```cpp
int largestPerimeter(vector<int>& nums) {
    
    // 由小排到大
    sort(nums.begin(), nums.end());
    
    // 從最有可能的最長元素開始
    for(auto i=nums.size()-1; i>=2; i--)
    {
        // 根據三角不等式確認是否能組成
        if(nums[i] < nums[i-1]+nums[i-2])
        {
            return nums[i] + nums[i-1] + nums[i-2];
        }
    }
    
    return 0;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(1)$.


