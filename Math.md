### ncr for smaller numbers without using factorial
- other way using pascal's triangle

way to find nCr withoud creating pascal triangle:
nCr = (n * n-1 * n-2 .... n-r+1) (n-r)! / r! * (n-r)!
nCr = [n * n-1 * n-2 .... n-r+1] / r!

[n * n-1 .... n-r+1] are r numbers bcz n - (n-r+1) + 1 = r
and r! are also r numbers 
so we just run a loop from 1 to r and calculate this [remember to divide after bcz of floating issues]

first multiply then divide

- T.C : O(r)

```c++
    int C(int N, int r) {
        r = max(r, N-r);    //for better complexity
        int ans = 1;
        for(int i=1; i<=r; ++i)
            ans = ans * (N-r+i) / i;
        return ans;
    }
```



### how to calculate gcd of array except self ? 
[https://www.hackerrank.com/challenges/the-chosen-one/problem]
    just calculate prefix gcd and suffix gcd






### most questions of number theory can be solved with sieve







### Smaller spf method
- if spf == -1 means it is not set then set spf of all multiples of i
    if spf[j] == -1 means it has not set before then set it otherwise it already has spf

```c++
    const int mxA = 1000010;
    vector<int> spf(mxA, -1);

    void calculate_spf(){
        for(int i=2; i*i<=mxA; ++i){
            if(spf[i] == -1){
                for(int j=i; j<mxA; j+=i){
                    if(spf[j] == -1)
                        spf[j] = i;
                }
            }
        }
    }

    //get prime factors of n
    unordered_map<int, int> m;
    while(spf[n] != -1){
        m[spf[n]]++;
        n /= spf[n];
    }
    if(n > 1) m[n]++;
```




### binary search is better than Ternary search
    bcz of constants
    in b.s we do only 1 comparison
    but in t.s we do 2 comparison that creates a difference

    
### sum of deviations is minimum from median




### Prime factors  (sqrt(n))

Can be done in log(n) per query
using spf

```c++
    //power, prime factor
	vector<pair<int, long long>> val;
	for (long long i = 2; i * i <= n; ++i) {
        if(n%i==0){
            int cnt = 0;
            //suck all primes
            while (n % i == 0) {
                ++cnt;
                n /= i;
            }

            val.push_back({cnt, i});
        }
	}
	if (n > 1) {
		val.push_back({1, n});
	}
```

### Prime factors (log(n) per query)

```c++
	// precompute spf array using sieve
	const int N = 2000000;
    vector<bool> mark(N, true);
    vector<int> spf(N, -1);

    void sieveSPF(){
        mark[0]= mark[1]=false;

        for(int i=2; i*i<=N; ++i){             
            if(mark[i]){
                for(int j=i; j<=N; j+=i){          
					//if it is already marked then don't update its spf bcz its spf is already set

					//if it is not marked before
					if(mark[j]){ 
						mark[j] = false;
						spf[j] = i;
					}
                }
            }
        }
    }


	...


	//per query divide the no. repetitively by its spf
	while(spf[n] != -1){ //-1 means the no. is prime and we can't divide it further
		cout << spf[n] << " " ;
		n /= spf[n];
	}

	if(n!=1) //if something is remaining then print that bcz it will be last prime no.
		cout << n << " ";


```

### segmented sieve

```c++
    void segSieve(ll a, ll b){
        
        ll lim = sqrt(b);
        vector<bool> mark(lim+1, true);         //create mark array for normal sieve
        vector<ll> primes;
        for(ll i=2; i<=lim; ++i){
            if(mark[i]){
                primes.emplace_back(i);
                for(ll j=i*i; j<=lim; j+=i){
                    mark[j] = false;
                }
            }
        }

        vector<bool> isprime(b-a+1, true);      //create bool for seg sieve
            
        for(auto i:primes){
			ll firstMultiple = ceil(a*1.0 / i);   //get the first multiple of prime i closest to a

			for(int j = max(i*i, firstMultiple*i); j<=b; j+=i){ 		//max(...) for optimization only
				isprime[j-a] = false;
			}


			//ll base = (a/i) * i;
			// if(base < i)
			// 	base += i;
        }

        if(a == 1)                  //when a == 1;
            isprime[0] = false;

        for(ll i=a; i<=b; ++i){         //print seg. sieve
            if(isprime[i-a])
                cout << i << endl;
        }

    }
```
