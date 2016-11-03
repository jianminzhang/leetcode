#Two pointer

##19	Remove Nth Node From End of List
注意考虑删除的元素是头、中间和尾部。

##28. Implement strStr()
注意needle的长度为0时，结果都是0.

可以使用 a.

##61. Rotate List
注意处理k % len == 0 和 len <= 1 ，直接返回head。

##75. Sort Colors*
使用三个指针。

一个l表示0的最后一个位置，一个r表示2的最开始的位置，再有一个p负责从左向右遍历。

```
	void sortColors(vector<int>& nums) {
        int len = nums.size();
        int l = 0, r = len - 1;
        int p = 0;
        while(p <= r) {
            if(nums[p] == 0) {
                if(p <= l) p++;
                else {
                    swap(nums[p], nums[l]);
                }
                l++; 
            }else if(nums[p] == 2) {
                swap(nums[p], nums[r]);
                r--;
            }else {
                p++;
            }
        }
    }
```

##125. Valid Palindrome **
不知道为什么不另外用一个string存下来有用的字符，就在原来的上面判断不可以，可能代码写错了。

##141. Linked List Cycle
注意：

1、链表的指针，值相同，next指针不同是两个，所以一旦存在环，就不可能走出；

2、ListNode *fast中，ListNode是类型，fast是一个指针，如果要定义两个，那么不可以简写。

要写成ListNode *fast, *slow;

做法：

一个快的指针，一个慢的指针，快的指针的速度是慢的两倍，进入循环的条件是 fast 和 fast->next都存在。如果出现 fast == slow 则存在环。

##142	Linked List Cycle II
做法：

先按照上一题的做法判断出有环，然后将slow置为head，fast 和 slow 的速度都变成1, 则他们两个再相遇的位置就是环的入口。

2 * (h + x) = h + nk + x  --------->  h = nk - x 即后面一次他们两个各自走的路程


##234. Palindrome Linked List
做法：

1、首先用一个fast和一个slow，fast是slow速度的两倍，slow则表示中点；

2、将slow和他后面的部分翻转，过程中首先在前面加一个NULL，然后比较翻转过来的部分和原来的前半部分是否相等。存在不等就不是回文。

##345. Reverse Vowels of a String
要注意a,e,i,o,u还有大写！












