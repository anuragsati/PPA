### Greedy denominations for min coin change

greedy works in few cases for others we will have to use dp
- 1 2 5 10 50 100 (euro coins)
- 1 k k^2 k^3 .... (powers of k)
    
in these type of denominations we can select min coins by picking max first any number of times
bcz if we dont pick max at current we will not be able to make this change

ex : change = 82
    coins = 1 9 81 789
    if we dont pick 81 then we will not be able to make change as 1...9 does not add up to 81







### [https://leetcode.com/problems/equal-sum-arrays-with-minimum-number-of-operations/]




### Finding total max/min peaks in array [IMPORTANT]
[https://leetcode.com/problems/wiggle-subsequence/]

- if prevdiff is + and currdiff is - then this is possible answer
- if prevdiff is - and currdiff is + then this is possible answer
    we update prevdiff to currentdiff in these cases
    else we move forward

- initially if array is like / or \ then ans is 2 else if it is equal then ans is 1

basically we are skipping middle elements which are not peaks

```c++
    int wiggleMaxLength(vector<int>& a) {
        int n = a.size();
        if(n < 2)
            return n;

        int prevdiff = (a[1]-a[0]);
        int ans = (prevdiff != 0) ? 2 : 1;

        for(int i=2; i<n; ++i){
            int diff = a[i]-a[i-1];

            if((diff>0 && prevdiff<=0) || (diff<0 && prevdiff>=0)){
                ++ans;
                prevdiff = diff;
            }
        }

        return ans;
    }
```
### mostly in stream/running questions max/min heap is the idea



### Huffman encoding
	we first creater priority queue of nodes and each time we get least two size elements with frequencies and 
	we make then sibblings
	we do this till pq.size == 1

	we are doing this so that min. freq. elements get longer byte codes and max. freq. elements get smaller bytecoeds

- comparator that creates min heap based on frequencies i.e if freq of parent is more then swap i.e bring it down and put less freq. element at top
- Node is a class that creates node of codings where only child nodes have char set other have char '-'
- Print code prints tree in preorder

```c++
	struct Node{
		char c;
		int freq;
		struct Node* left;
		struct Node* right;

		Node(char ch, int f, struct Node* l, struct Node* r){
			c = ch;
			freq = f;
			left = l;
			right = r;
		}
	};
	
	struct comp{
    	bool operator() (Node* a, Node* b){
    		return a->freq > b->freq;
    	}
	};
	

	public:

	void printCodes(string code, Node* root, vector<string> &ans){
		if(!root)
			return;

		if(!root->left && !root->right){
			ans.push_back(code);
			return;
		}

		printCodes(code+'0', root->left, ans);
		printCodes(code+'1', root->right, ans);

		return;
	}

	vector<string> huffmanCodes(string s,vector<int> f,int n) {

		priority_queue<Node*, vector<Node*>, comp> pq;

		for(int i=0; i<n; ++i){
			pq.push(new Node(s[i], f[i], NULL, NULL));
		}

		while(pq.size() > 1){
			//get two min freq elements
			Node* n1 = pq.top(); pq.pop();
			Node* n2 = pq.top(); pq.pop();

			Node* n3 = new Node('-', n1->freq + n2->freq, n1, n2);
			pq.push(n3);
		}

		vector<string> ans;
		printCodes("", pq.top(), ans);
		return ans;
	}
```


### Min platforms Greedy
	this problem has tons of different ways to solve

	- n^2 brute force (count overlapping intervals)

	- difference array (+1 / -1) (map arrival / departure on number line)

	- diff. array using map (use map instead of number line intellingent diff. array)

	- sort and two pointers 

	- sort and merge process (based on diff array using map)

	- activity selection using set (not intuitive) (we maintain a set and when new train comes we find how many elemts are greater thatn curent arrival time and then we push cur dep. time)
	(basically for each starting point we find how many ending points greater than that are there and we store ending point after processing a train)


- using multiset (activity selection)
apne se piche chek kr rha hu ki kitne overlap hore hain mujhse
dont even need multiset can be done with simple array too

```c++
bool comp(pair<int, int> a, pair<int, int> b){
	if(a.second == b.second)
		return a.first < b.first;
	return a.second < b.second;
}

int main(){
    ios_base::sync_with_stdio(false);cin.tie(0);
	int n;
	cin >> n;
	vector<pair<int, int> > a(n);
	for(int i=0; i<n; ++i)
		cin >> a[i].first >> a[i].second;
	
	sort(a.begin(), a.end(), comp);

	multiset<int> s;
	int ans = 1;
	s.insert(a[0].second);

	for(int i=1; i<n; ++i){
		int cur = a[i].first;
		auto p = s.upper_bound(cur); 
		int dist = distance(p, s.end()) + 1;

		ans = max(ans, dist);
		s.insert(a[i].second);
	}

	cout << ans;
    return 0;
}
```



- using 2 pointers

```c++
int main(){
    ios_base::sync_with_stdio(false);cin.tie(0);
	int n;
	cin >> n;
	vector<pair<int, int> > a(n);
	for(int i=0; i<n; ++i)
		cin >> a[i].first >> a[i].second;
	
	sort(a.begin(), a.end(), comp);

	int ans = 1;
	int l = 0, r = 0;

	while(r < n){
		int le = a[l].second;
		int rs = a[r].first;

		if(rs < le){ //overlap
			ans = max(ans, r-l+1);
			++r;
		}
		else
			++l;
	}

	cout << ans;
    return 0;
}
```





### imp trick in greedy keep one const and see how it affects other (ex: fractional knapsack)
- if it depends on two things i can come up with some relations




### Fractional knapsack & variations
	we can't compare quantities with different price and weight
	but we can compare how much profit we are making per unit
	ideally we want max profit so we pick one by one unit and we pick that unit which is giving max profit

	take that item which has max profit / weight ration i.e take that item which has max profit per unit

```c++
    public:
	bool comp(Item a, Item b){
		return a.value * b.weight > b.value * a.weight;
	}

    double fractionalKnapsack(int W, Item a[], int n) {
		sort(a, a+n, comp);

		double ans = 0.0;
		for(int i=0; i<n; ++i){
			if(W-a[i].weight >= 0){
				ans += a[i].value;
				W -= a[i].weight;
			}
			else{
				ans += W * (a[i].value * 1.0 / a[i].weight);
				break;
			}
		}

		return ans;
    }
```


- Variation : Job Sequencing Problem – Loss Minimization (variation of fractional knapsack)
[https://www.geeksforgeeks.org/job-sequencing-problem-loss-minimization/]
	
		we want to minimize loss and we have d days to do a job

		so 
			1. if days are same we want to do the job which has max loss first to minimize our loss
			2. if loss is same then we want to do the job which has smaller days to do so that we can do more jobs fast

			so ans proportional to loss / day

			so ans is loss/day i.e we do the job whose loss/day (loss per day) is more
			if we do that job we can minimize our loss

		just like fractional knapsack






### Job scheduling 
	we sort jobs by price in descending order i.e greatest price first
	then we try to do a job as late as possible
	we do this so that all jobs below this deadline can be accomplished and we try to choose best possible profit for current day

	for a deadline d we can do that job from [1....d] days
	so to do late we start from d days and move till 1


- O(n logn) approach use multiset/set instead of vis just like in activity selection

	create a set which contains the days which have not been filled
	at each index if we find some day and we can occupy it then we occupy it and erase that day
	if we can't occupy a day i.e p == s.begin() then we skip it

	to print job sequence you can create a vector vis(n+1, -1)  and mark every job you do in there with 1 and don't print -1

```c++
	bool comp(Job a, Job b){...}

    vector<int> JobScheduling(Job a[], int n) { 
		sort(a, a+n, comp);

		int ans = 0, tot_profit = 0;

		vector<int> vis(n+1, -1);
		set<int> s;

		for(int i=1; i<=n; ++i) //initially we have all days available
			s.insert(i);

		for(int i=0; i<n; ++i){
			auto p = s.upper_bound(a[i].dead); //search for the greatest day which we can occupy  i.e day <= a[i].dead

			if(p != s.begin()){ //if we find such day remove it from the set bcz it is filled by us and increase profit
				--p;

				s.erase(p);
				ans++;
				tot_profit += a[i].profit;
				vis[a[i].id] = *p; //mark what id was done on what day
			}
		}

		return vector<int> {ans, tot_profit};
    } 
```




- O(n^2) version
	to print sequence of jobs print using vis array just don't print -1s
	[https://www.youtube.com/watch?v=LjPx4wQaRIs&t=169s]

```c++
	bool comp(Job a, Job b){
		if(a.profit != b.profit)
			return a.profit > b.profit;
		else if(a.dead != b.dead)
			return a.dead < b.dead;
		else
			return a.id < b.id;
	}

    vector<int> JobScheduling(Job a[], int n) { 
		sort(a, a+n, comp);

		vector<int> vis(n+1, -1);

		int ans = 0, tot_profit = 0;

		for(int i=0; i<n; ++i){
			for(int j=a[i].dead; j>0; --j){ //start from cur jobs deadline and find the greatest deadline that is not filled
				if(vis[j] == -1){
					vis[j] = a[i].id;
					tot_profit += a[i].profit;
					++ans;
					break;
				}
			}
		}

		return vector<int> {ans, tot_profit};
    } 
```







### Activity selection (k persons) [IMP] (nlogk)
[https://www.geeksforgeeks.org/activity-selection-problem-with-k-persons/]

at every activity we find how many people we can assign this activity to 
	1. some people :
		assign to that person which has maximum end time

	2. no one : 
		1. create new person and assign him this activity if persons < k
		2. if persons are already k then simply skip this activity bcz it can't be assigned to anyone


[imp] first check if we can assign this activity to existing persons if not then only create new person (reason in 2 person problem example)
	first try to assign activity to person which are already doing job then only create new persons



```c++
bool comp(pair<int, int> a, pair<int, int> b){
	if(a.second == b.second)
		return a.first < b.first;
	return a.second < b.second;
}

int main(){
    ios_base::sync_with_stdio(false);cin.tie(0);
	int n, k;
	cin >> n >> k;
	vector<pair<int, int> > a;
	for(int i=0; i<n; ++i){
		int s, e;
		cin >> s >> e;
		a.push_back({s, e});
	}

	sort(a.begin(), a.end(), comp);
	
	int ans = 0;
	multiset<int> s; //multiset because same intervals can be there

	for(int i=0; i<n; ++i){

		//check if we can assign it to someone who is already busy
		if(!s.empty()){
			auto p = s.upper_bound(a[i].first); //find largest ending time that is < cur start time

			if(p != s.begin()){ //if found someone then remove it and add curr ending time
				--p;

				s.erase(p);
				s.insert(a[i].second);
				++ans;
				continue;
			}
		}

		// if we can't assign to someone busy check if we can add another person with this job
		if(s.size() < k){
			s.insert(a[i].second);
			++ans;
		}
	}

	cout << ans << endl;
    return 0;
}
```






### Activity selection (2 persons)

[imp]
	--------------
------ 		   -------------------
		----		 -------



consider above example you cannot say we will run normal activity selection with first person and then run with 2nd person again bcz 
that way we will do 4 jobs but it is possible to do 5 jobs


the correct idea is whenever at one point we arrive and we can assign an activity to both people
then we will assign activity to that person which has greater ending time because that way we make sure to have more jobs for lesser ending person

ex : at 4th (s.t 15) activity we can assign it to first person (e.t 9) as well as second person(e.t 12)
but we will assign it to second person because it has greater e.t so that if there exits some activity whose s.t is greater than et of first and smaller than e.t of second we can assign 
it to first person

but if we assign 4th activity to first person we will miss out all activities that are between first and second person ex 5th activity


initially both start = 0
we will assign activity to 2nd person only if we can't assign to first person


```c++
bool comp(pair<int, int> a, pair<int, int> b){
	if(a.second == b.second)
		return a.first < b.first;
	return a.second < b.second;
}

int main(){
    ios_base::sync_with_stdio(false);cin.tie(0);
	int n, k;
	cin >> n >> k;
	vector<pair<int, int> > a;
	for(int i=0; i<n; ++i){
		int s, e;
		cin >> s >> e;
		a.push_back({s, e});
	}

	sort(a.begin(), a.end(), comp);
	
	int ans = 1;
	int p1 = 0, p2 = -1;

	for(int i=1; i<n; ++i){
		int cur_start = a[i].first;
		int p1end = a[p1].second;
		int p2end = p2==-1 ? -1 : a[p2].second;

		if(cur_start >= p1end && cur_start >= p2end){ //can append in both
			++ans;

			if(p1end >= p2end)
				p1 = i;
			else
				p2 = i;
		}
		else if(cur_start >= p1end){
			++ans;
			p1 = i;
		}
		else if(cur_start >= p2end){
			++ans;
			p2 = i;
		}
	}

	cout << ans << endl;
    return 0;
}

```




***
### Job / Activity selection (greedy)
- idea : pick activity with earliest end time will leave us with more time to do other activities (jhn se mai jldi choot jaunga)

- first sort according to second element(earliest ending job)
- first job is always selected (first job with earliest ending time) (because if we pick first then ans will always be optimum while picking 2, 3... wil not necessarily give optimal soln)
	(solution starting with first job has to exist i.e if 2 5 7 exitst then 1 5 7 also exits bcz 1 < 2)
- move from job 1 to n and if start time of that job is greater than our ending time of prev job then take that job

- brute force : consider all n combination and permute those activites 2^n * n!
- start time does not matter only end time matters

```c++
	bool cmp(pair<ll, ll> &a, pair<ll, ll> &b){
		if(a.second == b.second) return a.first < b.first;
		return a.second < b.second;
	}


	sort(a.begin(), a.end(), cmp);
	int ans = 1, curr_job=0;
	for(int j=1; j<n; ++j){
		if(a[j].first >= a[curr_job].second){
			++ans;
			curr_job = j;
		}
	}

	cout << ans; //total jobs we can perform
```
