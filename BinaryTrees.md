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
	to get next greater element just go to left most node of its right child while pushing into stack
	while(node)
		stack.push(node-data);
		node = node->left;






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
