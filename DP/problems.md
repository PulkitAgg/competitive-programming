Here we have discussed three types of DP problem
1. 0/1 Knapsack - We can select at max one item [Link](#problems-based-on-01-knapsack)  
2. Unbounded 0/1 Knapsack - We can select as many items as we want [Link](#problems-based-on-unbounded-01-knapsack)  
3. Bounded 0/1 Knapsack - Quantity of each item will be given to us.  [Link](#problems-based-on-bounded-01-knapsack)  
4. Problems Based On String [Link](#problems-based-on-string)  
  
---
  
# Problems based on 0/1 Knapsack

### 1. 0/1 Knapsack Problem 
   Maximize profit for given knapsack capacity and given item list  
  #### Question Link - https://www.interviewbit.com/problems/0-1-knapsack/  
  
### 2. Subset Sum Problem  
  Check is there any subset exist whose sum is equal to given sum  
  #### Question Link - https://www.interviewbit.com/problems/subset-sum-problem/  

### 3. Partial Equal Subset Sum  
  Find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal. (Return true or false)  
  #### Question Link - https://leetcode.com/problems/partition-equal-subset-sum/  
  ##### Note: Convert this problem into Subset Sum Problem (Problem 2)  
   ```
   S = S1 + S2 ----- i) equation  
   S1 = S2 ------ ii) equation (divede set into two subset who has same sum)   
   S = 2S1 ---- From i) and ii) (it means only exist if sum is even)   
   If there exist a set whose sum is S/2 then there exist two sub set whose sum is equal   
   ```   
   
### 4. Count Subset with given Sum 
  Count all subsets of the given array with a sum equal to a given sum.    
  #### Question Link - https://practice.geeksforgeeks.org/problems/perfect-sum-problem5633/1  
  ##### Note: Check edge case for {0,0,0,0,0,1} sum = 1   
  ```
  Same as Problem 2 (Subset Sum Problem) instead of taking || this time we will add in case of inclusion because we want number.
  ```
  
  
### 5. Minimum Difference Subset or Minimum Subset Sum Difference 
  Divide the array A into two subsets S1 and S2 such that the absolute difference between their sums is minimum and return this minimum possible absolute difference     
  #### Question Link - https://www.interviewbit.com/problems/minimum-difference-subsets/
  ##### Note: Convert this problem into Subset Sum Problem (Problem 2)  
  ```
  S = S1 + S2  
  Diff = ABS(S2 - S1) = ABS((S-S1) - S1) = ABS(S - 2 * S1)  
  Diff = S - 2 * S1  
  Minimum diff must be 0 when S1 = S/2   
  If we check there exist S/2 i.e 0 is Mininum otherwise subtract by 1 and check it again
  ```  
  
### 6. No of Subset with given Difference  
  Find no of ways in which we can divide set into two subset whose diff is given i.e. S1 - S2 = diff (given)
  #### Question Link -  
  ##### Note: Comvert this problem into Count Subset with given Sum (Problem 4)
  ```
  S1 - S2 = Diff
  S1 + S2 = S
  => 2 * S1 = Diff + S
  => S1 = (Diff + S )/2
  Now new goal is to find no of subset with sum = S1 ie. (Diff + S )/2
  ```  
  
### 7. Target Sum Problem  
  Find no of ways to assign +/- symbols to numbers of array to make sum target  
  #### Question Link - https://leetcode.com/problems/target-sum/  
  ##### Note: Convert this problem into No of Subset with given Difference (Problem 6) which will be converted to Count Subset with given Sum (Problem 4)  
  ```
  Assume all +ve values in one subset and -ve values in other subset S1 and S2 repectively
  Now, S1 - S2 = Target its same as S1 - S2 = Diff (Problem 6)
  ```  
  
 ---
    
    
  
  # Problems based on Unbounded 0/1 Knapsack  
  Unbounded means we can select same item as many times as we want there is no restriction on limit of items.  
  
  ### 1. Coin Change Problem 
   Fewest number of coins that you need to make up that amount.  
  #### Question Link - https://leetcode.com/problems/coin-change/  
  
  
  ### 2. Coin Change Problem 2
  Number of combinations that make up that amount.  
  #### Question Link - https://leetcode.com/problems/coin-change-2/  
  
  ### 3. Rod Cutting Problem
  Given a rod of length N inches and an array of prices, price[] that contains prices of all pieces of size smaller than N. Determine the maximum value obtainable by cutting up the rod and selling the pieces.
  #### Question Link - https://practice.geeksforgeeks.org/problems/rod-cutting0840/1
  #### DP Solution: https://gist.github.com/PulkitAgg/f8c1a22bc88381fb87082e6415194a15   
  
  
### 4. Minimum Operations to Reduce X to Zero  
   You are given an integer array nums and an integer x. In one operation, you can either remove the leftmost or the rightmost element from the array nums and subtract its value from x. Note that this modifies the array for future operations.  
Return the minimum number of operations to reduce x to exactly 0 if it is possible, otherwise, return -1.

#### Question Link - https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/  
##### Note: 
  
---
    
# Problems based on Bounded 0/1 Knapsack   


---

# Problems Based On String  

### 1. Longest Common Subsequence  
Given two strings text1 and text2, return the length of their longest common subsequence. If there is no common subsequence, return 0.  
#### Question Link - https://leetcode.com/problems/longest-common-subsequence/
  
  
    
### 2. Shortest Common Supersequence  
Given two strings str1 and str2, return the shortest string that has both str1 and str2 as subsequences. If there are multiple valid strings, return any of them.  
#### Question Link - https://leetcode.com/problems/shortest-common-supersequence/  

##### Note: Use concept of LCS
```
Solution Steps
1. Find LCS String (Use LCS for finding LCS Len then find actual string)
2. Add all non common char to result until you find LCS char from both string
3. Add LCS character only once and move all pointers to next char

Len of SCS
 Max Len of SCS = L1 + L2
 LCS Len = LCS_LEN
 Len of SCS = Max Len of SCS - LCS Len =  L1 + L2 - LCS_LEN
```
  
  
### 3. Uncrossed Lines  
You are given two integer arrays nums1 and nums2. Return the maximum number of connecting lines we can draw in this way.  
#### Question Link - https://leetcode.com/problems/uncrossed-lines/

##### Note: Same as LCS because all uncrossed lines can be find only if we find LCS for all numbers.  


### 4. Longest Repeating Subsequence  
You are given a string ???str???, Your task is to find the length of the longest repeating subsequence such that two subsequences don???t have the same character at the same position.
#### Question Link - https://www.codingninjas.com/codestudio/problems/longest-repeating-subsequence_1118110

##### Note: Problem is just modification of LCS. The idea is to find the LCS(str, str) where str is the input string with the restrication that when both the character are same, they shouldn't be on the same index in the two strings.  


### 5. Edit Distance  
Given two strings word1 and word2, return the minimum number of operations required to convert word1 to word2.  
You have the following three operations permitted on a word:
Insert a character, Delete a character, Replace a character
#### Question Link - https://leetcode.com/problems/edit-distance/

##### Note: 
if s1 = "", ed = s2.length. s2 = "", ed = s1.length;  
if a == b -> no change in edit distance, copy dp[i-1][j-1]  
if(a != b) -> Min(Replace, Inset, Remove)  
Replace - Res(m,n) = Res(m-1, n-1) + 1  
Insert - Res(m, n) = Res(m, n-1) + 1  
Remove- Res(m,n) = Res(m-1,n) + 1  



   
   
