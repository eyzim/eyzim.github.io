# LeetCode 141. Linked List Cycle 解題紀錄


## [題目](https://leetcode.com/problems/linked-list-cycle/)

{{< admonition type=quote title="Problem">}}

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that** `pos` **is not passed as a parameter**.

Return `true` _if_ there is a cycle in the linked list. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
Input: head = [3,2,0,-4], pos = 1
Output: true
```

Explanation: There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

```
Input: head = [1,2], pos = 0
Output: true
```

Explanation: There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

```
Input: head = [1], pos = -1
Output: false
```

Explanation: There is no cycle in the linked list.

**Constraints:**

-   The number of the nodes in the list is in the range $[0, 10^4]$.
-   $-10^5 \leq$ `Node.val` $\leq 10^5$
    pos is -1 or a valid index in the linked-list.

**Follow up:** Can you solve it using $\mathcal{O}(1)$ (i.e. constant) memory?

{{< /admonition >}}

## 想法

這題我看了 n 遍才看懂他在幹嘛 orz

---

新增兩個指標，分別叫做 `walk` 和 `run` ，每一秒走一個、跑兩格。

前三個點為單向通行，不在 cycle 裡面，後五個點屬於 cycle 之中。

如果這個 linked list 存在 cycle，則 `walk` 和 `run` 總有一秒會相遇。

下圖中可以看到：

第零秒兩個同時從 1 出發，到第五秒時兩個在 6 相遇。

{{< image src="/images/leetcode_141/walkandrun.gif"  height="200"
          src_s="/images/leetcode_141/walkandrun.gif"
          src_l="/images/leetcode_141/walkandrun.gif"
alt="walk and run in the linked list" caption="walk and run in the linked list"  linked="false">}}

### 相遇時間

至於要花幾秒才會相遇呢?

1. 先耗費走完 line 的時間

2. 當 `walk` 進入 cycle 中後，再來只要看 cycle

3. **line % cycle** 是在等待 `walk` 進入 cycle 時，`run` 的位置

4. `run` 應花費 **cycle - (line % cycle)** 的時間追上 `walk`

    設 $line = 3$ 和 $cycle = 5$

    $ line+(cycle-(line\mod cycle))$

    $= 3 + (5-(3 \mod 5)) $

    $= 3+(5-3) $

    $= 3+2 = 5 $

    ⇒ 總共需要 $5 秒/steps$

`pos` 就是這樣反推出來ㄉ 凸^-^凸

## 解法

```cpp
bool hasCycle(ListNode *head) {

    if(!head)   return head;

    // walk 和 slow 都從起點出發
    ListNode *walk = head, *run = head;

    while(run->next && run->next->next)
    {
        // 一次走一步
        walk = walk->next;

        // 一次走兩步
        run = run->next->next;

        // walk 和 run 會重疊代表著 linked list 中有迴圈
        if(walk == run)
        {
            return 1;
        }
    }

    return 0;
}
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

