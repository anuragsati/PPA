### Cycle check in L.L

	we take slow pointer and a fast pointer
	we move slow by 1 and fast by 2 so relative velocity = 1
	now if list contains cycle they will definitely meet because once slow and fast reaches in a cycle
	their distance will decrease by 1 eventually they will meet 

class Solution {
public:
    bool hasCycle(ListNode *head) {
		ListNode* slow = head;
		ListNode* fast = head;

		while(fast && fast->next){
			slow = slow->next;
			fast = fast->next->next;

			if(slow == fast)
				return true;
		}
		return false;
    }
};



### check if L.L is palindrome or not
	reverse the half part and compare the elements

	// ======= O(n) time O(1) space

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


### Reverse linked list (Recursively)

```c++
class Solution {
	void solve(ListNode* prev, ListNode* curr, ListNode* head){
		if(curr == NULL){
			return;
		}

		solve(curr, curr->next, head);
		curr->next = prev;
		return;
	}
public:
    ListNode* reverseList(ListNode* head) {
		solve(NULL, head, head);
		return ans;
    }
};

```
### Reverse linked list (Iterative)
	use 3 pointers prev, curr, next
	initially prev = null
	curr = head

	while(cur->next)
		next = cur->next
		...


### Slow fast pointer method

move one pointer slowly and move other pointer fastly and then use this to your advantage.

ex. find middle element in linked list
	use slow pointer move by 1 and use fast pointer to move by 2 so by the end your fast reaches end your slow will be at mid.

	ListNode* getMiddle(ListNode* head){
		ListNode* slow = head;
		ListNode* fast = head;

		while(fast && fast->next){
			slow = slow->next;
			fast = fast->next->next;
		}

		return slow;
	}