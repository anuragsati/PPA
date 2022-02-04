***
- Tries are used when you want to find how many strings have `p` as prefix
[https://leetcode.com/problems/implement-trie-prefix-tree/]

- usually this process take serach : O(n*l) insert : O(n * l^2) delete : O(n^2 * l^2)

- whenever there is something related to `prefix` think of trie


***
```c++
    struct TrieNode {
        vector<TrieNode*> child;    //total child nodes
        bool isEnd;                 //denotes if this node is a string or not
        int numStrings;             //denotes how many strings are that have this as prefix (total strings below this node)

        TrieNode(){
            child.resize(26, NULL);
            isEnd = false;
            numStrings = 0;
        }
    };
    TrieNode* root;
```








## searching    : T.C : O(L)  S.C : O(L) or O(1) using iteration
- we start from cur = root
- then we move down tree at each point we match the word with current tree
- if current becomes null before finding answer means answer doesnot exist 
- if position = word.size then we check if current point is string or not if it is then we return

- if it is asked to return number of strings matching this then `return cur->numString;`


```c++
    bool search(string word, int pos, TrieNode* cur) {
        if(cur == NULL)
            return false;
        if(pos == word.size()){
            if(cur->isEnd) return true;
            else return false;
        }

        return search(word, pos+1, cur->child[word[pos]-'a']);
    }

    // search(word, 0, root)
```






## Insertion    : T.C : O(L)  S.C : O(L) or O(1) using iteration
- we start from cur = root and move down tree
- if some node doesnot exist then we create it and recurse on it
- at each point we increase number of strings of each node we are touching because new string will be added
- if we reach word.size then we mark that node as end and return

```c++
    void insert(string word, int pos=0, TrieNode* cur = root) {
        //increase number of strings
        cur->numStrings++;

        //base case
        if(pos == word.size()){
            cur->isEnd = true;
            return;
        }

        //if this node does not exist create it
        if(cur->child[word[pos]-'a'] == NULL){
            cur->child[word[pos]-'a'] = new TrieNode();
        }

        //move to next index
        insert(word, pos+1, cur->child[word[pos]-'a']);
        return;
    }
```



## Deletion

![picture 1](../images/af02723e57980db866f0b5a85eb8a217053cf2823f22e552d84190bb9b57ab85.png)  

- we maintain a bool variable found that denotes if word is found or not
- we only delete if it is found
- if we found a word then we start decrementing by -1 to all nodes in path
- and if some nodes value becomes 0 we delete them permanently `using their parents`

```c++
    bool found = false;

    void delete(string word, int pos, TrieNode* cur) {
        if(cur == NULL)
            return false;
        if(pos == word.size()){
            if(cur->isEnd){     //if this node is required string delete it
                found = true;
                cur->numStrings--;
            }
            return;
        }

        delete(word, pos+1, cur->child[word[pos]-'a']);

        //if this is part of deletion tree then subtract -1 bcz one node got deleted
        if(found) cur->numStrings--;

        //if some nodes childs numstrings become 0 then remove them
        if(cur->child[word[pos]-'a']->numstrings == 0){
            TrieNode* temp = cur->child[word[pos]-'a'];
            cur->child[word[pos]-'a'] = NULL;
            delete(temp);
        }

        return;
    }
```


## To find something is end or not without isEnd
if it is a non string then cur->numString = sum of all childrens numstrings
if it is a string then values will differ bcz extra values will be for currnt string