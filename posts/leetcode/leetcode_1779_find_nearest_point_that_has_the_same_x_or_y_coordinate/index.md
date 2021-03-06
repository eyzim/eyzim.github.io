# LeetCode 1779. Find Nearest Point That Has the Same X or Y Coordinate 解題紀錄


## [題目](https://leetcode.com/problems/find-nearest-point-that-has-the-same-x-or-y-coordinate/)

{{< admonition type=quote title="Problem">}}

You are given two integers, `x` and `y`, which represent your current location on a Cartesian grid: `(x, y)`. You are also given an array points where each `points[i] = [ai, bi]` represents that a point exists at `(ai, bi)`. A point is **valid** if it shares the same x-coordinate or the same y-coordinate as your location.

Return the index **_(0-indexed)_** of the **_valid_** point with the smallest **_Manhattan distance_** from your current location. If there are multiple, return the valid point with the **_smallest_** index. If there are no valid points, return -1.

The **Manhattan distance** between two points `(x1, y1)` and `(x2, y2)` is `abs(x1 - x2) + abs(y1 - y2)`.

**Example 1:**

```
Input: x = 3, y = 4, points = [[1,2],[3,1],[2,4],[2,3],[4,4]]
Output: 2
```

Explanation: Of all the points, only [3,1], [2,4] and [4,4] are valid. Of the valid points, [2,4] and [4,4] have the smallest Manhattan distance from your current location, with a distance of 1. [2,4] has the smallest index, so return 2.

**Example 2:**

```
Input: x = 3, y = 4, points = [[3,4]]
Output: 0
```

Explanation: The answer is allowed to be on the same location as your current location.

**Example 3:**

```
Input: x = 3, y = 4, points = [[2,3]]
Output: -1
```

Explanation: There are no valid points.

**Constraints:**

-   1 $\leq$ `points.length` $\leq 10^4$
-   `points[i].length` == 2
-   1 $\leq x , y, a_i, b_i \leq 10^4$

{{< /admonition >}}

## 想法

1. 確認是否和指定座標有相同的 $x$ 座標 或是 $y$ 座標
2. 取得和指定座標 $(x, y)$ 的距離，計算方式為：$\lvert(x_1 - x_2)\rvert + \lvert(y_1 - y_2)\rvert$
3. 求所有距離中的最小值

## 解法

```cpp
int nearestValidPoint(int x, int y, vector<vector<int>>& points) {

    // minipos 紀錄第幾組為最小距離，mini 計算最小距離的值
    auto minpos = -1, mini = INT_MAX;

    // 遍歷所有座標
    for(auto i=0; i<points.size(); i++)
    {
        // 確認這個座標是否符合條件
        if(points[i][0]==x || points[i][1]==y)
        {
            // 根據公式計算距離
            auto dis = abs(x-points[i][0]) + abs(y-points[i][1]);

            // 有更接近的座標距離出現
            if(mini > dis)
            {
                minpos = i;
                mini = dis;
            }
        }
    }

    return minpos;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

