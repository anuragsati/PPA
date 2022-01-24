# Dynamic Programming

-   0/1 knapsack
    -   subset sum
    -   equal sum partition
    -   count of subset sum
    -   minimum subset sum diff
    -   target sum
    -   no. of subset with given diff
-   Unbounded knapsack
-   LCS
-   LIS
-   Kadane's
-   Matrix chain multiplication
-   Dp on trees
-   Dp on grid


***
- Most 2D dp can be done in 1D by using the fact that we only need one row at a time

***
##  3 ways to write recursion

1. `include or not include`
    func(idx) => either include this or not include this

2. `Filling places one by one`
    we have n places _ _ _ _ _ _ _ _
    for a place we iterate through every element in array and try to fix that

    ```
    func(int idx){
        ...
        for(auto x : a)
            solve(idx+1, ...)
    }
    ```

    - this can lead to repeating sequences 
    - ex : 1 2 4, 2 1 4, 4 1 2 etc.

3. `Lexial filling`
    to solve repeating sequences we first choose one element then choose next element which is ahead of it
    ex: we choose element at 3 index to fill first place then to fill 2nd place we will start choosing from index 4 onwards

    ```
    func(int idx){
        ...
        for(int i=idx+1; i<n; ++i){
            func(i, ...);
        }
    }
    ```

    - this does not lead to repeating sequences
    - this doesnot need base case as well
    - this is needed if we have to make unique subsets


***


# ================================= 0/1 Knapsack =================================

### Subset Sum (T/F)

func(n, sum) ==> is it possible to make `sum` using [0.....n]

    `at each element we decice weajher to take it or not`

```c++
	bool isSubsetSum(int a[], int n, int sum){
		if(sum == 0)
			return true;
		if(n == 0 && sum != 0)
			return false;
		
		if(dp[n][sum])
			return dp[n][sum];

		if(sum-a[n-1] < 0)      //if sum is less then ignore it
			return dp[n][sum] = isSubsetSum(a, n-1, sum);
		else        //else take it or not take it
			return dp[n][sum] = isSubsetSum(a, n-1, sum-a[n-1]) || isSubsetSum(a, n-1, sum);
	}
```

```c++
    //n = size of arr 
    int dp[n+1][sum+1];

    //  T F F F F F F F
    //  T
    //  T
    //  T
    //  T
    //  T

    // 1st column true - we can make sum 0
    // 1 row except (0,0) false - can't make sum

    for(int i=1; i<=n; ++i){
        for(int j=1; j<=sum; ++j){
            if(a[i-1]<=j){
                dp[i][j] = dp[i-1][j] || dp[i-1][j-a[i-1]];
            }
            else{
                dp[i][j] = dp[i-1][j];
            }
        }
    }
```

### Equal sum partition

- first find out sum
- if sum is odd ans is not possible
- if sum is even check if we can make `sum/2` using subset sum method


### Count of subset whose sum = ?
- Only chane || to + in subset sum
- at each point we make a decision to take element or leave

func(n, sum) ==> total ways to make sum using elements [0....n]

                [0,10] 15
    [0,9] 13                [0, 9] 15
..      [0, 8] 13       [0, 8] 13      ..

```c++
	int func(int a[], int n, int sum){
		if(sum == 0)
			return true;
		if(n == 0 && sum != 0)
			return false;
		
		if(dp[n][sum])
			return dp[n][sum];

		if(sum-a[n-1] < 0)      //if sum is less then ignore it
			return dp[n][sum] = func(a, n-1, sum);
		else        //else take it or not take it
			return dp[n][sum] = func(a, n-1, sum-a[n-1]) + func(a, n-1, sum);
    }
```

### Minimum subset sum diff

- Minimize `abs(s2-s1)`
    also `s1 + s2 = range`

- Minimize `abs(range - 2*s1)`
- Maximize `s1` so that `abs(range-2*s1)` is minimum

- we find s1 in last row of `dp[n][sum]`
- finding the max `s1` in last row of dp matrix

- **Quick trick** : to find max `s1` we can start from sum/2 and move to 0 and find the first `true` in dp matrix's last row. That will be the max `s1` to exist

- **remember** : `s1` needs to be maximised if you are taking s1 in first half s2 can automatically be found in other half. so either maximise s1 or s2 it does not matter we just need to subtract that max from `range`. This max we can search in either half that's why we are finding in sum/2 only.

- one subset = s1[j] and other = sum - s1[j]
- thats why min diff = sum - 2*s1[j]


```c++
    // Calculate sum of all elements
    dp(Normal subset sum);

    // Find the largest j such that dp[n][j]
    // is true where j loops from sum/2 t0 0
    int diff = INT_MAX;
    for (int j=sum/2; j>=0; j--){
        if (dp[n][j]){
            diff = sum-2*j;
            break;
        }
    }
    return diff;

    // OR
    //loop through all possible values of s1 to find minimum sum-2*j

  	int ans = INT_MAX;
	for(int j=sum; j>=0; --j){
		if(dp[n][j] == true){
			ans = min(ans, sum-2*j);
		}
	}
	return ans;
```

### No. of subset with given diff

    s1 - s2 = diff
    s1 + s2 = sum
    =============
    2*s1 = diff + sum

    s1 = (diff + sum)/2     

- `diff` `sum` is given
- we need to find number of subsets that sum up to `s1`

```c++
    int s1 = (sum + diff)/2;

    dp(Normal count of subset sum, s1);

    cout << dp[n][s1];

```

### Target sum

- **Problem** assign `+ / -` to each element in array to make sum = D
- this is same as subset with differencd D
- answer reduces to finding 2 subsets whose difference is D
- let s1 be all + and s2 be all 
```
    s1 - s2 = diff
    s1 + s2 = sum
    =============
    2*s1 = diff + sum

    s1 = (diff + sum)/2    
```
- we need to find subset whose `sum = s1`





# ================================= Unbounded Knapsack =================================

-   Rod cutting
-   Coin change (Max ways)
-   Coin change (Min coins)

in unobounded knapsack we can reduce space by making use of fact that
we do not necessarily need index of element we can just loop through all elements

i.e for (auto x : coins)
        do  this ...

### Rod cutting
- **PROBLEM :** given rode of length `L` we can cut it in segments of a=[...] with price of each segment p=[...]. Find maximum price


- O(n^2) space
    - at each point we have 2 choices either to cut rod with this size or skip this size
```c++
	int rodcut(int i, int len, int* price, int* a){
		if(len == 0)
			return 0;
        if(i < 0 && len != 0)
            return INT_MIN;

		if(dp[i][len] != -1)
			return dp[i][len];

        if(len-a[i] >= 0)
    		return dp[i][len] = max(rodcut(i-1, len, price, a), price[i] + rodcut(i, len-a[i], price, a));
        else
    		return dp[i][len] = rodcut(i-1, len, price, a);
	}
```

- O(n) space method which makes use of fact that at each instance we can cut rod in any size from a=[...]
```c++
	int rodcut(int len=0, int* price, int n, vector<int> &dp){
		if(len == n)
			return 0;

		if(dp[len] != -1)
			return dp[len];

		int ans = 0;
		for(int cutsize=1; cutsize<=n; ++cutsize){
			if(len + cutsize <= n){
				ans = max(ans, price[cutsize-1] + rodcut(len+cutsize, price, n, dp));
			}
		}

		return dp[len] = ans;
	}
```



### Coin change (max ways)
- O(n^2) space way
- at one point we can choose this element and reduce our amount 
- or not choose and keep amount same
```c++
	int solve(int n, int amount, vector<int> &coins, vector<vector<int>> &dp){
        if(amount == 0)
			return 1;
		if(n <= 0 && amount != 0)
			return 0;
        
		if(dp[n][amount] != -1)
			return dp[n][amount];

        if(amount-coins[n-1] < 0)
            return dp[n][amount] = solve(n-1, amount, coins, dp);
        else
			return dp[n][amount] = solve(n-1, amount, coins, dp) + solve(n, amount-coins[n-1], coins, dp);
	}
```


### Coin change (min coins)

func(n, amount) => min coins needed to make `amount` by using array [0......n]

coices :
    - we choose the coin (we stay at same place bcz we still have choice to pick it again)
        `ans = 1 + solve(n, amount-coin[n])`
    - we dont choose  (we ignore the coin)
        `ans = solve(n-1, amount)`

```c++
    #define inf INT_MAX-1
    #define ll long long

	int solve(int i, int amount, vector<int> &coins, vector<vector<int>> &dp){
		if(amount == 0)
			return 0;
        if(i<0 && amount != 0)
            return inf;

		if(dp[i][amount] != -1)
			return dp[i][amount];

        if(amount-coins[i] >= 0)
            return dp[i][amount] = min(1 + solve(i, amount-coins[i], coins, dp), solve(i-1, amount, coins, dp));
        else
            return dp[i][amount] = solve(i-1, amount, coins, dp);
	}

    int coinChange(vector<int>& coins, int amount) {
        int n = coins.size();
		vector<vector<int>> dp(n+1, vector<int> (amount+1, -1));
		int ans = solve(coins.size()-1, amount, coins, dp);
		return (ans == inf) ? -1 : ans;
    }
```


- other method : at amount we have n choices to pick so we will take that choice which leads to min coins
    fun(x) => returns minimum coins to make x
```c++
	int solve(vector<int> &coins, int amount, vector<int> &cache){
		if(amount == 0)
			return 0;
		if(amount < 0)
			return inf;


		if(cache[amount] != -1)
			return cache[amount];

		int ans = inf;
		for(auto val : coins){
			ans = min(ans, 1 + solve(coins, amount-val, cache));
		}

		return cache[amount] = ans;
	}

    int coinChange(vector<int>& coins, int amount) {
		vector<int> cache(amount+1, -1);
		int ans = solve(coins, amount, cache);
		return (ans == inf) ? -1 : ans;
    }
```


# ================================= LIS =================================

#### LIS
- at every index check all previous index for lis jiske piche mai chipak skta hu
```c++
    int lengthOfLIS(vector<int>& a) {
        int n = a.size();
        vector<int> lis(n+1, 1);        //initially lis = 1
        int ans = 1;

        for(int i=0; i<n; ++i){
            for(int j=0; j<i; ++j){
                if(a[j] < a[i]){
                    lis[i] = max(lis[i], lis[j]+1);
                }
            }

            ans = max(ans, lis[i]);
        }

        return ans;
    }
```


# ================================= LCS =================================

### LCS
```c++
	int LCS(string &a, string &b, int m, int n){
		if(m==0 || n==0)
			return dp[m][n] = 0;

		if(dp[m][n] != -1)
			return dp[m][n];

		if(a[m-1] == b[n-1])
			return dp[m][n] = 1 + LCS(a, b, m-1, n-1);
		else
			return dp[m][n] = max(LCS(a, b, m-1, n), LCS(a, b, m, n-1));
	}
```

### Printing LCS
```c++
    vector<int> lcs;
    int i=n, j=m;
    while(i>0 && j>0){
        if(a[i-1] == b[j-1]){
          lcs.push_back(a[i-1]);
          --i, --j;
        }
        else{
            if(dp[i-1][j] > dp[i][j-1])
              --i;
            else
              --j;
        }
    }

    reverse(lcs.begin(), lcs.end());
    return lcs;
```

### Edit distance
[https://leetcode.com/problems/edit-distance/]

word1 = [.....i]
word2 = [.....j]

```c++
    //if word1 = "" word2 = "abc" then edit distance = 3 i.e j+1 
    //we can only insert
    if(i<0)
        return j+1;
    
    //if word1 = "abc" word2 = ""  then edit distance = 3 i.e i+1
    //we can only delete
    if(j<0)
        return i+1;


    if (s[i] == s[j])
        func(i-1, j-1);

    else
        min(
            1 + func(i-1, j-1)      //replacement
            1 + func(i, j-1)        //insertion  
            1 + func(i-1, j)        //deletion
        )
```

![picture 1](../images/dd506e8c71b98aa79f20d8e9ca6d58c91284dad27760e2c70e769b721cec19ed.png)  


```c++
    int ed(int i, int j, string &a, string &b){
        if(i<0)
            return j+1;
        if(j<0)
            return i+1;
        
        if(dp[i][j] != -1)
            return dp[i][j];
        
        
        if(a[i] == b[j])
            return dp[i][j] = ed(i-1, j-1, a, b);
        else
            return dp[i][j] = min({
                        1 + ed(i-1, j-1, a, b),
                        1 + ed(i, j-1, a, b),
                        1 + ed(i-1, j, a, b)
            });
    }

    // return ed(n-1, m-1, word1, word2);
```

- we can reduce space to O(n) because at one instance bcz at one instace we need only previous row

```c++
    int minDistance(string word1, string word2) {
        int n = word1.size();
        int m = word2.size();

        int dp[n+1][m+1];

        for(int i=0; i<=n; ++i)         // if a="abc" b="" then ED=i (1 index)
            dp[i][0] = i;

        for(int j=0; j<=m; ++j)         // if a="" b="ABC" then ED=j (1 index)
            dp[0][j] = j;
        
        for(int i=1; i<=n; ++i){
            for(int j=1; j<=m; ++j){
                if(word1[i-1]==word2[j-1])
                    dp[i][j] = dp[i-1][j-1];
                else
                    dp[i][j] = min({
                        1 + dp[i-1][j-1],   //replace
                        1 + dp[i][j-1],     //insert 
                        1 + dp[i-1][j]      //delete
                    });
            }
        }

        return dp[n][m];
    }
```

### Wildcard pattern matching 
[https://leetcode.com/problems/wildcard-matching/]

Everything written here
can also be done in O(n) space using prev. row only

### Distinct Subsequences
[https://leetcode.com/problems/distinct-subsequences/submissions/]


### Longest palindromic subsequence (LPS)

```c++
    int lps(int l, int r, string &s, vector<vector<int>> &dp){
        if(l > r)
            return 0;
        else if(l==r)
            return 1;

        if(dp[l][r] != -1)
            return dp[l][r];

        if(s[l] == s[r])
            return dp[l][r] = 2 + lps(l+1, r-1, s, dp);
        else
            return dp[l][r] = max(lps(l+1, r, s, dp), lps(l, r-1, s, dp));
    }
```

### Longest palindromic substring
[https://leetcode.com/problems/longest-palindromic-substring/]

```c++
    pair<int, int> ans = {0, 0};
    int mx = 1;

    bool lps(int i, int j, string &s, vector<vector<int>> &dp){
        if(i > j)
            return true;

        if(dp[i][j] != -1)
            return dp[i][j];

        if(s[i]==s[j] && lps(i+1, j-1, s, dp)){     //if start & ending char are same and middle is palindrome then this is palindrome
            if(j-i+1 > mx){     //set this as new ans
                mx = j-i+1;
                ans = {i, j};
            }

            return dp[i][j] = true;     //current substring is palindrome return true
        }
        else{
            lps(i+1, j, s, dp);
            lps(i, j-1, s, dp);
            return dp[i][j] = false;
        }
    }

    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<int>> dp(n, vector<int> (n, -1));
        lps(0, n-1, s, dp);
        return s.substr(ans.first, ans.second-ans.first+1);
    }
```







# ================================= Diagonal DP =================================

### MCM
[https://practice.geeksforgeeks.org/problems/matrix-chain-multiplication0303/1#]

- Recursion 
    we try to divide the matrices in brackets
    f(i, j) => returns minimum possible computations to multiply matrices from [i...j]

    so we divide by all possible ways just like trying to find all possible ways to add paranthesis

            xxxx 
    x|xxx   xx|xx   xxx|x

- Plain recursion by tracing dimentions too
```c++
    pair<int, pair<int,int>> solve(int l, int r, vector<pair<int, int>> &a){
        if(l == r)
            return {0, {a[l].first, a[l].second}};

        pair<int, pair<int, int>> ans = {INT_MAX, {INT_MAX, INT_MAX}};
        for(int i=l; i<=r-1; ++i){
            pair<int, pair<int,int>> lst = solve(l, i, a);
            pair<int, pair<int,int>> rst = solve(i+1, r, a);

            int curr = lst.second.first * lst.second.second * rst.second.second;

            int tot = lst.first + rst.first + curr;

            ans = min(ans, {tot, {lst.second.first, rst.second.second}});
        }

        return ans;
    }

    // return solve(0, a.size()-1, a).first;
```


- memoisation : we dont need dimentions we can just use fact that dimention of final matrix = l * mid * r
- ![picture 2](../images/90468aa364003b551fc1ec693fa6a80e3b6f51d35c163de9a5a3bdab54bb8d58.png)  
- T.C : O(n^3)
```c++
    int solve(int l, int r, vector<pair<int, int>> &a, vector<vector<int>> &dp){
        if(l == r)
            return 0;
        
        if(dp[l][r] != -1)
            return dp[l][r];

        int ans = INT_MAX;
        for(int i=l; i<=r-1; ++i){
            int lst = solve(l, i, a);
            int rst = solve(i+1, r, a);

            int curr = a[l].first * a[i].second * a[r].second;
            int tot = lst + rst + curr;

            ans = min(ans, tot);
        }

        return dp[l][r] = ans;
    }
```

- Bottom up 
```c++
    int matrixMultiplication(int n, int arr[]) {
        vector<pair<int, int>> a;
        for(int i=1; i<n; ++i){
            a.push_back({arr[i-1], arr[i]});
        }

        n = a.size();
        int dp[n+1][n+1];

        for(int d=0; d<n; ++d){
            for(int i=0, j=d; i<n && j<n; ++i, ++j){
                if(d==0){
                    dp[i][j] = 0;
                    continue;
                }

                int ans = INT_MAX;
                for(int k=i; k<j; ++k){
                    int curr = a[i].first * a[k].second * a[j].second;
                    int tot = curr + dp[i][k] + dp[k+1][j];

                    ans = min(ans, tot);
                }

                dp[i][j] = ans;
            }
        }

        return dp[0][n-1];
    }
```


# ================================= Bitmasking DP =================================

- It is used when we have to store whole vector as a state
- beauty of bitmasking is that you can represent whole vector using just an integer
