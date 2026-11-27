# **INTRODUCTION TO SLIDING WINDOW**

## 1. Concept (in one line)
A **caterpillar** crawling over the array: the **head (right)** eats new elements, the **tail (left)** drops old ones when the rule breaks. Both only move **forward** → each index is touched at most twice → **O(n)** instead of O(n²).

> Brute force restarts from every `i`. Sliding window **reuses** the previous window.

---

## 2. When to use it (recognise)
All three in the question:
1. **Contiguous** → "subarray" / "substring" (not subsequence / subset)
2. Asks for **longest / shortest / max / count**
3. A **rule about the window** → sum ≤ k, ≤ k distinct, no repeats, max − min ≤ limit

**Final test:**
> If the window breaks the rule, does making it **bigger** keep it broken?  
> **Yes** → sliding window works.  
> **No** (e.g. sum = k with **negative numbers**) → use **prefix sum + HashMap**.

---

## 3. Three types

| Type | Question says | Movement | Examples |
|---|---|---|---|
| **Fixed size** | "every window of size k" | left and right move together | LC 643, LC 239 |
| **Longest valid** | "longest subarray where…" | right always; left **while invalid** | LC 3, LC 1004, LC 1438 |
| **Shortest valid** | "smallest subarray where…" | right until valid; left **while still valid** | LC 209, LC 76 |

---

## 4. Templates (memorise)

### Longest valid → "Add right. While bad, remove left. Record."
    int left = 0, ans = 0;
    for (int right = 0; right < n; right++) {
        add(nums[right]);                  // 1. ADD
        while (invalid()) {                // 2. WHILE BAD
            remove(nums[left]);            // 3. REMOVE
            left++;
        }
        ans = Math.max(ans, right - left + 1);   // 4. RECORD
    }

### Shortest valid → "Add right. While good, record, remove left."
    int left = 0, ans = Integer.MAX_VALUE;
    for (int right = 0; right < n; right++) {
        add(nums[right]);
        while (valid()) {
            ans = Math.min(ans, right - left + 1);
            remove(nums[left]);
            left++;
        }
    }
    return ans == Integer.MAX_VALUE ? 0 : ans;

### Fixed size k
    for (int right = 0; right < n; right++) {
        add(nums[right]);
        if (right - k >= 0) remove(nums[right - k]);   // keep size k
        if (right >= k - 1) record();
    }

---

## 5. How to think (2 questions before coding)
1. **STATE** – what must I know about the window to decide valid / invalid?
2. **INVALID** – the exact `while` condition.

Then pick a structure that can **add, remove, check** fast:

| Problem | State | Invalid when | Structure |
|---|---|---|---|
| Max sum of k | sum | size > k | `int` |
| No repeating chars (LC 3) | count per char | some count > 1 | `HashMap` / `int[128]` |
| At most k distinct (LC 340, 904) | count per value | `map.size() > k` | `HashMap` |
| Min sum ≥ target (LC 209) | sum | (shortest) sum ≥ target | `int` |
| Max − min ≤ limit (LC 1438) | max & min | max − min > limit | **2 monotonic deques** |

> Sum / count = easy to subtract. **Max / min = hard** → need heap, TreeMap or monotonic deque.

---

## 6. Getting max / min of a window

| Approach | How remove works | Time |
|---|---|---|
| `TreeMap<value, count>` | count−1, delete key at 0; `firstKey()`=min, `lastKey()`=max | O(n log n) |
| 2 heaps of **indices** | lazy delete: `while (heap.peek() < left) poll()` | O(n log n) |
| 2 **monotonic deques** | pop front if it equals `nums[left]` | **O(n)** |

⚠️ `PriorityQueue.remove(value)` is **O(n)** → use lazy deletion.

---

## 7. Monotonic deque (max of window)
FRONT = `First` = head · BACK = `Last` = tail / end

    // BACK: keep order (decreasing for max)
    while (!maxQ.isEmpty() && maxQ.peekLast() < x) maxQ.pollLast();
    maxQ.offerLast(x);
    // FRONT: read answer / drop what left the window
    int max = maxQ.peekFirst();
    if (nums[left] == maxQ.peekFirst()) maxQ.pollFirst();

**Why pop smaller from back?** A smaller value that came **earlier** can never be the max while the newer bigger one is in the window.

**Rules:**
- **Back** → maintain order (pop useless, then add)
- **Front** → read answer, pop what left the window
- Max deque = **decreasing**, pop `<` · Min deque = **increasing**, pop `>`
- Use **strict** `<` / `>` so duplicates stay (each copy popped once)

---

## 8. Common mistakes
- Forgetting a **single element** is a valid window → `ans` should be ≥ 1 for non-empty arrays
- `Math.min` vs `Math.max` typo when tracking max
- Using `if` instead of `while` when shrinking
- `remove(Object)` on a heap → O(n), TLE
- Using sliding window with **negative numbers** on sum problems

---

## 9. Interview script
1. Brute force O(n²) → say why it repeats work
2. "Contiguous + rule that stays broken when growing → sliding window"
3. Name **state** and **invalid condition**
4. Start simple (TreeMap / heaps, O(n log n)) → optimise to monotonic deque O(n)
5. Dry run on a small example, state complexity

---

## 10. Practice ladder
1. [LC 643](https://leetcode.com/problems/maximum-average-subarray-i/) Max Average Subarray I – fixed
2. [LC 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) Longest Substring Without Repeating – longest + HashMap
3. [LC 209](https://leetcode.com/problems/minimum-size-subarray-sum/) Min Size Subarray Sum – shortest
4. [LC 1004](https://leetcode.com/problems/max-consecutive-ones-iii/) Max Consecutive Ones III – longest + counter
5. [LC 1438](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/) Abs Diff ≤ Limit – monotonic deques (Uber)
6. [LC 239](https://leetcode.com/problems/sliding-window-maximum/) Sliding Window Maximum – fixed + monotonic deque
7. [LC 76](https://leetcode.com/problems/minimum-window-substring/) Minimum Window Substring – shortest, hard
