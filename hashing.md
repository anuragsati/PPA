### check if points lie on same line
	for each point we make lines from it to all other points and calcuate their slopes 
	then store them in a map

```c++
	for(int i=0; i<n; ++i){
		map<pair<int, int> , int> m;
		for(int j=0; j<n; ++j){
			if(i == j)
				continue;
			
			int x1 = points[i][0], y1 = points[i][1];
			int x2 = points[j][0], y2 = points[j][1];

			int dy = y2 - y1;
			int dx = x2 - x1;
			int gd = __gcd(dx, dy);

			if(dx == 0){ //for infinite slopes 
				m[{inf, inf}]++;
			}
			else if(dy == 0){ //for 0 slopes
				m[{0,0}]++;
			}
			else{
				dx /= gd;
				dy /= gd;

				if((dx < 0 && dy < 0) || dy < 0)
					dx *= -1, dy *= -1;

				m[{dx, dy}]++;
			}
		}

		int tans = 0;
		for(auto x:m)
			tans = max(tans, x.second);
		
		ans = max(ans, tans);
	}
```


### whenever we need to club up we can use hashing



### in order to store major / minor diagonal we use [i-j] / [i+j] property and use a hash table



### manier time we transsform problem to sum=0 by changing all 0 -> -1


### count pairs/subarrays are generally hashing problems


### count subarrays whose sum is divisible by k

	if s1 % k = x
	and s2 % k = x

	then
	s1 = k * p + x
	s2 = k * q + x

	s2 - s1 = k(p-1)
	which is divisible by k
		
```c++
    int subarraysDivByK(vector<int>& nums, int k) {
        unordered_map<int, int> m;
        m[0]++;
        int pref=0, ans=0;
        
        for(int i=0; i<nums.size(); ++i){
            pref += nums[i];
            int crem = ((pref % k) + k) %k; //for negative numbers
            ans += m[crem];
            m[crem]++;
        }
        
        return ans;
    }
```



### Count distinct (i < j) pairs whose sum % k == 0
	already store a[i]%k in array
	so if x exits then we need k-x to also exist to make pair
	special case : number is divisible by k itself in that case we include that too


```c++
    int countDivisibiles(int a[], int n, int k){
        for(int i=0; i<n; ++i)
            a[i] %= k;
        
    	int ans = 0;
    	unordered_map<int, int> m;
    	for(int i=0; i<n; ++i){
    		if(a[i]==0)
    			ans += m[a[i]];
    		else
    			ans += m[k-a[i]];
    
    		m[a[i]]++;
    	}
    
    	return ans;
    }
```




### Boolean matrix (https://practice.geeksforgeeks.org/problems/boolean-matrix-problem-1587115620/1#)
	if a[0][0] == 1 then row0 and col0 will become 1
	if row0 contains 1 then row0 will become 1
	if col0 contains 1 then col0 will become 1

	use first row and column as hash table to solve in O(1)
		if a[i][j] == 1 then mark first element of that col. and row = 1 so that later we can mark whole row / col as 1

	finally after processing 
		from [1..n] if it is 1 mark whole row vis
		from [1..col] if it is 1 mark whole col vis

	if(top corner is 1)
		then mark whole first row & col vis

	otherwise if first row has 1 then make whole first row 1
	otherwise if first col has 1 then make whole first col 1

```c++
    void booleanMatrix(vector<vector<int> > &matrix){
		int n = matrix.size();
		int m = matrix[0].size();

		bool rzero = false, czero = false;
		bool tc;

		if(matrix[0][0] == 1) tc = true;
		else tc = false;

		for(int j=1; j<m; ++j){
			if(matrix[0][j] == 1){
				rzero = true;
				break;
			}
		}
		for(int i=1; i<n; ++i){
			if(matrix[i][0] == 1){
				czero = true;
				break;
			}
		}

		for(int i=1; i<n; ++i){
			for(int j=1; j<m; ++j){
				if(matrix[i][j] == 1){
					matrix[0][j] = 1;
					matrix[i][0] = 1;
				}
			}
		}

		//fill columns
		for(int j=1; j<m; ++j){
			if(matrix[0][j] == 1){
				for(int i=0; i<n; ++i){
					matrix[i][j] = 1;
				}
			}
		}
		//fill rows
		for(int i=1; i<n; ++i){
			if(matrix[i][0] == 1){
				for(int j=0; j<m; ++j){
					matrix[i][j] = 1;
				}
			}
		}

		//if matrix[0][0] = 1;
		if(tc || (rzero && czero)){
			for(int j=0; j<m; ++j)
				matrix[0][j] = 1;
			for(int i=0; i<n; ++i)
				matrix[i][0] = 1;
		}
		else{
			if(rzero)
				for(int j=0; j<m; ++j)
					matrix[0][j] = 1;
			if(czero)
				for(int i=0; i<n; ++i)
					matrix[i][0] = 1;
		}

    }
```




### Longest consecutive sequence
	we first find starting point of each chunk by checking 
	if a[i]-1 exits in a map or not
	if it exits then it is not a starting chunk skip it
	if it does not exits then we simply star a streak from that point and calculate ans
	
	warning : don't process duplicate starting points 


```c++
    int longestConsecutive(vector<int>& nums) {
		int n = nums.size();
		unordered_map<int, int> m;
		for(auto num:nums)
			m[num]++;
		
		int ans = 0;
		for(int i=0; i<n; ++i){
			if(m.find(nums[i]-1) != m.end() || m[nums[i]] == 0) //x-1 exists so don't process it or if x is duplicate of starting point
				continue;

			m[nums[i]] = 0;
			int nextnum = nums[i];
			int cnt = 0;
			while(m.find(nextnum) != m.end()){
				cnt++;
				nextnum++;
			}

			ans = max(ans, cnt);
		}

		return ans;
    }
```



### Check if array is consecutive
	ex : [1,2,3,4,5] , [5,4,3,1,2] are consecutive bcz they contain elements 

- Method 1 : using hashing
	
	we find the minimum element and if array is consecutive then it must contain [min, min+1, min+2 ... min+n-1]
	so we use hashing here : and for every value between min to min-n+1 we check if it exits or not
	if it doesnot exits then ans is false

```c++
	bool areConsecutives(long long a[], int n){ 
		unordered_map<long long, long long> m;
		long long mn = *min_element(a, a+n);

		for(int i=0; i<n; ++i)
			m[a[i]]++;
		
		for(int i=mn; i<=mn+n-1; ++i){
			if(m.find(i) == m.end())
				return false;
		}

		return true;
	}
```

- Method 2 : (for +ve numbers only)
	we subtract min element from all elements now the if array is consecutive it will contain elements from [0..n-1]
	if it contains foreign element then ans is false
	other wise we bring all elements to its posion in array by swapping and check if it is a good array or not

```c++
	bool areConsecutives(long long a[], int n){ 
		int mn = *min_element(a, a+n);
		for(int i=0; i<n; ++i){
			a[i] -= mn;

			if(a[i] >= n)
				return false;
		}

		for(int i=0; i<n; ++i){
			while(a[i]!=i)
				swap(a[i], a[a[i]]);
		}

		for(int i=0; i<n; ++i){
			if(a[i]!=i)
				return false;
		}
		
		return true;
	}
```


- Method 3 : using AP (for all numbers)
		this guarantees no duplicate elements bcz if elements are duplicate then sum would be lesser

		1. Find the sum of the array.
		2. If given array elements are consecutive that means they are in AP. So, find min element i.e. first term of AP then calculate 
			ap_sum = n/2 * [2a +(n-1)*d] where d = 1. So, ap_sum = n/2 * [2a +(n-1)]
		3. Compare both sums. Print Yes if equal, else No.


### Uses 
	- count of subarrays with sum = k
	- check if subarray with sum = k exits

```c++
    int subarraySum(vector<int>& nums, int k) {
		int n = nums.size();

		int pref = 0, ans=0;
		unordered_map<int, int> m;
		m[pref]++; //for empty prefix

		for(int i=0; i<n; ++i){
			pref += nums[i];
            
            //if same prefix exits add it to ans
			ans += m[pref-k];
			m[pref]++;
		}

		return ans;
    }
```