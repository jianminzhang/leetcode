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
