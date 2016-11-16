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