#Divide and Conquer
##53. Maximum Subarray
###题目：
Find the contiguous subarray within an array (containing at least one number) which has the largest sum.

For example, given the array **[-2,1,-3,4,-1,2,1,-5,4]**,

the contiguous subarray **[4,-1,2,1]** has the largest sum = **6**.
###思路：
dp思路。

**状态：**

dp[i] 代表含有位置i的最大子数组的加和。

**转移：**

dp[i] = (dp[i-1] > 0 ? dp[i-1] : 0) + nums[i]

**初始化：**

dp[0] = nums[0]，res = dp[0]

**结果：**

max{dp[i]}。 
###代码：

```
int maxSubArray(vector<int>& nums) {
    int len = nums.size();
    int dp[len], Max;
    dp[0] = nums[0];
    Max = dp[0];
    for(int i = 1; i < len; ++i) {
        dp[i] = (dp[i-1] > 0 ? dp[i-1] : 0) + nums[i];
        Max = max(Max, dp[i]);
    }
    return Max;
}
```

##169. Majority Element
###题目：
Given an array of size n, find the majority element. The majority element is the element that appears more than **⌊ n/2 ⌋** times.

You may assume that the array is non-empty and the majority element always exist in the array.
###思路：
最根本的思路是既然众数的个数大于一半，那么剩下的与众数不相等的数可以和众数一一抵消，最后没有被抵消的就是众数。

用cnt来记录“候选答案”的个数。

遍历整个数组，如果当前cnt == 0说明没有候选答案，将候选答案设为当前便利到的值；如果不为0，那么比较当前值和候选答案，如果相等，cnt++；否则cnt--，相当于抵消。
###代码：

```
int majorityElement(vector<int>& nums) {
    int res, cnt = 0;
    int len = nums.size();
    for(int i = 0; i < len; ++i) {
        if(cnt == 0) {
            res = nums[i];
            cnt = 1;
        }else {
            if(res == nums[i]) ++cnt;
            else --cnt;
        }
    }
    return res;
}
```

##215. Kth Largest Element in an Array
###题目：
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,

Given **[3,2,1,5,6,4]** and k = 2, return 5.

**Note:**

You may assume k is always valid, 1 ≤ k ≤ array's length.
###思路：
维护大小为k的小根堆，最后返回堆顶元素即可。
###代码：

```
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> q;
    int len = nums.size();
    for(int i = 0; i < len; ++i) {
        if(q.size() < k) q.push(nums[i]);
        else {
            if(nums[i] > q.top()) {
                q.pop();
                q.push(nums[i]);
            }
        }
    }
    return q.top();
}
```

##241. Different Ways to Add Parentheses
###题目：
Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are +, - and *.


**Example 1**

Input: **"2-1-1"**.

```
((2-1)-1) = 0
(2-(1-1)) = 2
```
Output: **[0, 2]**

**Example 2**

Input: **"2 * 3-4 * 5"**

```
(2*(3-(4*5))) = -34
((2*3)-(4*5)) = -14
((2*(3-4))*5) = -10
(2*((3-4)*5)) = -10
(((2*3)-4)*5) = 10
```
Output: **[-34, -14, -10, -10, 10]**

###思路：
分治的思想

遍历字符串，如果遇到一个运算符，那么先得到这个运算符之前和之后的字符串的所有运算结果

再将这些结果两两匹配，获得最后所有的结果。
###代码：

```
vector<int> diffWaysToCompute(string input) {
    int len = input.size();
    vector<int> res;
    for(int i = 0; i < len; ++i) {
        char now = input[i];
        if(now == '+' || now == '-' || now == '*') {
            vector<int> lr = diffWaysToCompute(input.substr(0, i));
            vector<int> rr = diffWaysToCompute(input.substr(i+1));
            for(int l : lr) {
                for(int r : rr) {
                    if(now == '+') res.push_back(l+r);
                    else if(now == '-') res.push_back(l-r);
                    else res.push_back(l*r);
                }
            }
        }
    }
    if(res.empty()) res.push_back(stoi(input));
    return res;
}
```
