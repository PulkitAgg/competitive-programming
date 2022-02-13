## 1. Subarray Sum Equals K
Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.
### Question Link - https://leetcode.com/problems/subarray-sum-equals-k/
#### Notes: 
```
array =  [3, 4, 7, 2, -3, 1, 4, 2]  
prefix sum = [3, 7, 14, 16, 13, 14, 18, 20]
currentSum == k => count++  
(currentSum - k) exist in prefix sum then count += no of times (currentSum - k ) exist  
Logic: upto i th index, prefix sum is a and upto j th index prefix sum is b the (b - a) is a sum of array from (i+1) index upto j th index  
It implies that if (currentSum - k) exist at i th index then there exist a sub array from i ith index to current j th index whose sum is k.
```
