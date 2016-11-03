#String

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
* 只能有一个符号；
* 只要有有效数字，后面由非法字符空开的部分可忽略；
* 如果超出int范围，大可返回INT__MAX，小可返回INT__MIN。

###代码：

```
int myAtoi(string str) {
    int len = str.size();
    string s = "";
    int i = 0;
    while(i < len && str[i] == ' ') ++i;
    for(;i < len; ++i) {
        if(str[i] >= '0' && str[i] <= '9' || str[i] == '+' || str[i] == '-') {
            s += str[i];
        }
        else break;
    }
    len = s.size();
    long long res = 0;
    int sign = 1;
    i = 0;
    while(i < len && (s[i] == '+' || s[i] == '-')) {
        sign = s[i] == '+' ? 1 : -1;
        i++;
    }
    
    if(i > 1) return 0;
    while(i < len) {
        res = res * 10 + s[i]-'0';
        if(res * sign >= INT_MAX) return INT_MAX;
        if(res * sign <= INT_MIN) return INT_MIN;
        i++;
    }
    return res * sign;
    
}
```
##13. Roman to Integer
###题目：
Given a roman numeral, convert it to an integer.

Input is guaranteed to be within the range from 1 to 3999.

###思路：
从左向右，先匹配大的且前缀不同的部分，然后向后移动。
###代码：

```
int romanToInt(string s) {
    const int radix[] = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
    const string symbol[] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
    int res = 0;
    int pos = 0;
    for (int i = 0; i < 13; ++i) {
        while (pos + (int)symbol[i].size() <= (int)s.size() && s.substr(pos, (int)symbol[i].size()) == symbol[i]) {
            res += radix[i];
            pos += (int)symbol[i].size();
        }
    }
    return res;
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