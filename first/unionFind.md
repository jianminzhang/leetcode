#Union Find
##128. Longest Consecutive Sequence*
###题目：
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

For example,

Given **[100, 4, 200, 1, 3, 2]**,

The longest consecutive elements sequence is **[1, 2, 3, 4]**. Return its length: **4**.

Your algorithm should run in *O(n)* complexity.
###思路：
这个题的关键在于复杂度要求高，否则先进行排序然后遍历一次即可。

这个题运用了并查集的思想。

将数组中的数字用hash表存储，对应值为1，如果后续处理过即已经进行了合并操作，那么对应值为-1；

遍历数组中的数字，如果hash为-1，则说明已经处理过这个数字，不再重复处理，这里保证了复杂度，也运用了并查集的思想，处理过即进行过合并；

如果数组中的数字原来没有处理过，那么向这个数字的左右进行延伸，判断延伸到的数字是否出现在数组中，如果是数组中的元素，那么将其hash=-1,且继续延伸

这里用到了一个小技巧，向左时包含当前数字本身，向右则不包含。
###代码：

```
int longestConsecutive(vector<int>& nums) {
    unordered_map<int, int> hash;
    int len = nums.size();
    for (int i : nums) hash[i] = 1;
    int res = 1;
    for (int i = 0; i < len; ++i) {
        if (hash[nums[i]] == -1) continue;
        int l = 0;
        while (hash.find(nums[i]-l) != hash.end()) {
            hash[nums[i]-l] = -1;
            ++l;
        }
        int r = 1;
        while (hash.find(nums[i]+r) != hash.end()) {
            hash[nums[i]+r] = -1;
            ++r;
        }
        r = r - 1;
        res = max(res, (l+r));
    }
    return res;
}
```

