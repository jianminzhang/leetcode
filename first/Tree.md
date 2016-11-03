#Tree

##95. Unique Binary Search Trees II

问题：TreeNode* root = new TreeNode(i);//不可以调换位置到循环外面

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
f[i]记录节点个数为i的BST树的个数。

节点个数为i，则可认为是 n种左子树 * m种右子树的结果。

```
f[0] = f[1] = 1;
for(int i = 2; i <= n; ++i) {
	for(int k = 1; k <= i; ++k) {
   		f[i] += f[k-1] * f[i-k];
   }
}
```

##98. Validate Binary Search Tree
传入左右边界的判断，注意左右边界不要用节点的值，值有可能越界，初始值设起来比较麻烦，传节点。

##102. Binary Tree Level Order Traversal
方法1、递归方法：

可以先计算出树的高度，然后再bfs过程中直接将节点的值push到相应高度的结果vector中。

```
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
```

方法2、非递归方法，使用queue：

关键点在于标记每一层的结束，这里使用结束之后加入一个NULL到queue中，当处理到NULL的时候，将vector放入结果，并且resize(0)，最后如果queue的大小不是0，说明下一层已经有内容，并且由于上一层已经结束，下一层也不会再加入，所以在queue中加入标记下一层结束的NULL。

```
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
方法1、同上递归法，得到上面的答案后加一步将从上向下数的偶数行vector翻转。
方法2、同上非递归，也需要翻转。

##105. Construct Binary Tree from Preorder and Inorder Traversal
递归方法

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
递归方法，代码基本同上。

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
重点在于不可以只是考虑最小高度，一定要结束在叶子节点，如果某一个分支没有节点了，那么就不向那个分支走。

##114. Flatten Binary Tree to Linked List
先找到root的左子树的最右，将其right节点指向root->right，然后将root->right = root->left。

注意：now->left要设为NULL

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
方法1：非递归，使用queue。

方法类似bfs一层一层遍历。特殊处理开头和结尾的指针指向即可。

方法2：递归。

从上到下，从左到右的链接。

```
void connect(TreeLinkNode *root) {
    if(!root)
        return;
    while(root -> left)
    {
        TreeLinkNode *p = root;
        while(p)
        {
            p -> left -> next = p -> right;
            if(p -> next)
                p -> right -> next = p -> next -> left;
            p = p -> next;
        }
        root = root -> left;
    }
}
```
##117. Populating Next Right Pointers in Each Node II
方法1、

用上面的方法1可以直接解决。

**方法2：**
空间复杂度低，但是不好理解

```
//based on level order traversal
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
dfs， 注意向下一层传参数的时候。

```
dfs(root->left, res, 10 * now + root->val);
```

##173. Binary Search Tree Iterator
需要一个stack和一个pushAllLeft函数，从根节点开始，只要有左节点即push入栈。

酱紫构造函数就是调用这个函数。

next()函数：首先当前的next就是栈顶，然后弹出栈顶，以栈顶为参数调用pushAllLeft。

##199. Binary Tree Right Side View
方法1：

用queue，bfs一层一层遍历整个树，输出每一层的最后一个。

方法2：


```
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
方法1：递归。

从根节点，一直向左走得到一直向左的高度，同理得到一直向右的高度。

如果两个相等，那么是满的树，用2^high-1计算节点数。

如果不相等，那么1+左子树递归调用原函数+右子树递归调用原函数。

方法2：利用二分思想。

一直查找左子树的最右节点。

如果存在，说明左子树是满树，那么root = root->right；不存在，root = root->left。


##230. Kth Smallest Element in a BST
方法1、修改BST节点的数据结构，加入以这个节点为根的子树的节点个数。

方法2、递归，按照先序遍历。遍历到root的时候k--，到k == 0时输出当时的root->val

```
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
利用BST的性质，初始化结果等于根节点，如果两个节点的值都大于根节点的值，向右找，小于向左找。

##236. Lowest Common Ancestor of a Binary Tree **

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

##255. Verify Preorder Sequence in Binary Search Tree
思想：

如果是合法的前序遍历结果，那么对于每一个值，其后面一旦出现大于这个值得数值，那么就不可能再出现小的，出现则说明不合法。

直接做会超时。

需要维护一个某一时刻，允许出现的最小值。

```
bool verifyPreorder(vector<int>& preorder) {
    int len = preorder.size();
    stack<int> all;
    int low = INT_MIN;
    for(int i = 0; i < len; ++i) {
        if(preorder[i] < low) return false;
        
        if(all.empty() || preorder[i] < all.top()) {
            all.push(preorder[i]);
        }
        else {
            while(preorder[i] > all.top()) {
                low = all.top();
                all.pop();
                if(all.empty()) break;
            }
            all.push(preorder[i]);
        }
    }
    return true;
}
```
##337. House Robber III
参考下面的解释：

https://discuss.leetcode.com/topic/39834/step-by-step-tackling-of-the-problem

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
使用一个变量来记录走入的是否是左子树。

##437. Path Sum III *
递归算法

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