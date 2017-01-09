#String

##3. Longest Substring Without Repeating Characters
###题目：
Given a string, find the length of the longest substring without repeating characters.

Examples:

Given **"abcabcbb"**, the answer is **"abc"**, which the length is 3.

Given **"bbbbb"**, the answer is **"b"**, with the length of 1.

Given **"pwwkew"**, the answer is **"wke"**, with the length of 3. Note that the answer must be a substring, **"pwke"** is a subsequence and not a substring.
###思路：
一个hash记录当前这个字母出现的最右位置。

两个指针，一个指针记录目前没有重复字母的字符串的左端点，一个记录右端点。

遍历右端点，如果右端点的字母最右出现的位置大于左指针，更新左指针，更新结果。
###代码：

```
int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> hash;
    int len = s.size(), res;
    
    int l = -1, r;
    res = 0;
    for(r = 0; r < len; ++r) {
        if(hash[s[r]] > l) {
            l = hash[s[r]]-1;
        }
        hash[s[r]] = r + 1;
        res = max(res, (r-l));
    }
    return res;
}
```

##5. Longest Palindromic Substring
###题目：
Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.

**Example:**

```
Input: "babad"

Output: "bab"

Note: "aba" is also a valid answer.
```
**Example:**

```
Input: "cbbd"

Output: "bb"
```
###思路：
从位置i出发，先向右跳过所有与i相等的字符（因为相邻的相等字符不论个数的奇偶，都可以是回文）。下一次开始的位置i就是右边与i不相等的第一个位置。

然后向左右拓展，如果相对于i对称的位置相等，则左右拓展的指针继续拓展，反之退出，更新答案。

###代码：

```
string longestPalindrome(std::string s) {
    if (s.size() < 2)
        return s;
    int len = s.size(), max_left = 0, max_len = 1, left, right;
    for (int start = 0; start < len && len - start > max_len / 2;) {
        left = right = start;
        while (right < len - 1 && s[right + 1] == s[right])
            ++right;
        start = right + 1;
        while (right < len - 1 && left > 0 && s[right + 1] == s[left - 1]) {
            ++right;
            --left;
        }
        if (max_len < right - left + 1) {
            max_left = left;
            max_len = right - left + 1;
        }
    }
    return s.substr(max_left, max_len);
}
```
##6. ZigZag Conversion
###题目：
The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```
P   A   H   N
A P L S I I G
Y   I   R
```
And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

```
string convert(string text, int nRows);
```
convert("PAYPALISHIRING", 3) should return "PAHNAPLSIIGYIR".

###思路：
主要是题意的理解。
这个做法是将原先的字符串存到相应的
###代码：

```
string convert(string s, int numRows) {
    if(numRows <= 1) return s;
    int len = s.size();
    vector<string> str;
    for(int i = 0; i < numRows; ++i) {
        str.push_back("");
    }
    int row = 0, step = 1;
    for(int i = 0; i < len; ++i) {
        str[row] += s[i];
        if(row == 0) 
            step = 1;
        else if(row == numRows-1) 
            step = -1;
        row += step;
    }
    s.clear();
    for(int i = 0; i < numRows; ++i) {
        s.append(str[i]);
    }
    return s;
}
```

##8. String to Integer (atoi)
###题目：
Implement atoi to convert a string to an integer.

**Hint:** 

Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.

**Notes:** 

It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.
###思路：
注意处理边界条件。

* 开头的空格可以忽略；
* 只能有一个符号，一开始就记录符号，正数为1，负数为-1；
* 只要有有效数字，后面由非法字符空开的部分可忽略；
* 如果超出int范围，大可返回INT__MAX，小可返回INT__MIN。

###代码：

```
int atoi(const char *str) {
    int sign = 1, base = 0, i = 0;
    while (str[i] == ' ') { i++; }
    if (str[i] == '-' || str[i] == '+') {
        sign = 1 - 2 * (str[i++] == '-'); 
    }
    while (str[i] >= '0' && str[i] <= '9') {
        if (base >  INT_MAX / 10 || (base == INT_MAX / 10 && str[i] - '0' > 7)) {
            if (sign == 1) return INT_MAX;
            else return INT_MIN;
        }
        base  = 10 * base + (str[i++] - '0');
    }
    return base * sign;
}
```
##12. Integer to Roman
###题目：
Given an integer, convert it to a roman numeral.

Input is guaranteed to be within the range from 1 to 3999.
###思路：

###代码：

```
string intToRoman(int num) {
    const int radix[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    const string symbol[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    string ans="";
    int i = 0;
    while (i < 13 && num > 0) {
        int cnt = num/radix[i];
        num %= radix[i];
        while (cnt--) ans += symbol[i];
        i++;
    }
    return ans;
}
```

##13. Roman to Integer
###题目：
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

###思路：
罗马数字的构造条件从表示的数字
###代码：

```
int romanToInt(string s) {
    unordered_map<char, int> dict = {   {'I', 1},
                                        {'V', 5},
                                        {'X', 10},
                                        {'L', 50},
                                        {'C', 100},
                                        {'D', 500},
                                        {'M', 1000} };
    int sum = dict[s.back()];
    for (int i = s.length() - 2; i >= 0; i--) {
        if (dict[s[i]] < dict[s[i + 1]]) 
            sum -= dict[s[i]];
        else 
            sum += dict[s[i]];
    }
    return sum;
}

```
##14. Longest Common Prefix
###题目：
Write a function to find the longest common prefix string amongst an array of strings.
###思路：
以字符串数组的第一个为标准，后面的字符串与其进行比较；

有长度不够或者某一位不同的，直接返回第一个字符串从开头到正在比较的这一位以前的部分，否则返回第一个字符串。
###代码：

```
string longestCommonPrefix(vector<string>& strs) {
    int len = strs.size();
    if(len == 0) return "";
    for(int i = 0; i < (int)strs[0].size(); ++i) {
        for(int j = 1; j < len; ++j) {
            if(i >= (int)strs[j].size() || strs[j][i] != strs[0][i]) 
                return strs[0].substr(0, i);
        }
    }
    return strs[0];
}
```
##17. Letter Combinations of a Phone Number
###题目：
Given a digit string, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

```
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
```
**Note:**

Although the above answer is in lexicographical order, your answer could be in any order you want.
###思路：
回溯
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

##20. Valid Parentheses
###题目：
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

###思路：
没必要建立map，三种情况注意判断即可。

向stack里面加的条件除了栈空，还有是左括号系列。

###代码：

```
bool isValid(string s) {
    if(s.size() % 2 == 1) return false;
    stack<char> all;
    for(int i = 0; i < s.size(); ++i) {
        if(all.empty() || s[i] == '(' || s[i] == '[' || s[i] == '{') 
            all.push(s[i]);
        else {
            if(s[i] == ']' && all.top() != '[') return false;
            else if(s[i] == ')' && all.top() != '(') return false;
            else if(s[i] == '}' && all.top() != '{') return false;
            else {
                all.pop();
            }
        }
    }
    if(all.empty())
        return true;
    else 
        return false;
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

##28. Implement strStr()
###题目：
Implement strStr().

Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
###思路：
从前向后对比两个串，三个指针，最多遍历长串一次；

一个pre指针指向本次比较的开头位置，另外两个表示两个串正在进行比较的位置；

如果相等，两个指针都向后移动，

否则指向needle的指针指向0， 指向haystack的指针指向pre的下一个,pre更新。
###代码：

```
int strStr(string haystack, string needle) {
    int lenh = haystack.size(), lenn = needle.size();
    if(lenn > lenh) return -1;
    int i = 0, j = 0, pre = 0;
    while(i < lenh && j < lenn) {
        if(haystack[i] == needle[j]) {
            i++;
            j++;
        }else {
            j = 0;
            i = pre + 1;
            pre = i;
        }
    }
    if(j == lenn) return i - lenn;
    else return -1;
}
```

##38. Count and Say
###题目：
The count-and-say sequence is the sequence of integers beginning as follows:

1, 11, 21, 1211, 111221, ...

* 1 is read off as "one 1" or 11.
* 11 is read off as "two 1s" or 21.
* 21 is read off as "one 2, then one 1" or 1211.

Given an integer n, generate the nth sequence.

Note: The sequence of integers will be represented as a string.
###思路：
递归得先得到n-1的结果，然后开始模拟生成n的结果。

也可以迭代进行。
###代码：

```
string countAndSay(int n) {
    if(n <= 1) return "1";
    string n1 = countAndSay(n-1);
    int len = n1.size();
    string res = "";
    int num = 1;
    char pre = n1[0];
    for(int i = 1; i < len; ++i) {
        if(n1[i] != pre) {
            res = res + to_string(num) + pre;
            num = 1;
            pre = n1[i];
        }else {
            num++;
        }
    }
    res = res + to_string(num) + pre;
    return res;
}
```

##43. Multiply Strings**
###题目：
Given two numbers represented as strings, return multiplication of the numbers as a string.

**Note:**

* The numbers can be arbitrarily large and are non-negative.
* Converting the input string to integer is NOT allowed.
* You should NOT use internal library such as BigInteger.

###思路：
需要熟悉乘法运算法则。

两个数相乘位数最大是两个数位数的加和。

如果大数在左，那么数a和数b相乘，a的i位和b的j位相乘的结果，会加在结果的i+j+1位。

善于利用sum.find_first_not_of(), substr()函数。
###代码：

```
class Solution {
public:
    string multiply(string num1, string num2) {
        string sum(num1.size() + num2.size(), '0');
        for (int i = num1.size() - 1; i >= 0; i--) {
            int carry = 0;
            for (int j = num2.size() - 1; j >= 0; j--) {
                int tmp = (sum[i + j + 1] - '0') + (num1[i] - '0')*(num2[j] - '0') + carry;
                sum[i + j + 1] = tmp % 10 + '0';
                carry = tmp / 10;
            }
            sum[i] += carry;
        }
        size_t startpos = sum.find_first_not_of("0");
        if (string::npos != startpos) {
            return sum.substr(startpos);
        }
        return "0";
    }
};
```

##58. Length of Last Word
###题目：
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: 

A word is defined as a character sequence consists of non-space characters only.

For example, 

Given s = "Hello World",
return 5.

###思路：
从后向前看，忽略前面的空格，从第一个不是空格的字母开始计数，直到再遇到空格返回答案
###代码：

```
int lengthOfLastWord(string s) {
    int len = s.size();
    int i = len-1, res = 0, yes = 0;
    while(i >= 0) {
        if(yes == 0) {
            if(s[i] != ' ') {
                yes = 1;
                res++;
            }
        }else {
            if(s[i] == ' ') return res;
            else res++;
        }
        i--;
    }
    return res;
}
```
##67. Add Binary
###题目：
Given two binary strings, return their sum (also a binary string).

For example,

a = "11"

b = "1"

Return "100".
###思路：
注意：

1、从后向前加可以不需要翻转字符串；

2、利用三目运算符可以省下很多if else；

3、进位数字可以和中间的加和结果进行合并。
###代码：

```
string addBinary(string a, string b) {
    int i = a.size() - 1, j = b.size() - 1, c = 0;
    string s;
    
    while (i >= 0 || j >= 0 || c) {
        c += i >= 0 ? a[i--] - '0' : 0;
        c += j >= 0 ? b[j--] - '0' : 0;
        s = char(c % 2 + '0') + s;
        c >>= 1;
    }
    return s;
}
```

##68. Text Justification
###题目：
Given an array of words and a length *L*, format the text such that each line has exactly *L* characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly *L* characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line do not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left justified and no extra space is inserted between words.

For example,

**words: ["This", "is", "an", "example", "of", "text", "justification."]**

**L: 16.**

Return the formatted lines as:

```
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```
**Note:** Each word is guaranteed not to exceed *L* in length.
###思路：
思路很简单，就是按照题目要求只要可以排列开就在一行排尽量多的单词。依次将所有单词排入

主要是空格个数的计算。

在排列过程中，通过计算字母个数，确定一行的起始和终止单词，然后确定每个单词中间空格的数目，由于不一定可能整除，前几个间隙空格数可能比后面多一个。

这里有2个例外：

1、最后一行前面每两个单词之间只有一个空格；

2、前面的如果一行只能放下一个单词，单词靠左放置，后面多余出来的空间放空格。
###代码：

```
vector<string> fullJustify(vector<string>& words, int maxWidth) {
    int len = words.size();
    vector<string> res;
    if (!len) return res;
    int i = 0;
    while (i < len) {
        int start = i, end = i;
        int num = words[i++].size();
        while (i < len && num + (int)words[i].size() + 1 <= maxWidth) {
            num = num + (int)words[i].size() + 1;
            i++;
        }
        end = i - 1;
        int gaps = end - start;
        int ava = 0, re = 0;
        if (gaps) {
            ava = (maxWidth - num) / gaps;
            re = (maxWidth - num) % gaps;
        }
        string now = "";
        for (int j = start; j < end; ++j) {
            now += words[j];
            if (end == len-1) now.append(1, ' ');
            else {
                now.append(ava + 1, ' ');
                if (re) {
                    now.append(1, ' ');
                    --re;
                }
            }
        }
        now += words[end];
        if ((int)now.size() < maxWidth) now.append(maxWidth-(int)now.size(), ' ');
        res.push_back(now);
    }
    return res;
}
```



##71. Simplify Path
###题目：
Given an absolute path for a file (Unix-style), simplify it.

For example,

**path** = "/home/", => "/home"

**path** = "/a/./b/../../c/", => "/c"

**Corner Cases:**

* Did you consider the case where path = "/../"?

  In this case, you should return "/".

* Another corner case is the path might contain multiple slashes '/' together, such as "/home//foo/".

	In this case, you should ignore redundant slashes and return "/home/foo".

###思路：
使用stringstream,根据/划分，然后用一个vector记录当前有的路径；

一个一个处理stringstream里面的内容，“”和“.”跳过，“..”且前面有路径，将前面的路径pop，如果不是“..”，放入路径。

最后处理如果没有路径，返回“/”。
###代码：

```
string simplifyPath(string path) {
    string res, tmp;
    vector<string> stk;
    stringstream ss(path);
    while (getline(ss, tmp, '/')) {
        if (tmp == "" || tmp == ".") continue;
        if (tmp == ".." && !stk.empty()) stk.pop_back();
        else if (tmp != "..") stk.push_back(tmp);
    }
    for (auto str:stk) res += "/"+str;
    return res.empty() ? "/" : res;
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

##93. Restore IP Addresses *
###题目：
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

For example:

Given **"25525511135"**,

return **["255.255.11.135", "255.255.111.35"]**. (Order does not matter)
###思路：
回溯的思想。

**注意：**

"010010"

["0.10.0.10","0.100.1.0"]
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

##151. Reverse Words in a String
###题目：
Given an input string, reverse the string word by word.

For example,

Given s = "the sky is blue",

return "blue is sky the".
###思路：
多个空格只保留一个。

注意substr的用法，s.substr(开始的位置，长度)。

注意使用stringstream
###代码：

```
void reverseWords(string &s) {
    string ans = "", temp;
    stringstream sin(s);
    while(sin >> temp) {
        if(ans != "") {
            ans = temp + " " + ans;
        } else {
            ans = temp;
        }
    }
    s = ans;
}

```
##165. Compare Version Numbers
###题目：
Compare two version numbers version1 and version2.
If version1 > version2 return 1, if version1 < version2 return -1, otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.
The . character does not represent a decimal point and is used to separate number sequences.

For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

Here is an example of version numbers ordering:

```
0.1 < 1.1 < 1.2 < 13.37
```
###思路：
注意：

不是位数多就大，还是要看前面的位数上面数字比较大算大。

getline()， istringstream()可以按照某个字符划分

```
while(getline(ss1, tmp, '.')){
	v1.push_back(tmp);
}
```
###代码：

```
vector<int> sTv(string v) {
    int len = v.size();
    vector<int> res;
    int now = 0;
    for(int i = 0; i < len; ++i) {
        if(v[i] == '.') {
            res.push_back(now);
            now = 0;
        }else {
            now = now * 10 + v[i]-'0';
        }
    }
    return res;
}
int compareVersion(string version1, string version2) {
    vector<int> v1 = sTv(version1 + '.');
    vector<int> v2 = sTv(version2 + '.');
    int len1 = v1.size(), len2 = v2.size();
    int mini = min(len1, len2);
    for(int i = 0; i < mini; ++i) {
        len1--;
        len2--;
        if(v1[i] > v2[i]) return 1;
        else if(v1[i] < v2[i]) return -1;
    }
    while(len1) {
        if(v1[v1.size() - len1] > 0) return 1;
        len1--;
    }
    while(len2) {
        if(v2[v2.size() - len2] > 0) return -1;
        len2--;
    }
    return 0;
}
```

##227. Basic Calculator II
###题目：
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, **+, -, *, /** operators and empty spaces. The integer division should truncate toward zero.

You may assume that the given expression is always valid.

Some examples:

```
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
```
**Note:** Do not use the **eval** built-in library function.

###思路：
方法1：

不需要存储符号。

数字的结果栈中存储的是每一步的中间结果，如果是“+” 或者 “-”，那么记录的就是当前数字的相反数或者其本身；

如果是“*” 或者 “/”，那么就是用数字栈顶和当前数字做运算，然后更换栈顶。

最后将栈中的数字相加即可。

方法2：

使用stringstream
###代码：

```
int calculate(string s) {
    stack<int> myStack;
    char sign = '+';
    int res = 0, tmp = 0;
    for (int i = 0; i < s.size(); i++) {
        if (isdigit(s[i]))
            tmp = 10 * tmp + s[i]-'0';
        if (!isdigit(s[i]) && !isspace(s[i]) || i == s.size()-1) {
            if (sign == '-')
                myStack.push(-tmp);
            else if (sign == '+')
                myStack.push(tmp);
            else {
                int num;
                if (sign == '*' )
                    num = myStack.top()*tmp;
                else
                    num = myStack.top()/tmp;
                myStack.pop();
                myStack.push(num);
            } 
            sign = s[i];
            tmp = 0;
        }
    }
    while (!myStack.empty()) {
        res += myStack.top();
        myStack.pop();
    }
    return res;
}

//使用stringstream
int calculate(string s) {
    stringstream ss(s);
    int tmp = 0, res = 0, total = 0;
    char op;
    ss >> tmp;
    while (ss >> op){
        if(op == '*' || op == '/'){
            int n;
            ss >> n;
            if(op == '*') {
                tmp = n * tmp;
            }
            else{
                tmp = tmp / n;
            }
        }
        if(op == '+' || op == '-'){
            total += tmp;
            ss >> tmp;
            if(op == '-')
                tmp = 0 - tmp;
        }
    }
    return total+tmp;
}
```

##383. Ransom Note
###题目：
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

Note:

You may assume that both strings contain only lowercase letters.

```
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
```
###思路：
比较简单。
###代码：

```
bool canConstruct(string ransomNote, string magazine) {
    unordered_map<char, int> all;
    int lenm = magazine.size(), lenr = ransomNote.size();
    if(lenr > lenm) return false;
    for(auto i : magazine) {
        all[i]++;
    }
    for(auto i : ransomNote) {
        if(all[i] == 0) return false;
        else all[i]--;
    }
    return true;
}
```

##385. Mini Parser
###题目：
Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Note:** You may assume that the string is well-formed:

* String is non-empty.
* String does not contain white spaces.
* String contains only digits 0-9, [, - ,, ].

**Example 1:**

```
Given s = "324",

You should return a NestedInteger object which contains a single integer 324.
```
**Example 2:**

```
Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.
```

###思路：
遇到**数字**，插入到栈顶的节点；

碰到**“[”** ：新建一个节点；

碰到**“]”** ：将栈顶的节点记录并且pop出来，如果栈为空，那么这个节点就是答案，否则将这个节点插入到栈顶。

###代码：

```
//使用stack的写法
NestedInteger deserialize(string s) {
    stack<NestedInteger> all;
    int len = s.size();
    int i = 0;
    NestedInteger res;
    while(i < len) {
        if(isdigit(s[i]) || s[i] == '-') {
            int j = i;
            while(isdigit(s[i]) || s[i] == '-') {
                ++i;
            }
            int now = stoi(s.substr(j, i-j));
            if(all.empty()) return NestedInteger(now);
            else all.top().add(NestedInteger(now));
        }else {
            
            if(s[i] == '[') {
                all.push(NestedInteger());
            }
            else if(s[i] == ']') {
                NestedInteger nn = all.top();
                all.pop();
                if(all.empty()) {
                    res = nn;
                    break;
                }
                all.top().add(nn);
            }
            ++i;
        }
    }
    return res;
}

//使用stringstream的写法，很优美
class Solution {
public:
    NestedInteger deserialize(string s) {
        istringstream in(s);
        return deserialize(in);
    }
private:
    NestedInteger deserialize(istringstream &in) {
        int number;
        if (in >> number)
            return NestedInteger(number);
        in.clear();
        in.get();
        NestedInteger list;
        while (in.peek() != ']') {
            list.add(deserialize(in));
            if (in.peek() == ',')
                in.get();
        }
        in.get();
        return list;
    }
};
```

##434. Number of Segments in a String
###题目：
Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any **non-printable** characters.

**Example:**

```
Input: "Hello, my name is John"
Output: 5
```
###思路：
注意第一个if的中括号不可以去掉，否则程序会认为 else 是第二个 if 对应
###代码：

```
int countSegments(string s) {
    s = s + " ";
    int res = 0, len = s.size();
    string now = "";
    for (int i = 0; i < len; ++i) 
        if (s[i] == ' ') {
            if (now != "") {
                res++;
                now = "";
            }
        }
        else now = now + s[i];
    return res;
}
```

##459. Repeated Substring Pattern
###题目：
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

**Example 1:**

```
Input: "abab"

Output: True

Explanation: It's the substring "ab" twice.
```
**Example 2:**

```
Input: "aba"

Output: False
```
**Example 3:**

```
Input: "abcabcabcabc"

Output: True

Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
```
###思路：
遍历str长度i，从 1 - len / 2

构造新的字符串，构造方法是将str前i个字符取下接到字符串后面。

如果新构造的字符串与原来的字符串相等，返回true；遍历结束后仍未返回，则返回false。
###代码：

```
//比较慢，但是思路很简单。
bool repeatedSubstringPattern(string str) {
    int len = str.size();
    if(len == 0) return false;
    for(int i = 1; i <= len / 2; ++i) {
        if(len % i == 0) {
            string right = str.substr(i);
            string left = str.substr(0,i);
            if(right + left == str) return true;
        }
    }
    return false;
}
```
