# Problems based on 0/1 Kanpsack

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
  
  
  
   
   
