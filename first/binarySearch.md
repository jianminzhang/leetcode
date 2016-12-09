#Binary Search

##29. Divide Two Integers*
###题目：
Divide two integers without using multiplication, division and mod operator.

If it is overflow, return MAX_INT.

###思路：
首先判断边界条件：除数为0和被除数为INT_MIN 且除数为-1；

接着判断符号，并且将除数被除数都变成正数，这里注意要用long long，否则负数不超范围的正数有可能超范围；

下面用位运算的左移操作，实现辗转相除，得到答案。
###代码：

```
int divide(int dividend, int divisor) {
    if(!divisor || (dividend == INT_MIN && divisor == -1)) return INT_MAX;
    int sign = ((dividend > 0) ^ (divisor > 0) == 1) ? -1 : 1;
    long long dvd = labs(dividend);
    long long dvs = labs(divisor);
    int res = 0;
    while(dvd >= dvs) {
        long long tmp = dvs, left = 1;
        while(dvd >= (tmp << 1)) {
            tmp <<= 1;
            left <<= 1;
        }
        dvd -= tmp;
        res += left;
    }
    return sign == 1 ? res : -res;
}
```
##33. Search in Rotated Sorted Array
###题目：
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.
###思路：
方法1：

自己可以理解，只是速度比较慢，情况分的比较多。

方法2：

速度快，自己想不到。
###代码：

```
//方法1：
int search(vector<int>& nums, int target) {
    int len = nums.size();
    int l = 0, r = len-1;
    
    while(l <= r) {
        int mid = (r - l) / 2 + l;
        if(target == nums[mid]) return mid;
        if(target == nums[l]) return l;
        if(target == nums[r]) return r;
            
        if(nums[0] < nums[len-1]) {
            if(target > nums[mid]) l = mid + 1;
            else r = mid - 1;
        }
        else {
            if(nums[mid] > nums[l]) {
                if(target > nums[mid] || target < nums[l]) l = mid + 1;
                else r = mid - 1;
            }
            else {
                if(target > nums[mid] && target < nums[r]) l = mid + 1;
                else r = mid - 1;
            }
        }    
    }
    return -1;
}

//方法2：
int searchIn(vector<int>& nums, int l, int r, int target) {
    if(l > r) return -1;
    int mid = l + (r - l)/2;
    if(nums[mid] == target) return mid;
    else if(nums[l] <= target){
        if(target < nums[mid] || nums[mid] < nums[l])
            return searchIn(nums, l, mid-1, target);
        else
            return searchIn(nums, mid+1, r, target);
    }
    else {
        if(target < nums[mid] && nums[mid] < nums[r])
            return searchIn(nums, l, mid-1, target);
        else
            return searchIn(nums, mid+1, r, target);
    }
}

int search(vector<int>& nums, int target) {
    int len = nums.size();
    return searchIn(nums,0,len-1,target);
}
```

##34. Search for a Range
###题目：
Given a sorted array of integers, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

For example,

Given [5, 7, 7, 8, 8, 10] and target value 8,

return [3, 4].
###思路：

###代码：

```
vector<int> searchRange(vector<int>& nums, int target) {
    int low = lower_bound(nums.begin(), nums.end(), target) - nums.begin();
    int up = upper_bound(nums.begin(), nums.end(), target) - nums.begin() - 1;
    if(low <= up) return {low, up};
    else return {-1, -1};
}
```

##35. Search Insert Position
###题目:
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

Here are few examples.

```[1,3,5,6]```, 5 → 2

```[1,3,5,6]```, 2 → 1

```[1,3,5,6]```, 7 → 4

```[1,3,5,6]```, 0 → 0

###思路:
###代码：

```
int searchInsert(vector<int>& nums, int target) {
    int len = nums.size();
    int res = len;
    int l = 0, r = len-1;
    while(l <= r) {
        int mid = (r - l) / 2 + l;
        if(nums[mid] >= target) {
            res = mid;
            r = mid - 1;
        }else {
            l = mid + 1;
        }
    }
    return res;
}
```

##50. Pow(x, n)
###题目：
Implement pow(x, n).
###思路：
首先处理次数，一定要用long long 来保存，否则有可能超过范围，但是一定要强制类型转换。

处理次数的同时，如果次数为负数，则底数变为倒数。

然后以2的幂次个底数一步一步得到结果。

###代码：

```
double myPow(double x, int n) {
    double res = 1;
    long long p;
    if(n == 0) return 1;
    if(n < 0) {
        p = -(long long)n;//强制类型转换，否则不可以
        x = 1 / x;
    }else {
        p = n;
    }
    while(p) {
        if(p & 1) res *= x;
        x *= x;
        p >>= 1;
    }
    return res;
}
```

##69. Sqrt(x)
###题目：
Implement int sqrt(int x).

Compute and return the square root of x.
###思路：
主要是 mid * mid 有可能很大，甚至都超过 long long 的范围。

要使用 x / mid 作为判断依据，可是要特殊判断 mid != 0。
###代码：

```
int mySqrt(int x) {
    int l = 0, r = x;
    int res = x;
    while (l <= r) {
        int mid = (r - l) / 2 + l;
        if(mid == 0) {
            return res;
        }
        int nn = x / mid;
        if(nn >= mid) {
            res = mid;
            l = mid + 1;
        }else {
            r = mid - 1;
        }
    }
    return res;
}
```

##74. Search a 2D Matrix
###题目：
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row.

For example,

Consider the following matrix:

```
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
```
Given target = 3, return true.
###思路：
先在第一列中二分，找到最后一个行首比目标值小的那一行；

然后再在那一行中二分，确定是否存在。

可以将二维转换成一维
###代码：

```
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    if(m <= 0) return false;
    int n = matrix[0].size();
    int l = 0, r = m-1;
    int row = 0;
    while(l <= r) {
        int mid = (r - l) / 2 + l;
        if(matrix[mid][0] == target) return true;
        else if(matrix[mid][0] < target) {
            row = mid;
            l = mid + 1;
        }
        else {
            r = mid - 1;
        }
    }
    l = 0, r = n-1;
    while(l <= r) {
        int mid = (r - l) / 2 + l;
        if(matrix[row][mid] == target) return true;
        else if(matrix[row][mid] > target) {
            r = mid - 1;
        }
        else {
            l = mid + 1;
        }
    }
    return false;
}
```

##153. Find Minimum in Rotated Sorted Array*
###题目：
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).

Find the minimum element.

You may assume no duplicate exists in the array.
###思路：

[题解](https://discuss.leetcode.com/topic/14768/4ms-simple-c-code-with-explanation)
In this problem, we have only three cases.

Case 1. The leftmost value is less than the rightmost value in the list: This means that the list is not rotated.
e.g> [1 2 3 4 5 6 7 ]

Case 2. The value in the middle of the list is greater than the leftmost and rightmost values in the list.
e.g> [ 4 5 6 7 0 1 2 3 ]

Case 3. The value in the middle of the list is less than the leftmost and rightmost values in the list.
e.g> [ 5 6 7 0 1 2 3 4 ]

As you see in the examples above, if we have case 1, we just return the leftmost value in the list. If we have case 2, we just move to the right side of the list. If we have case 3 we need to move to the left side of the list.

Following is the code that implements the concept described above.
###代码：

```
int findMin(vector<int>& nums) {
    int left = 0,  right = nums.size() - 1;
    while(left < right) {
        if(nums[left] < nums[right]) 
            return nums[left];
            
        int mid = (left + right)/2;
        if(nums[mid] > nums[right])
            left = mid + 1;
        else
            right = mid;
    }
    
    return nums[left];
}
```
##154. Find Minimum in Rotated Sorted Array II
###题目：
```
Follow up for "Find Minimum in Rotated Sorted Array":
What if duplicates are allowed?

Would this affect the run-time complexity? How and why?
```
Suppose a sorted array is rotated at some pivot unknown to you beforehand.

(i.e., **0 1 2 4 5 6 7** might become **4 5 6 7 0 1 2**).

Find the minimum element.

The array may contain duplicates.
###思路:

###代码：


##162. Find Peak Element*
###题目：
A peak element is an element that is greater than its neighbors.

Given an input array where **num[i] ≠ num[i+1]**, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that **num[-1] = num[n] = -∞**.

For example, in array **[1, 2, 3, 1]**, 3 is a peak element and your function should return the index number 2.

click to show spoilers.

**Note:**

Your solution should be in logarithmic complexity.
###思路：
找到一个比右边的值大的，如果比右边的小，就向右移动，否则记录。

###代码：

```
int findPeakElement(const vector<int> &num) {
    int left = 0;
    int right = num.size()-1;
    while(left<right){
        int mid = (right+left)/2;
        if(num[mid]<num[mid+1]){
            left = mid+1;
        }
        else right = mid;
    }
    return right;
}
```


##209. Minimum Size Subarray Sum
###题目:
Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.

For example, given the array [2,3,1,2,4,3] and s = 7,

the subarray [4,3] has the minimal length under the problem constraint.
###思路：
方法1：two pointer O(n)

注意res开始要设为len+1，因为len有可能成立。

方法2：O(nlogn)

二分

###代码：

```
//方法1：
int minSubArrayLen(int s, vector<int>& nums) {
    int len = nums.size();
    int l = 0, r = 0;
    int now = 0, res = len+1;
    while(l < len && r <= len) {
        if(now < s) {
            if(r == len && res == len+1) return 0;
            now += nums[r];
            r++;
        }else {
            res = min(res, r-l);
            now -= nums[l];
            l++;
        }
    }
    return res == len+1 ? 0 : res;
}

//方法2：
public:
    int minSubArrayLen(int s, vector<int>& nums) {
        vector<int> sums = accumulate(nums);
        int n = nums.size(), minlen = INT_MAX;
        for (int i = 1; i <= n; i++) { 
            if (sums[i] >= s) {
                int p = upper_bound(sums, 0, i, sums[i] - s);
                if (p != -1) minlen = min(minlen, i - p + 1);
            }
        }
        return minlen == INT_MAX ? 0 : minlen;
    }
private:
    vector<int> accumulate(vector<int>& nums) {
        int n = nums.size();
        vector<int> sums(n + 1, 0);
        for (int i = 1; i <= n; i++) 
            sums[i] = nums[i - 1] + sums[i - 1];
        return sums;
    }
    int upper_bound(vector<int>& sums, int left, int right, int target) {
        int l = left, r = right;
        while (l < r) {
            int m = l + ((r - l) >> 1);
            if (sums[m] <= target) l = m + 1;
            else r = m;
        }
        return sums[r] > target ? r : -1;
    }
```

##240. Search a 2D Matrix II
###题目：
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:

* Integers in each row are sorted in ascending from left to right.
* Integers in each column are sorted in ascending from top to bottom.

For example,

Consider the following matrix:

```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
Given target = 5, return true.

Given target = 20, return false.
###思路：
时间复杂度是O(m+n)

从右上角开始比较，如果目标大于右上角，则列数减一，否则行数加一。每次都可以减去一行或者一列的比较。
###代码：

```
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    if(m == 0) return false;
    int n = matrix[0].size();
    int row = 0, col = n-1;
    while(row < m && col >= 0) {
        if(matrix[row][col] == target) return true;
        else if(matrix[row][col] < target) {
            ++row;
        }
        else {
            --col;
        }
    }
    return false;
}
```

##275. H-Index II**
###题目：
Follow up for H-Index: What if the citations array is sorted in ascending order? Could you optimize your algorithm?

**Hint:**

* Expected runtime complexity is in O(log n) and the input is sorted.

###思路：
还是有点绕不清楚……
###代码：

```
int hIndex(vector<int>& citations) {
    int size = citations.size();
    int first = 0;
    int mid;
    int count = size;
    int step;
    while (count > 0) {
        step = count / 2;
        mid = first + step;
        if (citations[mid] < size - mid) {
            first = mid + 1;
            count -= (step + 1);
        }
        else {
            count = step;
        }
    }
    return size - first;
}
```

##278. First Bad Version
###题目：
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have **n** versions **[1, 2, ..., n]** and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API **bool isBadVersion(version)** which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.
###思路：
注意 l <= r 不要写成 l <= n。。。。
###代码：

```
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int l = 1, r = n;
        int res = n;
        while(l <= r) {
            int mid = (r - l) / 2 + l;
            if(isBadVersion(mid)) {
                res = mid;
                r = mid - 1;
            }else {
                l = mid + 1;
            }
        }
        return res;
    }
};
```

##300. Longest Increasing Subsequence *
###题目：
Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,

Given [10, 9, 2, 5, 3, 7, 101, 18],

The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. 

Note that there may be more than one LIS combination, it is only necessary for you to return the length.

Your algorithm should run in O(n2) complexity.

**Follow up:** Could you improve it to O(n log n) time complexity?
###思路：
方法1：时间复杂度：O(n^2)，利用DP记录

维护一个数组 R[len]，从右向左更新，记录以nums[i]为开头的LIS最大长度。然后取这个数组中最大的那一个。

方法2：时间复杂度 O(nlogn)，利用二分

[GeekForGeek](http://www.geeksforgeeks.org/longest-monotonically-increasing-subsequence-size-n-log-n/)具体讲解。

主要思路就是维护一个数组res来记录最长的连续串。从左向右遍历数组，如果nums[i]比res中所有数组都大，那么将其添加在最后，否则将其替换res中第一个大于等于他的位置上的数字，酱紫不会使原来的答案数组不成立，但是会给后面的数字有更多可能。

res[i]记录的是 长度是i+1的LIS最后一位的最小值。
###代码：

```
//方法1：
int lengthOfLIS(vector<int>& nums) {
    int res = 0, len = nums.size();
    if(len == 0) return 0;
    vector<int> R(len, 1);
    for(int i = len-2; i >= 0; --i) {
        for(int j = i+1; j < len; ++j) {
            if(nums[j] > nums[i]) {
                R[i] = max(R[i], R[j]+1);
            }
        }
    }
    for(int i = 0; i < len; ++i) {
        res = max(res, R[i]);
    }
    return res;
}

//方法2：
int lengthOfLIS(vector<int>& nums) {
    vector<int> res;
    for(int i = 0; i < nums.size(); i++) {
        auto it = lower_bound(res.begin(), res.end(), nums[i]);
        if(it == res.end()) res.push_back(nums[i]);
        else *it = nums[i];
    }
    return res.size();
}
```

##367. Valid Perfect Square
###题目：
Given a positive integer num, write a function which returns True if num is a perfect square else False.

**Note: Do not** use any built-in library function such as **sqrt**.

**Example 1:**

```
Input: 16
Returns: True
```
**Example 2:**

```
Input: 14
Returns: False
```
###思路：
注意判断某个数是不是其开方，除以这个数的时候结果要用double进行比较，否则5 / 2 == 2，但是2并不是我们要找的。
###代码：

```
bool isPerfectSquare(int num) {
    if(num < 0) return false;
    else if(num == 0 || num == 1) return true;
    else {
        int l = 0, r = num;
        while(l <= r) {
            int mid = (r - l) / 2 + l;
            if(mid == 0) {
                return false;
            }
            else {
                double left = (double)num / (double)mid;
                if(left == (double)mid) {
                    return true;
                }
                else if(left > (double)mid) {
                    l = mid + 1;
                }else {
                    r = mid - 1;
                }
            }
        }
        return false;
    }
}
```
##374. Guess Number Higher or Lower
###题目：
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API **guess(int num)** which returns 3 possible results **(-1, 1, or 0)**:

```
-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
```
**Example:**

```
n = 10, I pick 6.

Return 6.
```

###思路：
主要是题意的理解，-1是代表mid代表的数字大于结果。

还有要注意求mid得时候，用（r-l）/2 + l 比较保险，不然可能会爆int。

###代码：

```
// Forward declaration of guess API.
// @param num, your guess
// @return -1 if my number is lower, 1 if my number is higher, otherwise return 0
int guess(int num);

class Solution {
public:
    int guessNumber(int n) {
        int l = 1, r = n;
        int res = n;
        while(true) {
            int mid = (r - l) / 2 + l;
            int minus = guess(mid);
            if(minus == 0) {
                return mid;
            }else if(minus == 1) {
                l = mid + 1;
            }
            else {
                r = mid - 1;
            }
        }
    }
};
```
##378. Kth Smallest Element in a Sorted Matrix**
###题目：
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:

```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```
**Note:** 

You may assume k is always valid, 1 ≤ k ≤ n2.
###思路：
首先这个模板求得是满足条件的最左边界的位置；

然后条件是找出matrix中小于等于value的个数等于k的value，符合这个条件的数是matrix中【第k数-第k+1个数）左闭右开；

我们找到的是符合这个条件的最小的数，所以这个数一定是第k个数，是存在于矩阵中的。
###代码：

```
int kthSmallest(vector<vector<int>>& matrix, int k) {
    int n = matrix.size();
    int l = matrix[0][0], r = matrix[n-1][n-1];
    while(l < r) {
        int mid = (r - l) / 2 + l;
        int cnt = 0;
        for(int i = 0; i < n; ++i) {
            cnt += (upper_bound(matrix[i].begin(), matrix[i].end(), mid) - matrix[i].begin());
        }
        if(cnt < k) {
            l = mid + 1;
        }else {
            r = mid;
        }
    }
    return l;
}
```
##392. Is Subsequence
###题目：
Given a string s and a string t, check if s is subsequence of t.

You may assume that there is only lower case English letters in both s and t. t is potentially a very long (length ~= 500,000) string, and s is a short string (<=100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, **"ace"** is a subsequence of **"abcde"** while **"aec"** is not).

**Example 1:**

s = **"abc"**, t = **"ahbgdc"**

Return true.

**Example 2:**

s = **"axc"**, t = **"ahbgdc"**

Return false.

**Follow up:**

If there are lots of incoming S, say S1, S2, ... , Sk where k >= 1B, and you want to check one by one to see if T has its subsequence. In this scenario, how would you change your code?


###思路：
时间复杂度 O(n) 

两个指针分别遍历s和t。

follow up:

首先用一个hash来记录t字符串中所有字符出现的位置；

然后遍历每一个s串，pre代表s[i]可以从t[pre]及之后的字母中查找。

如果hash中不存在s[i]，返回false;

如果hash[s[i]]大于等于pre的位置不存在，那么返回false,说明在t中合适的位置已经没有s的相应字母
###代码：

```
bool isSubsequence(string s, string t) {
    int lens = s.size(), lent = t.size();
    int i = 0, j = 0;
    while(i < lens && j < lent) {
        if(s[i] == t[j]) {
            i++;
            j++;
        }else {
            j++;
        }
    }
    if(i == lens) return true;
    else return false;
}

//Follow up:
class Solution {
public:
    Solution(string t):target(t) {
        for(int i = 0; i < t.length(); ++i) {
            posMap[t[i] - 'a'].push_back(i);
        }
    }
    
    vector<bool> isSubsequence(vector<string> strs) {
        int pre = -1;
        int index[26];
        vector<bool> ans;
        for(string str: strs) {
            memset(index, -1, sizeof(index));
            pre = -1;
            int i = 0;
            for(;i < str.size(); ++i) {
                int j = str[i] - 'a';
                if(posMap.find(j) == posMap.end() || posMap[j].size()<= index[j] + 1 || posMap[j][index[j] + 1] <= pre) {
                    ans.push_back(false);
                    break;
                }
                pre = posMap[j][index[j] + 1];
                index[j]++;
            }
            if(i == str.size()) ans.push_back(true);
        }
        return ans;
    }
    
private:
    string target;
    unordered_map<int, vector<int> > posMap;
};
```
##410. Split Array Largest Sum
###题目：
Given an array which consists of non-negative integers and an integer m, you can split the array into m non-empty continuous subarrays. Write an algorithm to minimize the largest sum among these m subarrays.

**Note:**

Given m satisfies the following constraint: 1 ≤ m ≤ length(nums) ≤ 14,000.

**Examples:**

```
Input:
nums = [7,2,5,10,8]
m = 2

Output:
18

Explanation:
There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8],
where the largest sum among the two subarrays is only 18.
```
###思路：
###代码：

##436. Find Right Interval
###题目：
Given a set of intervals, for each of the interval i, check if there exists an interval j whose start point is bigger than or equal to the end point of the interval i, which can be called that j is on the "right" of i.

For any interval i, you need to store the minimum interval j's index, which means that the interval j has the minimum start point to build the "right" relationship for interval i. If the interval j doesn't exist, store -1 for the interval i. Finally, you need output the stored value of each interval as an array.

**Note:**

* You may assume the interval's end point is always bigger than its start point.
* You may assume none of these intervals have the same start point.

**Example 1:**

```
Input: [ [1,2] ]

Output: [-1]

Explanation: There is only one interval in the collection, so it outputs -1.
```
**Example 2:**

```
Input: [ [3,4], [2,3], [1,2] ]

Output: [-1, 0, 1]

Explanation: There is no satisfied "right" interval for [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point;
For [1,2], the interval [2,3] has minimum-"right" start point.
```
**Example 3:**

```
Input: [ [1,4], [2,3], [3,4] ]

Output: [-1, 2, -1]

Explanation: There is no satisfied "right" interval for [1,4] and [3,4].
For [2,3], the interval [3,4] has minimum-"right" start point.
```
###思路：
注意理解题意，"right"只是要求其start要大于比较对象的end并没有要求其位置也在比较对象的右面。

利用map的  **hash.lower_bound(in.end)**
###代码：

```
vector<int> findRightInterval(vector<Interval>& intervals) {
    map<int, int> hash;
    vector<int> res;
    int n = intervals.size();
    for (int i = 0; i < n; ++i)
        hash[intervals[i].start] = i;
    for (auto in : intervals) {
        auto itr = hash.lower_bound(in.end);
        if (itr == hash.end()) res.push_back(-1);
        else res.push_back(itr->second);
    }
    return res;
}
```

##441. Arranging Coins
###题目：
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of full staircase rows that can be formed.

n is a non-negative integer and fits within the range of a 32-bit signed integer.

**Example 1:**

```
n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
```
**Example 2:**

```
n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
```

###思路：
注意用int会超过范围，要用long long。
###代码：

```
int arrangeCoins(int n) {
    long long l = 0, r = n;
    long long res = 1;
    while(l <= r) {
        long long mid = (l + r) / 2;
        long long mm = mid * (1 + mid) / 2;
        if(mm <= (long long)n) {
            res = mid;
            l = mid + 1;
        }else {
            r = mid - 1;
        }
    }
    return (int)res;
}
```