 without duplicates [special]
- permutation can also be done like this just remove set 
- can be used in subset as well but careful it uses extra space

```c++
	vector<vector<int>> ans;

	void solve(vector<int> &a, int idx){
		if(idx == a.size()-1){ //when we are at last index swap will occur with itself so just print it instead
			ans.push_back(a);
			return;
		}

        set<int> s;
        for(int i=idx; i<a.size(); ++i){ //try bringing all elements at this position
            if(s.count(a[i]))   //if we have already put this element at this place before we will not put it again
                continue;

            s.insert(a[i]);
            swap(a[idx], a[i]); //bring element at this
            solve(a, idx+1); //recurse for that element
            swap(a[idx], a[i]); //backtrack
        }
		return;
	}

    // solve(nums, 0);
};
```

# Subsets without duplicates [special]
[https://leetcode.com/problems/subsets-ii/submissions/]

- we only include element which occurs for first time or it is not equal to its previous
- jaise jaise subset bnata ja rha hai waise waise add krte ja [KEY_POINT]

- special type of recursion : for loop subset recursion
    isme hmesha jaise jaise bnte ja rhe hain waise waise add krte jao

```c++
    void solve(int idx, vector<int> &a){

        ans.push_back(temp);

        for(int i=idx; i<a.size(); ++i){
            if(i==idx || a[i] != a[i-1]){
                temp.push_back(a[i]);
                solve(i+1, a);
                temp.pop_back();
            }
        }

        return;
    }
```

# ways to add paranthesis to solve equation

fun(l, r) ==== return total answers after solving equation 
then whenever we encounter an operaotr we break string in 2 parts [l, i-1] [i+1, r] and solves 
this gives us 2 vectors and we return final vector after computing every possible ans




```c++
class Solution {
public:
	vector<int> solve(int l, int r, string &exp){
		// when only a number remains
		if(l == r){
			return vector<int> {exp[l]-'0'};
		}

		if(l == r-1){
			return vector<int> {(exp[l]-'0') * 10 + (exp[r]-'0')};
		}

		vector<int> ans;

		for(int i=l; i<=r; ++i){
			if(exp[i] == '*' || exp[i] == '+' || exp[i] == '-'){
				vector<int> ta = solve(l, i-1, exp);
				vector<int> tb = solve(i+1, r, exp);

				for(auto x : ta){
					for(auto y : tb){
						if(exp[i] == '*')
							ans.push_back(x * y);
						else if(exp[i] == '+')
							ans.push_back(x + y);
						else if(exp[i] == '-')
							ans.push_back(x - y);
						else if(exp[i] == '/' && y > 0) 
							ans.push_back(x / y);
					}
				}
			}
		}

		return ans;
	}
    vector<int> diffWaysToCompute(string expression) {
		int n = expression.size();
		return solve(0, n-1, expression);
    }
};
```

# to optimize recursive solution : just try to prune the tree by removing useless states




# moving throught matrix in recursion

```c++
	int newr, newc;
	if(c == n-1){
		newr = r+1;
		newc = 0;
	}
	else{
		newr = r;
		newc = c+1;
	}


	//for major diag.
	for(int i=0, j=(c-r); i<n && j<n; ++i, ++j){
		if(isNonValidCell(i, j, n))
			continue;
		
		if(board[i][j] == 'Q')
			return false;
	}

	//for minor diag.
	for(int i=0, j=(r+c); i<n && j>=0; ++i, --j){
		if(isNonValidCell(i, j, n))
			continue;
		
		if(board[i][j] == 'Q')
			return false;
	}
```




# to count all valid somethings
[https://leetcode.com/problems/count-numbers-with-unique-digits/]

```c++
	void solve(string s, int n, vector<bool> &vis){
		if(s.size() > n)
			return;

		if(s.size() > 0 && !isValid(s))
			return;

		++ans; 			// ------------------------just do ans++ for all valid ans. stop at invalid instead of doing it for each size of letters
		for(int i=0; i<=9; ++i){
			if(!vis[i]){
				vis[i] = true;
				solve(s + char('0' + i), n, vis);
				vis[i] = false;
			}
		}
		return;
	}
```



# another way of recursion (VVV imp)
[https://leetcode.com/problems/decode-string/]





***
### Lexicographically printing subsets
-    phle mai khud ko print kr leta hu fir baad mai mere baad wale aayenge

first sort the array

vector<int> subset;
void lexicalSubset(vector<int> &a, int idx) {
	for(auto x:subset)
		cout << x << " ";
	cout << " --" << endl;

	if(idx >= (int)a.size())
		return;
	

	for(int i=idx; i<(int)a.size(); ++i){
		subset.push_back(a[i]);
		lexicalSubset(a, i+1);
		subset.pop_back();
	}

	return;
}


# Tower of hanoi 	(min moves = 2^(n-1))

```c++
	void hanoi(int N, int s, int d, int h){
		if(N==1){
			cout << "from " << s << " to " << d << endl;
			return;
		}

		hanoi(N-1, n, s, h, d); 	//n-1 disks from s->h 
		cout << "from " << s << " to " << d << endl;
		hanoi(N-1, n, h, d, s); 	//n-1 disks from h->d 
		return;
	}
    vector<int> shiftPile(int N){
		hanoi(N, 1, 3, 2);
    }
``
