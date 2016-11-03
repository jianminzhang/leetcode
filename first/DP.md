#DP
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

