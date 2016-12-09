#Locked

##157. Read N Characters Given Read4
###题目：
The API: **int read4(char `*`buf)** reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the **read4** API, implement the function **int read(char `*`buf, int n)** that reads n characters from the file.

Note:

The **read** function will only be called once for each test case.

###思路：
如下代码所示。

###代码：

```
// Forward declaration of the read4 API.
int read4(char *buf);

class Solution {
public:
    /**
     * @param buf Destination buffer
     * @param n   Maximum number of characters to read
     * @return    The number of characters read
     */
    int read(char *buf, int n) {
        int num = 0;
        while(num < n) {
            if(st < ed) {
                buf[num++] = buf4[st++];
            }else {
                int cur = read4(buf4);
                st = 0;
                ed = cur;
                if(cur == 0) return num;
            }
        }
        return num;
    }
private:
    char buf4[4];
    int st = 0, ed = 0;
};
```

##158. Read N Characters Given Read4 II - Call multiple times
###题目：

The API: **int read4(char `*`buf)** reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the **read4** API, implement the function **int read(char `*`buf, int n)** that reads n characters from the file.

Note:
The **read** function may be called multiple times.

###思路：
思路同上
###代码：
代码同上

##159. Longest Substring with At Most Two Distinct Characters
###题目：
Given a string, find the length of the longest substring T that contains at most 2 distinct characters.

For example, Given s = **“eceba”**,

T is "ece" which its length is 3.
###思路：
1、用hash来记录某个字母是不是原来出现过，用两个指针来标记现在处理的位置，l代表处理的第一位，r代表处理的最后一位的后一位。

2、diff来代表现在处理的段落中存在多少不一样的字母；

3、当diff <= 2，移动r指针，并且根据情况修改diff；
当diff > 2，移动l指针，并且根据情况修改diff；最后修改后判断如果满足diff <= 2，更新结果。

###代码：

```
int lengthOfLongestSubstringTwoDistinct(string s) {
    int hash[128] = {0};
    int l = 0, r = 0, n = s.size();
    int diff = 0;
    int res = 0;
    while(r < n) {
        if(diff <= 2) {
            if(hash[s[r]]++ == 0) {
                diff++;
            }
            r++;
        }
        else {
            if(hash[s[l]]-- == 1) diff--;
            l++;
        }
        if(diff <= 2) res = max(res, r-l);
    }
    return res;
}
```

##161. One Edit Distance
###题目：
Given two strings S and T, determine if they are both one edit distance apart.
###思路：
1、首先保证s长度小于等于t，在后面的判断中可以省掉一半的讨论；

2、如果长度相差大于1，那么直接返回false;

3、如果s和t长度相等，那么从左向右计算diff个数，大于1即可返回false;

4、如果 t.size() - s.size() == 1，那么应该有s[i] == t[i+diff]，否则diff++，当diff > 1，返回false。

5、这个题主要是考验写法。
###代码：

```
bool isOneEditDistance(string s, string t) {
    int lens = s.size(), lent = t.size();
    if(abs(lens - lent) > 1) return false;
    int diff = 0;
    if(lens > lent) {
        swap(s, t);
        swap(lens, lent);
    }
    
    if(lens == lent) {
        for(int i = 0; i < lens; ++i) {
            if(s[i] != t[i]) diff++;
        }
        if(diff != 1) return false;
        else return true;
    }else {
        int i = 0;
        while(i < lens) {
            if(s[i] != t[i+diff]) {
                diff++;
                if(diff > 1) break;
            }else {
                i++;
            }
        }
        if(i < lens) return false;
        else return true;
    }
}
```
##163. Missing Ranges
###题目：
Given a sorted integer array where the range of elements are in the inclusive range [lower, upper], return its missing ranges.

For example, given **[0, 1, 3, 50, 75]**, *lower = 0 and upper = 99*, return **["2", "4->49", "51->74", "76->99"]**.
###思路：
用pre记录已经处理的最后一个，遍历nums，如果遍历的这个大于pre+1，说明中间有断点，找到遗失的段落。

###代码：

```
string out(int l, int r) {
    if(l == r) return to_string(l);
    else if(l < r) return to_string(l) + "->" + to_string(r);
    else return "";
}

vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
    int pre = lower - 1;
    vector<string> res;
    int len = nums.size();
    for(int i = 0; i < len; ++i) {
        if(nums[i] > pre+1) {
            res.push_back( out(pre+1, nums[i]-1) );
        }
        pre = nums[i];
    }
    if(pre < upper) {
        res.push_back( out(pre+1, upper) );
    }
    return res;
}
```
##170. Two Sum III - Data structure design
###题目：
Design and implement a TwoSum class. It should support the following operations: **add** and **find**.

**add** - Add the number to an internal data structure.

**find** - Find if there exists any pair of numbers which sum is equal to the value.

For example,

```
add(1); add(3); add(5);
find(4) -> true
find(7) -> false
```
###思路：


###代码：

```
class TwoSum {
public:
    
    TwoSum() {
        hash.clear(); 
    }
    
    // Add the number to an internal data structure.
	void add(int number) {
	    if (hash.find(number) == hash.end()) hash[number] = 1;
	    else hash[number]++;
	}

    // Find if there exists any pair of numbers which sum is equal to the value.
	bool find(int value) {
	    for(auto x : hash) {
	        int tar = value - x.first;
	        
	        if(tar == x.first) {
	            if(hash.find(tar) != hash.end() && hash[tar] > 1) {
	                return true;
	            }
	        }else {
	            if(hash.find(tar) != hash.end()) {
	                return true;
	            }
	        }
	    }
	    return false;
	}

private:
    unordered_map<int, int> hash;
};
// Your TwoSum object will be instantiated and called as such:
// TwoSum twoSum;
// twoSum.add(number);
// twoSum.find(value);
```

##186. Reverse Words in a String II
###题目：
Given an input string, reverse the string word by word. A word is defined as a sequence of non-space characters.

The input string does not contain leading or trailing spaces and the words are always separated by a single space.

For example,

Given s = **"the sky is blue"**,  return **"blue is sky the"**.

Could you do it in-place without allocating extra space?
###思路：
先把整个字符串翻转，然后将每个单词翻转。

###代码：
```
void reverseWords(string &s) {
    reverse(s.begin(), s.end());
    int len = s.size();
    for(int i = 0,j = 0; i < len; i = j) {
        while( j < len && s[j] != ' ') j++;
        reverse(s.begin() + i, s.begin() + j);
        j++;
    }
}
```

##243. Shortest Word Distance
###题目：
Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

For example,
Assume that words = **["practice", "makes", "perfect", "coding", "makes"]**.

Given word1 = **“coding”**, word2 = **“practice”**, return 3.
Given word1 = **"makes"**, word2 = **"coding"**, return 1.

Note:
You may assume that word1 **does not equal to** word2, and word1 and word2 are both in the list.
###思路：
记录每个单词最后出现的位置，遇到一个单词，就用这个位置减去另一个单词最后出现的位置，更新最小距离。

###代码：

```
int shortestDistance(vector<string>& words, string word1, string word2) {
    unordered_map<string, int> hash;
    hash[word1] = -1;
    hash[word2] = -1;
    int len = words.size(), res = len + 1;
    for(int i = 0; i < len; ++i) {
        if(words[i] == word1){
            hash[word1] = i;
            if(hash[word2] != -1) {
                res = min(res, hash[word1] - hash[word2]);
            }
        }else if(words[i] == word2){
            hash[word2] = i;
            if(hash[word1] != -1) {
                res = min(res, hash[word2] - hash[word1]);
            }
        }
    }
    return res;
}
```

##244. Shortest Word Distance II
###题目：
This is a follow up of **Shortest Word Distance**. The only difference is now you are given the list of words and your method will be called repeatedly many times with different parameters. How would you optimize it?

Design a class which receives a list of words in the constructor, and implements a method that takes two words word1 and word2 and return the shortest distance between these two words in the list.

For example,
Assume that words = **["practice", "makes", "perfect", "coding", "makes"]**.

Given word1 = **“coding”**, word2 = **“practice”**, return 3.
Given word1 = **"makes"**, word2 = **"coding"**, return 1.

**Note:**

You may assume that word1 does not equal to word2, and word1 and word2 are both in the list.
###思路：
用map<string, vector<int>>记录每个单词出现的各个位置，两个指针遍历存放两个单词位置的vector。交替进行。
###代码：

```
class WordDistance {
public:
    WordDistance(vector<string>& words) {
        int len = words.size();
        for(int i = 0; i < len; ++i) {
            hash[words[i]].push_back(i);
        }
    }

    int shortest(string word1, string word2) {
        int l = 0, r = 0;
        int len1 = hash[word1].size(),len2 = hash[word2].size(); 
        int res = INT_MAX;
        while(l < len1 && r < len2) {
            res = min(res, abs(hash[word1][l] - hash[word2][r]));
            if(hash[word1][l] < hash[word2][r]) l++;
            else {
                r++;
            }
        }
        
        return res;
    }
    
private:
    unordered_map<string, vector<int>> hash;
};

// Your WordDistance object will be instantiated and called as such:
// WordDistance wordDistance(words);
// wordDistance.shortest("word1", "word2");
// wordDistance.shortest("anotherWord1", "anotherWord2");
```

##245. Shortest Word Distance III
###题目：
This is a follow up of **Shortest Word Distance**. The only difference is now word1 could be the same as word2.

Given a list of words and two words word1 and word2, return the shortest distance between these two words in the list.

word1 and word2 may be the same and they represent two individual words in the list.

For example,
Assume that words = **["practice", "makes", "perfect", "coding", "makes"]**.

Given word1 = **“makes”**, word2 = **“coding”**, return 1.
Given word1 = **"makes"**, word2 = **"makes"**, return 3.

**Note:**

You may assume word1 and word2 are both in the list.
###思路：
思路同题I，只是加了如果两个相等的情况，加了一个trik，使写法简单。
###代码：

```
int shortestWordDistance(vector<string>& words, string word1, string word2) {
    int len = words.size(), res = len + 1;
    int l1 = 3 * len, l2 = -3 * len; //可以省掉一些赘余的写法
    for(int i = 0; i < len; ++i) {
        if(words[i] == word1){
            if(word1 == word2) {   //重点
                res = min(res, abs(i - l1));
            }
            l1 = i;
        }else if(words[i] == word2){
            l2 = i;
        }
        res = min(res, abs(l1 - l2));
    }
    return res;
}
```
##246. Strobogrammatic Number
###题目：
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

For example, the numbers "69", "88", and "818" are all strobogrammatic.
###思路：
两个指针从两边向中间靠拢，如果遇到不符合的情况，返回false; 最中间的部分必须是 0 1 8.

###代码：

```
bool isStrobogrammatic(string num) {
    unordered_map<char, char> hash{{'0','0'},{'1','1'},{'8','8'},{'6','9'},{'9','6'}};
    int len = num.size();
    int l = 0, r = len-1;
    while(l < r) {
        if(num[r] != hash[num[l]]) return false;
        l++; r--;
    }
    if(l == r) {
        if(num[l] != '0' && num[l] != '1' && num[l] != '8') return false;
    }
    return true;
}
```

##247. Strobogrammatic Number II
###题目：
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

For example,
Given n = 2, return **["11","69","88","96"]**.

**Hint:**

Try to use recursion and notice that it should recurse with n - 2 instead of n - 1.
###思路：
递归得先生成n-2的部分，然后循环生成n的部分，在两边加上所有可能，要注意控制开头不能是0。
###代码：

```
class Solution {
public:
    vector<string> findStrobogrammatic(int n) {
        vector<string> res = findS(n, n);
        return res;
    }
    
    vector<string> findS(int n, int k) {
        if(k == 0) return {""};
        else if(k == 1) return {"0","1","8"};
        else {
            vector<string> res;
            for(auto x : findS(n, k-2)) {
                for(auto y : hash) {
                    if(n != k || y.first != '0') {
                        res.push_back(y.first + x + y.second);
                    }
                }
            }
            return res;
        }
    }
private:
    unordered_map<char, char> hash{{'0','0'},{'1','1'},{'8','8'},{'6','9'},{'9','6'}};
};
```

##248. Strobogrammatic Number III
###题目：
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to count the total strobogrammatic numbers that exist in the range of low <= num <= high.

For example,

Given low = "50", high = "100", return 3. Because 69, 88, and 96 are three strobogrammatic numbers.

Note:

Because the range might be a large number, the low and high numbers are represented as string.
###思路：
先确定位数，每一种位数用一个string保存。
###代码：

```
class Solution {
public:
    int strobogrammaticInRange(string low, string high) {
        int l = low.size(), h = high.size();
        int cnt = 0;
        for(int i = l; i <= h; ++i) {
            string tmp(i ,' ');
            strobogrammatic(tmp, cnt, low, high, 0, i-1);
        }
        return cnt;
    }
private:
    unordered_map<char, char> hash{{'0','0'},{'1','1'},{'8','8'},{'6','9'},{'9','6'}};
    
    bool less(string& s, string& t) {
        int m = s.length(), n = t.length(), i;
        if (m != n) return m < n;
        for (i = 0; i < m; i++)
            if (s[i] == t[i])
                continue;
            else
                break;
        return i == m || s[i] < t[i];
    }
    
    void strobogrammatic(string tmp, int &cnt, string &low, string &high, int ll, int rr) {
        if(ll > rr) {
            if((tmp[0] != '0' || tmp.size() == 1) && less(low, tmp) && less(tmp, high)) {
                cnt++;
            }
            return;
        }
        for(auto x : hash) {
            tmp[ll] = x.first;
            tmp[rr] = x.second;
            if(ll == rr && x.first == x.second || ll < rr) {
                strobogrammatic(tmp, cnt, low, high, ll+1, rr-1);
            }
        }
    }
};
```
##249. Group Shifted Strings
###题目：
Given a string, we can "shift" each of its letter to its successive letter, for example: **"abc" -> "bcd"**. We can keep "shifting" which forms the sequence:

```
"abc" -> "bcd" -> ... -> "xyz"
```
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

For example, given: **["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"]**, 
A solution is:

```
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
```
###思路：
用map以a开头的字符串和shift可得到它的各个字符串对应起来。

###代码：

```
string shiftString(string s) {
    int len = s.size();
    if(len == 0) return s;
    int ll = s[0] - 'a';
    for(int i = 0; i < len; ++i) {
        s[i] = (s[i] - ll + 26) % 26;
    }
    return s;
}

vector<vector<string>> groupStrings(vector<string>& strings) {
    unordered_map<string, vector<string>> hash;
    vector<vector<string>> res;
    int len = strings.size();
    for(int i = 0; i < len; ++i) {
        string now = shiftString(strings[i]);
        hash[now].push_back(strings[i]);
    }
    for(auto x : hash) {
        res.push_back(x.second);
    }
    return res;
}
```

##250. Count Univalue Subtrees
###题目：
Given a binary tree, count the number of uni-value subtrees.

A Uni-value subtree means all nodes of the subtree have the same value.

For example:

Given binary tree,

```
              5
             / \
            1   5
           / \   \
          5   5   5
```
return **4**.
###思路：
递归判断。要保证左右子树都是，且左右字数的值和根的值相等。
###代码：

```
int res;
bool isUni(TreeNode* root) {
    if(root == NULL) return true;
    bool fl = isUni(root->left);
    bool fr = isUni(root->right);
    if(!fl || !fr) return false;
    if(root->left && root->val != root->left->val) return false;
    if(root->right && root->val != root->right->val) return false;
    res++;
    return true;
}

int countUnivalSubtrees(TreeNode* root) {
    res = 0;
    isUni(root);
    return res;
}
```
##251. Flatten 2D Vector
###题目：
Implement an iterator to flatten a 2d vector.

For example,

Given 2d vector =

```
[
  [1,2],
  [3],
  [4,5,6]
]
```
By calling next repeatedly until hasNext returns false, the order of elements returned by next should be: [1,2,3,4,5,6].

**Hint:**

* How many variables do you need to keep track?
* Two variables is all you need. Try with x and y.
* Beware of empty rows. It could be the first few rows.
* To write correct code, think about the invariant to maintain. What is it?
* The invariant is x and y must always point to a valid point in the 2d vector. Should you maintain your invariant ahead of time or right when you need it?
* Not sure? Think about how you would implement hasNext(). Which is more complex?
* Common logic in two different places should be refactored into a common method.

**Follow up:**

As an added challenge, try to code it using only iterators in C++ or iterators in Java.
###思路：

###代码：

```
class Vector2D {
public:
    Vector2D(vector<vector<int>>& vec2d) {
        
    }

    int next() {
        
    }

    bool hasNext() {
        
    }
};

/**
 * Your Vector2D object will be instantiated and called as such:
 * Vector2D i(vec2d);
 * while (i.hasNext()) cout << i.next();
 */
```

##252. Meeting Rooms
###题目：
Given an array of meeting time intervals consisting of start and end times **[[s1,e1],[s2,e2],...]** (si < ei), determine if a person could attend all meetings.

For example,

Given **[[0, 30],[5, 10],[15, 20]]**,

return **false**.
###思路：
1、sort中的cmp函数，一定是static, 参数是const；

2、要先按照开始时间排序，开始时间一样的按照结束时间排序；

3、pre记录上一个时间段的结束时间（初始值为第一个时间段的开始时间），遍历所有时间段，如果当前时间段的开始时间小于pre，说明有重叠，返回false；否则遍历完所有返回true。
###代码：

```
static bool cmp(const Interval a, const Interval b) {
    if(a.start == b.start) return a.end < b.end;
    return a.start < b.start;
}
bool canAttendMeetings(vector<Interval>& intervals) {
    sort(intervals.begin(), intervals.end(), cmp);
    
    int len = intervals.size();
    if(len < 1) return true;
    int pre = intervals[0].start;
    for(int i = 0; i < len; ++i) {
        if(intervals[i].start < pre) return false;
        else pre = intervals[i].end;
    }
    return true;
}
```

##253. Meeting Rooms II
###题目：
Given an array of meeting time intervals consisting of start and end times **[[s1,e1],[s2,e2],...]** (si < ei), find the minimum number of conference rooms required.

For example,
Given **[[0, 30],[5, 10],[15, 20]]**,
return **2**.
###思路：
实质上计算的是在同一个时间重叠的时间段最多的个数。
###代码：

```
int minMeetingRooms(vector<Interval>& intervals) {
    map<int, int> all;
    int res = 0, now = 0;
    int len = intervals.size();
    for(int i = 0; i < len; ++i) {
        all[intervals[i].start]++;
        all[intervals[i].end]--;
    }
    for(auto x : all) {
        res = max(res, now + x.second);
        now = now + x.second;
    }
    return res;
}
```

##254. Factor Combinations
###题目：
Numbers can be regarded as product of its factors. For example,

```
8 = 2 x 2 x 2;
  = 2 x 4.
```
Write a function that takes an integer n and return all possible combinations of its factors.

**Note:**

* You may assume that n is always positive.
* Factors should be greater than 1 and less than n.

**Examples:**

input: 1

output: 

```
[]
```
input: 37

output: 

```
[]
```
input: 12

output:

```
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
```
input: 32

output:

```
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]
```
###思路：
递归的思想，注意保证不重复。

###代码：

```
vector<vector<int>> getFactors(int n) {
    int sq = sqrt(n);
    vector<vector<int>> res;
    for(int i = 2; i <= sq; ++i) {
        if(n % i == 0) {
            res.push_back({i, n / i});
            vector<vector<int>> now = getFactors(n / i);
            for(auto x : now) {
                if(i <= x.front()) {   //保证不重复
                    x.insert(x.begin(), i);
                    res.push_back(x);
                }
            }
        }
    }
    return res;
}
```

##255. Verify Preorder Sequence in Binary Search Tree
###题目：
Given an array of numbers, verify whether it is the correct preorder traversal sequence of a binary search tree.

You may assume each number in the sequence is unique.

**Follow up:**

Could you do it using only constant space complexity?
###思路：
实质是如果array合法，那么必须满足根的后面一旦出现比根大的数，那么就不能再出现小的，那么就要维护一个目前允许出现的最小值。

用low来维护最小值，用一个栈来维护还没有处理过的根。

如果遇到比low更小的，返回false;

如果遇到比栈顶小或者栈为空，那么入栈，否则出栈，每次出栈时更新最小值，直到栈顶大于当前值，入栈。

###代码：

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

##256. Paint House
###题目：
There are a row of n houses, each house can be painted with one of the three colors: red, blue or green. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a *n x 3* cost matrix. For example, **costs[0][0]** is the cost of painting house 0 with color red; **costs[1][2]** is the cost of painting house 1 with color green, and so on... Find the minimum cost to paint all houses.

**Note:**

All costs are positive integers.
###思路：
DP， 定义好状态和转移即可。
###代码：

```
int minCost(vector<vector<int>>& costs) {
    int len = costs.size();
    int f[len+1][3] = {0};
    for(int i = 1; i <= len; ++i) {
        for(int j = 0; j < 3; ++j) {
            int nowmin = INT_MAX;
            for(int k = 0; k < 3; ++k) {
                if(j == k) continue;
                nowmin = min(f[i-1][k], nowmin);
            }
            f[i][j] = nowmin + costs[i-1][j];
        }
    }
    int res = f[len][0];
    for(int i = 1; i < 3; ++i) res = min(res, f[len][i]);
    return res;
}
```

##259. 3Sum Smaller
###题目：
Given an array of n integers nums and a target, find the number of index triplets **i, j, k** with **0 <= i < j < k < n** that satisfy the condition **nums[i] + nums[j] + nums[k] < target**.

For example, given nums = **[-2, 0, 1, 3]**, and target = 2.

Return 2. Because there are two triplets which sums are less than 2:

```
[-2, 0, 1]
[-2, 0, 3]
```
**Follow up:**

Could you solve it in O(n2) runtime?
###思路：
还是two pointer的思路，不知如果符合的话，答案可以直接加r-l，因为r已经是目前最大，r向左走的话都会符合；

如果不符合那么l++。

###代码：

```
int threeSumSmaller(vector<int>& nums, int target) {
    int res = 0;
    int len = nums.size();
    if(len <= 0) return res;
    sort(nums.begin(), nums.end());
    for(int i = 0; i < len; ++i) {
        int l = i+1, r = len -1;
        while(l < r && l < len) {
            if(nums[i] + nums[l] + nums[r] < target) {
                res = res + (r-l);
                l++;
            }else {
                r--;
            }
        }
    }
    return res;
}
```

##261. Graph Valid Tree
###题目：
Given **n** nodes labeled from **0** to **n - 1** and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

For example:

Given **n = 5** and edges = **[[0, 1], [0, 2], [0, 3], [1, 4]]**, return **true**.

Given **n = 5** and edges = **[[0, 1], [1, 2], [2, 3], [1, 3], [1, 4]]**, return **false**.

**Hint:**

* Given **n = 5** and edges = **[[0, 1], [1, 2], [3, 4]]**, what should your return? Is this case a valid tree?
* According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”

**Note:**

you can assume that no duplicate **edges** will appear in edges. Since all edges are undirected, **[0, 1]** is the same as **[1, 0]** and thus will not appear together in **edges**.
###思路：
使用并查集的思想。

首先节点数为n，那么如果是合法的树，那么边数必然是n-1；

遍历每条边，将其加到一个并查集里面，如果某条边的两个顶点属于同一个并查集的集合，那么返回false；否则将其加入并查集。

如果所有边都处理完，那么返回true。

###代码：

```
int find(int *f, int u) {
    if(f[u] == u) return u;
    return f[u] = find(f, f[u]);
}
bool validTree(int n, vector<pair<int, int>>& edges) {
    int len = edges.size();
    if(len != n-1) return false;
    int f[n];
    for(int i = 0; i < n; ++i) f[i] = i;
    
    for(auto edge : edges) {
        int u = edge.first, v = edge.second;
        int fu = find(f, u), fv = find(f, v);
        if(fu == fv) return false;
        f[fu] = fv;
    }
    return true;
}
```

##265. Paint House II
###题目：
There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a **n x k** cost matrix. For example, **costs[0][0]** is the cost of painting house 0 with color 0; **costs[1][2]** is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

**Note:**

All costs are positive integers.

**Follow up:**

Could you solve it in O(nk) runtime?
###思路：
由于状态存储k中颜色的所有结果会爆内存，所以使用mini1和mini2记录位置，代价最小的两种颜色的结果，用old_mini1和old_mini2来记录上一轮的结果。持续更新。

###代码：

```
int minCostII(vector<vector<int>>& costs) {
    int n = costs.size();
    if(n == 0) return 0;
    int k = costs[0].size();
    int f[k] = {0};
    int mini1, mini2;
    for(int i = 1; i <= n; ++i) {
        int old_mini1 = (i == 1 ? 0 : mini1);
        int old_mini2 = (i == 1 ? 0 : mini2);
        mini1 = INT_MAX;
        mini2 = INT_MAX;
        for(int j = 0; j < k; ++j) {
            if(f[j] != old_mini1 || old_mini1 == old_mini2) {
                f[j] = old_mini1 + costs[i-1][j];
            }else {
                f[j] = old_mini2 + costs[i-1][j];
            }
            if(f[j] > mini1) {
                mini2 = min(mini2, f[j]);
            }else {
                mini2 = mini1;
                mini1 = f[j];
            }
        }
    }
    return mini1;
}
```
##266. Palindrome Permutation
###题目：

```
Given a string, determine if a permutation of the string could form a palindrome.

For example,
"code" -> False, "aab" -> True, "carerac" -> True.

Hint:

Consider the palindromes of odd vs even length. What difference do you notice?
Count the frequency of each character.
If each character occurs even number of times, then it must be a palindrome. How about character which occurs odd number of times?
```
###思路：
首先记录所有字母出现的个数；

如果字符串总长度是偶数，那么字母个数为单数的字母不能存在，存在即返回false；

如果字符串总长度是奇数，那么字母个数为单数的字母只能有一个，超过即返回false。
###代码：

```
bool canPermutePalindrome(string s) {
    int len = s.size();
    unordered_map<char, int> all;
    for(int i = 0; i < len; ++i) {
        all[s[i]]++;
    }
    int cnt = 0;
    for(auto x : all) {
        if(x.second % 2 == 1) cnt++;
        if(len % 2 == 0 && cnt > 0) return false;
        if(len % 2 == 1 && cnt > 1) return false;
    }
    return true;
}
```


##267. Palindrome Permutation II
###题目：

Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

**For example:**

Given s = "aabb", return ["abba", "baab"].

Given s = "abc", return [].

**Hint:**

If a palindromic permutation exists, we just need to generate the first half of the string.
To generate all distinct permutations of a (half of) string, use a similar approach from: Permutations II or Next Permutation.

###思路：

首先记录每种字母出现的次数，将中间字母设置为出现的第一种个数为单数的字母，后面如果再出现个数为单数的字母，则返回空集，因为不可能组成回文；

每类字母的一半组成一个新的字符串s，只要得到这个字符串的所有全排列

最后s + mid + reverse(s)作为一个结果。

利用next_permutation(ch.begin(), ch.end())直接求得下一个全排列。


###代码：

```
vector<string> generatePalindromes(string s) {
    vector<string> res;
    unordered_map<char, int> all;
    for(auto i : s) {
        all[i]++;
    } 
    string mid = "", ch;
    for(auto i : all) {
        if(i.second % 2) {
            if(mid != "") return res;
            else mid += i.first;
        }
        ch.append(i.second / 2, i.first);
        
    }
    
    return allPermutation(mid, ch);
}

vector<string> allPermutation(const string &mid, string &ch) {
    vector<string> res;
    sort(ch.begin(), ch.end());
    do {
        string re = ch;
        reverse(re.begin(), re.end());
        res.push_back(ch + mid + re);
    }while(next_permutation(ch.begin(), ch.end()));
    return res;
}
```
##269. Alien Dictionary
###题目：
There is a new alien language which uses the latin alphabet. However, the order among letters are unknown to you. You receive a list of words from the dictionary, where **words are sorted lexicographically by the rules of this new language**. Derive the order of letters in this language.

For example,

Given the following words in dictionary,

```
[
  "wrt",
  "wrf",
  "er",
  "ett",
  "rftt"
]
```
The correct order is: "wertf".

**Note:**

* You may assume all letters are in lowercase.
* If the order is invalid, return an empty string.
* There may be multiple valid order of letters, return any one of them is fine.

###思路：
拓扑排序
###代码：

```
string alienOrder(vector<string>& words) {
    map<char, set<char>> suc, pre;
    set<char> chars;
    string s;
    for (string t : words) {
        chars.insert(t.begin(), t.end());
        for (int i=0; i<min(s.size(), t.size()); ++i) {
            char a = s[i], b = t[i];
            if (a != b) {
                suc[a].insert(b);
                pre[b].insert(a);
                break;
            }
        }
        s = t;
    }
    set<char> free = chars;
    for (auto p : pre)
        free.erase(p.first);
    string order;
    while (free.size()) {
        char a = *begin(free);
        free.erase(a);
        order += a;
        for (char b : suc[a]) {
            pre[b].erase(a);
            if (pre[b].empty())
                free.insert(b);
        }
    }
    return order.size() == chars.size() ? order : "";
}
```

##270. Closest Binary Search Tree Value
###题目：

Given a non-empty binary search tree and a target value, find the value in the BST that is closest to the target.

**Note:**

* Given target value is a floating point.

* You are guaranteed to have only one unique value in the BST that is closest to the target.

###思路：

1、注意double要用fabs()来求绝对值；

2、要注意使用BST的性质，如果root->val - target > 0，那么和target更接近的值肯定不可能出现在root的右子树中，反之亦然。

###代码：

```
void closest(TreeNode* root, double target, double &minus, int &res) {
    if(root == NULL) return;
    double mm = (double)root->val - target;
    if(fabs(mm) < minus) {
        minus = fabs(mm);
        res = root->val;
    }
    if(mm > 0) closest(root->left, target, minus, res);
    else closest(root->right, target, minus, res);
}

int closestValue(TreeNode* root, double target) {
    double minus = fabs((double)root->val - target);
    int res = root->val;
    closest(root, target, minus, res);
    return res;
}
```
##271. Encode and Decode Strings
###题目：
Design an algorithm to encode **a list of strings** to **a string**. The encoded string is then sent over the network and is decoded back to the original list of strings.

Machine 1 (sender) has the function:

```
string encode(vector<string> strs) {
  // ... your code
  return encoded_string;
}
```
Machine 2 (receiver) has the function:

```
vector<string> decode(string s) {
  //... your code
  return strs;
}
```
So Machine 1 does:

```
string encoded_string = encode(strs);
```
and Machine 2 does:

```
vector<string> strs2 = decode(encoded_string);
```
**strs2** in Machine 2 should be the same as **strs** in Machine 1.

Implement the **encode** and **decode** methods.

**Note:**

* The string may contain any possible characters out of 256 valid ascii characters. Your algorithm should be generalized enough to work on any possible characters.
* Do not use class member/global/static variables to store states. Your encode and decode algorithms should be stateless.
* Do not rely on any library method such as **eval** or serialize methods. You should implement your own encode/decode algorithm.

###思路：
**编码方式：**

对于每一个字符串，都编码成：长度+“@”（类似的特殊符号）+字符串

**解码方式：**

先找到第一个“@”，前面的数字必定是字符串长度，然后将“@”后面相应长度的字符串放入答案，循环进行。

###代码：

```
class Codec {
public:

    // Encodes a list of strings to a single string.
    string encode(vector<string>& strs) {
        string res = "";
        for(string s : strs) {
            res += to_string(s.size()) + "@" + s;
        }
        return res;
    }

    // Decodes a single string to a list of strings.
    vector<string> decode(string s) {
        vector<string> res;
        int p = 0, len = s.size();
        while(p < len) {
            int pos = s.find_first_of("@", p);
            int l = stoi(s.substr(p, pos - p));
            string str = s.substr(pos+1, l);
            res.push_back(str);
            p = pos + l + 1;
        }
        return res;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec codec;
// codec.decode(codec.encode(strs));
```

##272. Closest Binary Search Tree Value II
###题目：

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

**Note:**

* Given target value is a floating point.
* You may assume k is always valid, that is: k ≤ total nodes.
* You are guaranteed to have only one unique set of k values in the BST that are closest to the target.

**Follow up:**

Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

**Hint:**

* Consider implement these two helper functions:
	1. getPredecessor(N), which returns the next smaller node to N.
	2. getSuccessor(N), which returns the next larger node to N.
* Try to assume that each node has a parent pointer, it makes the problem much easier.
* Without parent pointer we just need to keep track of the path from the root to the current node using a stack.
* You would need two stacks to track the path in finding predecessor and successor node separately.

###思路：
方法1：

时间复杂度O(n*log(k)) 空间复杂度O(k)
dfs，利用优先队列存储差值最小的k个。

方法2：

时间复杂度O(n) 空间复杂度O(k+logn)
用非递归的方式，中序遍历（即从小到大）维护与目标差值最小的k个在队列中。

###代码：

```
//方法1：
void dfs(TreeNode* root, double target, int k, priority_queue<pair<double, int>> &q) {
    if(!root) return;
    q.push(make_pair(fabs(root->val - target), root->val));
    
    if(q.size() > k) q.pop();
    
    dfs(root->left, target, k, q);
    dfs(root->right, target, k, q);
}

vector<int> closestKValues(TreeNode* root, double target, int k) {
    priority_queue<pair<double, int>> q;
    vector<int> res;
    dfs(root, target, k, q);
    while(!q.empty()) {
        res.push_back(q.top().second);
        q.pop();
    }
    return res;
}

//方法2：
vector<int> closestKValues(TreeNode* root, double target, int k) {
    if (root == NULL || k <= 0) { return vector<int>(); }
    deque<int> myQ; 
    stack<TreeNode *> ss; // use stack to do in-order search 
    while (root || !ss.empty()) {
        while(root) {
            ss.push(root);
            root = root->left;
        } 
        root = ss.top();
        ss.pop();
        // let's process root now
        if (myQ.size() < k) {
            myQ.push_back(root->val);
        } else { // size == k, 
            // if root->val is too small, update queue; otherwise, compare with queue's front
            if (target > root->val || (abs(target - root->val) < abs(target - myQ.front()))) {
                myQ.push_back(root->val);
                myQ.pop_front();
            } else {  
                break; 
            }
        }
        // move to next one 
        root = root->right;
    }
    return vector<int>(myQ.begin(), myQ.end());
}

```


##276. Paint Fence**
###题目：

There is a fence with n posts, each post can be painted with one of the k colors.

You have to paint all the posts such that no more than two adjacent fence posts have the same color.

Return the total number of ways you can paint the fence.

Note:
n and k are non-negative integers.

###思路：
主要是题意的理解！！

题意是相邻的篱笆可以重复颜色，但是重复颜色的篱笆个数不可以超过2.

根据题意那么每个篱笆可以有两种状态，一种是和前一个颜色相同，一种是不同。

###代码：

```
//容易明白版本
int numWays(int n, int k) {
    if (n <= 1 || k == 0) return n * k;
    int dp1[n + 1] = {0}, dp2[n + 1] = {0};
    dp1[1] = dp2[1] = k;
    for (int i = 2; i <= n; ++i) {
        dp1[i] = dp2[i - 1];
        dp2[i] = (k - 1) *
        (dp2[i - 2] + dp2[i - 1]);  
        // dp2[i]=(k-1)(dp1[i-1]+dp2[i-1])
        // =(k-1)(dp2[i-2]+dp2[i-1])
    }
    return dp1[n] + dp2[n];
}

//更加简洁版本
int numWays(int n, int k) {
    if (n < 2 || !k) return n * k; 
    int s = k, d1 = k, d2 = k * (k - 1); 
    for (int i = 2; i < n; i++) {
        s = d2;
        d2 = (k - 1) * (d1 + d2);
        d1 = s;
    }
    return s + d2;
}
```

##277. Find the Celebrity
###题目：

Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool **knows(a, b)** which tells you whether A knows B. Implement a function **int findCelebrity(n)**, your function should minimize the number of calls to **knows**.

Note: There will be exactly one celebrity if he/she is in the party. Return the celebrity's label if there is a celebrity in the party. If there is no celebrity, return **-1**.

###思路：
重点在于首先找到最有可能是celebrity的点，然后验证。
###代码：

```
int findCelebrity(int n) {
    if(n <= 1) return 0;
    int res = 0;
    //找到最有可能是celebrity的点，因为如果不是celebrity,那么下一个点不会认识他，res会不断更新；
    //但如果是celebrity，那么后面的点都会认识他，就不必更新了。
    //也有可能这个集合里面不存在celebrity，那么res会停留在最后一个点。
    for(int i = 1; i < n; ++i) {
        if(!knows(i, res)) {
            res = i;
        }
    }
    //验证上一步找到的可能的celebrity是否正确。如果存在这个点之外的点不认识他或者他认识其他点，则返回-1；
    //反之res就是答案。
    for(int i = 0; i < n; ++i) {
        if(i == res) continue;
        if(!knows(i, res) || knows(res, i)) 
            return -1;
    }
    return res;
}
```
##280. Wiggle Sort
###题目：
Given an unsorted array nums, reorder it in-place such that **nums[0] <= nums[1] >= nums[2] <= nums[3]**....

For example, given nums = [3, 5, 2, 1, 6, 4], one possible answer is [1, 6, 2, 5, 3, 4].

###思路：
The final sorted nums needs to satisfy two conditions:


* If i is odd, then nums[i] >= nums[i - 1];
* If i is even, then nums[i] <= nums[i - 1].

The code is just to fix the orderings of nums that do not satisfy 1 and 2.
###代码：

```
void wiggleSort(vector<int>& nums) {
    int len = nums.size();
    for(int i = 1; i < len; ++i) {
        if((i % 2 == 1 && nums[i] < nums[i-1]) || (i % 2 == 0 && nums[i] > nums[i-1])) {
            swap(nums[i], nums[i-1]);
        }
    }
}
```


##281. Zigzag Iterator
###题目：
Given two 1d vectors, implement an iterator to return their elements alternately.

For example, given two 1d vectors:

```
v1 = [1, 2]
v2 = [3, 4, 5, 6]
```
By calling next repeatedly until hasNext returns **false**, the order of elements returned by next should be: **[1, 3, 2, 4, 5, 6]**.

**Follow up:** What if you are given **k** 1d vectors? How well can your code be extended to such cases?

Clarification for the follow up question - Update (2015-09-18):
The "Zigzag" order is not clearly defined and is ambiguous for **k > 2 **cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example, given the following input:

```
[1,2,3]
[4,5,6,7]
[8,9]
```
It should return **[1,4,8,2,5,9,3,6,7]**.
###思路：
方法1：

用一个队列按照输出的顺序存放元素。

方法2：
如果是存在k个数组的话，使用此方法。queue里面存储的不是每个元素，是每个数组的起始和结束指针。
###代码：

```
//方法1：
class ZigzagIterator {
private:
    queue<int> all;
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        int len1 = v1.size(), len2 = v2.size();
        int i = 0;
        while(i < len1 && i < len2) {
            all.push(v1[i]);
            all.push(v2[i]);
            ++i;
        }
        while(i < len1) {
            all.push(v1[i]);
            ++i;
        }
        while(i < len2) {
            all.push(v2[i]);
            ++i;
        }
    }

    int next() {
        int n = all.front();
        all.pop();
        return n;
    }

    bool hasNext() {
        return !all.empty();
    }
};

//方法2：
class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        if (v1.size() != 0)
            Q.push(make_pair(v1.begin(), v1.end()));
        if (v2.size() != 0)
            Q.push(make_pair(v2.begin(), v2.end()));
    }

    int next() {
        vector<int>::iterator it = Q.front().first;
        vector<int>::iterator endIt = Q.front().second;
        Q.pop();
        if (it + 1 != endIt)
            Q.push(make_pair(it+1, endIt));
        return *it;
    }

    bool hasNext() {
        return !Q.empty();
    }
private:
    queue<pair<vector<int>::iterator, vector<int>::iterator>> Q;
};

/**
 * Your ZigzagIterator object will be instantiated and called as such:
 * ZigzagIterator i(v1, v2);
 * while (i.hasNext()) cout << i.next();
 */
```
##285. Inorder Successor in BST
###题目：
Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

Note: If the given node has no in-order successor in the tree, return **null**.

###思路：
在BST中寻找某个固定节点的后继，即找到比这个节点值大的最小的节点。

如果这个节点存在右儿子，那么答案是右子树中最左边的节点；

如果没有右儿子，那么就是从root到p最后一个向左转的节点。

###代码：

```
TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
    if(!p) return p;
    if(p->right) {
        TreeNode *q = p->right;
        while(q->left) {
            q = q->left;
        }
        return q;
    }
    
    TreeNode *res = NULL;
    while(root->val != p->val) {
        if(root->val < p->val) {
            root = root->right;
        }else {
            res = root;
            root = root->left;
        }
    }
    return res;
}
```

##286. Walls and Gates
###题目：

You are given a m x n 2D grid initialized with these three possible values.

* **-1** - A wall or an obstacle.
* **0** - A gate.
* **INF** - Infinity means an empty room. We use the value **2^31 - 1 = 2147483647** to represent **INF** as you may assume that the distance to a gate is less than **2147483647**.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a gate, it should be filled with INF.

For example, given the 2D grid:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```
After running your function, the 2D grid should be:

```
3  -1   0   1
2   2   1  -1
1  -1   2  -1
0  -1   3   4
```

###思路：
比较裸的BFS，但是要注意这个要防止重复访问。树的结构不会重复访问，所以不需要担心，这里不一样。

###代码：

```
private:
    struct NODE{
        int x,y,t;
        NODE(){}
        NODE(int _x,int _y,int _t) : x(_x), y(_y), t(_t) {}
    };
public:
    int go[4][2] = {{1, 0}, {-1, 0}, {0, -1}, {0, 1}};
    
    void wallsAndGates(vector<vector<int>>& rooms) {
        int m = rooms.size();
        if(m == 0) return;
        int n = rooms[0].size();
        queue<NODE> all;
        int visit[m][n];
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                visit[i][j] = 0;
                if(rooms[i][j] == 0) {
                    all.push(NODE(i,j,0));
                    visit[i][j] = 1;
                }
            }
        }
        NODE now;
        while(!all.empty()) {
            now = all.front();
            rooms[now.x][now.y] = now.t;
            all.pop();
            for(int i = 0; i < 4; ++i) {
                int x = now.x + go[i][0];
                int y = now.y + go[i][1];
                if(x < 0 || x >= m || y < 0 || y >= n) continue;
                if(rooms[x][y] == -1 || visit[x][y] == 1) continue;
                all.push(NODE(x, y, now.t+1));
                visit[x][y] = 1;
            }
        }
    }
```

##288. Unique Word Abbreviation
###题目：
An abbreviation of a word follows the form <first letter><number><last letter>. Below are some examples of word abbreviations:

```
a) it                      --> it    (no abbreviation)

     1
b) d|o|g                   --> d1g

              1    1  1
     1---5----0----5--8
c) i|nternationalizatio|n  --> i18n

              1
     1---5----0
d) l|ocalizatio|n          --> l10n
```
Assume you have a dictionary and given a word, find whether its abbreviation is unique in the dictionary. A word's abbreviation is unique if *no other* word from the dictionary has the same abbreviation.

Example: 

```
Given dictionary = [ "deer", "door", "cake", "card" ]

isUnique("dear") -> 
false

isUnique("cart") -> 
true

isUnique("cane") -> 
false

isUnique("make") -> 
true
```
###思路：
主要是对题意的理解。

unique是当字典里面没有和他同样缩写的或者字典里面和他缩写相同的都是它本身。

所以字典中不仅要记录各种缩写的个数，还要记录各个单词出现的个数，如果查询在字典中出现的个数小于其缩写出现的个数，那说明并不“Unique”。
###代码：

```
class ValidWordAbbr {
public:
    unordered_map<string, int> all;
    unordered_map<string, int> allW;
    string sTd(string s) {
        int len = s.size();
        if(len <= 2) return s;
        else {
            string res;
            res = s[0] + to_string(len-2) + s[len-1];
            return res;
        }
    }
    
    ValidWordAbbr(vector<string> &dictionary) {
        for(auto s : dictionary) {
            all[sTd(s)]++;
            allW[s]++;
        }
    }

    bool isUnique(string word) {
        if(all[sTd(word)] > allW[word]) return false;
        else return true;
    }
};


// Your ValidWordAbbr object will be instantiated and called as such:
// ValidWordAbbr vwa(dictionary);
// vwa.isUnique("hello");
// vwa.isUnique("anotherWord");
```
##291. Word Pattern II
###题目：
Given a **pattern** and a string **str**, find if **str** follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in **pattern** and a non-empty substring in **str**.

Examples:

* pattern = **"abab"**, str = **"redblueredblue"** should return true.
* pattern = **"aaaa"**, str = **"asdasdasdasd"** should return true.
* pattern = **"aabb"**, str = **"xyzabcxzyabc"** should return false.

**Notes:**

You may assume both pattern and str contains only lowercase letters.

###思路：
1、建立两个map来保证字符和字符串一一对应。

2、两个指针遍历pattern和str，如果两个指针都处理到最后，那么返回true，如果存在不同步，返回false;

3、str向后试着处理，如果与原先的map内容矛盾那么继续是其他情况，否则加入map，并且向后进行，如果并不能返回true，那么从map中删除掉这次加入的部分（完成回溯）。

###代码：

```
private:
    unordered_map<char, string> pHash;
    unordered_map<string, char> sHash;
public:

    bool wordPatternMatch(string pattern, string str) {
        return match(pattern, 0, str, 0);
    }
    
    bool match(string &p, int i, string &s, int j) {
        int m = p.size();
        int n = s.size();
        if(i == m || j == n) {
            if(i == m && j == n)
                return true;
            else return false;
        }
        else {
            bool yes = false;
            for(int k = j; k < n; ++k) {
                string now = s.substr(j, k-j+1);
                if(pHash.find(p[i]) != pHash.end()) {
                    if(pHash[p[i]] != now)
                        continue;
                }
                else if(sHash.find(now) != sHash.end()) {
                    if(sHash[now] != p[i])
                        continue;
                }else {
                    pHash[p[i]] = now;
                    sHash[now] = p[i];
                    yes = true;
                }
                if(match(p, i+1, s, k+1))
                    return true;
                if(yes) {
                     pHash.erase(p[i]);
                     sHash.erase(now);
                } 
            }
            return false;
        }
    }
```


##293. Flip Game
###题目：
You are playing the following Flip Game with your friend: Given a string that contains only these two characters: **+**and **-**, you and your friend take turns to flip two consecutive **"++"** into **"--"**. The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to compute all possible states of the string after one valid move.

For example, given **s = "++++"**, after one move, it may become one of the following states:

```
[
  "--++",
  "+--+",
  "++--"
]
```
If there is no valid move, return an empty list **[]**.

###思路：
从左向右遍历一遍，两两判断是否可以转换，可以的话，转换然后加入答案，记得要再转换回原始情况，才不会影响后面的判断。

###代码：

```
vector<string> generatePossibleNextMoves(string s) {
    int len = s.size();
    vector<string> res;
    if(len <= 1) return res;
    for(int i = 1; i < len; ++i) {
        if(s[i-1] == '+' && s[i] == '+') {
            s[i-1] = s[i] = '-';
            res.push_back(s);
            s[i-1] = s[i] = '+';
        }
    }
    return res;
}
```
##296. Best Meeting Point

###题目：
A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance**(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|**.

For example, given three people living at (0,0), (0,4), and (2,2):

```
1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point (0,2) is an ideal meeting point, as the total travel distance of 2+2+2=6 is minimal. So return 6.

**Hint:**

Try to solve it in one dimension first. How can this solution apply to the two dimension case?

###思路：
1、将问题先简化成一维问题，在一个数轴上面有两个点，那么在数轴上，到两个点距离加和最短的点在以这两个点为端点的线段上任意位置；

同理可推出3-n个点的情况，得到规律是在所有点的中位数（奇数点为最中间的点，偶数点为最中间两个点为端点的线段上）可得到最短距离和；

2、此题的二维情况中，两个维度的情况并不互相干扰，可以分别找出每个维度的结果，然后组合代表的点就是最终结果。

3、STL中的**nth_element()**方法的使用 通过调用**nth_element(start, start+n, end)** 方法可以使**第n大元素处于第n位置**（从0开始,其位置是下标为 n的元素），并且比这个元素小的元素都排在这个元素之前，比这个元素大的元素都排在这个元素之后，但**不能保证他们是有序的**。

###代码：
```
int minTotalDistance(vector<vector<int>>& grid) {
    int m = grid.size();
    if(m <= 0) return 0;
    int n = grid[0].size();
    
    int c[n * m];//保存左右节点的位置的某个维度的坐标
    int res = 0;
    for(int dim = 0; dim < 2; ++dim) { //分别处理两个维度
        int k = 0;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(grid[i][j] == 1) {
                    c[k++] = dim == 0 ? i : j;
                }
            }
        }
        nth_element(c, c + k / 2, c + k);
        int mid = c[k / 2];
        for(int i = 0; i < k; ++i) {
            res += abs(c[i] - mid);
        }
    }
    return res;
}
```


##298. Binary Tree Longest Consecutive Sequence
###题目：
Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

For example,

```
	1
    \
     3
    / \
   2   4
        \
         5
```
Longest consecutive sequence path is **3-4-5**, so return **3**.

```
 	2
    \
     3
    / 
   2    
  / 
 1
```
Longest consecutive sequence path is **2-3**,not**3-2-1**, so return **2**.

###思路：
1、首先题意需要理解清楚，这个题意是说要从父亲到儿子的序列，且序列中要是连续递增的数值，不可以断开也不可以递减。

2、要运用递归的解法，需要定义清楚递归函数的返回值，或者函数内部计算的各个变量的含义。

###代码：
**方法1：**

此方法dfs函数中计算的是**严格以cur节点结束**的连续递增序列的长度。

res是全局变量，记录最长的长度。

```
int res;
void dfs(TreeNode *parent, TreeNode *cur, int len) {
    if (!cur) return;
    if (parent && parent->val + 1 == cur->val) len = len + 1;
    else len = 1;
    res = max(len, res);
    dfs(cur, cur->left, len);
    dfs(cur, cur->right, len);
}
int longestConsecutive(TreeNode *root) {
    res = 0;
    dfs(NULL, root, 0);
    return res;
}
```

**方法2：**

此方法dfs函数中返回值是**严格以cur节点开始**的连续递增序列长度。

cnt是全局变量，记录严格得以任何一个节点为开头的最长值。

```
int cnt;
int dfs(TreeNode *root) {
    if (!root) return 0;
    int res = 1;
    if (root->left) {
        int lc = dfs(root->left);
        if (root->val + 1 == root->left->val) res = max(res, lc + 1);
    }
    if (root->right) {
        int rc = dfs(root->right);
        if (root->val + 1 == root->right->val) res = max(res, rc + 1);
    }
    cnt = max(cnt, res);
    return res;
}
int longestConsecutive(TreeNode *root) {
    cnt = 0;
    dfs(root);
    return cnt;
}
```
**方法3：**

此方法dfs返回的是包含了**严格以root为结束**或者**不严格得以它为开始**的最长的序列长度。


```
int dfs(TreeNode *root, TreeNode *parent, int len) {
    if(!root) return 0;
    if(parent && root->val - 1 == parent->val) len++;
    else len = 1;

    int lc = dfs(root->left, root, len);
    int rc = dfs(root->right, root, len);
    
    int cnt = max(len, max(lc, rc));
    return cnt;
}
int longestConsecutive(TreeNode* root) {
    return dfs(root, NULL, 0);
}
```

##302. Smallest Rectangle Enclosing Black Pixels
###题目：
An image is represented by a binary matrix with **0** as a white pixel and **1** as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location **(x, y)** of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

For example, given the following image:

```
[
  "0010",
  "0110",
  "0100"
]
```
and **x = 0**, **y = 2**,

Return **6**.
###思路：
方法1：DFS

时间复杂度O(n) 空间复杂度O(n)

dfs遍历每一个为"1"的点，更新x和y的最小、最大值，最后用两个差值进行乘积。

方法2：二分

在两个维度分别二分查找。
###代码：

```
//方法1：
int go[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
int minX, maxX, minY, maxY;
void dfs(vector<vector<char>>& image, int x, int y, int m, int n, vector<vector<bool>> &yes){
    for(int i = 0; i < 4; ++i) {
        int nowX = x + go[i][0];
        int nowY = y + go[i][1];
        if(nowX < 0 || nowX >= m || nowY < 0 || nowY >= n) continue;
        if(image[nowX][nowY] == '0' || yes[nowX][nowY] == 1) continue;
        minX = min(minX, nowX);
        minY = min(minY, nowY);
        maxX = max(maxX, nowX);
        maxY = max(maxY, nowY);
        yes[nowX][nowY] = 1;
        dfs(image, nowX, nowY, m, n, yes);
    }
}

int minArea(vector<vector<char>>& image, int x, int y) {
    int m = image.size();
    if(m <= 0) return 0;
    int n = image[0].size();
    vector<vector<bool>> yes(m, vector<bool>(n, false));
    minX = maxX = x;
    minY = maxY = y;
    dfs(image, x, y, m, n, yes);
    int res = (maxX - minX + 1) * (maxY - minY + 1);
    return res;
}

//方法2：
vector<vector<char>> *image;
int minArea(vector<vector<char>> &iImage, int x, int y) {
    image = &iImage;
    int m = int(image->size()), n = int((*image)[0].size());
    int top = searchRows(0, x, 0, n, true);
    int bottom = searchRows(x + 1, m, 0, n, false);
    int left = searchColumns(0, y, top, bottom, true);
    int right = searchColumns(y + 1, n, top, bottom, false);
    return (right - left) * (bottom - top);
}
int searchRows(int i, int j, int low, int high, bool opt) {
    while (i != j) {
        int k = low, mid = (i + j) / 2;
        while (k < high && (*image)[mid][k] == '0') ++k;
        if (k < high == opt)
            j = mid;
        else
            i = mid + 1;
    }
    return i;
}
int searchColumns(int i, int j, int low, int high, bool opt) {
    while (i != j) {
        int k = low, mid = (i + j) / 2;
        while (k < high && (*image)[k][mid] == '0') ++k;
        if (k < high == opt)
            j = mid;
        else
            i = mid + 1;
    }
    return i;
}
```
##305. Number of Islands II
###题目：
A 2d grid map of **m** rows and **n** columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, **count the number of islands after each addLand operation**. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example:**

Given **m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]**.

Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).

```
0 0 0
0 0 0
0 0 0
```
Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.

```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.

```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.

```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```
Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.

```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```
We return the result as an array: **[1, 1, 2, 3]**

**Challenge:**

Can you do it in time complexity O(k log mn), where k is the length of the **positions**?

###思路：
并查集的思想。

初始化将矩阵中每个点的root都设为-1，表示不是陆地。

没加入一个点，首先将其父亲设为自己，陆地数目+1，然后遍历其上下左右四个方向，如果存在陆地，那么合并，合并的同时陆地数目-1。

###代码：

```
private:
    vector<int> root;
    int go[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
public:
    int findRoot (int now) {
        if(root[now] == now) 
            return now;
        else return root[now] = findRoot(root[now]);
    }
    vector<int> numIslands2(int m, int n, vector<pair<int, int>>& positions) {
        root = vector<int>(m * n, -1);
        vector<int> res;
        int landNum = 0;
        for(auto p : positions) {
            int x = p.first, y = p.second, index = x * n + y;
            root[index] = index;
            landNum++;
            for(auto dir : go) {
                int nowx = x + dir[0], nowy = y + dir[1], nowindex = nowx * n + nowy;
                if(nowx >= 0 && nowx < m && nowy >= 0 && nowy < n && root[nowindex] != -1) {
                    int nowP = findRoot(nowindex), P = findRoot(index);
                    if(P != nowP) {
                        root[P] = nowP;
                        --landNum;
                    }
                }
            }
            res.push_back(landNum);
        }
        return res;
    }
```

##308. Range Sum Query 2D - Mutable **
###题目:
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

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
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```
**Note:**

* The matrix is only modifiable by the update function.
* You may assume the number of calls to update and sumRegion function is distributed evenly.
* You may assume that row1 ≤ row2 and col1 ≤ col2.

###思路：

###代码：

```
// Time:  ctor:   O(mlogm * nlogn)
//        update: O(logm * logn)
//        query:  O(logm * logn)
// Space: O(m * n)
// Binary Indexed Tree (BIT) solution.

class NumMatrix {
public:
    int **C;
    int n, m;
    NumMatrix(vector<vector<int>> &matrix) {
        n = matrix.size();
        m = n == 0 ? 0 : matrix[0].size();
        C = new int *[n + 1];
        for (int i = 0; i <= n; ++i) C[i] = new int[m + 1];
        
        for (int i = 0; i <= n; ++i)
            for (int j = 0; j <= m; ++j) C[i][j] = 0;
        
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < m; ++j) {
                int val = matrix[i][j];
                for (int p = i + 1; p <= n; p += lowbit(p))
                    for (int q = j + 1; q <= m; q += lowbit(q)) C[p][q] += val;
            }
    }
    
    int lowbit(int x) { return x & (-x); }
    void update(int row, int col, int val) {
        int t = sumRegion(row, col, row, col);
        val = val - t;
        
        row++;
        col++;
        for (int i = row; i <= n; i += lowbit(i))
            for (int j = col; j <= m; j += lowbit(j)) C[i][j] += val;
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        row1++;
        col1++;
        row2++;
        col2++;
        return sum(row2, col2) - sum(row1 - 1, col2) - sum(row2, col1 - 1) +
        sum(row1 - 1, col1 - 1);
    }
    
    int sum(int x, int y) {
        int res = 0;
        for (int i = x; i >= 1; i -= lowbit(i))
            for (int j = y; j >= 1; j -= lowbit(j)) {
                res += C[i][j];
            }
        return res;
    }
};

// Your NumMatrix object will be instantiated and called as such:
// NumMatrix numMatrix(matrix);
// numMatrix.sumRegion(0, 1, 2, 3);
// numMatrix.update(1, 1, 10);
// numMatrix.sumRegion(1, 2, 3, 4);
```

##311. Sparse Matrix Multiplication
###题目：
Given two sparse matrices A and B, return the result of AB.

You may assume that A's column number is equal to B's row number.

**Example:**

```
A = [
  [ 1, 0, 0],
  [-1, 0, 3]
]

B = [
  [ 7, 0, 0 ],
  [ 0, 0, 0 ],
  [ 0, 0, 1 ]
]


     |  1 0 0 |   | 7 0 0 |   |  7 0 0 |
AB = | -1 0 3 | x | 0 0 0 | = | -7 0 3 |
                  | 0 0 1 |
```

###思路：
时间复杂度：O(m * n * l)
空间复杂度：O(m * l)
做法就是按照矩阵乘法的过程进行。
###代码：

```
vector<vector<int>> multiply(vector<vector<int>>& A, vector<vector<int>>& B) {
    int m = A.size(), n = A[0].size(), l = B[0].size();
    vector<vector<int>> res(m, vector<int>(l,0));
    for(int i = 0; i < m; ++i) {
        for(int j = 0; j < n; ++j) {
            if(A[i][j]) {
                for(int k = 0; k < l; ++k) {
                    res[i][k] += A[i][j] * B[j][k];
                }
            }
        }
    }
    return res;
}
```




##314. Binary Tree Vertical Order Traversal
###题目：
Given a binary tree, return the vertical order traversal of its nodes' values. (ie, from top to bottom, column by column).

If two nodes are in the same row and column, the order should be from left to right.

**Examples:**

1、Given binary tree **[3,9,20,null,null,15,7]**,

```
   3
  /\
 /  \
 9  20
    /\
   /  \
  15   7
```
return its vertical order traversal as:

```
[
  [9],
  [3,15],
  [20],
  [7]
]
```
2、Given binary tree **[3,9,8,4,0,1,7]**,

```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
```
return its vertical order traversal as:

```
[
  [4],
  [9],
  [3,0,1],
  [8],
  [7]
]
```
3、Given binary tree **[3,9,8,4,0,1,7,null,null,null,2,5]** (0's right child is 2 and 1's left child is 5),

```
     3
    /\
   /  \
   9   8
  /\  /\
 /  \/  \
 4  01   7
    /\
   /  \
   5   2
```
return its vertical order traversal as:

```
[
  [4],
  [9,5],
  [3,0,1],
  [8,2],
  [7]
]
```
###思路：
BFS用queue的方式遍历一遍树。遍历的时候需要在queue里面存下来点在第几列，默认root列数为0，向左减一，向右加一。

在队列pop的时候把节点加入到map中，map的key是列数，遍历完之后将map中按照key从小到大的value放入答案即可。

###代码：

```
vector<vector<int>> verticalOrder(TreeNode* root) {
    queue<pair<TreeNode*, int>> q;
    map<int, vector<int>> all;
    vector<vector<int>> res;
    if(!root) return res;
    q.push(make_pair(root, 0));
    while(!q.empty()) {
        TreeNode* now = q.front().first;
        int nowL = q.front().second;
        q.pop();
        all[nowL].push_back(now->val);
        if(now->left) {
            q.push(make_pair(now->left, nowL-1));
        }
        if(now->right) {
            q.push(make_pair(now->right, nowL+1));
        }
    }
    for(auto x : all) {
        res.push_back(x.second);
    }
    return res;
}
```
##317. Shortest Distance from All Buildings
###题目：
You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

* Each 0 marks an empty land which you can pass by freely.
* Each 1 marks a building which you cannot pass through.
* Each 2 marks an obstacle which you cannot pass through.

For example, given three buildings at **(0,0), (0,4), (2,2)**, and an obstacle at **(0,2)**:

```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point **(1,2)** is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

**Note:**

There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

###思路：
使用bfs思路。

建立两个二维数组，一个存储每个点能被多少个房子（为1的点）走到，一个存储从这些房子走到的最小路径长度累加和。

分别从每个房子出发，进行bfs，更新以上的两个二维数组，走完所有房子，找到那个被所有房子都走到过，且路径类加和最小的点，那么就找到了最小路径和。

进行bfs的时候有一些小技巧：

假设现在已经处理了n个房子，第n+1个房子在bfs的时候，他可以拓展的点是那些已经被n个房子路过的点。

###代码：

```
class Solution {

public:
    struct Node{
        int x, y;
        int depth;
        Node(int _x, int _y, int d) : x(_x), y(_y), depth(d) {}
    };
    
    int go[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};
    
    int shortestDistance(vector<vector<int>>& grid) {
        int m = grid.size();
        if(m <= 0) return -1;
        int n = grid[0].size();
        
        vector<vector<int>> visi(m, vector<int>(n, 0));
        vector<vector<int>> cnt(m, vector<int>(n, 0));
        int bNum = 0, res = INT_MAX;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                if(grid[i][j] == 1) bNum++;
            }
        }
        
        int bnow = 0;
        for(int i = 0; i < m; ++i) {
            for(int j = 0; j < n; ++j) {
                
                if(grid[i][j] == 1) {
                    bnow++;
                    vector<vector<bool>> v(m, vector<bool>(n, false));
                    queue<Node> Q;
                    Q.push(Node(i, j, 0));
                    v[i][j] = true;
                    while(!Q.empty()) {
                        Node tmp = Q.front();
                        Q.pop();
                        for(int k = 0; k < 4; ++k) {
                            int nowx = tmp.x + go[k][0];
                            int nowy = tmp.y + go[k][1];
                            if(nowx >= 0 && nowx < m && nowy >= 0 && nowy < n && grid[nowx][nowy] == 0 && visi[nowx][nowy] == bnow-1 && v[nowx][nowy] == false)
                            { 
                                visi[nowx][nowy]++;
                                cnt[nowx][nowy] += (tmp.depth + 1);
                                Q.push(Node(nowx, nowy, tmp.depth + 1));
                                v[nowx][nowy] = true;
                                if(bnow == bNum) res = min(res, cnt[nowx][nowy]);
                            }
                        }
                    }
                    if(bnow == bNum) return res == INT_MAX ? -1 : res;
                }
            }
        }
        return res == INT_MAX ? -1 : res;
        
    }
};
```

##320. Generalized Abbreviation
###题目：
Write a function to generate the generalized abbreviations of a word.

**Example:**

Given word = **"word"**, return the following list (order does not matter):

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```
###思路：
主要区分好两个数字不可以重复出现，dfs的时候传入上一步是否使用了数字。

每一步都可以将拼写加上word目前处理的这一位字符，或者使用数字代替，数字代替的个数可以循环遍历。
###代码：

```
vector<string> generateAbbreviations(string word) {
    int len = word.size();
    vector<string> res;
    dfs(word, res, 0, false, "", len);
    return res;
}

void dfs(string &word, vector<string> &res, int i, bool yes, string now, const int len) {
    if(i == len) {
        res.push_back(now);
        return;
    }
    dfs(word, res, i + 1, false, now + word[i], len);
    if(!yes) {
        for(int j = 1; j + i <= len; ++j) {
            dfs(word, res, i + j, true, now + to_string(j), len);
        }
    }
}
```



##323. Number of Connected Components in an Undirected Graph
###题目：
Given **n** nodes labeled from **0** to **n - 1** and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

**Example 1:**

```
     0          3
     |          |
     1 --- 2    4
```
Given **n = 5** and **edges = [[0, 1], [1, 2], [3, 4]]**, return **2**.

**Example 2:**

```
     0           4
     |           |
     1 --- 2 --- 3
```
Given **n = 5** and **edges = [[0, 1], [1, 2], [2, 3], [3, 4]]**, return **1**.

**Note:**

You can assume that no duplicate edges will appear in **edges**. Since all edges are undirected, **[0, 1]** is the same as **[1, 0]** and thus will not appear together in edges.
###思路：
并查集思想。

答案就是最后的集合数。
###代码：

```
int find(vector<int> &f, int u) {
    if(f[u] == u) return u;
    else {
        return f[u] = find(f, f[u]);
    }
}
int countComponents(int n, vector<pair<int, int>>& edges) {
    vector<int> f(n);
    if(n <= 0) return 0;
    
    for(int i = 0; i < n; ++i) {
        f[i] = i;
    }
    for(auto edge : edges) {
        int u = edge.first, v = edge.second;
        int fu = find(f, u), fv = find(f, v);
        if(fu != fv) f[fu] = fv;
    }
    set<int> all;
    for(int i = 0; i < n; ++i) {
        all.insert(find(f, f[i]));
    }
    return all.size();
}
```
##325. Maximum Size Subarray Sum Equals k
###题目：
Given an array nums and a target value *k*, find the maximum length of a subarray that sums to *k*. If there isn't one, return 0 instead.

**Note:**

The sum of the entire *nums* array is guaranteed to fit within the 32-bit signed integer range.

**Example 1:**

Given nums = **[1, -1, 5, -2, 3]**, k = **3**,
return **4**. (because the subarray **[1, -1, 5, -2]** sums to 3 and is the longest)

**Example 2:**

Given nums = **[-2, -1, 2, 1]**, k = **1**,
return **2**. (because the subarray **[-1, 2]** sums to 1 and is the longest)

**Follow Up:**

Can you do it in O(n) time?

###思路：
hash和前缀和的思想。

用hash[sum]=i；即前i个元素取到和为sum。

处理到一个元素的时候判断其前缀和sum[i]是否和k相等，相等则更新答案为res = max(res, i + 1);

否则判断sum[i]-k是否是出现过的前缀和，如果出现过，那么这两个前缀和中间的部分可更新答案。

###代码：

```
int maxSubArrayLen(vector<int>& nums, int k) {
    int len = nums.size();
    if(len == 0) return 0;
    
    unordered_map<int, int> all;
    int res = 0, cur_sum = 0;
    
    for(int i = 0; i < len; ++i) {
        cur_sum += nums[i];
        if(cur_sum == k) res = max(res, i + 1);
        if(all.find(cur_sum-k) != all.end()) {
            res = max(res, i-all[cur_sum-k]);
        }
        if(all.find(cur_sum) == all.end()) {
            all[cur_sum] = i;
        }
    }
    return res;
}
```

##333. Largest BST Subtree
###题目：
Given a binary tree, find the largest subtree which is a Binary Search Tree (BST), where largest means subtree with largest number of nodes in it.

**Note:**

A subtree must include all of its descendants.

Here's an example:

```
    10
    / \
   5*  15
  / \   \ 
 1*   8* 7
```
The Largest BST Subtree in this case is the highlighted one. 

The return value is the subtree's size, which is 3.

**Hint:**

* You can recursively use algorithm similar to **98. Validate Binary Search Tree** at each node of the tree, which will result in O(nlogn) time complexity.

**Follow up:**

Can you figure out ways to solve it with O(n) time complexity?
###思路：
构造一个新的结构体，存储以当前节点为根的BST的最小值，最大值和大小，若不是BST，则大小为-1。

dfs遍历整个树，全局变量res记录最大的BST的大小。
###代码：

```
class Solution {
public:
    struct Node {
        int size, low, up;
        Node(int _size, int _low, int _up) : size(_size), low(_low), up(_up) {}
    };
    
    int largestBSTSubtree(TreeNode* root) {
        res = 0;
        dfs(root);
        return res;
    }
    
    Node dfs(TreeNode* root) {
        if(root == NULL) {
            return Node(0, INT_MAX, INT_MIN);
        }
        
        Node leftNode = dfs(root->left);
        Node rightNode = dfs(root->right);
        
        if(leftNode.size == -1 || rightNode.size == -1 || root->val < leftNode.up || root->val > rightNode.low) {
            return Node(-1, 0, 0);
        }
        int num = leftNode.size + rightNode.size + 1;
        res = max(res, num);
        return Node(num, min(root->val, leftNode.low), max(root->val, rightNode.up));
    }
    
private:
    int res;
};
```

##339. Nested List Weight Sum
###题目：
Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Example 1:**

Given the list **[[1,1],2,[1,1]]**, return 10. (four 1's at depth 2, one 2 at depth 1)

**Example 2:**

Given the list **[1,[4,[6]]]**, return 27. (one 1 at depth 1, one 4 at depth 2, and one 6 at depth 3; 1 + 4 * 2 + 6 * 3 = 27)
###思路：
成员函数都是"对象.函数"的使用方法。

运用递归，传递一个深度的参数即可。
###代码：

```
int dfs(vector<NestedInteger>& nestedList, int depth) {
    int res = 0;
    for(auto now : nestedList) {
        if(now.isInteger()) res += depth * now.getInteger();
        else res += dfs(now.getList(), depth + 1);
    }
    return res;
}
int depthSum(vector<NestedInteger>& nestedList) {
    return dfs(nestedList, 1);
}
```

##340. Longest Substring with At Most K Distinct Characters
###题目：
Given a string, find the length of the longest substring T that contains at most k distinct characters.

For example, Given s = **“eceba”** and k = 2,

T is "ece" which its length is 3.
###思路：
1、左右两个指针移动来遍历字符串，如果不同的字母个数不足k，那么右指针向右移动；如果不同的字母个数超过k，那么左指针向右移动；

2、指针移动的同时更新记录字母个数的hash，hash更新的同时更新不同字母的个数，如果某个字母个数在hash更新后变为0，则字母个数减一，相反加一；

3、每次指针移动，如果字母个数小于k，曾更新答案。
###代码：

```
int lengthOfLongestSubstringKDistinct(string s, int k) {
    int len = s.size();
    if(len <= 0 || k <= 0) return 0; 
    int res = 0, diff = 0, l = 0, r = 0;
    unordered_map<char, int> hash;
    while(l <= r && r < len) {
        if(diff <= k) {
            hash[s[r]]++;
            if(hash[s[r]] == 1) diff++;
            r++;
        }else {
            hash[s[l]]--;
            if(hash[s[l]] == 0) diff--;
            l++;
        }
        if(diff <= k) res = max(res, r-l);
    }
    return res;
}
```


##346. Moving Average from Data Stream
###题目：
Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.

For example,

```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```
###思路：
private变量可以记录窗口的大小和窗口中数字的和，避免需要遍历queue。
###代码：

```
class MovingAverage {
private:
    queue<int> q;
    int len, sum;
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) {
        len = size;
        sum = 0;
    }
    
    double next(int val) {
        if((int)q.size() < len) {
            q.push(val);
            sum += val;
        }else {
            sum -= q.front();
            q.pop();
            q.push(val);
            sum += val;
        }
        int n = q.size();
        return n == 0 ? 0 : (double)sum / n;
    }
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage obj = new MovingAverage(size);
 * double param_1 = obj.next(val);
 */
```
##348. Design Tic-Tac-Toe
###题目：
Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:

* A move is guaranteed to be valid and is placed on an empty block.
* Once a winning condition is reached, no more moves is allowed.
* A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.

**Example:**

```
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```
**Follow up:**

Could you do better than O(n2) per move() operation?

**Hint:**

* Could you trade extra space such that move() operation can be done in O(1)?
* You need two arrays: int rows[n], int cols[n], plus two variables: diagonal, anti_diagonal.

###思路：
1、用两个数组分别表示某行某列某个选手下的棋子的个数，如果个数达到n，则说明这一行或者一列都是这个选手的棋子，即可说明这个选手获胜；

2、这里运用了一个技巧，选手1下棋+1，选手二-1，则如果结果等于n，说明是选手1下了n次，等于-n，则说明选手二下了n次，只要不是这两种说明两个选手都下了，都无法成功。

3、初始化方法要注意掌握。
###代码：

```
class TicTacToe {
private:
    vector<int> Row;
    vector<int> Col;
    int Dia1;
    int Dia2;
    int num;
public:
    /** Initialize your data structure here. */
    TicTacToe(int n) : num(n), Row(n), Col(n), Dia1(0), Dia2(0){}
    
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    int move(int row, int col, int player) {
        int sig = player == 1 ? 1 : -1;
        Row[row] += sig;
        Col[col] += sig;
        if(row == col) Dia1 += sig;
        if(row + col == num-1) Dia2 += sig;
        if(Row[row] == num || Col[col] == num || Dia1 == num || Dia2 == num) return 1;
        else if(Row[row] == -num || Col[col] == -num || Dia1 == -num || Dia2 == -num) return 2;
        else return 0;
    }
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

##351. Android Unlock Patterns
###题目：
Given an Android **3x3** key lock screen and two integers m and n, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of m keys and maximum n keys.

**Rules for a valid pattern:**

* Each pattern must connect at least m keys and at most n keys.
* All the keys must be distinct.
* If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
* The order of keys used matters.

**Explanation:**

```
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |
```
Invalid move: **4 - 1 - 3 - 6** 

Line 1 - 3 passes through key 2 which had not been selected in the pattern.

Invalid move: **4 - 1 - 9 - 2**

Line 1 - 9 passes through key 5 which had not been selected in the pattern.

Valid move: **2 - 4 - 1 - 3 - 6**

Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

Valid move: **6 - 5 - 4 - 1 - 9 - 2**

Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

**Example:**

Given m = 1, n = 1, return 9.

**Credits:**

Special thanks to @elmirap for adding this problem and creating all test cases.

###思路：
dfs思路。

以1-9中任意一个数字开始，Dfs剩下的数字，如果没有遍历过并且他们之间没有跨过未处理的数字，那么进入下一层的遍历。
###代码：

```
class Solution {
private:
    int skip[10][10];
    bool yes[10];
    int res = 0;
public:
    void init() {
        memset(skip, -1, sizeof(skip));
        skip[1][3] = 2;
        skip[4][6] = 5;
        skip[7][9] = 8;
        skip[1][7] = 4;
        skip[2][8] = 5;
        skip[3][9] = 6;
        skip[1][9] = 5;
        skip[3][7] = 5;
        
        skip[3][1] = 2;
        skip[6][4] = 5;
        skip[9][7] = 8;
        skip[7][1] = 4;
        skip[8][2] = 5;
        skip[9][3] = 6;
        skip[9][1] = 5;
        skip[7][3] = 5;
    }
    
    void dfs(int now, int num, const int m, const int n) {
        yes[now] = 1;
        if(num >= m && num <= n) {
            res++;
        }
        if(num > n) {
            yes[now] = 0;
            return;
        }
        for(int i = 1; i <= 9; ++i) {
            if(yes[i]) continue;
            if(skip[now][i] != -1 && yes[skip[now][i]] == false) continue;
            dfs(i, num+1, m, n);
        }
        yes[now] = 0;
    }
    
    int numberOfPatterns(int m, int n) {
        init();
        for(int i = 1; i <= 9; ++i) {
            dfs(i, 1,  m, n);
        }
        return res;
    }
};
```

##353. Design Snake Game
###题目：
Design a Snake game that is played on a device with screen size = width x height. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

**Example:**

```
Given width = 3, height = 2, and food = [[1,2],[0,1]].

Snake snake = new Snake(width, height, food);

Initially the snake appears at position (0,0) and the food at (1,2).

|S| | |
| | |F|

snake.move("R"); -> Returns 0

| |S| |
| | |F|

snake.move("D"); -> Returns 0

| | | |
| |S|F|

snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )

| |F| |
| |S|S|

snake.move("U"); -> Returns 1

| |F|S|
| | |S|

snake.move("L"); -> Returns 2 (Snake eats the second food)

| |S|S|
| | |S|

snake.move("U"); -> Returns -1 (Game over because snake collides with border)
```

###思路：
代码如下。
###代码：

```
class SnakeGame {
public:
/** Initialize your data structure here.
@param width - screen width
@param height - screen height
@param food - A list of food positions
E.g food = [[1,1], [1,0]] means the first food is positioned at [1,1], the second is at [1,0]. */

int w, h, pos;
vector<pair<int, int>> food;
set<pair<int, int>> hist;
deque<pair<int, int>> q;

SnakeGame(int width, int height, vector<pair<int, int>> food) {
	this->food = food;
	w = width, h = height, pos = 0;
	q.push_back(make_pair(0, 0));
}

/** Moves the snake.
@param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
@return The game's score after the move. Return -1 if game over.
Game over when snake crosses the screen boundary or bites its body. */
int move(string direction) {
	int row = q.back().first, col = q.back().second;
	pair<int, int> d = q.front(); q.pop_front();
	hist.erase(d);

	if (direction == "U")
		row--;
	else if (direction == "D")
		row++;
	else if (direction == "L")
		col--;
	else if (direction == "R")
		col++;

	if (row < 0 || col < 0 || col >= w || row >= h || hist.count(make_pair(row, col)))
		return -1;

	hist.insert(make_pair(row, col));
	q.push_back(make_pair(row, col));

	if (pos >= food.size())
		return food.size();

	if (row == food[pos].first && col == food[pos].second) {
		pos++;
		q.push_front(d);
		hist.insert(d);
	}

	return pos;
  }
};
```

##356. Line Reflection
###题目：
Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

**Example 1:**

Given points = **[[1,1],[-1,1]]**, return **true**.

**Example 2:**

Given points = **[[1,1],[-1,-1]]**, return **false**.

**Follow up:**

Could you do better than O(n2)?

**Hint:**

* Find the smallest and largest x-value for all points.
* If there is a line then it should be at y = (minX + maxX) / 2.
* For each point, make sure that it has a reflected point in the opposite side.

###思路：


###代码：

```
//trick n == 0 and repetition points
class Solution {
public:
    unordered_map<int, vector<int>> mp;
    bool isReflected(vector<pair<int, int>>& points) {
        int n = points.size();
        if (n < 1) return true;
        
        mp.clear();
        int maxX = INT_MIN, minX = INT_MAX;
        for (auto p : points) {
            maxX = max(maxX, p.first);
            minX = min(minX, p.first);
            mp[p.second].push_back(p.first);
        }
        
        double midLine = (minX + maxX) / 2.0;
        for (auto &it : mp) {
            vector<int>& vec = it.second;
            sort(vec.begin(), vec.end());
            auto iter = unique(vec.begin(), vec.end()); 
            vec.erase(iter, vec.end()); 
            int l = 0, r = vec.size() - 1;
            while (l <= r) {
                if (fabs( (vec[l] + vec[r]) / 2.0 - midLine ) > 1e-8) 
                	return false;
                l++; r--;
            }
        }
        return true;
    }
};


bool isReflected(vector<pair<int, int>>& points) {
    unordered_map<int, vector<int>> hashy;
    set<pair<int, int>> all;
    int len = points.size();
    if(len <= 0) return true;
    int minx = points[0].first, maxx = points[0].first;
    for(int i = 0; i < len; ++i) {
        if(all.find(points[i]) != all.end()) continue;
        all.insert(points[i]);
        int x = points[i].first;
        int y = points[i].second;
        minx = min(minx, x);
        maxx = max(maxx, x);
        if(hashy.find(y) == hashy.end()) {
            hashy[y] = {x, 1};
        }else {
            hashy[y][0] += x;
            hashy[y][1] += 1;
        }
        
    }
    double mid = (double)(minx + maxx) / 2;
    for(auto cur : hashy) {
        double mm = (double)cur.second[0] / cur.second[1];
        if(mm != mid) return false;
    }
    return true;
}
```
##358. Rearrange String k Distance Apart
###题目：
Given a non-empty string str and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

**Example 1:**

```
str = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.
```
**Example 2:**

```
str = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.
```
**Example 3:**

```
str = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.
```
###思路：
本质是要保证在距离k之内不能出现相同的字母，处理的思路是先处理出现个数多的那些字母。

使用节点数据结构，记录字母和他相应的个数。

用优先队列来存储每个节点，排序依据是先按照字母出现个数从大到小，一样的时候根据字典序，从小到大。

维护还未生成的个数len，每次处理m = min(k, len)，一次处理过程中出现队列空，则返回“”，如果非空，将其头结点取出，放入答案，出现次数-1，如果次数还没有为0，那么存储在一个数组里面，处理完一次之后将临时的数组中的节点都放到优先队列中，len = len - m。
###代码：

```
class NODE {
public:
    char ch;
    int num;
    NODE(char c, int n) : ch(c), num(n) {}
};
class Cmp {
public:
    bool operator() (NODE a, NODE b) {
        if(a.num == b.num) return a.ch > b.ch;
        return a.num < b.num;
    }
};

class Solution {
public:
    string rearrangeString(string str, int k) {
        string res = "";
        int len = str.size();
        if(k <= 1) return str;
        unordered_map<char, int> H;
        priority_queue<NODE, vector<NODE>, Cmp> Q;
        for(char x : str) H[x]++; 
        for(auto x : H) Q.push(NODE(x.first, x.second));
        
        while(!Q.empty()) {
            int n = min(k, len);
            vector<NODE> tmp;
            while(n) {
                if(Q.empty()) return "";
                NODE now = Q.top();
                Q.pop();
                res += now.ch;
                now.num--;
                if(now.num > 0) tmp.push_back(now);
                n--;
                len--;
            }
            for(NODE x : tmp) Q.push(x);
        }
        return res;
        
    }
};
```

##359. Logger Rate Limiter
###题目：
Design a logger system that receive stream of messages along with its timestamps, each message should be printed if and only if it is **not printed in the last 10 seconds**.

Given a message and a timestamp (in seconds granularity), return true if the message should be printed in the given timestamp, otherwise returns false.

It is possible that several messages arrive roughly at the same time.

**Example:**

```
Logger logger = new Logger();

// logging string "foo" at timestamp 1
logger.shouldPrintMessage(1, "foo"); returns true; 

// logging string "bar" at timestamp 2
logger.shouldPrintMessage(2,"bar"); returns true;

// logging string "foo" at timestamp 3
logger.shouldPrintMessage(3,"foo"); returns false;

// logging string "bar" at timestamp 8
logger.shouldPrintMessage(8,"bar"); returns false;

// logging string "foo" at timestamp 10
logger.shouldPrintMessage(10,"foo"); returns false;

// logging string "foo" at timestamp 11
logger.shouldPrintMessage(11,"foo"); returns true;
```
###思路：
注意时间戳可以为0，判断某个字符串没有加入过要判断在map里面找不到它，而不是它的值是0.
###代码：

```
class Logger {
private:
    unordered_map<string, int> hash;
public:
    /** Initialize your data structure here. */
    Logger() {
        hash.clear();
    }
    
    /** Returns true if the message should be printed in the given timestamp, otherwise returns false.
    If this method returns false, the message will not be printed.
    The timestamp is in seconds granularity. */
    bool shouldPrintMessage(int timestamp, string message) {
        if(hash.find(message) == hash.end() || timestamp - hash[message] >= 10) {
            hash[message] = timestamp;
            return true;
        }else {
            return false;
        }
    }
};

/**
 * Your Logger object will be instantiated and called as such:
 * Logger obj = new Logger();
 * bool param_1 = obj.shouldPrintMessage(timestamp,message);
 */
```


##360. Sort Transformed Array
###题目：
Given a **sorted** array of integers nums and integer values a, b and c. Apply a function of the form *f(x) = ax2 + bx + c* to each element x in the array.

The returned array must be in **sorted order**.

Expected time complexity: **O(n)**

**Example:**

```
nums = [-4, -2, 2, 4], a = 1, b = 3, c = 5,

Result: [3, 9, 15, 33]

nums = [-4, -2, 2, 4], a = -1, b = 3, c = 5

Result: [-23, -5, 1, 7]
```
###思路：
1、最根本的是要对fx排序，可以直接算出来，然后排序，时间复杂度是O(nlogn)。

2、利用一元二次函数的性质。

3、首先如果a为0的话，那么fx也是有序的，b > 0的话，本身就是升序排列，否则是降序，reverse结果即可；

4、如果a不是0，那么利用抛物线的性质，-b/2a是极值点，距离极值点越远的点越大或者越小，用two pointer从两边向中间夹，根据点距离极值点的距离，按照从远到近的顺序插入结果，如果a>0，那么reverse结果即可。
###代码：

```
int fx(int x, int a, int b, int c) {
    return a * x * x + b * x + c;
}
vector<int> sortTransformedArray(vector<int>& nums, int a, int b, int c) {
    vector<int> res;
    int len = nums.size();
    if(len <= 0) return res;
    if(a == 0) {
        for(int i = 0; i < len; ++i) {
            res.push_back(fx(nums[i], a, b, c));
        }
        if(b < 0) reverse(res.begin(), res.end());
        return res;
    }
    int l = 0, r = len-1;
    double mid = -1 * (double)b / (2 * (double)a);
    while(l <= r) {
        double ll = fabs((double)nums[l] - mid), rr = fabs((double)nums[r] - mid);
        if(ll > rr) {
            res.push_back(fx(nums[l], a, b, c));
            l++;
        }else {
            res.push_back(fx(nums[r], a, b, c));
            r--;
        }
    }
    if(a > 0) reverse(res.begin(), res.end());
    return res;
}
```



##361. Bomb Enemy
###题目：
Given a 2D grid, each cell is either a wall **'W'**, an enemy **'E'** or empty **'0'** (the number zero), return the maximum enemies you can kill using one bomb.
The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.
Note that you can only put the bomb at an empty cell.

**Example:**

```
For the given grid

0 E 0 0
E 0 W E
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)
```
###思路：
方法1：

最暴力的方法：时间复杂度：O(m * n * (m+n))

从每一个可以放炸弹的地方开始，向上下左右四个方向，计算最多能炸到的敌人数。

方法2：

DP，时间复杂度：O(m * n)

维护在每个点上向上、下、左、右，四个方向最多能炸多少个敌人。

###代码：

```
int maxKilledEnemies(vector<vector<char>>& grid) {
    int m = grid.size();
    if(m <= 0) return 0;
    int n = grid[0].size();
    int dp1[m+1][n+1][2], dp2[m+1][n+1][2];
    memset(dp1, 0, sizeof(dp1));
    memset(dp2, 0, sizeof(dp2));
    
    for(int i = 0; i < m; ++i) {
        for(int j = 0; j < n; ++j) {
            if(grid[i][j] == 'W') dp1[i+1][j+1][0] = dp1[i+1][j+1][1] = 0;
            else {
                dp1[i+1][j+1][0] = dp1[i+1][j][0] + (grid[i][j] == 'E');
                dp1[i+1][j+1][1] = dp1[i][j+1][1] + (grid[i][j] == 'E');
            }
        }
    }
    
    for(int i = m - 1; i >= 0; --i) {
        for(int j = n - 1; j >= 0; --j) {
            if(grid[i][j] == 'W') dp2[i][j][0] = dp2[i][j][1] = 0;
            else {
                dp2[i][j][0] = dp2[i][j+1][0] + (grid[i][j] == 'E');
                dp2[i][j][1] = dp2[i+1][j][1] + (grid[i][j] == 'E');
            }
        }
    }
    int res = 0;
    for(int i = 0; i < m; ++i) {
        for(int j = 0; j < n; ++j) {
            if(grid[i][j] == '0') {
                int now = dp1[i+1][j+1][0] + dp1[i+1][j+1][1] + dp2[i][j][0] + dp2[i][j][1];
                res = max(res, now);
            }
        }
    }
    return res;
}
```

##362. Design Hit Counter
###题目：
Design a hit counter which counts the number of hits received in the past 5 minutes.

Each function accepts a timestamp parameter (in seconds granularity) and you may assume that calls are being made to the system in chronological order (ie, the timestamp is monotonically increasing). You may assume that the earliest timestamp starts at 1.

It is possible that several hits arrive roughly at the same time.

**Example:**

```
HitCounter counter = new HitCounter();

// hit at timestamp 1.
counter.hit(1);

// hit at timestamp 2.
counter.hit(2);

// hit at timestamp 3.
counter.hit(3);

// get hits at timestamp 4, should return 3.
counter.getHits(4);

// hit at timestamp 300.
counter.hit(300);

// get hits at timestamp 300, should return 4.
counter.getHits(300);

// get hits at timestamp 301, should return 3.
counter.getHits(301); 
```
**Follow up:**

What if the number of hits per second could be very large? Does your design scale?
###思路：
1、利用hash来记录每个时间点进入的个数；

2、用queue来按照时间顺序记录有哪些时间点，因为这里是按照时间顺序进入的，所以普通队列即可，如果并不按照时间顺序，需要使用优先队列；

3、sum记录现在有效的，即在当前询问的时间点向前5分钟的时间段内的进入个数。这个需要在每次访问的时候先删去queue前面位于有效时间以外的时间点，删去时间点的同时将hash里面相应时间点的个数置为0，sum减去相应个数。
###代码：

```
class HitCounter {
private:
    unordered_map<int, int> hash;
    queue<int> line;
    int sum;
public:
    /** Initialize your data structure here. */
    HitCounter() {
        hash.clear();
        while(!line.empty()) line.pop();
        sum = 0;
    }
    
    /** Record a hit.
        @param timestamp - The current timestamp (in seconds granularity). */
    void hit(int timestamp) {
        hash[timestamp]++;
        if(hash[timestamp] == 1) line.push(timestamp);
        sum++;
    }
    
    /** Return the number of hits in the past 5 minutes.
        @param timestamp - The current timestamp (in seconds granularity). */
    int getHits(int timestamp) {
        if(line.empty()) return 0;
        while(!line.empty() && line.front() <= timestamp - 300) {
            int now = line.front();
            sum -= hash[now];
            hash[now] = 0;
            line.pop();
        }
        return sum;
    }
};

/**
 * Your HitCounter object will be instantiated and called as such:
 * HitCounter obj = new HitCounter();
 * obj.hit(timestamp);
 * int param_2 = obj.getHits(timestamp);
 */
```

##364. Nested List Weight Sum II
###题目：
Given a nested list of integers, return the sum of all integers in the list weighted by their depth.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Different from the previous question where weight is increasing from root to leaf, now the weight is defined from bottom up. i.e., the leaf level integers have weight 1, and the root level integers have the largest weight.

**Example 1:**

Given the list **[[1,1],2,[1,1]]**, return **8**. (four 1's at depth 1, one 2 at depth 2)

**Example 2:**

Given the list **[1,[4,[6]]]**, return **17**. (one 1 at depth 3, one 4 at depth 2, and one 6 at depth 1; 1 * 3 + 4 * 2 + 6 * 1 = 17)

###思路：
dfs时传入当前的深度；

利用hash来记录每一层节点的和；

利用全局变量记录总的层数，最后将每一层的加权加和进行叠加。

###代码：

```
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Constructor initializes an empty nested list.
 *     NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     NestedInteger(int value);
 *
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Set this NestedInteger to hold a single integer.
 *     void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     void add(const NestedInteger &ni);
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */
class Solution {
private:
    unordered_map<int, int> hash;
    int maxL;
public:
    void dfs(vector<NestedInteger>& nestedList, int level) {
        maxL = max(maxL, level);
        for(auto nn : nestedList) {
            if(nn.isInteger()) hash[level] += nn.getInteger();
            else {
                dfs(nn.getList(), level + 1);
            }
        }
    }
    
    int depthSumInverse(vector<NestedInteger>& nestedList) {
        if(nestedList.size() == 0) return 0;
        int now = 1;
        maxL = 1;
        for(auto nn : nestedList) {
            if(nn.isInteger()) hash[now] += nn.getInteger();
            else {
                dfs(nn.getList(), now + 1);
            }
        }
        int res = 0;
        for(int i = 1; i <= maxL; ++i) {
            
            res += hash[i] * (maxL + 1 - i);
        }
        return res;
    }
};
```
##366. Find Leaves of Binary Tree
###题目：
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

**Example:**

Given binary tree 

```
          1
         / \
        2   3
       / \     
      4   5    
```
Returns **[4, 5, 3], [2], [1]**.

**Explanation:**

* Removing the leaves **[4, 5, 3]** would result in this tree:

```
          1
         / 
        2      
```
* Now removing the leaf **[2]** would result in this tree:

```
          1          
```
* Now removing the leaf **[1]** would result in the empty tree:

```
          []         
```
Returns **[4, 5, 3], [2], [1]**.

###思路：
这个题的根本是求每个节点到距离他最远的叶子节点的长度，所以还是相应层数的节点值存入相应层数的结果中，dfs过程中记录层数。

如果结果的大小小于等于层数，那么在结果中新建立一个vector<int>()，存入结果。
###代码：

```
int dfs(TreeNode* root, vector<vector<int>> &res) {
    if(!root) return -1;
    int left = dfs(root->left, res);
    int right = dfs(root->right, res);
    int depth = max(left, right) + 1;
    if(res.size() <= depth) res.push_back(vector<int>());
    res[depth].push_back(root->val);
    return depth;
}
vector<vector<int>> findLeaves(TreeNode* root) {
    vector<vector<int>> res;
    int d = dfs(root, res);
    return res;
}
```

##369. Plus One Linked List
###题目：
Given a non-negative number represented as a singly linked list of digits, plus one to the number.

The digits are stored such that the most significant digit is at the head of the list.

**Example:**

```
Input:
1->2->3

Output:
1->2->4
```
###思路：
递归得返回每一位的进位，并在返回的同时修改当前位的值。

如果第一个节点有进位，那么新建节点，否则返回第一个节点。

###代码：

```
ListNode* plusOne(ListNode* head) {
    if(!head) return head;
    int nn = add(head);
    if(nn) {
        ListNode* newHead = new ListNode(nn);
        newHead->next = head;
        return newHead;
    }else {
        return head;
    }
}
int add(ListNode* head) {
    int num;
    if(head->next == NULL) {
        num = head->val + 1;
    }else {
        num = head->val + add(head->next);
    }
    if(num < 10) {
        head->val = num;
        return 0;
    }else {
        head->val = 0;
        return 1;
    }
}
```

##370. Range Addition
###题目：
Assume you have an array of length **n** initialized with all **0**'s and are given **k** update operations.

Each operation is represented as a triplet: **[startIndex, endIndex, inc]** which increments each element of subarray **A[startIndex ... endIndex]** (startIndex and endIndex inclusive) with **inc**.

Return the modified array after all **k** operations were executed.

**Example:**

```
Given:

    length = 5,
    updates = [
        [1,  3,  2],
        [2,  4,  3],
        [0,  2, -2]
    ]

Output:

    [-2, 0, 3, 5, 3]
```
**Explanation:**

```
Initial state:
[ 0, 0, 0, 0, 0 ]

After applying operation [1, 3, 2]:
[ 0, 2, 2, 2, 0 ]

After applying operation [2, 4, 3]:
[ 0, 2, 5, 5, 3 ]

After applying operation [0, 2, -2]:
[-2, 0, 3, 5, 3 ]
```
**Hint:**

* Thinking of using advanced data structures? You are thinking it too complicated.
* For each update operation, do you really need to update all elements between i and j?
* Update only the first and end element is sufficient.
* The optimal time complexity is O(k + n) and uses O(1) extra space.

###思路：
1、首先建立一个长度比length大1的数组，初始值为0；

2、然后每次更新只将需要更新的第一个位置加上更新值，最后一个位置后面的一位减去更新值；

3、最后从第二个位置，从左向右遍历一次数组，后一位加上前一位的数字，这样需要更新内容的部分即可更新，不需要更新的也可以抵消影响。

###代码：

```
vector<int> getModifiedArray(int length, vector<vector<int>>& updates) {
    vector<int> res(length+1);
    for(auto u : updates) {
        res[u[0]] += u[2];
        res[u[1]+1] -= u[2];
    }
    for(int i = 1; i < length; ++i) {
        res[i] += res[i-1];
    }
    res.pop_back();
    return res;
}
```

##379. Design Phone Directory
###题目：
Design a Phone Directory which supports the following operations:

* **get**: Provide a number which is not assigned to anyone.
* **check**: Check if a number is available or not.
* **release**: Recycle or release a number.

**Example:**

```
// Init a phone directory containing a total of 3 numbers: 0, 1, and 2.
PhoneDirectory directory = new PhoneDirectory(3);

// It can return any available phone number. Here we assume it returns 0.
directory.get();

// Assume it returns 1.
directory.get();

// The number 2 is available, so return true.
directory.check(2);

// It returns 2, the only number that is left.
directory.get();

// The number 2 is no longer available, so return false.
directory.check(2);

// Release number 2 back to the pool.
directory.release(2);

// Number 2 is available again, return true.
directory.check(2);
```
###思路：
主要是注意实现。

思路就是一个队列或者链表来表示还可以分配的号码，一个数组来表示哪些号码已经用过。初始化队列放入全部号码，数组全部为false；

实现get函数：如果队列不为空，返回并且删除队列头，将数组相应值设为true；

实现check函数：查询数组中相应位置为true则为用过，false则为没用过；

实现release函数：如果用过，将数组设为false,并将其加入到队列中，否则什么都不做。
###代码：

```
class PhoneDirectory {
private:
    int maxNum;
    queue<int> q;
    bool *s;
public:
    /** Initialize your data structure here
        @param maxNumbers - The maximum numbers that can be stored in the phone directory. */
    PhoneDirectory(int maxNumbers) {
        maxNum = maxNumbers;
        s = new bool[maxNum];
        memset(s, false, sizeof(s));
        for(int i = 0; i < maxNumbers; ++i) {
            q.push(i);
        }
    }
    
    /** Provide a number which is not assigned to anyone.
        @return - Return an available number. Return -1 if none is available. */
    int get() {
        if(q.empty()) return -1;
        else {
            int now = q.front();
            q.pop();
            s[now] = true;
            return now;
        }
    }
    
    /** Check if a number is available or not. */
    bool check(int number) {
        return !s[number];
    }
    
    /** Recycle or release a number. */
    void release(int number) {
        if (!s[number]) return;
        s[number] = false;
        q.push(number);
    }
};


/**
 * Your PhoneDirectory object will be instantiated and called as such:
 * PhoneDirectory obj = new PhoneDirectory(maxNumbers);
 * int param_1 = obj.get();
 * bool param_2 = obj.check(number);
 * obj.release(number);
 */
```


##408. Valid Word Abbreviation
###题目：
Given a non-empty string **s** and an abbreviation **abbr**, return whether the string matches with the given abbreviation.

A string such as **"word"** contains only the following valid abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```
Notice that only the above abbreviations are valid abbreviations of the string **"word"**. Any other string is not a valid abbreviation of **"word"**.

**Note:**

Assume **s** contains only lowercase letters and **abbr** contains only lowercase letters and digits.

**Example 1:**

```
Given s = "internationalization", abbr = "i12iz4n":

Return true.
```
**Example 2:**

```
Given s = "apple", abbr = "a2e":

Return false.
```
###思路：
两个指针分别遍历word和abbr。
###代码：

```
bool validWordAbbreviation(string word, string abbr) {
    int lenw = word.length(), lena = abbr.length();
    int i = 0, j = 0;
    while(i < lena) {
        if (abbr[i] >= 'a' && abbr[i] <= 'z') {
            if (j >= lenw || word[j] != abbr[i]) return false;
            i++, j++;
        } else {
            int cur = 0;
            while (i < lena && abbr[i] >= '0' && abbr[i] <= '9') {
                cur = 10 * cur + (abbr[i++] - '0');
                if (cur == 0) return false;
            }
            j += cur; 
            if (j > lenw) return false;
        }
    }
    return (i == lena && j == lenw);
}
```

##411. Minimum Unique Word Abbreviation **
###题目：
A string such as **"word"** contains the following abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```
Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the ***smallest possible*** length such that it does not conflict with abbreviations of the strings in the dictionary.

Each **number** or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.

**Note:**

* In the case of multiple answers as shown in the second example below, you may return any one of them.
* Assume length of target string = **m**, and dictionary size = **n**. You may assume that **m ≤ 21, n ≤ 1000**, and **log2(n) + m ≤ 20**.

**Examples:**

```
"apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")

"apple", ["plain", "amber", "blade"] -> "1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").
```
###思路：

###代码：

```
class Solution {  
public:  
    string minAbbreviation(string target, vector<string>& dictionary) {  
        int len=target.size();  
        if(len==0) return "";  
        vector<string> dic;  
        for(string s:dictionary) {  
            if(s.size()==len) {  
                dic.push_back(s);  
            }  
        }  
        int len_d=dic.size();  
        if(len_d==0) return to_string(len);  
        string res=target;  
        dfs("",0,target,0,dic,res,len);  
        return res;  
    }  
  
    void dfs(string cur,int cur_len,string& target,int pos,vector<string>&dic,string&res,int& minlen) {  
        if(pos>=(int)target.size()) {  
            if(cur_len<minlen) {  //剪枝  
                bool f=true;  
                for(string s:dic) {  
                    if(validWordAbbreviation(s,cur)) {  
                        f=false;break;  
                    }  
                }  
                if(f){  
                    res=cur;  
                    minlen=cur_len;  
  
                }  
            }  
            return;  
        }  
        if(minlen==cur_len) return;   //剪枝  
        if(cur.empty()||!isdigit(cur.back())) {   //  
            for(int i=target.size()-1;i>=pos;i--) {  
                 string add=to_string(i-pos+1);  
                 dfs(cur+add,cur_len+1,target,i+1,dic,res,minlen);  
            }  
        }  
        dfs(cur+target[pos],cur_len+1,target,pos+1,dic,res,minlen);  
    }  
  
    bool validWordAbbreviation(string word, string abbr) {
        int lenw = word.size(), lena = abbr.size();
        int i = 0, j = 0;
        while(i < lenw && j < lena) {
            if(abbr[j] >= 'a' && abbr[j] <= 'z') {
                if(abbr[j] == word[i]) {
                    i++;
                    j++;
                }else {
                    return false;
                }
            }else {
                int now = 0;
                string nn = "";
                while(j < lena && (abbr[j] >= '0' && abbr[j] <= '9')) {
                    nn += abbr[j];
                    now = now * 10 + abbr[j] - '0';
                    j++;
                }
                
                if(now == 0 || nn[0] == '0') return false;
                i += now;
                if(i > lenw) return false;
            }
        }
        if(i < lenw || j < lena) return false;
        return true;
    }
};  
```

##418. Sentence Screen Fitting
###题目：
Given a **rows x cols** screen and a sentence represented by a list of words, find how many times the given sentence can be fitted on the screen.

Note:

* A word cannot be split into two lines.
* The order of words in the sentence must remain unchanged.
* Two consecutive words in a line must be separated by a single space.
* Total words in the sentence won't exceed 100.
* Length of each word won't exceed 10.
* 1 ≤ rows, cols ≤ 20,000.

**Example 1:**

```
Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output: 
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.
```
**Example 2:**

```
Input:
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output: 
2

Explanation:
a-bcd- 
e-a---
bcd-e-

The character '-' signifies an empty space on the screen.
```
**Example 3:**

```
Input:
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output: 
1

Explanation:
I-had
apple
pie-I
had--

The character '-' signifies an empty space on the screen.
```
###思路：
维护两个数组，num[i]和next[i]。

num[i]:代表以第i个单词作为一行的开头，这一行能放结束几次这个句子，即找这一行有几个结尾的单词；

next[i]:代表这个单词做开头放完一行之后，下一行的开头是哪个单词。

维护好以上两个数组之后，只需要每一行处理一次即可。

###代码：

```
int wordsTyping(vector<string>& sentence, int rows, int cols) {
    int len = sentence.size();
    int num[len] = {0}, next[len] = {0};
    for(int i = 0; i < len; ++i) {
        int now = 0, n = 0;
        bool yes = true;
        while(now + sentence[(i + n) % len].size() + (yes == false) <= cols) {
            now = now + sentence[(i + n) % len].size() + (yes == false);
            yes = false;
            n++;
            if((i + n) % len == 0) num[i]++;
        }
        next[i] = (i + n) % len;
    }
    
    int res = 0, nowR = 0;
    while(rows--) {
        res += num[nowR];
        nowR = next[nowR];
    }
    return res;
}
```

##422. Valid Word Square
###题目：
Given a sequence of words, check whether it forms a valid word square.

A sequence of words forms a valid word square if the
<math>
    <msubsup><mi>k</mi> <mi></mi> <mi>th</mi></msubsup>
</math>
row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

**Note:**

* The number of words given is at least 1 and does not exceed 500.
Word length will be at least 1 and does not exceed 500.
* Each word contains only lowercase English alphabet **a-z**.

**Example 1:**

```
Input:
[
  "abcd",
  "bnrt",
  "crmy",
  "dtye"
]

Output:
true

Explanation:
The first row and first column both read "abcd".
The second row and second column both read "bnrt".
The third row and third column both read "crmy".
The fourth row and fourth column both read "dtye".

Therefore, it is a valid word square.
```
**Example 2:**

```
Input:
[
  "abcd",
  "bnrt",
  "crm",
  "dt"
]

Output:
true

Explanation:
The first row and first column both read "abcd".
The second row and second column both read "bnrt".
The third row and third column both read "crm".
The fourth row and fourth column both read "dt".

Therefore, it is a valid word square.
```
**Example 3:**

```
Input:
[
  "ball",
  "area",
  "read",
  "lady"
]

Output:
false

Explanation:
The third row reads "read" while the third column reads "lead".

Therefore, it is NOT a valid word square.
```
###思路：
遍历每一个字符串的每个字符，判断关于对角线对称位置的字符是否存在且相等。
###代码：

```
bool validWordSquare(vector<string>& words) {
    int len = words.size();
    for(int i = 0; i < len; ++i) {
        int lens = words[i].size();
        for(int j = 0; j < lens; ++j) {
            if(j >= len || words[j].size() <= i) return false;
            if(words[i][j] != words[j][i]) return false;
        }
    }
    return true;
}
```
##425. Word Squares
###题目：
Given a set of words **(without duplicates)**, find all word squares you can build from them.

A sequence of words forms a valid word square if the 
<math>
    <msubsup><mi>k</mi> <mi></mi> <mi>th</mi></msubsup>
</math> 
row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence **["ball","area","lead","lady"]** forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```
**Note:**

* There are at least 1 and at most 1000 words.
* All words will have the exact same length.
* Word length is at least 1 and at most 5.
* Each word contains only lowercase English alphabet **a-z**.

**Example 1:**

```
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```
**Example 2:**

```
Input:
["abat","baba","atan","atal"]

Output:
[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```
###思路：
回溯的思路。

试着将每一个字符串放在方阵的第一个，试下一步的方法是提取下一个应该满足的前缀，然后从所有字符串里面找到符合前缀的字符串。

寻找符合前缀的字符串的方法可以是二分（首先将字符串数组排序），或者字典树。
###代码：

```
//二分的方法：
class Solution {
public:
    vector<vector<string>> res;
    int findFirst(const vector<string>& words, string str, int n) {
        int len = str.length();
        int l = 0, r = n-1, mid, R = n;
        while(l <= r) {
            mid = (l + r) / 2;
            if(str <= words[mid].substr(0, len)) {
                R = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
            
        }
        return R;
    }

    void dfs(const vector<string>& words, vector<string> path, int num, const int len, const int n) {
        if(num >= len) {
            res.push_back(path);
            return;
        }
        string str = "";
        for(int i = 0; i < num; ++i) {
            str += path[i][num];
        }
        
        int fir = findFirst(words, str, n);
        while(fir < n && words[fir].substr(0, num) == str) {
            path[num] = words[fir];
            dfs(words, path, num + 1, len, n);
            fir++;
        }
    }
    
    vector<vector<string>> wordSquares(vector<string>& words) {
        int n = words.size();
        if(n <= 0) return res;
        int len = words[0].size();
        
        sort(words.begin(), words.end());
        
        vector<string> path(len);
        for(int i = 0; i < n; ++i) {
            path[0] = words[i];
            dfs(words, path, 1, len, n);
        }
        return res;
    }
};
```


##439. Ternary Expression Parser
###题目：
Given a string representing arbitrarily nested ternary expressions, calculate the result of the expression. You can always assume that the given expression is valid and only consists of digits **0-9, ?, :, T** and **F** (**T** and **F** represent True and False respectively).

**Note:**

* The length of the given string is ≤ 10000.
* Each number will contain only one digit.
* The conditional expressions group right-to-left (as usual in most languages).
* The condition will always be either **T** or **F**. That is, the condition will never be a digit.
* The result of the expression will always evaluate to either a digit **0-9, T or F**.

**Example 1:**

```
Input: "T?2:3"

Output: "2"

Explanation: If true, then result is 2; otherwise result is 3.
```
**Example 2:**

```
Input: "F?1:T?4:5"

Output: "4"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(F ? 1 : (T ? 4 : 5))"                   "(F ? 1 : (T ? 4 : 5))"
          -> "(F ? 1 : 4)"                 or       -> "(T ? 4 : 5)"
          -> "4"                                    -> "4"
```
**Example 3:**

```
Input: "T?T?F:5:3"

Output: "F"

Explanation: The conditional expressions group right-to-left. Using parenthesis, it is read/evaluated as:

             "(T ? (T ? F : 5) : 3)"                   "(T ? (T ? F : 5) : 3)"
          -> "(T ? F : 3)"                 or       -> "(T ? F : 5)"
          -> "F"                                    -> "F"
```
###思路:
用栈，从后向前进行比配和运算。
###代码：

```
string parseTernary(string expression) {
    stack<char> op, ch;
    int len = expression.size(), i = len-1;
    while(i > 0) {
        if(expression[i] == ':') {
            op.push(expression[i]);
        }
        else if(expression[i] != '?') {
            ch.push(expression[i]);
            
        }else {
            char q = ch.top();
            ch.pop();
            char h = ch.top();
            ch.pop();
            if(expression[i-1] == 'T') {
                ch.push(q);
            }
            else {
                ch.push(h);
            }
            op.pop();
            i--;
        }
        i--;
    }
    string res;
    stringstream stream;
    stream << ch.top();
    res = stream.str();
    return res;
}

//答案，很简洁，但是比较难理解
string parseTernary(string& expression, int begin = 0) {
    if (begin >= expression.size()) return "";
    if (begin == expression.size() - 1 || expression[begin + 1] == ':') return expression.substr(begin, 1);
    if (expression[begin] == 'T') return parseTernary(expression, begin + 2);
    int level = 1, i = begin + 2;
    for (; i < expression.size() && level; i++) {
        if (expression[i] == '?') level++;
        else if (expression[i] == ':') level--;
    }
    return parseTernary(expression, i);
}
```


##444. Sequence Reconstruction
###题目：
Check whether the original sequence **org** can be uniquely reconstructed from the sequences in **seqs**. The **org** sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 104. Reconstruction means building a shortest common supersequence of the sequences in **seqs** (i.e., a shortest sequence so that all sequences in seqs are subsequences of it). Determine whether there is only one sequence that can be reconstructed from **seqs** and it is the **org** sequence.

**Example 1:**

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3]]

Output:
false

Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.
```
**Example 2:**

```
Input:
org: [1,2,3], seqs: [[1,2]]

Output:
false

Explanation:
The reconstructed sequence can only be [1,2].
```
**Example 3:**

```
Input:
org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]

Output:
true

Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
```
**Example 4:**

```
Input:
org: [4,1,5,2,6,3], seqs: [[5,2,6,3],[4,1,5,2]]

Output:
true
```
###思路：
方法1：

利用拓扑排序。由于最后需要的图只是一条链，所以后继可以不需要再建图，直接用出度的个数表示即可。

方法2：

只需要确认在org中每一对相邻的数字，都在seqs中的某一个序列中出现过即可。

###代码：

```
//方法1：拓扑排序
bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
    if (seqs.size() == 0) return false;
    int n = org.size(), count = 0;
    unordered_map<int, unordered_set<int>> graph;   // record parents
    vector<int> degree(n+1, 0); // record out degree
    for (auto s : seqs) {   // build graph
        for (int i = s.size()-1; i >= 0; --i) {
            if (s[i] > n or s[i] < 0) return false; // in case number in seqs is out of range 1-n
            if (i > 0 and !graph[s[i]].count(s[i-1])) {
                graph[s[i]].insert(s[i-1]);
                if (degree[s[i-1]]++ == 0) count ++;
            }
        }
    }
    if (count != n-1) return false; // all nodes should have degree larger than 0 except the last one
    for (int i = n-1; i >= 0; --i) {    // topological sort
        if (degree[org[i]] > 0) return false;   // the last node should have 0 degree
        for (auto p : graph[org[i]]) 
            if (--degree[p] == 0 and p != org[i-1]) // found a node that is not supposed to have 0 degree
                return false;
    }
    return true;
}

//方法2：
bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
    if(seqs.empty()) return false;
    vector<int> pos(org.size()+1);
    for(int i=0;i<org.size();++i) pos[org[i]] = i;
    
    vector<char> flags(org.size()+1,0);
    int toMatch = org.size()-1;
    for(const auto& v : seqs) {
        for(int i=0;i<v.size();++i) {
            if(v[i] <=0 || v[i] >org.size())return false;
            if(i==0)continue;
            int x = v[i-1], y = v[i];
            if(pos[x] >= pos[y]) return false;
            if(flags[x] == 0 && pos[x]+1 == pos[y]) flags[x] = 1, --toMatch;
        }
    }
    return toMatch == 0;
}
```
