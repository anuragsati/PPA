### preorder / postorder to bst

-	method 1 : naive O(n^2)
	we know preorder : root left right
	all nodes in left are smaller and all nodes in right are larger
		root [smaller]  [larger]
	
	```c++
		//this function accepts preorder array and makes bst out of it and returns its head
		TreeNode* solve(vector<int> &pre, int l, int r){
			if(l>r)
				return NULL;

			TreeNode* root = new TreeNode(pre[l]);

			//the elements which are smaller than pre[l] ==> lst
			// rest in rst
			int i;
			for(i=l+1; i<=r; ++i){
				if(pre[l] < pre[i])
					break;
			}

			root->left = solve(pre, l+1, i-1);
			root->right= solve(pre, i, r);
			return root;
		}

		TreeNode* bstFromPreorder(vector<int>& preorder) {
			int n = preorder.size();
			return solve(preorder, 0, n-1);
		}
	```


-	method 2 : better brute force O(n) O(n)
	for every node instead of naive way we can directly check which node is next greatest in preorder
	we use next greater element to right for preorder -> bst


-	method 3 : O(n) O(1) we use interval method
	we try to construct tree sequentially and check if this node can be placed here or not based on interval
	if it can be placed we place it else we put null and go up the recursion 

	```c++
		TreeNode* solve(vector<int> &pre, int mn, int mx, int &idx){
			if(idx >= pre.size()){
				--idx;
				return NULL;
			}
			
			if(mn < pre[idx] && pre[idx] < mx){
				TreeNode* node = new TreeNode(pre[idx]);

				node->left = solve(pre, mn, pre[idx], ++idx);
				node->right = solve(pre, pre[idx], mx, ++idx);

				return node;
			}
			else{
				--idx;
				return NULL;
			}
		}
		TreeNode* bstFromPreorder(vector<int>& preorder) {
			int idx = 0;
			return solve(preorder, INT_MIN, INT_MAX, idx);
		}
	```


### interval method in bst is very imp [low, high]



### array to height balanced BST
function(l, r) => takes an array and returns height balnced bst
we find mid make that node
then lst will have all nodes from [l, mid-1]
rst will have nodes from [mid+1, r]


```c++
	TreeNode* convert(int l, int r, vector<int> &a){
		if(l > r)
			return NULL;

		int mid = l + (r-l) / 2;
		TreeNode* root = new TreeNode(a[mid]);
		root->left = convert(l, mid-1, a);
		root->right = convert(mid+1, r, a);

		return root;
	}
```



### max sum BST in a binary tree

```c++
	struct subtreeInfo {
		bool isBST;
		int mn, mx, sum;

		subtreeInfo(bool bst, int mnval, int mxval, int treesum){
			isBST = bst;
			mn = mnval;
			mx = mxval;
			sum = treesum;
		}
	};

	int ans = 0;
	
	subtreeInfo* solve(TreeNode* root){
		if(!root)
			return new subtreeInfo(true, INT_MAX, INT_MIN, 0);


		subtreeInfo* lst = solve(root->left);
		subtreeInfo* rst = solve(root->right);

		int mn = min({root->val, lst->mn, rst->mn});
		int mx = max({root->val, lst->mx, rst->mx});
		int curr_sum = root->val + lst->sum + rst->sum;

		if(lst->isBST && rst->isBST && lst->mx < root->val && root->val < rst->mn){		//this subtree is bst
			ans = max(ans, curr_sum);
			return new subtreeInfo(true, mn, mx, curr_sum);
		}
		else{
			return new subtreeInfo(false, mn, mx, curr_sum);
		}
	}

    int maxSumBST(TreeNode* root) {
		ans = 0;
		solve(root);
		return ans;
    }
```



### Largest BST in a binary tree
we simply return all the info we need from subtrees
just like check if tree is bst or not except here we need one more data i.e size of subtree

```c++
	struct sInfo{
		bool isBST;
		int mn;
		int mx;
		int subtree_size;

		sInfo(bool bst, int mnVal, int mxVal, int cnt){
			isBST = bst;
			mn = mnVal, mx = mxVal, subtree_size = cnt;
		}
	};

	int ans = 0;

	sInfo* solve(Node* root){
		if(!root)
			return new sInfo(true, INT_MAX, INT_MIN, 0);

		sInfo* lst = solve(root->left);
		sInfo* rst = solve(root->right);

		int subSize = 1 + lst->subtree_size + rst->subtree_size;
		int mn = min({root->data, lst->mn, rst->mn});
		int mx = max({root->data, lst->mx, rst->mx});
		bool isBST = lst->isBST && rst->isBST;

		if(lst->isBST && rst->isBST && lst->mx < root->data && root->data < rst->mn){
			ans = max(ans, subSize);
			return new sInfo(isBST, mn, mx, subSize);
		}
		else{
			return new sInfo(false, mn, mx, subSize);
		}
	}

    int largestBst(Node *root) {
		ans = 0;
		solve(root);
		return ans;
    }

```



### Check if tree is BST or not
- method 1 : check if inorder traversal is sorted or not (do it by maintaining prev pointer in recursion)

- method 2 : recursion
	just checking if root>left && root < right will not help
	you will have to check :
		if left subtree is bst 
		if right subtree is bst
		and current node value is greater than max value of left tree and smaller than min value of right tree

```c++
	// [bool, <max, min>]
	pair<bool, pair<ll, ll> > solve(TreeNode* root){
		if(!root)
			return {true, {LONG_LONG_MIN, LONG_LONG_MAX}};
		
		pair<bool, pair<ll, ll> > lst = solve(root->left);
		pair<bool, pair<ll, ll> > rst = solve(root->right);

		ll mx = max({(ll)root->val, lst.second.first, rst.second.first});
		ll mn = min({(ll)root->val, lst.second.second, rst.second.second});
		bool ans = lst.first && rst.first;

		if(root->val > lst.second.first && root->val < rst.second.second){
			return {ans, {mx, mn}};
		}
		else{
			return {false, {mx, mn}};
		}
	}

    bool isValidBST(TreeNode* root) {
		return solve(root).first;
    }
```

- method 3 : using intervals/ranges in recursion
	intial interval (-inf, inf)
	if we move to right subtree then left interval increases
	if we move left subtree then right interval decreases

```c++
	const long inf = LONG_MAX;
	bool ans = true;

	void solve(TreeNode* root, long lb, long ub){
		if(!root)
			return;

		if(!(root->val > lb && root->val < ub)){
			ans = false;
			return;
		}

		solve(root->left, lb, root->val);
		solve(root->right, root->val, ub);
		return;
	}

    bool isValidBST(TreeNode* root) {
		solve(root, -inf, inf);
		return ans;
    }
```



### Delete in BST

- Special case when key is at root

- case 0 : Node is a leaf node with 0 child 
	simply delete the node and set its parent pointer to null

- case 1 : Node has 1 child
	simply delete node and set its parents child to nodes' child

- case 2 : Node has 2 child
	either swap node with largest value from left subtree
	or swap it with smallest value in right subtree

	then we can guarantee that the swapped value has 0 or 1 child so delete it normally


```c++
class Solution {
	pair<TreeNode*, TreeNode*> search_node(TreeNode* root, int key){
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

		return make_pair(cur, par);
	}

	void remove_leaf(TreeNode* cur, TreeNode* par){
		if(par->left == cur)
			par->left = nullptr;
		else
			par->right = nullptr;

		delete(cur);
	}

	void remove_single_child(TreeNode* cur, TreeNode* par){
		TreeNode* newChild = (cur->left) ? cur->left : cur->right; //if left child exist then set to left else right

		if(par->left == cur)
			par->left = newChild;
		else
			par->right = newChild;
		
		delete(cur);		
	}

	void remove_two_child(TreeNode* cur){
		//find left max
		TreeNode* newpar = cur;
		TreeNode* lmax = cur->left;

		while(lmax->right){
			newpar = lmax;
			lmax = lmax->right;
		}

		//swap cur and lmax
		cur->val = lmax->val;

		//delete lmax (it can have 0 or 1 child)
		//only one child
		if(!lmax->left && !lmax->right){
			remove_leaf(lmax, newpar);
		}

		//2 child
		else if(!lmax->left || !lmax->right){
			remove_single_child(lmax, newpar);
		}
	}

public:
    TreeNode* deleteNode(TreeNode* root, int key) {
		//search for that node
		pair<TreeNode*, TreeNode*> sans = search_node(root, key);
		TreeNode* cur = sans.first;
		TreeNode* par = sans.second;


		//key is not found in tree return original tree
		if(!cur){
			return root;
		}

		//if key is found at root
		if(cur == root){
			if(!cur->left && !cur->right){
				delete(cur);
				return nullptr;
			}
			else if(!cur->left || !cur->right){
				TreeNode* newRoot = (cur->left) ? cur->left : cur->right; 
				delete(cur);
				return newRoot;
			}
			else{
				remove_two_child(cur);
			}

			return root;
		}


		//key is a leaf node remove the key and find out which pointer is it and set parent's left/right to nullptr
		if(!cur->left && !cur->right)
			remove_leaf(cur, par);

		//if key has a single child only
		else if(!cur->left || !cur->right)
			remove_single_child(cur, par);

		//key has 2 childs 
		else
			remove_two_child(cur);

		return root;
    }
};
```


### Insert in BST
inserting as a leaf node
if val is greater than curr node then move right if right doesnot exist then insert here else move right

```c++
    TreeNode* insertIntoBST(TreeNode* root, int val) {
		TreeNode* newNode = new TreeNode(val);

		if(!root){
			root = newNode;
			return root;
		}

		TreeNode* node = root;
		while(node){
			if(node->val == val)
				break;

			if(node->val < val){
				if(!node->right)
					node->right = newNode;

				node = node->right;
			}
			else{
				if(!node->left)
					node->left = newNode;

				node = node->left;
			}
		}

		return root;
    }
```