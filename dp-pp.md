# many times in dp you can reduce space complexity if one row is dependent on previous row only



# Problems that can be reduced to fibonacci 
	f[i] = f[i+1] + f[i+2]  ==	f[i] = f[i-1] + f[i-2]
	...


# Space reduction ideas
-	Think in terms of backward DP
	problem might reduce in terms of fibonacci

	space optimization is done mainly in backward DP.


# many times dp flow is like (to reduce space)
-	Recursion 
	forward dp (top-down) 
	backward(bottom-up) 
	space reduction(fibonacci)


# PROBLEM LIST	: 	

-	Staircase problem : [https://practice.geeksforgeeks.org/problems/count-ways-to-reach-the-nth-stair-1587115620/1]

-	Tile the lane : [https://practice.geeksforgeeks.org/problems/ways-to-tile-a-floor5836/1]

-	Max non-adjacent sum : [https://practice.geeksforgeeks.org/problems/stickler-theif-1587115621/1]

-	decode ways : [https://leetcode.com/problems/decode-ways/]

-	Rod Cutting : [https://practice.geeksforgeeks.org/problems/rod-cutting0840/1]

-	subset sum : [https://practice.geeksforgeeks.org/problems/subset-sum-problem-1611555638/1/]

-	Coin change : [https://leetcode.com/problems/coin-change/]

-	Coin change 2 : [https://practice.geeksforgeeks.org/problems/coin-change2448/1#]

-   0/1 knapsack : [https://practice.geeksforgeeks.org/problems/0-1-knapsack-problem0945/1]

-   LIS         : [https://leetcode.com/problems/longest-increasing-subsequence/]
    Envelopes   : [https://leetcode.com/problems/russian-doll-envelopes/]

-   Envelopes 2 (envelopes can be flipped)

        method 1 : using two states and sort using area
            lis[i][0] = dont flip
            lis[i][1] = flipped

            lis[i][0] = max(1+lis[j][0], 1+lis[j][1]) [conditions]
            lis[i][1] = max(1+lis[j][0], 1+lis[j][1]) [conditions]

        method 2 : just put lxb and bxl in array new size = 2*n then sort and do normal lis

- Longest palindromic subsequence : [https://leetcode.com/problems/longest-palindromic-subsequence/]

- Longest palindromic substring : [https://leetcode.com/problems/longest-palindromic-substring/]

- 
