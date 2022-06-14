# Max index diff [suraj_bhaiya_question]
[https://www.geeksforgeeks.org/given-an-array-arr-find-the-maximum-j-i-such-that-arrj-arri/]



# special election
[https://www.codechef.com/MARCH18B/problems/MINVOTE]

+1 / -1 array
binary search on pref



# minimum size subarray with sum >= k
[https://leetcode.com/problems/minimum-size-subarray-sum/]




# activity selection (k person)
[https://www.geeksforgeeks.org/activity-selection-problem-with-k-persons/]






# Max distance (sorting)
[https://practice.geeksforgeeks.org/problems/maximum-index-1587115620/1]
[https://www.interviewbit.com/problems/max-distance/]

first we see we need a[i] <= a[j] and i<=j and we need max of j-i

so we fix one condition a[i] <= a[j] by sorting array but this will disrupt indexes we want original index so we store index too

now at any index ...... x .... we want to get max index in front of it for every value
so we calculate a suffix array to store max index after an index

```c++
    int maxIndexDiff(int A[], int N) { 
        vector<pair<int, int> > a;
        vector<int> smax(N);
        for(int i=0; i<N; ++i){
            a.push_back({A[i], i});
        }
        
        sort(a.begin(), a.end());
        
        for(int i=N-1; i>=0; --i){
            if(i==N-1) smax[i] = a[i].second;
            else smax[i] = max(a[i].second, smax[i+1]);
        }
        
        int ans = INT_MIN;
        for(int i=0; i<N-1; ++i){
            int tans = - a[i].second + smax[i];
            ans = max(ans, tans);
        }
        
        return ans;
    }
```





# another way of recursion {not really recursion it is iteration}
[https://leetcode.com/problems/decode-string/]

```c++
	string helper(int &pos, string s){
		int num = 0;
		string ans = "";

		for( ; pos<s.size(); ++pos){
			char c = s[pos];

			if(c == '['){
				string temp = helper(++pos, s);
				for(int i=0; i<num; ++i)
					ans += temp;
				
				num = 0;
			}
			else if(c == ']'){
				return ans;
			}
			else if(c >= '0' && c <= '9'){
				num = num * 10 + (c-'0');
			}
			else{
				ans += c;
			}
		}

		return ans;
	}

    string decodeString(string s) {
		int pos = 0;
		return helper(pos, s);
    }
```








# Count Number of Nice Subarrays
[https://leetcode.com/problems/count-number-of-nice-subarrays/]

- very imp idea that can be extended to others (number of subarrays with sum = k)
 
```c++
    int numberOfSubarrays(vector<int>& nums, int k) {
		int n = nums.size();

		for(int i=0; i<n; ++i){
			if(nums[i] & 1) nums[i] = 1;
			else nums[i] = 0;
		}

		int pref=0, ans=0;
		map<int, int> m; 
		m[pref]++;

		for(int i=0; i<n; ++i){
			pref += nums[i];
			ans += m[pref-k];
            m[pref]++;

		}

		return ans;
    }
```





# Distinct rectangles [pp. 2pointers lec : 7]
given arr of distinct elements fint how many rectangles (lxb) can be made whose area <= B (lxb) is different from (bxl)
and (lxl) and (bxb) are also valid

x x x x x x x x x x 
L 				  R 

fix one of dimentions as L now if L x R is greater than B we can decrease R 
if it is <= B then total rectangles possible in this range is 2*(r-l+1) - 1; -1 because we are considering lxl twice

then we move our L

we are not missing any because if lxr is greater than B than surely something greater than l thn (lxb) will be even more greater

```c++
	int l = 0, r = n-1;
	while(l<=r){
		if(a[l] * a[r] <= B){
			ans += 2 * (r-l+1) -1;
			++l;
		}
		else
			--r;
	}
```



# [Subarray With B Odd Numbers]
- https://www.interviewbit.com/problems/subarray-with-b-odd-numbers/




# count of distinct pair with diff k
[https://practice.geeksforgeeks.org/problems/count-distinct-pairs-with-difference-k1233/1#]





# Max consecutive 1's by changing m 0's
[https://www.geeksforgeeks.org/find-zeroes-to-be-flipped-so-that-number-of-consecutive-1s-is-maximized/]


- other approach O(n) O(1)space
	For all positions of 0’s calculate left[] and right[] which defines the number of consecutive 1’s to the left of i and right of i respectively. 
	store indexes of all zeroes in a third array say zeroes[]. 
	Now traverse zeroes[] and for all consecutive m entries in this array, compute the sum of 1s that can be produced. This step can be done in O(n) using left[] and right[]. 


- sliding window
	increase window size until we reach more than m zeroes 
	once we reach m zeroes decrease window size

```c++
    int appleSequences(int n, int m, string s){
		int l = 0, r = 0;
		int ans = 0;
		int z = 0;
		while(r < n){
			if(s[r] == 'O')
				++z;
			
			if(z <= m){
				ans = max(ans, r-l+1);
				++r;
			}
			else{
				while(l<n && l<r && s[l]!='O'){
					++l;
				}
				++l;
				--z;
				++r;
			}
		}

		return ans;
    }
```



# Kth smallest in an unsorted array (binary search on ans)



# https://leetcode.com/problems/remove-duplicate-letters/



# [Minimum lights to activate] 
[https://www.interviewbit.com/problems/minimum-lights-to-activate/]