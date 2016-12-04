#Array

##11.Container With Most Water
### 题意开始理解错误。
题意是有n个竖线，选择其中的两个和x轴组成一个容器，以两个竖线里面较短的当做高，两个竖线之间的距离当做宽。（并不需要两个竖线中间的其他竖线）。

two pointer

从两边夹，短的那一边移动。以为向里面移动，宽肯定减少，那么高必须变大，只能让原来的短板变大。

##15.3Sum
### 最重要的是保持没有重复
3个数字，确定两个可以保证第三个不一样。

最外层循环保证进入的时候不相同；two pointer里面两个的时候也确定一下第二位不同（拿一个变量纪录上一个的第二位是多少）。

##16.3Sum Closest
###1.只能根据内层循环加和的大小来判断l和r的变化

考虑到外层循环的数字有可能是负数。

###2.不仅结果要变化，记录差值的数值也要变化

##18.4Sum
### 记住更新！prej, prel

##33. Search in Rotated Sorted Array
### 按照mid的位置分类不容易错

##34. Search for a Range

lower_bound(nums.begin(), nums.end(), target) - nums.begin();

##39. Combination Sum
1.dfs 需要记录路径的时候，需要将路径用引用传递

2.由于一旦出现一个数字，dfs就会尝试这个数字出现各种次数的所有方法，所以在遇到后面相同的数字都会超出数字范围，故不需要排序，不需要避免后面的数字和前面的不一样

##40. Combination Sum II
由于一个数字只能出现一次，需要排序。

第一次出现一个数字，会把能用到这个数字后面的所有情况试过，所以后面的这个数字不用再试，直接跳过。

##41. First Missing Positive**
###1.维护nums[i]=i+1,连环变换，最后从前向后遍历，只要不符合上述情况，为结果；

###2.如果本身已经符合，在最后遍历的时候，结果要每次++，得到n+1是结果；

###3.如果已经换对，不重复进行，否则会有死循环。

##42. Trapping Rain Water
只需要维护一个左边最大和右边最大，two pointer维护，每个位置只要比左右值里面小的就累加差值，大就更新。

##53. Maximum Subarray
如果全部数字都是负数，就要能求出所有数字中最大的那一个，需要在遍历过程中求 Max = max(Max, nums[i]);

##54. Spiral Matrix 59. Spiral Matrix II
相同，固定四个角，循环遍历。

注意在循环过程中，每次的最后一个不要处理，留给下一个循环。

最后处理剩下的部分，需要处理最后一个。

##56. Merge Intervals
自己构造结构体比较函数：

**static** bool lesser(**const** Interval a, **const** Interval b) {
        return a.start < b.start;
    }

##57. Insert Interval
一定保证最后是有序的，开始已经按照start排序。

##84. Largest Rectangle in Histogram
###题目：
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

For example,

Given heights = **[2,1,5,6,2,3]**,

return **10**.
###思路：
用栈存储一个范围内的最小值。

当前值大于等于栈顶的时候，加入栈。

否则更新答案。
###代码：

```
int largestRectangleArea(vector<int>& heights) {
    int len = heights.size();
    int res = 0;
    if (len == 0) return res;
    stack<int> mini;
    heights.push_back(0);
    int i = 0;
    while (i <= len) {
        if (mini.empty() || heights[i] >= heights[mini.top()]) mini.push(i++);
        else {
            int now = mini.top();
            mini.pop();
            res = max(res, heights[now] * (mini.empty() ? i : i - mini.top() - 1));
        }
    }
    return res;
}
```

##121. Best Time to Buy and Sell Stock
维护一个前i个位置的时候的最小值，用当前位置的值减去最小值，与最大结果对比，如果大即更新。

