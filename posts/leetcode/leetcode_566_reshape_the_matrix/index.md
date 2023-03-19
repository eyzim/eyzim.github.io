# LeetCode 566. Reshape the Matrix 解題紀錄


## [題目](https://leetcode.com/problems/reshape-the-matrix/)

{{< admonition type=quote title="Problem">}}

In MATLAB, there is a handy function called `reshape` which can reshape an `m x n` matrix into a new one with a different size `r x c` keeping its original data.

You are given an `m x n` matrix `mat` and two integers `r` and `c` representing the number of rows and the number of columns of the wanted reshaped matrix.

The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were.

If the `reshape` operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.

**Example 1:**

![](/images/leetcode_566/reshape1-grid.jpg)

```
Input: mat = [[1,2],[3,4]], r = 1, c = 4
Output: [[1,2,3,4]]
```

**Example 2:**

![](/images/leetcode_566/reshape2-grid.jpg)

```
Input: mat = [[1,2],[3,4]], r = 2, c = 4
Output: [[1,2],[3,4]]
```

**Constraints:**

-   m == `mat.length`
-   n == `mat[i].length`
-   1 $\leq$ m, n $\leq$ 100
-   -1000 $\leq$ `mat[i][j]` $\leq$ 1000
-   1 $\leq$ r, c $\leq$ 300

{{< /admonition >}}

## 想法

把原本的元素依序存入暫存的一維陣列中，再依照指定修正後的 matrix 大小一一存入。

## 解法

```cpp
vector<vector<int>> matrixReshape(vector<vector<int>>& mat, int r, int c) {

    // 指定修正大小不符合規定則回傳原始 mat
    if(!mat.size() || !mat[0].size())   return mat;
    if(mat.size() * mat[0].size() != r*c)   return mat;

    // 設定修改後的 mat
    vector<vector<int>> ans(r, vector<int> (c, 0));

    // 將所有元素存入一個一維 temp array 中
    vector<int> temp(r*c, 0);

    for(auto i=0, t=0; i<mat.size(); i++)
    {
        for(auto j=0; j<mat[0].size(); j++)
        {
            temp[t++] = mat[i][j];
        }
    }

    // 存入修改後的 mat
    for(auto i=0, t=0; i<r; i++)
    {
        for(auto j=0; j<c; j++)
        {
            ans[i][j] = temp[t++];
        }
    }

    return ans;
}
```

-   Time complexity: $\mathcal{O}(n^2)$.
-   Space complexity: $\mathcal{O}(n^2)$.

