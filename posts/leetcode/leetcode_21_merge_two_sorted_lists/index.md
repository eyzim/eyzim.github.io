# LeetCode 21. Merge Two Sorted Lists 解題紀錄


## [題目](https://leetcode.com/problems/merge-two-sorted-lists/)

{{< admonition type=quote title="Problem">}}

You are given the heads of two sorted linked lists list1 and list2.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return _the head of the merged linked list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/merge_ex1.jpg)

```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

**Example 2:**

```
Input: list1 = [], list2 = []
Output: []
```

**Example 3:**

```
Input: list1 = [], list2 = [0]
Output: [0]
```

**Constraints:**

-   The number of nodes in both lists is in the range $[0, 50]$.
-   -100 $\leq$ `Node.val` $\leq$ 100
-   Both `list1` and `list2` are sorted in non-decreasing order.

{{< /admonition >}}

## 想法

ListNode 的新增在 [這邊](https://eyzim.github.io/posts/leetcode/leetcode_24_swap_nodes_in_pairs/#%E8%A7%A3%E6%B3%95)，如果非必要，我還是比較喜歡用指標，光是不用一直切換 `node.next` 還是 `node->next` 就省很多力氣了 😊

概念類似於 [Merge Sorted Array](https://eyzim.github.io/posts/leetcode/leetcode_88_merge_sorted_array/)，但是 Linked List 不能偷吃步從最末端開始 sort。

從兩個 Linked List 最前端一一比較、放入新的 Linked List `newList`。

最後別忘記把剩下不需要比較的 nodes 放進排序好的 `newList` 啊。

## 解法

```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

    // 任一 list 為空
    if(!list1)  return list2;
    if(!list2)  return list1;

    // 新增一個 ListNode 並以指標指向它 (方便待會回傳)
    ListNode *newList = new ListNode(0);

    // 各種指向工作位置的指標
    ListNode *l1 = list1, *l2 = list2, *n = newList;

    // l1 和 l2 還需要比較的話...
    while(l1 && l2)
    {
        if(l1->val > l2->val)
        {
            n->next = l2;
            l2 = l2->next;
        }
        else
        {
            n->next = l1;
            l1 = l1->next;
        }

        n = n->next;
    }

    // 剩下 l2 直接貼上
    if(l2)
    {
        n->next = l2;
    }

    // 剩下 l1 直接貼上
    if(l1)
    {
        n->next = l1;
    }

    return newList->next;
}
```

### 解法二

-   Time complexity: $\mathcal{O}(n)$.
-   Space complexity: $\mathcal{O}(n)$.

