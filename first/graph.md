#graph
##133. Clone Graph
###题目：
Clone an undirected graph. Each node in the graph contains a label and a list of its neighbors.


**OJ's undirected graph serialization**:

Nodes are labeled uniquely.

We use ```#``` as a separator for each node, and , as a separator for node label and each neighbor of the node.

As an example, consider the serialized graph ```{0,1,2#1,2#2,2}```.

The graph has a total of three nodes, and therefore contains three parts as separated by ```#```.

 1. First node is labeled as 0. Connect node 0 to both nodes 1 and 2.
 2. Second node is labeled as 1. Connect node 1 to node 2.
 3. Third node is labeled as 2. Connect node 2 to node 2 (itself), thus forming a self-cycle.

Visually, the graph looks like the following:

```
       1
      / \
     /   \
    0 --- 2
         / \
         \_/
```
###思路：

###代码：

```
unordered_map<UndirectedGraphNode*, UndirectedGraphNode*> hash;
UndirectedGraphNode *cloneGraph(UndirectedGraphNode *node) {
    if (!node) return node;
    if (hash.find(node) == hash.end()) {
        hash[node] = new UndirectedGraphNode(node->label);
        for (auto x : node->neighbors) 
            (hash[node]->neighbors).push_back(cloneGraph(x));
    }
    return hash[node];
}
```

##207. Course Schedule
###题目：
There are a total of n courses you have to take, labeled from **0** to **n - 1**.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: **[0,1]**

Given the total number of courses and a list of prerequisite **pairs**, is it possible for you to finish all courses?

For example:

```
2, [[1,0]]
```
There are a total of 2 courses to take. To take course 1 you should have finished course 0. So it is possible.

```
2, [[1,0],[0,1]]
```
There are a total of 2 courses to take. To take course 1 you should have finished course 0, and to take course 0 you should also have finished course 1. So it is impossible.

**Note:**

The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
###思路：
转换成有向图找环的问题，有环则不可以，无环可以。

如果拓扑排序可以处理所有的点，说明没有环，否则有环。
###代码：

```
//拓扑排序
bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
    unordered_map<int, set<int>> pre, post;
    stack<int> one;
    int len = prerequisites.size();
    for (int i = 0; i < len; ++i) {
        int f = prerequisites[i].first, s = prerequisites[i].second;
        pre[f].insert(s);
        post[s].insert(f);
    }
    for (int i = 0; i < numCourses; ++i) {
        if (pre.find(i) == pre.end()) one.push(i);
    }
    int num = 0;
    while (!one.empty()) {
        num++;
        int now = one.top();
        one.pop();
        if (post.find(now) == post.end()) continue;
        set<int>::iterator it;
        for (it = post[now].begin(); it != post[now].end(); ++it) {
            pre[*it].erase(now);
            if (pre[*it].empty()) one.push(*it);
        }
    }
    if (num < numCourses) return false;
    else return true;
}
```

##210. Course Schedule II
###题目：
There are a total of n courses you have to take, labeled from **0** to **n - 1**.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: **[0,1]**

Given the total number of courses and a list of prerequisite **pairs**, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

For example:

```
2, [[1,0]]
```
There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is **[0,1]**

```
4, [[1,0],[2,0],[3,1],[3,2]]
```
There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is **[0,1,2,3]**. Another correct ordering is**[0,2,1,3]**.

**Note:**

The input prerequisites is a graph represented by **a list of** edges, not adjacency matrices. Read more about how a graph is represented.
###思路：
思路和上一题一样，代码只是多了要按照处理的顺序记录每一个点

最后如果处理的点的个数等于点的总数，那么返回结果，否则清空结果集合并返回。
###代码：

```
vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
    vector<int> res;
    unordered_map<int, unordered_set<int>> pre, post;
    stack<int> one;
    int len = prerequisites.size();
    for (int i = 0; i < len; ++i) {
        int f = prerequisites[i].first, s = prerequisites[i].second;
        pre[f].insert(s);
        post[s].insert(f);
    }
    for (int i = 0; i < numCourses; ++i) {
        if (pre.find(i) == pre.end()) one.push(i);
    }
    while (!one.empty()) {
        int now = one.top();
        res.push_back(now);
        one.pop();
        if (post.find(now) == post.end()) continue;
        unordered_set<int>::iterator it;
        for (it = post[now].begin(); it != post[now].end(); ++it) {
            pre[*it].erase(now);
            if (pre[*it].empty()) one.push(*it);
        }
    }
    if ((int)res.size() < numCourses) res.clear();
    return res;
}
```

##310. Minimum Height Trees
###题目：
For a undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

Format

The graph contains n nodes which are labeled from **0 to n - 1**. You will be given the number **n** and a list of undirected **edges** (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

**Example 1:**

Given n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3
return [1]

**Example 2:**

Given n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5
return [3, 4]

**Hint:**

How many MHTs can a graph have at most?

**Note:**

(1) According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”

(2) The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.
###思路：

###代码：

