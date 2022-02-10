### Binary tree from Levelorder and inorder[imp]
[https://practice.geeksforgeeks.org/problems/construct-tree-from-inorder-and-levelorder/1#]

- Root is that node which comes up first in level order
- from l to r search which node comes first in levelorder

```c++
unordered_map<int, int> m;

Node* solve(int l, int r, int* io, int* lo){
    if(l > r)
        return NULL;

    //find which node comes first in levelorder that is node
    pair<int, int> rv = {INT_MAX, l};
    for(int i=l; i<=r; ++i){
        rv = min(rv, {m[io[i]], i});
    }

    Node* root = new Node(io[rv.second]);
    root->left = solve(l, rv.second-1, io, lo);
    root->right = solve(rv.second+1, r, io, lo);
    return root;
}

Node* buildTree(int inorder[], int levelorder[], int iStart, int iEnd,int n) {
    for(int i=0; i<n; ++i)
        m[levelorder[i]] = i;

    return solve(0, n-1, inorder, levelorder);
}
```







### Path sum 3 (using hashing in trees)
[https://leetcode.com/problems/path-sum-iii/]

at every node we take prefix of parent nodes and we insert this prefix in map
when we finish executing this node we remove this prefix from map
at each prefix we do ans += m[prefix-target]







### search for list in Binary tree
[https://leetcode.com/problems/linked-list-in-binary-tree/]

we can search linearly in each tree by moving next pointer in arguments
think like :
    if match then search for next element below
    if dont match then return false

```c++
    //return true if there is LL matching starting from this point
    bool search(ListNode* head, TreeNode* root){
        if(head == NULL)
            return true;
        if(root == NULL && head != NULL)
            return false;
        
        if(head->val != root->val)
            return false;

        return search(head->next, root->left) || search(head->next, root->right);
    }
```




### whenever we need to make balanced bst we divide and conquer using mid element


### whenver array / LL comes with trees dividing the array in function calls is good choice

like this

			[1, 2, 3, 4, 5, 6]
		[1, 2, 3]			[4, 5, 6]
	...			...		...				...	





### ways to uniquely identify a tree

-	preorder + NULLS
-	postorder + NULLS
-	preorder + inorder			(we find position of root in inorder and then find nodes in lst and rst using inorder)
-	postorder + inorder


### 2*i+1 and 2*i+2  happens only in complete binary treee
-	and it can be done using queues



### serialization / deserialization
	in deserialization after first call to left finishes the (++idx) will point to right subtrees first node
	bcz we are moving idx using reference

	like this

	node* solve(vector<int> &pre, int &idx){
		if(idx >= pre.size())
			return NULL;
		
		node* temp = newNode(pre[idx]);
		temp->left = solve(pre, preLN, ++idx);
		temp->right = solve(pre, preLN, ++idx);
		return temp;
	}



```c++
class Solution
{
    public:

    //serialize using preorder with null nodes 
	void serializeHelper(Node* root, vector<int> &ans){
		if(!root){
			ans.push_back(-1);
			return;
		}

		ans.push_back(root->data);
		serializeHelper(root->left, ans);
		serializeHelper(root->right, ans);
		return;
	}
    vector<int> serialize(Node *root) {
		vector<int> ans;
		serializeHelper(root, ans);
		return ans;
    }
    

    //deserialize 
	//f(i) makes tree starting from ith index considering [i, a.size()]
	Node* deSerializeHelper(int &idx, vector<int> &a){
		if(idx >= a.size())
			return NULL;
		
		if(a[idx] == -1)
			return NULL;
		
		Node* node = new Node(a[idx]);
		node->left = deSerializeHelper(++idx, a);
		node->right = deSerializeHelper(++idx, a);
		return node;
	}
    Node * deSerialize(vector<int> &a) {
		int idx = 0;
		return deSerializeHelper(idx, a);
    }

};
```



### whenever we have generate all possible trees
	use array returning method
	like this

	for(int i=1; i<n; ++i){
		vector<TreeNode*> lst = allPossibleFBT(i);
		vector<TreeNode*> rst = allPossibleFBT(n-1-i);

		for(auto x : lst){
			for(auto y : rst){
				ans.push_back(new TreeNode(0, x, y));
			}
		}
	}







***
### solution to missing / overlapping levels in vertical order traversal
[https://leetcode.com/problems/maximum-width-of-binary-tree/submissions/]
	since we are missing levels
	i.e we have overlapping levels

	then we can level them vertically as
				
					n
			2*n			2*n + 1






***
### generate all possible binary trees
[https://leetcode.com/problems/all-possible-full-binary-trees/solution/]

	we use recursion 
	each function returns vector of all possible trees
	then we simply calculate all possible using rule of product
	total = lst * rst
	and merge and send then ans. above

```c++
    vector<TreeNode*> allPossibleFBT(int n) {
		//base case : only one node then create it
		if(n==1){
			return vector<TreeNode*> {new TreeNode(0, NULL, NULL)};
		}

		//otherwise create all possible trees from left as well as right
		vector<TreeNode*> ans;

		for(int i=1; i<n; ++i){
			vector<TreeNode*> lst = allPossibleFBT(i);
			vector<TreeNode*> rst = allPossibleFBT(n-1-i);

			for(auto x : lst){
				for(auto y : rst){
					ans.push_back(new TreeNode(0, x, y));
				}
			}
		}

		return ans;
    }
```





***
### bottom View 

- just output back of every level in vertical traversal
- space optimization : instead of list just use a single variable and store node in that 

	map<int, int> m;
	at every time 
	m[level] = node;

```c++
	vector<int> ans;
	for(int i=mn; i<=mx; ++i){
		ans.push_back(m[i].back());
	}

	return ans;
```




***
### Top View 

- just output front of every level in vertical traversal
- space optimization : instead of list just use a single variable and store node in that 
	only update level's node if you encounter a new level

	map<int, int> m;
	at every time 
	if(m.find(level) == m.end())
		m[level] = node;

```c++
	vector<int> ans;
	for(int i=mn; i<=mx; ++i){
		ans.push_back(m[i].front());
	}

	return ans;
```

### Space optimized top/bottom view

- top view : only update map if new level is seen
- bottom view : update map everytime level is seen (current possible ans)
```c++
    vector<int> verticalOrder(Node *root) {
		unordered_map<int, int > m;
		int mn = INT_MAX, mx = INT_MIN;
		queue<pair<Node*, int> > q;
		q.push({root, 0});

		while(!q.empty()){
			int sz = q.size();
			while(sz--){
				Node* node = q.front().first;
				int lvl = q.front().second;
				q.pop();

				mn = min(mn, lvl);
				mx = max(mx, lvl);

				if(m.count(lvl)) 		//for top view
					m[lvl] = node->data;

				m[lvl] = node->data 	//for bottom view

				if(node->left) q.push({node->left, lvl-1});
				if(node->right) q.push({node->right, lvl+1});
			}
		}

    }
```






***
### vertical order traversal (using bfs and unordered_map)
https://www.youtube.com/watch?v=EBTku_aIPXk


-2	-1	0	1 	2  <== levels(using map)

		1
	2		3
4		5		6

- If you use map T.C would shoot to nlogn so use unordered_map for o(n)
- but in unordered_map we have a problem we can't track starting level
- so maintain 2 variables extra [mn, mx] mn denotes min level and mx as max level
- then finally iterate from min level to max level and print nodes on each level

```c++
    vector<int> verticalOrder(Node *root) {
		unordered_map<int, vector<int> > m;
		int mn = INT_MAX, mx = INT_MIN;
		queue<pair<Node*, int> > q;
		q.push({root, 0});

		while(!q.empty()){
			int sz = q.size();
			while(sz--){
				Node* node = q.front().first;
				int lvl = q.front().second;
				q.pop();

				mn = min(mn, lvl);
				mx = max(mx, lvl);

				m[lvl].push_back(node->data);

				if(node->left)
					q.push({node->left, lvl-1});
				if(node->right)
					q.push({node->right, lvl+1});
			}
		}

		vector<int> ans;
		for(int i=mn; i<=mx; ++i){
			for(auto x:m[i])
				ans.push_back(x);
		}

		return ans;
    }
```



***
### Maintain parent of node easy way
	TreeNode* par = nullptr;
	TreeNode* cur = root;
	while(cur){
		if(cur->val == key)
			break;
		
		par = cur;
		if(cur->val < key) 
			cur = cur->right;
		else
			cur = cur->left;
	}



***
### BFS on trees
	T.C : O(n)
	S.C : O(Width of tree)



*** 
### LCA [VVV IMP] O(n) O(H)
[https://www.youtube.com/watch?v=13m9ZCB8gjw]

	works only if elements are unique
	
	corner case : if both elements are in same subtree then it will return LCA 
		but if one of the elements is not presesnt it will still return LCA so make sure both elements are present in tree
		you can do that by applying dfs afterwards


```c++
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
	if(!root)
		return NULL;
	
	if(root->val == p->val|| root->val == q->val)
		return root;
	
	TreeNode* lst = lowestCommonAncestor(root->left, p, q);
	TreeNode* rst = lowestCommonAncestor(root->right, p, q);

	if(lst && rst) //this is lca
		return root;
	else if(!lst && !rst) //found nothing in subtrees
		return NULL;
	
	return (lst ? lst : rst);
}
```



***
### Remove half nodes
[https://practice.geeksforgeeks.org/problems/remove-half-nodes/1#]

first set left node of root = whatever ans we get by solving left subtree (root of tree after removing all half nodes)
first set right node of root = whatever ans we get by solving right subtree

now if root is leaf node or is a good node then return that 
else if it does not have right child then return its left child as next good node and delete it
same for left child

```c++
Node *RemoveHalfNodes(Node *root) {
	if(!root) return root;

	root->left = RemoveHalfNodes(root->left);
	root->right = RemoveHalfNodes(root->right);

	if(root->left && root->right) //this is a good node
		return root;
	if(!root->left && !root->right) //this is a leaf node
		return root;
	
	if(!root->left){
		Node* temp = root->right;
		delete(root);
		return temp;
	}
	if(!root->right){
		Node* temp = root->left;
		delete(root);
		return temp;
	}
}
```






***
### ITERATIVE TRAVERSALS
the element that needs to be processed next must come at top of the stack
	ex: in preorder left is processed first so right must be pushed in stack first so it comes at bottom

- 1.Preorder traversal
	N L R 
	<----

	L
	R
	N

```c++
    vector<int> preorderTraversal(TreeNode* root) {
		vector<int> ans;
		if(!root)
			return ans;


		stack<TreeNode*> s;
		s.push(root);
		while(!s.empty()){
			TreeNode* node = s.top();
			s.pop();
			ans.push_back(node->val);

			if(node->right) s.push(node->right);
			if(node->left) s.push(node->left);
		}

		return ans;
    }
```

- 2.Inorder 
	1 = NOT visited
	2 = VISITED


	First visit root for first time indicated by 1 then while stack is not empty
	if top.second = 2 then it means that it is being visited again so we simply print it
	else 
		we insert right child then root with updated vis = 2 then left child
		bcz we want to visit left first

	L N R
	<----

	L
	N
	R

	can be done for preorder too

```c++
    vector<int> InorderTraversal(TreeNode* root) {
		vector<int> ans;
		if(!root)
			return ans;

		stack<pair<TreeNode*, int> > s;
		s.push({root, 1});

		while(!s.empty()){
			pair<TreeNode*, int> node = s.top();
			s.pop();

			if(node.second == 2){
				ans.push_back(node.first->val);
			}
			else{
				if(node.first->right)
					s.push({node.first->right, 1});
				
				s.push({node.first, 2});

				if(node.first->left)
					s.push({node.first->left, 1});
				
			}
		}

		return ans; 
    }
```

- 3.PostOrder
L R N
<----

	L
	R
	N

```c++
    vector<int> PostorderTraversal(TreeNode* root) {
		vector<int> ans;
		if(!root)
			return ans;

		stack<pair<TreeNode*, int> > s;
		s.push({root, 1});

		while(!s.empty()){
			pair<TreeNode*, int> node = s.top();
			s.pop();

			if(node.second == 2){
				ans.push_back(node.first->val);
			}
			else{
				s.push({node.first, 2});

				if(node.first->right)
					s.push({node.first->right, 1});

				if(node.first->left)
					s.push({node.first->left, 1});
				
			}
		}

		return ans; 
    }
```

***
### we can return two or more than two informations together in a same function to reduce extra space complexity
	imp. trick used in calculating diameter


***
### We can use iterative inorder traversal in BST using stack [2sum using BST leetcode] 
	to get smallest element we get to leftmost element
	to get next greater element just go to left most node of its right child while pushing into stack

```c++
	//this function give a root goes to leftmost while remembering the past
	void getleft(TreeNode*& root, stack<TreeNode*> &s){
		TreeNode* temp = root;
		while(temp){
			s.push(temp);
			temp = temp->left;
		}
	}

    vector<int> inorderTraversal(TreeNode* root) {
		vector<int> ans;
		stack<TreeNode*> s;
		getleft(root, s);

		while(!s.empty()){
			TreeNode* node = s.top();
			s.pop();

			ans.push_back(node->val);
			getleft(node->right, s);
		}

		return ans;
    }
```
	




***
### Get level of a node in binary tree
returns -1 if node doesnot exits else returns 

int getLevel(Node* root, int val, int depth){
	if(!root)
		return -1;

	if(root->data == val)
		return depth;

	return max(getLevel(root->left, val, depth+1), getLevel(root->right, val, depth+1));
}
