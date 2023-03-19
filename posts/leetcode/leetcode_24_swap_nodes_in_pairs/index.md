# LeetCode 24. Swap Nodes in Pairs 解題紀錄


## [題目](https://leetcode.com/problems/swap-nodes-in-pairs/)

{{< admonition type=quote title="Problem">}}

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

**Example 1:**

![](/images/leetcode_24/ex1.png)

```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**

```
Input: head = []
Output: []
```

**Example 3:**

```
Input: head = [1]
Output: [1]
```

**Constraints:**

-   The number of nodes in the list is in the range [0, 100].
-   0 $\leq$ Node.val $\leq$ 100

{{< /admonition >}}

## 想法

下列所有圖片中的 nodes 皆已按照 linked 順序排列，虛線部分為規畫路徑。

移動前，規劃路徑如下：

由於第一個 node 會改變，因此我們先新增一個起始點 pre，它的用途在於指向第一個 node。

要移動 node->next 需要往後一直進行 recursion，因此我們又新增一個指標 cur 指向 pre。

每次 recursion 只要交換一組相鄰的點，第一次我們要交換的是 1 和 2。

{{< image
src="/images/leetcode_24/th1.png"  height="200"
src_s="/images/leetcode_24/th1.png"
src_l="/images/leetcode_24/th1.png"
alt="before"
caption="第一步"
linked="false">}}

移動後，再進入下一步：

{{< image
src="/images/leetcode_24/th2.png"  height="200"
src_s="/images/leetcode_24/th2.png"
src_l="/images/leetcode_24/th2.png"
alt="before"
caption="下一步"
linked="false">}}

## 解法

```cpp
ListNode* swapPairs(ListNode* head) {

    if(!head)   return head;

    /* 新增一個點 pre 指向 head*/
    ListNode pre(0);
    pre.next = head;
    ListNode *cur = &pre;

    while(cur->next && cur->next->next)
    {
        /* 暫存兩個點的資料，方便進行移動 */
        ListNode *tmp1 = cur->next;
        ListNode *tmp2 = cur->next->next->next;

        cur->next = cur->next->next;    // * -> 2
        cur->next->next = tmp1;         // 2 -> 1
        cur->next->next->next = tmp2;   // 1 -> 3

        cur = cur->next->next;
    }

    return pre.next;
}
```

其中，新增點 pre 的方法也可以這樣寫：

上面的做法是直接新增點 pre，並使用 cur 指向它；

下面的作法是先新增指標 pre 指向新的點，再另外使用 cur 指向它。

```cpp
    ListNode *pre = new ListNode(0);
    pre->next = head;
    ListNode *cur = pre;

    ...


    return pre->next;
```

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(1)$.

