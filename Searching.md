### Binary search is used in places where we need to find smallest / largest in a range





### Allocate minimum number of pages 
[https://practice.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1]

binary search on answer if we can allocate x books then total number of segments we can form must be <= m
if for any x we are forming > m segments that is bad

F F F F F T T T T [find first true]

```c++
    bool isValid(int a[], int n, int m, int sum) {
		ll cur = 0;
		ll seg = 1;
		for(int i=0; i<n; ++i){
			cur += a[i];
			if(cur <= sum)
				continue;

			cur = a[i];
			++seg;

			if(cur > sum)
				return false;
		}

		if(seg > m)
			return false;
		
		return true;
	}

    int findPages(int a[], int n, int m) {
		if(n < m)
			return -1;

		ll l = 0, r = 1e12;

		ll ans = 0;
		while(l<=r){
			ll mid = l + (r-l)/2;

			if(isValid(a, n, m, mid)){
				ans = mid;
				r = mid - 1;
			}
			else
				l = mid + 1;
		}

		return ans;
    }
```




### Kth smallest in an unsorted array : 
can apply same idea on matrix as well in n^2 log (mx - mn)

- 1 : O(n log(max - min))

	smol returns no. of smaller or equal elements so kth smallest element must have >= k smaller or equal elements
	we use binary search for this

	_ _ _ _ 4 4 4 _ _
	T T T T F F F F F 

	we are trying to find first F
	if smol of mid < k then all numbers less than mid will be bad so l = mid + 1
	else this could be possible ans search left
	we are guaranteed that final ans will lie in array



	if x is kth smallest element then it is the first element with no. of smaller or equal ele. >= k
	so don't be afraid that it will not be optimal by the end of BS
	if x is kth smallest x-1 will never be an answer



```c++
	int smol(vector<int> &a, int k){
		int cnt = 0;
		for(int i=0; i<(int)a.size(); ++i)
			if(a[i] <= k)
				++cnt;
		
		return cnt;
	}


	main(){
		int l = mn, r = mx;
		int ans = 0;
		while(l <= r){
			int mid = l + (r-l)/2;

			int numSmaller = smol(a, mid);

			if(numSmaller < k){
				l = mid + 1;
			}
			else{
				ans = mid;
				r = mid -1;
			}
		}

		cout << ans << endl;
	}

```


- 2 : O(n^2) approach 

	we start from first element till last element and for each element we find how many elements are smaller than or equal this are there
	for kth smalles element no. of element smaller or equal than this must be >= k

	there can be duplicates as well so for kth smallest element 
	it must have >= k smaller or equal element and smaller elements must be < k 
	smaller element < k because there might be some elements (ex largest ele.) which have more than >=k and everything is smaller


```c++
	bool isvalid(a, k){
		int cnt = 0;
		for(auto x:a)
			if(x < k) smaller++;
			else if(x==k) equal++;
		

		total = smaller + equal;
		if(total >= k && smaller < k)
			return true;
	}


	...
	check element one by one
```









### Aggressive cows
[https://www.spoj.com/problems/AGGRCOW/]

Binary search on answer

we want to find how many cows we can put in stalls such that min distance is maximised
let min distance be d

so we binary search on d
if we find a d such that we can place c cows with d distance then surely we can also put c cows with less than or equal to d distance 
l = mid + 1;

if we can't put cows with d dist then we can't put cows with d+1 ... dist. too
r = mid -1;





### Binary search can be used in a monotonic array where we need to find first/last something



### MOST of the time binary search applied to n^2 algos to reduce it to nlogn

	instead of searching for all possible values we search using binary search







### Lecture 13 - Maximize K (nlogn)
- Q. given array (unsorted) it contain +ve elements only and X >= 0
	find max. possible k such that all subarrays of size k have sum <= x

ans : use binary search on answer

	L = 0 , R = n-1
	if mid exists i.e if a subarray of size mid exits such that all subarray of size mid have sum <= x
	then surely [0...mid] will also exit

	and if mid is false i.e k = mid atleast one subarray have sum > x
	then all subarrays of size > mid will be false

	0 1 2 3 4 5 6 7 8 9
	T T T T T F F F F F

	we can use binary search on ans. and check if mid is possible or not
		if possible we store it as ans and do l = mid+1;
		else we do r = mid-1;







### Kth element in 2 sorted arrays ***IMP***
[https://www.youtube.com/watch?v=nv7F4PiLUzo]

	we will apply binary search in first array (smaller one)
	l = max(0, k-m) either it is 0 or k-m if we have to pick atleast some elements from 1st array
	r = min(k, n) either pick whole array or k elements

	...l1 | r1 ...
	...l2 | r2 ...
	(left ke saare elelemts right ke saare elements se smaller hone chahiye)
	(we are maintaining size = k)

	if(l1 > r2)
		then l1 is higher so reduce search space to left r = mid1 - 1
	else
		increase space l = mid1 + 1;

- T.C : O(log(min(n, m))) if we apply bs on smaller array
- S.C : O(1)

```c++
    int kthElement(int a[], int b[], int n, int m, int k){
		int l = max(0, k-m), r = min(k, n);
		while(l<=r){
			int mid1 = l + (r-l)/2;
			int mid2 = k - mid1;

			int l1 = mid1 == 0 ? INT_MIN : a[mid1-1];
			int l2 = mid2 == 0 ? INT_MIN : b[mid2-1];

			int r1 = mid1 == n ? INT_MAX : a[mid1];
			int r2 = mid2 == m ? INT_MAX : b[mid2];

			if(l1 <= r2 && l2 <= r1){
				return max(l1, l2);
			}
			else if(l1 > l2){
				r = mid1 - 1;
			}
			else{
				l = mid1 + 1;
			}
		}

		return 1;
    }
```



### sqrt(N) in log(n)
```c++
    int mySqrt(int x) {
		int L = 0, R = x;
		int ans = 0;
		while(L <= R){
			long long mid = L + (R-L) / 2;

			if(mid * mid <= x){
				ans = mid;
				L = mid + 1;
			}
			else{
				R = mid -1;
			}
		}

		return ans;
	}
```


	
### Binary seach is applied in mountain array kind of problems along with stack 



### Handle corner cases in binary search like this

	while(...){

		if(mid == 0){
			...
		}
		if(mid == n-1){
			...
		}


		//now no corner cases will be there
	}




### find one peak element in array (https://leetcode.com/problems/find-peak-element/submissions/)
	use binary search and move to higher altitudes
	because it is guaranteed that at higher altitudes atleast one peak exits




### Find largest element in rotated sorted array
compare with last element
when you find possilbe ans store it in ans
- very similar to smalles in rsa 
- compare with a[0] bcz array could be sorted too

```c++
	int l=0, r=n-1, pivot = -1;
	while(l<=r){
		int mid = l+(r-l)/2;

		if(a[mid] > a[0]){
			l = mid+1;
		}
		else{
			pivot = mid;
			r = mid-1;
		}
	}
```



### Find smallest element in rotated sorted array
- compare with last element a[n-1]
when you find possilbe ans store it in ans

```c++
	int l=0, r=n-1, pivot = -1;
	while(l<=r){
		int mid = l+(r-l)/2;

		if(a[mid] > a[n-1]){
			l = mid+1;
		}
		else{
			pivot = mid;
			r = mid-1;
		}
	}
```






### we can also solve b.s problems with storing possilbe ans in a varialbe instead of returning L, R
	Its much better approach




### Frequency of a number is sorted array
find first occurance  
find last occurance
then ans = last occurance - first occurance + 1




### Last occurance in sorted array
- check if a[mid] > k then simply search in left half
- check if a[mid] < k then simply search in right half
- check if a[mid] == k
	-	check if a[mid] != a[mid+1] if they are not equal then mid is the ans (careful of m+1 so if m==n-1 the ans. is always true)
	-	if a[mid] == a[mid+1] then search in right space because we are finding last occurance

```c++
l = 0, h = n-1;

while(l <= h){
	mid = l + (r-l) / 2;

	if(a[mid] < k)
		l = mid+1;
	else if(a[mid] > k)
		h = mid-1;
	else{
		if(m==n-1) 
			return mid;

		if(a[mid+1] != a[mid])
			return mid;
		else
			l = mid+1;
	}
}
```




### First occurance in sorted array
can solve with possible answer approach
given element exits in array

- check if a[mid] > k then simply search in left half
- check if a[mid] < k then simply search in right half
- check if a[mid] == k
	-	check if a[mid] != a[mid-1] if they are not equal then mid is the ans (careful of m-1 so if m==0 the ans. is always true)
	-	if a[mid] == a[mid-1] then search in left space because we are finding first occurance

```c++
l = 0, h = n-1;

while(l <= h){
	mid = l + (r-l) / 2;

	if(a[mid] < k)
		l = mid+1;
	else if(a[mid] > k)
		h = mid-1;
	else{
		//now we know a[mid] == k now check if a[mid - 1] is not equal to a[mid]
		if(m==0) 
			return mid;

		if(a[mid-1] != a[mid])
			return mid;
		else
			h = mid-1;
	}
}
```





# First and last occurance (easy way)
```c++
  int first_occurance(int x, vector<int> &a){
      int l=0, r=a.size()-1;
      int ans = -1;
      while(l<=r){
          int mid = l + (r-l) / 2;

          if(a[mid] >= x){
              ans = mid;
              r = mid - 1;
          }
          else
              l = mid + 1;
      }

      if(ans != -1 && x == a[ans])
          return ans;
      else
          return -1;
  }

  int last_occurance(int x, vector<int> &a){
      int l=0, r=a.size()-1;
      int ans = -1;
      while(l<=r){
          int mid = l + (r-l) / 2;

          if(a[mid] <= x){
              ans = mid;
              l = mid + 1;
          }
          else
              r = mid - 1;
      }

      if(ans != -1 && x == a[ans])
          return ans;
      else
          return -1;
  }
```

### Upper bound (STL)
if a[mid] <= x then this could not be answer because we want elements strictly greater than x so l = mid+1
if a[mid] > x then it could be possible ans so h = mid

```c++
int upper_bound(int a[], int n, int x) {
    int l = 0;
    int h = n; // Not n - 1
    while (l < h) {
        int mid =  l + (h - l) / 2;
        if (a[mid] <= x) {
            l = mid + 1;
        } else {
            h = mid;
        }
    }
    return l;
}
```

### Lower bound (STL)
if a[mid] >= x then it could be possible answer h = mid
if a[mid] < x it can never be possible answer so search in right half l = mid+1

```c++
int lower_bound(int a[], int n, int x) {
    int l = 0;
    int h = n; // Not n - 1
    while (l < h) {
        int mid =  l + (h - l) / 2;
        if (a[mid] >= x) { //this could be possilbe answer
            h = mid;
        } else {  //if a[mid] < x strictly smaller
            l = mid + 1;
        }
    }
    return l;
}
```