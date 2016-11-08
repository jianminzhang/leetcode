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

##69. Sqrt(x)
###题目：
mplement int sqrt(int x).

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

##300. Longest Increasing Subsequence
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
    for(int i=0; i<nums.size(); i++) {
        auto it = std::lower_bound(res.begin(), res.end(), nums[i]);
        if(it==res.end()) res.push_back(nums[i]);
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