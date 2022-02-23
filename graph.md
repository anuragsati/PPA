### Centroids in a graph [imp]
[https://leetcode.com/problems/minimum-height-trees/]

- If we view the graph as an area of circle, and the leaf nodes as the peripheral of the circle,
    then what we are looking for are actually the centroids of the circle,
    i.e. nodes that is close to all the peripheral nodes (leaf nodes).

- For the tree-alike graph, the number of centroids is no more than 2.

- To find centrod we can remove leaf nodes one by one 
    in topological sort fashion
    remove only when indegree becomes 0

- wait krna hai baaki edges khtm hone ka 
    that is basically topsort






### connected components on blocked edges in matrix

- we do normal dfs 
    when jumping we check if this jump is valid or not
    ex : path (11)->(21) is blocked
    so when we make jump from 11 to 21 we check if it exist in map or not
    if it exist then we dont move 







### Many times you might want to return bool from dfs 

- do not do return dfs(x)
    because that will not visit all current neighbours
    if false then only return false

```c++
    bool dfs(int node, vector<vector<int>> &adj){
        vis[node] = 1;

        for(auto x : adj[node]){
            if(vis[x] == 0){
                if(dfs(x, adj)==false)      //this is imp
                    return false;
            }
            if(vis[x] == 1)
                return false;
        }

        vis[node] = 2;
        return true;
    }
```



### DSU
[https://www.youtube.com/watch?v=kaBX2s3pYO4]
[https://cp-algorithms.com/data_structures/disjoint_set_union.html]

- used to find number of connected components 
    component related problems

- without union by size and path compression
    union : O(n)
    find : O(n)

- with path compression : 
    union : O(logn)
    find : O(logn)

- with both optimization
    union : O(1) average
    find : O(1) average

if we combine both optimizations - path compression with union by size / rank we will reach nearly constant time queries. 
It turns out, that the final amortized time complexity is alpha(n), where alpha is the inverse Ackermann function, which grows very slowly. 


```c++
    vector<int> parent(100000);
    vector<int> size(100000);

    void make_set(int v) {
        parent[v] = v;
        size[v] = 1;
    }

    //union by size
    void union(int a, int b) { 
        a = find_set(a);
        b = find_set(b);

        //if both are not in same set
        //make bigger set as leader
        if (a != b) {
            if (size[a] <= size[b]){
                parent[a] = b;
                size[b] += size[a];
            }
            else{
                parent[b] = a;
                size[a] += size[b];
            }
        }
    }

    //path compression
    int find_set(int node) { 
        if (node == parent[node])
            return node;

        return parent[node] = find_set(parent[node]);
    }
```



### Dijkstra shortest path [using priority queue]

- this is bfs but at each node we only select those nodes where we can reach in shortest time
- initially dist of all nodes = inf
- parent array is for reconstructing path

- priority queue <distance, node> 

- if we arrive at some node and its dist is smaller than our current path dist then we dont process it bcz
    already a smaller path exist for that node

- else we move forward, now we try to process neighbours if
    currentdist + weight of edge is smalller than dist of future node then only we will move to that
        set its new dist 
        set its parent to node
        and push it in queue
    
- basically bfs but we are trying to visit smaller path first that way if a bigger path comes we wont have to update it
    we just do work for smaller paths

- the reason we choose smaller dist to visit first is beacause if we choose bigger then we will have to again update it for smaller 
    but if we start from smaller then we wont have to do it for bigger


[https://www.youtube.com/watch?v=EFg3u_E6eHU]
- 2 step process
    update our neighbours
    move to neighbours in smallest priority

- if we explore some node then it is guaranteed that that will be shortes bcz if it is not then shortest would have already
    visited it first


- T.C : (V+E) Log V
- S.C : O(V)

```c++
unordered_map<ll, vector<pll>> adj;
const ll inf = 1e15;

void dijkstra(ll start, ll end, ll n){
    priority_queue<pll, vector<pll>, greater<pll>> pq;

    vector<ll> dist(n+10, inf);
    vector<ll> par(n+10, -1);
    dist[start] = 0;

    pq.push({dist[start], start});

    while(!pq.empty()){
        ll sz = pq.size();
        while(sz--){
            auto p = pq.top();
            pq.pop();

            ll node = p.second;
            ll curr_dist = p.first;

            //optional : optimization
            if(dist[node] < curr_dist)
                continue;
            
            for(auto x : adj[node]){
                ll v = x.first;
                ll next_dist = curr_dist + x.second;

                if(next_dist < dist[v]){
                    dist[v] = next_dist;
                    par[v] = node;
                    pq.push({next_dist, v});
                }
            }
        }
    }


    if(dist[end] == inf){
        cout << -1 << endl;
        return;
    }

    //print path
    ll i = end;
    vector<ll> path;
    while(i != -1){
        path.push_back(i);
        i = par[i];
    }

    reverse(path.begin(), path.end());
    for(auto x : path)
        cout << x << " ";
    return;
}
```




### Cycle in grid
[https://leetcode.com/problems/detect-cycles-in-2d-grid/]
[https://codeforces.com/problemset/problem/510/B]






### Detect cycles in a DAG / Directed graph
- comes in hady in topsort algo

-   0 : not visited
    1 : in process
    2 : completely visited


```c++
    void dfs(int v){

        vis[v] = 1;         //mark an in process

        for(auto x:g[v]){
            if(vis[x]==0)           //if not processed visit it
                dfs(x);
            else if(vis[x]==1)          // if encounterd a processed node means it has a cycle
                cycle = 1;
            else                //else dont process node at all bcz it has been visited
                continue;
        }

        vis[v] = 2;             // mark as finished processing
    }


	//to visit all nodes
	for(int i=1; i<=n; ++i)
		if(vis[i] != 2)
			dfs(i);
```






### Level order traversal without visited [imp]
[https://leetcode.com/problems/word-ladder-ii/]

- used to calculate all shortest paths
- maintain levels array
- if we arrive at one level at k time then if we arrive at same at k + x time then we dont visit it bcz
    we know k is smaller
- when we dont need visited but need to visit graph in level wise fashion


DP Approach :
there can be multiple shortest ways to reach destination
we cant use visited array here bcz there can be ways where we visit one node multiple times

idea : 
	once we have visited node at some level then we cant visit it at some later level
	this avoids cycles
	
	so we maintain a levels array for each node and before processing each node we check if this level has been visited before or not it has been visited before and current level is greater than previous level then we dont visit it as it will not lead to answer


```c++
class Solution {
public:
        unordered_map<string, int> level;

        queue<vector<string>> q;
        q.push(vector<string> {start});
        while(!q.empty()){
            int sz = q.size();
            while(sz--){
                //dont visit that node whose level is smaller  
                if(level.count(node) && level[node] < curlevel)
                    continue;

                level[node] = curlevel;
            }
        }

        return ans;
    }
```







### Graph DP
[https://leetcode.com/problems/cheapest-flights-within-k-stops/]

Approach 2 : 
	func(node, k) returns us minimum cost to reach destination from node using atmost k steps 
	for this we decrease k at each step and call for k+1 bcz we track number of edges



Here we must know that if we cannot apply bfs 
bfs is for undirected graph without weights
but because of constraint that we can have atmost k stops we can use bfs

we can't use visited array because there can be multiple ways to reach a vertex
with different costs

this will give TLE

we need to think that if we are reaching a vertex with cost = x
and we reach that vertex again with cost > x then we will not visit that again 
that is unnecessary computation

we store dist from source in dp array and whenever we reach a vertex we make sure that we are
reaching it with less distance


** mai ek index pe bohot saare tareeko se phuch skta hu
mai whi tareeka lunga jiska cost min hai **



### whenever we have [start, end] or [begin, end] then this might be graph problem of shortest path (bfs)



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





### distance from one string to other is graph problem [start, end]

at one string we try to make all possible states we can reach by modifying one letter at a time
then checking if it is a valid state or not
we also store what was originally at this place


[https://leetcode.com/problems/word-ladder/]
[https://leetcode.com/problems/minimum-genetic-mutation/submissions/]
[https://practice.geeksforgeeks.org/problems/2b9978653b4d905d12c04387a60e16464ef40733/1/]
[https://practice.geeksforgeeks.org/problems/40c021237f84e4abd74e2025f06221fb4cf4cd85/1]








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
[https://leetcode.com/problems/pacific-atlantic-water-flow/] (think in reverse)