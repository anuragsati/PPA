***
### Number of elements towards right which are greater than current element (extension of NGE)

```c++
	stack<int> s;
	vector<int> ng(n);
	for(int i=n-1; i>=0; --i){
		while(!s.empty() && a[s.top()] < a[i])
			s.pop();

		ng[i] = s.size();
		s.push(i);
	}
```


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
			s.pop();
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