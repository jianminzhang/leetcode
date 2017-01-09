#Stack
##20. Valid Parentheses
###题目：
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.
###思路：
用栈来存储左括号，遇到右括号，如果栈空或者栈顶和这个右括号不匹配，则返回false;

如果右括号处理完，栈非空，返回false；否则返回true。

这个题比较简单，但是如果这里是只有一种括号的，那么这里不需要用栈解决，只需要
###代码：

```
bool isValid(string s) {
    unordered_map<char, char> go;
    go['('] = ')';
    go['{'] = '}';
    go['['] = ']';
    stack<char> all;
    int i = 0, len = s.size();
    if(len <= 0) return true;
    if(len % 2 == 1) return false;
    while(i < len) {
        if(all.empty() || s[i] == '(' || s[i] == '[' || s[i] == '{') {
            all.push(s[i]);
        }
        else {
            if(go[all.top()] != s[i]) return false;
            else {
                all.pop();
            }
        }
        ++i;
    }
    if(all.size() > 0) return false;
    return true;
}
```

##42. Trapping Rain Water *
###题目：
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example, 

Given **[0,1,0,2,1,0,1,3,2,1,2,1]**, return **6**.
###思路：
two pointer的思路

见代码
###代码：

```
int trap(vector<int>& height) {
    int len = height.size();
    int res = 0;
    int maxl = 0, maxr = 0;
    int l = 0, r = len - 1;
    while (l < r) 
        if (height[l] <= height[r]) {
            if (height[l] > maxl) maxl = height[l];
            else res += (maxl - height[l]);
            l++;
        }else {
            if (height[r] > maxr) maxr = height[r];
            else res += (maxr - height[r]);
            r--;
        }
    return res;
}
```

##150. Evaluate Reverse Polish Notation
###题目：
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

Some examples:

```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```
###思路：
运用栈存储运算数字

当遇到不是运算符号时，将其转化成数字入栈；

当遇到运算符号时，从栈中取出相应的运算数字，再将计算结果入栈。
###代码：

```
int evalRPN(vector<string>& tokens) {
    int len = tokens.size();
    if(len == 0) return 0;
    stack<int> operand;
    for (int i = 0; i < len; ++i) 
        if (tokens[i] != "+" && tokens[i] != "-" && tokens[i] != "*" && tokens[i] != "/") {
            operand.push(stoi(tokens[i]));
        }
        else {
            int a,b;
            a = operand.top();
            operand.pop();
            b = operand.top();
            operand.pop();
            if (tokens[i] == "+") operand.push(a + b);
            else if (tokens[i] == "-") operand.push(b - a);
            else if (tokens[i] == "*") operand.push(a * b);
            else if (tokens[i] == "/") operand.push(b / a);
        }
    
    return operand.top();
}
```

##224. Basic Calculator **
###题目：
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

You may assume that the given expression is always valid.

Some examples:

```
"1 + 1" = 2
" 2-1 + 2 " = 3
"(1+(4+5+2)-3)+(6+8)" = 23
```
**Note: Do not** use the **eval** built-in library function.
###思路：
这个题思路还是比较巧妙的。

首先括号中的整体的符号是取决于整个括号外面的符号 和 这个括号处于的这一层的符号。

例如：1-(5-(6+7)) 6所处的括号内部的所有的符号是 -1 * -1 = 1.

还是一个一个数处理，注意符号处理即可。
###代码：

```
int calculate(string s) {
    stack<int> signs;
    int sign = 1, num = 0, res = 0, len = s.size();
    signs.push(1);
    for (int i = 0; i < len; ++i) {
        char c = s[i];
        if (c == ' ') continue;
        if (c >= '0' && c <= '9') num = num * 10 + c - '0';
        else if (c == '+' || c == '-') {
            res = res + num * sign * signs.top();
            num = 0;
            sign = (c == '+' ? 1 : -1);
        }
        else if (c == '(') {
            signs.push(sign * signs.top());
            sign = 1;
        }
        else {
            res = res + num * sign * signs.top();
            num = 0;
            sign = 1;
            signs.pop();
        }
    }
    if (num) res = res + num * sign * signs.top();
    return res;
}
```
##316. Remove Duplicate Letters *
###题目：
Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

**Example:**

Given **"bcabc"**
Return **"abc"**

Given **"cbacdcbc"**
Return **"acdb"**

###思路：
用string实现栈来存储结果。

num数组记录字母还剩下的次数，yes代表字母是否在结果中已经包含。

如果结果中已经包含当前处理的字母，那么continue。

如果当前处理的字母比结果的末尾要小，且结果的末尾字母剩下的次数不是0，则将末尾字母弹出，直到当前字母大于结尾字母或者结尾字母已经是最后一次出现。

res开始是"0"，即栈顶存储一个最小的部分，保证后面的字母都可以加入，最后返回res.substr(1)即可
###代码：

```
string removeDuplicateLetters(string s) {
    vector<int> num(256, 0);
    vector<bool> yes(256, false);
    string res = "0";
    int len = s.size();
    for (char c : s) num[c]++;
    for (char c : s) {
        num[c]--;
        if (yes[c]) continue;
        while (c < res.back() && num[res.back()]) {
            yes[res.back()] = false;
            res.pop_back();
        }
        res += c;
        yes[c] = true;
    }
    return res.substr(1);
}
```

##331. Verify Preorder Serialization of a Binary Tree
###题目：
One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as ```#```.

```
     _9_
    /   \
   3     2
  / \   / \
 4   1  #  6
/ \ / \   / \
# # # #   # #
```
For example, the above binary tree can be serialized to the string ```"9,3,4,#,#,1,#,#,2,#,6,#,#"```, where ```#``` represents a null node.

Given a string of comma separated values, verify whether it is a correct preorder traversal serialization of a binary tree. Find an algorithm without reconstructing the tree.

Each comma separated value in the string must be either an integer or a character ```'#'``` representing ```null``` pointer.

You may assume that the input format is always valid, for example it could never contain two consecutive commas such as **"1,,3"**.

**Example 1:**

"9,3,4,#,#,1,#,#,2,#,6,#,#"

Return true

**Example 2:**

"1,#"

Return false

**Example 3:**

"9,#,#,1"

Return false
###思路：
字符串如果是空，返回false；如果只有一个空节点“#”，返回true。

栈中存储的是目前节点可能的父节点的子节点个数。

如果当前处理的是“#”，栈为空，返回false；

否则如果栈顶元素为2，说明栈顶的父节点的子节点已经满，那么就弹出栈顶，直至出现不是2的栈顶，栈顶加1，说明将当前的节点作为此节点的下一个子节点。

如果当前节点不是“#”，那么入栈0，说明插入了一个新的父节点，且还没有子节点。
###代码：

```
bool isValidSerialization(string preorder) {
    if(preorder == "") return false;
    if(preorder == "#") return true;
    string tmp;
    stringstream ss(preorder);
    stack<int> all;
    while (getline(ss, tmp, ',')) {
        if (tmp == "#" && all.empty()) return false;
        
        if(!all.empty()) {
            while(all.top() == 2) {
                all.pop();
                if (all.empty()) return false;
            }
            all.top()++;
        }
        
        if (tmp != "#") all.push(0);
    }
    while (!all.empty()) {
        if (all.top() < 2) return false;
        all.pop();
    }
    return true;
}
```

##394. Decode String
###题目：
Given an encoded string, return it's decoded string.

The encoding rule is: **k[encoded_string]**, where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, *k*. For example, there won't be input like **3a** or **2[4]**.

**Examples:**

```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```
###思路：

###代码：

##402. Remove K Digits
###题目：
Given a non-negative integer *num* represented as a string, remove *k* digits from the number so that the new number is the smallest possible.

**Note:**

* The length of *num* is less than 10002 and will be ≥ k.
* The given num does not contain any leading zero.

**Example 1:**

```
Input: num = "1432219", k = 3
Output: "1219"
Explanation: Remove the three digits 4, 3, and 2 to form the new number 1219 which is the smallest.
```
**Example 2:**

```
Input: num = "10200", k = 1
Output: "200"
Explanation: Remove the leading 1 and the number is 200. Note that the output must not contain leading zeroes.
```
**Example 3:**

```
Input: num = "10", k = 2
Output: "0"
Explanation: Remove all the digits from the number and it is left with nothing which is 0.
```
###思路：
可以用res来表示结果

去掉原来字符串中的某k个，那么我们可以理解为依次放入结果过程中，是否被替换，被替换一次，那么等于去掉了一位。

依次从左向右处理num中的字符，如果res为空，res加上num[i]，

否则，while循环判断是否满足```num[i] < res.back() && k > 0```,满足的话res.pop_back()，且k--，跳出循环后res += num[i]。

处理开头是‘0’的情况，要找到第一个不是0的位置，这里要注意处理剩下的数字都是0的情况；

最后有可能去掉的字母全部是后面的，那么直接截断到应该要输出的长度。
###代码：

```
string removeKdigits(string num, int k) {
    string res;
    int len = num.size(), resLen = len - k;
    if (k == len) return "0";
    for (int i = 0; i < len; ++i) {
        if (res.empty()) res += num[i];
        else {
            while (res.size() > 0 && num[i] < res.back() && k > 0) {
                res.pop_back();
                k--;
            }
            res += num[i];
        }
    }
    int now = 0;
    while (now < resLen && res[now] == '0') now++;
    return now == resLen ? "0" : res.substr(now, resLen);
}
```

##456. 132 Pattern
###t
