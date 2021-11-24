### Prime factors  (sqrt(n))

Can be done in log(n) per query

```c++
	vector<pair<int, long long>> val;
	for (long long i = 2; i * i <= n; ++i) {
		int cnt = 0;
		while (n % i == 0) {
			++cnt;
			n /= i;
		}
		if (cnt > 0) {
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
                for(int j=i*i; j<=N; j+=i){          
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
