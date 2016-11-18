#Backtracking
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
##39. Combination Sum
###题目：
Given a set of candidate numbers (**C**) and a target number (**T**), find all unique combinations in **C** where the candidate numbers sums to **T**.

The **same** repeated number may be chosen from **C** unlimited number of times.

**Note:**

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
问题：

这里的path是否传引用

注意这里对于重复的定义，这里每个数字都是不一样的，不论大小是否相同。


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

Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
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

且要求保证不重复
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
void dfs(vector<int> nums, vector<vector<int>> &res, int i, const int len) {
    if(i == len - 1) {
        res.push_back(nums);
        return;
    }
    else {
        for(int j = i; j < len; ++j) {
            swap(nums[i], nums[j]);
            dfs(nums, res, i+1, len);
        }
    }
}

vector<vector<int>> permute(vector<int>& nums) {
    int len = nums.size();
    sort(nums.begin(), nums.end());
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

###代码：

```

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


