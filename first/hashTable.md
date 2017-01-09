#Hash Table

##3. Longest Substring Without Repeating Characters
一个指针标记没有重复的最左，一个标记最右。

遇到重复的时候，最左变成max(l, yes[i])，yes[i]是当前这个重复的字母前一个出现的位置。不可以直接变成yes[i]，因为yes[i]有可能比l小。

更新yes[i]。

##30. Substring with Concatenation of All Words
###题目：
You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

For example, given:

s: **"barfoothefoobarman"**

words: **["foo", "bar"]**

You should return the indices: **[0,9]**.

(order does not matter).
###思路：
容易忽略的问题： words列表中可能会出现重复的单词，不可以将单词映射成编号，重复的单词无法处理，要映射成个数

方法1:

速度不快，但是想法比较直接。

使用两个hash，hash1记录words列表中每个单词出现的次数，hash2在每一次根据起始位置进行遍历的过程中，记录这个连续子串中出现的合法单词的个数。每次hash2与hash1进行比较。

方法2：

速度快，不容易理解

借鉴：
[思路1](https://discuss.leetcode.com/topic/7552/my-ac-c-code-o-n-complexity-26ms) 
[思路2](https://discuss.leetcode.com/topic/6617/an-o-n-solution-with-detailed-explanation)
###代码：

```
//方法1：
vector<int> findSubstring(string s, vector<string>& words) {
    int len = s.size(), n = words.size();
    vector<int> res;
    if (!len || !n) return res;
    int m = words[0].size();
    
    unordered_map<string, int> hash;
    for (int i = 0; i < n; ++i) hash[words[i]]++;
    
    for (int i = 0; i <= (len - m * n); ++i) {
        unordered_map<string, int> H;
        int num = 0;
        for (int j = i; j <= (i + m * n - m); j += m) {
            string nn = s.substr(j, m);
            if (hash.find(nn) == hash.end()) break;
            else if (H.find(nn) != H.end() && H[nn] >= hash[nn]) break;
            else {
                num++;
                H[nn]++;
            }
        }
        if (num == n) res.push_back(i);
    }
    return res;
}
```

##49. Group Anagrams
hash 表示 `<string, vector`< string>` >` 或者 `<string, int >`都可以。

##94. Binary Tree Inorder Traversal
需要一个TreeNode指针来遍历，不能直接把节点都push到stack中。

##136. Single Number
利用^= 亦或运算。
开始答案是0，亦或每个数，存在两个的就抵消变成0，最后剩下的就是只有一个的答案。

##166. Fraction to Recurring Decimal **
**需要注意的：** 要用long long否则会溢出。

做法：

1、首先判断为0，答案是0；

2、然后处理符号：if(a < 0 ^ b < 0) 符号为-，两个数都是原来数字的绝对值；

3、然后处理整数部分：a/b为结果，a-b*整数部分，如果不等于0，那么还有小数部分，加".";

4、小数部分要记录出现过的结果，如果出现重复，则先加上不重复的部分，然后加上循环节；如果不出现重复，则加上出现的部分即可。计算时要 c = a * 10 / b得到每个出现的部分，然后剩余到下一次运算的部分是 a = a * 10 - b * c得到。

##187. Repeated DNA Sequences
从左向右，用s.substr(i,10)，取不同的子串很方便。

不仅要找出现过的，而且次数要是1次，出现多次只算一次，不能只用set看是否出现过。

##204. Count Primes**
找到一个小的质数，就把这个质数的倍数都标记为和数，依次进行。

##205. Isomorphic Strings
要注意保证一一对应，左边的集合不可以重复，右边的也不可以。

##274. H-Index ***
```
	int hIndex(vector<int>& citations) {
        int len = citations.size();
        if(len == 0) return 0;
        
        unordered_map<int, int> hash(len+1);
        for(int i = 0; i < len; ++i) {
            if(citations[i] >= len) {
                hash[len]++;
            }else {
                hash[citations[i]]++;
            }
        }
        
        int p = 0, res = 0;
        for(int i = len; i >= 0; --i) {
            p += hash[i];
            if(p == i) {
                res = i;
                break;
            }
        }
        return res;
    }
```
##290. Word Pattern
istringstream is(str);输入流
ostringstream is(str);输出流
stringstream is(str);输出输入流


##299. Bulls and Cows
方法1、
要注意位置和内容相同的部分不需要建立hash,只需要记录secrect里面不是位置和内容都相同的部分。遍历guess部分，如果不是内容和位置都相同，其他的相应hash值>0说明位置不同，内容相同，并且对hash--。

方法2、
用

##347. Top K Frequent Elements
如果存在出现个数相同的，并没有区分一定要返回哪个。

利用multimap的有序性，先用一个map存下来每个数字出现的次数，然后键值颠倒插入到multimap，然后从后向前输出前k个。

multimap插入O(logN), 删除O(logN), 搜索O(logN)，迭代慢。

##350. Intersection of Two Arrays II
方法1.遍历nums1，将其存在hash里面；遍历nums2的同时修改hash内容；
方法2.可以先把两个数组排序，两个指针向后移动，相等就加入，指针都后移，否则将小的那一方后移。

##389. Find the Difference
可以使用亦或，将两个串都遍历一遍，亦或，剩下的就是结果。

##409. Longest Palindrome
只需要记录字母个数是奇数的即可，总的字母个数减去奇数个数，如果有奇数的，最后加1.














