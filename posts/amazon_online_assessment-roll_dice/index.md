# Amazon Online Assessment (OA) - Roll Dice 解題紀錄


# Amazon Online Assessment (OA) - Roll Dice

Complexity: medium
Date: October 7, 2021
No: 0
Status: Completed
Tags: array, hash table
url: https://algo.monster/problems/roll_dice

## **Amazon Online Assessment (OA) - Roll Dice**

-   [stackoverflow](https://stackoverflow.com/questions/56712677/does-anyone-know-a-better-solution-to-the-below-dice-problem) 也有人問過

> Given `N` dices each face ranging from `1` to `6`, return the minimum number of rotations necessary for each dice show the same face.
> Notice in one rotation you can rotate the dice to the adjacent face.
> For example, you can rotate the dice shows `1` to show `2`, `3`, `4`, or `5`. But to make it show 6, you need two rotations.

Input
The input consists of three arguments:
`N`: a list of integer represent dices each face ranging from 1 to 6
Output
return the minimum number of rotations necessary for each dice show the same face

Example 1:

> ```cpp
> Input: N = [6, 5, 4]
> Output: 2
> ```
>
> Example 2:
>
> ```cpp
> Input: N = [6, 6, 1]
> Output: 2
> ```
>
> Example 3:
>
> ```cpp
> Input: N = [6, 1, 5, 4]
> Output: 3
> ```

### 思路：

1. 先統計一下 1~6 點各擲出幾次
2. 假設每個點都需要花一次翻面次數就能翻到大家都一樣的 `理想目標`，總花費應該等於擲骰子的次數
3. 事與願違，我們需要找出真正能被骰子擲出的 `目標`，這個目標值的範圍是 1~6 之間。我們要在所有目標值造成的花費中尋找出最便宜的方法。

假設 input = [1, 2, 3, 4, 5, 6]

如果要計算總花費，

-   當目標值為 1，
    1 面翻到 1 的次數 + 2 面翻到 1 的次數 + 3 面翻到 1 的次數 +
    4 面翻到 1 的次數 + 5 面翻到 1 的次數 + 6 的翻到 1 的次數
    = 0 + 1 + 1 + 1 + 1 + 2
-   當目標值為 2，
    1 面翻到 2 的次數 + 2 面翻到 2 的次數 + 3 面翻到 2 的次數 +
    4 面翻到 2 的次數 + 5 面翻到 2 的次數 + 6 面翻到 2 的次數
    = 1 + 0 + 1 + 1 + 2 + 1
-   當目標值為 3，
    1 面翻到 3 的次數 + 2 面翻到 3 的次數 + 3 面翻到 3 的次數 +
    4 面翻到 3 的次數 + 5 面翻到 3 的次數 + 6 面翻到 3 的次數
    = 1 + 1 + 0 + 2 + 1 + 1

發現到

1. 當 `目標值` 移動到該數字時，它自己就是目標，所以它的總花費會比自己原本要移動到 `理想目標` 少一步。
   而現在 `目標值` 的背面，也就是在骰子上我們看到 1 面背面是 6、2 面背面是 5、3 面背面是 4，這些背面的值要再比原本到 `理想目標` 多一步。
2. 既不是 `目標值` 也不是 `目標值` 背面的數字，真正的符合了原先 `理想目標` 的假設 ⇒ 只需要一步就可以翻到目標的數字。如果大家都只需要一步，那是不是可以先省下這部分的計算，最後再加上就好了呢？

```cpp
class Solution {
public:
    int minCostofRotation(vector<int>& dices) {

        unordered_map<int, int> mp;
        int cost = INT_MAX;

				// step 1
        for(auto &i:dices)
        {
            mp[i]++;
        }

				// step 3
        for(auto i=1; i<=6; i++)
        {
            cost = min(cost, mp[7-i]-mp[i]);
        }

				// step 2
        return cost + dices.size();
    }
};
```

Time complexity: $\mathcal{O}(n)$.

Space complexity: $\mathcal{O}(n)$.

-   很完整能直接跑出結果的解法 (但我只想看方法 oao)
    ```cpp
    #include <algorithm> // copy
    #include <iostream> // cin, cout
    #include <iterator> // back_inserter, istream_iterator
    #include <sstream> // istringstream
    #include <string> // getline, string
    #include <vector> // vector
    #include <limits.h> //INT_MAX

    using namespace std;

    int number_of_rotations(vector<int> dices) {
        // WRITE YOUR BRILLIANT CODE HERE

        unordered_map<int, int> mp;
        int cost = INT_MAX;

        for(auto &i:dices)
        {
            mp[i]++;
        }

        for(auto i=1; i<=6; i++)
        {
            cost = min(cost, mp[7-i]-mp[i]);
        }

        return cost + dices.size();

    }

    template<typename T>
    vector<T> get_words() {
        string line;
        getline(cin, line);
        istringstream ss{line};
        vector<T> v;
        copy(istream_iterator<T>{ss}, istream_iterator<T>{}, back_inserter(v));
        return v;
    }

    int main() {
        vector<int> dice = get_words<int>();
        int res = number_of_rotations(dice);
        cout << res << '\n';
    }
    ```

