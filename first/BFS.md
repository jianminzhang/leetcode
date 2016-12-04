#BFS
##130. Surrounded Regions
###题目：
Given a 2D board containing **'X'** and **'O'** (the letter O), capture all regions surrounded by **'X'**.

A region is captured by flipping all **'O'**s into **'X'**s in that surrounded region.

For example,

```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:

```
X X X X
X X X X
X X X X
X O X X
```
###思路：
从最外围的一圈进行开始进行判断，如果是O标为1，并且将与他相邻的O也标为1.（表示不会被包围的O）

判断完所有的部分，将所有的O变成X, 再将所有的1变成O。
###代码：

```
void solve(vector<vector<char>>& board) {
    int i, j;
    int row = board.size();
    if(row == 0) return;
    int col = board[0].size();

	for (i = 0; i < row; i++) {
		check(board, i, 0, row, col);
		if(col>1) check(board, i, col-1, row, col);
	}
	for (j = 1; j + 1 < col; j++) {
		check(board, 0, j, row, col);
		if(row>1) check(board, row-1, j, row, col);
	}
	for (i=0;i<row;i++)
		for(j=0;j<col;j++)
			if(board[i][j]=='O') board[i][j]='X';
	for (i=0;i<row;i++)
		for(j=0;j<col;j++)
			if(board[i][j]=='1') board[i][j]='O';
}
void check(vector<vector<char> >&vec, int i, int j, int row, int col) {
	if (vec[i][j] == 'O') {
		vec[i][j] = '1';
		if(i > 1) check(vec, i-1, j, row, col);
		if(j > 1) check(vec, i, j-1, row, col);
		if(i + 1 < row) check(vec, i+1, j, row, col);
		if(j + 1 < col) check(vec, i, j+1, row, col);
	}
}
```

##200. Number of Islands
###题目：
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

***Example 1:***

```
11110
11010
11000
00000
```
Answer: 1

***Example 2:***

```
11000
11000
00100
00011
```
Answer: 3
###思路：
遍历每一个位置，如果是1，那么开始bfs，将与其相连的部分标记为特殊符号，结果+1
###代码：

```
void BFS(vector<vector<char>>& grid, const int m, const int n, int x, int y) {
    int go[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    queue<pair<int, int>> Q;
    grid[x][y] = '@';
    Q.push(make_pair(x, y));
    while(!Q.empty()) {
        int prex = Q.front().first;
        int prey = Q.front().second;
        Q.pop();
        for(int i = 0; i < 4; ++i) {
            int nowx = prex + go[i][0];
            int nowy = prey + go[i][1];
            if(nowx < 0 || nowx >= m | nowy < 0 | nowy >= n) continue;
            if(grid[nowx][nowy] == '1') {
                grid[nowx][nowy] = '@';
                Q.push(make_pair(nowx, nowy));
            }
        }
    }
}

int numIslands(vector<vector<char>>& grid) {
    int res = 0;
    int m = grid.size();
    if(m == 0) return res;
    int n = grid[0].size();
    
    for(int i = 0; i < m; ++i) 
        for(int j = 0; j < n; ++j) 
            if(grid[i][j] == '1') {
                res++;
                BFS(grid, m, n, i, j);
            }
    return res;   
}
```
