Here we have discussed Union-Find (DSU) problems (concept and template: [intro.md](./intro.md))
1. Basic Connectivity - count groups / detect cycle [Link](#problems-based-on-basic-connectivity)  
2. Connections Over Time - answer after each union [Link](#problems-based-on-connections-over-time)  
3. Merge By Shared Key / Group Size [Link](#problems-based-on-merge-by-shared-key--group-size)  

All solutions use this DSU (union by size + path compression):
```java
class DSU {
    int[] parent, size;
    int count;
    DSU(int n) {
        parent = new int[n]; size = new int[n]; count = n;
        for (int i = 0; i < n; i++) { parent[i] = i; size[i] = 1; }
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    boolean union(int a, int b) {
        int ra = find(a), rb = find(b);
        if (ra == rb) return false;
        if (size[ra] < size[rb]) { int t = ra; ra = rb; rb = t; }   // make ra the bigger tree
        parent[rb] = ra;
        size[ra] += size[rb];
        count--;
        return true;
    }
}
```
> Solutions are inside **Show solution** blocks – try first, then open.

---

# Problems based on Basic Connectivity

### 1. Number of Provinces  
  Given an n x n adjacency matrix of cities, return the number of connected groups (provinces).  
  #### Question Link - https://leetcode.com/problems/number-of-provinces/  
  ##### Note: Union every connected pair, answer = `count`.  
<details><summary>Show solution</summary>

```java
public int findCircleNum(int[][] isConnected) {
    int n = isConnected.length;
    DSU dsu = new DSU(n);
    for (int i = 0; i < n; i++)
        for (int j = i + 1; j < n; j++)
            if (isConnected[i][j] == 1) dsu.union(i, j);
    return dsu.count;
}
```
Time O(n² · α(n)), Space O(n)
</details>

### 2. Redundant Connection  
  A tree with one extra edge added. Return the edge that creates the cycle (last one in input if many).  
  #### Question Link - https://leetcode.com/problems/redundant-connection/  
  ##### Note: The edge whose `union` returns **false** joins two nodes that are already connected → cycle.  
<details><summary>Show solution</summary>

```java
public int[] findRedundantConnection(int[][] edges) {
    DSU dsu = new DSU(edges.length + 1);          // nodes are 1..n
    for (int[] e : edges) {
        if (!dsu.union(e[0], e[1])) return e;
    }
    return new int[0];
}
```
Time O(n · α(n)), Space O(n)
</details>

---

# Problems based on Connections Over Time

### 3. The Earliest Moment When Everyone Become Friends (Asked in Uber)  
  `logs[i] = [timestamp, x, y]` means x and y become friends. Friendship is transitive. Return the earliest timestamp when all n people are connected, else -1.  
  #### Question Link - https://leetcode.com/problems/the-earliest-moment-when-everyone-become-friends/ (Premium)  
  ##### Note: Sort by timestamp, union, stop when `count == 1`. Uber variant: "timestamp when all users are in one cab".  
<details><summary>Show solution</summary>

```java
public int earliestAcq(int[][] logs, int n) {
    Arrays.sort(logs, (a, b) -> Integer.compare(a[0], b[0]));
    DSU dsu = new DSU(n);
    for (int[] log : logs) {
        dsu.union(log[1], log[2]);
        if (dsu.count == 1) return log[0];
    }
    return -1;
}
```
Time O(L log L) for sorting, Space O(n)
</details>

### 4. Number of Islands II (Asked in Uber)  
  m x n grid starts as all water. `positions[i] = [r, c]` turns a cell into land. Return the island count after each operation.  
  #### Question Link - https://leetcode.com/problems/number-of-islands-ii/ (Premium)  
  ##### Note:  
  ```
  cell id = r * n + c
  start count = 0 (all water)
  new land      -> count++
  each land neighbour -> union (count-- only if it was a different island)
  same position twice -> no change
  ```
  Example: `m = 1, n = 3, positions = [[0,0],[0,2],[0,1]]` → `[1, 2, 1]` (adding (0,1) merges two islands)  
  Uber variant: restaurant grid with `openRestaurant(r, c)` and `countDeliveryZones()` – same idea.  
<details><summary>Show solution</summary>

```java
private static final int[][] DIRS = {{1,0},{-1,0},{0,1},{0,-1}};

public List<Integer> numIslands2(int m, int n, int[][] positions) {
    DSU dsu = new DSU(m * n);
    dsu.count = 0;                              // grid starts as all water
    boolean[] land = new boolean[m * n];
    List<Integer> res = new ArrayList<>();

    for (int[] p : positions) {
        int id = p[0] * n + p[1];
        if (!land[id]) {
            land[id] = true;
            dsu.count++;                        // new island on its own
            for (int[] d : DIRS) {
                int nr = p[0] + d[0], nc = p[1] + d[1];
                if (nr >= 0 && nc >= 0 && nr < m && nc < n && land[nr * n + nc]) {
                    dsu.union(id, nr * n + nc); // merges reduce count
                }
            }
        }
        res.add(dsu.count);
    }
    return res;
}
```
Time O(m·n + k · α(m·n)), Space O(m·n)
</details>

---

# Problems based on Merge By Shared Key / Group Size

### 5. Accounts Merge  
  Each account is `[name, email1, email2, ...]`. Accounts sharing any email belong to the same person. Return merged accounts with sorted emails.  
  #### Question Link - https://leetcode.com/problems/accounts-merge/  
  ##### Note: `Map<email, firstAccountIndex>`. If an email is seen again → union the two accounts. Then group emails by `find(account)`.  
<details><summary>Show solution</summary>

```java
public List<List<String>> accountsMerge(List<List<String>> accounts) {
    DSU dsu = new DSU(accounts.size());
    Map<String, Integer> owner = new HashMap<>();
    for (int i = 0; i < accounts.size(); i++) {
        for (int j = 1; j < accounts.get(i).size(); j++) {
            String email = accounts.get(i).get(j);
            Integer prev = owner.putIfAbsent(email, i);
            if (prev != null) dsu.union(i, prev);
        }
    }
    Map<Integer, TreeSet<String>> groups = new HashMap<>();
    for (Map.Entry<String, Integer> e : owner.entrySet()) {
        groups.computeIfAbsent(dsu.find(e.getValue()), k -> new TreeSet<>()).add(e.getKey());
    }
    List<List<String>> res = new ArrayList<>();
    for (Map.Entry<Integer, TreeSet<String>> g : groups.entrySet()) {
        List<String> merged = new ArrayList<>();
        merged.add(accounts.get(g.getKey()).get(0));   // name
        merged.addAll(g.getValue());                   // TreeSet = sorted emails
        res.add(merged);
    }
    return res;
}
```
Time O(E log E) for sorting emails, Space O(E)
</details>

### 6. Making A Large Island  
  n x n binary grid. You may change at most one 0 to 1. Return the size of the largest island.  
  #### Question Link - https://leetcode.com/problems/making-a-large-island/  
  ##### Note: Union all land, then for each 0 add sizes of **distinct** neighbour roots + 1. Use a `Set` so the same island is not counted twice. This is why **union by size** is useful.  
<details><summary>Show solution</summary>

```java
private static final int[][] DIRS = {{1,0},{-1,0},{0,1},{0,-1}};

public int largestIsland(int[][] grid) {
    int n = grid.length;
    DSU dsu = new DSU(n * n);
    for (int r = 0; r < n; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 1)
                for (int[] d : DIRS) {
                    int nr = r + d[0], nc = c + d[1];
                    if (nr >= 0 && nc >= 0 && nr < n && nc < n && grid[nr][nc] == 1)
                        dsu.union(r * n + c, nr * n + nc);
                }

    int ans = 0;
    for (int r = 0; r < n; r++)                       // no 0 to flip case
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 1) ans = Math.max(ans, dsu.size[dsu.find(r * n + c)]);

    for (int r = 0; r < n; r++)
        for (int c = 0; c < n; c++)
            if (grid[r][c] == 0) {
                Set<Integer> roots = new HashSet<>();
                for (int[] d : DIRS) {
                    int nr = r + d[0], nc = c + d[1];
                    if (nr >= 0 && nc >= 0 && nr < n && nc < n && grid[nr][nc] == 1)
                        roots.add(dsu.find(nr * n + nc));
                }
                int total = 1;
                for (int root : roots) total += dsu.size[root];
                ans = Math.max(ans, total);
            }
    return ans;
}
```
Time O(n² · α(n²)), Space O(n²)
</details>
