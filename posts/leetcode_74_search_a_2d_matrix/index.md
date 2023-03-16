# LeetCode 74. Search a 2D Matrix 解題紀錄



## [題目](https://leetcode.com/problems/search-a-2d-matrix/)


{{< admonition type=quote title="Problem">}}

Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted from left to right.
- The first integer of each row is greater than the last integer of the previous row.
 

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat.jpg)
```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
Output: true
```
**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg)
```
Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
Output: false
```

**Constraints:**

- m == `matrix.length`
- n == `matrix[i].length`
- 1 $\leq$ m, n $\leq$ 100
- $-10^4 \leq$ `matrix[i][j]`, target $\leq$ 104

{{< /admonition >}}


## 想法

{{< admonition type=info title="">}}


{{< /admonition >}}



{{< image src="/images/leetcode_53/exercise.png"  height="200" 
          src_s="/images/leetcode_53/exercise.png" 
          src_l="/images/leetcode_53/exercise.png" 
alt="try dynamic programming yourself" caption="try dynamic programming yourself :)" linked="false">}}

## 解法

### 解法一

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
        
    if(!matrix.size() || matrix[0].size())
    {
        return 0;
    }

    auto r = matrix.size(), c = matrix[0].size();
        
    for(auto i=0; i<r; i++)
    {
        if(matrix[i][c-1]>=target)
        {
            for(auto j=0; j<matrix[i].size(); j++)
            {
                if(matrix[i][j] == target)
                {
                    return 1;
                }
            }
            
            return 0;
        }
    }
    
    return 0;
}
```


### 解法二

- Time complexity:  $\mathcal{O}(n)$.
- Space complexity:  $\mathcal{O}(n)$.



## Reference

