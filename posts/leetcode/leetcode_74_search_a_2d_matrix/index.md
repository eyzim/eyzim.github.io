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

分兩種做法：

1. 每一 row 都是已經依照順序排好的 `vector`，先依照每個 row 的第一個元素尋找，再往 column 內部檢查

2. 將整個矩陣視為一個很長的已排序數列，使用 **Two pointers** 夾擊出元素。

{{< admonition type=note title="Two pointers">}}
若是出現一個已經排序好很長的 array，要搜尋某一數字，一個一個檢查太浪費時間了。

我們會直接抓正中間的元素和目標值比大小，若目標值比較大，則向右邊尋找；
若目標值比較小，則向左尋找，依照這個模式，可以有一個簡單的模板。

```cpp
// 目標值
int target = 90;

// 要搜尋的 array
vector<int> arr = {1, 3, 5, 7, ... , 101};

auto left = 0, right = arr.size()-1;

while(left <= right)
{
    // 取中間位置
    int mid = (right + left) / 2;

    // 目標值較大
    if(arr[mid] < target)
    {
        left = mid + 1;
    }
    // 目標較小
    else if(arr[mid] > target)
    {
        right = mid - 1;
    }
    // 是目標
    else
    {
        return 1;
    }

    return 0;
}
```

{{< /admonition >}}

## 解法

### 解法一

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {

    auto r = matrix.size(), c = matrix[0].size();
        
    for(auto i=0; i<r; i++)
    {
        // 比較前一行的最後一個元素與 target 的大小
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
- Time complexity:  $\mathcal{O}(log(mn)) = {O}(log(m) + log(n))$.
- Space complexity:  $\mathcal{O}(1)$.


### 解法二

```cpp
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    
    int c = matrix[0].size(), r = matrix.size(), 
        left = 0, 
        right = r * c - 1;
    
    while(left <= right)
    {
        // 取得中間的位置
        int mid = (right + left) / 2;
        
        // 找到了
        if(target == matrix[mid/c][mid%c])
        {
            return 1;
        }
        // 往左邊尋找
        else if(target > matrix[mid/c][mid%c])
        {
            left = mid + 1;
        }
        // 往右邊尋找
        else
        {
            right = mid - 1;
        }
    }
    
    return 0;
}
```

- Time complexity:  $\mathcal{O}(log(mn)) = {O}(log(m) + log(n))$.
- Space complexity:  $\mathcal{O}(1)$.

