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
when we need to find what is closest distance of 1s from 0s
we push all 0 int queue

[https://leetcode.com/problems/01-matrix/]