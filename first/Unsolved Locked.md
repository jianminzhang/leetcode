#Unsolved locked
##156. Binary Tree Upside Down
###题目：
Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

For example:

Given a binary tree **{1,2,3,4,5}**,

```
    1
   / \
  2   3
 / \
4   5
```
return the root of the binary tree [4,5,2,#,#,3,1].

```
   4
  / \
 5   2
    / \
   3   1  
```
###思路：
###代码：


##294. Flip Game II
###题目：
You are playing the following Flip Game with your friend: Given a string that contains only these two characters: **+** and **-**, you and your friend take turns to flip two consecutive **"++"** into **"--"**. The game ends when a person can no longer make a move and therefore the other person will be the winner.

Write a function to determine if the starting player can guarantee a win.

For example, given **s = "++++"**, return true. The starting player can guarantee a win by flipping the middle **"++"** to become **"+--+"**.

**Follow up:**

Derive your algorithm's runtime complexity.




##308. Range Sum Query 2D - Mutable
###题目:
Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

**Example:**

```
Given matrix = [
  [3, 0, 1, 4, 2],
  [5, 6, 3, 2, 1],
  [1, 2, 0, 1, 5],
  [4, 1, 0, 1, 7],
  [1, 0, 3, 0, 5]
]

sumRegion(2, 1, 4, 3) -> 8
update(3, 2, 2)
sumRegion(2, 1, 4, 3) -> 10
```
**Note:**

* The matrix is only modifiable by the update function.
* You may assume the number of calls to update and sumRegion function is distributed evenly.
* You may assume that row1 ≤ row2 and col1 ≤ col2.

###思路：
###代码：



##317. Shortest Distance from All Buildings
###题目：
You want to build a house on an empty land which reaches all buildings in the shortest amount of distance. You can only move up, down, left and right. You are given a 2D grid of values 0, 1 or 2, where:

* Each 0 marks an empty land which you can pass by freely.
* Each 1 marks a building which you cannot pass through.
* Each 2 marks an obstacle which you cannot pass through.

For example, given three buildings at **(0,0), (0,4), (2,2)**, and an obstacle at **(0,2)**:

```
1 - 0 - 2 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0
```
The point **(1,2)** is an ideal empty land to build a house, as the total travel distance of 3+3+1=7 is minimal. So return 7.

**Note:**

There will be at least one building. If it is not possible to build such house according to the above rules, return -1.

###思路：

###代码：



##351. Android Unlock Patterns
###题目：
Given an Android **3x3** key lock screen and two integers m and n, where 1 ≤ m ≤ n ≤ 9, count the total number of unlock patterns of the Android lock screen, which consist of minimum of m keys and maximum n keys.

**Rules for a valid pattern:**

* Each pattern must connect at least m keys and at most n keys.
* All the keys must be distinct.
* If the line connecting two consecutive keys in the pattern passes through any other keys, the other keys must have previously selected in the pattern. No jumps through non selected key is allowed.
* The order of keys used matters.

**Explanation:**

```
| 1 | 2 | 3 |
| 4 | 5 | 6 |
| 7 | 8 | 9 |
```
Invalid move: **4 - 1 - 3 - 6** 

Line 1 - 3 passes through key 2 which had not been selected in the pattern.

Invalid move: **4 - 1 - 9 - 2**

Line 1 - 9 passes through key 5 which had not been selected in the pattern.

Valid move: **2 - 4 - 1 - 3 - 6**

Line 1 - 3 is valid because it passes through key 2, which had been selected in the pattern

Valid move: **6 - 5 - 4 - 1 - 9 - 2**

Line 1 - 9 is valid because it passes through key 5, which had been selected in the pattern.

**Example:**

Given m = 1, n = 1, return 9.

**Credits:**

Special thanks to @elmirap for adding this problem and creating all test cases.

###思路：
###代码：

##353. Design Snake Game
###题目：
Design a Snake game that is played on a device with screen size = width x height. Play the game online if you are not familiar with the game.

The snake is initially positioned at the top left corner (0,0) with length = 1 unit.

You are given a list of food's positions in row-column order. When a snake eats the food, its length and the game's score both increase by 1.

Each food appears one by one on the screen. For example, the second food will not appear until the first food was eaten by the snake.

When a food does appear on the screen, it is guaranteed that it will not appear on a block occupied by the snake.

**Example:**

```
Given width = 3, height = 2, and food = [[1,2],[0,1]].

Snake snake = new Snake(width, height, food);

Initially the snake appears at position (0,0) and the food at (1,2).

|S| | |
| | |F|

snake.move("R"); -> Returns 0

| |S| |
| | |F|

snake.move("D"); -> Returns 0

| | | |
| |S|F|

snake.move("R"); -> Returns 1 (Snake eats the first food and right after that, the second food appears at (0,1) )

| |F| |
| |S|S|

snake.move("U"); -> Returns 1

| |F|S|
| | |S|

snake.move("L"); -> Returns 2 (Snake eats the second food)

| |S|S|
| | |S|

snake.move("U"); -> Returns -1 (Game over because snake collides with border)
```

###思路：
###代码：


##358. Rearrange String k Distance Apart
###题目：
Given a non-empty string str and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

**Example 1:**

```
str = "aabbcc", k = 3

Result: "abcabc"

The same letters are at least distance 3 from each other.
```
**Example 2:**

```
str = "aaabc", k = 3 

Answer: ""

It is not possible to rearrange the string.
```
**Example 3:**

```
str = "aaadbbcc", k = 2

Answer: "abacabcd"

Another possible answer is: "abcabcda"

The same letters are at least distance 2 from each other.
```
###思路：
###代码：



##360. Sort Transformed Array
###题目：
Given a **sorted** array of integers nums and integer values a, b and c. Apply a function of the form *f(x) = ax2 + bx + c* to each element x in the array.

The returned array must be in **sorted order**.

Expected time complexity: **O(n)**

**Example:**

```
nums = [-4, -2, 2, 4], a = 1, b = 3, c = 5,

Result: [3, 9, 15, 33]

nums = [-4, -2, 2, 4], a = -1, b = 3, c = 5

Result: [-23, -5, 1, 7]
```
###思路：
###代码：

##361. Bomb Enemy
###题目：
Given a 2D grid, each cell is either a wall **'W'**, an enemy **'E'** or empty **'0'** (the number zero), return the maximum enemies you can kill using one bomb.
The bomb kills all the enemies in the same row and column from the planted point until it hits the wall since the wall is too strong to be destroyed.
Note that you can only put the bomb at an empty cell.

**Example:**

```
For the given grid

0 E 0 0
E 0 W E
0 E 0 0

return 3. (Placing a bomb at (1,1) kills 3 enemies)
```
###思路：
###代码：




##366. Find Leaves of Binary Tree
###题目：
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

**Example:**

Given binary tree 

```
          1
         / \
        2   3
       / \     
      4   5    
```
Returns **[4, 5, 3], [2], [1]**.

**Explanation:**

* Removing the leaves **[4, 5, 3]** would result in this tree:

```
          1
         / 
        2      
```
* Now removing the leaf **[2]** would result in this tree:

```
          1          
```
* Now removing the leaf **[1]** would result in the empty tree:

```
          []         
```
Returns **[4, 5, 3], [2], [1]**.

###思路：

###代码：


##379. Design Phone Directory **
###题目：
Design a Phone Directory which supports the following operations:

* **get**: Provide a number which is not assigned to anyone.
* **check**: Check if a number is available or not.
* **release**: Recycle or release a number.

**Example:**

```
// Init a phone directory containing a total of 3 numbers: 0, 1, and 2.
PhoneDirectory directory = new PhoneDirectory(3);

// It can return any available phone number. Here we assume it returns 0.
directory.get();

// Assume it returns 1.
directory.get();

// The number 2 is available, so return true.
directory.check(2);

// It returns 2, the only number that is left.
directory.get();

// The number 2 is no longer available, so return false.
directory.check(2);

// Release number 2 back to the pool.
directory.release(2);

// Number 2 is available again, return true.
directory.check(2);
```
###思路：
###代码：



##411. Minimum Unique Word Abbreviation
###题目：
A string such as **"word"** contains the following abbreviations:

```
["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
```
Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the ***smallest possible*** length such that it does not conflict with abbreviations of the strings in the dictionary.

Each **number** or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.

**Note:**

* In the case of multiple answers as shown in the second example below, you may return any one of them.
* Assume length of target string = **m**, and dictionary size = **n**. You may assume that **m ≤ 21, n ≤ 1000**, and **log2(n) + m ≤ 20**.

**Examples:**

```
"apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")

"apple", ["plain", "amber", "blade"] -> "1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").
```
###思路：
###代码：

##418. Sentence Screen Fitting
###题目：
Given a **rows x cols** screen and a sentence represented by a list of words, find how many times the given sentence can be fitted on the screen.

Note:

* A word cannot be split into two lines.
* The order of words in the sentence must remain unchanged.
* Two consecutive words in a line must be separated by a single space.
* Total words in the sentence won't exceed 100.
* Length of each word won't exceed 10.
* 1 ≤ rows, cols ≤ 20,000.

**Example 1:**

```
Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output: 
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.
```
**Example 2:**

```
Input:
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output: 
2

Explanation:
a-bcd- 
e-a---
bcd-e-

The character '-' signifies an empty space on the screen.
```
**Example 3:**

```
Input:
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output: 
1

Explanation:
I-had
apple
pie-I
had--

The character '-' signifies an empty space on the screen.
```
###思路：
###代码：


##425. Word Squares
###题目：
Given a set of words **(without duplicates)**, find all word squares you can build from them.

A sequence of words forms a valid word square if the 
<math>
    <msubsup><mi>k</mi> <mi></mi> <mi>th</mi></msubsup>
</math> 
row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence **["ball","area","lead","lady"]** forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```
**Note:**

* There are at least 1 and at most 1000 words.
* All words will have the exact same length.
* Word length is at least 1 and at most 5.
* Each word contains only lowercase English alphabet **a-z**.

**Example 1:**

```
Input:
["area","lead","wall","lady","ball"]

Output:
[
  [ "wall",
    "area",
    "lead",
    "lady"
  ],
  [ "ball",
    "area",
    "lead",
    "lady"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```
**Example 2:**

```
Input:
["abat","baba","atan","atal"]

Output:
[
  [ "baba",
    "abat",
    "baba",
    "atan"
  ],
  [ "baba",
    "abat",
    "baba",
    "atal"
  ]
]

Explanation:
The output consists of two word squares. The order of output does not matter (just the order of words in each word square matters).
```
###思路：
###代码：


