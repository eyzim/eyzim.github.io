# LeetCode 2. Add Two Numbers 解題紀錄



## [題目](https://leetcode.com/problems/add-two-numbers/)


{{< admonition type=quote title="Problem">}}

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

 

**Example 1:**

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
```
Explanation: 342 + 465 = 807.

**Example 2:**
```
Input: l1 = [0], l2 = [0]
Output: [0]
```
**Example 3:**
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

**Constraints:**

- The number of nodes in each linked list is in the range $[1, 100]$.
- 0 $\leq$ `Node.val` $\leq$ 9
- It is guaranteed that the list represents a number that does not have leading zeros.

{{< /admonition >}}


## 想法

{{< admonition type=info title="">}}

使用指標新增 
      ListNode * ans = new ListNode(0);
      ListNode * temp = ans;
最後只要
      return ans→next;

{{< /admonition >}}



## 解法

### 解法一

1. 就像 [66. Plus One]() 這題一樣，需要一個 carry 記住進位。
    
    1) 如果 `l1` 或 `l2` 有人的 node 為 NULL 時要特別設計算值為 0。
    
    2) 若 `l1` 或 `l2` 為 NULL，代表下一個點一定也是 NULL。

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
    return cal(l1, l2, 0);
}

ListNode* cal(ListNode* l1, ListNode* l2, int carry)
{
    if(!l1 && !l2 && !carry)    return NULL;  
        
    int sum = (l1? l1->val:0) + (l2? l2->val:0) + carry;
        
    ListNode* ans = new ListNode(sum%10);
        
    ans->next = cal(l1?(l1->next):NULL, l2?(l2->next):NULL, sum/10);
    
    return ans;
}
```

- Time complexity:  $\mathcal{O}(m+n)$.
- Space complexity:  $\mathcal{O}(1)$.

### 解法二

如果 `l1`、`l2`、`carry` 其中任一具有值，則繼續生成新點。

```cpp
ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
    ListNode * ans = new ListNode(0);
    ListNode * temp = ans;
        
    int carry = 0;
        
    while(l1 || l2 || carry)
    {
        carry += (l1 ? l1->val : 0) + (l2 ? l2->val : 0);

        temp->next = new ListNode(carry%10);

        temp = temp->next;
        carry /= 10;
            
        if(l1) l1 = l1->next;
        if(l2) l2 = l2->next;
    }
        
    return ans->next;
}
```

- Time complexity:  $\mathcal{O}(m+n)$.
- Space complexity:  $\mathcal{O}(1)$.


