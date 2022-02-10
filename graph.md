### Way to write dfs in grid
```c++
    void dfs(int r, int c, vector<vector<int>>& grid){
        if(r < 0 || c < 0 || r >= grid.size() || c >= grid[0].size())
            return;
        if(vis[r][c])
            return;
        
        vis[r][c] = true;
        dfs(r-1, c, grid);
        dfs(r+1, c, grid);
        dfs(r, c-1, grid);
        dfs(r, c+1, grid);
        return;
    }
```




### Cycles in undirected graph using DFS

- If graph have back edge it contains a cycle.
- **back edge** : edge who is connected to its ancestor other than its parent.
- Run dfs on each node
    - If node is visited check if it is equal to its parent if not then it is cycle.
    - if not visited visit and now pass new parent.

Returns true if contains cycle

```c++
    vector<int> a[sz];
    vector<int> vis(sz);
	bool cycle = false;

    void dfs(int node, int par){
		if(vis[node] == true){
			cycle = true;
			return;
		}

        vis[node] = true;       //visit this node

        for(auto x : a[node]){
			if(x == par) //don't visit the parent 
				continue;
			
			dfs(x, node); //pass current node as par to other nodes
        }
    }

```




### Bipartite check
[https://leetcode.com/problems/is-graph-bipartite/]

first we color the graph in 0 and 1's and keep marking them visited
if we reach a node which was already visited then we perform one extra check
if the color which was previously there == the current color 
if not this means that we are trying to color same node with 2 colors means graph is not bipartite




### count connected components : 
run a dfs from every node which is not visited and if you find that node 
visit that component


```c++
    int components=0;
    for(int i=0; i<n; ++i){
        if(!vis[i]){
            dfs(i);
            components++;
        }
    }
```





### distance from one string to other is graph problem
[https://leetcode.com/problems/word-ladder/]
[https://practice.geeksforgeeks.org/problems/2b9978653b4d905d12c04387a60e16464ef40733/1/]








### To maintain path in bfs we can create queue with path as parameter
[https://codeforces.com/problemset/problem/3/A]

    queue<vector<pii>> q;
    q.push(vector<pii> {start});

    ...

    for(int i=x-1; i<=x+1; ++i){
        for(int j=y-1; j<=y+1; ++j){
            path.push_back({i, j});
            q.push(path);
            path.pop_back();
        }
    }




### word ladder
[https://leetcode.com/problems/word-ladder/]
take advantage of small m constraint






### Time complexity of bfs O(v*e)
we are visiting all nodes and at each node we are doing O(e) amount of work to visit all adjacent nodes






### Parallel BFS
- used mostly in questions where we have many answers and we have to find min/max possible from many points

when we need to find what is closest distance of 1s from 0s
we push all 0 int queue

[https://leetcode.com/problems/01-matrix/]