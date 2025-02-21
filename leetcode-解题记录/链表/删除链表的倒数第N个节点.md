# 删除链表的倒数第N个节点

[力扣题目链接](https://gitee.com/link?target=https%3A%2F%2Fleetcode.cn%2Fproblems%2Fremove-nth-node-from-end-of-list%2F)

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？

示例 1：

![19.删除链表的倒数第N个节点](https://code-thinking-1253855093.file.myqcloud.com/pics/20210510085957392.png)

输入：head = [1,2,3,4,5], n = 2 输出：[1,2,3,5]

示例 2：

输入：head = [1], n = 1 输出：[]

示例 3：

输入：head = [1,2], n = 1 输出：[1]



```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
typedef struct ListNode ListNode;
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    ListNode* dummy = malloc(sizeof(struct ListNode));
    dummy->val = 0;
    dummy->next = head;
    ListNode* slow=dummy;
    ListNode* fast=dummy->next;
    for (int i = 0; i < n; ++i) {
        fast = fast->next;
    }
    while (fast) {
        fast = fast->next;
        slow = slow->next;
    }
    ListNode* temp=slow->next;
    slow->next=slow->next->next;
    head = dummy->next;
    free(dummy);//删除虚拟节点dummy
    free(temp);
    return head;
}
```

曾经错误思路:

```c
typedef struct ListNode ListNode;
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    ListNode* dummy = malloc(sizeof(struct ListNode));
    dummy->val = 0;
    dummy->next = head;
    ListNode* slow=dummy;
    ListNode* fast=dummy->next;
    while(fast!=NULL){
        if(n--) fast=fast->next;
        slow=slow->next;
        fast=fast->next;
    }//无法判断slow->next->next是否存在
    ListNode* temp=slow->next;
    slow->next=slow->next->next;
    head = dummy->next;
    free(dummy);//删除虚拟节点dummy
    free(temp);
    return head;
}
```

第一段代码报空指针异常的原因在于循环逻辑中对快指针（`fast`）的移动处理不当，导致在特定情况下访问了空指针的`next`。具体分析如下：

1. **循环逻辑错误**：
   第一段代码的循环中，每次迭代会先尝试让`fast`移动（若`n > 0`），随后强制让`fast`再移动一次。这可能导致`fast`在移动后变为`NULL`，但循环仍会继续，此时再次执行`fast = fast->next`就会触发空指针异常。

   **示例**：当链表长度为5，`n = 5`时：

   - 初始`fast`指向头节点（第一个节点）。
   - 循环内`n--`使`n`递减，`fast`移动到第二个节点，随后`fast`再次移动到第三个节点。
   - 重复此过程，最终`fast`会变为`NULL`，但循环仍会执行`fast = fast->next`，导致崩溃。

2. **步数控制错误**：
   正确的逻辑是让快指针先移动`n`步，再同步移动快慢指针。但第一段代码未正确实现这一逻辑，导致快指针可能未移动足够的步数，或移动过多步数。

------

**第二段代码的正确性**：
第二段代码通过以下步骤避免了问题：

1. **快指针先移动`n`步**：
   使用`for`循环明确让快指针`fast`先移动`n`步。若链表长度不足`n`，`fast`会指向`NULL`，但后续逻辑仍能处理。
2. **同步移动直到快指针为`NULL`**：
   此时慢指针`slow`指向待删除节点的前驱节点。无论链表长度如何，均不会出现空指针访问，因为：
   - 若链表长度等于`n`，`fast`在`for`循环后为`NULL`，`slow`保持在虚拟头节点，直接删除头节点。
   - 其他情况下，`slow`和`fast`同步移动，确保操作安全。

改正(王道书)

```c
typedef struct ListNode ListNode;
struct ListNode* removeNthFromEnd(struct ListNode* head, int n) {
    ListNode* dummy = malloc(sizeof(struct ListNode));
    dummy->val = 0;
    dummy->next = head;
    ListNode* slow=dummy;
    ListNode* fast=dummy->next;
    //slow最终指向倒数第n+1的节点,fast指向链表末尾的NULL
    while(fast!=NULL){
        if(--n>=0) {
            fast=fast->next;
        }
        else{
            slow=slow->next;
            fast=fast->next;
        }
    }
    ListNode* temp=slow->next;
    if(slow->next) slow->next=slow->next->next;
    head = dummy->next;
    free(dummy);//删除虚拟节点dummy
    free(temp);
    return head;
}
```

