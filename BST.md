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
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
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