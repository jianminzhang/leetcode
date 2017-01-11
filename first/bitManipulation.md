#Bit Manipulation

##371. Sum of Two Integers
###题意：
Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.

Example:

Given a = 1 and b = 2, return 3.
###思路：
位运算：[详细讲解](https://discuss.leetcode.com/topic/50315/a-summary-how-to-use-bit-manipulation-to-solve-problems-easily-and-efficiently)

###代码：

```
int getSum(int a, int b) {
    int sum = a;
    while (b) {
        sum = a ^ b;//计算不进位的部分
        b = (a & b) << 1;//计算进位的部分
        a = sum;
    }
    return sum;
}
```

##393. UTF-8 Validation
###题意：
A character in UTF8 can be from 1 to 4 bytes long, subjected to the following rules:

1. For 1-byte character, the first bit is a 0, followed by its unicode code.
2. For n-bytes character, the first n-bits are all one's, the n+1 bit is 0, followed by n-1 bytes with most significant 2 bits being 10.
This is how the UTF-8 encoding would work:

```

   Char. number range  |        UTF-8 octet sequence
      (hexadecimal)    |              (binary)
   --------------------+---------------------------------------------
   0000 0000-0000 007F | 0xxxxxxx
   0000 0080-0000 07FF | 110xxxxx 10xxxxxx
   0000 0800-0000 FFFF | 1110xxxx 10xxxxxx 10xxxxxx
   0001 0000-0010 FFFF | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx
```
Given an array of integers representing the data, return whether it is a valid utf-8 encoding.

**Note:**

The input is an array of integers. Only the least significant 8 bits of each integer is used to store the data. This means each integer represents only 1 byte of data.

**Example 1:**

```
data = [197, 130, 1], which represents the octet sequence: 11000101 10000010 00000001.

Return true.
It is a valid utf-8 encoding for a 2-bytes character followed by a 1-byte character.
```
**Example 2:**

```
data = [235, 140, 4], which represented the octet sequence: 11101011 10001100 00000100.

Return false.
The first 3 bits are all one's and the 4th bit is 0 means it is a 3-bytes character.
The next byte is a continuation byte which starts with 10 and that's correct.
But the second continuation byte does not start with 10, so it is invalid.
```
###思路：
首先只保留当前处理数字的前5位，判断这个数的范围即可知道这个数代表的是几byte数

然后判断后面的几个数是否符合10开头，不符合或者后面的数字个数小于byte数，那么返回false.
###代码：

```
bool validUtf8(vector<int>& data) {
    int len = data.size();
    for (int i = 0; i < len; ++i) {
        int now = data[i] & 248;
        int nowLen = 0;
        if (now < 128) continue;
        else if (now >= 128 && now < 192) return false;
        else if (now >= 192 && now < 224) nowLen = 1;
        else if (now >= 224 && now < 240) nowLen = 2;
        else if (now == 240) nowLen = 3;
        else return false;
        for (int j = 1; j <= nowLen; ++j) {
            if (i + j >= len) return false;
            if ((data[i+j] & 192) != 128) return false;
        }
        i += nowLen;
    }
    return true;
}
```

##397. Integer Replacement
###题目：
Given a positive integer n and you can do operations as follow:

1. If n is even, replace n with n/2.
2. If n is odd, you can replace n with either n + 1 or n - 1.

What is the minimum number of replacements needed for n to become 1?

**Example 1:**

```
Input:
8

Output:
3

Explanation:
8 -> 4 -> 2 -> 1
```
**Example 2:**

```
Input:
7

Output:
4

Explanation:
7 -> 8 -> 4 -> 2 -> 1
or
7 -> 6 -> 3 -> 2 -> 1
```
###思路：

通过观察发现当a是奇数，如果 (a + 1) % 4 == 0则计算integerReplacement(n+1)，否则计算integerReplacement(n-1)

如果是偶数，integerReplacement(n / 2)

每次调用一次 integerReplacement(n)函数，结果加1

边界条件：

n == 1, res = 0;

n == 3，res = 2;

n == INT_MAX, res = 32.
###代码：

```
int res = 0;
int integerReplacement(int n) {
    if (n == 1) return res;
    if (n == 3) {
        res += 2;
        return res;
    }
    if (n == INT_MAX) return 32;
    res++;
    if (n & 1) {
        if ((n + 1) % 4 == 0) integerReplacement(n + 1);
        else integerReplacement(n - 1);
    }
    else integerReplacement(n / 2);
    return res;
}
```


##405. Convert a Number to Hexadecimal
###题目：
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

**Note:**

1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a 32-bit signed integer.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

**Example 1:**

```
Input:
26

Output:
"1a"
```
**Example 2:**

```
Input:
-1

Output:
"ffffffff"
```
###思路：
需要将整数转换成16进制的表示，正负数都可以表示，不需要特殊处理。

16进制相当于每4位转换成1为，那么可以用数字与15做&运算，留下末四位的结果加入到Res中，然后将数字右移4位，处理下一个四位。
###代码：

```
string toHex(int num) {
    if (!num) return "0";
    int cnt = 0;
    string res = "";
    while (num && cnt < 8) {
        int tmp = num & 15;
        if (tmp < 10) res += ('0' + tmp);
        else res += ('a' + (tmp-10));
        num >>= 4;
        cnt++;
    }
    reverse(res.begin(), res.end());
    return res;
}
```
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

##461. Hamming Distance
###题意：
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

**Note:**

0 ≤ x, y < 231.

**Example:**

```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```
###思路：
两个数亦或可以得到不一样的位数为1，计算亦或后结果的1的位数即可

方法1：时间复杂度：O(m)，m代表位数

将亦或后的结果循环得去掉低位，检查低位如果是1则将结果加1.

方法2：GCC的内建函数 __builtin_popcount(x)可得到x的1的位数
###代码：

```
//方法1：
int hammingDistance(int x, int y) {
    int now = x ^ y;
    int num = 0;
    while (now) {
        if (now & 1) num++;
        now >>= 1;
    }
    return num;
}
//方法2：
int hammingDistance(int x, int y) {
    return __builtin_popcount(x ^ y);
}
```

##477. Total Hamming Distance
###题意:
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Now your job is to find the total Hamming distance between all pairs of the given numbers.

**Example:**

```
Input: 4, 14, 2

Output: 6

Explanation: In binary representation, the 4 is 0100, 14 is 1110, and 2 is 0010 (just
showing the four bits relevant in this case). So the answer will be:
HammingDistance(4, 14) + HammingDistance(4, 2) + HammingDistance(14, 2) = 2 + 2 + 2 = 6.
```
**Note:**

* Elements of the given array are in the range of 0 to 10^9
* Length of the array will not exceed 10^4.

###思路：
方法1：暴力方法：每两个不一样的数字对进行分别计算海明距离，距离方法见leetcode461。

会超时，因为需要进行O(n^2)次距离计算

方法2：按照位来进行 O(n)

如果只考虑一位的话，m个数中有p个数那一位是1，q个数那一位是0，那么在这一位总的距离之和是p*q，只有分属于不同集合中的数之间在这一位才存在1的海明距离。

那么可以从低位开始处理，每次遍历所有数字，判断其最后一位是1还是0，res += p*q;

然后将数字右移一位，直到所有数字都变成0

见[详解](https://discuss.leetcode.com/topic/72099/share-my-o-n-c-bitwise-solution-with-thinking-process-and-explanation)
###代码：

```
int totalHammingDistance(vector<int>& nums) {
    int res = 0, len = nums.size();
    while (true) {
        int p = 0, q = 0, nn = 0;
        for (int i = 0; i < len; ++i) {
            if (!nums[i]) ++nn;
            if (nums[i] & 1) ++p;
            else ++q;
            nums[i] >>= 1;
        }
        res += (p * q);
        if (nn == len) return res;
    }
}
```

