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




# Tower of Hanoi
	The minimal number of moves required to solve the Tower of Hanoi n disks is (2^n) − 1.

	

***
### Lexicographically printing subsets
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
