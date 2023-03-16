# LeetCode 118. Pascal's Triangle 解題紀錄



## [題目](https://leetcode.com/problems/pascals-triangle/)


{{< admonition type=quote title="Problem">}}

Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

![](/images/leetcode_118/PascalTriangleAnimated2.gif)
 

**Example 1:**
```
Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
```
**Example 2:**
```
Input: numRows = 1
Output: [[1]]
```

**Constraints:**

- 1 $\leq$ `numRows` $\leq$ 30

{{< /admonition >}}


## 想法

1. 預設每個 row 的元素都是 1。
2. 由前一個 row 獲得位置的值：從每個 row 第二個元素起與前一 row 同位置與前一位置的值相加。

## 解法

```cpp
vector<vector<int>> generate(int numRows) {
    
    vector<vector<int>> ans;
            
    for(auto i=0; i<numRows; ++i)
    {
        vector<int> row(i+1, 1);
        
        for(auto j=1; j<i; ++j)
        {
            row[j] = ans[i-1][j-1] + ans[i-1][j];
        }
        
        ans.push_back(row);
    }
    
    return ans;
}
```

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.

