### Merge K sorted arrays (K^2 logk)
[https://practice.geeksforgeeks.org/problems/merge-k-sorted-arrays/1#]

    1. O(k^2 log k^2) 
        create an array of k^2 elements and sort it

    2. O(k^3) 
        merge one by one

        start = 0;
        for(int i=1; i<k; ++i)
            merge(start, i);
        
    
        T.C : 2k + 3k + 4k + .... = o(K^3)

    3. O(k^2 logk) 
        using divide and conquer merging
        logk for outer merge
        and at each level we do k^2 work

        O(k^2) extra memory

    4. O(K^3) 
        maintain k pointers at start
        and find min at each point and insert into final array 
    
    5. O(k^2 logk) 
        problem with k pointer approach is we need to find min of k elements fast
        we can maintain min heap of k elements
        and remove min of them and add it to final ans
        after that we insert next min and so on

        O(k) extra memory for heap


```c++
    #define pii pair<int,int>
    #define pipii pair<int, pii>

    vector<int> mergeKArrays(vector<vector<int>> a, int k) {
        priority_queue<pipii, vector<pipii>, greater<pipii>> pq;

        //first push first column 
        for(int i=0; i<k; ++i)
            pq.push({a[i][0], {i, 0}});
        
        vector<int> ans;
        while(!pq.empty()){
            //everytime we pop one element and push its i, j+1 in p_queue
            auto x = pq.top();
            pq.pop();

            ans.push_back(x.first);
            int i = x.second.first;
            int j = x.second.second;

            //insert only if next exist
            if(j+1 < k){
                pq.push({a[i][j+1], {i, j+1}});
            }
        }

        return ans;
    }
```





### IN heaps it is better to limit the size of heap till k to get logk complexity
    insetad of adding everything to queue 
    only insert k first then delete and insert






### Heap sort
- not stable
- O(1) space if we only change array by using buildHeap() on array
- T.C : nLogn

```c++
    //lol heap sort
    priority_queue<int, vector<int>, greater<int>> q(a.begin(), a.end());
    while(!q.empty()){
        ans.push_back(q.top());
        q.pop();
    }

```
    //OR
    for increasing order
    1. Build a max heap from the input data. 
    2. At this point, the largest item is stored at the root of the heap. Replace it with the last item of the heap followed by reducing the size of heap by 1. Finally, heapify the root of the tree. 
    3. Repeat step 2 while the size of the heap is greater than 1.

```c++
    void heapSort(int a[], int n) {
        //using O(n) and heapify
        buildHeap(a, n);

        while(n > 1){
            swap(a[1], a[n]);
            --n;
            heapify(a, n, 1);
        }
    }
```





### When to use what heap
-   for min k values I should use max heap(priority queue) and max k values min heap
	
-   To find min k elelmets use max heap and maintain the size of max heap till k
	now if smaller elements comes we can easily remove topmost element from maxheap and then insert that element
	that's why we use maxheap in min k problems.




=============================================
# Priority queue


- default : max heap

- priority_queue<1, 2, 3>
    1 : data type of each element of pq
    2 : sequential data structure to make pq from (must be sequential)
    3 : comp (not a function)
        this is struct / user defined type
        

### comparator for priority queue
-   Return true if you want to put a after b in array
    false : do nothing

-   This is reverse of array comparators
    if you want smaller number first
    when a < b return false 
    when a > b return true bcz a is greater so return true to put a behing b i.e bring b to top

```c++
	struct comp{
    	bool operator() (Type &a, Type &b){
            return a > b    //min heap
            return a < b    //max heap
    	}
    }
```




=============================================
## Heap operations

### inserting in heap one by one = O(nlogn)
    log1 + log2 + log3 + log4 .... logn
    log (1*2*3 ... n)
    log n!
    log n^n
    n log n

### inserting in heap at once using array = O(N)



### get Max 
    O(1)
    return a[1]

### Insert(x)
    O(logn)
    first insert at end i.e insert at a[n+1]
    then bubble it up

    ++size;
    a[size] = x;
    while(i>1 && a[i] < a[i/2]){
        swap(a[i], a[i/2]);
        i /= 2;
    }


### delete (root)
    O(logn)

    swap first and last element and then do size-- to forget it
    then apply maxheapify on root to fix it

    swap(a[1], a[size]);
    --size;
    maxHeapify(root);


### delete (i)
    O(logn)

    make this element as infinity to get it to root 
    then simply apply delete(root)

    a[i] = inf;
    bubbleup(i);
    delete(root);



=============================================

### Building a heap

1. Heapify O(n)
heapify is a process that assumes left and right subtree is already heap and it fixes root 

            root
    heap            heap

so starting from last non leaf node we call heapify on all nodes till root and fix them 
bcz leaf nodes are heap by default

```c++
    void maxHeapify(int i){
        if(i > n/2)
            return;
        
        //max index
        int mx = i;

        if(2*i <= n && a[2*i] > mx)
            mx = 2*i;
        if(2*i+1 <= n && a[2*i+1] > a[mx])
            mx = 2*i + 1;
        
        if(mx != i){
            swap(a[i], a[mx]);
            maxHeapify(mx);
        }
    }

    int main(){
        for(int i=n/2; i>=1; --i){
            maxHeapify(i);
        }
    }
```


.... 
...... n/16 * 4
......... n/8 * 3
............ n/4 * 2
............... n/2 * 1
....................


n/2 level need 1 step to reach bottom
n/4 level need 2 step to reach bottom
...


steps to reach bottom = 
n/2 + n/4 * 2 + n/8 * 3 ....

n ( 1/2 + 1/2 + 3/8 + 4/16 ....) [this part is very small]

= O(n)


in this method less nodes will need logn steps to move to bottom 
in prev method all leaf nodes needed logn time
so this method is better




2. Using heap property (n Logn)
we can iterate over all elements and try to fix them one by one
iterating takes O(n) and fixing takes O(logn)

for fixing : if any element is in bad position we swap it with its parent till its in good position
i.e we bubble the element up

```c++
    //to build max heap
    for(int i=1; i<=n; ++i){
        //if element is greater than parent then swap and move up
        //we do this till we reach i==1
        while( i > 1  &&  a[i] > a[i/2]){   
            swap(a[i], a[i/2]);
            i /= 2;
        }
    }
```

last level n/2 nodes can travel => log n
n/4 nodes can travel => log(n-1)
n/8 nodes can travel => log(n-2)

n/2 logn + n/4 log(n-1) + n/8 log(n-2) ....
n/2 logn + n/4 logn + n/8 logn

n logn (1/2 + 1/4 + 1/8 ....)

n log n




3. Using sorting (n Logn)
if we sort the array we will get heap like behaviour 
if we want max heap we sort array in descending ordder and if we want min heap we sort in ascending order
that way every parent will be bigger / smaller than its child



### Heap representation
    we represent heaps in array
    we start heaps from 1 based indexing

    left    : 2*i
    right   : 2*i + 1
    parent  : i/2

- heap using array will always be a complete binary tree 
- all leaves lie after = `n/2`
    if(index > n/2)
        it is a leaf

- last node of tree  =  `a[n]`
- last non leaf node  =  parent of last node = `n/2`
- last level has `n/2` nodes
