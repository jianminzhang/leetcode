#Dynamic Programming
##10. Regular Expression Matching
###题目：
```
Implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:

isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "a*") → true
isMatch("aa", ".*") → true
isMatch("ab", ".*") → true
isMatch("aab", "c*a*b") → true
```
###思路：
**状态：**

bool f[i][j]：代表s的0-i和p的0-j是否可以匹配

**转移：**

```
if(p[j-1] != '*') {
    f[x][j] = f[1^x][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.');
}
else {
    f[x][j] = f[x][j-2] || (f[1^x][j] && (s[i-1] == p[j-2] || p[j-2] == '.'));
}
```
**初始化：**

```
for(int i = 2; i <= lenp; i+=2) {
	if(p[i-1] == '*') f[0][i] = true;
  	else break;
}
```

1、注意想清楚状态和转移；

2、注意初始化的时候要用memset, 每隔一个判断是否等于*并且一旦不等于就要退出；

3、要注意下标；

4、滚动数组的转移一定要记得j从0开始，只转移f[x][0]即可。

###代码

```
bool isMatch(string s, string p) {
	int lens = s.size(), lenp = p.size();
	bool f[2][lenp+1];
	memset(f, false, sizeof(f));
   	f[0][0] = true;
        
   	for(int i = 2; i <= lenp; i+=2) {
   		if(p[i-1] == '*') f[0][i] = true;
      	else break;
   }
   
   int x = 0;
   for(int i = 1; i <= lens; ++i) {
        x = 1 ^ x;
        for(int j = 1; j <= lenp; ++j) {
            if(p[j-1] != '*') {
                f[x][j] = f[1^x][j-1] && (s[i-1] == p[j-1] || p[j-1] == '.');
            }
            else {
                f[x][j] = f[x][j-2] || (f[1^x][j] && (s[i-1] == p[j-2] || p[j-2] == '.'));
            }
        }
        f[1^x][0] = f[x][0];
    }
    
    return f[x][lenp];
}
```
##32. Longest Valid Parentheses **
###题目：
Given a string containing just the characters **'('** and **')'**, find the length of the longest valid (well-formed) parentheses substring.

For **"(()"**, the longest valid parentheses substring is **"()"**, which has length = 2.

Another example is **")()())"**, where the longest valid parentheses substring is "()()", which has length = 4.
###思路：
这里需要明确括号匹配会带来的性质：只有左括号多于右括号，才可能匹配。

这里用栈来存储左括号的下标。

用一个last存储有效匹配最左边的左边那个位置。初始值是-1。

如果遇到左括号，将其下标入栈，

如果遇到右括号，栈非空则弹出栈顶，然后如果栈非空用（i-栈顶）更新结果，否则用（i-last）更新结果。
###代码：

```
int longestValidParentheses(string s) {
    stack<int> left;
    int len = s.size();
    int last = -1;
    int sum = 0;
    for(int i = 0; i < len; ++i) {
        if(s[i] == '(') left.push(i);
        else {
            if(left.empty()) last = i;
            else {
                left.pop();
                if(left.empty()) sum = max(sum, i-last);
                else sum = max(sum, i-left.top());
            }
        }
    }
    return sum;
}
```

##44. Wildcard Matching
###题目：

```
Implement wildcard pattern matching with support for '?' and '*'.

'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```
###思路：
解法同上，只需要简单注意两个符号的不同转移公式。

###代码：

```

bool isMatch(string s, string p) {
    int lens = s.size(), lenp = p.size();
    bool f[2][lenp+1];
    memset(f, false, sizeof(f));
    f[0][0] = true;
    
    for(int i = 1; i <= lenp; ++i) {
        if(p[i-1] == '*') f[0][i] = true;
        else break;
    }
    
    int x = 0;
    for(int i = 1; i <= lens; ++i) {
        x = 1 ^ x;
        for(int j = 1; j <= lenp; ++j) {
            if(p[j-1] != '*') {
                f[x][j] = f[1^x][j-1] && (s[i-1] == p[j-1] || p[j-1] == '?');
            }
            else 
                f[x][j] = f[x][j-1] || f[1^x][j];
        } 
        f[1^x][0] = f[x][0];
    }
    return f[x][lenp];
}
```

##53. Maximum Subarray
###题目：
Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array **[-2,1,-3,4,-1,2,1,-5,4]**,

the contiguous subarray **[4,-1,2,1]** has the largest sum = **6**.
###思路：
res记录最后的答案，pre记录当前位置前连续的序列的加和。

如果pre已经为负数，那么pre从当前的位置重新开始，因为原先的加和为负数，那么会将后面所有的结果变小，不如出掉前面的影响。
###代码：

```
int maxSubArray(vector<int>& nums) {
    int len = nums.size();
    int res = nums[0], pre = nums[0];
    for(int i = 1; i < len; ++i) {
        res = max(res, pre);
        if(pre < 0) {
            pre = nums[i];
        }else {
            pre += nums[i];
        }
    }
    res = max(res, pre);
    return res;
}
```


##62. Unique Paths
###题目：
A robot is located at the top-left corner of a *m x n* grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

**Note:** *m* and *n* will be at most 100.
###思路：
注意memset只能将数组的每一个值设置成-1或者0，其他不可以！！

**1、 方法1：**

**状态：**

f[i][j]代表机器人从左上角走到(i,j)位置处有多少种方法。

**转移：**

由于机器人只能向右和下走，所以(i,j)只能由(i-1, j) 和(i, j-1)得到。

f[i][j] = f[i-1][j] + f[i][j-1]。

**初始化：**

f[0][i] = 1, f[i][0] = 1

**2、方法2：状态压缩**

f[i] = f[i] + f[i-1]

由于f[i]是上一行在这一列的结果相当于f[i-1][j]，f[i-1]相当于f[i][j-1]

###代码：

```
//方法1：
int uniquePaths(int m, int n) {
    if(m == 0) return 0;
    int f[m][n];
    memset(f, 0, sizeof(f));
    for(int i = 0; i < n; ++i) f[0][i] = 1;
    for(int i = 1; i < m; ++i) f[i][0] = 1;
    for(int i = 1; i < m; ++i) 
        for(int j = 1; j < n; ++j) 
            f[i][j] = f[i-1][j] + f[i][j-1];
    
    return f[m-1][n-1];
}

//方法2：
int uniquePaths(int m, int n) {
    vector<int> f(n, 1);
    for(int i = 1; i < m; ++i) 
        for(int j = 1; j < n; ++j) 
            f[j] = f[j] + f[j-1];
    return f[n-1];
}
```

##63. Unique Paths II
###题目：
Follow up for "Unique Paths":

Now consider if some obstacles are added to the grids. How many unique paths would there be?

An obstacle and empty space is marked as **1** and **0** respectively in the grid.

For example,
There is one obstacle in the middle of a 3x3 grid as illustrated below.

```
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
```
The total number of unique paths is 2.

Note: m and n will be at most 100.
###思路：
**状态：**

f[i][j]:代表从左上角到位置(i,j)有多少种不同的路径。

**转移：**

由于这里有障碍，如果是障碍，那么直接为0，否则从(i-1, j) 和(i, j-1)两个位置转移

f[i][j] = obstacleGrid[i][j] == 1 ? 0 : (f[i-1][j] + f[i][j-1])

**初始化：**

这里的初始化要注意：初始化第一行和第一列的时候，不仅是当前位置为障碍，他自己的路径数目变为0，他后面的的位置都是0，因为这两行只能由前一个转移得到，前一个为障碍，那么永远不可以到达后面的位置。

###代码：

```
int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
    int m = obstacleGrid.size();
    if(m == 0) return 0;
    int n = obstacleGrid[0].size();
    int f[m][n];
    memset(f, 0, sizeof(f));
    for(int i = 0; i < n; ++i) {
        if(obstacleGrid[0][i] == 1) break;
        f[0][i] = 1;
    }
    for(int i = 0; i < m; ++i) {
        if(obstacleGrid[i][0] == 1) break;
        f[i][0] = 1;
    }
    
    for(int i = 1; i < m; ++i) 
        for(int j = 1; j < n; ++j) 
            f[i][j] = obstacleGrid[i][j] == 1 ? 0 : (f[i-1][j] + f[i][j-1]);
    return f[m-1][n-1];
}
```

##64. Minimum Path Sum
###题目：
Given a *m x n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.
###思路：
**状态：**

f[i][j]:表示从左上角到位置(i, j)的最小路径和。

**转移：**

根据规则，从位置(i-1, j) 和 (i, j-1)转移。

f[i][j] = min(f[i-1][j], f[i][j-1]) + grid[i][j]。

**初始化：**

第一行和第一列没有别的选择，将整个路径的和加起来即可。

f[0][i] = grid[0][i] + f[0][i-1]

f[i][0] = grid[i][0] + f[i-1][0]

还是可以状态压缩


###代码：

```
int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size();
    if(m == 0) return 0;
    int n = grid[0].size();
    int f[m][n];
    f[0][0] = grid[0][0];
    for(int i = 1; i < n; ++i) f[0][i] = grid[0][i] + f[0][i-1];
    for(int i = 1; i < m; ++i) f[i][0] = grid[i][0] + f[i-1][0];
    
    for(int i = 1; i < m; ++i) 
        for(int j = 1; j < n; ++j) 
            f[i][j] = min(f[i-1][j], f[i][j-1]) + grid[i][j];
        
    return f[m-1][n-1];
}
```

##70. Climbing Stairs
###题目：
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
###思路：
很朴素的dp。
###代码：

```
int climbStairs(int n) {
    int f[n+1] = {0};
    f[0] = f[1] = 1;
    for(int i = 2; i <= n; ++i) {
        f[i] = f[i-1] + f[i-2];
    }
    return f[n];
}
```

##72. Edit Distance
###题目：
Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character

b) Delete a character

c) Replace a character
###思路：
**状态：**

f[i][j]：代表处理完word1的前i个，和word2的前j个字母之后，最小的编辑次数。

这里i,j的范围是0-len1 和0-len2，一般都要空出来0，酱紫可以满足i-1,j-1转移不会出现负数。

**转移：**

当word1[i-1] == word2[j-1]，f[i][j] = f[i-1][j-1]

否则需要进行编辑，编辑的条件有三个，分情况讨论：

如果是删除和增加，那么是相当于有一位不变，另一位减一的情况，增加一个操作：即f[i-1][j]和f[i][j-1]。

还有一个就是修改，f[i-1][j-1]。以上三种情况的最小值再加一。

**初始化：**

f[0][0]=0;
f[0][i] = i, f[i][0] = i。
###代码：

```
int minDistance(string word1, string word2) {
    int len1 = word1.size(), len2 = word2.size();
    int f[len1+1][len2+1];
    memset(f, INT_MAX, sizeof(f));
    f[0][0] = 0;
    for(int i = 1; i <= len1; ++i) f[i][0] = i;
    for(int i = 1; i <= len2; ++i) f[0][i] = i;
    
    for(int i = 1; i <= len1; ++i) 
        for(int j = 1; j <= len2; ++j) {
            if(word1[i-1] == word2[j-1]) f[i][j] = f[i-1][j-1];
            else {
                int t = min(f[i-1][j], f[i-1][j-1]);
                t = min(f[i][j-1], t);
                f[i][j] = t + 1;
            }
        }
    return f[len1][len2];
}
```

##85. Maximal Rectangle
###题目：
Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

For example, given the following matrix:

```
1 0  1  0  0
1 0 *1 *1 *1
1 1 *1 *1 *1
1 0  0  1  0
```
Return 6.
###思路：
思路继承自84.Largest Rectangle in Histogram

讲解见 [二维](https://discuss.leetcode.com/topic/1634/a-o-n-2-solution-based-on-largest-rectangle-in-histogram/2)

一行一行处理相当于固定了下边，处理每一行的时候都相当于处理了一个上面的问题。
###代码：

```
int maximalRectangle(vector<vector<char>>& matrix) {
    int m = matrix.size();
    if (m == 0) return 0;
    int n = matrix[0].size();
    int height[n+1];
    memset(height, 0, sizeof(height));
    int res = 0;
    for (int i = 0; i < m; ++i) {
        stack<int> mini;
        for (int j = 0; j <= n; ++j) {
            if (j < n) {
                if (matrix[i][j] == '1') height[j] += 1;
                else height[j] = 0;
            }
                
            if (mini.empty() || height[j] >= height[mini.top()])
                mini.push(j);
            else {
                while (!mini.empty() && height[j] < height[mini.top()]) {
                    int now = mini.top();
                    mini.pop();
                    res = max(res, height[now] * (mini.empty() ? j : (j - mini.top() - 1)));
                }
                mini.push(j);
            }
        }
    }
    return res;
}
```

##91. Decode Ways
###题目：
A message containing letters from **A-Z** is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message **"12"**, it could be decoded as **"AB"** (1 2) or **"L"** (12).

The number of ways decoding **"12"** is 2.
###思路：
动态规划思路。

**状态 f[i]：** 

代表处理到位置i的时候，最多的解码数目。

**转移：**

如果位置i和位置i-1可以组成一个合法的编码，那么f[i] += f[i-2]，否则 f[i] += 0;

如果位置i本身是合法编码，那么f[i] += f[i-1]，否则 f[i] += 0;

若f[i] == 0,返回0。


```
int x = 10 * (s[i - 1] - '0') + s[i] - '0';
dp[i] = (x <= 26 && x >= 10) ? dp[i - 2] : 0;
int y = s[i] - '0';
dp[i] += (y != 0 ? dp[i - 1] : 0);
if(dp[i] == 0) return 0;
```

**初始化：**

这里初始化不仅要初始化f[0]，还需要初始化f[1]，因为这里的转移要依赖前面的两项的结果。

###代码：

```
int numDecodings(string s) {
    int n = s.size();
    if (n == 0) return 0;
    if (s[0] == '0') return 0;
    if (n == 1) return 1;
    
    int dp[n + 1];
    dp[0] = 1;
    dp[1] = 0;
    if (10 * (s[0] - '0') + s[1] - '0' <= 26) dp[1]++;
    if (s[1] - '0' != 0) dp[1]++;
    
    for (int i = 2; i < n; ++i) {
        int x = 10 * (s[i - 1] - '0') + s[i] - '0';
        dp[i] = (x <= 26 && x >= 10) ? dp[i - 2] : 0;
        int y = s[i] - '0';
        dp[i] += (y != 0 ? dp[i - 1] : 0);
        if(dp[i] == 0) return 0;
    }
    
    return dp[n - 1];
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
//方法1：没有用到动态规划
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

//方法2：用动态规划
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* addTree(TreeNode* root, int k) {
        if(root == NULL) return NULL;
        TreeNode* nr = new TreeNode(root->val + k);
        nr->left = addTree(root->left, k);
        nr->right = addTree(root->right, k);
        return nr;
    }
    vector<TreeNode*> generateTrees(int n) {
        vector<vector<TreeNode*> > f(n+1);
        if(n == 0) return f[0];
        if(n == 1) return {new TreeNode(1)};
        f[0] = {new TreeNode(0)};
        f[1] = {new TreeNode(1)};
        for(int i = 2; i <= n; i++) {
            vector<TreeNode*> t;
            for(int j = 1; j <= i; j++) {
                for(int ii = 0; ii < f[j-1].size(); ii++) {
                    for(int jj = 0; jj < f[i-j].size(); jj++) {
                        TreeNode* root = new TreeNode(j);
                        if(j-1 == 0) {
                            root->left = NULL;
                        }
                        else{
                            root->left = f[j-1][ii];
                        }
                        if(i-j == 0) {
                            root->right = NULL;
                        }
                        else {
                            root->right = addTree(f[i-j][jj], root->val);
                        }
                        t.push_back(root);
                    }
                }
            }
            
            f[i] = t;
        }
        return f[n];
    }
};
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

##97. Interleaving String
###题目：
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.

For example,

Given:

*s1* = **"aabcc"**,

*s2* = **"dbbca"**,

When s3 = **"aadbbcbcac"**, return true.

When s3 = **"aadbbbaccc"**, return false.
###思路：
**状态：**

bool f[i][j]：代表用s1的前i个字母，和s2的前j个字母是否可以保证内部顺序不变的情况下得到s3。

**转移：**

这里要考虑当处理到(i,j)位时，即在决定s3的第（i+j）位由s1的第i位还是由s2的第j为决定。

判断是否可以由这两种情况决定的前提是，这两位之前的情况为真。

f[i][j] = (f[i-1][j] && s3[i+j-1] == s1[i-1]) 
                    || (f[i][j-1] && s3[i+j-1] == s2[j-1]) 

**初始化：**

初始化不仅要保证f[0][0] = true，而且要看单独使用一个串是否成功，即一旦出现不符合，那么后面全部不符合。

###代码：

```
bool isInterleave(string s1, string s2, string s3) {
    int len1 = s1.size(), len2 = s2.size(), len3 = s3.size();
    if(len1 + len2 != len3) return false;
    bool f[len1+1][len2+1];
    memset(f, false, sizeof(f));
    f[0][0] = true;
    
    for(int i = 1; i <= len2; ++i) {
        if(s3[i-1] == s2[i-1]) f[0][i] = true;
        else break;
    }
    
    for(int i = 1; i <= len1; ++i) {
        if(s3[i-1] == s1[i-1]) f[i][0] = true;
        else break;
    }
    
    for(int i = 1; i <= len1; ++i) 
        for(int j = 1; j <= len2; ++j) 
            f[i][j] = (f[i-1][j] && s3[i+j-1] == s1[i-1])
                    ||(f[i][j-1] && s3[i+j-1] == s2[j-1]) ;
        
    return f[len1][len2];
}
```

##115. Distinct Subsequences
###题目：
Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, **"ACE"** is a subsequence of **"ABCDE"** while **"AEC"** is not).

Here is an example:

**S = "rabbbit", T = "rabbit"**

Return **3**.
###思路：
**状态：**

int f[i][j]：代表s的前i个字母包含有几种t的前j个字母。

**转移：**

如果s的第i个和t的第j个字母相同，那么当前结果加上f[i-1][j-1]，否则等于f[i-1][j]。

f[i][j] = f[i-1][j] + (s[i-1] == t[j-1] ? f[i-1][j-1] : 0)。

**初始化：**

lent == 0, return 1

lens == 0, return 0

(lent == 0 && lens == 0, return 1)

f[i][0] = 1

###代码：

```
int numDistinct(string s, string t) {
    int lens = s.size(), lent = t.size();
    if(lent == 0) return 1;
    if(lens == 0) return 0;
    
    int f[lens+1][lent+1];
    memset(f, 0, sizeof(f));
    for(int i = 0; i <= lens; ++i) f[i][0] = 1;
    
    for(int i = 1; i <= lens; ++i) 
        for(int j = 1; j <= lent; ++j) 
            f[i][j] = f[i-1][j] + (s[i-1] == t[j-1] ? f[i-1][j-1] : 0);
        
    return f[lens][lent];
}
```


##120. Triangle
###题目：
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

For example, given the following triangle

```
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is **11** (i.e., **2 + 3 + 5 + 1 = 11**).

**Note:**

Bonus point if you are able to do this using only *O(n)* extra space, where n is the total number of rows in the triangle.
###思路：
时间复杂度：O(m^2) ,  空间复杂度：O(m^2)

**状态：**

int f[i][j]: 代表第i行的第j个位置为终点，从顶到这里的最小路径和。

**转移：**

由于题意的相邻，在这里观察可得，只和上一行同一位置和上一个位置有关。

f[i][j] = min(f[i-1][j], f[i-1][j-1]) + triangle[i][j]

**初始化：**

要初始化最左边一列和最右边一列。

压缩空间的方法见：[空间复杂度为O(m)](https://discuss.leetcode.com/topic/1669/dp-solution-for-triangle/2)。

###代码：

```
//最直接的时间O(m^2),空间O(m^2)
int minimumTotal(vector<vector<int>>& triangle) {
    int m = triangle.size();
    if(m == 0) return 0;
    int f[m][m];
    f[0][0] = triangle[0][0];
    for(int i = 1; i < m; ++i) f[i][0] = triangle[i][0] + f[i-1][0];
    for(int i = 1; i < m; ++i) f[i][i] = triangle[i][i] + f[i-1][i-1];
        
    for(int i = 2; i < m; ++i) 
        for(int j = 1; j < i; ++j) 
            f[i][j] = min(f[i-1][j], f[i-1][j-1]) + triangle[i][j];
            
    int res = INT_MAX;
    for(int i = 0; i < m; ++i) res = min(res, f[m-1][i]);
        
    return res;
}

```

##121. Best Time to Buy and Sell Stock
###题目：
Say you have an array for which the i^th element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.

**Example 1:**

```
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
```
**Example 2:**

```
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.
```
###思路：
这里由于只进行一次交易，则只要记录处理到当前位置，最小交易值即可。

若当前位置的值小于等于最小交易值，那么更新最小交易值；

否则用当前值减去最小交易值更新结果。
###代码：

```
int maxProfit(vector<int>& prices) {
    int len = prices.size(), res = 0;
    int m = INT_MAX;
    if(len == 0) return res;
    for(int i = 0; i < len; ++i) {
        if(prices[i] <= m) m = prices[i];
        else {
            res = max(prices[i]-m, res);
        }
    }
    return res;
}
```

##122. Best Time to Buy and Sell Stock II
###题目：
Say you have an array for which the i^th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
###思路：
这个题目不是动态规划的题。

这个题目是贪心的思想。

记录当前位置以前的最小值，如果当前值小于等于最小值，那么更新最小值；

如果当前值大于最小值，那么答案加上（当前值-最小值），最小值更新为当前值。

###代码：

```
int maxProfit(vector<int>& prices) {
    int res = 0, m = INT_MAX;
    int len = prices.size();
    for(int i = 0; i < len; ++i) {
        if(prices[i] <= m) m = prices[i];
        else {
            res += (prices[i] - m);
            m = prices[i];
        }
    }
    return res;
}
```

##123. Best Time to Buy and Sell Stock III**
###题目：
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most **two** transactions.

**Note:**

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
###思路：
这个方法可以拓展到k的情况，但是数据量不可以太大，因为要开二维数组。
###代码：

```
int maxProfit(vector<int>& prices) {
    int len = prices.size();
    if(len < 2) return 0;
    vector<vector<int>> dp(3, vector<int>(len, 0));
    for(int i = 1; i <= 2; ++i) {
        int tmp = dp[i-1][0] - prices[0];
        for(int j = 1; j < len; ++j) {
            dp[i][j] = max(dp[i][j-1], prices[j] + tmp);
            tmp = max(tmp, dp[i-1][j] - prices[j]);
        }
    }
    return dp[2][len-1];
}
```

##188. Best Time to Buy and Sell Stock IV **
###题目：
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete at most **k** transactions.

**Note:**

You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
###思路：
不会……

参考[讲解](https://discuss.leetcode.com/topic/9522/c-solution-with-o-n-klgn-time-using-max-heap-and-stack)
###代码：

```
int maxProfit(int k, vector<int>& prices) {
    int n = (int)prices.size(), ret = 0, v, p = 0;
    priority_queue<int> profits;
    stack<pair<int, int> > vp_pairs;
    while (p < n) {
        // find next valley/peak pair
        for (v = p; v < n - 1 && prices[v] >= prices[v+1]; v++);
        for (p = v + 1; p < n && prices[p] >= prices[p-1]; p++);
        // save profit of 1 transaction at last v/p pair, if current v is lower than last v
        while (!vp_pairs.empty() && prices[v] < prices[vp_pairs.top().first]) {
            profits.push(prices[vp_pairs.top().second-1] - prices[vp_pairs.top().first]);
            vp_pairs.pop();
        }
        // save profit difference between 1 transaction (last v and current p) and 2 transactions (last v/p + current v/p),
        // if current v is higher than last v and current p is higher than last p
        while (!vp_pairs.empty() && prices[p-1] >= prices[vp_pairs.top().second-1]) {
            profits.push(prices[vp_pairs.top().second-1] - prices[v]);
            v = vp_pairs.top().first;
            vp_pairs.pop();
        }
        vp_pairs.push(pair<int, int>(v, p));
    }
    // save profits of the rest v/p pairs
    while (!vp_pairs.empty()) {
        profits.push(prices[vp_pairs.top().second-1] - prices[vp_pairs.top().first]);
        vp_pairs.pop();
    }
    // sum up first k highest profits
    for (int i = 0; i < k && !profits.empty(); i++) {
        ret += profits.top();
        profits.pop();
    }
    return ret;
}
```

##309. Best Time to Buy and Sell Stock with Cooldown **
###题目：
Say you have an array for which the i^th element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times) with the following restrictions:

* You may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
* After you sell your stock, you cannot buy stock on next day. (ie, cooldown 1 day)

**Example:**

```
prices = [1, 2, 3, 0, 2]
maxProfit = 3
transactions = [buy, sell, cooldown, buy, sell]
```
###思路：
方法1：

**状态：**

s[i]代表第i天

**转移：**



**初始化：**



方法2：
 参考[解释](https://discuss.leetcode.com/topic/31015/very-easy-to-understand-one-pass-o-n-solution-with-no-extra-space)
###代码：

```
//方法1：
int maxProfit(vector<int>& p) {
    int len = p.size();
    if(len == 0) return 0;
    vector<int> b(len, 0);
    vector<int> s(len, 0);
    b[0] = 0 - p[0];
    s[0] = 0;
    int res = 0;
    for(int i = 1; i < len; i++) {
        s[i] = max(b[i-1]+p[i], s[i-1]-p[i-1]+p[i]);
        res = max(res, s[i]);
        if(i == 1) {
            b[i] = 0-p[i];
            continue;
        }
        b[i] = max(s[i-2]-p[i], b[i-1]+p[i-1]-p[i]);
    }
    return res;
}

//方法2：
int maxProfit(vector<int>& prices) {
	int L = prices.size();
	if(L < 2) return 0;

	int has1_doNothing = -prices[0];
	int has1_Sell = 0;
	int has0_doNothing = 0;
	int has0_Buy = -prices[0];
	for(int i=1; i<L; i++) {
		has1_doNothing = has1_doNothing > has0_Buy ? has1_doNothing : has0_Buy;
		has0_Buy = -prices[i] + has0_doNothing;
		has0_doNothing = has0_doNothing > has1_Sell ? has0_doNothing : has1_Sell;
		has1_Sell = prices[i] + has1_doNothing;
	}
    return has1_Sell > has0_doNothing ? has1_Sell : has0_doNothing;
}
```
##132. Palindrome Partitioning II *
###题目：
Given a string s, partition s such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of s.

For example, given s = **"aab"**,

Return **1** since the palindrome partitioning **["aa","b"]** could be produced using 1 cut.
###思路：
**状态：**

这里需要两个DP状态:

* bool yes[i][j]:代表s[i..j]是否是一个回文

* int f[i]：代表s[i..n-1]的最少切割次数，f[0] 是答案。

**转移：**

一旦 yes[i][j] == true:

* 如果 j == n-1， 那么 s[i..n-1] 是回文,f[i] = 0;

* 否则: 用1+f[j+1] 和 f[i]的最小值更新f[i].

**初始化：**

yes[i][j] = false;

f[i] = len - i - 1

参考[DP](https://discuss.leetcode.com/topic/2048/my-dp-solution-explanation-and-code)
###代码：

```
int minCut(string s) {
    int len = s.size();
    if (len == 0) return 0;
    vector<vector<bool>> yes(len, vector<bool>(len, false));
    vector<int> f(len);
    for (int i = len - 1; i >= 0; --i) {
        f[i] = len - i - 1;
        for (int j = i; j < len; ++j) {
            if (s[i] == s[j] && (j - i < 2 || yes[i+1][j-1])) {
                yes[i][j] = true;
                if (j == len - 1) f[i] = 0;
                else if (f[j+1] + 1 < f[i]) f[i] = f[j+1] + 1;
            }
        }
    }
    return f[0];
}
```


##152. Maximum Product Subarray *
###题目：
Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array **[2,3,-2,4]**,

the contiguous subarray **[2,3]** has the largest product = **6**.
###思路：
每一次计算都存储一个以当前位置为结尾的正的最大连续结果和负的连续结果。

```
pMax = max(pMax * nums[i], nums[i]);
nMax = min(nMax * nums[i], nums[i]);
```
结果用每个位置的正数最大结果更新。

这个题开始只想到要记录以每个位置为结尾的最大结果，但是没想到要记录正数最大和负数最大。
###代码：

```
int maxProduct(vector<int>& nums) {
    int len = nums.size();
    if(len == 1) return nums[0];
    int pMax = 0, nMax = 0;
    int res = 0;
    for(int i = 0; i < len; ++i) {
        if(nums[i] < 0) swap(pMax, nMax);
        pMax = max(pMax * nums[i], nums[i]);
        nMax = min(nMax * nums[i], nums[i]);
        res = max(res, pMax);
    }
    return res;
}
```

##174. Dungeon Game *
###题目：
The demons had captured the princess (**P**) and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of M x N rooms laid out in a 2D grid. Our valiant knight (**K**) was initially positioned in the top-left room and must fight his way through the dungeon to rescue the princess.

The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.

Some of the rooms are guarded by demons, so the knight loses health (negative integers) upon entering these rooms; other rooms are either empty (0's) or contain magic orbs that increase the knight's health (positive integers).

In order to reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.


**Write a function to determine the knight's minimum initial health so that he is able to rescue the princess.**

For example, given the dungeon below, the initial health of the knight must be at least 7 if he follows the optimal path **RIGHT-> RIGHT -> DOWN -> DOWN**.

```
-2 (K)	-3	   3
-5	   -10	   1
10	    30	  -5 (P)
```

**Notes:**

* The knight's health has no upper bound.
* Any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.

###思路：
**状态：**

int f[i][j]:代表(i,j)位置上骑士需要的最少的生命值。

从右下角向左上角求值，f[0][0]是答案。（这里不懂，为什么不从左上向右下）

**转移：**

```
int now = min(f[i+1][j], f[i][j+1]) - dungeon[i][j];
f[i][j] = (now <= 0 ? 1 : now);
```

**初始化：**

```
vector<vector<int>> f(m+1, vector<int>(n+1, INT_MAX));
f[m][n-1] = 1;
f[m-1][n] = 1;
```
这里多加一行，一列，会使代码写起来简单，减少边界条件。
###代码：

```
int calculateMinimumHP(vector<vector<int>>& dungeon) {
    int m = dungeon.size();
    int n = dungeon[0].size();
    
    vector<vector<int>> f(m+1, vector<int>(n+1, INT_MAX));
    
    f[m][n-1] = 1;
    f[m-1][n] = 1;
    for (int i = m-1; i >= 0; --i) 
        for (int j = n-1; j >= 0; --j) {
            int now = min(f[i+1][j], f[i][j+1]) - dungeon[i][j];
            f[i][j] = (now <= 0 ? 1 : now);
        }
    
    return f[0][0];
}
```


##198. House Robber
###题目：
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

###思路：
时间复杂度：O(n)，空间复杂度：O(1)

**状态：**

只需要f0[2], f1[2]，四个状态。分别表示正在处理的这一位和上一位的状态

f0表示这一位不偷窃，f1代表这一位偷窃

**转移：**

```
f1[x] = f0[x^1] + nums[i];
f0[x] = max(f1[x^1], f0[x^1]);
```
表示如果这一位偷窃，那么上一位一定不可以偷窃；如果这一位不偷窃，那么利益为上一位是否偷窃两种情况的最大值。

**初始化：**

f0[0] = 0, f1[0] = nums[0]

###代码：

```
int rob(vector<int>& nums) {
    int len = nums.size();
    if(len == 0) return 0;
    int x = 0;
    int f0[2], f1[2];
    f0[x] = 0;
    f1[x] = nums[0];
    
    for(int i = 1; i < len; ++i) {
        x = x^1;
        f1[x] = f0[x^1] + nums[i];
        f0[x] = max(f1[x^1], f0[x^1]);
    }
    return max(f1[x], f0[x]);
}
```

##213. House Robber II
###题目：
**Note:** This is an extension of House Robber.

After robbing those houses on that street, the thief has found himself a new place for his thievery so that he will not get too much attention. This time, all houses at this place are **arranged in a circle**. That means the first house is the neighbor of the last one. Meanwhile, the security system for these houses remain the same as for those in the previous street.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.
###思路：
将整个圆圈拆成两部分，一部分由（0-len-2）构成，一部分由（1-len-1）构成；

每部分按照上一题的解法得到最大值，这两个最大值的较大值为答案。
###代码：

```
int rob1(vector<int>& nums, int l, int r, int len) {
    if(len-1 == 0) return 0;
    int x = 0;
    int f0[2], f1[2];
    f0[x] = 0;
    f1[x] = nums[l];
    
    for(int i = l+1; i <= r; ++i) {
        x = x^1;
        f1[x] = f0[x^1] + nums[i];
        f0[x] = max(f1[x^1], f0[x^1]);
    }
    return max(f1[x], f0[x]);
}

int rob(vector<int>& nums) {
    int len = nums.size();
    if(len == 0) return 0;
    if(len == 1) return nums[0];
    return max(rob1(nums, 0, len-2, len), rob1(nums, 1, len-1, len));
}
```


##221. Maximal Square
###题目：
Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

For example, given the following matrix:

```
1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0
```
Return 4.
###思路：
####方法1：最暴力的方法是O(n^4)复杂度

在每一个位置，都用O(n^2)的复杂度得到当前以这个位置为左上角，最大的全为1的正方形。

####方法2：稍微预处理，O(n^3)复杂度

预处理每一行，得到每一个位置向后最长的连续1的个数，O(n^2)

然后遍历每一个位置，在这个位置上，向下遍历，得到以当前位置为左上角的最大全为1的正方形。
O(n^2) * O(n)。

####方法3：DP（代码如下）

由于肯定需要遍历过所有的元素，复杂度不会少于O(mn)，上面稍微处理的复杂度就是O(n^3)，所以这里DP的复杂度就是O(mn)。

又因为这里状态没法用O(m)表示，状态就是O(mn)，那么转移就是O(1)。

**状态：**

f[i][j]表示以(i,j)点为右下角，最大的全1正方形的边长。

**转移：**

```
if(matrix[i][j] == '0') f[i][j] = 0;
else {
    int t = min(f[i-1][j-1], f[i-1][j]);
    t = min(f[i][j-1], t);
    f[i][j] = t + 1;
}
```

**初始化：**

由于涉及i-1和j-1的操作，先初始化第一行和第一列。
###代码：

```
int maximalSquare(vector<vector<char>>& matrix) {
    int m = matrix.size();
    if(m == 0) return 0;
    int n = matrix[0].size();
    int f[m][n];
    memset(f, 0, sizeof(f));
    for(int i = 0; i < n; ++i) 
        f[0][i] = matrix[0][i] == '1' ? 1 : 0;
        
    for(int i = 0; i < m; ++i) 
        f[i][0] = matrix[i][0] == '1' ? 1 : 0;
        
    for(int i = 1; i < m; ++i) 
        for(int j = 1; j < n; ++j) {
            if(matrix[i][j] == '0') f[i][j] = 0;
            else {
                int t = min(f[i-1][j-1], f[i-1][j]);
                t = min(f[i][j-1], t);
                f[i][j] = t + 1;
            }
        }
        
    int res = 0;
    for(int i = 0; i < m; ++i) 
        for(int j = 0; j < n; ++j) 
            res = max(f[i][j], res);
        
    return res * res;
}
```

##264. Ugly Number II *
###题目：
Write a program to find the **n**-th ugly number.

Ugly numbers are positive numbers whose prime factors only include **2, 3, 5**. For example, **1, 2, 3, 4, 5, 6, 8, 9, 10, 12** is the sequence of the first **10** ugly numbers.

Note that **1** is typically treated as an ugly number.

**Hint:**

1. The naive approach is to call isUgly for every number until you reach the nth one. Most numbers are not ugly. Try to focus your effort on generating only the ugly ones.
2. An ugly number must be multiplied by either 2, 3, or 5 from a smaller ugly number.
3. The key is how to maintain the order of the ugly numbers. Try a similar approach of merging from three sorted lists: L1, L2, and L3.
4. Assume you have Uk, the kth ugly number. Then Uk+1 must be Min(L1 * 2, L2 * 3, L3 * 5).

###思路：
由于Ugly Number一定满足是全部由2,3,5的乘积，那么后面的数字一定是由前面的数字乘以这3个数得到。

开始l2,l3,l5都指向第一个数字，每次寻找(l2 * 2), (l3 * 3), (l5 * 5)中最小的一个加入结果，并且将对应的l*向后移动。 

这里注意下面的if 并不是if else关系，因为6这样的数字需要l2 和l3都加
###代码：

```
int nthUglyNumber(int n) {
    vector<int> res(n);
    res[0] = 1;
    int l2 = 0, l3 = 0, l5 = 0;
    for(int i = 1; i < n; ++i) {
        res[i] = min(res[l2] * 2, min(res[l3] * 3, res[l5] * 5));
        if(res[i] == res[l2] * 2) ++l2;
        if(res[i] == res[l3] * 3) ++l3;
        if(res[i] == res[l5] * 5) ++l5;
    }
    return res[n-1];
}
```


##279. Perfect Squares
###题目：
Given a positive integer n, find the least number of perfect square numbers (for example, **1, 4, 9, 16, ...**) which sum to n.

For example, given n = **12**, return **3** because **12 = 4 + 4 + 4**; given *n* = **13**, return **2** because **13 = 4 + 9**.
###思路：
**状态:**

f[i]表示加和为i的最少完全平方数的个数；

**转移：**

要遍历j满足1-sqrt(i)， ```f[i] = min(f[i], 1 + f[i - j * j])```

**初始化：**

f[0] = 0
###代码：

```
int numSquares(int n) {
    int f[n+1];
    f[0] = 0;
    for(int i = 1; i <= n; ++i) {
        f[i] = INT_MAX;
        for(int j = 1; j <= (int)sqrt(i); ++j) {
            f[i] = min(f[i], 1 + f[i - j * j]);
        }
    }
    return f[n];
}
```

##300. Longest Increasing Subsequence
###题目：
Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,

Given [10, 9, 2, 5, 3, 7, 101, 18],

The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. 

Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

**Follow up:** Could you improve it to O(n log n) time complexity?
###思路：
方法1：时间复杂度：O(n^2)，利用DP记录

维护一个数组 R[len]，从右向左更新，记录以nums[i]为开头的LIS最大长度。然后取这个数组中最大的那一个。

方法2：时间复杂度 O(nlogn)，利用二分

[GeekForGeek](http://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/)具体讲解。

主要思路就是维护一个数组res来记录最长的连续串。从左向右遍历数组，如果nums[i]比res中所有数组都大，那么将其添加在最后，否则将其替换res中第一个大于等于他的位置上的数字，酱紫不会使原来的答案数组不成立，但是会给后面的数字有更多可能。

###代码：

```
//方法1：
int lengthOfLIS(vector<int>& nums) {
    int res = 0, len = nums.size();
    if(len == 0) return 0;
    vector<int> R(len, 1);
    for(int i = len-2; i >= 0; --i) {
        for(int j = i+1; j < len; ++j) {
            if(nums[j] > nums[i]) {
                R[i] = max(R[i], R[j]+1);
            }
        }
    }
    for(int i = 0; i < len; ++i) {
        res = max(res, R[i]);
    }
    return res;
}

//方法2：
int lengthOfLIS(vector<int>& nums) {
    vector<int> res;
    for(int i = 0; i < nums.size(); i++) {
        auto it = std::lower_bound(res.begin(), res.end(), nums[i]);
        if(it == res.end()) res.push_back(nums[i]);
        else *it = nums[i];
    }
    return res.size();
}
```
##303. Range Sum Query - Immutable
###题目：
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

**Example:**

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```
**Note:**

* You may assume that the array does not change.
* There are many calls to sumRange function.

###思路：
主要是结果不是直接sumPre[j]-sumPre[i]

如果i == 0, res = sumPre[j]

如果i != 0, res = sumPre[j] - sumPre[i-1]
###代码：

```
class NumArray {
public:
    NumArray(vector<int> &nums) {
        int len = nums.size();
        sumPre = vector<int>(len,0);
        if(len == 0) return;
        sumPre[0] = nums[0];
        for(int i = 1; i < len; ++i) 
            sumPre[i] = sumPre[i-1] + nums[i];
    }

    int sumRange(int i, int j) {
        if(i == 0) return sumPre[j];
        else return sumPre[j] - sumPre[i-1];
    }
private:
    vector<int> sumPre;
};

// Your NumArray object will be instantiated and called as such:
// NumArray numArray(nums);
// numArray.sumRange(0, 1);
// numArray.sumRange(1, 2);
```


##304. Range Sum Query 2D - Immutable
###题目：
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner *(row1, col1)* and lower right corner *(row2, col2)*.


**Example:**

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
sumRegion(1, 1, 2, 2) -> 11
sumRegion(1, 2, 2, 4) -> 12
```
**Note:**

* You may assume that the matrix does not change.
* There are many calls to sumRegion function.
* You may assume that *row1 ≤ row2* and *col1 ≤ col2*.

###思路：
参见[思路](https://discuss.leetcode.com/topic/29536/clean-c-solution-and-explaination-o-mn-space-with-o-1-time)

**状态：**

sum[i][j] 代表以点(i,j)这个点为右下角，以整个矩阵的左上角为左上角组成的矩形的数字和。
 
**转移：**

```
if(i > 0) sum[i][j] += sum[i-1][j];
if(j > 0) sum[i][j] += sum[i][j-1];
if(i > 0 && j > 0) sum[i][j] -= sum[i-1][j-1];
sum[i][j] += matrix[i][j];
```
结果需要根据sum矩阵进行计算得到，但是O(1)的时间可以得到。

**初始化：**

sum[0][0] = matrix[0][0]。

###代码：

```
class NumMatrix {
public:
    NumMatrix(vector<vector<int>> &matrix) {
        m = matrix.size();
        if(m > 0) {
            n = matrix[0].size();
            sum = vector<vector<int>>(m, vector<int>(n, 0));
            sum[0][0] = matrix[0][0];
            for(int i = 0; i < m; ++i) {
                for(int j = 0; j < n; ++j) {
                    if(i == 0 && j == 0) continue;
                    if(i > 0) sum[i][j] += sum[i-1][j];
                    if(j > 0) sum[i][j] += sum[i][j-1];
                    if(i > 0 && j > 0) sum[i][j] -= sum[i-1][j-1];
                    sum[i][j] += matrix[i][j];
                }
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        int res = sum[row2][col2];
        if(row1 > 0) res -= sum[row1-1][col2];
        if(col1 > 0) res -= sum[row2][col1-1];
        if(row1 > 0 && col1 > 0) res += sum[row1-1][col1-1];
        return res;
    }
private:
    vector<vector<int>> sum;
    int m, n;
};

// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.sumRegion(1, 2, 3, 4);
```

##312. Burst Balloons *
###题目：
Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.

Find the maximum coins you can collect by bursting the balloons wisely.

**Note:**

(1) You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.
(2) 0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100

**Example:**

Given [3, 1, 5, 8]

Return 167

```
    nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []
   coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167
```
###思路：
**状态：**

int dp[i][j]:代表（i,j）范围内的最大乘积。

**转移：**

这里需要遍历范围的长度，左端点，然后再遍历这个范围内最后一个爆破的气球位置。

```
for (int len = 1; len <= n; ++len) 
    for (int left = 1; left <= (n - len + 1); ++left) {
        int right = left + len - 1;
        for (int x = left; x <= right; ++x) 
            dp[left][right] = max(dp[left][right], dp[left][x-1] + nums[left-1] * nums[x] * nums[right+1] + dp[x+1][right]);
        
    }
```
**初始化：**

在nums头尾加1，然后dp[i][j] = 0

[解释](https://discuss.leetcode.com/topic/41086/c-dp-detailed-explanation)
###代码：

```
int maxCoins(vector<int>& nums) {
    int n = nums.size();
    nums.insert(nums.begin(), 1);
    nums.push_back(1);
    
    vector<vector<int>> dp(nums.size(), vector<int>(nums.size(), 0));
    
    for (int len = 1; len <= n; ++len) 
        for (int left = 1; left <= (n - len + 1); ++left) {
            int right = left + len - 1;
            for (int x = left; x <= right; ++x) 
                dp[left][right] = max(dp[left][right], dp[left][x-1] + nums[left-1] * nums[x] * nums[right+1] + dp[x+1][right]);
            
        }
    
    return dp[1][n];
}
```


##322. Coin Change
###题目：
You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

**Example 1:**

coins = **[1, 2, 5]**, amount = **11**

return **3** (11 = 5 + 5 + 1)

**Example 2:**

coins = **[2]**, amount = **3**

return **-1**.

**Note:**

You may assume that you have an infinite number of each kind of coin.
###思路：
**状态：**

int f[i]：代表面值加和为i用到的最少钱币个数

**转移：**

要遍历所有钱币，看如果可以的话去掉这个钱币面值的最小个数是多少，加一则是此位置答案。

```
for(int j = 0; j < m; ++j) 
	if(i >= coins[j]) f[i] = min(f[i], f[i - coins[j]] + 1);
```
**初始化：**

开始所有值均为（最大面值+1），f[0] = 0
###代码：

```
int coinChange(vector<int>& coins, int amount) {
    int m = coins.size();
    if(m == 0) {
        if(!amount) return 0;
        else return -1;
    }
    vector<int> f(amount+1, amount+1);
    f[0] = 0;
    for(int i = 1; i <= amount; ++i) 
        for(int j = 0; j < m; ++j) 
            if(i >= coins[j]) f[i] = min(f[i], f[i - coins[j]] + 1);
    
    return (f[amount] == (amount+1) ? -1 : f[amount]);
}
```

##338. Counting Bits
###题目：
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

**Example:**

For **num = 5** you should return **[0,1,1,2,1,2]**.

**Follow up:**

* It is very easy to come up with a solution with run time **O(n * sizeof(integer))**. But can you do it in linear time **O(n)** /possibly in a single pass?
* Space complexity should be **O(n)**.
* Can you do it like a boss? Do it without using any builtin function like **__builtin_popcount** in c++ or in any other language.

**Hint:**

* You should make use of what you have produced already.
* Divide the numbers in ranges like [2-3], [4-7], [8-15] and so on. And try to generate new range from previous.
* Or does the odd/even status of the number help you in calculating the number of 1s?

###思路：

####方法1： 时间复杂度O(n*sizeof(i))：

对于每个数都计数它的1的位数，用右移操作。

####方法2： 时间复杂度O(n):

**状态：**

int f[i]：代表数字i的1的位数。

**转移：**

其中用到了数学知识：i & (i-1)会去掉i的最后一个1，使1的位数少1，而且这个循环从小到大，计算大的结果时小的结果已经得到。

f[i] = f[i & (i-1)] + 1

**初始化：**

f[0] = 0

###代码：

```
//最容易的时间复杂度O(n*sizeof(i))
vector<int> countBits(int num) {
    vector<int> res(num+1, 0);
    for(int i = 0; i <= num; ++i) {
        int now = i;
        while(now > 0) {
            if(now & 1) 
                res[i]++;
            now >>= 1;
        }
    }
    return res;
}

//时间复杂度是O(n)
vector<int> countBits(int num) {
    vector<int> res(num+1, 0);
    for(int i = 1; i <= num; ++i) res[i] = res[i & (i-1)] + 1;
    return res;
}
```


##343. Integer Break
###题目：
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.

For example, given *n* = 2, return 1 (2 = 1 + 1); given *n* = 10, return 36 (10 = 3 + 3 + 4).

**Note:** You may assume that n is not less than 2 and not larger than 58.

**Hint:**

* There is a simple O(n) solution to this problem.
* You may check the breaking results of n ranging from 7 to 10 to discover the regularities.
###思路：
**状态：**

int f[i]：表示加和为i的最少两个正整数的最大乘积。

**转移：**

转移复杂度为O(n)。因为需要遍历所有小于i的正整数。

f[i] = max(f[i], j * (i-j), j * f[i-j])

**初始化：**

f[1] = 0, f[2] = 1。

经过观察的O(n)的解法：[分割出3结果最大](https://discuss.leetcode.com/topic/43525/c-o-n-solution-with-dp)
###代码：

```
//最直接的O(n^2)
int integerBreak(int n) {
    int f[n+1];
    memset(f, 0, sizeof(f));
    f[0] = 0;
    f[1] = 0;
    f[2] = 1;
    for(int i = 3; i <= n; ++i) 
        for(int j = 1; j < i; ++j) {
            f[i] = max(f[i], j * (i-j));
            f[i] = max(j * f[i-j], f[i]);
        }
    
    return f[n];
}

//O(n)
int integerBreak(int n) {
    int dp[n + 1];
    dp[0] = 0;
    dp[1] = 1;
    dp[2] = 1;
    dp[3] = 2;
    dp[4] = 4;
    for (int i = 5; i <= n; ++i) {
        dp[i] = 3 * max(i - 3, dp[i - 3]);
    }
    return dp[n];
}
```

##357. Count Numbers with Unique Digits
###题目：
Given a **non-negative** integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

Given n = 2, return 91. (The answer should be the total numbers in the range of 0 ≤ x < 100, excluding **[11,22,33,44,55,66,77,88,99]**)

**Hint:**

* A direct way is to use the backtracking approach.
* Backtracking should contains three states which are (the current number, number of steps to get that number and a bitmask which represent which number is marked as visited so far in the current number). Start with state (0,0,0) and count all valid number till we reach number of steps equals to 10n.
* This problem can also be solved using a dynamic programming approach and some knowledge of combinatorics.
* Let f(k) = count of numbers with unique digits with length equals k.
* f(1) = 10, ..., f(k) = 9 * 9 * 8 * ... (9 - k + 2) [The first factor is 9 because a number cannot start with 0].

###思路：
结果存在规律，可以直接用公式计算。

f(1) = 10, ..., f(k) = 9 * 9 * 8 * ... (9 - k + 2)
###代码：

```
int countNumbersWithUniqueDigits(int n) {
    if(n == 0) return 1;
    if(n == 1) return 10;
    int i = 1;
    int res = 9;
    while(i < n) {
        res *= (10-i);
        i++;
    } 
    res += countNumbersWithUniqueDigits(n-1);
    return res;
}
```

##368. Largest Divisible Subset
###题目：
Given a set of **distinct** positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

**Example 1:**

```
nums: [1,2,3]

Result: [1,2] (of course, [1,3] will also be ok)
```
**Example 2:**

```
nums: [1,2,4,8]

Result: [1,2,4,8]
```
###思路：
首先动归都需要一个方向，所以首先将数组进行排序。

这个题目需要利用数学的一个背景：如果集合中的元素已经按照从小到大排序，且满足每两个之间可整除，说明处在最后的最大的数可以整除其他所有。那么新加入一个更大的数时，只要这个数可整除最后一个数字，那么同时肯定耶可以整除集合中其他更小的数。

**状态：**

vector<pair<int, int>> dp。由于此处需要记录路径，如果不记录路径，数组即可。

**转移：**

遍历i前面所有可被i整除的节点，取dp[i].first和dp[j].first+1的最大值更新结果。

最后沿着最大值的位置向前寻找路径。

**初始化：**

dp[0].first = 0, dp[0].second = -1。
###代码：

```
vector<int> largestDivisibleSubset(vector<int>& nums) {
    int len = nums.size();
    if(len <= 1) return nums;
    sort(nums.begin(), nums.end());
    vector<pair<int, int>> all(len, make_pair(1, -1));
    int maxNum = 1, maxIndex = 0;
    for(int i = 1; i < len; ++i) 
        for(int j = 0; j < i; ++j) {
            if(nums[i] % nums[j] == 0 && all[j].first + 1 > all[i].first) {
                all[i].first = all[j].first + 1;
                all[i].second = j;
                if(all[i].first > maxNum) {
                    maxNum = all[i].first;
                    maxIndex = i;
                }
            }
        }
    vector<int> res;
    for(; maxIndex >= 0; maxIndex = all[maxIndex].second) {
        res.push_back(nums[maxIndex]);
    }
    return res;
}
```

##375. Guess Number Higher or Lower II **
###题目：
We are playing the Guess Game. The game is as follows:

I pick a number from **1** to **n**. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number I picked is higher or lower.

However, when you guess a particular number x, and you guess wrong, you pay **x**. You win the game when you guess the number I picked.

**Example:**

```
n = 10, I pick 8.

First round:  You guess 5, I tell you that it's higher. You pay $5.
Second round: You guess 7, I tell you that it's higher. You pay $7.
Third round:  You guess 9, I tell you that it's lower. You pay $9.

Game over. 8 is the number I picked.

You end up paying $5 + $7 + $9 = $21.
```

Given a particular **n ≥ 1**, find out how much money you need to have to guarantee a **win**.

**Hint:**

* The best strategy to play the game is to minimize the maximum loss you could possibly face. Another strategy is to minimize the expected loss. Here, we are interested in the first scenario.
* Take a small example (n = 3). What do you end up paying in the worst case?
* Check out this article if you're still stuck.
* The purely recursive implementation of minimax would be worthless for even a small n. You MUST use dynamic programming.
* As a follow-up, how would you modify your code to solve the problem of minimizing the expected loss, instead of the worst-case loss?

###思路：
**状态**

f[i][j]:表示从区间[i, j]的最大代价的最小值

f[1][n]为答案

**转移**

外层循环是区间右边界，中间层循环是区间左边界，内层循环是中间猜的位置

```
int globalMin = INT_MAX;
for(int k = i+1; k < j; k++){
    int localMax = k + max(table[i][k-1], table[k+1][j]);
    globalMin = min(globalMin, localMax);
}
table[i][j] = (i + 1 == j ? i : globalMin);
```
 **初始化**

vector<vector<int>> table(n+1, vector<int>(n+1, 0));

###代码：

```
int getMoneyAmount(int n) {
    vector<vector<int>> table(n+1, vector<int>(n+1, 0));
    
    for(int j = 2; j <= n; j++){
        for(int i = j - 1; i > 0; i--){
            int globalMin = INT_MAX;
            for(int k = i+1; k < j; k++){
                int localMax = k + max(table[i][k-1], table[k+1][j]);
                globalMin = min(globalMin, localMax);
            }
            table[i][j] = (i + 1 == j ? i : globalMin);
        }
    }
    return table[1][n];
}
```


##376. Wiggle Subsequence
###题目：
A sequence of numbers is called a wiggle sequence if the differences between successive numbers strictly alternate between positive and negative. The first difference (if one exists) may be either positive or negative. A sequence with fewer than two elements is trivially a wiggle sequence.

For example, **[1,7,4,9,2,5]** is a wiggle sequence because the differences **(6,-3,5,-7,3)** are alternately positive and negative. In contrast, **[1,4,7,2,5]** and **[1,7,4,5,5]** are not wiggle sequences, the first because its first two differences are positive and the second because its last difference is zero.

Given a sequence of integers, return the length of the longest subsequence that is a wiggle sequence. A subsequence is obtained by deleting some number of elements (eventually, also zero) from the original sequence, leaving the remaining elements in their original order.

**Examples:**

```
Input: [1,7,4,9,2,5]
Output: 6
The entire sequence is a wiggle sequence.

Input: [1,17,5,10,13,15,10,5,16,8]
Output: 7
There are several subsequences that achieve this length. One is [1,17,10,13,10,16,8].

Input: [1,2,3,4,5,6,7,8,9]
Output: 2
```
**Follow up:**

Can you do it in O(n) time?
###思路：
//方法1：
如果个数小于等于1，那么直接返回个数；

sig记录上一个趋势，即前两个数中后一个减去前一个是正还是负。开始是0，这里正的用1表示，负的用-1表示。

pre记录上一个数， res记录答案个数。

过程是：从前向后扫，如果是刚开始，只要后面的数和前面的不一样，那么更新sig和pre，res++，否则只是更新pre。

如果当前数字减去pre和sig符号相同，说明并不符合规则，更新pre；

如果当前数字减去pre和sig符号不同，说明符合规则，更新sig和pre，res++。

//方法2：

cnt1表示结尾是上升的串的个数，cnt2表示结尾是下降的串的个数

如果遇到下降，那么cnt2 = cnt1 + 1;

如果遇到上升，那么cnt1 = cnt2 + 1;
###代码：

```
//方法1：
int wiggleMaxLength(vector<int>& nums) {
    int len = nums.size();
    if(len < 2) return len;
    
    int res = 1, pre = nums[0], sig = 0;
    for(int i = 1; i < len; ++i) {
        if(sig == 0) {
            if(nums[i] != pre) {
                sig = (nums[i] - pre > 0 ? 1 : -1);
                res++;
            }
            pre = nums[i];
        }else if((nums[i] - pre) * sig >= 0) {
            pre = nums[i];
        }else {
            sig = 0 - sig;
            pre = nums[i];
            res++;
        }
    }
    return res;
}

//方法2：
int wiggleMaxLength(vector<int>& nums) {
	int count1 = 1, count2 = 1;
	for(int i = 1; i < nums.size(); ++i) 
	    if(nums[i] > nums[i-1]) count1 = count2 + 1;
	    else if(nums[i] < nums[i-1]) count2 = count1 + 1;
	return nums.size() == 0 ? 0 : max(count1, count2);
}
```

##377. Combination Sum IV
###题目：
Given an integer array with all positive numbers and no duplicates, find the number of possible combinations that add up to a positive integer target.

Example:

```
nums = [1, 2, 3]
target = 4

The possible combination ways are:
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

Note that different sequences are counted as different combinations.

Therefore the output is 7.
```
**Follow up:**

* What if negative numbers are allowed in the given array?
* How does it change the problem?
* What limitation we need to add to the question to allow negative numbers?

###思路：
这里不是简单的背包问题，因为他是排列问题，不仅仅是取过这种物品，后面不会再取这种物品。

**状态：**

dp[i]，i代表加和为i的集合有多少了。

**转移：**

对于每个i，遍历所有的nums[j]，如果nums[j] <= i，那说明可以在原先(dp[i-nums[j]])的集合中后面都加上nums[j]组成新的集合，这样的话将所有nums[j]遍历的结果进行加和则可以。

**初始化：**

dp[nums[j]] = 1
###代码：

```
int combinationSum4(vector<int>& nums, int target) {
    int len = nums.size();
    int f[target+1];
    memset(f, 0, sizeof(f));
    for(int i = 0; i < len; ++i) {
        if(nums[i] <= target) f[nums[i]] = 1;
    }
    for(int i = 1; i <= target; ++i) {
        for(int j = 0; j < len; ++j) {
            if(i >= nums[j]) {
                f[i] += f[i-nums[j]];
            }
        }
    }
    return f[target];
}
```

##392. Is Subsequence
###题目：
Given a string **s** and a string **t**, check if s is subsequence of t.

You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length ~= 500,000) string, and s is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).

**Example 1:**

s = **"abc"**, t = **"ahbgdc"**

Return **true**.

**Example 2:**

s = **"axc"**, t = **"ahbgdc"**

Return **false**.

**Follow up:**

If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?
###思路：
基础思路：很简单，见代码

拓展的思路见 [解释](https://discuss.leetcode.com/topic/58367/binary-search-solution-for-follow-up-with-detailed-comments)
###代码：

```
//基础：
bool isSubsequence(string s, string t) {
    int lens = s.size(), lent = t.size();
    int i = 0, j = 0;
    while(i < lens && j < lent) {
        if(s[i] == t[j]) {
            i++;
            j++;
        }else {
            j++;
        }
    }
    if(i == lens) return true;
    else return false;
}

//适用于拓展的方法：
bool isSubsequence(string s, string t) {
    unordered_map<char, vector<int>> hash;
    int lent = t.size(), lens = s.size();
    for(int i = 0; i < lent; ++i) {
        if(hash.find(t[i]) == hash.end()) {
            vector<int> now;
            now.push_back(i);
            hash[t[i]] = now;
        }
        else hash[t[i]].push_back(i);
    }
    
    int pre = 0;
    for(int i = 0; i < lens; ++i) {
        if(hash.find(s[i]) == hash.end()) return false;
        else {
            auto now = lower_bound(hash[s[i]].begin(), hash[s[i]].end(), pre);
            if (now == hash[s[i]].end()) return false;
            else pre = hash[s[i]][now - hash[s[i]].begin()] + 1;
        }
    }
    return true;
}
```

##413. Arithmetic Slices
###题目：
A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.

For example, these are arithmetic sequence:

```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```
The following sequence is not arithmetic.

```
1, 1, 2, 5, 7
```
A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.

A slice (P, Q) of array A is called arithmetic if the sequence:

A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.

The function should return the number of arithmetic slices in the array A.


**Example:**

```
A = [1, 2, 3, 4]

return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
```
###思路：

####纯DP思路：
**状态：**

int dp[i]:代表以i为结尾的arithmetic slices的个数。答案就是将所有dp[i]相加。

**转移：**

if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) dp[i] = dp[i - 1] + 1;

如果i位可以和前两位组成arithmetic slices，那么以i结尾的个数会是i-1结尾+1，因为以i-1结尾的加上i位都合法，从i-2~i这个是新加入的。
**初始化：**

dp[i] = 0

####结合数学思想：

遍历一次数组，就可以将所有等差序列的序列个数记录下来

如果序列个数大于等于3，那么这个等差序列可以得到的arithmetic slices个数可以由公式(num-1)*(num-2)/2 计算得到。
###代码：

```
//纯DP思想：
int numberOfArithmeticSlices(vector<int>& A) {
    if (A.size() < 3)   return 0;
    vector<int> dp(A.size(), 0);
    int res = 0;
    for (int i = 2; i < A.size(); i ++) {
        if (A[i] - A[i - 1] == A[i - 1] - A[i - 2])
            dp[i] = dp[i - 1] + 1;
        res += dp[i];
    }
    return res;
}

//结合数学思想：
int numberOfArithmeticSlices(vector<int>& A) {
    vector<int> res;
    int len = A.size();
    if(len <= 2) return 0;
    int first = 0, minus = A[1] - A[0];
    for(int i = 2; i < len; ++i) {
        if(A[i] - A[i-1] != minus) {
            int now = i - first;
            if(now >= 3) res.push_back(now);
            minus = A[i] - A[i-1];
            first = i-1;
        }
    }
    if(len - first >= 3) res.push_back(len - first);
    int R = 0;
    for(int each : res) {
        R += (each-1)*(each-2)/2;
    }
    return R;
}
```

##416. Partition Equal Subset Sum
###题目：
Given a **non-empty** array containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Note:**

* Each of the array element will not exceed 100.
* The array size will not exceed 200.

**Example 1:**

```
Input: [1, 5, 11, 5]

Output: true

Explanation: The array can be partitioned as [1, 5, 5] and [11].
```
**Example 2:**

```
Input: [1, 2, 3, 5]

Output: false

Explanation: The array cannot be partitioned into equal sum subsets.
```
###思路：
问题的本质是是否可以有一个子集和为一个特定值。

**状态：**

bool f[i]:代表是否有子集的和为i。

**转移：**

这里一定要从target到nums[i]进行遍历，不可以从nums[i]到target，反过来的话会重复选。

```
for(int j = target; j >= nums[i]; --j) {
    f[j] = f[j] || f[j-nums[i]];
    if(f[target]) return true;
}
```

**初始化：**

```
memset(f, false, sizeof(f));
f[0] = true;
```
###代码：

```
bool canPartition(vector<int>& nums) {
    int sumAll = accumulate(nums.begin(), nums.end(), 0);
    if(sumAll % 2) return false;
    int target = sumAll / 2, len = nums.size();
    bool f[target+1];
    memset(f, false, sizeof(f));
    f[0] = true;
    for(int i = 0; i < len; ++i) 
        for(int j = target; j >= nums[i]; --j) {
            f[j] = f[j] || f[j-nums[i]];
            if(f[target]) return true;
        }
    
    return false;
}
```


