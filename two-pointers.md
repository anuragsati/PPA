### subarray sum = k can be done using 2 pointer if there are no -ve




### DNF problem (sort 0 1 and 2s)

- 0s will be sorted similar to partition problem
- 2s will be sorted similar but when we swap 2 we might mess up order 
    so we will not immediately move forward we will fix this first one more time

```c++
    void Solution::sortColors(vector<int> &a) {
        int n = a.size();
        int l=-1, r=n;
        for(int i=0; i<r; ++i){
            if(a[i] == 0)
                swap(a[i], a[++l]);
            else if(a[i] == 2){
                swap(a[i], a[--r]);
                --i;
            }
        }
    }
```




=======================================================
# 2 pointer patterns (sliding window)
- [https://leetcode.com/list/x1lbzfk3/]

- [https://leetcode.com/discuss/general-discussion/657507/sliding-window-for-beginners-problems-template-sample-solutions/]

- [https://leetcode.com/problems/frequency-of-the-most-frequent-element/discuss/1175088/C%2B%2B-Maximum-Sliding-Window-Cheatsheet-Template!]



### generally `longest substring` is sliding window

### exactly(k) = atmost(k) - atmost(k-1)

### atmost trick : when you want to find number of subarray in range [a, b]
    or when you want exactly k in a window
[https://www.interviewbit.com/problems/numrange/]


### [VVVIMP] subarrays with unique elements atmost equal to k

- here at each point we get a valid substring ending at r
- at each step we move r by 1 and try to adjust l to make window valid
- at each point we get a valid window [l, r] 

```c++
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
        unordered_map<int, int> m;
        int ans=0, n=s.size();
        for(int r=0; r<n; ++r){
            m[s[r]]++;   //hashmap to subarray ending at current indix

            while(l<=r && m.size() > k){    //while to reduce window
                m[s[l]]--;
                if(m[s[l]]==0)
                    m.erase(s[l]);
                ++l;
            }

            //updating ans
            ans += (r-l+1);     
            // ans = max(ans, r-l+1) for longest substring with atmost k distinct
        }

        return ans;
    }
```





=======================================================




# Sum of all subarrays
- standard problem
- i can pick (i+1) elements on left and (n-i) on right to include this element

```c++
    ll SubArraySum(vector<int> arr) {
        ll result = 0;
    
        // computing sum of subarray using formula
        for (int i=0; i<n; i++)
            result += (arr[i] * (i+1) * (n-i));
    
        // return all subarray sum
        return result ;
    }
```



# Ugly numbers (prime factors 2,3,5)
[https://practice.geeksforgeeks.org/problems/ugly-numbers2254/1#]

brute force : for each number check p.f and then verify

observation : if we have a valid number n then we can create 2*n, 3*n, 5*n by it 
so we only create the valid numbers

every number n can be created by multiplying 2,3,5 to its previous numbers
now we just need to align numbers in right places

for ith number we find 
p2 = first index for which 2th multiple has not been created
p3 = first index for which 3th multiple has not been created
p5 = first index for which 5th multiple has not been created

and we find min of those that will be number at this position.

but these are redundant calculations we are doing for every i
bcz for i => p2 the i+1 will also be greater or equal than p2
so we can maintain a pointer which we will increment if we use this










# Minimize (max(A[i], B[j], C[k]) – min(A[i], B[j], C[k])) of three different sorted arrays
[https://www.geeksforgeeks.org/minimize-maxai-bj-ck-minai-bj-ck-three-different-sorted-arrays/]

the idea is to minimize exp. then we need to decrease diff between max ele. and min ele.
1. that we can do by increasing min element  (move in forward direction)
2. or by decreasing max element  (move in backward direction)

using 1. if one of pointers reaches end we can say that after this no ans. is possible as min. element remains same and other items increase which increases the answer

		x . y . z









# Equivalent Sub-Arrays 
[https://practice.geeksforgeeks.org/problems/equivalent-sub-arrays3731/1]

hard 2 pointers with implementation

if we have found for some i a j such that between [i, j] there are k distinct elements then all subarrays after theat j will also have k
distinct elements as k is max distict elements of subarray

```c++
    int countDistinctSubarray(int a[], int n) {
		unordered_map<int, int> m;
		for(int i=0; i<n; ++i)
			m[a[i]]++;
		
		int origde = m.size();
		m.clear();

		int ans = 0;
		int de = 0;	//distinct elements
		int l = 0, r = 0;

		for(l= 0; l<n; ++l){
			while(r < n && de < origde){
				m[a[r]]++;

				if(m[a[r]] == 1) 	//this element is the new element
					++de;
				
				r++;
			}

			if(de == origde) 		//+1 bez r points to 1 elment forward
				ans += (n-r+1);
			

			//remove this element
			m[a[l]]--;

			if(m[a[l]] == 0)
				--de;
		}

		return ans;
    }
```



# find last element >= x towards right side
# can be extended to first item <= x or min or max element to right / left
	just take suffix_max array from right side and do binary search on that number
	[https://leetcode.com/problems/container-with-most-water/]

	arr   1 8 6 2 5 4 8 3 7
	suff  8 8 8 8 8 8 8 7 7 

	now last element just greater than or equal to 6 is 7 (we want last occurance of this)





# two pointers can be used to convert n2 => n algorithm


# whenever in 2 pointer technique
	always calculate answer for first window then calculate for other windows ex below

	this avoids recalculating a[r] every time
	this is the ideal way to slide window in 2 pointer

	l=0, r=0;
	cur = a[0];
	while(l<r){
		if(cur < X)
			ans = ..
			++r;
			if(r<n) cur += a[r];
		
		else
			cur -= a[l];
			++l;
	}



# Smallest subarray with sum greater than x
[https://practice.geeksforgeeks.org/problems/smallest-subarray-with-sum-greater-than-x5651/1#]


```c++
    int smallestSubWithSum(int a[], int n, int x) {
		int l = 0, r = 0;
		int csum = a[0];
		int ans = INT_MAX;
		while(r<n){
			if(csum <= x){
				++r;
				if(r < n) csum += a[r];
			}
			else{
				ans = min(ans, r-l+1);
				csum -= a[l];
				++l;
			}
		}

		return ans;
    }
```




# Count Number of Nice Subarrays
[https://leetcode.com/problems/count-number-of-nice-subarrays/]




# whenever we have 2 strings / count subarray / window / max/min substring consider thinking about 2 pointers 





# Minimum window substring between two strings
[https://leetcode.com/problems/minimum-window-substring/]





# very common trick to get unique pairs in 2sum 3sum 4sum

```c++
	for(int i=0; i<n; ++i){
		if(i!=0 && a[i] == a[i-1]) 			// remove duplicates starting points but always consider first element
			continue;
		
		for(int j=i+1; j<n; ++j){
			if(j!=i+1 && a[j] == a[j-1]) 		//remove duplicates but always consider first element
				continue;
```

```c++
	if(csum == target){
		++l;
		while(l<r && a[l]==a[l-1]){ 			//move l till we have same numbers   5 5 5 5 ........R
			++l;
		}

		--r;
		while(r>l && a[r]==a[r+1]){ 			// move r till we have same numbers ...L ...... 5 5 5 5 5 5
			--r;
		}
	}
```




# 4 Sum  (common a 3 sum)
[https://leetcode.com/problems/4sum/submissions/]

```c++
    vector<vector<int>> fourSum(vector<int>& a, int target) {
		vector<vector<int>> ans;
		if(a.size() < 4)
			return ans;

		sort(a.begin(), a.end());

		int n = a.size();

		for(int i=0; i<n; ++i){
			if(i!=0 && a[i] == a[i-1]) 			// remove duplicates
				continue;

			for(int j=i+1; j<n; ++j){
				if(j!=i+1 && a[j] == a[j-1]) 		//remove duplicates
					continue;

				int l = j+1, r = n-1;
				while(l<r){
					long long csum = a[i]+ a[j]+ a[l]+ a[r];

					if(csum == target){
						ans.push_back(vector<int> {a[i], a[j], a[l], a[r]});

						++l;
						while(l<r && a[l]==a[l-1]){ 			//move l till we have same numbers   5 5 5 5 ........R
							++l;
						}

						--r;
						while(r>l && a[r]==a[r+1]){ 			// move r till we have same numbers ...L ...... 5 5 5 5 5 5
							--r;
						}
					}
					else if(csum < target)
						l++;
					else
						r--;
				}
			}
		}

		return ans;
    }
```



# Max consecutive 1's by changing m 0's
[https://www.geeksforgeeks.org/find-zeroes-to-be-flipped-so-that-number-of-consecutive-1s-is-maximized/]

same as below


- other approach O(n) O(1)space
	For all positions of 0’s calculate left[] and right[] which defines the number of consecutive 1’s to the left of i and right of i respectively. 
	store indexes of all zeroes in a third array say zeroes[]. 
	Now traverse zeroes[] and for all consecutive m entries in this array, compute the sum of 1s that can be produced. This step can be done in O(n) using left[] and right[]. 


- sliding window
	increase window size until we reach more than m zeroes 
	once we reach m zeroes decrease window size

	x x x x x x x x x x
	L				  
	R

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




### Subarray of max size with max k bad elements can be solved with 2 pointers
	x x x x x x x x x x
	L				  
	R

	increase R whenever we have less than k bad elements
	otherwise increase l to decrease max size

```c++
int maxofchar(string s, char c, int k){
	int n = sz(s);
	int ans = 0;

	int l=0, r=0;
	int b=0;
	while(l<=r && r<n){
		if(s[r]==c)
			++b;

		if(b<=k){
			ans = max(ans, r-l+1);
			++r;
		}
		else{
			//reach first char c bad character
			while(s[l]!=c){
				++l;
			}

			++l;
			++r;

			--b;
		}
	}

	return ans;
}
```



### Substring matching / max substring is done with 2 pointers


### count pairs with sum = k

	x x x x x x x x x x
	L				  R

move towards middle if you encounter multiple l and r just add them and choose 1 from total L and 1 from total R
if a[l] and a[r] are same then choose 2no. from total a[l]s


### count pairs with diff = k

	x x x x x x x x x
	L R
	
	if a[r]-a[l] is bigger than diff then we decrease gap by increasing l++
	if a[r]-a[l] is smaller we increase gap by adding bigger number i.e r++

	if k is +
		then do a[r] - a[l]
	if k is -
		then do a[l] - a[r] because -ve is different here we to increase diff we need to move left pointer


### whenerver we have 2 things to compare or 2 arrays think of 2 pointers
