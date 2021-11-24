==========================================
# radix sort






==========================================
# count sort
 
-	works best if range of data is very less
	T.C : max(max-min, n)
	S.C : O(max-min)

-	make a freq. array and plot every element of orig. array in freq. array
	size = [max - min + 1]
	and we shift every element by min bcz array can have -ve elements
	so new idx =  freq[a[i] - min]

-	cons : if range of data is huge then it is bad


```c++
    void countSort(int a[], int n) {
		int mx = *max_element(a, a+n);
		int mn = *min_element(a, a+n);

		vector<int> freq(mx-mn+1);

		//create freq. array
		for(int i=0; i<n; ++i){
			freq[a[i]-mn]++;
		}

		//create cumulative freq. array
		for(int i=1; i<freq.size(); ++i){
			freq[i] += freq[i-1];
		}

		//iterate from back of array a and decrease cumulative freq. everytime to ensure stability
		vector<int> ans(n);
		for(int i=n-1; i>=0; --i){
			ans[freq[a[i]-mn] - 1] = a[i];
			freq[a[i]-mn]--;
		}

		//reassign to a
		for(int i=0; i<n; ++i)
			a[i] = ans[i];
    }
```


==========================================
# count smaller/bigger elements to right/left is count of inversions (merge sort)





# we use sorting whenever we want to find how many elements are smaller/greater than an element for all elements



# whenever we have partion / divide array in 2-3 parts use partition algo from quick sort


# Partition array in 3 parts
	first put first half in first place
	then problem reduces to partion in two halves

```c++
   void sort012(int a[], int n) {
		int l = 0;

		//put first part first
		for(int i=0; i<n; ++i){
			if(a[i]==0){
				swap(a[i], a[l]);
				++l;
			}
		}

		//partition remaining 2 parts
		for(int i=l; i<n; ++i){
			if(a[i]==1){
				swap(a[i], a[l]);
				++l;
			}
		}
    }
```




# IMP diff b/w quick and merge sort
- Why Quick Sort is preferred over MergeSort for sorting Arrays 
Quick Sort in its general form is an in-place sort (i.e. it doesn’t require any extra storage) whereas merge sort requires O(N) extra storage, N denoting the array size which may be quite expensive. Allocating and de-allocating the extra space used for merge sort increases the running time of the algorithm. Comparing average complexity we find that both type of sorts have O(NlogN) average complexity but the constants differ. For arrays, merge sort loses due to the use of extra O(N) storage space.
Most practical implementations of Quick Sort use randomized version. The randomized version has expected time complexity of O(nLogn). The worst case is possible in randomized version also, but worst case doesn’t occur for a particular pattern (like sorted array) and randomized Quick Sort works well in practice.
Quick Sort is also a cache friendly sorting algorithm as it has good locality of reference when used for arrays.
Quick Sort is also tail recursive, therefore tail call optimizations is done.

- Why MergeSort is preferred over QuickSort for Linked Lists? 
In case of linked lists the case is different mainly due to difference in memory allocation of arrays and linked lists. Unlike arrays, linked list nodes may not be adjacent in memory. Unlike array, in linked list, we can insert items in the middle in O(1) extra space and O(1) time. Therefore merge operation of merge sort can be implemented without extra space for linked lists.
In arrays, we can do random access as elements are continuous in memory. Let us say we have an integer (4-byte) array A and let the address of A[0] be x then to access A[i], we can directly access the memory at (x + i*4). Unlike arrays, we can not do random access in linked list. Quick Sort requires a lot of this kind of access. In linked list to access i’th index, we have to travel each and every node from the head to i’th node as we don’t have continuous block of memory. Therefore, the overhead increases for quick sort. Merge sort accesses data sequentially and the need of random access is low. 




# Quick sort
[https://www.youtube.com/watch?v=MZaf_9IZCrc]

- it can never happen that i is at smaller element bcz if it were it would have already processed it

```c++
    void quickSort(int a[], int low, int high) {
		if(low >= high) return;

		int p = partition(a, low, high);
		quickSort(a, low, p-1);
		quickSort(a, p+1, high);
		return;
    }

    int partition (int a[], int low, int high) {
		// Generate a random number in between [low, high]
		srand(time(NULL));
		int random = low + rand() % (high - low);
	
		// Swap A[random] with A[high]
		swap(arr[random], arr[low]);

		int pivot = a[high];
		int i = low-1; 		//bcz it might happen the left part is empty

		for(int j=low; j<high; ++j){
			if(a[j] <= pivot){
				++i;
				swap(a[j], a[i]);
			}
		}

		++i;
		swap(a[high], a[i]);
		return i;
    }
```







# for inversion : look for questions that ask pair of integers with a condition

	ex : number of smaller/greater elements to right/left of a[i]





# Sum of Manhatten distances [IMP]
[https://www.geeksforgeeks.org/sum-manhattan-distances-pairs-points/]

	the idea is to sort x and y coordinates separately and caluculate ans. separately because x any y don't affect each other
	find pattern in x and y 

```c++

```




# Generally absolute difference means something is to be plotted on number line



# count inversions 
[https://practice.geeksforgeeks.org/problems/inversion-of-array-1587115620/1#]

	similar to merge sort just some changes in merge procedure

	sorting is not necessary here we are sorting so that we can find elements greater than a[j] faster i.3 (m-i=1)
	if we don't sort then we would need to find number of elements greater than a[j] which takes n^2 time

	inv count = inv of left subarry 
					+
				inv of right subarray
					+
				inter array inv. count from merge process

	ideally we want [l, m] to come before [m+1, r] but if at some point we switch to j that means that (m-i+1) elements are bigger than a[j]
	so we add m-i+1 to inversion count

	ex : 
		1 2 3 5
		0 4

		at first we arrive at j that means [1..5] are bigger than 0 so they are inversions
		then we arrive at 1 no inversion in 1st array will be there
		then we arrive at 2 no inversion in 1st array will be there
		then we arrive at 3 no inversion in 1st array will be there
		then we arrive at 4 which is in j so that means [5] is invesion because it is occuring before 4 but is greater than 4
		...



	ll i=l, j=m+1;
	while(i<=m && j<=r){
		if(a[i] <= a[j])
			temp.push_back(a[i++]);
		else{
			inv += (m-i+1);
			temp.push_back(a[j++]);
		}
	}


# Merge sort
- inclusive [l, r] i.e l = 0, r = n-1 initially


```c++
    void merge(int arr[], int l, int m, int r) {
		vector<int> temp;

		int i=l, j=m+1;
		while(i<=m && j<=r){
			if(arr[i] <= arr[j])
				temp.push_back(arr[i++]);
			else
				temp.push_back(arr[j++]);
		}

		while(i<=m)
			temp.push_back(arr[i++]);

		while(j<=r)
			temp.push_back(arr[j++]);
		

		int c = 0;
		for(int k=l; k<=r; ++k)
			arr[k] = temp[c++];

		return;
    }

    void mergeSort(int arr[], int l, int r) {
		if(l>=r)
			return;

		int mid = l + (r-l)/2;
		mergeSort(arr, l, mid);
		mergeSort(arr, mid+1, r);

		merge(arr, l, mid, r);
		return;
    }
```





# Common merge procedure keywords
-	given already sorted array you need to return another sorted array by modifying elements
	
	in this type you apply modified merging process
	where the first element is either L or R then inc. / dec. L or R based on values

	x x x x x x x x 
	L			  R


- Or you could be given to form a sorted array by applying some condition


- when we have 1 valley or hill then we can merge them
	
	\      /  	OR 		 /\
	 \    / 			/  \
	  \	 / 			   /    \
	   \/


# Bubble sort

```c++
bool done = false;
while(!done){
	done = true;

	for(int i=0; i<n-1; ++i){
		if(a[i] < a[i+1]){
			swap(a[i], a[i+1]);
			done = false;
		}
	}
}
```




# 3 level comparator like this

bool comp(vector<int> x, vector<int> y){
	if(x[0]....y[0])

	if(x[1] ... y[1])

	return x[2] ... y[2]
}




# sort based on 2nd value and if 2nd value same sort on first value in ascending

```c++
bool comp(pair<int, int> x, pair<int, int> y){

	if(x.second == y.second) //if second is same sort based on first
		return x.first < y.first 


	return x.second < y.second //if x.second is smaller return true i.e put x first else put y first
}
```


# with comparator function you can compare 2,3,4,5 any number of values just pass the values and compare them


# Descending order comparator

```c++
bool comp(int x, int y){
	if(x > y)
		return true;
	return false
}
//shorthand : return x > y
```



# Ascending order comparator

```c++
bool comp(int x, int y){
	if(x < y)
		return true;
	return false
}
//shorthand : return x < y
```




# comparator logic

	1. If we want x to come first we return true;
	2. If we want y to come first we return false;
	3. x and y can be any 2 numbers from array need not to be in order