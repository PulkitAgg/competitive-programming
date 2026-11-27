# Topic wise Concepts
1. String [Link](#string)
2. Array [Link](#array)
3. Collection [Link](#collection)
4. Interface vs Implementation [Link](#interface-vs-implementation)
5. HashMap [Link](#hashmap)
6. Queue / Deque [Link](#queue--deque)
7. LinkedHashSet [Link](#linkedhashset)
8. TreeMap [Link](#treemap)
9. Map.Entry [Link](#mapentry)
10. Concurrency [Link](#concurrency)
11. Custom Class / Gotchas [Link](#custom-class--gotchas)
12. Loops [Link](#loops)

## String
1. #### Reverse of String    
        new StringBuilder(str).reverse().toString();  
   
2. #### Length of String
        str.length();  

3. #### Compare Strings  
        a.equals(b);         // never use == for String content  
        
        
## Array
1. #### Length of Array  
        arr.length;  
      
2. #### Clone of Array  
        arr.clone();  

3. #### Fixed size array (default values 0)  
        int[] times = new int[300];  

4. #### Array as a pair / tuple  
        int[] pair = new int[]{timestamp, count};  
        pair[0]; pair[1];  
        
## Collection  
![Image](https://www.scientecheasy.com/wp-content/uploads/2018/09/java-collection-hierarchy.png)

1. #### Add object  
        add(Object);  
        
2. #### Check object exist  
        contains(Object o);  
   
3. #### Check is collection empty  
        isEmpty();  
     
4. #### Remove item  
        remove(Object o);  
        
5. #### Size  
        size();  

6. #### Conver to Array  
        toArray();  

## Interface vs Implementation
Interface = which methods exist. Class = the actual code. You can only `new` a class.

1. #### Declare with interface, create with class  
        List<Integer> list = new ArrayList<>();  
        Map<String, Integer> map = new HashMap<>();  
        Set<Integer> set = new HashSet<>();  
        Queue<Integer> q = new ArrayDeque<>();  
        Deque<Integer> dq = new ArrayDeque<>();  

2. #### Not allowed (compile error: cannot be instantiated)  
        new Queue<>();  new List<>();  new Map<>();  

## HashMap
O(1) get / put. No order.

1. #### Create  
        Map<String, Integer> map = new HashMap<>();  

2. #### Get with default (no NullPointerException)  
        map.getOrDefault(key, 0);  

3. #### Increment count  
        map.merge(key, 1, Integer::sum);   // returns the new value  

4. #### Create value if missing  
        map.computeIfAbsent(key, k -> new ArrayList<>()).add(x);  

5. #### Check / Remove  
        map.containsKey(key);  
        map.remove(key);  

## Queue / Deque
Use `ArrayDeque` (faster than `LinkedList`, O(1) at both ends). Does not allow `null`.

1. #### Create Queue  
        Queue<Integer> q = new ArrayDeque<>();  
        Deque<Integer> dq = new ArrayDeque<>();   // double-ended  

2. #### Add to end  
        q.offer(x);          // returns false if it fails  
        dq.offerLast(x);  
        dq.offerFirst(x);    // add to front  

3. #### Look at front (no remove)  
        q.peek();            // null if empty  
        dq.peekFirst();  
        dq.peekLast();       // look at end  

4. #### Remove from front  
        q.poll();            // null if empty  
        dq.pollFirst();  
        dq.pollLast();       // remove from end  

5. #### Use as Stack  
        dq.push(x);          // add to front  
        dq.pop();            // remove from front  
        dq.peek();  

6. #### Empty / Size  
        q.isEmpty();  
        q.size();  

7. #### Loop over Queue  
        for (int x : q) {  
            System.out.println(x);  
        }  

8. #### Priority Queue (Heap)  
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();  
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());  
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);  // custom compare  

9. #### Lazy removal from front (pattern)  
        while (!q.isEmpty() && isExpired(q.peek())) {  
            q.poll();  
        }  

> Note: `offer/peek/poll` return null/false on failure; `add/element/remove` throw exceptions.  
> Need `peekLast` / `pollLast`? Declare as `Deque`, not `Queue`.

## LinkedHashSet
HashSet that keeps insertion order. add / remove / contains are O(1).

1. #### Create LinkedHashSet  
        Set<Integer> set = new LinkedHashSet<>();  

2. #### Add item  
        set.add(x);          // returns false if already present  

3. #### Check item exist  
        set.contains(x);  

4. #### Remove item  
        set.remove(x);  

5. #### Get first item (oldest inserted)  
        set.iterator().next();       // check !set.isEmpty() first  

6. #### Empty / Size  
        set.isEmpty();  
        set.size();  

7. #### Loop over LinkedHashSet (insertion order)  
        for (int x : set) {  
            System.out.println(x);  
        }  

> Related: `LinkedHashMap<K,V>` keeps insertion order for maps; `new LinkedHashMap<>(16, 0.75f, true)` keeps access order (LRU cache).

## TreeMap
Map with keys always sorted (red-black tree). get / put / remove / nearest key are O(log n).

1. #### Create  
        TreeMap<Integer, Integer> map = new TreeMap<>();   // declare as TreeMap to use the methods below  

2. #### Nearest entry (null if none)  
        map.floorEntry(k);     // largest key <= k  
        map.ceilingEntry(k);   // smallest key >= k  
        map.lowerEntry(k);     // largest key <  k  
        map.higherEntry(k);    // smallest key >  k  

3. #### Nearest key only  
        map.floorKey(k);  map.ceilingKey(k);  

4. #### First / Last  
        map.firstKey();  map.lastKey();  
        map.firstEntry(); map.lastEntry();  
        map.pollFirstEntry();          // remove and return smallest  

5. #### Range views  
        map.headMap(k);                // keys <  k  
        map.tailMap(k);                // keys >= k  
        map.subMap(from, true, to, true);   // keys in [from, to]  
        map.headMap(k).clear();        // delete all keys < k  

6. #### Interval overlap check (My Calendar pattern)  
        Map.Entry<Integer,Integer> prev = map.floorEntry(start);  
        if (prev != null && prev.getValue() > start) return false;  
        Map.Entry<Integer,Integer> next = map.ceilingEntry(start);  
        if (next != null && next.getKey() < end) return false;  
        map.put(start, end);  

> Overlap of [s1,e1) and [s2,e2): `s1 < e2 && s2 < e1`

## Map.Entry
One key-value pair from a map (like `[key, value]` in JS `map.entries()`).

1. #### Read key / value  
        Map.Entry<Integer, Integer> e = map.floorEntry(15);  
        e.getKey();  
        e.getValue();  

2. #### Loop over map with entrySet (better than keySet + get)  
        for (Map.Entry<String, Integer> e : map.entrySet()) {  
            System.out.println(e.getKey() + " = " + e.getValue());  
        }  

3. #### Update value while looping  
        e.setValue(e.getValue() * 2);  

4. #### Create a pair (Java 9+, immutable)  
        Map.Entry<String, Integer> p = Map.entry("uber", 5);  

> Entries from `floorEntry` / `ceilingEntry` / `firstEntry` are read-only: `setValue` throws. Use `map.put` instead.

## Concurrency
1. #### Thread-safe map  
        ConcurrentHashMap<String, LongAdder> counts = new ConcurrentHashMap<>();  

2. #### Atomic create-if-missing + increment  
        counts.computeIfAbsent(key, k -> new LongAdder()).increment();  

3. #### Fast counter for many writers  
        LongAdder total = new LongAdder();  
        total.increment();  
        total.sum();  

4. #### Atomic counter (compare-and-set, exact reads)  
        AtomicLong c = new AtomicLong();  
        c.incrementAndGet();  
        c.get();  

5. #### Lock a whole method (several values change together)  
        public synchronized void put(String key) { ... }  

> `map.merge` on a plain HashMap and `count++` are NOT thread-safe (lost updates).

## Custom Class / Gotchas
1. #### Small inner class  
        private static class Hit {  
            final String element;  
            int count;  
            Hit(String element) { this.element = element; this.count = 1; }  
        }  

2. #### Integer comparison  
        map.get(k) == 1;        // OK: Integer unboxed to int  
        a.equals(b);            // use for two Integer objects (== fails above 127)  

3. #### Avoid overflow  
        long total = 0;         // use long for large counts  

4. #### Max / Min  
        Math.max(a, b);  Math.min(a, b);  

## Loops  
1. #### Loop over hashmap  
        Map<String,String> gfg = new HashMap<String,String>();
        for (String name : gfg.keySet()) 
        {
            String url = gfg.get(name);
            System.out.println("Key = " + name + ", Value = " + url);
        }
