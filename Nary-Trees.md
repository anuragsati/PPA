
- Represented by adjacency list
- by default edges are bidirectional
- can be rooted or unrooted


### Diameter 
- If i change the root of tree height will change
- diameter remains same

- If root not given then you can take any node as root

- if we are at a node we want to consider two maximum heights for cuuent diameter
- we will pass max diameter from all childs

- at each node we try to calculate max height using 1 + max of child height
- diameter = max of child diameter or 1 + max height + 2nd max height among all childs
    basically we are trying to find 2 max heights among all childs
    we try to maintan a max height variable that denoes max height till now and we take max with cuurent nodes height + 1

- to find sum of two max in running array
    max(ans, max + current)

```c++
    //[diameter, height]
    pair<int, int> diameter(int node, int par){
        if(adj[node].size() == 1)
            return {0, 0};

        int mxd = 0;
        int mxh = 0;

        for(auto x : adj[node]){
            if(x == par)
                continue;
            
            auto [cd, ch] = diameter(x, node);      //gives child diameter and child height

            mxd = max({mxd, cd, 1 + mxh + ch });
            mxh = max(mxh, 1 + ch);
        }

        return {mxd, mxh};
    }
```

OR
[imp]

```c++
    //[diameter, height]
    pair<int, int> diameter(int node, int par){
        if(adj[node].size() == 1)
            return {0, 0};

        int mxd = 0;

        vector<pair<int, int>> temp;
        for(auto x : adj[node]){
            if(x == par)
                continue;
            
            temp.push_back(diameter(x, node));      //gives child diameter and child height
        }


        //find max two height
        int mh=0, smh=0;
        for(auto x : temp){
            if(mh < x.second) {
                smh = mh;
                mh = x.second;
            }
            else if(smh < x.second)
                smh = x.second;
        }

        //find max diameter
        for(auto x : temp){
            mxd = max({mxd, x.first, mh+smh+1});
        }

        return {mxd, 1+mh};
    }
```






### Height

```c++
    void height(int node, int par){
        if(adj[node].size() == 1){
            return 0;
        }

        int mx = 0;
        for(auto x : adj[node]){
            if(x == node)
                continue;

            mx = max(mx, height(x, node));
        }

        return 1 + mx;
    }


    adj[root].push_back(-1);  // dummy parent -1
    dfs(root, -1);      //current, parent
```



### DFS Traversal
    normal dfs but we stop at nodes which are leaf
    leaf nodes are those which have 1 element in adj. list (for undirected tree)
    for directed tree leaf nodes adj. list is empty

    root node can also have only 1 node in adj. list
    to distinguish root node from leaf node we can add dummy parent node to adj. list of root

```c++

    void dfs(int node, int par){
        if(adj[node].size() == 1){
            //leaf
            return;
        }

        cout << node ;

        for(auto x : adj[node]){
            if(x == par)        //if parent then dont visit
                continue;
            
            dfs(x, node);
        }
    }


    adj[root].push_back(-1);  // dummy parent -1
    dfs(root, -1);      //current, parent
```