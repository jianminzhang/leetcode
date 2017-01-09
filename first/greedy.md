#Greedy
##134. Gas Station
###题目：
There are N gas stations along a circular route, where the amount of gas at station i is **gas[i]**.

You have a car with an unlimited gas tank and it costs **cost[i]** of gas to travel from station *i* to its next station (*i+1*). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.

**Note:**

The solution is guaranteed to be unique.
###思路：
从第一个位置出发，判断这个位置是否可以运行到下一个位置，可以的话，运行到下一个位置，并且将油量累加，直到运行到此次开始旅行的起始位置。返回起始位置。

如果不可以，从不可以的这个位置的下一个位置进行遍历。

如果将要遍历的下一个位置恰好是上一次旅行的起始位置，说明开始循环，并不可以得到结果，那么返回-1.
###代码：

```
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int num = gas.size();
    if(num == 0) return -1;

    for(int i = 0; i < num;) {
        int add = 0, now = i;
        bool yes = false;
        int start = i;
        while(!yes) {
            if(gas[now] + add < cost[now]) {
                i = now + 1;
                if(i == start) return -1;
                break;
            }
            add = gas[now] + add - cost[now];
            now = (now + 1) % num;
            if(now == start) {
                yes = true;
            }
        }
        if(yes) return start;
    }
    return -1;
}
```
##135. Candy
###题目：
There are N children standing in a line. Each child is assigned a rating value.

You are giving candies to these children subjected to the following requirements:

* Each child must have at least one candy.
* Children with a higher rating get more candies than their neighbors.

What is the minimum candies you must give?

###思路：
**方法1：空间复杂度O(N),时间复杂度O(N)**

用num数组来记录位置为i的孩子能获得的糖果数量

遍历两次ratings数组，一遍从左向右遍历，一次从右向左遍历。

从左向右：保证num[i] 和 num[i-1] 的关系符合要求；

从右向左：保证num[i] 和 num[i+1] 的关系满足要求。从右向左将num加和得到结果。

**方法2：空间复杂度为O(1), 时间复杂度为O(N)**

只需要从左向右遍历一次ratings数组。参考[解释](https://discuss.leetcode.com/topic/17722/two-c-solutions-given-with-explanation-both-with-o-n-time-one-with-o-1-space-the-other-with-o-n-space)
###代码：

```
//方法1：
int candy(vector<int>& ratings) {
    int len = ratings.size();
    vector<int> num(len, 1);
    for (int i = 1; i < len; ++i) 
        num[i] = (ratings[i] > ratings[i-1] ? num[i-1] + 1 : 1);
    int res = num[len-1];
    for (int i = len-2; i >= 0; --i) {
        if (ratings[i] > ratings[i+1] && num[i] < (num[i+1] + 1)) num[i] = num[i+1] + 1;
        res += num[i];
    }
    return res;
}


//方法2：
int candy(vector<int>& ratings) {
    const int len = ratings.size();
    if (len<=1) return len;
    int i, pPos, res=1, peak=1; // peak: # candies given to the i-1 child
    bool neg_peak = false; // flag to indicate if it is a local dip
    for (i=1; i<len;i++) {
        if(ratings[i] >= ratings[i-1]) {   // it is increasing
            if(neg_peak) {  
                // it is a local dip, we need to make sure i-1 has one candy
                res -= (peak-1) * (i-pPos - (peak>0));
                peak = 1;
                neg_peak = false;
            }
            // update child i candy number, if equal, set to 1
            peak = (ratings[i] == ratings[i-1]) ? 1 : ++peak;
            res += peak;
        }
        else { 
        // decreasing, just give one less candy, if it is the starting point of a decrease, update pPos
        	  if(!neg_peak) {
            		pPos = i-1; 
            		neg_peak = true;
            }
            res += --peak;
        }
    }
	// don't forget to update res, if the last one is a local dip
   return !neg_peak? res : res - (peak-1) * (i-pPos - (peak>0));
}
```
