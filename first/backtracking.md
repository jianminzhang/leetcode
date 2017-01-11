#Backtracking
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

##17. Letter Combinations of a Phone Number
###题目：
Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters just like on the telephone buttons.

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.
###思路：

###代码：

```
unordered_map<char, string> hash;
void init() {
    hash['2'] = "abc";
    hash['3'] = "def";
    hash['4'] = "ghi";
    hash['5'] = "jkl";
    hash['6'] = "mno";
    hash['7'] = "pqrs";
    hash['8'] = "tuv";
    hash['9'] = "wxyz";
    hash['0'] = " ";
}

void getAll(const string digits, const int n, int i, vector<string> &res, string path) {
    if(i >= n) {
        res.push_back(path);
        return;
    }
    string now = hash[digits[i]];
    int len = now.size();
    for(int j = 0; j < len; ++j) {
        getAll(digits, n, i+1, res, path + now[j]);
    }
}

vector<string> letterCombinations(string digits) {
    int n = digits.size();
    
    init();
    vector<string> res;
    string path;
    if(n == 0) return res;
    getAll(digits, n, 0, res, path);
    return res;
}
```

##22. Generate Parentheses
###题目：
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
###思路：
从左向右生成合理的括号匹配结果。每次生成的时候，只要是有没有用的“（”就可以直接填充，如果剩下的右括号比左括号多，那么也可以填充右括号。

如果左右括号都用完了，那么可以记录这一次结果。
###代码：

```
void getParenthesis(vector<string> &res, string path, int left, int right) {
    if(left == 0 && right == 0) res.push_back(path);
    if(left > 0) getParenthesis(res, path + "(", left - 1, right);
    if(right > left) getParenthesis(res, path + ")", left, right - 1);
} 

vector<string> generateParenthesis(int n) {
    vector<string> res;
    getParenthesis(res, "", n, n);
    return res;
}
```
##37. Sudoku Solver
###题目：
Write a program to solve a Sudoku puzzle by filling the empty cells.

Empty cells are indicated by the character '.'.

You may assume that there will be only one unique solution.
###思路：
用三个二维数组分别记录每行、每列和每个小方格中1-9的使用情况；

初始状态已经填写的数字要首先赋值；

后面回溯遍历每种情况时再修改和恢复原状；

当这个棋盘都填完之后，即可返回。
###代码：

```
class Solution {
public:
    bool dfs(vector<vector<char>>& board, int x, int y) {
        if(x > 8) {
            res = board;
            return true;
        }
        int xx = x, yy = y;
        if(y > 7) xx = x+1, yy = 0;
        else yy = y+1;
        if(board[x][y] != '.') {
            if(dfs(board, xx, yy)) return true;
            else return false;
        }
        
        for(int i = 0; i < 9; ++i) {
            if(r[x][i] || c[y][i] || b[(x/3)*3+y/3][i]) continue;
            r[x][i] = c[y][i] = b[(x/3)*3+y/3][i] = true;
            board[x][y] = i + '1';
            if(dfs(board, xx, yy)) return true;
            board[x][y] = '.';
            r[x][i] = c[y][i] = b[(x/3)*3+y/3][i] = false;
        }
        return false;
    }
    
    void solveSudoku(vector<vector<char>>& board) {
        memset(r, false, sizeof(r));
        memset(c, false, sizeof(c));
        memset(b, false, sizeof(b));
        for(int i = 0; i < 9; ++i)
            for(int j = 0; j < 9; ++j) 
                if(board[i][j] != '.') {
                    int now = board[i][j] - '0';
                    r[i][now-1] = true;
                    c[j][now-1] = true;
                    b[(i/3)*3+j/3][now-1] = true;
                    
                }
        res.clear();
        dfs(board, 0, 0);
        board = res;
    }
private:
    bool r[9][9], c[9][9], b[9][9];
    vector<vector<char>> res;
};
```

##39. Combination Sum
###题目：
Given a set of candidate numbers (C) (without duplicates) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

The same repeated number may be chosen from C unlimited number of times.

Note:

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

For example, given candidate set [2, 3, 6, 7] and target 7, 

A solution set is: 

```
[
  [7],
  [2, 2, 3]
]
```
###思路：

###代码：

```
void com(vector<int>& candidates, vector<int> &path, const int n, int i, int target, vector<vector<int>> &res) {
    if(target == 0) {
        res.push_back(path);
        return;
    }
    else if(target < 0) return;
    else {
        for(int j = i; j < n; ++j) {
            path.push_back(candidates[j]);
            com(candidates, path, n, j, target - candidates[j], res);
            path.pop_back();
        }
    }
}

vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    int n = candidates.size();
    vector<vector<int>> res;
    vector<int> path;
    com(candidates, path, n, 0, target, res);
    return res;
}
```
##40. Combination Sum II
###题目：
Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.

Each number in C may only be used once in the combination.

**Note:**

* All numbers (including target) will be positive integers.
* The solution set must not contain duplicate combinations.

For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8, 
A solution set is: 

```
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```
###思路：
这个题和上题不一样之处在于每个位置的数字只能用一次

且要求保证不重复，这里相同值得数字是相同对待的
###代码：

```
void com(vector<int>& candidates, vector<int> &path, const int n, int i, int target, vector<vector<int>> &res) {
    if(target == 0) {
        res.push_back(path);
        return;
    }
    else if(target < 0) return;
    else {
        for(int j = i; j < n; ++j) {
            if(j == i || candidates[j] != candidates[j-1]) { //保证不重复
                path.push_back(candidates[j]);
                com(candidates, path, n, j+1, target - candidates[j], res); //保证只用一次
                path.pop_back();
            }
        }
    }
}

vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end());
    int n = candidates.size();
    vector<vector<int>> res;
    vector<int> path;
    com(candidates, path, n, 0, target, res);
    return res;
}
```

##216. Combination Sum III
###题目：
Find all possible combinations of ***k*** numbers that add up to a number ***n***, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.

***Example 1:***

Input: ***k*** = 3, ***n*** = 7

Output:

```
[[1,2,4]]
```
***Example 2:***

Input: ***k*** = 3, ***n*** = 9

Output:

```
[[1,2,6], [1,3,5], [2,3,4]]
```
###思路：
思路和前两道基本相同，只是需要判断现在参与组合的个数是否达到标准。
###代码：

```
void dfs(int n, int k, int i, vector<vector<int>> &res, vector<int> &path) {
    if(k == 0 && n == 0) {
        res.push_back(path);
        return;
    }else if(k == 0 && n != 0 || k != 0 && n == 0) {
        return;
    }else {
        for(int j = i; j <= 9; ++j) {
            path.push_back(j);
            dfs(n-j, k-1, j+1, res, path);
            path.pop_back();
        }
    }
}

vector<vector<int>> combinationSum3(int k, int n) {
    vector<vector<int>> res;
    vector<int> path;
    dfs(n, k, 1, res, path);
    return res;
}
```

##46. Permutations
###题目：
Given a collection of **distinct** numbers, return all possible permutations.

For example,

**[1,2,3]** have the following permutations:

```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```
###思路：
回溯的思想

方法1：这里需要使用所有数字，且每个结果里面不能重复使用数字。

所以要用结果内数字个数作为结束条件，里面需要有一个yes数组记录每个数字是否用过。

方法2：

因为不需要去掉原先数组的内容，只是调换顺序，所以可以使用swap生成所有可能。

###代码：

```
//方法1：
void dfs(vector<int>& nums, vector<vector<int>> &res, vector<int> &path, vector<bool> &yes, int num, const int len) {
    if(num == len) {
        res.push_back(path);
        return;
    }
    else {
        for(int i = 0; i < len; ++i) {
            if(yes[i] == false) {
                path.push_back(nums[i]);
                yes[i] = true;
                dfs(nums, res, path, yes, num+1, len);
                path.pop_back();
                yes[i] = false;
            }
        }
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    int len = nums.size();
    vector<vector<int>> res;
    vector<int> path;
    vector<bool> yes(len, false);
    dfs(nums, res, path, yes, 0, len);
    return res;
}

//方法2：
void dfs(vector<int> &nums, vector<vector<int>> &res, int i, const int len) {
    if(i == len - 1) {
        res.push_back(nums);
        return;
    }
    else {
        for(int j = i; j < len; ++j) {
            swap(nums[i], nums[j]);
            dfs(nums, res, i+1, len);
            swap(nums[i],nums[j]);
        }
    }
}
    
vector<vector<int>> permute(vector<int>& nums) {
    int len = nums.size();
    vector<vector<int>> res;
    dfs(nums, res, 0, len);
    return res;
}
```
##47. Permutations II
###题目：
Given a collection of numbers that might contain duplicates, return all possible unique permutations.

For example,

**[1,1,2]** have the following unique permutations:

```
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]
```
###思路：
回溯的思想，使用swap进行。

重要的地方是需要保证不重复。

```
if(j != i && nums[j] == nums[i]) continue;
```
###代码：

```
void dfs(vector<int> nums, vector<vector<int>> &res, int i, const int len) {
    if(i == len - 1) {
        res.push_back(nums);
        return;
    }
    else {
        for(int j = i; j < len; ++j) {
            if(j != i && nums[j] == nums[i]) continue;
            swap(nums[i], nums[j]);
            dfs(nums, res, i+1, len);
        }
    }
}

vector<vector<int>> permuteUnique(vector<int>& nums) {
    int len = nums.size();
    sort(nums.begin(), nums.end());
    vector<vector<int>> res;
    dfs(nums, res, 0, len);
    return res;
}
```

##60. Permutation Sequence
###题目：
The set [1,2,3,…,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):


1. "123"
2. "132"
3. "213"
4. "231"
5. "312"
6. "321"

Given n and k, return the kth permutation sequence.

Note: Given n will be between 1 and 9 inclusive.
###思路：

###代码：

```
void dfs(vector<int> &num, int per, int k, string &res) {
    if(num.empty()) return;
    per /= num.size();
    int now = k / per;
    res += to_string(num[now]);
    num.erase(num.begin() + now);
    dfs(num, per, k % per, res);
}

string getPermutation(int n, int k) {
    string res = "";
    int per = 1;
    for(int i = 2; i <= n; ++i) per *= i;
    vector<int> num(n);
    for(int i = 0; i < n; ++i) num[i] = i+1;
    dfs(num, per, k-1, res);
    return res;
}
```

##51. N-Queens
###题目：
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

For example,

There exist two distinct solutions to the 4-queens puzzle:

```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```
###思路：
n皇后问题的对角线攻击不只是指主对角线或者副对角线。
###代码：

```
class Solution {
public:
    void dfs(int n, vector<string> &path, int index) {
        if(index >= n) {
            res.push_back(path);
            return;
        }
        for(int i = 0; i < n; ++i) {
            if(c[i] || d1[index-i+n] || d2[index+i]) continue;
            c[i] = d1[index-i+n] = d2[index+i] = true;
            path[index][i] = 'Q';
            dfs(n, path, index+1);
            path[index][i] = '.';
            c[i] = d1[index-i+n] = d2[index+i] = false;
        }
    }
    
    vector<vector<string>> solveNQueens(int n) {
        c = vector<bool>(n, false);
        d1 = vector<bool>(2 * n, false);
        d2 = vector<bool>(2 * n, false);
        
        string each = "";
        for(int i = 0; i < n; ++i) each += '.';
        vector<string> path(n, each); 
        dfs(n, path, 0);
        return res;
    }
private:
    vector<vector<string>> res;
    vector<bool> c, d1, d2;
};
```

##78. Subsets
###题目：
Given a set of distinct integers, nums, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,

If ***nums*** = **[1,2,3]**, a solution is:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```
###思路：
子集不仅包含相邻的，还包含不相邻的所有集合，所以在回溯函数内部需要遍历。
###代码：

```
void dfs(vector<int>& nums, const int len, vector<vector<int>> &res, vector<int> &path, int i) {
    res.push_back(path);
    for(int j = i; j < len; ++j) {
        path.push_back(nums[j]);
        dfs(nums, len, res, path, j+1);
        path.pop_back();
    }
}

vector<vector<int>> subsets(vector<int>& nums) {
    int len = nums.size();
    vector<vector<int>> res;
    vector<int> path;
    dfs(nums, len, res, path, 0);
    return res;
}
```

##90. Subsets II
###题目：
Given a collection of integers that might contain duplicates, ***nums***, return all possible subsets.

Note: The solution set must not contain duplicate subsets.

For example,
If ***nums*** = **[1,2,2]**, a solution is:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```
###思路：
首先是子集不仅包含相邻的，还包含不相邻的所有集合，所以在回溯函数内部需要遍历。

其次这里包含有重复元素，又要求结果不出现重复，那么需要判断重复。

**去掉重复的方法：**

首先对数组排序，然后再回溯程序中，与前一个不同的再加入，继续回溯，反之不加入。
###代码：

```
void back(vector<int>& nums, int i, vector<int> path, const int len, vector<vector<int>> &res) {
    res.push_back(path);
    for(int j = i; j < len; ++j) {
        if(j == i || nums[j] != nums[j-1]) {
            path.push_back(nums[j]);
            back(nums, j+1, path, len, res);
            path.pop_back();
        }
        
    }
}

vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int len = nums.size();
    vector<vector<int>> res;
    vector<int> path;
    back(nums, 0, path, len, res);
    return res;
}
```

##77. Combinations
###题目：
Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

For example,

If n = 4 and k = 2, a solution is:

```
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```
###思路：

###代码：

```
void dfs(vector<vector<int>> &res, vector<int> path, const int n, int i, int k) {
    if (k == 0) {
        res.push_back(path);
        return;
    }
    for (int j = i; j <= n; ++j) {
        path.push_back(j);
        dfs(res, path, n, j+1, k-1);
        path.pop_back();
    }
}

vector<vector<int>> combine(int n, int k) {
    vector<int> path;
    vector<vector<int>> res;
    dfs(res, path, n, 1, k);
    return res;
}
```

##79. Word Search
###题目：
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

For example,

Given **board** =

```
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```
word = **"ABCCED"**, -> returns **true**,
word = **"SEE"**, -> returns **true**,
word = **"ABCB"**, -> returns **false**.
###思路：
dfs 
###代码：

```
bool dfs(vector<vector<char>>& board, string &word, int num, int i, int j, int R, int C, int W) {
    char now = board[i][j];
    bool yes = false;
    if(now != word[num]) return false;
    if(num == W-1) return true;
    board[i][j] = '#';
    if(i > 0) yes = dfs(board, word, num+1, i-1, j, R, C, W);
    if(!yes && i < R-1) yes = dfs(board, word, num+1, i+1, j, R, C, W);
    if(!yes && j < C-1) yes = dfs(board, word, num+1, i, j+1, R, C, W);
    if(!yes && j > 0) yes = dfs(board, word, num+1, i, j-1, R, C, W);
    board[i][j] = now;
    return yes;
   
}

bool exist(vector<vector<char>>& board, string word) {
    int R = board.size(), C = board[0].size(), W = word.size();
    int i, j;
    if(R && C && W) {
        for (i = 0; i < R; ++i) 
            for (j = 0; j < C; ++j) 
                if(dfs(board, word, 0, i, j, R, C, W))
                    return true;
    }
    
    return false;
}
```

##89. Gray Code **
###题目：
The gray code is a binary numeral system where two successive values differ in only one bit.

Given a non-negative integer n representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.

For example, given *n* = 2, return **[0,1,3,2]**. Its gray code sequence is:

```
00 - 0
01 - 1
11 - 3
10 - 2
```
**Note:**

For a given n, a gray code sequence is not uniquely defined.

For example, **[0,2,3,1]** is also a valid gray code sequence according to the above definition.

For now, the judge is able to judge based on one instance of gray code sequence. Sorry about that.
###思路：
方法1：

格雷码计算公式

方法2：

DFS方法
###代码：

```
//方法1：
vector<int> grayCode(int n) {
    vector<int> res;
    for (int i = 0; i < 1 << n; i++) {
        res.push_back(i ^ i >> 1);
    }
    return res;
}

//方法2：
void findC(vector<int> &res, vector<int>& visited, int num, int n) {
    if(visited[num]) {
        return;
    }
    res.push_back(num);
    visited[num] = true;
    for(int i = 0; i < n; i++) {
        int c = 1 << i;
        findC(res, visited, num^c, n);
    }
}
vector<int> grayCode(int n) {
    if(n == 0) return {0};
    int c = 1 << n;
    vector<int> visited(c, false);
    vector<int> res;
    findC(res, visited, 0, n);
    return res;
}
```

##93. Restore IP Addresses
###题目：
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:

Given **"25525511135"**,

return **["255.255.11.135", "255.255.111.35"]**. (Order does not matter)
###思路：

###代码：

```
void back(vector<string> &res, vector<string> path, int i, int num, const int len, string &s) {
    if(num == 4 && i == len) {
        string nowR;
        for(string all : path) {
            nowR += all;
            nowR += ".";
        }
        nowR = nowR.substr(0, nowR.size()-1);
        res.push_back(nowR);
    }
    else if(num == 4 && i != len || num != 4 && i == len){
        return;
    }else {
        for(int j = 1; j <= 3; ++j) {
            if(j != 1 && s[i] == '0') continue;
            if(i+j-1 < len) {
                string nn = s.substr(i, j);
                int now = stoi(nn);
                if(now >= 0 && now <= 255) {
                    path.push_back(nn);
                    back(res, path, i+j, num+1, len, s);
                    path.pop_back();
                }
            }
        }
    }
}

vector<string> restoreIpAddresses(string s) {
    int len = s.size();
    vector<string> res;
    vector<string> path;
    if(len < 4 || len > 12) return res;
    back(res, path, 0, 0, len, s);
    return res;
}

//用三个循环
bool isValid(string s) {
    if (atoi(s.c_str()) > 255 || s[0] == '0' && s.size() > 1)
        return false;
    return true;
}    
public:
vector<string> restoreIpAddresses(string s) {
    vector<string> res;
    int len = s.length();
    for (int i = 1; i < 4 && i < len-2; i++) {
        for (int j = i + 1; j < i + 4 && j < len - 1; j++) {
            for (int k = j + 1; k < j + 4 && k < len; k++) {
                if (len - k > 3) continue;
                string s1 = s.substr(0,i), s2 = s.substr(i,j-i), s3 = s.substr(j,k-j), s4 = s.substr(k,len-k);
                if (isValid(s1) && isValid(s2) && isValid(s3) && isValid(s4))
                    res.push_back(s1+'.'+s2+'.'+s3+'.'+s4);
            }
        }
    }
    return res;
}

```

##127. Word Ladder
###题目：
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:

* Only one letter can be changed at a time

* Each intermediate word must exist in the word list

For example,

Given:

*beginWord* = **"hit"**

*endWord* = **"cog"**

*wordList* = **["hot","dot","dog","lot","log"]**

As one shortest transformation is **"hit" -> "hot" -> "dot" -> "dog" -> "cog"**,

return its length **5**.

Note:

* Return 0 if there is no such transformation sequence.
* All words have the same length.
* All words contain only lowercase alphabetic characters.

###思路：
首先注意读题，和下面的题有点区别，开始单词本身的距离是1

只需要下一个题的BFS即可。

BFS的时候需要用回溯，要尝试所有替换方式，试过一种之后要还原。
###代码：

```
struct NODE {
    string s;
    int t;
    NODE() {}
    NODE(string _s, int _t) : s(_s), t(_t) {}
};

class Solution {
public:
    int ladderLength(string beginWord, string endWord, unordered_set<string>& wordList) {
        wordList.insert(beginWord);
        wordList.insert(endWord);
        unordered_map<string, int> hash;
        hash[beginWord] = 1;
        queue<NODE> Q;
        Q.push(NODE(beginWord, 1));
        int len = beginWord.size();
        while(!Q.empty()) {
            NODE now = Q.front();
            string nows = now.s;
            int nowt = now.t;
            Q.pop();
            if(nows == endWord) {
                return nowt;
            }
            for(int i = 0; i < len; ++i) {
                for(char j = 'a'; j <= 'z'; ++j) {
                    if(nows[i] == j) continue;
                    nows[i] = j;
                    if(wordList.find(nows) == wordList.end() || hash.find(nows) != hash.end()) {
                        nows[i] = now.s[i];
                        continue;
                    }
                    Q.push(NODE(nows, nowt+1));
                    hash[nows] = nowt+1;
                    nows[i] = now.s[i];
                }
            }
        }
        return 0;
    }
};
```

##126. Word Ladder II
###题目：
Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:

1. Only one letter can be changed at a time
2. Each intermediate word must exist in the word list

For example,

Given:

*beginWord* = **"hit"**

*endWord* = **"cog"**

*wordList* = **["hot","dot","dog","lot","log"]**

Return

```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```
**Note:**

* All words have the same length.
* All words contain only lowercase alphabetic characters.

###思路：
1、需要的数据结构：

unordered_map<string, int> dis：表示string距离初始字符串的距离

vector<vector<string>> level：level[i]存储距离初始字符串距离为i的字典中的字符串

vector<vector<string>> res:存储答案

2、过程：

首先进行BFS，得到dis，然后对dis进行处理得到level;

下面进行dfs，从dis[endWord]层开始遍历level数组，要求数组中元素和上一层元素相差一个字母。
###代码：

```
struct NODE {
    string s;
    int t;
    NODE() {}
    NODE(string _s, int _t) : s(_s), t(_t) {}
};

class Solution {
public:
    void BFS(string beginWord, unordered_set<string> &wordList) {
        queue<NODE> Q;
        dis[beginWord] = 0;
        Q.push(NODE(beginWord, 0));
        int len = beginWord.size();
        while(!Q.empty()) {
            NODE tmp = Q.front();
            Q.pop();
            string nowW = tmp.s;
            int nowT = tmp.t;
            for(int i = 0; i < len; ++i) {
                for(int j = 0; j < 26; ++j) {
                    if(nowW[i] - 'a' == j) continue;
                    nowW[i] = j + 'a';
                    if(wordList.find(nowW) != wordList.end()) {
                        if(dis.find(nowW) != dis.end()) {
                            nowW[i] = tmp.s[i];
                            continue;
                        }
                        dis[nowW] = nowT + 1;
                        Q.push(NODE(nowW, nowT+1));
                    }
                    nowW[i] = tmp.s[i];
                }
            }
        }
    }

    void dfs(int l, vector<string> path, string now, const int len) {
        if(l == 0) {
            reverse(path.begin(), path.end());
            res.push_back(path);
            return;
        }
        
        for(string can : level[l-1]) {
            int diff = 0;
            for(int i = 0; i < len; ++i) {
                if(now[i] != can[i]) diff++;
            }
            if(diff != 1) continue;
            path.push_back(can);
            dfs(l-1, path, can, len);
            path.pop_back();
        }
    }

    vector<vector<string>> findLadders(string beginWord, string endWord, unordered_set<string> &wordList) {
        res.clear();
        dis.clear();
        wordList.insert(beginWord);
        wordList.insert(endWord);
        
        BFS(beginWord, wordList);
        
        if (dis.find(endWord) == dis.end()) return res;
        level = vector<vector<string>>(dis[endWord] + 1);
        for (auto w : wordList) {
            if (dis.find(w) == dis.end() || dis[w] > dis[endWord]) continue;
            level[dis[w]].push_back(w);
        }
        int allL = dis[endWord];
        int len = beginWord.size();
        
        vector<string> path;
        path.push_back(endWord);
        dfs(allL, path, endWord, len);
        return res;
    }
private:
    unordered_map<string, int> dis;
    vector<vector<string>> level;
    vector<vector<string>> res;
};
```

##131. Palindrome Partitioning
###题目：
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

For example, given s = **"aab"**,

Return

```
[
  ["aa","b"],
  ["a","a","b"]
]
```
###思路：
遍历所有划分，保证每次划分都能得到一个回文，需要一个额外的函数来判断。

划分到最后一个字符则得到一种结果。
###代码：

```
bool isPalindrome (string &s, int l, int r) {
    while(l <= r) {
        if(s[l] != s[r]) return false;
        ++l; --r;
    }
    return true;
}
void dfs(string &s, const int len, int index, vector<vector<string>> &res, vector<string> &path) {
    if(index == len) {
        res.push_back(path);
        return;
    }
    for(int r = index; r < len; ++r) {
        if(isPalindrome (s, index, r)) {
            path.push_back(s.substr(index, r-index+1));
            dfs(s, len, r+1, res, path);
            path.pop_back();
        }
    }
}
vector<vector<string>> partition(string s) {
    int len = s.size();
    vector<vector<string>> res;
    if(len == 0) {
        return res;
    }
    vector<string> path;
    dfs(s, len, 0, res, path);
    return res;
}
```
##140. Word Break II
###题目：
Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

For example, given

s = **"catsanddog"**,

dict = **["cat", "cats", "and", "sand", "dog"]**.

A solution is **["cats and dog", "cat sand dog"]**.
###思路：
首先用bool dp[i]数组维护0-len位置前切开是否前面可以有答案。这里数组值一旦变为true即可break,可以提高速度。

然后从后向前进行回溯，回溯使用dp数组，判断从len向前的某些切割组合可以得到答案，成立的判断标准是两个相邻切割中间形成的字符串在字典中。
###代码：

```
class Solution {
public:
    void Dp(string &s, unordered_set<string>& wordDict, const int len) {
        for (int i = 1; i <= len; ++i) {
            for(int j = i-1; j >= 0; --j) {
                string now = s.substr(j, i-j);
                if(dp[j] && wordDict.find(now) != wordDict.end()) {
                    dp[i] = true;
                    break;
                }
            }
        }
    }

    void dfs(string &s, int index, unordered_set<string>& wordDict, vector<string> &path) {
        if(index == 0) {
            int n = path.size();
            string now = "";
            for(int i = n-1; i > 0; --i) {
                now = now + path[i] + " ";
            }
            now += path[0];
            res.push_back(now);
            return;
        }
        for(int i = index-1; i >= 0; --i) {
            string nn = s.substr(i, index-i);
            if(dp[i] && wordDict.find(nn) != wordDict.end()) {
                path.push_back(nn);
                dfs(s, i, wordDict, path);
                path.pop_back();
            }
        }
    }

    vector<string> wordBreak(string s, unordered_set<string>& wordDict) {
        int len = s.size();
        dp = vector<bool>(len+1, false);
        dp[0] = true;
        Dp(s, wordDict, len);
        vector<string> path;
        dfs(s, len, wordDict, path);
        return res;
    }
private:
    vector<bool> dp;
    vector<string> res;
};
```

##357. Count Numbers with Unique Digits
###题目：
Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

**Example:**

Given n = 2, return 91. (The answer should be the total numbers in the range of 0 ≤ x < 100, excluding [11,22,33,44,55,66,77,88,99])

**Hint:**

1. A direct way is to use the backtracking approach.
2. Backtracking should contains three states which are (the current number, number of steps to get that number and a bitmask which represent which number is marked as visited so far in the current number). Start with state (0,0,0) and count all valid number till we reach number of steps equals to 10n.
3. This problem can also be solved using a dynamic programming approach and some knowledge of combinatorics.
4. Let f(k) = count of numbers with unique digits with length equals k.
5. f(1) = 10, ..., f(k) = 9 * 9 * 8 * ... (9 - k + 2) [The first factor is 9 because a number cannot start with 0].

###思路：
递推计算得到结果。
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
##401. Binary Watch
###题目：
A binary watch has 4 LEDs on the top which represent the hours **(0-11)**, and the 6 LEDs on the bottom represent the minutes **(0-59)**.

Each LED represents a zero or one, with the least significant bit on the right.

For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

**Example:**

```
Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```
**Note:**

* The order of output does not matter.
* The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
* The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

###思路：
将代表小时和分钟的部分拼起来，初始值所有位都是0，枚举所有num个位数为1的情况，并且计算出符合小时和分钟表示的情况。
###代码：

```
class Solution {
public:
    void dfs(int num, int index, vector<int> &tmpNum) {
        if(num == 0) {
            if(index > 10) return;
            int h = 0;
            for(int i = 0; i < 4; ++i) {
                h = h * 2 + tmpNum[i] % 2;
            }
            int m = 0;
            for(int i = 4; i < 10; ++i) {
                m = m * 2 + tmpNum[i] % 2;
            }
            if(h < 12 && m < 60)
                res.push_back(to_string(h) + ":" + (m < 10 ? "0" : "") + to_string(m));
            return;
        }
        for(int i = index; i < 10; ++i) {
            tmpNum[i] = 1;
            dfs(num-1, i+1, tmpNum);
            tmpNum[i] = 0;
        }
    }
    vector<string> readBinaryWatch(int num) {
        vector<int> tmpNum = vector<int>(10, 0);
        res.clear();
        dfs(num, 0, tmpNum);
        return res;
    }
private:
    vector<string> res;
};
```


