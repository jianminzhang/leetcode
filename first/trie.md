#Trie
##208. Implement Trie (Prefix Tree)
###题意：
Implement a trie with **insert**, **search**, and **startsWith** methods.

**Note:**

You may assume that all inputs are consist of lowercase letters a-z.

###思路：
字典树构造和基础。

这里字典树就是树的结构，每个节点上有26叉，每一叉代表一个字母

每个节点上的cnt代表以这个字母结尾的前缀。

判断某个字符串是否存在在这个字典树中就从root开始。

###代码：

```
class TrieNode {
public:
    int cnt;
    TrieNode* child[26];
    // Initialize your data structure here.
    TrieNode() {
        cnt = 0;
        for (int i = 0; i < 26; ++i) child[i] = NULL;
    }
};

class Trie {
public:
    Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    void insert(string word) {
        int len = word.size();
        TrieNode *cur = root;
        for (int i = 0; i < len; ++i) {
            int x = word[i] - 'a';
            if (cur->child[x] == NULL) cur->child[x] = new TrieNode();
            cur = cur->child[x];
        }
        cur->cnt = cur->cnt + 1;
    }

    // Returns if the word is in the trie.
    bool search(string word) {
        int len = word.size();
        TrieNode *cur = root;
        for (int i = 0; i < len; ++i) {
            int x = word[i] - 'a';
            if (cur->child[x] == NULL) return false;
            cur = cur->child[x];
        }
        if (cur->cnt > 0) return true;
        else return false;
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    bool startsWith(string prefix) {
        int len = prefix.size();
        TrieNode *cur = root;
        for (int i = 0; i < len; ++i) {
            int x = prefix[i] - 'a';
            if (cur->child[x] == NULL) return false;
            cur = cur->child[x];
        }
        return true;
    }

private:
    TrieNode* root;
};

// Your Trie object will be instantiated and called as such:
// Trie trie;
// trie.insert("somestring");
// trie.search("key");
```

##211. Add and Search Word - Data structure design
###题意：
Design a data structure that supports the following two operations:

```
void addWord(word)
bool search(word)
```
search(word) can search a literal word or a regular expression string containing only letters a-z or .. A . means it can represent any one letter.

For example:

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```
**Note:**

You may assume that all words are consist of lowercase letters a-z.

###思路：
首先建立字典树，然后查询的时候如果不存在"."那么直接从root向下查询字典树即可，不过需要注意单词的结尾在字典树上的节点必须要isOK = true，否则说明没有单词结尾。

但是如果有"."，那么需要遍历所有当前节点，这里运用了递归的做法。

结束条件是当前处理的是单词的最后一个字母，判断字典树是否有单词结束，有的话返回true,否则返回false。
###代码：

```
class TrieNode {
public:
    bool isOK;
    TrieNode* child[26];
    TrieNode() {
        isOK = false;
        memset(child, NULL, sizeof(child));
    }
};

class WordDictionary {
public:
    WordDictionary() {
        root = new TrieNode();
    }
    
    // Adds a word into the data structure.
    void addWord(string word) {
        int len = word.size();
        TrieNode *cur = root;
        for (int i = 0; i < len; ++i) {
            int x = word[i] - 'a';
            if (cur->child[x] == NULL) cur->child[x] = new TrieNode();
            cur = cur->child[x];
        }
        cur->isOK = true;
    }

    // Returns if the word is in the data structure. A word could
    // contain the dot character '.' to represent any one letter.
    bool search(string word) {
        int len = word.size();
        return searchNow(word, 0, len, root);
    }
private:
    TrieNode * root;
    bool searchNow(string word, int pos, int len, TrieNode* cur) {
        if (pos == len - 1) {
            if (word[pos] == '.') {
                for (int i = 0; i < 26; ++i) {
                    if (cur->child[i] != NULL && cur->child[i]->isOK) return true;
                }
                return false;
            }
            else {
                int x = word[pos]-'a';
                if (cur->child[x] != NULL && cur->child[x]->isOK) return true;
                else return false;
            }
        }
        
        if (word[pos] == '.') {
            for (int j = 0; j < 26; ++j) {
                if (cur->child[j] != NULL && searchNow(word, pos+1, len, cur->child[j])) return true;     
            }
            return false;
        }else {
            int x = word[pos] - 'a';
            if (cur->child[x] == NULL) return false;
            return searchNow(word, pos+1, len, cur->child[x]);
        }
       
    }
};

// Your WordDictionary object will be instantiated and called as such:
// WordDictionary wordDictionary;
// wordDictionary.addWord("word");
// wordDictionary.search("pattern");
```

##212. Word Search II
###题意：
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

For example,

Given words = ["oath","pea","eat","rain"] and board =

```
[
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]
```
Return ["eat","oath"].

**Note:**

You may assume that all inputs are consist of lowercase letters a-z.

###思路：


###代码：




##421. Maximum XOR of Two Numbers in an Array
###题意：
Given a non-empty array of numbers, a0, a1, a2, … , an-1, where 0 ≤ ai < 231.

Find the maximum result of ai XOR aj, where 0 ≤ i, j < n.

Could you do this in O(n) runtime?

**Example:**

```
Input: [3, 10, 5, 25, 2, 8]

Output: 28

Explanation: The maximum result is 5 ^ 25 = 28.
```
###思路：
时间复杂度 O(N)，建立树和计算数字的XOR结果都是32 * N的复杂度

这个问题的题意很重要，是要计算XOR结果最大值，不是考虑亦或的位数个数，而是要考虑亦或之后组成的结果，在一定程度上亦或不一致的位数越高，结果越大。

使用字典树存储每个数字的每一位，字典树的每个节点为2叉，总共有32层。

关键在于如何根据存有每个数的字典树来计算每个数和其他数之间最大的XOR。

对于每个数的每一位，从高位开始计算，如果与当前位不同值的节点在这个字典树中，那么res << 1 + 1， 下一次字典树指针指向不同值节点，否则res << 1并且下一次回归本身节点。

这样做的依据是，只要高位亦或值不同，那么就放弃相同的树的那一枝，只考虑不一样的那一枝即可，就算相同的那一枝后面每一位都不同，结果也比高位那一个不同得来的结果小。
###代码：

```
class TrieNode {
public:
    TrieNode *child[2];
    TrieNode() {
        child[0] = child[1] = NULL;
    }
};

class Solution {
public:
    void buildTrie(vector<int>& nums, int len) {
        root = new TrieNode();
        for (int i = 0; i < len; ++i) {
            TrieNode *cur = root;
            for (int j = 31; j >= 0; --j) {
                int now = (nums[i] >> j) & 1;
                if (cur->child[now] == NULL) cur->child[now] = new TrieNode();
                cur = cur->child[now];
            }
        }
    }
    
    int findMaximumXOR(vector<int>& nums) {
        int len = nums.size();
        buildTrie(nums, len);
        int res = 0;
        for (int i = 0; i < len; ++i) {
            TrieNode *cur = root;
            int nowRes = 0;
            for (int j = 31; j >= 0; --j) {
                int index = nums[i] >> j & 1 ? 0 : 1;
                if (cur->child[index] != NULL) {
                    nowRes <<= 1;
                    nowRes += 1;
                    cur = cur->child[index];
                }
                else {
                    nowRes <<= 1;
                    index = (index == 1 ? 0 : 1);
                    cur = cur->child[index];
                }
            }
            res = max(res, nowRes);
        }
        return res;
    }
private:
    TrieNode *root;
};
```

