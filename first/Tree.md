#Tree
##94. Binary Tree Inorder Traversal
###题目：
Given a binary tree, return the inorder traversal of its nodes' values.

For example:
Given binary tree [1,null,2,3],

```
   1
    \
     2
    /
   3
```
return [1,3,2].

Note: Recursive solution is trivial, could you do it iteratively?
###思路：

###代码：

```
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> all;
    if(!root) return res;
    TreeNode* cur = root;
    while(cur || !all.empty()) {
        if(cur) {
            all.push(cur);
            cur = cur->left;
        }
        else {
            TreeNode* tmp = all.top();
            res.push_back(tmp->val);
            all.pop();
            cur = tmp->right;
        }
    }
    return res;
}
```

##95. Unique Binary Search Trees II
###题目：
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.

For example,

Given n = 3, your program should return all 5 unique BST's shown below.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
###思路：
问题：TreeNode* root = new TreeNode(i);//不可以调换位置到循环外面
###代码：
```
vector<TreeNode*> generateT(int low, int up) {
    vector<TreeNode*> res;
    if(up < low) {
        res.push_back(NULL);
        return res;
    }
    for(int i = low; i <= up; ++i) {
        vector<TreeNode*> left = generateT(low, i-1);
        vector<TreeNode*> right = generateT(i+1, up);
        
        for(auto j : left) {
            for(auto k : right) {
                TreeNode* root = new TreeNode(i);//不可以调换位置到循环外面
                root->left = j;
                root->right = k;
                res.push_back(root);
            }
        }
    }
    return res;
}


vector<TreeNode*> generateTrees(int n) {
    vector<TreeNode*> res;
    if(n != 0) {
        res = generateT(1, n);
    }
    return res;
}

```

##96. Unique Binary Search Trees
###题目：
Given n, how many structurally unique BST's (binary search trees) that store values 1...n?

For example,

Given n = 3, there are a total of 5 unique BST's.

```
   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
###思路：
f[i]记录节点个数为i的BST树的个数。

节点个数为i，则可认为是 n种左子树 * m种右子树的结果。

###代码：
```
int numTrees(int n) {
    int f[n+1] = {0};
    f[0] = f[1] = 1;
    for(int i = 2; i <= n; ++i) {
        for(int j = 0; j < i; ++j) {
            f[i] += f[j] * f[i-1-j];
        }
    }
    return f[n];
}
```

##98. Validate Binary Search Tree
###题目：
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

* The left subtree of a node contains only nodes with keys less than the node's key.
* The right subtree of a node contains only nodes with keys greater than the node's key.
* Both the left and right subtrees must also be binary search trees.
**Example 1:**
```
    2
   / \
  1   3
```
Binary tree [2,1,3], return true.

**Example 2:**

```
    1
   / \
  2   3
```
Binary tree [1,2,3], return false.
###思路：
传入左右边界的判断，注意左右边界不要用节点的值，值有可能越界，初始值设起来比较麻烦，传节点。
###代码：

```
bool isValid(TreeNode* root, TreeNode* l, TreeNode* r) {
    if(!root) return true;
    else if(l != NULL && root->val <= l->val || r != NULL && root->val >= r->val) return false;
    else {
        return isValid(root->left, l, root) && isValid(root->right, root, r);
    }
}

bool isValidBST(TreeNode* root) {
     if(!root) return true;
     else {
         TreeNode* l = NULL, *r = NULL;
         return isValid(root->left, l, root) && isValid(root->right, root, r);
     }
}
```

##100. Same Tree
###题目：
Given two binary trees, write a function to check if they are equal or not.

Two binary trees are considered equal if they are structurally identical and the nodes have the same value.
###思路：

###代码：

```
bool isSameTree(TreeNode* p, TreeNode* q) {
    if(p && q) {
        if(p->val != q->val) return false;
        else {
            return isSameTree(p->left, q->left) && isSameTree(p->right, q->right);
        }
    }else if(p && !q || !p && q) return false;
    else return true;
}
```

##101. Symmetric Tree
###题目：
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following [1,2,2,null,3,null,3] is not:

```
    1
   / \
  2   2
   \   \
   3    3
```
**Note:**

Bonus points if you could solve it both recursively and iteratively.
###思路：

###代码：

```
bool isSymmetric(TreeNode* root) {
    if(!root) return true;
    else return yesOrNo(root->left, root->right);
}

bool yesOrNo(TreeNode* l, TreeNode* r) {
    if(!l && !r) {
        return true;
    }else if(!l || !r) {
        return false;
    }else if(l->val != r->val) {
        return false;
    }else {
        return yesOrNo(l->left, r->right) && yesOrNo(l->right, r->left);
    }
}
```

##102. Binary Tree Level Order Traversal
###题目：
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:

```
[
  [3],
  [9,20],
  [15,7]
]
```
###思路：
方法1、递归方法：

可以先计算出树的高度，然后再bfs过程中直接将节点的值push到相应高度的结果vector中。

方法2、非递归方法，使用queue：

关键点在于标记每一层的结束，这里使用结束之后加入一个NULL到queue中，当处理到NULL的时候，将vector放入结果，并且resize(0)，最后如果queue的大小不是0，说明下一层已经有内容，并且由于上一层已经结束，下一层也不会再加入，所以在queue中加入标记下一层结束的NULL。

###代码：

```
//方法1：
int height(TreeNode* root) {
    if(root == NULL) return 0;
    else {
        return max(height(root->left), height(root->right)) + 1;
    }
}

void bfs(TreeNode* root, vector<vector<int>> &res, int height) {
    if(root == NULL) return;
    res[height].push_back(root->val);
    bfs(root->left, res, height+1);
    bfs(root->right, res, height+1);
}


vector<vector<int>> levelOrder(TreeNode* root) {
    
    int h = height(root);
    vector<vector<int>> res(h);
    if(h != 0) bfs(root, res, 0);
    return res;
}

//方法2：
vector<vector<int> > levelOrder(TreeNode *root) {
    vector<vector<int> >  result;
    if (!root) return result;
    queue<TreeNode*> q;
    q.push(root);
    q.push(NULL);
    vector<int> cur_vec;
    while(!q.empty()) {
        TreeNode* t = q.front();
        q.pop();
        if (t==NULL) {
            result.push_back(cur_vec);
            cur_vec.resize(0);
            if (q.size() > 0) {
                q.push(NULL);
            }
        } else {
            cur_vec.push_back(t->val);
            if (t->left) q.push(t->left);
            if (t->right) q.push(t->right);
        }
    }
    return result;
}
```
##103. Binary Tree Zigzag Level Order Traversal
###题目：
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:

Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:

```
[
  [3],
  [20,9],
  [15,7]
]
```
###思路：
方法1、同上递归法，得到上面的答案后加一步将从上向下数的偶数行vector翻转。

方法2、同上非递归，也需要翻转。
###代码：

```
//方法1：
int height(TreeNode* root) {
    if(root == NULL) return 0;
    else {
        return max(height(root->left), height(root->right)) + 1;
    }
}

void bfs(TreeNode* root, vector<vector<int>> &res, int height) {
    if(root == NULL) return;
    res[height].push_back(root->val);
    bfs(root->left, res, height+1);
    bfs(root->right, res, height+1);
}

vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    int h = height(root);
    vector<vector<int>> res(h);
    if(h != 0) {
        bfs(root, res, 0);
        for(int i = 1; i < h; i+=2) {
            vector<int>* v = &res[i];
            reverse(v->begin(), v->end());
        }
    }
    return res;
}

//方法2：
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> res;
    if(!root) return res;
    queue<TreeNode*> q;
    vector<int> tmp;
    int num = 1, level = 1;
    q.push(root);
    while(!q.empty()) {
        TreeNode* now = q.front();
        tmp.push_back(now->val);
        q.pop();
        num--;
        if(now->left != NULL) q.push(now->left);
        if(now->right != NULL) q.push(now->right);
        if(num <= 0) {
            if(level % 2 == 0) 
                reverse(tmp.begin(), tmp.end());
            res.push_back(tmp);
            num = q.size();
            level++;
            tmp.clear();
        }
    }
    return res;
}	
```

##104. Maximum Depth of Binary Tree
###题目：
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
###思路：

###代码：

```
int maxDepth(TreeNode* root) {
    if(root == NULL) return 0;
    else {
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
}
```


##105. Construct Binary Tree from Preorder and Inorder Traversal
###题目：
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:

You may assume that duplicates do not exist in the tree.
###思路：
递归方法
###代码：

```
TreeNode* build(vector<int>& preorder, vector<int>& inorder, int preL, int preR, int inL, int inR) {
    if(preL > preR || inL > inR) return NULL;
    TreeNode* root = new TreeNode(preorder[preL]);
    int i;
    for(i = inL; i <= inR; ++i) {
        if(inorder[i] == preorder[preL]) break;
    }
    TreeNode* left = build(preorder, inorder, preL+1, preL + (i-inL), inL, i-1);
    TreeNode* right = build(preorder, inorder, preL + (i-inL) + 1, preR, i+1, inR);
    root->left = left;
    root->right = right;
    return root;
}

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    int len = preorder.size();
    return build(preorder, inorder, 0, len-1, 0, len-1);
}
```

##106. Construct Binary Tree from Inorder and Postorder Traversal
###题目：
Given inorder and postorder traversal of a tree, construct the binary tree.

Note:

You may assume that duplicates do not exist in the tree.
###思路：
递归方法，代码基本同上。
###代码：

```
TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
    int len = inorder.size();
    return build(postorder, inorder, 0, len-1, 0, len-1);
}

TreeNode* build(vector<int>& postorder, vector<int>& inorder, int postL, int postR, int inL, int inR) {
    if(postL > postR || inL > inR) return NULL;
    TreeNode* root = new TreeNode(postorder[postR]);
    int i;
    for(i = inL; i <= inR; ++i) {
        if(inorder[i] == postorder[postR]) break;
    }
    TreeNode* left = build(postorder, inorder, postL, postL + (i-inL) -1, inL, i-1);
    TreeNode* right = build(postorder, inorder, postL + (i-inL), postR-1, i+1, inR);
    root->left = left;
    root->right = right;
    return root;
}
```

##107. Binary Tree Level Order Traversal II
###题目：
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree [3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:

```
[
  [15,7],
  [9,20],
  [3]
]
```
###思路：
###代码：

```
int H(TreeNode* root) {
    if(!root) return 0;
    else {
        return max(H(root->left), H(root->right)) + 1;
    }
}

void level(TreeNode* root, vector<vector<int>> &res, int h, int height) {
    if(!root) return ;
    else {
        res[height - h - 1].push_back(root->val);
        level(root->left, res, h+1, height);
        level(root->right, res, h+1, height);
    }
}

vector<vector<int>> levelOrderBottom(TreeNode* root) {
    int height = H(root);
    vector<vector<int>> res(height);
    level(root, res, 0, height);
    return res;
}
```

##108. Convert Sorted Array to Binary Search Tree
###题目：
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.
###思路：

###代码：

```
TreeNode* arrayToBST(vector<int>& nums, int low, int up) {
    TreeNode* root = NULL;
    if(up < low) return root;
    int len = up - low + 1;
    int mid = low + len/2;
    root = new TreeNode(nums[mid]);
    TreeNode* left = arrayToBST(nums, low, mid-1);
    root->left = left;
    TreeNode* right = arrayToBST(nums, mid+1, up);
    root->right = right;
    return root;
}

TreeNode* sortedArrayToBST(vector<int>& nums) {
    return arrayToBST(nums, 0, nums.size()-1);
}
```

##110. Balanced Binary Tree
要维护每个节点处左右子树的高度。递归实现。

```
int dfs(TreeNode* root) {
    if(!root) return 0;
    int l = dfs(root->left);
    int r = dfs(root->right);
    if(l < 0 || r < 0 || abs(l-r) > 1) {
        return -1;
    }else {
        return l < r ? r + 1 : l + 1;  
    }
    
}

bool isBalanced(TreeNode* root) {
    if(dfs(root) < 0) return false;
    else return true;
    }
```

##111. Minimum Depth of Binary Tree
###题目：
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
###思路：
重点在于不可以只是考虑最小高度，一定要结束在叶子节点，如果某一个分支没有节点了，那么就不向那个分支走。
###代码：

```
int minDepth(TreeNode* root) {
    if(!root) return 0;
    else {
        if(!root->left) return minDepth(root->right) + 1;
        else if(!root->right) return minDepth(root->left) + 1;
        else return min(minDepth(root->left), minDepth(root->right)) + 1;
    }
}
```
##112. Path Sum
###题目：
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

For example:
Given the below binary tree and sum = 22,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
###思路：

###代码：

```
bool hasPathSum(TreeNode* root, int sum) {
    if(!root) return false;
    else if(!root->left && !root->right && sum == root->val) return true;
    else {
        return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
    }
}
```

##113. Path Sum II
###题目：
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

For example:
Given the below binary tree and sum = 22,

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```
return

```
[
   [5,4,11,2],
   [5,8,4,5]
]
```
###思路：

###代码：

```
void P(TreeNode* root, int sum, vector<int> &path, vector<vector<int>> &res) {
    if(!root) return;
    else if(!root->left && !root->right && sum == root->val) {
        path.push_back(root->val);
        res.push_back(path);
        path.pop_back();
    }else {
        path.push_back(root->val);
        P(root->left, sum - root->val, path, res);
        P(root->right, sum - root->val, path, res);
        path.pop_back();
    }
}

vector<vector<int>> pathSum(TreeNode* root, int sum) {
    vector<vector<int>> res;
    vector<int> path;
    P(root, sum, path, res);
    return res;
}
```

##114. Flatten Binary Tree to Linked List
###题目：
Given a binary tree, flatten it to a linked list in-place.

For example,
Given

```
         1
        / \
       2   5
      / \   \
     3   4   6
```
The flattened tree should look like:

```
   1
    \
     2
      \
       3
        \
         4
          \
           5
            \
             6
```
###思路：
先找到root的左子树的最右，将其right节点指向root->right，然后将root->right = root->left。

注意：now->left要设为NULL
###代码：

```
void flatten(TreeNode* root) {
    TreeNode *now = root;
    while(now) {
        if(now->left) {
            TreeNode *pre = now->left;
            while(pre->right) {
                pre = pre->right;
            }
            pre->right = now->right;
            now->right = now->left;
            now->left = NULL; // 重要！！！
        }
        now = now->right;
    }
}
```


##116. Populating Next Right Pointers in Each Node
###题目：
Given a binary tree

```
    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

**Note:**

* You may only use constant extra space.
* You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

For example,

Given the following perfect binary tree,

```
         1
       /  \
      2    3
     / \  / \
    4  5  6  7
```
After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \  / \
    4->5->6->7 -> NULL
```
###思路：
方法1：非递归，使用queue。

方法类似bfs一层一层遍历。特殊处理开头和结尾的指针指向即可。

方法2：递归。

从上到下，从左到右的链接。

###代码：

```
//方法1：
void connect(TreeLinkNode *root) {
    queue<TreeLinkNode*> all;
    if(!root) return;
    int num = 0;
    all.push(root);
    TreeLinkNode* pre = NULL;
    while(!all.empty()) {
        num = all.size();
        while(num) {
            TreeLinkNode* tmp = all.front();
            all.pop();
            if(tmp->left) all.push(tmp->left);
            if(tmp->right) all.push(tmp->right);
            num--;
            if(pre != NULL) {
                pre->next = tmp;
            }
            pre = tmp;
            if(num == 0) {
                tmp->next = NULL;
                pre = NULL;
            }
        }
    }
}

//方法2：
void connect(TreeLinkNode *root) {
    connect(root,nullptr);
}

void connect(TreeLinkNode *root,TreeLinkNode *sibling) {
    if(root==nullptr)
        return;
    else
        root->next=sibling;
    
    connect(root->left,root->right);
    
    if(sibling!=nullptr)
        connect(root->right,sibling->left);
    else
        connect(root->right,nullptr);
}
```

##117. Populating Next Right Pointers in Each Node II

###题目：
Follow up for problem "Populating Next Right Pointers in Each Node".

What if the given tree could be any binary tree? Would your previous solution still work?

Note:

You may only use constant extra space.
For example,
Given the following binary tree,

```
         1
       /  \
      2    3
     / \    \
    4   5    7
```
After calling your function, the tree should look like:

```
         1 -> NULL
       /  \
      2 -> 3 -> NULL
     / \    \
    4-> 5 -> 7 -> NULL
```    
###思路：
方法1、

用上面的方法1可以直接解决。

**方法2：**

空间复杂度低，但是不好理解
###代码：

```
//方法1：
void connect(TreeLinkNode *root) {
    queue<TreeLinkNode*> all;
    if(!root) return;
    int num = 0;
    all.push(root);
    TreeLinkNode* pre = NULL;
    while(!all.empty()) {
        num = all.size();
        while(num) {
            TreeLinkNode* tmp = all.front();
            all.pop();
            if(tmp->left) all.push(tmp->left);
            if(tmp->right) all.push(tmp->right);
            num--;
            if(pre != NULL) {
                pre->next = tmp;
            }
            pre = tmp;
            if(num == 0) {
                tmp->next = NULL;
                pre = NULL;
            }
        }
    }
}

//方法2：
public void connect(TreeLinkNode root) {

    TreeLinkNode head = null; //head of the next level
    TreeLinkNode prev = null; //the leading node on the next level
    TreeLinkNode cur = root;  //current node of current level

    while (cur != null) {
        
        while (cur != null) { //iterate on the current level
            //left child
            if (cur.left != null) {
                if (prev != null) {
                    prev.next = cur.left;
                } else {
                    head = cur.left;
                }
                prev = cur.left;
            }
            //right child
            if (cur.right != null) {
                if (prev != null) {
                    prev.next = cur.right;
                } else {
                    head = cur.right;
                }
                prev = cur.right;
            }
            //move to next node
            cur = cur.next;
        }
        
        //move to next level
        cur = head;
        head = null;
        prev = null;
    }
    
}
```

##129. Sum Root to Leaf Numbers
###题目：
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.

An example is the root-to-leaf path 1->2->3 which represents the number 123.

Find the total sum of all root-to-leaf numbers.

For example,

```
    1
   / \
  2   3
```
The root-to-leaf path 1->2 represents the number 12.

The root-to-leaf path 1->3 represents the number 13.

Return the sum = 12 + 13 = 25.
###思路：
dfs， 注意向下一层传参数的时候。
###代码：

```
void dfs(TreeNode* root, int &res, int now) {
    if(!root) return;
    if(!root->left && !root->right) {
        now = 10*now + root->val;
        res += now;
    }
    else {
        dfs(root->left, res, 10* now + root->val);
        dfs(root->right, res, 10 * now + root->val);
    }
}

int sumNumbers(TreeNode* root) {
    int res = 0, now = 0;
    dfs(root, res, now);
    return res;
}
```

##144. Binary Tree Preorder Traversal
###题目：
Given a binary tree, return the preorder traversal of its nodes' values.

For example:

Given binary tree {1,#,2,3},

```
   1
    \
     2
    /
   3
```
return [1,2,3].

**Note:** Recursive solution is trivial, could you do it iteratively?
###思路：

###代码：

```
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> all;
    if(!root) return res;
    TreeNode* cur = root;
    while(cur || !all.empty()) {
        if(cur) {
            all.push(cur);
            res.push_back(cur->val);
            cur = cur->left;
        }
        else {
            TreeNode* tmp = all.top();
            all.pop();
            cur = tmp->right;
        }
    }
    return res;
}
```

##173. Binary Search Tree Iterator
###题目：
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling next() will return the next smallest number in the BST.

**Note:** next() and hasNext() should run in average O(1) time and uses O(h) memory, where h is the height of the tree.

###思路：
需要一个stack和一个pushAllLeft函数，从根节点开始，只要有左节点即push入栈。

酱紫构造函数就是调用这个函数。

next()函数：首先当前的next就是栈顶，然后弹出栈顶，以栈顶为参数调用pushAllLeft。
###代码：

```
class BSTIterator {
private:
    stack<TreeNode*> all;

public:
    BSTIterator(TreeNode *root) {
        while(root) {
            all.push(root);
            root = root->left;
        }
    }

    /** @return whether we have a next smallest number */
    bool hasNext() {
        return !all.empty();
    }

    /** @return the next smallest number */
    int next() {
        TreeNode* tmp = all.top();
        all.pop();
        if(tmp->right) BSTIterator(tmp->right);
        return tmp->val;
    }
};

/**
 * Your BSTIterator will be called like this:
 * BSTIterator i = BSTIterator(root);
 * while (i.hasNext()) cout << i.next();
 */
```

##199. Binary Tree Right Side View
###题目：
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

For example:
Given the following binary tree,
   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
You should return [1, 3, 4].
###思路：
方法1：

用queue，bfs一层一层遍历整个树，输出每一层的最后一个。

方法2：

递归传递层数。
###代码：

```
//方法1：
vector<int> rightSideView(TreeNode* root) {
    queue<TreeNode*> all;
    vector<int> res;
    if(!root) return res;
    TreeNode* cur = root;
    all.push(cur);
    int num = 0;
    while(!all.empty()) {
        num = all.size();
        while(num) {
            TreeNode *now = all.front();
            all.pop();
            if(now->left) {
                all.push(now->left);
            }
            if(now->right) {
                all.push(now->right);
            }
            num--;
            if(num == 0) res.push_back(now->val);
        }
    }
    return res;
} 

//方法2：
void dfs(TreeNode* root, int lv, vector<int> &res){
    if(!root)   return;
    if(lv >= res.size()) res.push_back(root->val);
    dfs(root->right, lv+1, res);
    dfs(root->left, lv+1, res);
}

vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    dfs(root, 0, res);
    return res;
}

```

##222. Count Complete Tree Nodes
###题目：
Given a complete binary tree, count the number of nodes.

Definition of a complete binary tree from Wikipedia:

In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2^h nodes inclusive at the last level h.

###思路：
方法1：递归。

从根节点，一直向左走得到一直向左的高度，同理得到一直向右的高度。

如果两个相等，那么是满的树，用2^high-1计算节点数。

如果不相等，那么1+左子树递归调用原函数+右子树递归调用原函数。

方法2：利用二分思想。

一直查找左子树的最右节点。

如果存在，说明左子树是满树，那么root = root->right；不存在，root = root->left。

###代码：

```
//方法1：
int countNodes(TreeNode* root) {
    if(!root) return 0;
    int hl = 0, hr = 0;
    TreeNode* left = root;
    TreeNode* right = root;
    int res = 0;
    while(left) {
        hl++;
        left = left->left;
    }
    while(right) {
        hr++;
        right = right->right;
    }
    if(hl == hr) return pow(2, hl) - 1;
    else return 1 + countNodes(root->left) + countNodes(root->right);
    
}

//方法2：
int countNodes(TreeNode* root) {
    if(!root) return 0;
    TreeNode *temp = root;
    int height = 0, count = 0, level;
    while(temp) {
        temp = temp->left;
        height ++;
    }
    temp = root;
    level = height - 2;
    while(level >= 0) {
        TreeNode *left = temp->left;
        for(int i = 0;i < level;i ++) {
            left = left->right;
        }
        if(left) {
            temp = temp->right;
            count += (1 << level);
        } else temp = temp->left;
        level --;
    }
    if(temp) count ++;
    return (1 << (height - 1)) + count - 1;
}
```

##226. Invert Binary Tree
###题目：
Invert a binary tree.

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```
to

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
###思路：

###代码：

```
TreeNode* invertTree(TreeNode* root) {
    if(!root) return root;
    else {
        TreeNode* tmp = root->left;
        root->left = root->right;
        root->right = tmp;
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
}
```

##230. Kth Smallest Element in a BST
###题目：
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.

**Note:**

You may assume k is always valid, 1 ≤ k ≤ BST's total elements.

**Follow up:**

* What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?

**Hint:**

* Try to utilize the property of a BST.
* What if you could modify the BST node's structure?
* The optimal runtime complexity is O(height of BST).
###思路：
方法1、修改BST节点的数据结构，加入以这个节点为根的子树的节点个数。

方法2、递归，按照先序遍历。遍历到root的时候k--，到k == 0时输出当时的root->val

###代码：
```
//方法2：
void dfs(TreeNode* root, int &cnt, int &num) {
    if(!root) return;
    dfs(root->left, cnt, num);
    cnt--;
    if(cnt == 0) {
        num = root->val;
        return;
    }
    dfs(root->right, cnt, num);
}

int kthSmallest(TreeNode* root, int k) {
    int cnt = k, num = 0;
    dfs(root, cnt, num);
    return num;
}

```

##235	Lowest Common Ancestor of a Binary Search Tree
###题目:
Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

```
        _______6______
       /              \
    ___2__          ___8__
   /      \        /      \
   0      _4       7       9
         /  \
         3   5
```
For example, the lowest common ancestor (LCA) of nodes 2 and 8 is 6. Another example is LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
###思路：
利用BST的性质，初始化结果等于根节点，如果两个节点的值都大于根节点的值，向右找，小于向左找。
###代码：

```
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    TreeNode* res = root;
    if(p->val < root->val && q->val < root->val) {
        res = lowestCommonAncestor(root->left, p, q);
    }else if(p->val > root->val && q->val > root->val){
        res = lowestCommonAncestor(root->right, p, q);
    }
    return res;
}
```

##236. Lowest Common Ancestor of a Binary Tree **
###题目：
Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the definition of LCA on Wikipedia: “The lowest common ancestor is defined between two nodes v and w as the lowest node in T that has both v and w as descendants (where we allow a node to be a descendant of itself).”

```
        _______3______
       /              \
    ___5__          ___1__
   /      \        /      \
   6      _2       0       8
         /  \
         7   4
```
For example, the lowest common ancestor (LCA) of nodes 5 and 1 is 3. Another example is LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

###思路：
注意最后的判断。
###代码：

```
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(!root || root == p || root == q) return root;
    else {
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(left && right) return root;
        else if(!left && right) return right;
        else return left;
    }
}
```


##257. Binary Tree Paths
###题目：
Given a binary tree, return all root-to-leaf paths.

For example, given the following binary tree:

```
   1
 /   \
2     3
 \
  5
```
All root-to-leaf paths are:

```
["1->2->5", "1->3"]
```
###思路：
###代码：

```
void dfs(TreeNode* root, vector<string> &R, string path) {
    if(!root) return;
    else if(!root->left && !root->right) {
        if(path != "") path += "->";
        path += to_string(root->val);
        R.push_back(path);
    }else {
        if(path != "") path += "->";
        path += to_string(root->val);
        dfs(root->left, R, path);
        dfs(root->right, R, path);
    }
}


vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> res;
    string path;
    dfs(root, res, path);
    return res;
}
```

##337. House Robber III
###题目：
The thief has found himself a new place for his thievery again. There is only one entrance to this area, called the "root." Besides the root, each house has one and only one parent house. After a tour, the smart thief realized that "all houses in this place forms a binary tree". It will automatically contact the police if two directly-linked houses were broken into on the same night.

Determine the maximum amount of money the thief can rob tonight without alerting the police.

**Example 1:**

```
     3
    / \
   2   3
    \   \ 
     3   1
```
Maximum amount of money the thief can rob = 3 + 3 + 1 = 7.

**Example 2:**

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
```
Maximum amount of money the thief can rob = 4 + 5 = 9.
###思路：
参考这里的解释：
[discuss](https://discuss.leetcode.com/topic/39834/step-by-step-tackling-of-the-problem)

###代码：

```
vector<int> R(TreeNode* root) {
    vector<int> res(2,0);
    if(root == NULL) return res;
    vector<int> left = R(root->left);
    vector<int> right = R(root->right);
    res[0] = left[1] + right[1] + root->val;
    res[1] = max(left[0], left[1]) + max(right[0], right[1]);
    return res;
}
int rob(TreeNode* root) {
    vector<int> res = R(root);
    return max(res[0], res[1]);
}
```

##404. Sum of Left Leaves
###题目：
Find the sum of all left leaves in a given binary tree.

Example:

```
    3
   / \
  9  20
    /  \
   15   7
   
There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```
###思路：
使用一个变量来记录走入的是否是左子树。
###代码：

```
void sumLeft(TreeNode* root, bool leftOrNot, int &res) {
    if(!root) res += 0;
    else if(!root->left && !root->right && leftOrNot) {
        res += root->val;
    }else {
        sumLeft(root->left, true, res);
        sumLeft(root->right, false, res);
    }
}
int sumOfLeftLeaves(TreeNode* root) {
    int res = 0;
    if(!root || (!root->left && !root->right)) return res;
    else {
        sumLeft(root->left, true, res);
        sumLeft(root->right, false, res);
    }
    return res;
}
```

##437. Path Sum III *
###题目：
You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

**Example:**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```
###思路：
递归算法
###代码：

```
int pathS(TreeNode* root, int &sum, int pre) {
    if(!root) return 0;
    int now = pre + root->val;
    return (now == sum) + pathS(root->left, sum, now) + pathS(root->right, sum, now);
}

int pathSum(TreeNode* root, int sum) {
    if(!root) return 0;
    return pathS(root, sum, 0) + pathSum(root->left, sum) + pathSum(root->right, sum);
}
```