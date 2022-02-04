### If we want to remove kth element from end we need to move fast by k+1 from front to reach parent of kth node
[https://leetcode.com/problems/remove-nth-node-from-end-of-list/]




### In linked list questions always add dummy head for better edge case handling [vvvimp]




### Insert procedure is just merging of 2 linked lists
	one LL : 		1 -> 2 -> 6 -> 7
	other LL : 		3

	merge with list of size 1 = insert procedure




### we can use divide and conquer where we are doing something one by one
	like one by one merging....





### How to pass pointer as a reference
many times we need to modify a pointer so we pass it as referece so that we could modify original value
OR
use pointer to pointer but you will have to modify the code a bit


	void func(Node*& front){
		...
	}

	OR

	void func(Node** front){
		...
	}



### Cycle check in L.L

	T.C :
		O(n) for both to reach cycle
		O(n) for pointers to meet inside cycle

	we take slow pointer and a fast pointer
	we move slow by 1 and fast by 2 so relative velocity = 1
	now if list contains cycle they will definitely meet because once slow and fast reaches in a cycle
	their distance will decrease by 1 eventually they will meet 

	proof : 
		time = relative dist / relative speed
		time = relative dist / 1

		since 1 divides all then there will be a time when both pointers will have same dis. i.e they will meet

```c++
	int func(struct Node *head) {
		Node* slow = head;
		Node* fast = head;
		Node* meet = NULL;

		while(fast && fast->next){		//find the meeting point of slow and fast
			slow = slow->next;
			fast = fast->next->next;

			if(slow == fast){
				meet = slow;
				break;
			}
		}

		if(meet == NULL)	//no cycle found
			return 0;


        //using floyd's algo we need to move l1 dist from d 
		//i.e we need to move until head and meet meet
		ListNode* start = head;
		while(start != meet){
			start = start->next;
			meet = meet->next;
		}
	}
```



### check if L.L is palindrome or not
	reverse the half part and compare the elements

	// ======= O(n) time O(1) space

```c++
	ListNode* getMiddle(ListNode* head){
		ListNode* slow = head;
		ListNode* fast = head;

		while(fast && fast->next){
			slow = slow->next;
			fast = fast->next->next;
		}

		return slow;
	}

	ListNode* reverse(ListNode* head){
		ListNode* curr = head;
		ListNode* prev = NULL;
		ListNode* next;

		while(curr){
			next = curr->next;
			curr->next = prev;
			prev = curr;
			curr = next;
		}

		head = prev;
		return head;
	}

	class Solution {
	public:
		bool isPalindrome(ListNode* head) {
			ListNode* middleNode = getMiddle(head);
			ListNode* end = reverse(middleNode);
			ListNode* start = head;

			while(start && end){
				if(start->val != end->val)
					return false;

				start = start->next;
				end = end->next;
			}

			return true;
		}
	};
```


### Reverse linked list (Recursively)

```c++
	void solve(ListNode* prev, ListNode* curr, ListNode* head){
		if(curr == NULL){
			head = prev; //this is the new head
			return;
		}

		solve(curr, curr->next, head);
		curr->next = prev;
		return;
	}

    ListNode* reverseList(ListNode* head) {
		solve(NULL, head, head);
		return ans;
    }
```



### Reverse linked list (Iterative)
	use 3 pointers prev, curr, next
	initially prev = null
	curr = head

	ListNode* reverse(ListNode* head){
		ListNode* curr = head;
		ListNode* prev = NULL;
		ListNode* next;

		while(curr){
			next = curr->next;
			curr->next = prev;
			prev = curr;
			curr = next;
		}

		head = prev;
		return head;
	}


### Slow fast pointer method

move one pointer slowly and move other pointer fastly and then use this to your advantage.

ex. find middle element in linked list
	use slow pointer move by 1 and use fast pointer to move by 2 so by the end your fast reaches end your slow will be at mid.


### get middle of LL floor(n/2)          [if n is even get the second element]

```c++
	ListNode* getMiddle(ListNode* head){
		ListNode* slow = head;
		ListNode* fast = head;

		while(fast && fast->next){
			slow = slow->next;
			fast = fast->next->next;
		}

		return slow;
	}
```

### get middle of LL ceil(n/2)		[if n is even get the first element]

```c++
	ListNode* getMiddle(ListNode* head){
		ListNode* slow = head;
		ListNode* fast = head;

		while(fast->next && fast->next->next){
			slow = slow->next;
			fast = fast->next->next;
		}

		return slow;
	}
```