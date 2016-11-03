#Linked List

##2. Add Two Numbers
另外建立一个链表，将两个需要进行加法的链表加和结果赋值给单独的这个链表。

或者直接加到一个的上面

##19. Remove Nth Node From End of List
可以直接fast移动n+1，然后slow和fast一起向后移动，fast指向NULL，slow是要删除的节点的pre，然后pre的next变成他的next->next


##23. Merge k Sorted Lists
###题目：
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

###思路：
方法1、

维护一个大小为k的优先队列，每次取出其中最小的，然后被取出的那个链表后面如果有的话就把后续加入。

###代码：
```
// Time Complexity: O(n * logk)
// Space Complexity: O(k)
struct Node {
    ListNode* N;
    Node(ListNode* n){
        N = n;
    }
    bool operator< (const Node a)const {
        return N->val > a.N->val;
    }
};
 
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<Node> Q;
        int len = lists.size();
        for(int i = 0; i < len; ++i) {
            if(lists[i] == NULL) continue;
            Q.push(Node(lists[i]));
        }
        
        ListNode* head = NULL, *pre = NULL;
        while(!Q.empty()) {
            Node now = Q.top();
            Q.pop();
            if(head == NULL) {
                head = now.N;
                pre = head;
            }
            else {
                pre->next = now.N;
                pre = pre->next;
            }
            if(now.N->next != NULL) {
                Q.push(Node(now.N->next));
            }
        }
        return head;
    }
};

```
##24. Swap Nodes in Pairs
需要向后看两个，分情况讨论。

后面的两个里面都有的，有一个的，和都没有的。

##61. Rotate List
先遍历到最后，得到长度；

然后取模，将最后一个和第一个连起来，向后走n-k步；

head指向now->next,now->next = NULL。

##82. Remove Duplicates from Sorted List II
```
ListNode* deleteDuplicates(ListNode* head) {
    if(!head || !head->next) return head;
    ListNode *Cur = head;
    ListNode *newPre = new ListNode(-1);
    ListNode *Pre = newPre;
    Pre->next = head;
    while(Cur) {
        while(Cur->next && Cur->next->val == Pre->next->val) {
            Cur = Cur->next;
        }
        if(Pre->next == Cur) {
            Pre = Pre->next;
        }else {
            Pre->next = Cur->next;
        }
        Cur = Cur->next;
    }
    return newPre->next;
}
```


##92. Reverse Linked List II
```
ListNode* reverseBetween(ListNode* head, int m, int n) {
    if(!head) return NULL;
    if(m == n) return head;
    
    ListNode *begin = head, *beginPre = NULL;
    for(int i = 2; i <= m; ++i) {
        beginPre = begin;
        begin = begin->next;
    }
    
    ListNode *Pre = begin;
    ListNode *Now = begin->next;
    for(int i = 0; i < (n - m); ++i) {
        ListNode *Next = Now->next;
        Now->next = Pre;
        Pre = Now;
        Now = Next;
    }
    if(beginPre == NULL) {
        head = Pre;
    }else {
        beginPre->next = Pre;
    }
    begin->next = Now;
    return head;
}
```

##109. Convert Sorted List to Binary Search Tree
```
TreeNode* listToBST(ListNode* &head, int low, int up) {
    if(up < low) return NULL;
    int mid = low + (up-low) / 2;
    TreeNode* left = listToBST(head, low, mid-1);
    TreeNode* root = new TreeNode(head->val);
    root->left = left;
    head = head->next;
    TreeNode* right = listToBST(head, mid+1, up);
    root->right = right;
    return root;
}
TreeNode* sortedListToBST(ListNode* head) {
    ListNode* tmp = head;
    int len = 0;
    while(tmp) {
        len++;
        tmp = tmp->next;
    }
    return listToBST(head, 0, len-1);
}
```
##143. Reorder List
做法：

先把后半部分翻转，然后将两个合并。

技巧要保证前半部分永远比后半部分多一个或者相等。while(fast && fast->next)

##148. Sort List
链表的归并排序

先将链表用快慢指针的方法分成两个部分，然后将两个部分做有序链表的merge，用递归实现。

```
class Solution {
public:
    ListNode* mergeSort(ListNode* l1, ListNode* l2) {
        ListNode *vRoot = new ListNode(-1), *cur = vRoot;
        while(l1 && l2) {
            if(l1->val < l2->val) {
                cur->next = l1;
                l1 = l1->next;
            }else {
                cur->next = l2;
                l2 = l2->next;
            }
            cur = cur->next;
        }
        while(l1) {
            cur->next = l1;
            l1 = l1->next;
            cur = cur->next;
        }
        while(l2) {
            cur->next = l2;
            l2 = l2->next;
            cur = cur->next;
        }
        cur->next = NULL;
        cur = vRoot->next;
        return cur;
    }
    
    ListNode* sortList(ListNode* head) {
        if(!head || !head->next) return head;
        
        int count = 0;
        ListNode *mid = head, *cur = head, *l1, *l2, *Re;
        
        while(cur) {
            ++count;
            cur = cur->next;
        }
        for(int i = 1; i < count / 2; ++i) {
            mid = mid->next;
        }
        cur = mid;
        mid = mid->next;
        cur->next = NULL;
        l1 = sortList(head);
        l2 = sortList(mid);
        Re = mergeSort(l1, l2);
        return Re;
    }
};
```

##160. Intersection of Two Linked Lists
很巧妙的一个方法：两个指针分别走，走到NULL之后从另一个链表的头开始走，两个指针走到相等地方返回。一定会走到相等的地方。相当于把这个两个接起来了。

##203. Remove Linked List Elements
要注意删除的元素在最后一个的情况。


##237. Delete Node in a Linked List
非常简短；

*node = *node->next

1、可以使用delete删除node后面的一个节点；

2、将后面的节点值赋到前面，删除链表的最后一个节点，记得当处理到倒数第二的时候，将他的后继设为空。



