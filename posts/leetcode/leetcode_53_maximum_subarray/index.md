# LeetCode 53. Maximum Subarray 解題紀錄



## [題目](https://leetcode.com/problems/maximum-subarray/)


{{< admonition type=quote title="Problem">}}

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

A subarray is a contiguous part of an array.


**Example 1:**
```
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
Output: 6
```
Explanation: [4,-1,2,1] has the largest sum = 6.

**Example 2:**
```
Input: nums = [1]
Output: 1
```
**Example 3:**
```
Input: nums = [5,4,-1,7,8]
Output: 23
```

**Constraints:**

- 1 <= nums.length <= 105
- $-10^4 \leq$ nums[i] $\leq 10^4$
 

Follow up: If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

{{< /admonition >}}


## 解法


### 解法一：暴力解

全部可能的 subarray 計算過一輪，就能得到總和最大的值。

先比較 nums 中自己和從頭到自己哪個較大，再比較 local maximum 中誰最大。

注意：
因為有可能 local maximum 都是負值，所以要將 maxi 設為 `INT_MIN`。

```cpp
int maxSubArray(vector<int>& nums) {
    
    auto maxi = INT_MIN, sum = 0;
    
    for(auto i=0; i<nums.size(); i++)
    {
        for(auto j=i, sum=0; j<nums.size(); j++)
        {
            // 一直累加
            sum += nums[j];

            // 紀錄最大值
            maxi = max(maxi, sum);
        }
    }
    
    return maxi;
}
```
- Time complexity:  $\mathcal{O}(n^2)$.
- Space complexity:  $\mathcal{O}(1)$.


### 解法二：DP


1. 若從 $(a_1+ ... + a_n+a_{n+1})$ 大於 $a_{n+1}$ 獨自起始，那麼我們可以認為團結繼續進行加法的值會越來越大；反之，若 $(a_1+ ... + a_n+a_{n+1})$ 小於 $a_{n+1}$，由此從頭開始 / 斷尾累計反而比較好。
2. 紀錄每次斷尾的最大值為多少。

{{< image src="/images/leetcode_53/do.png"  height="200" 
          src_s="/images/leetcode_53/do.png" 
          src_l="/images/leetcode_53/do.png" 
alt="dynamic programming" caption="dynamic programming" linked="false">}}


透過上圖：

由第二個元素 `1` 可知，與前面項的總和相比，它自己可以獨立產生一個更大的值，於是我們就選擇從它這個項目開始累加，放棄前面累計的值。

由第八個元素 `-5` 可知，前幾項累計最大值為 6，當將自己這項加上去，會使總和變小。


```cpp
int maxSubArray(vector<int>& nums) {
    
    auto maxi = INT_MIN, sum = 0;
    
    for(auto i:nums)
    {
        // 斷尾 or 延續前面的 subarray
        sum = max(i, sum+i);
        
        // 紀錄最大值
        maxi = max(sum, maxi);
    }
            
    return maxi;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(1)$.


太久沒寫 DP 題目、或是對敘述理解不來，可以嘗試看看自己寫一遍 :smile:

{{< image src="/images/leetcode_53/exercise.png"  height="200" 
          src_s="/images/leetcode_53/exercise.png" 
          src_l="/images/leetcode_53/exercise.png" 
alt="try dynamic programming yourself" caption="try dynamic programming yourself :)" linked="false">}}


