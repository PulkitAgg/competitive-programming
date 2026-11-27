# **INTRODUCTION TO UNION-FIND (DISJOINT SET UNION / DSU)**

## 1. Concept (in one line)
**Companies and CEOs.** Every element belongs to a group; every group has one **CEO (root)**.  
- Same group? → compare their CEOs  
- Merge groups? → one CEO starts **reporting to** the other CEO

Tracks groups that **only merge (never split)** and answers *"are these two connected?"* in ~O(1).

| Company idea | DSU term |
|---|---|
| person | element (cell, user, node) |
| company | set / group / component |
| CEO | **root** (representative) |
| "who is your boss?" | `parent[x]` |
| "who is your CEO?" | **`find(x)`** |
| merge companies | **`union(a, b)`** |

---

## 2. How it is stored – one array
    parent[x] = x's boss        a root is its own boss: parent[x] == x

    start:        parent = [0, 1, 2, 3, 4]      5 groups
    union(0,1):   parent = [0, 0, 2, 3, 4]      {0,1} {2} {3} {4}
    union(1,2):   parent = [0, 0, 0, 3, 4]      {0,1,2} {3} {4}
    find(2):      2 -> 0 -> 0   root = 0

         0        3    4
        / \
       1   2

---

## 3. Two tricks (why it is fast)

**Problem:** naive union can build a chain `0 → 1 → 2 → ... → n-1`, so `find(0)` = n steps.

**1. Path compression (in `find`)** – while walking up, point everyone **directly to the root**.

    before: 3 → 2 → 1 → 0      after find(3): 1 → 0, 2 → 0, 3 → 0

- Fixes the path **after** walking it → one `find` can still be O(n), average becomes O(log n)

**2. Union by size (in `union`)** – attach the **smaller** tree under the **bigger** one.
- A node gets 1 level deeper only when its tree joins a tree **at least as big** → its tree size **doubles** each time → depth ≤ **log₂ n** → even the first `find` is fast

| Version | One op (worst) | Average over m ops |
|---|---|---|
| No tricks | O(n) | O(n) |
| Path compression only | O(n) | O(log n) amortised |
| Union by size/rank only | O(log n) | O(log n) |
| **Both** | O(log n) | **O(α(n)) ≈ O(1)** |

> α(n) = inverse Ackermann, < 5 for any real input.

---

## 4. Size vs Rank (only 3 lines differ)

| | Size | Rank |
|---|---|---|
| ① start | `size[i] = 1` | `rank[i] = 0` |
| ② compare | node count | upper bound on height |
| ③ after merge | `size[ra] += size[rb]` **always** | `rank[ra]++` **only if ranks equal** |
| meaning | exact group size | estimate (compression never updates it) |
| group size for free? | ✅ `size[find(x)]` | ❌ |

> **Use size by default.** Same speed + gives group size (needed in LC 827, LC 952).

---

## 5. Template (memorise)
**"Find the CEO (compress the path). Union the CEOs (small under big). Count drops only on a real merge."**

    class DSU {
        int[] parent, size;
        int count;                               // number of groups

        DSU(int n) {
            parent = new int[n];
            size = new int[n];
            count = n;
            for (int i = 0; i < n; i++) {
                parent[i] = i;                   // everyone is their own root
                size[i] = 1;
            }
        }

        int find(int x) {
            if (parent[x] != x) parent[x] = find(parent[x]);   // path compression
            return parent[x];
        }

        boolean union(int a, int b) {
            int ra = find(a), rb = find(b);
            if (ra == rb) return false;          // already same group
            if (size[ra] < size[rb]) {           // make ra the bigger tree
                int t = ra; ra = rb; rb = t;     // swap (JS: [ra, rb] = [rb, ra])
            }
            parent[rb] = ra;                     // smaller under bigger
            size[ra] += size[rb];
            count--;
            return true;
        }
    }

> `union` returns **false** when already connected → detects cycles / duplicate merges.

---

## 6. When to use it (recognise)
1. **Groups / connected components** (islands, friends, networks, accounts)
2. Connections are **only added, never removed**
3. Asks *how many groups now?*, *are A and B connected?*, *when did all become connected?*

**Final test:**
> Connections arrive **one by one** with a question after each → **DSU**  
> Whole graph given once → DFS/BFS works equally well

| Signal in question | DSU use |
|---|---|
| "after each operation return count" | `count` after each `union` |
| "earliest time all connected" | sort by time, stop when `count == 1` |
| "which edge makes a cycle" | edge whose `union` returns false |
| "merge items sharing X" | union items via a `Map<X, firstItem>` |
| "largest group" | `size[find(x)]` |

---

## 7. Grid → DSU
Flatten a cell to one number:

    id = r * cols + c          (3x3: (0,0)->0, (1,0)->3, (2,2)->8)

Grid starting as **all water** → start `count = 0`, do `count++` when a cell becomes land.

---

## 8. Common mistakes
- Comparing `parent[a] == parent[b]` instead of `find(a) == find(b)`
- `parent[a] = b` instead of linking **roots** `parent[ra] = rb`
- Doing `count--` even when `ra == rb`
- Forgetting `size[ra] += size[rb]`
- Recursive `find` can overflow only without union by size (deep chains) – with it, depth ≤ log n

---

## 9. Interview script
1. "Connections arrive over time → rerunning DFS is O(k·m·n), use Union-Find"
2. Write the template (parent, size, find with compression, union by size)
3. Map the problem: what is an element? when do we `union`? what do we read (`count`, `size`, return value)?
4. Complexity: O(α(n)) per op ≈ O(1) because of path compression + union by size
5. Dry run on a small example

---

## 10. Practice ladder
1. [LC 547](https://leetcode.com/problems/number-of-provinces/) Number of Provinces – basic count
2. [LC 684](https://leetcode.com/problems/redundant-connection/) Redundant Connection – union returns false
3. [LC 1101](https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/) Earliest Moment Everyone Become Friends – sort by time (**Uber**)
4. [LC 305](https://leetcode.com/problems/number-of-islands-ii/) Number of Islands II – grid + online count (**Uber**)
5. [LC 721](https://leetcode.com/problems/accounts-merge/) Accounts Merge – union via map
6. [LC 827](https://leetcode.com/problems/making-a-large-island/) Making A Large Island – union by size
