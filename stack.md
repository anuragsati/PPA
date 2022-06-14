### [VVVIMP] sum of sub min
[https://leetcode.com/problems/sum-of-subarray-minimums/]

contribution nikaalna hr element ka
ek element min kbtk hoga : in prev smaller element and next smaller element window mai
to remove duplicate : maintain striclty less condition on one side




### 132 pattern
[https://leetcode.com/problems/132-pattern/]


### Furthest/last greater element (FGE)  [VVVVVIMP]
[https://leetcode.com/submissions/detail/588051869/]


```c++
    //suff max will be monotonically decreaseing
    //5 5 5 4 4 2 2 1 
	int searchRight(int idx, int val, vector<int> &suff){
		int n = suff.size();
		int l = idx+1, r = n-1;
		int ans = -1;

		while(l<=r){
			int mid = l + (r-l) / 2;

			if(suff[mid] >= val){
				ans = mid;
				l = mid + 1;
			}
			else
				r = mid - 1;
		}

		return ans;
	}


    //pref max will be monotonically increasing
	int searchLeft(int idx, int val, vector<int> &pref){
		int n = pref.size();
		int l = 0, r = idx-1;
		int ans = -1;

		while(l<=r){
			int mid = l + (r-l) / 2;

			if(pref[mid] >= val){
				ans = mid;
				r = mid - 1;
			}
			else
				l = mid + 1;
		}

		return ans;
	}


    int main() {
        //create prefix max array
		vector<int> prefMax(n);
		for(int i=0; i<n; ++i){
			if(i==0) prefMax[i] = a[i];
			else prefMax[i] = max(prefMax[i-1], a[i]);
		}

        //create suffix max array
		vector<int> suffMax(n)
		for(int i=n-1; i>=0; --i){
			if(i==n-1) suffMax[i] = a[i];
			else suffMax[i] = max(suffMax[i+1], a[i]);
		}

        //at every point do a binary search on prefix and suffix max array to find FGE
        //because pref & suff max arrays are monotonic 
		for(int i=0; i<n; ++i){
			int lastRight = searchRight(i, a[i], suffMax);
			int firstLeft = searchLeft(i, a[i], prefMax);
		}
    }
```


### whenever we need to optimize better than O(n) then it is either using -1 or using encoding



### solve innermost then outer then outer is a stack property




### whenver we are inserting and deleting from back of array it is a stack




### greatest minimum on right (uses binary search) (nlogn)
we traverse from right to left and maintain a multiset(repeated elements)
for each index we encounter we search in the set the max value which is smaller than current


```c++
	//then find greatest minimum on right (element which is just smaller than current element towards right)
	vector<int> greatestMinRight(n);
	multiset<int> s;
	for(int i=n-1; i>=0; --i){
		if(i==n-1){
			greatestMinRight[i] = INT_MIN; 		//greatese min not found
		}
		else{
			auto p = s.upper_bound(a[i]-1);
			if(p != s.begin()){
				--p;
				greatestMinRight[i] = *p;
			}
			else
				greatestMinRight[i] = INT_MIN;
		}

		s.insert(a[i]);
	}
```

- ====== can also be done in O(n) =======
we try to find max element which is smaller than current element on right  ------->
alternatively 
for every right we try to find next greater element of left <--------

ex :
3 5 0 3 4
for 5 we are finding 4 but we can say for 4 we try to find next greater on left which is 5 (prev greater element)










***
### Next Greater Element (concept that can be extended to other ideas)
	store indexes not values

	maintain a stack in monotonic order [strictly decreasing in this case from bottom to top]
	whenever bigger element than stack.top() comes then start popping elements from stack and set their next
		greater to current element
	whenever smaller element comes don't do anything just push in stack bcz it can never be next greater of
		its prev elements
	
	in the end their might be some elemtns left in stack bcz they don't have next greater

```c++
	vector<int> nextLargerElement(vector<int> a, int n){
		stack<int> s;
		vector<int> ans(n);
		for(int i=0; i<n; ++i){
			while(!s.empty() && a[s.top()] < a[i]){
				ans[s.top()] = a[i];
				s.pop();
			}

			s.push(i);
		}

		while(!s.empty()){
			ans[s.top()] = -1;
			s.pop();		//dont forget this line
		}

		return ans;
    }
```






***
### Reverse a stack using recursion (n^2)
pop the top element then reverse the remaining stack then push removed ele. to the bottom


```c++
void reverse(stack<int> &s){
	if(s.empty())
		return;

	int temp = s.top();
	s.pop();

	reverse(s);
 
	push_bottom(s, temp); //O(N)
}
```



### Push_bottom in a stack (n)time (n)space
empty the stack and push the value then push the popped elements back

```c++
void push_bottom(stack<int> &s, int val){
	if(s.empty()){
		s.push(val);
		return;
	}

	int temp = s.top();
	s.pop();
	push_bottom(s, val);
	s.push(temp);
	return;
}
```