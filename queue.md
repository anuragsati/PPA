### Gas station problem
[https://leetcode.com/problems/gas-station/]




### we use the concept of deque in sliding window to keep track of max and min in a window


### 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit



### First negative integer in every window of size k  (https://practice.geeksforgeeks.org/problems/first-negative-integer-in-every-window-of-size-k3345/1#)

	if value is negative push into queue
	if any negative window goes out of window do q.pop()
	for any subarray ans = q.front()


```c++
	queue<ll> q;
	vector<ll> ans;

	//process first window
	for(int i=0; i<K; ++i){
		if(a[i] < 0)
			q.push(a[i]);
	}

	if(q.empty())
		ans.push_back(0);
	else
		ans.push_back(q.front());

	for(int i=K; i<N; ++i){
		ll re = a[i-K];
		if(!q.empty() && re == q.front())
			q.pop();
		
		if(a[i] < 0)
			q.push(a[i]);
		
		//write ans for this subarray
		if(q.empty())
			ans.push_back(0);
		else
			ans.push_back(q.front());
	}

	return ans;

```





### Sliding window maximum (https://leetcode.com/problems/sliding-window-maximum/)

```c++

class Solution {
	deque<int> dq;

	void dqpush(int data){
		while(!dq.empty() && dq.back() < data){
			dq.pop_back();
		}
		dq.push_back(data);
	}

    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
		int n = nums.size();
		vector<int> ans;

		//first process window of size k
		for(int i=0; i<k; ++i){
			dqpush(nums[i]);
		}

		ans.push_back(dq.front());

		//process all subarray of windowsize
		for(int i=k; i<n; ++i){
			//add new element to window
			dqpush(nums[i]);

			//remove last window element if it exits in dq
			if(dq.front()==nums[i-k])
				dq.pop_front();
			
			//add the front of queue to ans
			ans.push_back(dq.front());
		}

		return ans;
    }
};

```


### Slinding window can be used with queue/deque data structure


### Generate all binary numbers from 1 to N in sorted order
use BFS
		1
	10		11
100	  101  110  111

basically at each node you either add 0 or add 1

vector<string> generate(int N){
	vector<string> ans;

	queue<string> q;
	q.push("1");
	while(!q.empty() && ans.size() < N){
		string curr = q.front();
		ans.push_back(curr);
		q.pop();

		q.push(curr+"0");
		q.push(curr+"1");
	}

	return ans;
}

### queue can be used in cases where you generate numbers in sorted order or where you need to find numbers which contain some numbers from 1 to N



### Reverse a queue using recursion

```c++
	void reverse(queue){
		if(q.empty())
			return;
		
		int temp = q.front();
		q.pop();

		reverse(q);

		q.push(temp);
	}
```