Here we have discussed three types of Sliding Window problem (concept and templates: [intro.md](./intro.md))
1. Fixed Size Window - Window size k is given [Link](#problems-based-on-fixed-size-window)  
2. Longest Valid Window - Grow right, shrink left while invalid [Link](#problems-based-on-longest-valid-window)  
3. Shortest Valid Window - Grow right until valid, shrink left while still valid [Link](#problems-based-on-shortest-valid-window)  
  
---
  
# Problems based on Fixed Size Window

### 1. Maximum Average Subarray I  
  Find the contiguous subarray of length k with the maximum average.  
  #### Question Link - https://leetcode.com/problems/maximum-average-subarray-i/  
  ##### Note: Add the new element, subtract the element that left (`nums[i - k]`).  
  ```java
  public double findMaxAverage(int[] nums, int k) {
      int sum = 0;
      for (int i = 0; i < k; i++) sum += nums[i];
      int max = sum;
      for (int i = k; i < nums.length; i++) {
          sum += nums[i] - nums[i - k];
          max = Math.max(max, sum);
      }
      return max / (double) k;
  }
  ```
  Time O(n), Space O(1)

### 2. Sliding Window Maximum  
  Return the max of every window of size k.  
  #### Question Link - https://leetcode.com/problems/sliding-window-maximum/  
  ##### Note: Monotonic deque of **indices** (values decreasing). Front = max. Drop front when index is out of window.  
  ```java
  public int[] maxSlidingWindow(int[] nums, int k) {
      int[] res = new int[nums.length - k + 1];
      Deque<Integer> dq = new ArrayDeque<>();   // indices, nums decreasing
      for (int i = 0; i < nums.length; i++) {
          if (!dq.isEmpty() && dq.peekFirst() <= i - k) dq.pollFirst();         // FRONT: left the window
          while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i]) dq.pollLast(); // BACK: keep order
          dq.offerLast(i);
          if (i >= k - 1) res[i - k + 1] = nums[dq.peekFirst()];
      }
      return res;
  }
  ```
  Time O(n), Space O(k)

---

# Problems based on Longest Valid Window
Template: **Add right. While bad, remove left. Record.**

### 3. Longest Substring Without Repeating Characters  
  Find the length of the longest substring without repeating characters.  
  #### Question Link - https://leetcode.com/problems/longest-substring-without-repeating-characters/  
  ##### Note: State = count of each char. Invalid = count of new char > 1.  
  ```java
  public int lengthOfLongestSubstring(String s) {
      int[] count = new int[128];
      int left = 0, ans = 0;
      for (int right = 0; right < s.length(); right++) {
          char c = s.charAt(right);
          count[c]++;
          while (count[c] > 1) {
              count[s.charAt(left)]--;
              left++;
          }
          ans = Math.max(ans, right - left + 1);
      }
      return ans;
  }
  ```
  Time O(n), Space O(1)

### 4. Max Consecutive Ones III  
  Longest subarray of 1s if you can flip at most k zeros.  
  #### Question Link - https://leetcode.com/problems/max-consecutive-ones-iii/  
  ##### Note: State = number of zeros in window. Invalid = zeros > k.  
  ```java
  public int longestOnes(int[] nums, int k) {
      int left = 0, zeros = 0, ans = 0;
      for (int right = 0; right < nums.length; right++) {
          if (nums[right] == 0) zeros++;
          while (zeros > k) {
              if (nums[left] == 0) zeros--;
              left++;
          }
          ans = Math.max(ans, right - left + 1);
      }
      return ans;
  }
  ```
  Time O(n), Space O(1)

### 5. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit (Asked in Uber)  
  Longest subarray where the absolute difference between any two elements is ≤ limit.  
  #### Question Link - https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/  
  ##### Note: "Any two elements ≤ limit" is the same as `max - min <= limit`. State = max and min of window.  
  ```
  Brute force O(n^2) -> TLE
  TreeMap<value, count>         -> O(n log n)
  2 heaps of indices (lazy del) -> O(n log n)   (heap.remove(value) is O(n), avoid it)
  2 monotonic deques            -> O(n)  (optimal)
  ```
  **a) TreeMap - O(n log n)**  
  ```java
  public int longestSubarray(int[] nums, int limit) {
      TreeMap<Integer, Integer> window = new TreeMap<>();   // value -> count
      int left = 0, ans = 0;
      for (int right = 0; right < nums.length; right++) {
          window.merge(nums[right], 1, Integer::sum);
          while (window.lastKey() - window.firstKey() > limit) {
              window.merge(nums[left], -1, Integer::sum);
              if (window.get(nums[left]) == 0) window.remove(nums[left]);
              left++;
          }
          ans = Math.max(ans, right - left + 1);
      }
      return ans;
  }
  ```
  **b) Two heaps with lazy deletion - O(n log n)**  
  ```java
  public int longestSubarray(int[] nums, int limit) {
      PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> Integer.compare(nums[b], nums[a]));
      PriorityQueue<Integer> minHeap = new PriorityQueue<>((a, b) -> Integer.compare(nums[a], nums[b]));
      int left = 0, ans = 0;
      for (int right = 0; right < nums.length; right++) {
          maxHeap.offer(right);
          minHeap.offer(right);
          while (nums[maxHeap.peek()] - nums[minHeap.peek()] > limit) {
              left++;
              while (maxHeap.peek() < left) maxHeap.poll();   // drop stale tops
              while (minHeap.peek() < left) minHeap.poll();
          }
          ans = Math.max(ans, right - left + 1);
      }
      return ans;
  }
  ```
  **c) Two monotonic deques - O(n)**  
  ```java
  public int longestSubarray(int[] nums, int limit) {
      Deque<Integer> maxQ = new ArrayDeque<>();   // decreasing, FRONT = max
      Deque<Integer> minQ = new ArrayDeque<>();   // increasing, FRONT = min
      int left = 0, ans = 0;
      for (int right = 0; right < nums.length; right++) {
          int x = nums[right];
          while (!maxQ.isEmpty() && maxQ.peekLast() < x) maxQ.pollLast();   // BACK: keep order
          maxQ.offerLast(x);
          while (!minQ.isEmpty() && minQ.peekLast() > x) minQ.pollLast();
          minQ.offerLast(x);
          while (maxQ.peekFirst() - minQ.peekFirst() > limit) {             // FRONT: shrink
              if (nums[left] == maxQ.peekFirst()) maxQ.pollFirst();
              if (nums[left] == minQ.peekFirst()) minQ.pollFirst();
              left++;
          }
          ans = Math.max(ans, right - left + 1);
      }
      return ans;
  }
  ```
  ##### Note: Use strict `<` / `>` when popping from back so duplicates stay and each copy is popped once.  
  Dry run: `nums = [10,1,2,4,7,2], limit = 5` → window `[2,4,7,2]` → answer 4

---

# Problems based on Shortest Valid Window
Template: **Add right. While good, record, remove left.**

### 6. Minimum Size Subarray Sum  
  Minimal length of a subarray whose sum ≥ target (0 if none). All numbers positive.  
  #### Question Link - https://leetcode.com/problems/minimum-size-subarray-sum/  
  ##### Note: Works only because numbers are positive. With negatives use prefix sum.  
  ```java
  public int minSubArrayLen(int target, int[] nums) {
      int left = 0, sum = 0, ans = Integer.MAX_VALUE;
      for (int right = 0; right < nums.length; right++) {
          sum += nums[right];
          while (sum >= target) {
              ans = Math.min(ans, right - left + 1);
              sum -= nums[left];
              left++;
          }
      }
      return ans == Integer.MAX_VALUE ? 0 : ans;
  }
  ```
  Time O(n), Space O(1)

### 7. Minimum Window Substring  
  Smallest substring of s that contains all characters of t (with duplicates).  
  #### Question Link - https://leetcode.com/problems/minimum-window-substring/  
  ##### Note: `need[c]` = how many more of c we need. `missing` = total chars still needed. Valid when missing == 0.  
  ```java
  public String minWindow(String s, String t) {
      int[] need = new int[128];
      for (char c : t.toCharArray()) need[c]++;
      int missing = t.length(), left = 0, bestLen = Integer.MAX_VALUE, bestStart = 0;
      for (int right = 0; right < s.length(); right++) {
          char c = s.charAt(right);
          if (need[c] > 0) missing--;
          need[c]--;
          while (missing == 0) {
              if (right - left + 1 < bestLen) {
                  bestLen = right - left + 1;
                  bestStart = left;
              }
              char d = s.charAt(left);
              need[d]++;
              if (need[d] > 0) missing++;
              left++;
          }
      }
      return bestLen == Integer.MAX_VALUE ? "" : s.substring(bestStart, bestStart + bestLen);
  }
  ```
  Time O(n + m), Space O(1)
